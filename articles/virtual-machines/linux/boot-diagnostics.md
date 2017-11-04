---
title: "Linux sanal makineleri için tanılama Azure'da önyükleme | Microsoft belge"
description: "Azure'daki Linux sanal makineleri için iki hata ayıklama özelliklerine genel bakış"
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: Deland-Han
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: troubleshooting
ms.date: 08/21/2017
ms.author: delhan
ms.openlocfilehash: 79e412bd7523a55fc7d081121af9434520868880
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
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

- [Dosya sistemi sorunları](https://blogs.msdn.microsoft.com/linuxonazure/2016/09/13/linux-recovery-cannot-ssh-to-linux-vm-due-to-file-system-errors-fsck-inodes/)
- [Çekirdek sorunları](https://blogs.msdn.microsoft.com/linuxonazure/2016/10/09/linux-recovery-manually-fixing-non-boot-issues-related-to-kernel-problems/)
- [FSTAB hataları](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/ )

## <a name="enable-diagnostics-on-a-new-virtual-machine"></a>Yeni bir sanal makine üzerinde tanılamayı etkinleştir
1. Önizleme Portalı'ndan yeni bir sanal makine oluştururken dağıtım modeli açılır menüsünde **Azure Resource Manager**’ı seçin:
 
    ![Resource Manager](./media/boot-diagnostics/screenshot3.jpg)

2. Burada bu tanılama dosyalarını yerleştirmek istediğiniz depolama hesabını seçmek için izleme seçeneğini yapılandırın.
 
    ![VM oluşturma](./media/boot-diagnostics/screenshot4.jpg)

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

## <a name="update-an-existing-virtual-machine"></a>Mevcut bir sanal makineyi güncelleştirme

Önyükleme tanılaması portalı üzerinden etkinleştirmek için mevcut bir sanal makine Portalı aracılığıyla da güncelleştirebilirsiniz. Önyükleme Tanılaması seçeneğini ve Kaydet’i seçin. Etkili olması için VM'yi yeniden başlatın.

![Mevcut VM’yi güncelleştirme](./media/boot-diagnostics/screenshot5.png)