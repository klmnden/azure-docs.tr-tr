---
title: Uygulama Öngörüler genel bakış Panosu'na | Microsoft Docs
description: Azure Application Insights ve genel bakış Panosu'na işlevselliğiyle uygulamalarını izleyin.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/10/2018
ms.author: mbullwin
ms.openlocfilehash: bccb56ad45d9054a437bf2d85e74a8d81fbc3db1
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
---
# <a name="overview-dashboard-preview"></a>Genel Bakış Panosu'na (Önizleme)

Application Insights her zaman, uygulamanızın sistem durumunu ve performansını hızlı, bir bakışta değerlendirmesi izin vermek için bir Özet genel bakış bölmesinde sağlanan. Yeni bir hızlı daha esnek deneyimi 15 Mayıs 2018 üzerinde başlayan bir önizleme olarak yayınlanacaktır. 29 Mayıs 2018 üzerinde Klasik genel bakış deneyimi kullanımdan kaldırılacaktır.

## <a name="how-do-i-test-out-the-new-experience"></a>Yeni deneyimi test nasıl?

Yeni deneyimi Application Insights altında görünmesine 15 Mayıs başlar: _Araştır_ > _genel bakış (Önizleme)_.

![Genel Önizleme](.\media\app-insights-overview-dashboard\01.png)

Bu yeni varsayılan genel bakış Panosu'na başlatılır:

![Genel Bakış önizleme bölmesinde](.\media\app-insights-overview-dashboard\02.png)

## <a name="better-performance"></a>Daha iyi performans

Zaman aralığı seçim için basit bir tek tıklamayla arabirim basitleştirilmiştir.

![Zaman aralığı](.\media\app-insights-overview-dashboard\04.png)

Genel performansı önemli ölçüde çıkarılmıştır. Dinamik olarak KPI döşeme güncelleştirme her varsayılan karşılık gelen Application Insights özellik bağlıdır. Örneğin, başarısız olan istekler seçerek başlatacak _hataları_ bölmesi:

![Başarısızlıklar](.\media\app-insights-overview-dashboard\03.png)

## <a name="application-dashboard"></a>Uygulama panosu

Uygulama Panosu uygulama sağlık ve performans tamamen özelleştirilebilir bölmeden görünümünü sağlamak için Azure içinde varolan Pano teknolojisini kullanır.

Varsayılan Pano seçin erişmek için _uygulama Panosu_ sol üst köşedeki.

![Pano görünümü](.\media\app-insights-overview-dashboard\0009.png)

Bu, ilk kez kullanıyorsanız Pano erişme, varsayılan görünüm başlatılır:

![Pano görünümü](.\media\app-insights-overview-dashboard\06.png)

Bunu istiyorsanız varsayılan görünüm kullanmaya devam edebilir, ancak de ekleyebilirsiniz ve en iyi panodan Sil ekibinizin gereksinimlerine uyacak.

Gezinmek için genel bakış deneyimi geriye yalnızca seçin:

![Genel Bakış düğmesi](.\media\app-insights-overview-dashboard\07.png)

Ayrıca adlandırılan yeni bir düğme olan _PIN bölümleri_.

![Genel Bakış düğmesi](.\media\app-insights-overview-dashboard\008.png)

Bu eski genel bakış deneyiminde döşeme hiçbirini yapmanıza imkan tanıyan Klasik genel bakış'den az bilinen bir özellik çoğaltır _(uyarıları, kullanılabilirlik, Canlı ölçümleri, kullanım, öngörülü algılama ve uygulama eşlemesi)_ ve bunları özel panolar ekleyin. 

Varsayılan durumunda _uygulama Panosu_ bu kutucuklar zaten ekledik. Ancak ek özel panolar oluşturursanız veya birisi ekibinizin Klasik döşeme siler ve geri eklemek istiyorsanız _PIN bölümleri_ herhangi bu işlevsellik sağlar yeri bulma kolaydır.
