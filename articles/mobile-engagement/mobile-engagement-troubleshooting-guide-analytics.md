---
title: "Sorun giderme kılavuzu - Analytics'i azure Mobile Engagement"
description: "Azure Mobile Engagement Analytics, izleme, Segment ve Pano sorunlarını giderme"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 04a7020a-ad74-4491-be69-0bd574890029
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: e30c9ac0a8421ffcf4fc3e2548cfd7ac49701900
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshooting-guide-for-analytics-monitoring-segmentation-and-dashboard-issues"></a>Analiz, izleme, Segment ve Pano sorunları için sorun giderme kılavuzu
Olası sorunlar aşağıda verilmiştir Azure Mobile Engagement'ın uygulamaları, cihazlar ve kullanıcılar hakkında bilgileri nasıl toplar karşılaşabilirsiniz.

## <a name="missingdelayed-information"></a>Eksik Gecikmeli bilgi
### <a name="issue"></a>Sorun
* Bilgi Analytics, Segment veya Pano görünmesini içinde ertelendi.
* İzleme bilgileri eksik.
* Analytics, Segment veya pano bilgileri eksik.
* Segment basarsa sınırlar.

### <a name="causes"></a>Neden olur.
* Analytics bir API İzleyici API'si kullanabilir ve kesimleri herhangi bir veri, kullanıcı Arabiriminden eksik olup olmadığını görmek için API API'leri aracılığıyla görülebilir.
* Ardından Azure Mobile Engagement SDK'sını uygulamanıza doğru tümleşik değilse Analytics, Segment, izleme veya panolar bilgileri görmeye olmayacaktır.
* Parçaları olamaz oluşturulduktan sonra kesimleri yalnızca "kopya" (kopyalanır) veya "(silinir) yok" değiştirilebilir. Segmentler yalnızca 10 ölçüt içerebilir.
* İzleme eksik bilgilerinin test etmek için en iyi yolu bir test cihazı Kurulum, kaldırma ve/veya uygulama test aygıtta yeniden yüklemektir.
* Bilgileri Analytics, Segment veya panolar için 24 saatte bir yenilenir.
* Segment önceki bilgilere dayalı olsa bile oluşturulduktan sonra 24 saate kadar yeni kesimleri bilgilerinde görüntülenmeyebilir.
* Kullanıcı arabiriminde analiz verilerinizi filtrelemeyi uygulamanızı sürümünden bağımsız olarak bu türdeki tüm örnekler (örneğin gösterir "Adına göre filtre kilitleniyor" sürüm 1 ve sürüm 2, uygulamanızın gösterir).
* Yanlış ayarlanmış tarih, telefon sahip bir kullanıcı yanlış bir zaman diliminde gösterebilirsiniz şekilde süre analiz için kullanıcıların cihaz ayarlarını tarihinden temel alır.
* "Test etmek için" düğmesini kullandığınızda veriler günlüğe kaydedilir hiçbir sunucu tarafı iter, veriler yalnızca gerçek anında iletme kampanyalarını için günlüğe kaydedilir.

## <a name="cant-locate-items-in-ui"></a>Öğeleri kullanıcı Arabiriminde bulunamıyor
### <a name="issue"></a>Sorun
* Bazı yerleşik göre kesimleri oluşturulamıyor veya özel uygulama bilgisi ölçütleri etiketi.
* Bazı yerleşik bulunamıyor veya özel uygulama bilgisi ölçütleri Analytics, izleme veya panolar etiketi.
* Veri analizi, izleme, Segment veya panolar yorumlayamayacağı.

### <a name="causes"></a>Neden olur.
* Bazı öğeler oluşturulur ve uygulama bilgisi etiketleri yalnızca itme ölçüt olarak kullanılabilir, ancak Analytics, izleme veya Pano kesimi eklenebilir ya da görünür olmayabilir. 
* Yerleşik öğeleri ve bir kesimine eklenemez uygulama bilgisi etiketler, bir kesiminde dayalı hedefleme olarak aynı işlevi gerçekleştirmek için her kampanya ölçütünde hedefleme Kurulum listesine gerekir.
* Daha fazla yardım için Azure Mobile Engagement UI Analytics, izleme, Segment ve panolar bölümlerindeki bağlam menülerini bakın ve bilgileri nasıl.

## <a name="crash-troubleshooting"></a>Sorun giderme kilitlenme
### <a name="issue"></a>Sorun
* Uygulama analizi, izleme veya Pano görünmesini kilitleniyor.

### <a name="causes"></a>Neden olur.
* Sorun giderme için uygulama analizi, izleme veya Pano görülen çöküyor SDK'ın önceki sürümleri ile ilgili bilinen sorunlar için sürüm notları denetlemek emin olun.
* Daha fazla sorun giderme için bir test cihazı uygulamaların yüklü olduğu ve cihaz Kimliğinizi Azure Mobile Engagement UI "İzleyici – olayları" bölümündeki Ara olay uygulama kilitlenmelerine gerçekleştirin. Sonra uygulamanızın kilitlenme ve Azure Mobile Engagement UI "İzleyici – kilitlenme" bölümünde ek bilgileri aramak neden olan olay gerçekleştirin. 

