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
ms.openlocfilehash: 355fa90113e931fa3e21df1ccca5736622475bb3
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54810389"
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
    },
    "publisher": "Microsoft.Azure.Security",
    "settings": {
      "AADClientID": "[aadClientID]",
      "EncryptionOperation": "[encryptionOperation]",
      "KeyEncryptionAlgorithm": "[keyEncryptionAlgorithm]",
      "KeyEncryptionKeyURL": "[keyEncryptionKeyURL]",
      "KeyVaultURL": "[keyVaultURL]",
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
| Yayımcı | Microsoft.Azure.Security | dize |
| type | AzureDiskEncryptionForWindows| dize |
| typeHandlerVersion | 1.0, 2.2 (VMSS) | int |
| (isteğe bağlı) Aadclientıd | xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx | GUID | 
| (isteğe bağlı) AADClientSecret | password | dize |
| (isteğe bağlı) AADClientCertificate | parmak izi | dize |
| EncryptionOperation | EnableEncryption | dize | 
| KeyEncryptionAlgorithm | RSA OAEP | dize |
| KeyEncryptionKeyURL | url | dize |
| KeyVaultURL | url | dize |
| SequenceVersion | benzersiz tanımlayıcı | dize |
| VolumeType | İşletim sistemi, veri, tüm | dize |

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