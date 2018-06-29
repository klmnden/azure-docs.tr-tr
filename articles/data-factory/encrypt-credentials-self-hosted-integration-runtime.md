---
title: Azure veri fabrikası'nda kimlik bilgilerini şifrelemek | Microsoft Docs
description: Şifrelemek ve bir makinede kendini barındıran tümleştirmesi çalışma zamanı ile şirket içi veri depoları için kimlik bilgilerini depolamak öğrenin.
services: data-factory
documentationcenter: ''
author: nabhishek
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/15/2018
ms.author: abnarain
ms.openlocfilehash: b577c276627c3a187215cd0da551428fbb32791f
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37050915"
---
# <a name="encrypt-credentials-for-on-premises-data-stores-in-azure-data-factory"></a>Şirket içi veri depolarında Azure veri fabrikası için kimlik bilgilerini şifrelemek
Şifreleme ve kendi kendini barındıran tümleştirmesi çalışma zamanı olan bir makinede, şirket içi veri depoları (hassas bilgiler ile bağlantılı hizmetler) için kimlik bilgilerini depolamak. 

JSON tanım dosyası kimlik bilgileriyle geçirin <br/>[**AzureRmDataFactoryV2LinkedServiceEncryptedCredential yeni** ](https://docs.microsoft.com/powershell/module/azurerm.datafactoryv2/New-AzureRmDataFactoryV2LinkedServiceEncryptedCredential?view=azurermps-4.4.0) şifrelenmiş kimlik bilgileri ile bir çıktı JSON tanım dosyası oluşturmak için cmdlet'i. Ardından, bağlı hizmetler oluşturma için güncelleştirilmiş JSON tanımını kullanın.

## <a name="author-sql-server-linked-service"></a>Yazar bağlı SQL Server hizmeti
Adlı bir JSON dosyası oluşturun **SqlServerLinkedService.json** aşağıdaki içeriğe sahip herhangi bir klasörde:  

Değiştir `<servername>`, `<databasename>`, `<username>`, ve `<password>` dosyayı kaydetmeden önce SQL Server'ınızdaki değerlere sahip. Ve değiştirme `<integration runtime name>` tümleştirmesi çalışma zamanı adı. 

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
Bir şirket içi kendi kendini barındıran tümleştirme çalışma zamanında JSON yükü hassas verileri şifrelemek için Çalıştır **yeni AzureRmDataFactoryV2LinkedServiceEncryptedCredential**ve JSON yükü geçirin. Bu cmdlet, kimlik bilgileri DPAPI kullanarak ve kendi kendini barındıran tümleştirmesi çalışma zamanı düğüm üzerinde yerel olarak depolanan şifrelenmiş sağlar. Çıktı yükü şifrelenmiş kimlik bilgileri içeren başka bir JSON dosyası için (Bu durumda 'encryptedLinkedService.json'), yönlendirilebilir.

```powershell
New-AzureRmDataFactoryV2LinkedServiceEncryptedCredential -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -Name "SqlServerLinkedService" -DefinitionFile ".\SQLServerLinkedService.json" > encryptedSQLServerLinkedService.json
```

## <a name="use-the-json-with-encrypted-credentials"></a>JSON ile şifrelenmiş kimlik bilgilerini kullan
Şimdi kurmak için şifreli kimlik bilgilerini içeren önceki komutu çıktı JSON dosyasından kullanın **SqlServerLinkedService**.

```powershell
Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -Name "EncryptedSqlServerLinkedService" -DefinitionFile ".\encryptedSqlServerLinkedService.json" 
```

## <a name="next-steps"></a>Sonraki adımlar
Veri taşıma için güvenlik konuları hakkında daha fazla bilgi için bkz: [veri taşıma güvenlik konuları](data-movement-security-considerations.md).

