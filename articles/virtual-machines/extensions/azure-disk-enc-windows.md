---
title: Windows için Azure Disk şifrelemesi | Microsoft Docs
description: Azure Disk şifrelemesi, bir sanal makine uzantısı kullanarak bir Windows sanal makinesine dağıtır.
services: virtual-machines-windows
documentationcenter: ''
author: ejarvi
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 06/12/2018
ms.author: ejarvi
ms.openlocfilehash: ff77f9fc017627143b14544af03d0d5e80813db9
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67051702"
---
# <a name="azure-disk-encryption-for-windows-microsoftazuresecurityazurediskencryption"></a>Windows (Microsoft.Azure.Security.AzureDiskEncryption) için Azure Disk şifrelemesi

## <a name="overview"></a>Genel Bakış

Azure Disk şifrelemesi, Windows çalıştıran Azure sanal makinelerinde tam disk şifreleme sağlamak için BitLocker'ı kullanır.  Bu çözüm, disk şifreleme anahtarlarını ve gizli anahtar kasası aboneliğinizi yönetmek için Azure Key Vault ile tümleşiktir. 

## <a name="prerequisites"></a>Önkoşullar

Önkoşullar tam bir listesi için bkz. [Azure Disk şifrelemesi önkoşulları](
../../security/azure-security-disk-encryption-prerequisites.md).

### <a name="operating-system"></a>İşletim sistemi

Şu anda Windows sürümlerinin bir listesi için bkz. [Azure Disk şifrelemesi önkoşulları](../../security/azure-security-disk-encryption-prerequisites.md).

### <a name="internet-connectivity"></a>İnternet bağlantısı

Azure Disk şifrelemesi, erişim için Active Directory, Key Vault, depolama ve paket yönetim uç noktalarını Internet bağlantısı gerektirir.  Ağ güvenlik ayarları hakkında daha fazla bilgi için bkz. [Azure Disk şifrelemesi önkoşulları](
../../security/azure-security-disk-encryption-prerequisites.md).

## <a name="extension-schemata"></a>Uzantı şemaların

Azure Disk şifrelemesi için iki şemaların vardır: v1.1, Azure Active Directory (AAD) özellikleri ve v0.1 kullanmaz daha yeni ve önerilen şema, AAD özellikleri gerektiren daha eski bir şema. Kullanmakta olduğunuz uzantısı için karşılık gelen şema sürümü kullanmanız gerekir: şema v1.1 uzantısı sürüm 1.1, uzantı sürümü 0,1 AzureDiskEncryption için şema v0.1 AzureDiskEncryption için.

### <a name="schema-v11-no-aad-recommended"></a>Şema v1.1: Hiçbir AAD (önerilir)

V1.1 şeması önerilir ve Azure Active Directory özelliklerini gerektirmez.

```json
{
  "type": "extensions",
  "name": "[name]",
  "apiVersion": "2015-06-15",
  "location": "[location]",
  "properties": {
    "publisher": "Microsoft.Azure.Security",
    "settings": {
      "EncryptionOperation": "[encryptionOperation]",
      "KeyEncryptionAlgorithm": "[keyEncryptionAlgorithm]",
      "KeyEncryptionKeyURL": "[keyEncryptionKeyURL]",
      "KekVaultResourceId": "[keyVaultResourceID]",
      "KeyVaultURL": "[keyVaultURL]",
      "KeyVaultResourceId": "[keyVaultResourceID]",
      "SequenceVersion": "sequenceVersion]",
      "VolumeType": "[volumeType]"
    },
  "type": "AzureDiskEncryption",
  "typeHandlerVersion": "[extensionVersion]"
  }
}
```


### <a name="schema-v01-with-aad"></a>Şema v0.1: AAD ile 

0,1 şema gerektirir `aadClientID` ve her iki `aadClientSecret` veya `AADClientCertificate`.

Kullanarak `aadClientSecret`:

