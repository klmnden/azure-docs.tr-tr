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
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/24/2018
ms.author: magoedte
ms.openlocfilehash: d137576b4beb5cf36dce99ffb1869049f37b60b2
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60494652"
---
# <a name="overview-of-monitoring-containers-in-azure"></a>Kapsayıcıları azure'da izlemeye genel bakış
Azure ile etkili bir şekilde izleyebilir ve Kubernetes veya Docker'ı çalıştıran Azure kapsayıcıları üzerinde dağıtılan iş yüklerinizi yönetin. Uygun ölçekte güvenilir bir hizmet sunun ve izleme planınızı desteklemek için birden fazla mikro hizmet uygulamaları ile kapsayıcıları nasıl gerçekleştiriyorsunuz anlamak önemlidir. Bu makalede, yönetim ve izleme bunları anlamanıza yardımcı olması için Azure işlevlerini kısa bir genel bakış sağlar ve gereksinimlerinize göre uygun olan.

Kullanarak [kapsayıcılar için Azure İzleyici](container-insights-overview.md), performansı ve durumu Linux kapsayıcı altyapınızın bir bakışta görmek ve sorunları hızlı bir şekilde araştırın. Telemetri bir Log Analytics çalışma alanında depolanır ve burada keşfedebilirsiniz, Azure portalında tümleşik filtrelemek ve segment toplanmış veriler yük, performans ve sistem durumu takip panolarla.  

Barındırılan Azure Kubernetes hizmeti dışında Log Analytics çalışan kapsayıcılar için [Windows ve Docker kapsayıcı çözümü](../../azure-monitor/insights/containers.md) görüntüleyip yönettiğiniz Windows ve Docker kapsayıcı konaklarınız yardımcı olur. Log Analytics çalışma alanınızda Envanter ayrıntılarını, performans ve düğümlerin ve kapsayıcıların olaylardan ortamına görüntüleyebilirsiniz. Kapsayıcılar ile kullanılan komutları gösteren ayrıntılı denetim bilgileri görüntüleyebilir ve kapsayıcıları, Docker veya Windows konak uzaktan erişmek zorunda kalmadan Merkezi günlük arama ve görüntüleme giderebilirsiniz.

Bir Azure veya şirket içinde kaynak olup olmadığını bütünsel ya da için uçtan uca uygulamayı izleme elde etmek için herhangi bir bağımlılık Azure İzleyici ya da Log Analytics ile izlenmelidir.  Sistem durumu tanıma, hem Application Insights'ı kullanarak platform ve uygulama düzeyinde ek bir katmanı eklemek için uygulama katmanı eklenmelidir. Platform düzeyinde için Application Insights SDK'ları vardır [Kubernetes]( https://github.com/Microsoft/ApplicationInsights-Kubernetes), [Docker](https://hub.docker.com/r/microsoft/applicationinsights/), ve [Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-analysis-appinsights). Mikro hizmet uygulamaları için desteği yoktur [Java](../../azure-monitor/app/java-get-started.md), [Node.js](../../azure-monitor/learn/nodejs-quick-start.md), [.NET](../../azure-monitor/app/asp-net.md), [.NET Core](../../azure-monitor/app/asp-net-core.md), diğer sayısıyanısıra[diller/çerçeveler](../../azure-monitor/app/platforms.md). 

Aksi takdirde, sorunları tanımlanmamış geçer uygulamanın kullanılabilirliğini etkileyebilir ve hizmet düzeyi hedefleri karşılanmadı.  
