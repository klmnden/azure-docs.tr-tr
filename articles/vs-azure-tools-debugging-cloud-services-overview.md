---
title: Bulut hizmetlerini Azure hata ayıklama için Seçenekler | Microsoft Docs
description: Hata ayıklama Azure bulut Hizmetleri
services: visual-studio-online
documentationcenter: n/a
author: mikejo
manager: douge
editor: ''
ms.assetid: 80755da7-8350-4f5c-97ce-2962beabb36d
ms.service: visual-studio-online
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/18/2017
ms.author: mikejo
ms.openlocfilehash: 3d559bea8043ebcde9ffc655b617f711bfe0ea7f
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
ms.locfileid: "30292430"
---
# <a name="learn-the-various-ways-to-debug-an-azure-cloud-service"></a>Bir Azure bulut hizmetinde hata ayıklama için değişik yollarını öğrenin
Bu makale bir Azure bulut hizmetinde hata ayıklama için çeşitli şekillerde bağlantılar sağlar. 

## <a name="debugging-an-azure-cloud-service-in-visual-studio"></a>Bir Azure bulut hizmeti Visual Studio'da hata ayıklama
Zamandan tasarruf edebilirsiniz ve Azure kullanarak para işlem öykünücüsü, bulut hizmeti yerel makinede hata ayıklamak için. Dağıtmadan önce bir hizmet yerel olarak hata ayıklama tarafından, güvenilirlik ve performans işlem zaman için ödeme olmadan artırabilir. Ancak, yalnızca bir bulut hizmeti Azure'da çalıştırdığınızda bazı hatalar oluşabilir. Yalnızca bir bulut hizmeti Azure'da çalıştırdığınızda oluşan hataları hizmetinizi yayımladığınızda, uzaktan hata ayıklama ve ardından bir rol örneği için hata ayıklayıcı ekleme etkinleştirerek hata ayıklaması yapılabilir. Daha fazla bilgi için bkz: [, bulut hizmeti, yerel bilgisayarınızda hata ayıklama](vs-azure-tools-debug-cloud-services-virtual-machines.md#debug-your-cloud-service-on-your-local-computer).

## <a name="using-intellitrace"></a>Using IntelliTrace 
.NET Framework 4.5 rolleri hedeflenen yazmak için Visual Studio Enterprise kullanıyorsanız, bir Azure bulut hizmeti Visual Studio'dan dağıttığınız zaman IntelliTrace etkinleştirebilirsiniz. IntelliTrace ile Visual Studio Azure'da çalışıyormuş gibi uygulamanızda hata ayıklama için kullanabileceğiniz bir günlük sağlar. Daha fazla bilgi için bkz: [yayımlanan bulut hizmeti IntelliTrace ve Visual Studio ile hata ayıklama](http://go.microsoft.com/fwlink/p/?LinkId=623016).

## <a name="remote-debugging"></a>Uzaktan hata ayıklama 
Uzaktan bulut hizmetlerinizi Visual Studio bulut hizmetinden dağıttığınızda zamanında hata ayıklamayı etkinleştirebilirsiniz. Uzak bir dağıtım için hata ayıklamayı etkinleştirmek seçerseniz, her rol örneğini çalıştıran sanal makinelere uzaktan hata ayıklama hizmetleri yüklenir. Gibi bu hizmetleri - `msvsmon.exe` - performansını etkilemez ya da neden ek maliyetleri de. Daha fazla bilgi için bkz: [Azure bulut hizmetinde hata ayıklama](vs-azure-tools-debug-cloud-services-virtual-machines.md#debug-a-cloud-service-in-azure).

## <a name="next-steps"></a>Sonraki adımlar
- [Bir Azure bulut hizmeti ya da VM Visual Studio'da hata ayıklama](./vs-azure-tools-debug-cloud-services-virtual-machines.md) -Azure bulut hizmetlerine hata ayıklama ayrıntılarını öğrenin.