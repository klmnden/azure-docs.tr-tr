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
ms.openlocfilehash: 892342dfa4407a7ed138ffb004e7854c0cd07b4a
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/27/2018
ms.locfileid: "53798185"
---
# <a name="correlating-application-insights-data-with-custom-data-sources"></a>Application Insights verileri özel veri kaynakları ile ilişkilendirme

Application Insights, birçok farklı veri türlerini toplar: özel durumlar, izlemeler, sayfa görüntülemeleri ve diğerleri. Bu genellikle, uygulamanızın performans, güvenilirlik ve kullanım araştırmak için yeterli olmakla birlikte, bazı durumlarda tamamen özel diğer veri kümeleri için uygulama anlayışları'nda depolanan verileri ilişkilendirmek kullanışlıdır.

Özel veri isteyebileceğiniz bazı durumlar şunlardır:

- Veri zenginleştirme veya arama tabloları: Örneğin, sunucu ve içinde bulunabilir Laboratuvar konumunu sahip bir sunucu adı tamamlar 
- Application Insights veri kaynaklarıyla bağıntı: Örneğin, web mağazası bilgileri satın alma yerine getirme hizmetinizden ne kadar doğru sevkiyat zaman tahminleri belirlemek için üzerinde bir satın alma hakkında verilerin ilişkilendirilmesini olan 
- Tamamen özel veri: birçok müşterimizin sevdiğiniz sorgu dili ve Application Insights yedekler Log Analytics Veri Platformu performansını ve Application ınsights'ı hiç ilgili veri kullanmak istiyorsanız. Örneğin, bir akıllı ana yükleme olarak bir parçası olarak Güneş paneli performansını izlemek için ana hatlarıyla belirtilen [burada]( http://blogs.catapultsystems.com/cfuller/archive/2017/10/04/using-log-analytics-and-a-special-guest-to-forecast-electricity-generation/).

## <a name="how-to-correlate-custom-data-with-application-insights-data"></a>Application Insights verilerini özel verilerle ilişkilendirmek nasıl 

Application Insights güçlü Log Analytics Veri Platformu tarafından desteklenen olduğundan, bu verileri almak için Log Analytics'ın tüm gücünden kullanabilmek için duyuyoruz. Ardından, biz Log Analytics'te kullanımımıza açıktır verilere sahip özel bu verilerin bağıntısını "birleştirme" işleci kullanarak sorgu yazacağız. 

## <a name="ingesting-data"></a>Veri alma

Bu bölümde, biz verilerinizi Log Analytics'e alma inceleyin.

İzleyerek yeni bir Log Analytics çalışma alanı zaten yoksa, sağlama [bu yönergeleri]( https://docs.microsoft.com/azure/log-analytics/log-analytics-quick-collect-azurevm) ile ve "çalışma alanı oluşturma" adım dahil.

Log Analytics'e veri göndermeye başlamak için. Çeşitli seçenekler mevcuttur:

- Zaman uyumlu bir mekanizma, ya da doğrudan çağırabilirsiniz [veri toplayıcı API'si](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-collector-api) veya bizim mantıksal uygulama bağlayıcısı kullanın – yalnızca "Azure Log Analytics" bulun ve "Veri gönderme" seçeneğini seçin:

 ![Ekran görüntüsü seçin ve eylem](./media/custom-data-correlation/01-logic-app-connector.png)  

- Zaman uyumsuz bir seçenek için veri toplayıcı API'sini kullanarak bir işleme işlem hattı oluşturmak için kullanın. Bkz: [bu makalede](https://docs.microsoft.com/azure/log-analytics/log-analytics-create-pipeline-datacollector-api) Ayrıntılar için.

## <a name="correlating-data"></a>Verileri ilişkilendirme

Application Insights ve Log Analytics veri platformda dayanır. Bu nedenle kullanabiliriz [kaynaklar arası birleştirmeler](https://docs.microsoft.com/azure/log-analytics/log-analytics-cross-workspace-search) Application Insights verilerimizi biz alınan Log Analytics'e tüm veriler ilişkilendirmek için.

Örneğin, size sunduğumuz Laboratuvar Envanter ve konumları "myLA" adlı bir Log Analytics çalışma alanında "LabLocations_CL" adlı bir tabloya alabilen. Ardından "myAI" adlı Application Insights uygulamada izlenen gönderdiğimiz istekleri gözden geçirin ve yukarıda açıklanan özel tablosunda depolanan bu makinelerde konumları isteklerine hizmet makine adları bağıntısını istedik, aşağıdaki sorgusunun çalıştırılacağı Application Insights veya Log Analytics içerik:

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
