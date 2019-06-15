---
title: Azure'dan bir Windows VHD indirme | Microsoft Docs
description: Azure portalını kullanarak bir Windows VHD indirin.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/01/2018
ms.author: cynthn
ms.openlocfilehash: 3d44a4a723c39bf9780475a2ac3088da94285f6e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61076353"
---
# <a name="download-a-windows-vhd-from-azure"></a>Azure'dan bir Windows VHD indirme

Bu makalede, Azure portalını kullanarak Azure Windows sanal sabit disk (VHD) dosya indirme öğrenin.

## <a name="stop-the-vm"></a>VM’yi durdurma

Çalışan bir VM'ye ekli ise bir VHD Azure'dan karşıdan yüklenemiyor. Bir VHD yüklemek için sanal Makineyi durdurun gerekir. Bir VHD'yi kullanılabilir olarak kullanmak istiyorsanız bir [görüntü](tutorial-custom-images.md) diğer sanal makineleri ile yeni diskler oluşturmak için kullandığınız [Sysprep](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--generalize--a-windows-installation) dosyasında yer alan işletim sistemini genelleştirir ve ardından sanal Makineyi durdurun. VHD için yeni bir örnek var olan bir sanal makine veya veri diski disk olarak kullanmak için yalnızca durdurun ve VM'yi serbest bırakın gerekir.

VHD, diğer sanal makineler oluşturmak için bir görüntü olarak kullanmak için aşağıdaki adımları tamamlayın:

1.  Önceden yapmadıysanız, [Azure portal](https://portal.azure.com/)da oturum açın
2.  [VM'ye bağlanın](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 
3.  VM'de yönetici olarak komut istemi penceresi açın.
4.  Dizinine *%windir%\system32\sysprep* ve sysprep.exe çalıştırın.
5.  Sistem Hazırlama Aracı iletişim kutusunda **girin sistem kullanıma hazır deneyimi (OOBE)** , emin olun **Generalize** seçilir.
6.  Kapatma seçenekleri belirleyin **kapatma**ve ardından **Tamam**. 

VHD için yeni bir örnek var olan bir sanal makine veya veri diski disk olarak kullanmak için aşağıdaki adımları tamamlayın:

1.  Azure portalında Hub menüsünde **sanal makineler**.
2.  Listeden VM'yi seçin.
3.  Sanal makine için dikey penceresinde tıklayın **Durdur**.

    ![VM'yi durdurma](./media/download-vhd/export-stop.png)

## <a name="generate-sas-url"></a>SAS URL'si oluşturun

Oluşturmak istediğiniz VHD dosyasını indirmek için bir [paylaşılan erişim imzası (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) URL'si. URL oluşturulduğunda, sona erme süresini URL'sine atanır.

1.  Sanal makine için dikey pencerenin menüsünde **diskleri**.
2.  VM için işletim sistemi diski seçin ve ardından **dışarı**.
3.  Sona erme saati URL'sini Ayarla *36000*.
4.  Tıklayın **URL'yi oluşturmak**.

    ![URL oluştur](./media/download-vhd/export-generate.png)

> [!NOTE]
> Büyük VHD dosyasını bir Windows Server işletim sistemi yüklemek için yeterli süre sağlamak için varsayılan süre artar. Windows Server işletim sistemine bağlı olarak, bir bağlantı karşıdan yüklemek için birkaç saat içeren bir VHD dosyası bekleyebilirsiniz. Bir VHD için bir veri diski indiriyorsanız, varsayılan zaman yeterli olur. 
> 
> 

## <a name="download-vhd"></a>VHD indirme

1.  Oluşturulan URL'si altında VHD dosyası yükleme'ye tıklayın.

    ![VHD indirme](./media/download-vhd/export-download.png)

2.  Tıklaymanız gerekebilir **Kaydet** yüklemeyi başlatmak için tarayıcıda. VHD dosyası için varsayılan ad *abcd*.

    ![Tarayıcıda Kaydet'e tıklayın](./media/download-vhd/export-save.png)

## <a name="next-steps"></a>Sonraki adımlar

- Bilgi edinmek için nasıl [Azure'a VHD dosyası yüklemek](upload-generalized-managed.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 
- [Bir depolama hesabında yönetilmeyen disklerden yönetilen diskler oluşturma](attach-disk-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
- [PowerShell ile Azure disklerini yönetme](tutorial-manage-data-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

