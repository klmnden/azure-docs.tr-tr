---
title: Azure Data factory'de kimlik bilgilerini şifreleme | Microsoft Docs
description: Şifreleme ve şirket içinde barındırılan tümleştirme çalışma zamanı olan bir makinede, şirket içi veri depoları için kimlik bilgilerini saklamak hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: nabhishek
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/15/2018
ms.author: abnarain
ms.openlocfilehash: 8e8a4cabd948783278981c61fa718e51b679ad72
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54014174"
---
# <a name="encrypt-credentials-for-on-premises-data-stores-in-azure-data-factory"></a>Azure Data factory'de şirket içi veri depoları için kimlik bilgilerini şifrele
Şifreleme ve kimlik bilgilerini şirket içinde barındırılan tümleştirme çalışma zamanı olan bir makinede, şirket içi veri depoları (bağlı hizmetler ile hassas bilgileri) depolamak. 

Bir JSON tanımı dosyası kimlik bilgileriyle geçirin <br/>[**Yeni-AzureRmDataFactoryV2LinkedServiceEncryptedCredential** ](https://docs.microsoft.com/powershell/module/azurerm.datafactoryv2/New-AzureRmDataFactoryV2LinkedServiceEncryptedCredential?view=azurermps-4.4.0) şifrelenmiş kimlik bilgileriyle bir çıkış JSON tanımı dosyası oluşturmak için cmdlet'i. Ardından, güncelleştirilen JSON tanımı bağlı hizmet oluşturmak için kullanın.

## <a name="author-sql-server-linked-service"></a>Yazar SQL Server bağlı hizmeti
Adlı bir JSON dosyası oluşturun **C:\adfv2tutorial** herhangi bir klasörde aşağıdaki içerikle:  

Değiştirin `<servername>`, `<databasename>`, `<username>`, ve `<password>` dosyayı kaydetmeden önce SQL Server için değerlerle. Ve Değiştir `<integration runtime name>` tümleştirme çalışma zamanınızın adıyla. 

```json
{
    "properties": {
        "type": "SqlServer",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server=<servername>;Database=<databasename>;User ID=<username>;Password=<password>;Timeout=60"
            }
        },
        "connectVia": {
            "type": "integrationRuntimeReference",
            "referenceName": "<integration runtime name>"
        },
        "name": "SqlServerLinkedService"
    }
}
```

## <a name="encrypt-credentials"></a>Kimlik bilgilerini şifrele
Bir şirket içi şirket içinde barındırılan tümleştirme çalışma zamanı üzerinde JSON yükünden hassas verileri şifrelemek için **New-AzureRmDataFactoryV2LinkedServiceEncryptedCredential**ve JSON yükünü geçirebiliriz. Bu cmdlet, DPAPI kullanılarak ve şirket içinde barındırılan tümleştirme çalışma zamanı düğümüne yerel olarak depolanan kimlik bilgileri şifrelenir sağlar. Çıktı yükü, şifrelenmiş kimlik bilgilerini içeren başka bir JSON dosyasına (Bu örnekte 'encryptedLinkedService.json') yönlendirilebilir.

```powershell
New-AzureRmDataFactoryV2LinkedServiceEncryptedCredential -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -Name "SqlServerLinkedService" -DefinitionFile ".\SQLServerLinkedService.json" > encryptedSQLServerLinkedService.json
```

## <a name="use-the-json-with-encrypted-credentials"></a>Şifrelenmiş kimlik bilgileriyle JSON kullanın
Şimdi, ayarlamak için şifrelenmiş kimlik bilgileri içeren önceki komuttan çıkış JSON dosyasını kullanın **SqlServerLinkedService**.

```powershell
Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -Name "EncryptedSqlServerLinkedService" -DefinitionFile ".\encryptedSqlServerLinkedService.json" 
```

## <a name="next-steps"></a>Sonraki adımlar
Veri taşıma için güvenlik konuları hakkında daha fazla bilgi için bkz. [veri taşıma güvenlik konuları](data-movement-security-considerations.md).

