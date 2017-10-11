---
title: "VM bir veri diski D: sürücüyü hale | Microsoft Docs"
description: "Bir veri sürücüsü olarak D: sürücü kullanabilmesi için bir Windows VM sürücü harflerini değiştirmek açıklar."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 0867a931-0055-4e31-8403-9b38a3eeb904
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: cynthn
ms.openlocfilehash: 7667175c01be2421bfc3badd83b1d8aaeb29bfde
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-d-drive-as-a-data-drive-on-a-windows-vm"></a>Bir Windows VM üzerinde bir veri sürücüsü olarak kullanmak üzere D: sürücü
Uygulamanızın veri depolamak için D sürücüsündeki kullanması gerekirse, farklı bir sürücü harfi için geçici disk kullanmak için bu yönergeleri izleyin. Hiçbir zaman tutmak için gereken verileri depolamak için geçici disk kullanın.

Yeniden boyutlandırma varsa veya **Dur (Deallocate)** bir sanal makine, bu yeni bir hiper yönetici sanal makinenin yerleştirme tetikleyebilir. Bir planlı veya plansız bir bakım olayı de bu yerleştirme tetikleyebilir. Bu senaryoda, geçici disk ilk kullanılabilir sürücü harfi atanır. Özellikle D: sürücü gerektiren bir uygulama varsa, geçici olarak pagefile.sys taşıyın, yeni bir veri diskini ve harf D atayın ve ardından pagefile.sys geçici sürücü taşımak için şu adımları izlemesi gerekir. VM için farklı bir hiper taşınırsa tamamlandıktan sonra Azure geri D: olmayacaktır.

Azure geçici disk nasıl kullandığı hakkında daha fazla bilgi için bkz: [geçici sürücü Microsoft Azure sanal makineler üzerinde anlama](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)

## <a name="attach-the-data-disk"></a>Veri diski Ekle
İlk olarak, sanal makine veri diski eklemeniz gerekir. Portalı kullanarak bunu yapmak için bkz: [Azure portalında bir yönetilen veri diskini nasıl](attach-managed-disk-portal.md).

## <a name="temporarily-move-pagefilesys-to-c-drive"></a>Geçici olarak pagefile.sys C sürücüye taşıyın
1. Sanal makineye bağlanın. 
2. Sağ **Başlat** menü ve select **sistem**.
3. Sol menüde seçin **Gelişmiş Sistem ayarları**.
4. İçinde **performans** bölümünde, select **ayarları**.
5. Seçin **Gelişmiş** sekmesi.
6. İçinde **sanal bellek** bölümünde, select **değişiklik**.
7. Seçin **C** sürücü ve ardından **sistem yönetilen boyutu** ve ardından **ayarlamak**.
8. Seçin **D** sürücü ve ardından **herhangi bir disk belleği dosyası** ve ardından **ayarlamak**.
9. Uygula'yı tıklatın. Bilgisayarın değişikliklerin etkili olması için yeniden başlatılması gerekiyor bir uyarı alırsınız.
10. Sanal makineyi yeniden başlatın.

## <a name="change-the-drive-letters"></a>Sürücü harflerini değiştirme
1. VM yeniden başlatıldıktan sonra geri açın VM oturum açın.
2. Tıklatın **Başlat** menü ve türü **diskmgmt.msc** ve Enter tuşuna basın. Disk Yönetimi başlar.
3. Sağ **D**, geçici depolama sürücüsü ve select **değişiklik sürücü harfi ve yolları**.
4. Sürücü harfi altında yeni bir sürücü gibi seçin **T** ve ardından **Tamam**. 
5. Üzerinde veri diski sağ tıklatın ve seçin **değişiklik sürücü harfi ve yolları**.
6. Sürücü harfi altında seçin **D** ve ardından **Tamam**. 

## <a name="move-pagefilesys-back-to-the-temporary-storage-drive"></a>Pagefile.sys geçici bir depolama sürücüsüne geri dönün
1. Sağ **Başlat** menü ve select **sistem**
2. Sol menüde seçin **Gelişmiş Sistem ayarları**.
3. İçinde **performans** bölümünde, select **ayarları**.
4. Seçin **Gelişmiş** sekmesi.
5. İçinde **sanal bellek** bölümünde, select **değişiklik**.
6. İşletim sistemi sürücüsü **C** tıklatıp **herhangi bir disk belleği dosyası** ve ardından **ayarlamak**.
7. Geçici depolama sürücüsünü seçin **T** ve ardından **sistem yönetilen boyutu** ve ardından **ayarlamak**.
8. **Uygula**'ya tıklayın. Bilgisayarın değişikliklerin etkili olması için yeniden başlatılması gerekiyor bir uyarı alırsınız.
9. Sanal makineyi yeniden başlatın.

## <a name="next-steps"></a>Sonraki adımlar
* Sanal makinenize tarafından kullanılabilir depolama alanı artırabilirsiniz [bir ek veri diski eklemeyi](attach-managed-disk-portal.md).

