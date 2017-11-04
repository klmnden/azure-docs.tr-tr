---
title: "Azure veri fabrikası'nda kimlik bilgilerini şifrelemek | Microsoft Docs"
description: "Şifrelemek ve bir makinede kendini barındıran tümleştirmesi çalışma zamanı ile şirket içi veri depoları için kimlik bilgilerini depolamak öğrenin."
services: data-factory
documentationcenter: 
author: nabhishek
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: abnarain
ms.openlocfilehash: eb07deab834f63b48b7441f32521c48b27e549ca
ms.sourcegitcommit: c50171c9f28881ed3ac33100c2ea82a17bfedbff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2017
---
# <a name="encrypt-credentials-for-on-premises-data-stores-in-azure-data-factory"></a>Şirket içi veri depolarında Azure veri fabrikası için kimlik bilgilerini şifrelemek
Şifreleme ve kendi kendini barındıran tümleştirmesi çalışma zamanı olan bir makinede, şirket içi veri depoları (hassas bilgiler ile bağlantılı hizmetler) için kimlik bilgilerini depolamak. 

JSON tanım dosyası kimlik bilgileriyle geçirin <br/>[**AzureRmDataFactoryV2LinkedServiceEncryptedCredential yeni** ](https://docs.microsoft.com/powershell/module/azurerm.datafactoryv2/New-AzureRmDataFactoryV2LinkedServiceEncryptedCredential?view=azurermps-4.4.0) şifrelenmiş kimlik bilgileri ile bir çıktı JSON tanım dosyası oluşturmak için cmdlet'i. Ardından, bağlı hizmetler oluşturma için güncelleştirilmiş JSON tanımını kullanın.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Data Factory hizmetinin genel kullanıma açık 1. sürümünü kullanıyorsanız bkz. [Data Factory sürüm 1 belgeleri](v1/data-factory-introduction.md).

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

## <a name="encrypt-credentials"></a>Kimlik bilgilerini şifrelemek
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

