---
title: Linux için Azure Disk şifrelemesi | Microsoft Docs
description: Azure Disk şifrelemesi için Linux sanal makine uzantısı kullanarak bir sanal makineye dağıtır.
services: virtual-machines-linux
documentationcenter: ''
author: ejarvi
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 06/12/2018
ms.author: danis
ms.openlocfilehash: adf0ce6c8424d2350578d9bfd19c70ebb15fc473
ms.sourcegitcommit: e3d5de6d784eb6a8268bd6d51f10b265e0619e47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2018
ms.locfileid: "39389873"
---
# <a name="azure-disk-encryption-for-linux-microsoftazuresecurityazurediskencryptionforlinux"></a>Linux (Microsoft.Azure.Security.AzureDiskEncryptionForLinux) için Azure Disk şifrelemesi

## <a name="overview"></a>Genel Bakış

Azure Disk şifrelemesi yararlanır dm-crypt alt tam disk şifreleme sağlamak için Linux sisteminde [seçin Azure Linux dağıtımları](https://aka.ms/adelinux).  Bu çözüm, disk şifreleme anahtarlarını ve gizli anahtarları yönetmek için Azure anahtar kasası ile tümleştirilmiştir.

## <a name="prerequisites"></a>Önkoşullar

Önkoşullar tam bir listesi için bkz. [Azure Disk şifrelemesi önkoşulları](
../../security/azure-security-disk-encryption-prerequisites.md).

### <a name="operating-system"></a>İşletim sistemi

Azure Disk şifrelemesi, şu anda select dağıtımları ve sürümlerinde desteklenir.  Bkz: [Azure Disk şifrelemesi hakkında SSS](../../security/azure-security-disk-encryption-faq.md#bkmk_LinuxOSSupport) desteklenen Linux dağıtımları listesi.

### <a name="internet-connectivity"></a>İnternet bağlantısı

Linux için Azure Disk şifrelemesi, erişim için Active Directory, Key Vault, depolama ve paket yönetim uç noktalarını Internet bağlantısı gerektirir.  Daha fazla bilgi için [Azure Disk şifrelemesi önkoşulları](../../security/azure-security-disk-encryption-prerequisites.md).

## <a name="extension-schema"></a>Uzantı şeması

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

### <a name="property-values"></a>Özellik değerleri

| Ad | Değer / örnek | Veri Türü |
| ---- | ---- | ---- |
| apiVersion | 2015-06-15 | tarih |
| Yayımcı | Microsoft.Azure.Security | dize |
| type | AzureDiskEncryptionForLinux | dize |
| typeHandlerVersion | 0.1, 1.1 (VMSS) | int |
| Aadclientıd | xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx | GUID | 
| AADClientSecret | password | dize |
| AADClientCertificate | parmak izi | dize |
| DiskFormatQuery | {"dev_path": "", "name": "","file_system": ""} | JSON sözlüğü |
| EncryptionOperation | EnableEncryption, EnableEncryptionFormatAll | dize | 
| KeyEncryptionAlgorithm | 'OAEP RSA', 'RSA-OAEP-256', 'RSA1_5' | dize |
| KeyEncryptionKeyURL | url | dize |
| KeyVaultURL | url | dize |
| Parola | password | dize | 
| SequenceVersion | benzersiz tanımlayıcı | dize |
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