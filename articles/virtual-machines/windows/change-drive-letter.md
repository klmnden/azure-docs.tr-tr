---
title: "Bir VM'ye veri diski D: sürücüsünü olun | Microsoft Docs"
description: 'Bir veri sürücüsü olarak D: sürücüsü kullanabilmesi için bir Windows VM için sürücü harflerini değiştirme işlemini açıklamaktadır.'
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: 0867a931-0055-4e31-8403-9b38a3eeb904
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 01/02/2018
ms.author: cynthn
ms.openlocfilehash: 12986068a761b92611c557a0dfcf08905283b8bd
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67719245"
---
# <a name="use-the-d-drive-as-a-data-drive-on-a-windows-vm"></a>Bir Windows VM'de veri sürücüsü olarak D: sürücüsü kullanın.
Uygulama verilerini depolamak için D sürücüsünü kullanması gerekirse, farklı bir sürücü harfi için geçici diski kullanmak için bu yönergeleri izleyin. Hiçbir zaman çalışır durumda bulundurmanıza gerek verileri depolamak için geçici diski kullanın.

Yeniden boyutlandırma, ya da **durdurun (Serbest)** bir sanal makine, bu yeni bir hiper yönetici sanal makinenin yerleştirme tetikleyebilir. Bir planlı veya plansız bir bakım olayı, ayrıca bu yerleştirme tetikleyebilir. Bu senaryoda, geçici disk için ilk kullanılabilir sürücü harfi atanır. Özellikle D: sürücüsü gerektiren bir uygulama varsa, geçici olarak pagefile.sys taşıma, yeni bir veri diski ekleme ve D harfi atamak ve ardından pagefile.sys geçici sürücüyü taşımak için şu adımları izlemesi gerekir. Sanal makine için farklı bir hiper yönetici taşınırsa tamamlandıktan sonra Azure geri D: olmayacaktır.

Azure geçici disk nasıl kullandığı hakkında daha fazla bilgi için bkz. [Microsoft Azure sanal makineler üzerinde geçici sürücüyü anlama](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)

## <a name="attach-the-data-disk"></a>Veri diski ekleme
İlk olarak, sanal makineye veri diski gerekir. Portalı kullanarak bunu yapmak için bkz: [Azure portalında yönetilen veri diski ekleme](attach-managed-disk-portal.md).

## <a name="temporarily-move-pagefilesys-to-c-drive"></a>Geçici olarak pagefile.sys C sürücüye taşıyın
1. Sanal makineye bağlanın. 
2. Sağ **Başlat** menü ve select **sistem**.
3. Sol taraftaki menüde **Gelişmiş Sistem ayarları**.
4. İçinde **performans** bölümünden **ayarları**.
5. Seçin **Gelişmiş** sekmesi.
6. İçinde **sanal bellek** bölümünden **değişiklik**.
7. Seçin **C** sürücü ve ardından **sistem yönetilen boyutu** ve ardından **ayarlamak**.
8. Seçin **D** sürücü ve ardından **disk belleği dosyası** ve ardından **ayarlamak**.
9. Uygula düğmesini tıklatın. Bilgisayar değişikliklerin etkili olabilmesi için yeniden başlatılması gerektiğini bir uyarı alırsınız.
10. Sanal makineyi yeniden başlatın.

## <a name="change-the-drive-letters"></a>Sürücü harfini değiştirme
1. VM yeniden başlatıldıktan sonra VM tekrar açın oturum açın.
2. Tıklayın **Başlat** menü ve türü **diskmgmt.msc** ve Enter tuşuna basın. Disk Yönetimi başlar.
3. Sağ **D**, geçici depolama sürücüsü ve select **sürücü harfi ve Yolları Değiştir**.
4. Sürücü harfi altında yeni bir sürücü gibi seçin **T** ve ardından **Tamam**. 
5. Veri diski üzerinde sağ tıklatın ve seçin **sürücü harfi ve Yolları Değiştir**.
6. Sürücü harfi altında seçin **D** ve ardından **Tamam**. 

## <a name="move-pagefilesys-back-to-the-temporary-storage-drive"></a>Geçici depolama sürücüsüne Pagefile.sys dönün
1. Sağ **Başlat** menü ve select **sistem**
2. Sol taraftaki menüde **Gelişmiş Sistem ayarları**.
3. İçinde **performans** bölümünden **ayarları**.
4. Seçin **Gelişmiş** sekmesi.
5. İçinde **sanal bellek** bölümünden **değişiklik**.
6. İşletim sistemi sürücüsünü seçin **C** tıklatıp **disk belleği dosyası** ve ardından **ayarlamak**.
7. Geçici depolama sürücüsünü seçin **T** ve ardından **sistem yönetilen boyutu** ve ardından **ayarlamak**.
8. **Uygula**'ya tıklayın. Bilgisayar değişikliklerin etkili olabilmesi için yeniden başlatılması gerektiğini bir uyarı alırsınız.
9. Sanal makineyi yeniden başlatın.

## <a name="next-steps"></a>Sonraki adımlar
* Sanal makinenize kullanılabilir depolama alanı artırabilirsiniz [bir ek veri diski eklemeyi](attach-managed-disk-portal.md).

