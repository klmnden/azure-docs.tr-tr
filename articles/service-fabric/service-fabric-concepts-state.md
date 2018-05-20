---
title: Definine ve Azure mikro durumunu yönetmek | Microsoft Docs
description: Tanımlayın ve Service Fabric hizmet durumunda yönetme
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: ''
ms.assetid: f5e618a5-3ea3-4404-94af-122278f91652
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 46d2e27b9cdcb03213648982c7e9a0576838bc92
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="service-state"></a>Hizmet durumu
**Hizmet durumu** bellekte veya işlevi için bir hizmet gerektiren disk verilerini gösterir. Bu, örneğin, hizmet okur ve çalışmak yazan üye değişkenleri ve veri yapıları içerir. Hizmet nasıl geliştirilmiştir bağlı olarak dosyalara veya diskte depolanan diğer kaynaklara de içerebilir. Örneğin, dosyaları, veri ve işlem günlüklerini depolamak için bir veritabanı kullanırsınız.

Bir örnek hizmeti olarak şimdi bir hesap makinesi göz önünde bulundurun. Temel hesaplayıcı hizmet iki sayıyı alır ve bunların toplamı döndürür. Bu hesaplamayı gerçekleştiren hiç üye değişkeni veya diğer bilgileri içerir.

Şimdi aynı hesaplayıcı düşünün, ancak depolamak ve son toplam döndürmek için ek bir yöntem ile hesaplanan. Bu hizmet durum bilgisi olan sunulmuştur. Durum bilgisi olan yeni bir toplam hesaplar ve son hesaplanan toplam döndürülecek söylediğinizde okur yükleyen Yazar bazı durumu içerdiği anlamına gelir.

Azure Service Fabric içinde ilk hizmeti durumsuz bir hizmet olarak adlandırılır. İkinci hizmeti durum bilgisi olan bir hizmet olarak adlandırılır.

## <a name="storing-service-state"></a>Hizmetinin durumunu saklama
Durum ya da externalized veya durumu düzenleme kod ile birlikte bulunabilir. Externalization durumunun genellikle dış veritabanı veya ağ üzerinden veya aynı makine üzerindeki işlemi dışında farklı makinelerde çalışan başka bir veri deposunda kullanılarak yapılır. Hesaplayıcı Örneğimizde, veri deposunda bir SQL veritabanına veya Azure tablo deposu örneği olabilir. Toplam işlem için her istek bu veriler üzerinde bir güncelleştirme gerçekleştirir ve değeri sonucu deposundan çekilen geçerli değeri döndürmek için hizmete ister. 

Durum ayrıca durumu işleyen kodu ile birlikte bulunabilir. Durum bilgisi olan Service Fabric Hizmetleri'nde genellikle bu modeli kullanılarak oluşturulur. Service Fabric bu durum yüksek oranda kullanılabilir, tutarlı ve dayanıklı olduğundan ve Hizmetleri bu şekilde yerleşik kolayca ölçeklendirebilirsiniz emin olmak için altyapı sağlar.

## <a name="next-steps"></a>Sonraki adımlar
Service Fabric kavramları hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Service Fabric hizmetlerin kullanılabilirliğini](service-fabric-availability-services.md)
* [Service Fabric Hizmetleri ölçeklenebilirliği](service-fabric-concepts-scalability.md)
* [Service Fabric Hizmetleri bölümlendirme](service-fabric-concepts-partitioning.md)
* [Service Fabric güvenilir hizmetler](service-fabric-reliable-services-introduction.md)
