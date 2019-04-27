---
title: Azure Application Insights ile istekleri izlemek için kod yazma | Microsoft Docs
description: İsteklerinizi için profilleri alabilmeniz için Application Insights ile istekleri izlemek için kod yazın.
services: application-insights
documentationcenter: ''
author: cweining
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.reviewer: mbullwin
ms.date: 08/06/2018
ms.author: cweining
ms.openlocfilehash: 4782e560b580b7f565724dbb35ed9876bffdc256
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60730863"
---
# <a name="write-code-to-track-requests-with-application-insights"></a>Application Insights ile istekleri izlemek için kod yazma

Profilleri uygulamanız için performans sayfada görüntülemek için uygulamanız için istekleri izlemek Azure Application Insights gerekir. Application Insights isteği zaten izlenen çerçeveleri üzerinde oluşturulan uygulamalar için otomatik olarak izleyebilirsiniz. ASP.NET ve ASP.NET Core iki örnek verilmiştir. 

Azure Cloud Services çalışan rolleri ve Service Fabric durum bilgisi olmayan API'leri gibi diğer uygulamalar için Application ınsights'ı isteklerinizi burada başlar ve son bildirmek için kod yazmanız gerekir. Bu kodu yazdıktan sonra istekleri telemetriyi Application Insights'a gönderilir. Performans sayfada telemetriyi görüntüleyebilir ve profilleri için bu istekleri toplanır. 

El ile istekleri izlemek için aşağıdakileri yapın:

  1. Uygulama ömrü erken, aşağıdaki kodu ekleyin:  

        ```csharp
        using Microsoft.ApplicationInsights.Extensibility;
        ...
        // Replace with your own Application Insights instrumentation key.
        TelemetryConfiguration.Active.InstrumentationKey = "00000000-0000-0000-0000-000000000000";
        ```
      Bu genel bir izleme anahtarı yapılandırma hakkında daha fazla bilgi için bkz. [Application Insights ile kullanım Service Fabric](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/blob/dev/appinsights/ApplicationInsights.md).  

  1. İzleme, eklemek istediğiniz kodu herhangi bir parçası için bir `StartOperation<RequestTelemetry>` **kullanarak** çevresinde, aşağıdaki örnekte gösterildiği gibi deyimi:

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
