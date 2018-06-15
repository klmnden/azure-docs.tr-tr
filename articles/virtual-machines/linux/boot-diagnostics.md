---
title: Linux sanal makineleri için tanılama Azure'da önyükleme | Microsoft belge
description: Azure'daki Linux sanal makineleri için iki hata ayıklama özelliklerine genel bakış
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: Deland-Han
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/19/2018
ms.author: delhan
ms.openlocfilehash: 38cc806cb77af60cda10f3aeac2e5ed13b445b8c
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
ms.locfileid: "33941862"
---
# <a name="how-to-use-boot-diagnostics-to-troubleshoot-linux-virtual-machines-in-azure"></a>Linux sanal makineleri azure'da gidermek için önyükleme tanılaması kullanma

Azure’da artık iki hata ayıklama özelliği desteklenmektedir: Azure Sanal Makineler Kaynak Yöneticisi dağıtım modelinde Konsol Çıktısı ve Ekran Görüntüsü desteği vardır. 

Azure’a kendi görüntünüzü ekleme ve hatta bir platform görüntüsünden önyükleme yapma sırasında bir Sanal Makineni önyüklenebilir olmayan bir duruma gelmesinin birçok nedeni olabilir. Bu özellikler sanal makinelerinizin önyükleme hatalarını kolayca tanılamanızı ve düzeltmenizi sağlar.

Linux sanal makineleri için Portal'dan konsol oturum çıktısını kolayca görüntüleyebilirsiniz:

![Azure portalına](./media/boot-diagnostics/screenshot1.png)
 
Ancak, hem Windows hem de Linux sanal makineleri için Azure, hiper yöneticiden VM'nin ekran görüntüsünü görmenizi de sağlar:

![Hata](./media/boot-diagnostics/screenshot2.png)

Bu özelliklerin her ikisi de tüm bölgelerdeki Azure sanal makineler için desteklenir. Not, ekran görüntüleri ve çıktının depolama hesabınızda görünmesi 10 dakika sürebilir.

## <a name="common-boot-errors"></a>Sık karşılaşılan önyükleme hataları

- [Dosya sistemi sorunları](https://support.microsoft.com/help/3213321/linux-recovery-cannot-ssh-to-linux-vm-due-to-file-system-errors-fsck) 
- [Çekirdek sorunları](https://support.microsoft.com/help/4091524/how-recovery-azure-linux-vm-from-kernel-related-boot-related-issues/) 
- [FSTAB hataları](https://support.microsoft.com/help/3206699/azure-linux-vm-cannot-start-because-of-fstab-errors)

## <a name="enable-diagnostics-on-a-new-virtual-machine"></a>Yeni bir sanal makine üzerinde tanılamayı etkinleştir
1. Yeni bir sanal makine Azure portalından oluştururken seçin **Azure Resource Manager** gelen dağıtım modeli açılır:
 
    ![Resource Manager](./media/boot-diagnostics/screenshot3.jpg)

2. İçinde **ayarları**, etkinleştirme **önyükleme tanılama**ve ardından bu tanılama dosyaları yerleştirmek istediğiniz depolama hesabını seçin.
 
    ![VM oluşturma](./media/boot-diagnostics/create-storage-account.png)

    > [!NOTE]
    > Premium depolama hesabı önyükleme Tanılama özelliğini desteklemiyor. Premium depolama hesabı için önyükleme tanılaması kullanıyorsanız, VM başlattığınızda StorageAccountTypeNotSupported hatayı alabilirsiniz. 
    >
    > 

3. Bir Azure Resource Manager şablonu dağıtıyorsanız, sanal makine kaynağınız gidin ve tanılama profil bölümü ekleyin. "2015-06-15" API sürümü üst bilgisini kullanmayı unutmayın.

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

Önyükleme tanılaması etkin bir örnek sanal makine dağıtmak için burada bizim depodaki denetleyin.

## <a name="enable-boot-diagnostics-on-existing-virtual-machine"></a>Varolan sanal makinesindeki önyükleme tanılaması etkinleştir 

Önyükleme Tanılaması, var olan bir sanal makinede etkinleştirmek için aşağıdaki adımları izleyin:

1. Oturum [Azure portal](https://portal.azure.com)ve ardından sanal makineyi seçin.
2. İçinde **destek + sorun giderme**seçin **önyükleme tanılama** > **ayarları**, durumunu değiştir **üzerinde**ve ardından bir depolama hesabı seçin. 
4. Önyükleme tanılama seçeneğinin seçili olduğundan emin olun ve ardından değişikliği kaydedin.

    ![Mevcut VM’yi güncelleştirme](./media/boot-diagnostics/enable-for-existing-vm.png)

3. Etkili olması için VM'yi yeniden başlatın.

## <a name="next-steps"></a>Sonraki adımlar

VM önyükleme tanılaması kullandığınızda bir "günlük dosyasının içeriğini almak başarısız" hatası görürseniz, bkz: [VM önyükleme tanılama günlük hatası içeriğini alamadı](https://support.microsoft.com/help/4094480/failed-to-get-contents-of-the-log-error-in-vm-boot-diagnostics-in-azur).