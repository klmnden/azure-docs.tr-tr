---
title: Yönetilen bir veri diski bir Windows VM - Azure | Microsoft Docs
description: Azure portalını kullanarak bir Windows VM'ye yönetilen bir veri diski ekleme yapma.
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
ms.date: 10/08/2018
ms.author: cynthn
ms.openlocfilehash: 8d83af114ebb5e5ff78372897d3e08ed592d4012
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2018
ms.locfileid: "49093910"
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

## <a name="use-trim-with-standard-storage"></a>TRIM standart depolama ile kullanma

Standart depolama (HDD) kullanıyorsanız, etkinleştirmelisiniz **TRIM** komutu. **TRIM** komutu, yalnızca gerçekten kullandığınız depolama için faturalandırılırsınız. böylece diskte kullanılmayan blokları atar. Kullanarak **TRIM**, büyük dosyaları oluşturur ve daha sonra bunları silin maliyetlerinden tasarruf edebilirler. 

Denetlenecek **TRIM** ayarı, Windows sanal makinesinde bir komut istemi açın ve aşağıdaki komutu girin:

```
fsutil behavior query DisableDeleteNotify
```

Komut, 0, döndürürse **TRIM** doğru şekilde etkinleştirilir. 1 döndürür, aksi takdirde, etkinleştirmek için aşağıdaki komutu çalıştırın **TRIM**:

```
fsutil behavior set DisableDeleteNotify 0
```

Diskinizden verileri sildikten sonra sağlayabilirsiniz **TRIM** düzgün şekilde çalıştırarak temizleme işlemleri birleştirme ile **TRIM**:

```
defrag.exe <volume:> -l
```

Ayrıca, tüm birim kırpılır emin olmak için birim biçimlendirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Ayrıca [PowerShell kullanarak veri diski ekleme](attach-disk-ps.md).
- Uygulamanızın kullanması gerekirse *D:* verileri depolamak için sürücü, yapabilecekleriniz [Windows geçici diskinin sürücü harfini değiştirme](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
