---
title: Linux için Azure Disk şifrelemesi | Microsoft Docs
description: Azure Disk şifrelemesi için Linux sanal makine uzantısı kullanarak bir sanal makineye dağıtır.
services: virtual-machines-linux
documentationcenter: ''
author: ejarvi
manager: gwallace
editor: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 06/10/2019
ms.author: ejarvi
ms.openlocfilehash: d544aae33faf60be00a2b4ea0a45f405efcedb39
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67706137"
---
# <a name="azure-disk-encryption-for-linux-microsoftazuresecurityazurediskencryptionforlinux"></a>Linux (Microsoft.Azure.Security.AzureDiskEncryptionForLinux) için Azure Disk şifrelemesi

## <a name="overview"></a>Genel Bakış

Azure Disk şifrelemesi yararlanır dm-crypt alt tam disk şifreleme sağlamak için Linux sisteminde [seçin Azure Linux dağıtımları](https://aka.ms/adelinux).  Bu çözüm, disk şifreleme anahtarlarını ve gizli anahtarları yönetmek için Azure anahtar kasası ile tümleştirilmiştir.

## <a name="prerequisites"></a>Önkoşullar

Önkoşullar tam bir listesi için bkz. [Azure Disk şifrelemesi önkoşulları](
../../security/azure-security-disk-encryption-prerequisites.md).

### <a name="operating-system"></a>İşletim sistemi

Azure Disk şifrelemesi, şu anda select dağıtımları ve sürümlerinde desteklenir.  Bkz: [Azure Disk şifrelemesi desteklenen işletim sistemleri: Linux](../../security/azure-security-disk-encryption-prerequisites.md#linux) desteklenen Linux dağıtımları listesi.

### <a name="internet-connectivity"></a>İnternet bağlantısı

Linux için Azure Disk şifrelemesi, erişim için Active Directory, Key Vault, depolama ve paket yönetim uç noktalarını Internet bağlantısı gerektirir.  Daha fazla bilgi için [Azure Disk şifrelemesi önkoşulları](../../security/azure-security-disk-encryption-prerequisites.md).

## <a name="extension-schemata"></a>Uzantı şemaların

Azure Disk şifrelemesi için iki şemaların vardır: v1.1, Azure Active Directory (AAD) özellikleri ve v0.1 kullanmaz daha yeni ve önerilen şema, AAD özellikleri gerektiren daha eski bir şema. Kullanmakta olduğunuz uzantısı için karşılık gelen şema sürümü kullanmanız gerekir: şema v1.1 uzantısı sürüm 1.1, uzantı sürümü 0,1 AzureDiskEncryptionForLinux için şema v0.1 AzureDiskEncryptionForLinux için.
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
          "DiskFormatQuery": "[diskFormatQuery]",
          "EncryptionOperation": "[encryptionOperation]",
          "KeyEncryptionAlgorithm": "[keyEncryptionAlgorithm]",
          "KeyEncryptionKeyURL": "[keyEncryptionKeyURL]",
          "KeyVaultURL": "[keyVaultURL]",
          "SequenceVersion": "sequenceVersion]",
          "VolumeType": "[volumeType]"
        },
        "type": "AzureDiskEncryptionForLinux",
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
      "AADClientSecret": "[aadClientSecret]",
      "Passphrase": "[passphrase]"
    },
    "publisher": "Microsoft.Azure.Security",
    "settings": {
      "AADClientID": "[aadClientID]",
      "DiskFormatQuery": "[diskFormatQuery]",
      "EncryptionOperation": "[encryptionOperation]",
      "KeyEncryptionAlgorithm": "[keyEncryptionAlgorithm]",
      "KeyEncryptionKeyURL": "[keyEncryptionKeyURL]",
      "KeyVaultURL": "[keyVaultURL]",
      "SequenceVersion": "sequenceVersion]",
      "VolumeType": "[volumeType]"
    },
    "type": "AzureDiskEncryptionForLinux",
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
      "AADClientCertificate": "[aadClientCertificate]",
      "Passphrase": "[passphrase]"
    },
    "publisher": "Microsoft.Azure.Security",
    "settings": {
      "AADClientID": "[aadClientID]",
      "DiskFormatQuery": "[diskFormatQuery]",
      "EncryptionOperation": "[encryptionOperation]",
      "KeyEncryptionAlgorithm": "[keyEncryptionAlgorithm]",
      "KeyEncryptionKeyURL": "[keyEncryptionKeyURL]",
      "KeyVaultURL": "[keyVaultURL]",
      "SequenceVersion": "sequenceVersion]",
      "VolumeType": "[volumeType]"
    },
    "type": "AzureDiskEncryptionForLinux",
    "typeHandlerVersion": "[extensionVersion]"
  }
}
```


### <a name="property-values"></a>Özellik değerleri

| Ad | Değer / örnek | Veri Türü |
| ---- | ---- | ---- |
| apiVersion | 2015-06-15 | date |
| publisher | Microsoft.Azure.Security | dize |
| türü | AzureDiskEncryptionForLinux | dize |
| typeHandlerVersion | 0.1, 1.1 | int |
| (0,1 Şeması) AADClientID | xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx | guid | 
| (0,1 Şeması) AADClientSecret | password | dize |
| (0,1 Şeması) AADClientCertificate | thumbprint | dize |
| DiskFormatQuery | {"dev_path": "", "name": "","file_system": ""} | JSON sözlüğü |
| EncryptionOperation | EnableEncryption, EnableEncryptionFormatAll | dize | 
| KeyEncryptionAlgorithm | 'OAEP RSA', 'RSA-OAEP-256', 'RSA1_5' | dize |
| KeyEncryptionKeyURL | url | dize |
| (optional) KeyVaultURL | url | dize |
| Passphrase | password | dize | 
| SequenceVersion | uniqueidentifier | dize |
| VolumeType | İşletim sistemi, veri, tüm | dize |

## <a name="template-deployment"></a>Şablon dağıtımı

Şablonu dağıtım örneği için bkz: [çalışan bir Linux VM üzerinde şifrelemeyi etkinleştir](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm).

## <a name="azure-cli-deployment"></a>Azure CLI dağıtım

Yönergeler için en [Azure CLI belgeleri](/cli/azure/vm/encryption?view=azure-cli-latest). 

## <a name="troubleshoot-and-support"></a>Sorun giderme ve Destek

### <a name="troubleshoot"></a>Sorun giderme

Sorun giderme için başvurmak [Azure Disk şifrelemesi sorun giderme kılavuzu](../../security/azure-security-disk-encryption-tsg.md).

### <a name="support"></a>Destek

Bu makalede herhangi bir noktada daha fazla yardıma ihtiyacınız olursa, üzerinde Azure uzmanlarıyla iletişime geçebilirsiniz [Azure MSDN ve Stack Overflow forumları](https://azure.microsoft.com/support/community/). Alternatif olarak, bir Azure destek olayına dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) ve Destek Al'ı seçin. Azure desteği hakkında daha fazla bilgi için okuma [Microsoft Azure desteği SSS](https://azure.microsoft.com/support/faq/).

## <a name="next-steps"></a>Sonraki adımlar

VM uzantıları hakkında daha fazla bilgi için bkz: [Linux yönelik sanal makine uzantılarına ve özelliklerine](features-linux.md).