---
title: Azure Service Fabric Hizmetleri durumda yönetme | Microsoft Docs
description: Tanımlamak ve Service Fabric Hizmetleri hizmet durumunda yönetme hakkında bilgi edinin.
services: service-fabric
documentationcenter: .net
author: masnider
manager: chackdan
editor: ''
ms.assetid: f5e618a5-3ea3-4404-94af-122278f91652
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: e3ab36def2d210bd763f3ce2dc5df155e37e2dba
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60870902"
---
# <a name="service-state"></a>Hizmet durumu
**Hizmet durumu** belleğe veya işlevi için bir hizmet gerektiren disk verileri ifade eder. Bu, örneğin, hizmet okur ve yazar çalışmak üye değişkenleri ve veri yapıları içerir. Hizmet nasıl geliştirilmiştir bağlı olarak dosyalara veya diske depolanmış diğer kaynaklara de içerebilir. Örneğin, dosyalar, bir veritabanı veri ve işlem günlüklerini depolamak için kullanırsınız.

Bir örnek hizmeti olarak bir hesap makinesi düşünelim. Temel hesaplayıcı hizmet, iki sayıyı alır ve kendi toplamını döndürür. Bu hesaplama gerçekleştirmek hiç üye değişkeni veya diğer bilgileri içerir.

Artık aynı hesaplayıcı göz önünde bulundurun, ancak son toplamını döndüren ve depolamak için ek bir yöntem ile bunu hesapladı. Bu hizmet, artık durum bilgisi olan sunulur. Durum bilgisi olan yeni toplam değeri hesaplar ve son hesaplanan toplam söylediğinizde okur, yazar bazı durumu içerdiği anlamına gelir.

Azure Service Fabric'te durum bilgisi olmayan hizmet hizmetlerinden birinin ilk kez çağrılır. İkinci hizmeti, durum bilgisi olan bir hizmet olarak adlandırılır.

## <a name="storing-service-state"></a>Depolama hizmeti durumu
Durum te dış veya durumu düzenleme koduyla birlikte bulunan. Externalization durumu genellikle bir dış veritabanından veya ağ üzerinden veya aynı makinede işlemi dışında farklı makinelerde çalışan başka bir veri deposunda kullanılarak gerçekleştirilir. Hesaplayıcı Örneğimizde, veri deposu, SQL veritabanı veya Azure tablo Store örneği olabilir. Toplam hesaplaması için her istek bu veriler üzerinde bir güncelleştirme gerçekleştirir ve hizmete Mağaza'dan getirilen geçerli değeri değeri sonuç ister. 

Durum durumu işleyen kod ile birlikte bulunan olabilir. Service fabric'te durum bilgisi olan hizmetler, genellikle bu modeli kullanılarak oluşturulur. Service Fabric, bu durum yüksek oranda kullanılabilir, tutarlı ve dayanıklı olduğundan ve Hizmetleri bu şekilde oluşturulan kolayca ölçeklendirebilirsiniz emin olmak için altyapı sağlar.

## <a name="next-steps"></a>Sonraki adımlar
Service Fabric kavramları hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Service Fabric hizmetlerinin kullanılabilirliği](service-fabric-availability-services.md)
* [Service Fabric hizmetlerinin ölçeklenebilirliği](service-fabric-concepts-scalability.md)
* [Service Fabric hizmetlerini bölümleme](service-fabric-concepts-partitioning.md)
* [Service Fabric güvenilir hizmetler](service-fabric-reliable-services-introduction.md)
