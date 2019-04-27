---
title: Azure'da VM'ler için önyükleme tanılama | Microsoft belgeleri
description: Azure'da sanal makineler için iki hata ayıklama özelliklerine genel bakış
services: virtual-machines
author: Deland-Han
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines
ms.topic: troubleshooting
ms.date: 10/31/2018
ms.author: delhan
ms.openlocfilehash: 59602977c1b7f6dd0524c6535d8458d3eb1a3f26
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60505910"
---
# <a name="how-to-use-boot-diagnostics-to-troubleshoot-virtual-machines-in-azure"></a>Azure'da sanal makine sorunlarını gidermek için önyükleme tanılama kullanma

Bir sanal makine önyüklenebilir olmayan bir duruma girer birçok nedeni olabilir. Aşağıdaki hata ayıklama özellikleri kullanabileceğinizi Resource Manager dağıtım modeli kullanılarak oluşturulan sanal makinelerinize sorunlarını gidermek için: Konsol çıkışını ve ekran için Azure sanal makineleri destekler. 

Linux sanal makineleri için Portal'dan konsol oturum çıktısını görüntüleyebilirsiniz. Hem Windows hem de Linux sanal makineleri için Azure, Hiper yöneticiden VM'nin ekran görmenizi sağlar. Her iki özellik, tüm bölgelerde Azure sanal makineler için desteklenir. Not, ekran görüntüleri ve çıktının depolama hesabınızda görünmesi 10 dakika sürebilir.

Seçebileceğiniz **önyükleme tanılaması** günlük ve ekran görüntülemek için seçeneği.

![Resource Manager](./media/virtual-machines-common-boot-diagnostics/screenshot1.png)

## <a name="common-boot-errors"></a>Sık karşılaşılan önyükleme hataları

- [0xC000000E](https://support.microsoft.com/help/4010129)
- [0xC000000F](https://support.microsoft.com/help/4010130)
- [0xC0000011](https://support.microsoft.com/help/4010134)
- [0xC0000034](https://support.microsoft.com/help/4010140)
- [0xC0000098](https://support.microsoft.com/help/4010137)
- [0xC00000BA](https://support.microsoft.com/help/4010136)
- [0xC000014C](https://support.microsoft.com/help/4010141)
- [0xC0000221](https://support.microsoft.com/help/4010132)
- [0xC0000225](https://support.microsoft.com/help/4010138)
- [0xC0000359](https://support.microsoft.com/help/4010135)
- [0xC0000605](https://support.microsoft.com/help/4010131)
- [Bir işletim sistemi bulunamadı](https://support.microsoft.com/help/4010142)
- [Önyükleme hatası veya INACCESSIBLE_BOOT_DEVICE](https://support.microsoft.com/help/4010143)

## <a name="enable-diagnostics-on-a-virtual-machine-created-using-the-azure-portal"></a>Azure portalı kullanılarak oluşturulan bir sanal makinede tanılamayı etkinleştirme

Aşağıdaki yordam, Resource Manager dağıtım modeli kullanılarak oluşturulan bir sanal makine için kullanılır.

Üzerinde **Yönetim** sekmesinde **izleme** bölümünde, emin **önyükleme tanılaması** açıktır. Gelen **tanılama depolama hesabı** aşağı açılan listesinde, Tanılama dosyalarını yerleştirmek için bir depolama hesabı seçin.
 
![VM oluşturma](./media/virtual-machines-common-boot-diagnostics/enable-boot-diagnostics-vm.png)

> [!NOTE]
> Premium depolama hesabı, önyükleme Tanılama özelliğini desteklemiyor. Önyükleme tanılaması için premium depolama hesabı kullanırsanız, sanal Makineyi başlattığınızda StorageAccountTypeNotSupported hata alabilirsiniz.
>

### <a name="deploying-from-an-azure-resource-manager-template"></a>Bir Azure Resource Manager şablonu dağıtma

Bir Azure Resource Manager şablonu dağıtıyorsanız, sanal makine kaynağınıza gidin ve tanılama profili bölümünü ekleyin. API sürümü üst bilgisini "2015-06-15" veya sonraki bir sürüme ayarlayın. "2018-10-01" en son sürümüdür.

```json
{
  "apiVersion": "2018-10-01",
  "type": "Microsoft.Compute/virtualMachines",
  … 
```

Tanılama profili, bu günlükleri yerleştirmek istediğiniz depolama hesabını seçmenize olanak sağlar.

```json
    "diagnosticsProfile": {
    "bootDiagnostics": {
    "enabled": true,
    "storageUri": "[concat('https://', parameters('newStorageAccountName'), '.blob.core.windows.net')]"
    }
    }
    }
}
```

Şablonları kullanarak kaynakları dağıtma hakkında daha fazla bilgi için bkz. [hızlı başlangıç: Oluşturma ve Azure portalını kullanarak Azure Resource Manager şablonlarını dağıtma](../../azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal.md).

## <a name="enable-boot-diagnostics-on-existing-virtual-machine"></a>Mevcut bir sanal makinede önyükleme tanılamasını etkinleştirme 

Mevcut bir sanal makinede önyükleme tanılamasını etkinleştirmek için aşağıdaki adımları izleyin:

1. Oturum [Azure portalında](https://portal.azure.com)ve ardından sanal makineyi seçin.
2. İçinde **destek + sorun giderme** bölümünden **önyükleme tanılaması**, ardından **ayarları** sekmesi.
3. İçinde **önyükleme tanılaması** ayarlarını değiştirmek için durum **üzerinde**, gelen ve giden **depolama hesabı** açılır listede, bir depolama hesabı seçin. 
4. Yaptığınız değişikliği kaydedin.

    ![Mevcut VM’yi güncelleştirme](./media/virtual-machines-common-boot-diagnostics/enable-for-existing-vm.png)

Sanal makine için değişikliğin etkili olması için yeniden başlatmanız gerekir.

### <a name="enable-boot-diagnostics-using-the-azure-cli"></a>Azure CLI kullanarak önyükleme tanılamasını etkinleştirme

Var olan Azure sanal makinesinde önyükleme tanılamasını etkinleştirmek için Azure CLI'yı kullanabilirsiniz. Daha fazla bilgi için [az vm önyükleme tanılaması](
https://docs.microsoft.com/cli/azure/vm/boot-diagnostics?view=azure-cli-latest).