```json
{
  "type": "extensions",
  "name": "[name]",
  "apiVersion": "2015-06-15",
  "location": "[location]",
  "properties": {
    "protectedSettings": {
      "AADClientSecret": "[aadClientSecret]"
    },    
    "publisher": "Microsoft.Azure.Security",
    "settings": {
      "AADClientID": "[aadClientID]",
      "EncryptionOperation": "[encryptionOperation]",
      "KeyEncryptionAlgorithm": "[keyEncryptionAlgorithm]",
      "KeyEncryptionKeyURL": "[keyEncryptionKeyURL]",
      "KekVaultResourceId": "[keyVaultResourceID]",
      "KeyVaultURL": "[keyVaultURL]",
      "KeyVaultResourceId": "[keyVaultResourceID]",
      "SequenceVersion": "sequenceVersion]",
      "VolumeType": "[volumeType]"
    },
  "type": "AzureDiskEncryption",
  "typeHandlerVersion": "[extensionVersion]"
  }
}
```

Kullanarak `AADClientCertificate`:

```json
{
  "type": "extensions",
  "name": "[name]",
  "apiVersion": "2015-06-15",
  "location": "[location]",
  "properties": {
    "protectedSettings": {
      "AADClientCertificate": "[aadClientCertificate]"
    },    
    "publisher": "Microsoft.Azure.Security",
    "settings": {
      "AADClientID": "[aadClientID]",
      "EncryptionOperation": "[encryptionOperation]",
      "KeyEncryptionAlgorithm": "[keyEncryptionAlgorithm]",
      "KeyEncryptionKeyURL": "[keyEncryptionKeyURL]",
      "KekVaultResourceId": "[keyVaultResourceID]",
      "KeyVaultURL": "[keyVaultURL]",
      "KeyVaultResourceId": "[keyVaultResourceID]",
      "SequenceVersion": "sequenceVersion]",
      "VolumeType": "[volumeType]"
    },
  "type": "AzureDiskEncryption",
  "typeHandlerVersion": "[extensionVersion]"
  }
}
```


### <a name="property-values"></a>Özellik değerleri

| Ad | Değer / örnek | Veri Türü |
| ---- | ---- | ---- |
| apiVersion | 2015-06-15 | date |
| publisher | Microsoft.Azure.Security | string |
| türü | AzureDiskEncryptionForLinux | string |
| typeHandlerVersion | 0.1, 1.1 | int |
| (0,1 Şeması) Aadclientıd | xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx | GUID | 
| (0,1 Şeması) AADClientSecret | password | string |
| (0,1 Şeması) AADClientCertificate | thumbprint | string |
| DiskFormatQuery | {"dev_path": "", "name": "","file_system": ""} | JSON sözlüğü |
| EncryptionOperation | EnableEncryption, EnableEncryptionFormatAll | string | 
| KeyEncryptionAlgorithm | 'OAEP RSA', 'RSA-OAEP-256', 'RSA1_5' | string |
| KeyEncryptionKeyURL | url | string |
| KeyVaultURL | url | string |
| (isteğe bağlı) Parola | password | string | 
| SequenceVersion | uniqueidentifier | string |
| VolumeType | İşletim sistemi, veri, tüm | string |

## <a name="template-deployment"></a>Şablon dağıtımı
Şablonu dağıtım örneği için bkz: [ galeri görüntüsünden yeni şifrelenmiş bir Windows VM oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image).

## <a name="azure-cli-deployment"></a>Azure CLI dağıtım

Yönergeler için en [Azure CLI belgeleri](/cli/azure/vm/encryption?view=azure-cli-latest). 

## <a name="troubleshoot-and-support"></a>Sorun giderme ve Destek

### <a name="troubleshoot"></a>Sorun giderme

Başvurmak [Azure Disk şifrelemesi sorun giderme kılavuzu](../../security/azure-security-disk-encryption-tsg.md).

### <a name="support"></a>Destek

Bu makalede herhangi bir noktada daha fazla yardıma ihtiyacınız olursa, üzerinde Azure uzmanlarıyla iletişime geçebilirsiniz [Azure MSDN ve Stack Overflow forumları](https://azure.microsoft.com/support/community/). Alternatif olarak, bir Azure destek olayına dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) ve Destek Al'ı seçin. Azure desteği hakkında daha fazla bilgi için okuma [Microsoft Azure desteği SSS](https://azure.microsoft.com/support/faq/).

## <a name="next-steps"></a>Sonraki adımlar
Uzantıları hakkında daha fazla bilgi için bkz. [sanal makine uzantıları ve özellikleri Windows için](features-windows.md).
