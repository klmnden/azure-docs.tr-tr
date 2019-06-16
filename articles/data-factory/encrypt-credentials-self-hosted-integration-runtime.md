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
ms.openlocfilehash: 8e705a4430f6ccee847dc7d41ef80456a6dc4ea5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66155143"
---
# <a name="encrypt-credentials-for-on-premises-data-stores-in-azure-data-factory"></a>Azure Data factory'de şirket içi veri depoları için kimlik bilgilerini şifrele
Şifreleme ve kimlik bilgilerini şirket içinde barındırılan tümleştirme çalışma zamanı olan bir makinede, şirket içi veri depoları (bağlı hizmetler ile hassas bilgileri) depolamak. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bir JSON tanımı dosyası kimlik bilgileriyle geçirin <br/>[**Yeni AzDataFactoryV2LinkedServiceEncryptedCredential** ](/powershell/module/az.datafactory/New-AzDataFactoryV2LinkedServiceEncryptedCredential) şifrelenmiş kimlik bilgileriyle bir çıkış JSON tanımı dosyası oluşturmak için cmdlet'i. Ardından, güncelleştirilen JSON tanımı bağlı hizmet oluşturmak için kullanın.

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
Bir şirket içi şirket içinde barındırılan tümleştirme çalışma zamanı üzerinde JSON yükünden hassas verileri şifrelemek için **yeni AzDataFactoryV2LinkedServiceEncryptedCredential**ve JSON yükünü geçirebiliriz. Bu cmdlet, DPAPI kullanılarak ve şirket içinde barındırılan tümleştirme çalışma zamanı düğümüne yerel olarak depolanan kimlik bilgileri şifrelenir sağlar. Çıktı yükü, şifrelenmiş kimlik bilgisi başvuru içeren başka bir JSON dosyasına (Bu örnekte 'encryptedLinkedService.json') yönlendirilebilir.

```powershell
New-AzDataFactoryV2LinkedServiceEncryptedCredential -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -Name "SqlServerLinkedService" -DefinitionFile ".\SQLServerLinkedService.json" > encryptedSQLServerLinkedService.json
```

## <a name="use-the-json-with-encrypted-credentials"></a>Şifrelenmiş kimlik bilgileriyle JSON kullanın
Şimdi, ayarlamak için şifrelenmiş kimlik bilgileri içeren önceki komuttan çıkış JSON dosyasını kullanın **SqlServerLinkedService**.

```powershell
Set-AzDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -Name "EncryptedSqlServerLinkedService" -DefinitionFile ".\encryptedSqlServerLinkedService.json" 
```

## <a name="next-steps"></a>Sonraki adımlar
Veri taşıma için güvenlik konuları hakkında daha fazla bilgi için bkz. [veri taşıma güvenlik konuları](data-movement-security-considerations.md).

