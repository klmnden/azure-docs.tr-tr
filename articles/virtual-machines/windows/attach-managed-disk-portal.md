---
title: Yönetilen bir veri diski bir Windows VM - Azure | Microsoft Docs
description: Azure portalını kullanarak bir Windows VM'ye yönetilen bir veri diski ekleme yapma.
services: virtual-machines-windows
documentationcenter: ''
author: roygara
manager: twooley
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/08/2018
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: e3641960131d23bf5a8e5b2310a09e7a4dbd70b9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64680395"
---
# <a name="attach-a-managed-data-disk-to-a-windows-vm-by-using-the-azure-portal"></a>Azure portalını kullanarak bir Windows VM'ye yönetilen bir veri diski ekleme

Bu makalede, Azure portalını kullanarak bir Windows sanal makinesi (VM) için yeni bir yönetilen veri diski ekleme işlemini göstermektedir. VM boyutunu, iliştirebilirsiniz kaç veri diskinin belirler. Daha fazla bilgi için [sanal makine boyutları](sizes.md).


## <a name="add-a-data-disk"></a>Veri diski ekleme

1. İçinde [Azure portalında](https://portal.azure.com), sol taraftaki menüden **sanal makineler**.
2. Listeden bir sanal makine seçin.
3. Üzerinde **sanal makine** sayfasında **diskleri**.
4. Üzerinde **diskleri** sayfasında **Ekle veri diski**.
5. Açılan menü için yeni disk, seçin **Oluştur disk**.
6. İçinde **yönetilen disk oluşturma** sayfasında, disk için bir ad yazın ve diğer ayarları gerektiği gibi ayarlayın. İşiniz bittiğinde **Oluştur**’u seçin.
7. İçinde **diskleri** sayfasında **Kaydet** VM için yeni disk yapılandırmasını kaydetmek için.
8. Azure disk oluşturur ve sanal makineye iliştirir sonra yeni disk sanal makinenin disk ayarları altında listelenir **veri diskleri**.


## <a name="initialize-a-new-data-disk"></a>Yeni bir veri diski başlatın

1. VM’ye bağlanın.
1. Windows seçin **Başlat** çalışan VM içindeki menü girin **diskmgmt.msc** arama kutusuna. **Disk Yönetimi** konsolu açılır.
2. Disk Yönetimi tanıdığı yeni, başlatılmamış bir disk olması ve **diski başlatma** penceresi görüntülenir.
3. Yeni disk, seçilen ve ardından doğrulamak **Tamam** başlatmak üzere.
4. Yeni disk olarak görünür **ayrılmamış**. Herhangi bir yere sağ tıklatın ve disk **yeni basit birim**. **Yeni Basit Birim Sihirbazı** penceresi açılır.
5. Tüm Varsayılanları, tutma sihirbazdaki adımları izleyin ve select bitirdiğinizde **son**.
6. Kapat **Disk Yönetimi**.
7. Kullanabilmeniz için önce yeni diski biçimlendirmek için gereken bildiren bir açılır pencere görüntülenir. Seçin **diski Biçimlendir**.
8. İçinde **yeni diski Biçimlendir** penceresinde ayarlarını kontrol edin ve ardından **Başlat**.
9. Diskleri biçimlendirme tüm verileri siler olduğunu bildiren bir uyarı görüntülenir. **Tamam**’ı seçin.
10. Biçimlendirme tamamlandıktan sonra seçin **Tamam**.

## <a name="next-steps"></a>Sonraki adımlar

- Ayrıca [PowerShell kullanarak veri diski ekleme](attach-disk-ps.md).
- Uygulamanızın kullanması gerekirse *D:* verileri depolamak için sürücü, yapabilecekleriniz [Windows geçici diskinin sürücü harfini değiştirme](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
