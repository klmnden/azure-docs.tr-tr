---
title: Azure kapsayıcı genel bakış izleme | Microsoft Docs
description: Bu makalede kümeleri durumunu ve kullanılabilirliğini hızla anlamak için Azure kapsayıcılarında izlemek için Azure'nın farklı yöntemlere genel bir bakış sağlar.
services: log-analytics
documentationcenter: ''
author: MGoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/26/2018
ms.author: magoedte
ms.openlocfilehash: 0d511c1f6dfd482e5754741da15b2852ee77c11e
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/04/2018
---
# <a name="overview-of-monitoring-containers-in-azure"></a>Azure kapsayıcı izleme genel bakış
Azure ile verimli izleyebilir ve Kubernetes veya Docker çalışan Azure kapsayıcılarında dağıtılan iş yüklerinizi yönetebilirsiniz. Mikro hizmet uygulamalarla birden çok kapsayıcı ölçekte güvenilir bir hizmet sunmak ve izleme planı desteklemek için nasıl çalıştığını anlamak önemlidir. Bu makalede yönetim kısa bir genel bakış sağlar ve izleme kapasiteleri yardımcı olmak için Azure her anlamak ve gereksinimlerinizi temel alarak uygun olduğu.

Kullanarak [Azure İzleyici kapsayıcı sağlık](monitoring-container-health.md), performans ve Linux kapsayıcı altyapınızın durumunu bir bakışta görüntüleyebilir ve sorunları hızla araştırın. Telemetri günlük analizi çalışma alanında depolanır ve burada keşfedebilirsiniz, Azure portalında tümleşik filtrelemek ve veri yükü, performans ve sistem durumu takip panolarla segment bir araya getirilir.  

Barındırılan Azure Kubernetes hizmet dışında günlük analizi çalıştıran kapsayıcıları için [Windows ve Docker kapsayıcısı çözümü](../log-analytics/log-analytics-containers.md) görüntülemenizi ve yönetmenizi, Windows ve Docker kapsayıcısı ana bilgisayar yardımcı olur. Günlük analizi çalışma alanınız stok ayrıntıları, performans ve düğümler ve kapsayıcıları olaylarından ortamda görüntüleyebilirsiniz. Görüntüleme ve Docker veya Windows ana uzaktan erişim gerek kalmadan merkezi günlükleri arama yaparak kapsayıcıları giderebilirsiniz ve kapsayıcılar ile kullanılan komutları gösteren ayrıntılı denetim bilgilerini görüntüleyebilir.

Bu, Azure veya şirket içi kaynak olup olmadığını bütünsel veya uçtan uca uygulamayı izleme elde etmek için herhangi bir bağımlılığı Azure İzleyici veya günlük analizi ile izlenmelidir.  Uygulama katmanı, Application Insights kullanılarak platform ve uygulama düzeyinde hem de sistem durumu tanıma ek katmanı eklemek için eklenmelidir. Platform düzeyinde uygulama Insights SDK'ları olduğundan [Kubernetes]( https://github.com/Microsoft/ApplicationInsights-Kubernetes), [Docker](https://hub.docker.com/r/microsoft/applicationinsights/), ve [Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-analysis-appinsights). Mikro hizmet uygulamaları için desteği yoktur [Java](../application-insights/app-insights-java-get-started.md), [Node.js](../application-insights/app-insights-nodejs-quick-start.md), [.Net](../application-insights/app-insights-asp-net.md), [.Net Core](../application-insights/app-insights-asp-net-core.md), yanı sıra diğer sayısı[diller/çerçeveler](../application-insights/app-insights-platforms.md). 

Aksi takdirde, sorunları tanımlanamayan gider, uygulamanın kullanılabilirliğini etkileyecek ve hizmet düzeyi hedeflerine karşılanmadı.  
