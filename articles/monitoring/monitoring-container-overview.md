---
title: Azure kapsayıcıları izlemeye genel bakış | Microsoft Docs
description: Bu makalede, azure'da bir küme durumunu ve kullanılabilirliğini hızlı bir şekilde anlamak için azure'da kapsayıcılar izlemek için kullanılabilen farklı yöntemleri için genel bir bakış sağlar.
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
ms.date: 09/24/2018
ms.author: magoedte
ms.openlocfilehash: db85f85011154dcc7adfa9d569e9015a9c5c33ca
ms.sourcegitcommit: 715813af8cde40407bd3332dd922a918de46a91a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47055076"
---
# <a name="overview-of-monitoring-containers-in-azure"></a>Kapsayıcıları azure'da izlemeye genel bakış
Azure ile etkili bir şekilde izleyebilir ve Kubernetes veya Docker'ı çalıştıran Azure kapsayıcıları üzerinde dağıtılan iş yüklerinizi yönetin. Uygun ölçekte güvenilir bir hizmet sunun ve izleme planınızı desteklemek için birden fazla mikro hizmet uygulamaları ile kapsayıcıları nasıl gerçekleştiriyorsunuz anlamak önemlidir. Bu makalede, yönetim ve izleme bunları anlamanıza yardımcı olması için Azure işlevlerini kısa bir genel bakış sağlar ve gereksinimlerinize göre uygun olan.

Kullanarak [kapsayıcılar için Azure İzleyici](monitoring-container-insights-overview.md), performansı ve durumu Linux kapsayıcı altyapınızın bir bakışta görmek ve sorunları hızlı bir şekilde araştırın. Telemetri bir Log Analytics çalışma alanında depolanır ve burada keşfedebilirsiniz, Azure portalında tümleşik filtrelemek ve segment toplanmış veriler yük, performans ve sistem durumu takip panolarla.  

Barındırılan Azure Kubernetes hizmeti dışında Log Analytics çalışan kapsayıcılar için [Windows ve Docker kapsayıcı çözümü](../log-analytics/log-analytics-containers.md) görüntüleyip yönettiğiniz Windows ve Docker kapsayıcı konaklarınız yardımcı olur. Log Analytics çalışma alanınızda Envanter ayrıntılarını, performans ve düğümlerin ve kapsayıcıların olaylardan ortamına görüntüleyebilirsiniz. Kapsayıcılar ile kullanılan komutları gösteren ayrıntılı denetim bilgileri görüntüleyebilir ve kapsayıcıları, Docker veya Windows konak uzaktan erişmek zorunda kalmadan Merkezi günlük arama ve görüntüleme giderebilirsiniz.

Bir Azure veya şirket içinde kaynak olup olmadığını bütünsel ya da için uçtan uca uygulamayı izleme elde etmek için herhangi bir bağımlılık Azure İzleyici ya da Log Analytics ile izlenmelidir.  Sistem durumu tanıma, hem Application Insights'ı kullanarak platform ve uygulama düzeyinde ek bir katmanı eklemek için uygulama katmanı eklenmelidir. Platform düzeyinde için Application Insights SDK'ları vardır [Kubernetes]( https://github.com/Microsoft/ApplicationInsights-Kubernetes), [Docker](https://hub.docker.com/r/microsoft/applicationinsights/), ve [Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-analysis-appinsights). Mikro hizmet uygulamaları için desteği yoktur [Java](../application-insights/app-insights-java-get-started.md), [Node.js](../application-insights/app-insights-nodejs-quick-start.md), [.Net](../application-insights/app-insights-asp-net.md), [.Net Core](../application-insights/app-insights-asp-net-core.md), diğer sayısıyanısıra[diller/çerçeveler](../application-insights/app-insights-platforms.md). 

Aksi takdirde, sorunları tanımlanmamış geçer uygulamanın kullanılabilirliğini etkileyebilir ve hizmet düzeyi hedefleri karşılanmadı.  
