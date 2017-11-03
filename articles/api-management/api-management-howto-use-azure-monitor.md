---
title: "API Management ile Azure İzleyicisi İzleme | Microsoft Docs"
description: "Azure İzleyicisi'ni kullanarak Azure API Management hizmeti izleme öğrenin."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 2fa193cd-ea71-4b33-a5ca-1f55e5351e23
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 717e033aa4bbd4dd8ebcc727c3f551aee81770dc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="monitor-api-management-with-azure-monitor"></a>Azure İzleyicisi ile İzleyici API Yönetimi
Azure İzleyicisi tüm Azure kaynakları izlemek için tek bir kaynak sağlayan bir Azure hizmetidir. Azure izleme ile görselleştirme, sorgulama yapabilir, yönlendirmek, arşiv ve ölçümleri ve API Management gibi Azure kaynakları'ten gelen günlükleri eylemleri gerçekleştirin. 

Aşağıdaki video Azure İzleyicisi'ni kullanarak API Management izleme gösterir. Azure İzleyicisi hakkında daha fazla bilgi için bkz: [Azure İzleyicisi ile çalışmaya başlama]. 


> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Monitor-API-Management-with-Azure-Monitor/player]
>
>
 
## <a name="metrics"></a>Ölçümler
API Management anda beş ölçümleri yayar ve gelecekte daha ekleme planlıyoruz. Bu ölçümler dakikada, Apı'lerinizi durumunu ve durumunu gerçek zamanlı görünürlük yakın vermiş gösterilen. Ölçümler bir özeti aşağıda verilmiştir:
* : Toplam ağ geçidi API sayısı dönemde istekleri. 
* Başarılı ağ geçidi istekleri: 304, 307 ve 301 (örneğin, 200) daha küçük bir şey de dahil olmak üzere başarılı HTTP yanıt kodları alınan API istek sayısı. 
* Başarısız olan ağ geçidi istekleri: Hatalı HTTP yanıt kodu 400 ve 500'den büyük herhangi bir şey de dahil olmak üzere alınan API istek sayısı.
* Yetkisiz ağ geçidi istekleri: 401, 403 ve 429 dahil olmak üzere HTTP yanıt kodları alınan API istek sayısı. 
* Diğer ağ geçidi istekleri: önceki kategorileri (örneğin, 418) birine ait değil HTTP yanıt kodları alınan API istek sayısı.

API Management hizmetiniz ölçümlerini ya da Azure İzleyicisi'nde tüm Azure kaynaklarına erişim ölçümlerini erişebilir. API Management hizmetiniz ölçümleri görüntülemek için:
1. Azure Portalı'nı açın.
2. API Management hizmetiniz gidin.
3. Tıklatın **ölçümleri**.

![Ölçüm dikey penceresi][metrics-blade]

Ölçümleri kullanma hakkında daha fazla bilgi için bkz: [genel bakış, ölçümleri].

## <a name="activity-logs"></a>Etkinlik Günlükleri
Etkinlik günlükleri, API Management services üzerinde gerçekleştirilen işlemler hakkında bilgi sağlar. Daha önce "denetim günlüklerini" veya "işletimsel logs" olarak bilinirdi. Etkinlik Günlükleri'ni kullanarak, belirleyebilirsiniz "ne, kimin, ne zaman ve" herhangi bir yazma, API Management hizmetlerde yapılan işlemleri (PUT, POST, DELETE) için. 

> [!NOTE]
> Etkinlik günlükleri okuma (GET) işlemleri veya Klasik yayımcı portalında gerçekleştirilen veya özgün yönetim API'leri kullanılarak işlemleri içermez.

API Management hizmetiniz etkinlik günlükleri erişmek veya günlükleri, Azure kaynaklarınızın Azure İzleyicisi'nde erişim. Etkinliğini görüntülemek için API Management hizmetiniz kaydeder:
1. Azure Portalı'nı açın.
2. API Management hizmetiniz gidin.
3. Tıklatın **etkinlik günlüğü**.

![Etkinlik günlükleri dikey penceresi][activity-logs-blade]

Ölçümleri kullanma hakkında daha fazla bilgi için bkz: [etkinlik günlükleri genel bakış].

