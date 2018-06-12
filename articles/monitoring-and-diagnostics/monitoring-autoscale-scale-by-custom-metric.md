---
title: Özel bir ölçümü kullanarak azure'da otomatik ölçeklendirme
description: Özel bir ölçü Azure tarafından kaynağınız ölçeklendirmek öğrenin.
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/07/2017
ms.author: ancav
ms.component: autoscale
ms.openlocfilehash: 739ef5423f7b1769fa793f0cac5306efa634b781
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35262982"
---
# <a name="get-started-with-auto-scale-by-custom-metric-in-azure"></a>Otomatik ölçek ile özel bir ölçü Azure tarafından kullanmaya başlama
Bu makalede, Azure portal'ın özel bir ölçü tarafından kaynağınız ölçeklendirme açıklar.

Azure İzleyici otomatik ölçek yalnızca sanal makine ölçek kümeleri (VMSS), bulut Hizmetleri, uygulama hizmeti planları ve app service ortamları için geçerlidir. 

# <a name="lets-get-started"></a>Sağlar kullanmaya başlama
Bu makalede, application ınsights ile yapılandırılmış bir web uygulaması olduğunu varsayar. Zaten yoksa, şunları yapabilirsiniz [ASP.NET Web siteniz için Application Insights'ı ayarlama][1]

- Açık [Azure portalı][2]
- Sol gezinti bölmesinde Azure İzleyici simgeyi tıklatın.
  ![Azure izleyicisini Başlat][3]
- Otomatik ölçeklendirme ayarı için Otomatik ölçek geçerli otomatik ölçeklendirme durumunun yanı sıra uygulanabilir tüm kaynakları görüntülemek için tıklatın ![Azure İzleyicisi'nde otomatik ölçek Bul][4]
- Azure İzleyicisi'nde 'Otomatik ölçeklendirme' dikey penceresini açın ve ölçeklendirmek istediğiniz bir kaynak seçin
> Not: App ınsights yapılandırılmış olan bir web uygulaması ile ilişkili bir uygulama hizmeti planı aşağıdaki adımları kullanın.
- Kaynak için ölçek ayarı dikey penceresinde geçerli örnek sayısı 1 olduğuna dikkat edin. 'Etkinleştir otomatik 'ölçeklendirme'yi tıklatın.
  ![Yeni web uygulaması için ölçek ayarı][5]
- Ölçek ayarını ve "Kural Ekle" tıklatıldığında için bir ad sağlayın. Sağ taraftaki içerik bölmesinde olarak açılır ölçek kuralı seçeneklerini dikkat edin. Varsayılan olarak, kaynağın CPU percetage % 70 aşarsa, örnek sayınız 1 ile ölçeklendirme seçeneği ayarlar. "Application Insights" üst ölçüm kaynağı değiştirmek, 'Resource' açılır listede app ınsights kaynağını seçin ve ardından dayalı özel bir ölçü seçin ölçeklendirmek istediğiniz üzerinde.
  ![Özel bir ölçü ölçeklendirin][6]
- Yukarıdaki adıma benzer şekilde, ölçeklendirme ve özel bir ölçü eşiğin altına ise ölçek sayısı 1 ile azaltmak ölçek kuralı ekleyin.
  ![CPU üzerinde göre ölçeklendirin][7]
- Ayarladığınız örnek sınırlar. Özel ölçüm dalgalanmaları bağlı olarak 2-5 örnekleri arasında ölçeklendirmek istiyorsanız, örneğin, 'minimum' için '2', 'maksimum.', '5' ve 'default' 2 '' ayarlayın
> Not: kaynak ölçümleri okunurken bir sorun oluştu ve Geçerli kapasitenin varsayılan kapasitenin altında olduğu durumda, ardından kaynak kullanılabilirliğini sağlamak için otomatik ölçeklendirme varsayılan değere ölçeğini. Geçerli kapasitenin zaten varsayılan kapasitesinden yüksek ise, otomatik ölçeklendirme, ölçekleme sağlamaz.
- 'Kaydet' tıklayın

Tebrikler. Web uygulamanız özel bir ölçü, artık otomatik olarak ayarlanması, Ölçek başarıyla oluşturuldu şimdi ölçeklendirin.

> Not: Aynı adımları VMSS veya Bulut hizmet rolü ile çalışmaya başlamak için geçerlidir.

<!--Reference-->
[1]: https://docs.microsoft.com/azure/application-insights/app-insights-asp-net
[2]: https://portal.azure.com
[3]: ./media/monitoring-autoscale-scale-by-custom-metric/azure-monitor-launch.png
[4]: ./media/monitoring-autoscale-scale-by-custom-metric/discover-autoscale-azure-monitor.png
[5]: ./media/monitoring-autoscale-scale-by-custom-metric/scale-setting-new-web-app.png
[6]: ./media/monitoring-autoscale-scale-by-custom-metric/scale-by-custom-metric.png
[7]: ./media/monitoring-autoscale-scale-by-custom-metric/autoscale-setting-custom-metrics-ai.png
