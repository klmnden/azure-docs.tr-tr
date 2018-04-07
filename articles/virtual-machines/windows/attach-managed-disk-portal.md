---
title: Bir Windows VM - Azure bir yönetilen veri diski Ekle | Microsoft Docs
description: Bir Windows VM Resource Manager dağıtım modelini kullanarak Azure portalında yeni yönetilen veri diski ekleme yapma.
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
ms.date: 12/13/2017
ms.author: cynthn
ms.openlocfilehash: 14721b2f2bc7913c2b7eadfc5ee801a223201ea9
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="how-to-attach-a-managed-data-disk-to-a-windows-vm-in-the-azure-portal"></a>Nasıl bir Windows VM Azure portalında bir yönetilen veri diski kullanıma açın

Bu makalede Windows sanal makineleri Azure portalında yeni bir yönetilen veri diski ekleme gösterilmiştir. Bunu önce bu ipuçlarını gözden geçirin:

* Sanal makinenin boyutunu, iliştirebilirsiniz kaç tane veri diskleri denetler. Ayrıntılar için bkz [sanal makineler için Boyutlar](sizes.md).
* Yeni bir disk için bunu eklediğinizde Azure bunu oluşturduğundan ilk oluşturmanız gerekmez.

Ayrıca [Powershell kullanarak bir veri diskini](attach-disk-ps.md).



## <a name="add-a-data-disk"></a>Bir veri diski Ekle
1. Soldaki menüde tıklatın **sanal makineleri**.
2. Listeden sanal makineyi seçin.
3. Sanal makine sayfasında, tıklatın **diskleri**.
4. Üzerinde **diskleri** sayfasında, **+ Ekle veri diski**.
5. Açılan yeni disk için seçin **oluşturma disk**.
6. İçinde **oluşturma yönetilen disk** sayfasında, disk için bir ad yazın ve diğer ayarları gerektiği gibi ayarlayın. İşiniz bittiğinde **Oluştur**’a tıklayın.
7. İçinde **diskleri** sayfasında, **kaydetmek** VM için yeni disk yapılandırmasını kaydetmek için.
6. Azure disk oluşturur ve sanal makineye iliştirir sonra yeni disk sanal makinenin disk ayarları altında listelenen **veri diskleri**.


## <a name="initialize-a-new-data-disk"></a>Yeni bir veri diski başlatın

1. VM'ye bağlanın.
1. Başlat menüsünü türü ve VM içinde **diskmgmt.msc** ve isabet **Enter**. Disk Yönetimi ek bileşenini açar.
2. Disk Yönetimi tanıdığı yeni ve başlatılmamış bir disk olması ve **diski başlatma** penceresi açılır.
3. Yeni disk seçildiğinden emin olun ve tıklayın **Tamam** başlatmak üzere.
4. Yeni disk olarak görüntülenir **ayrılmamış**. Herhangi bir yere sağ tıklatın ve disk **yeni basit birim**. **Yeni Basit Birim Sihirbazı** açar.
5. Seçim yapıldığında tüm varsayılanları, tutma Sihirbazı aracılığıyla gidin **son**.
6. Disk Yönetimi'ni kapatın.
7. Kullanabilmeniz için önce yeni diski biçimlendirmek için gereken bir açılır pencere alın. Tıklatın **biçimi disk**.
8. İçinde **biçimi yeni disk** iletişim ayarlarını kontrol edin ve ardından **Başlat**.
9. Diskleri biçimlendirme tüm verileri siler, bir uyarı almak, tıklatın **Tamam**.
10. Biçimlendirme tamamlandığında tıklatın **Tamam**.

## <a name="use-trim-with-standard-storage"></a>Standart depolama ile kullanmak üzere KIRPMA

Standart depolama (HDD) kullanırsanız, KIRPMA etkinleştirmeniz gerekir. Yalnızca gerçekten kullandığınız depolama için faturalandırılır şekilde KIRPMA diskte kullanılmayan blokları atar. Büyük dosyaları oluşturmak ve bunları silerseniz bu maliyetleri kaydedebilirsiniz. 

KIRPMA ayarını denetlemek için bu komutu çalıştırabilirsiniz. Windows VM ve türü bir komut istemi açın:

```
fsutil behavior query DisableDeleteNotify
```

Komut 0 döndürürse, KIRPMA doğru şekilde etkinleştirilir. 1 değeri döndürülürse, KIRPMA etkinleştirmek için aşağıdaki komutu çalıştırın:
```
fsutil behavior set DisableDeleteNotify 0
```

Veri diskinizden sildikten sonra düzgün çalıştırarak temizleme KIRPMA işlemleri KIRPMA ile birleştirme sağlayabilirsiniz:

```
defrag.exe <volume:> -l
```

Ayrıca, tüm birim birim biçimlendirme kırpılır emin olabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Uygulamanızı D: kullanması gerekirse, verileri depolamak için sürücü, şunları yapabilirsiniz [Windows geçici disk sürücü harfini değiştirin](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