## <a name="alerts"></a>Uyarılar
Ölçümleri ve etkinlik açtığında göre uyarıları almak üzere yapılandırabilirsiniz. Azure İzleyici tetikler, aşağıdakileri yapmak için bir uyarı yapılandırmanıza olanak sağlar:

* Bir e-posta bildirimi gönder
* bir Web kancası çağırın
* Bir Azure mantıksal uygulamayı çağırmak

API Management hizmetiniz veya Azure İzleyicisi uyarı kuralları yapılandırabilirsiniz. API Management'te yapılandırmak için: 
1. Azure Portalı'nı açın.
2. API Management hizmetiniz gidin.
3. Tıklatın **uyarı kuralları**.

![Uyarı kuralları dikey penceresi][alert-rules-blade]

Uyarıları kullanma hakkında daha fazla bilgi için bkz: [genel bakış, uyarıları].

## <a name="diagnostic-logs"></a>Tanılama Günlükleri
Tanılama günlüklerini işlemleri ve denetim yanı sıra sorun giderme amacıyla önemli hataları hakkında zengin bilgi sağlar. Tanılama günlükleri, etkinlik günlükleri farklılık gösterir. Etkinlik günlükleri Azure kaynaklarınızı üzerinde gerçekleştirilen işlemler fikir sağlar. Tanılama günlükleri işlemleri kaynağınız kendisini gerçekleştirilen bir anlayış sağlar.

API Management anda tanılama sağlar (saatlik toplu hale) günlükleri ayrı API'si hakkında aşağıdaki yapıya sahip her girişi iste:

```
{
    "Tenant": "",
      "DeploymentName": "",
      "time": "",
      "resourceId": "",
      "category": "GatewayLogs",
      "operationName": "Microsoft.ApiManagement/GatewayLogs",
      "durationMs": ,
      "Level": ,
      "properties": "{
          "ApiId": "",
          "OperationId": "",
          "ProductId": "",
          "SubscriptionId": "",
          "Method": "",
          "Url": "",
          "RequestSize": ,
          "ServiceTime": "",
          "BackendMethod": "",
          "BackendUrl": "",
          "BackendResponseCode": ,
          "ResponseCode": ,
          "ResponseSize": ,
          "Cache": "",
          "UserId"
      }"
 }
```

API Management hizmetiniz tanılama günlüklerine erişmek veya günlükleri, Azure kaynaklarınızın Azure İzleyicisi'nde erişim. API Management hizmetiniz tanılama günlükleri görüntülemek için:
1. Azure Portalı'nı açın.
2. API Management hizmetiniz gidin.
3. Tıklatın **tanılama günlük**.

![Tanılama günlükleri dikey penceresi][diagnostic-logs-blade]

Ölçümleri kullanma hakkında daha fazla bilgi için bkz: [tanılama günlükleri'ne genel bakış].

## <a name="next-step"></a>Sonraki adım

* [Azure İzleyicisi ile çalışmaya başlama]
* [genel bakış, ölçümleri]
* [etkinlik günlükleri genel bakış]
* [tanılama günlükleri'ne genel bakış]
* [genel bakış, uyarıları]

[Azure İzleyicisi ile çalışmaya başlama]: ../monitoring-and-diagnostics/monitoring-get-started.md
[genel bakış, ölçümleri]: ../monitoring-and-diagnostics/monitoring-overview-metrics.md
[etkinlik günlükleri genel bakış]: ../monitoring-and-diagnostics/monitoring-overview-activity-logs.md
[tanılama günlükleri'ne genel bakış]: ../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md
[genel bakış, uyarıları]: ../monitoring-and-diagnostics/insights-alerts-portal.md



[metrics-blade]: ./media/api-management-azure-monitor/api-management-metrics-blade.png
[activity-logs-blade]: ./media/api-management-azure-monitor/api-management-activity-logs-blade.png
[alert-rules-blade]: ./media/api-management-azure-monitor/api-management-alert-rules-blade.png
[diagnostic-logs-blade]: ./media/api-management-azure-monitor/api-management-diagnostic-logs-blade.png
