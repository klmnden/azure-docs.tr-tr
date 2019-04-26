---
title: Özel ölçüm kullanarak azure'da otomatik ölçeklendirme
description: Kaynağınızı azure'da özel bir ölçü olarak ölçeklendirmeyi öğrenin.
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/07/2017
ms.author: ancav
ms.subservice: autoscale
ms.openlocfilehash: e6423f2ce3659fd3dd738dcc8a990261bc7bf60c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60334373"
---
# <a name="get-started-with-auto-scale-by-custom-metric-in-azure"></a>Otomatik ölçeklendirme ile azure'da özel bir ölçü olarak kullanmaya başlayın
Bu makalede, Azure portalındaki özel ölçüm kaynağınızı ölçeklendirilmesine açıklar.

Azure İzleyici otomatik ölçeklendirme için yalnızca geçerlidir [sanal makine ölçek kümeleri](https://azure.microsoft.com/services/virtual-machine-scale-sets/), [Cloud Services](https://azure.microsoft.com/services/cloud-services/), [App Service - Web Apps](https://azure.microsoft.com/services/app-service/web/), ve [APIManagementHizmetleri](https://docs.microsoft.com/azure/api-management/api-management-key-concepts).

## <a name="lets-get-started"></a>Sağlar kullanmaya başlayın
Bu makalede, application ınsights ile yapılandırılmış bir web uygulaması sahibi olduğunuzu varsayar. Zaten yoksa, şunları yapabilirsiniz [ASP.NET Web siteniz için Application ınsights'ı ayarlama][1]

- Açık [Azure portalı][2]
- Sol gezinti bölmesinde Azure İzleyici simgesine tıklayın.
  ![Azure İzleyicisi'ni başlatma][3]
- Otomatik ölçeklendirme ayarı için otomatik ölçeklendirme, geçerli bir otomatik ölçeklendirme durumu ile birlikte geçerli olan tüm kaynakları görüntülemek için tıklatın ![Azure İzleyici otomatik ölçeklendirme keşfedin][4]
- Azure İzleyicisi'nde 'Otomatik ölçeklendirme' dikey penceresini açın ve ölçeklendirmek için istediğiniz bir kaynak seçin
  > Not: App ınsights yapılandırılmış olan bir web uygulaması ile ilişkili bir app service planı aşağıdaki adımları kullanın.
- Kaynak için ölçek ayarı dikey penceresinde, özel olarak geçerli örnek sayısını 1 olduğuna dikkat edin. 'Otomatik ölçeklendirmeyi etkinleştirme üzerinde' tıklayın.
  ![Yeni web uygulaması için ölçek ayarı][5]
- Ölçek ayarı ve "Bir kural Ekle'yi" tıklayın için bir ad sağlayın. Bir bağlam bölmesinde sağ tarafı olarak açılır ölçek kuralı seçeneklerini dikkat edin. Varsayılan olarak, kaynak CPU yüzdesi % 70'i aşarsa, örnek sayınız 1 ile ölçeklendirme seçeneği ayarlar. "Application Insights" üst ölçüm kaynağı değiştirme, app ınsights kaynağı 'Resource' açılır menüde seçip dayalı özel ölçüm ölçeklendirmek istediğiniz üzerinde.
  ![Özel bir ölçüme göre ölçeklendirin][6]
- Yukarıdaki adıma benzer şekilde, ölçeklendirme ve özel ölçüm eşiğin altında ise ölçek sayısı 1 ile azaltma ölçek kuralı ekleyin.
  ![CPU üzerinde göre ölçeklendirin][7]
- Ayarladığınız örnek limitleri. Özel ölçüm dalgalanmaları bağlı olarak 2-5 örnekleri arasında ölçeklendirme istiyorsanız, örneğin, 'minimum' için '2', 'maksimum.', '5' ve 'default' için ' 2' ayarlayın
  > Not: Kaynak ölçümlerin okunmasıyla bir sorun olduğunu ve geçerli kapasite, varsayılan kapasitenin altında olduğu durumda, ardından kaynak kullanılabilirliğini sağlamak için otomatik ölçeklendirme varsayılan değer olarak ölçeklendirilir. Geçerli kapasite, varsayılan kapasiteden zaten yüksektir, otomatik ölçeklendirme, ölçeği değil.
- 'Kaydet' tıklayın

Tebrikler. Artık web uygulamanıza özel bir ölçüme göre otomatik ölçeklendirme ayarını ölçek oluşturuldu.

> Not: Aynı adımları VMSS veya Bulut hizmet rolü ile kullanmaya başlamak için geçerlidir.

<!--Reference-->
[1]: https://docs.microsoft.com/azure/application-insights/app-insights-asp-net
[2]: https://portal.azure.com
[3]: ./media/autoscale-custom-metric/azure-monitor-launch.png
[4]: ./media/autoscale-custom-metric/discover-autoscale-azure-monitor.png
[5]: ./media/autoscale-custom-metric/scale-setting-new-web-app.png
[6]: ./media/autoscale-custom-metric/scale-by-custom-metric.png
[7]: ./media/autoscale-custom-metric/autoscale-setting-custom-metrics-ai.png

