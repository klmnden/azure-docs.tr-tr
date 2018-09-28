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
ms.date: 06/15/2018
ms.author: delhan
ms.openlocfilehash: 64641f8acfe7b58763756e2a0707fa799ee804b2
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2018
ms.locfileid: "47414678"
---
# <a name="how-to-use-boot-diagnostics-to-troubleshoot-virtual-machines-in-azure"></a>Azure'da sanal makine sorunlarını gidermek için önyükleme tanılama kullanma

İki hata ayıklama özelliği şimdi Azure'da sunulan desteği: konsol çıkışını ve ekran görüntüsü, Azure sanal makinelerini Resource Manager dağıtım modeli için destek. 

Kendi görüntünüzü Azure veya hatta bir platform görüntüleri önyükleme yapma sırasında neden bir sanal makine önyüklenebilir olmayan bir duruma gelmesinin birçok nedeni olabilir. Bu özellikler, kolayca tanılayın ve sanal makinelerinizin önyükleme hatalarını kurtarma sağlar.

Linux sanal makineleri için Portal'dan konsol oturum çıktısını kolayca görüntüleyebilirsiniz. Azure, hem Windows hem de Linux sanal makineleri için hiper yöneticiden VM'nin ekran görmek de sağlar. Bu özelliklerin her ikisi de tüm bölgelerdeki Azure sanal makineler için desteklenir. Not, ekran görüntüleri ve çıktının depolama hesabınızda görünmesi 10 dakika sürebilir.

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

## <a name="enable-diagnostics-on-a-new-virtual-machine"></a>Yeni bir sanal makine üzerinde tanılamayı etkinleştir
1. Azure Portalı'ndan yeni bir sanal makine oluştururken seçin **Azure Resource Manager** dağıtım modeli açılır listesinden:
 
    ![Resource Manager](./media/virtual-machines-common-boot-diagnostics/screenshot3.jpg)

2. İçinde **ayarları**, etkinleştirme **önyükleme tanılaması**ve ardından bu Tanılama dosyalarını yerleştirmek istediğiniz depolama hesabı seçin.
 
    ![VM oluşturma](./media/virtual-machines-common-boot-diagnostics/create-storage-account.png)

    > [!NOTE]
    > Premium depolama hesabı, önyükleme Tanılama özelliğini desteklemiyor. Önyükleme tanılaması için premium depolama hesabı kullanırsanız, sanal Makineyi başlattığınızda StorageAccountTypeNotSupported hata alabilirsiniz.
    >
    > 

3. Bir Azure Resource Manager şablonu dağıtıyorsanız, sanal makine kaynağınıza gidin ve tanılama profili bölümünü ekleyin. "2015-06-15" API sürümü üst bilgisini kullanmayı unutmayın.

    ```json
    {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Compute/virtualMachines",
          … 
    ```

4. Tanılama profili, bu günlükleri yerleştirmek istediğiniz depolama hesabını seçmenize olanak sağlar.

    ```json
            "diagnosticsProfile": {
                "bootDiagnostics": {
                "enabled": true,
                "storageUri": "[concat('http://', parameters('newStorageAccountName'), '.blob.core.windows.net')]"
                }
            }
            }
        }
    ```

Önyükleme tanılaması etkin bir örnek sanal makine dağıtmak için buradaki depomuza göz denetleyin.

## <a name="enable-boot-diagnostics-on-existing-virtual-machine"></a>Mevcut bir sanal makinede önyükleme tanılamasını etkinleştirme 

Mevcut bir sanal makinede önyükleme tanılamasını etkinleştirmek için aşağıdaki adımları izleyin:

1. Oturum [Azure portalında](https://portal.azure.com)ve ardından sanal makineyi seçin.
2. İçinde **destek + sorun giderme**seçin **önyükleme tanılaması** > **ayarları**, durumunu değiştir **üzerinde**ve ardından bir depolama hesabı seçin. 
4. Önyükleme tanılama seçeneğinin seçili olduğundan emin olun ve ardından değişikliği kaydedin.

    ![Mevcut VM’yi güncelleştirme](./media/virtual-machines-common-boot-diagnostics/enable-for-existing-vm.png)

3. Etkili olması için VM'yi yeniden başlatın.


