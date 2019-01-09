---
title: Azure Application Insights ile istekleri izlemek için kod yazma | Microsoft Docs
description: Profilleri için reqeusts alabilmeniz için Application Insights ile istekleri izlemek için kod yazma
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.reviewer: cawa
ms.date: 08/06/2018
ms.author: mbullwin
ms.openlocfilehash: 20f408d9dd32c3fd7a0e319e4051483e3aa54dd9
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54083168"
---
# <a name="write-code-to-track-requests-with-application-insights"></a>Application Insights ile istekleri izlemek için kod yazma

Profilleri uygulamanız için performans sayfada görmek için Application Insights uygulamanız için istekleri izleme gerekir. Application Insights istek önceden, ASP.net ve ASP.Net Core gibi işaretlenmiş çerçeveleri üzerinde oluşturulan uygulamalar için otomatik olarak izleyebilirsiniz. Ancak Azure bulut hizmeti çalışan rolleri ve Service Fabric durum bilgisi olmayan API'leri, Application Insights isteklerinizi nereden başlamalı ve bitmelidir bildirmek için kod yazma gereksiniminizi gibi diğer uygulamalar için. Bir kez istek telemetri, Application Insights'a gönderilir ve profilleri bu istekleri için toplanacak performans sayfada telemetri görürsünüz, bu kod yazdığınız. 

El ile istekleri izlemek için atmanız gereken adımlar şunlardır:


  1. Uygulama ömrü erken, aşağıdaki kodu ekleyin:  

        ```csharp
        using Microsoft.ApplicationInsights.Extensibility;
        ...
        // Replace with your own Application Insights instrumentation key.
        TelemetryConfiguration.Active.InstrumentationKey = "00000000-0000-0000-0000-000000000000";
        ```
      Bu genel bir izleme anahtarı yapılandırma hakkında daha fazla bilgi için bkz. [Application Insights ile kullanım Service Fabric](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/blob/dev/appinsights/ApplicationInsights.md).  

  1. İzleme, eklemek istediğiniz kodu herhangi bir parçası için bir `StartOperation<RequestTelemetry>` **kullanma** çevresinde, aşağıdaki örnekte gösterildiği gibi deyimi:

        ```csharp
        using Microsoft.ApplicationInsights;
        using Microsoft.ApplicationInsights.DataContracts;
        ...
        var client = new TelemetryClient();
        ...
        using (var operation = client.StartOperation<RequestTelemetry>("Insert_Your_Custom_Event_Unique_Name"))
        {
          // ... Code I want to profile.
        }
        ```

        Çağırma `StartOperation<RequestTelemetry>` içindeki başka `StartOperation<RequestTelemetry>` kapsamı desteklenmiyor. Kullanabileceğiniz `StartOperation<DependencyTelemetry>` iç içe kapsam yerine. Örneğin:  
        
        ```csharp
        using (var getDetailsOperation = client.StartOperation<RequestTelemetry>("GetProductDetails"))
        {
        try
        {
          ProductDetail details = new ProductDetail() { Id = productId };
          getDetailsOperation.Telemetry.Properties["ProductId"] = productId.ToString();
        
          // By using DependencyTelemetry, 'GetProductPrice' is correctly linked as part of the 'GetProductDetails' request.
          using (var getPriceOperation = client.StartOperation<DependencyTelemetry>("GetProductPrice"))
          {
              double price = await _priceDataBase.GetAsync(productId);
              if (IsTooCheap(price))
              {
                  throw new PriceTooLowException(productId);
              }
              details.Price = price;
          }
        
          // Similarly, note how 'GetProductReviews' doesn't establish another RequestTelemetry.
          using (var getReviewsOperation = client.StartOperation<DependencyTelemetry>("GetProductReviews"))
          {
              details.Reviews = await _reviewDataBase.GetAsync(productId);
          }
        
          getDetailsOperation.Telemetry.Success = true;
          return details;
        }
        catch(Exception ex)
        {
          getDetailsOperation.Telemetry.Success = false;
        
          // This exception gets linked to the 'GetProductDetails' request telemetry.
          client.TrackException(ex);
          throw;
        }
        }
        ```