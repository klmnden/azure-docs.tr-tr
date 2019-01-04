---
title: Azure Application Insights'tan daha fazla alın | Microsoft Docs
description: Application Insights ile Başlarken sonra keşfedebilirsiniz özelliklerin özeti aşağıda verilmiştir.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 7ec10a2d-c669-448d-8d45-b486ee32c8db
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 02/03/2017
ms.author: mbullwin
ms.openlocfilehash: 7580089782eb9330d5b533588265d156213f397f
ms.sourcegitcommit: da69285e86d23c471838b5242d4bdca512e73853
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2019
ms.locfileid: "54000589"
---
# <a name="more-telemetry-from-application-insights"></a>Application ınsights'tan daha fazla telemetri
Sonra [ASP.NET kodunuza Application Insights eklenmiş](../../azure-monitor/app/asp-net.md), daha fazla telemetri almak için yapabileceğiniz birkaç şey vardır. 

| Eylem | Ne alacaksınız|
|---|---|
|(IIS sunucuları) [Durum İzleyicisi yükleyin](https://go.microsoft.com/fwlink/?LinkId=506648) her sunucu makinesinde.<br/>(Azure web uygulamaları) Web uygulaması için Azure denetim masasında Application Insights dikey penceresini açın.| [**Performans sayaçları**](../../azure-monitor/app/performance-counters.md)<br/>[**Özel durumlar** ](asp-net-exceptions.md) - ayrıntılı Yığın izlemeleri<br/>[**Bağımlılıkları**](../../azure-monitor/app/asp-net-dependencies.md)|
|[JavaScript kod parçacığını web sayfalarınıza ekleme](../../azure-monitor/app/javascript.md)|[Sayfa performansı](../../application-insights/app-insights-usage-overview.md), tarayıcı özel durumları, AJAX performans. Özel istemci tarafı telemetri.|
|[Kullanılabilirlik web testleri oluşturun](../../azure-monitor/app/monitor-web-app-availability.md)|Sitenizi kullanılamaz hale gelirse uyarılar alın|
|[Buildınfo.config olun](https://msdn.microsoft.com/library/dn449058.aspx) MSBuild tarafından oluşturulur|[Ek açıklamalar ölçüm grafikleri oluşturma](https://blogs.msdn.microsoft.com/visualstudioalm/2013/11/14/implementing-deployment-markers-in-application-insights/)
|[Yazma özel olaylar ve ölçümler](../../azure-monitor/app/api-custom-events-metrics.md)|İş olayları ve ölçümleri sayısı, ayrıntılı kullanım ve daha fazlasını izleyin.|
|[Sitenizin Canlı profil](https://aka.ms/AIProfilerPreview)|Canlı web uygulamanızdan ayrıntılı işlevi zamanlamaları|






