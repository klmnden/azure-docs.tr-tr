---
title: Azure Application Insights | Microsoft Docs
description: ''
services: application-insights
documentationcenter: ''
author: eternovsky
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 08/08/2018
ms.reviewer: mbullwin
ms.author: Evgeny.Ternovsky
ms.openlocfilehash: cbb144cc8aac6dc8e90d196147b0c154471b7239
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60901501"
---
# <a name="correlating-application-insights-data-with-custom-data-sources"></a>Application Insights verileri özel veri kaynakları ile ilişkilendirme

Application Insights, birçok farklı veri türlerini toplar: özel durumlar, izlemeler, sayfa görüntülemeleri ve diğerleri. Bu genellikle, uygulamanızın performans, güvenilirlik ve kullanım araştırmak için yeterli olmakla birlikte, bazı durumlarda tamamen özel diğer veri kümeleri için uygulama anlayışları'nda depolanan verileri ilişkilendirmek kullanışlıdır.

Özel veri isteyebileceğiniz bazı durumlar şunlardır:

- Veri zenginleştirme veya arama tabloları: Örneğin, sunucu ve içinde bulunabilir Laboratuvar konumunu sahip bir sunucu adı tamamlar 
- Application Insights veri kaynaklarıyla bağıntı: Örneğin, web mağazası bilgileri satın alma yerine getirme hizmetinizden ne kadar doğru sevkiyat zaman tahminleri belirlemek için üzerinde bir satın alma hakkında verilerin ilişkilendirilmesini olan 
- Tamamen özel veri: birçok müşterimizin sevdiğiniz sorgu dili ve Application Insights yedekler Azure izleyici günlüğü platformun performansı ve Application ınsights'ı hiç ilgili verileri sorgulamak için kullanmak istiyorsanız. Örneğin, bir akıllı ana yükleme olarak bir parçası olarak Güneş paneli performansını izlemek için ana hatlarıyla belirtilen [burada]( https://blogs.catapultsystems.com/cfuller/archive/2017/10/04/using-log-analytics-and-a-special-guest-to-forecast-electricity-generation/).

## <a name="how-to-correlate-custom-data-with-application-insights-data"></a>Application Insights verilerini özel verilerle ilişkilendirmek nasıl 

Application Insights güçlü Azure izleyici günlüğü platformu tarafından desteklenen olduğundan, bu verileri almak için Azure İzleyici'nın tüm gücünden kullanabilmek için duyuyoruz. Ardından, biz Azure İzleyici günlüklerine kullanımımıza açıktır verilere sahip özel bu verilerin bağıntısını "birleştirme" işleci kullanarak sorgu yazacağız. 

## <a name="ingesting-data"></a>Veri alma

Bu bölümde, biz verilerinizi Azure İzleyici günlüklerine alma inceleyin.

İzleyerek yeni bir Log Analytics çalışma alanı zaten yoksa, sağlama [bu yönergeleri](../learn/quick-collect-azurevm.md) ile ve "çalışma alanı oluşturma" adım dahil.

Azure İzleyici ile günlük veri göndermeye başlamak için. Çeşitli seçenekler mevcuttur:

- Zaman uyumlu bir mekanizma, ya da doğrudan çağırabilirsiniz [veri toplayıcı API'si](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-collector-api) veya bizim mantıksal uygulama bağlayıcısı kullanın – yalnızca "Azure Log Analytics" bulun ve "Veri gönderme" seçeneğini seçin:

  ![Ekran görüntüsü seçin ve eylem](./media/custom-data-correlation/01-logic-app-connector.png)  

- Zaman uyumsuz bir seçenek için veri toplayıcı API'sini kullanarak bir işleme işlem hattı oluşturmak için kullanın. Bkz: [bu makalede](https://docs.microsoft.com/azure/log-analytics/log-analytics-create-pipeline-datacollector-api) Ayrıntılar için.

## <a name="correlating-data"></a>Verileri ilişkilendirme

Application Insights Azure izleyici günlüğü platformuna dayanır. Bu nedenle kullanabiliriz [kaynaklar arası birleştirmeler](https://docs.microsoft.com/azure/log-analytics/log-analytics-cross-workspace-search) Application Insights verilerimizi biz alınan Azure İzleyici ile tüm veriler ilişkilendirmek için.

Örneğin, size sunduğumuz Laboratuvar Envanter ve konumları "myLA" adlı bir Log Analytics çalışma alanında "LabLocations_CL" adlı bir tabloya alabilen. Ardından "myAI" adlı Application Insights uygulamada izlenen gönderdiğimiz istekleri gözden geçirin ve yukarıda açıklanan özel tablosunda depolanan bu makinelerde konumları isteklerine hizmet makine adları bağıntısını istedik, aşağıdaki sorgusunun çalıştırılacağı Application ınsights'ı veya Azure İzleyici Bağlam:

```
app('myAI').requests
| join kind= leftouter (
    workspace('myLA').LabLocations_CL
    | project Computer_S, Owner_S, Lab_S
) on $left.cloud_RoleInstance == $right.Computer
```

## <a name="next-steps"></a>Sonraki Adımlar

- Kullanıma [veri toplayıcı API'sini](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-collector-api) başvuru.
- Daha fazla bilgi için [kaynaklar arası birleştirmeler](https://docs.microsoft.com/azure/log-analytics/log-analytics-cross-workspace-search).
