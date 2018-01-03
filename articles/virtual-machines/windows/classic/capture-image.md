---
title: "Bir Azure Windows VM görüntüsünü yakalama | Microsoft Docs"
description: "Klasik dağıtım modeliyle oluşturulan bir Azure Windows sanal makinesinin bir görüntüsünü yakalama"
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: a5986eac-4cf3-40bd-9b79-7c811806b880
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: cynthn
ms.openlocfilehash: 66a7cef250890f1b6940f7bc7f3c5ae0ec6340f0
ms.sourcegitcommit: b7adce69c06b6e70493d13bc02bd31e06f291a91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/19/2017
---
# <a name="capture-an-image-of-an-azure-windows-virtual-machine-created-with-the-classic-deployment-model"></a>Klasik dağıtım modeliyle oluşturulan bir Azure Windows sanal makinesinin bir görüntüsünü yakalama
> [!IMPORTANT]
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli kullanarak yer almaktadır. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. Resource Manager modeli için bilgi [genelleştirilmiş VM Azure ile yönetilen bir görüntüsünü yakalama](../capture-image-resource.md).
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

Bu makalede, diğer sanal makineler oluşturmak için bir resim olarak kullanabilmek için Windows çalıştıran bir Azure sanal makinesi yakalama gösterilmektedir. Bu görüntü, işletim sistemi diski ve sanal makineye bağlı olan veri disklerinin içerir. Diğer görüntü kullanan sanal makineleri oluşturduğunuzda ağ yapılandırmalarını ayarlamanız gerekir böylece ağ yapılandırmaları içermez.

Azure depolar altındaki görüntüyü **VM görüntüleri (Klasik)**, **işlem** tüm Azure hizmetlerini görüntülediğinizde listelenen hizmet. Bu, yüklediğiniz tüm görüntüleri depolandığı aynı yerdir. Görüntüleri hakkında daha fazla ayrıntı için bkz: [sanal makineler için görüntüleri hakkında](about-images.md?toc=%2fazure%2fvirtual-machines%2fWindows%2fclassic%2ftoc.json).

## <a name="before-you-begin"></a>Başlamadan önce
Bu adımlarda, bir Azure sanal makinesi oluşturulur ve yapılandırılmış olan veri disklerinin ekleme de dahil olmak üzere işletim sistemi varsayılmaktadır. Bunu henüz yapmadıysanız oluşturma ve sanal makine hazırlanıyor hakkında bilgi için aşağıdaki makalelere bakın:

* [Bir görüntüden sanal makine oluşturma](createportal.md)
* [Nasıl bir sanal makineye bir veri diski Ekle](attach-disk.md)
* Sunucu rolleri Sysprep ile desteklendiğinden emin olun. Daha fazla bilgi için bkz: [sunucu rolleri için Sysprep desteği](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).

> [!WARNING]
> Bu işlem, yakalandıktan sonra özgün sanal makine siler.
>
>

Bir Azure sanal makinesi görüntüsünü yapılandırmadan önce hedef sanal makine yedeklenebilir önerilir. Azure sanal makineleri Azure Yedekleme kullanılarak yedeklenebilir. Ayrıntılar için bkz. [Azure sanal makinelerini yedekleme](../../../backup/backup-azure-arm-vms.md). Sertifikalı iş ortaklarının sunduğu başka çözümler vardır. Şu anda hangi çözümlerin kullanılabilir olduğunu öğrenmek için Azure Market’te arama yapın.

## <a name="capture-the-virtual-machine"></a>Sanal makine yakalanamadı
1. İçinde [Azure portal](http://portal.azure.com), **Bağlan** sanal makineye. Yönergeler için bkz: [Windows Server çalıştıran bir sanal makine için oturum açma][How to sign in to a virtual machine running Windows Server].
2. Yönetici olarak bir komut istemi penceresi açın.
3. Dizinine değiştirin `%windir%\system32\sysprep`, ve ardından sysprep.exe çalıştırın.
4. **Sistem Hazırlama aracı** iletişim kutusu görüntülenir. Şunları yapın:

   * İçinde **sistem temizleme eylemi**seçin **girin sistem Out-of-Box deneyimi (OOBE)** emin olun **Generalize** denetlenir. Sysprep kullanma hakkında daha fazla bilgi için bkz: [kullanım Sysprep nasıl: Giriş][How to Use Sysprep: An Introduction].
   * İçinde **kapatma seçenekleri**seçin **kapatma**.
   * **Tamam**’a tıklayın.

   ![Sysprep'i çalıştırın](./media/capture-image/SysprepGeneral.png)
5. Azure portalında sanal makinenin durumunu değiştiren sanal makine, Sysprep kapandıktan **durduruldu**.
6. Azure portalında tıklatın **sanal makineleri (Klasik)** ve yakalamak istediğiniz sanal makineyi seçin. **VM görüntüleri (Klasik)** grubu altında listelenen **işlem** görüntülediğinizde **daha fazla hizmet**.

7. Komut çubuğunda **yakalama**.

   ![Sanal makine yakalama](./media/capture-image/CaptureVM.png)

   **Sanal makine yakalanamadı** iletişim kutusu görüntülenir.

8. İçinde **görüntü adı**, yeni görüntüsü için bir ad yazın. İçinde **görüntü etiketi**, yeni görüntüsü için bir etiket yazın.

9. Tıklatın **sanal makinede Sysprep çalıştırdım**. Adım 3-5 Sysprep ile eylemler için bu onay kutusunu gösterir. Bir görüntü _gerekir_ genelleştirilmiş bir Windows Server görüntüsü özel resimler kümenize eklemeden önce Sysprep çalıştırarak.

10. Yakalama işlemi tamamlandıktan sonra yeni görüntüyü içinde etkin hale **Market**, **işlem**, **VM görüntüleri (Klasik)** kapsayıcı.

    ![Görüntü yakalama başarılı](./media/capture-image/VMCapturedImageAvailable.png)

## <a name="next-steps"></a>Sonraki adımlar
Sanal makineler oluşturmak için kullanılacak görüntüyü hazırdır. Bunu yapmak için bir sanal makine seçerek oluşturacaksınız **daha fazla hizmet** Hizmetleri menüsünün alt menü öğesi **VM görüntüleri (Klasik)** içinde **işlem** grubu. Yönergeler için bkz: [bir görüntüden sanal makine oluşturma](createportal.md).

[How to sign in to a virtual machine running Windows Server]:connect-logon.md
[How to Use Sysprep: An Introduction]: http://technet.microsoft.com/library/bb457073.aspx
[Run Sysprep.exe]: ./media/virtual-machines-capture-image-windows-server/SysprepCommand.png
[Enter Sysprep.exe options]: ./media/capture-image/SysprepGeneral.png
[The virtual machine is stopped]: ./media/virtual-machines-capture-image-windows-server/SysprepStopped.png
[Capture an image of the virtual machine]: ./media/capture-image/CaptureVM.png
[Enter the image name]: ./media/virtual-machines-capture-image-windows-server/Capture.png
[Image capture successful]: ./media/virtual-machines-capture-image-windows-server/CaptureSuccess.png
[Use the captured image]: ./media/virtual-machines-capture-image-windows-server/MyImagesWindows.png
