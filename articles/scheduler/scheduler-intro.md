---
title: Azure Scheduler nedir? | Microsoft Docs
description: Azure'un içinde veya dışındaki hizmetleri çağıran otomatik işler oluşturmayı, zamanlamayı ve çalıştırmayı öğrenin
services: scheduler
ms.service: scheduler
ms.suite: infrastructure-services
author: derek1ee
ms.author: deli
ms.reviewer: klam
ms.assetid: 52aa6ae1-4c3d-43fb-81b0-6792c84bcfae
ms.topic: conceptual
ms.date: 09/17/2018
ms.openlocfilehash: 21204e85adf1c68264ea448360c9e1120567ef3f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60530927"
---
# <a name="what-is-azure-scheduler"></a>Azure Scheduler nedir?

> [!IMPORTANT]
> Kullanımdan kaldırılan Azure Scheduler uygulamasının yerini [Azure Logic Apps](../logic-apps/logic-apps-overview.md) alacaktır. İş zamanlamak için [Azure Logic Apps'ı deneyebilirsiniz](../scheduler/migrate-from-scheduler-to-logic-apps.md). 

[Azure Scheduler](https://azure.microsoft.com/services/scheduler/), eylemleri açık bir şekilde bildirerek bulutta çalışan [işler](../scheduler/scheduler-concepts-terms.md) oluşturmanıza yardımcı olur. Hizmet bundan sonra bu eylemleri otomatik olarak zamanlar ve çalıştırır. Örneğin, Azure'un içinden ve dışında hizmetleri çağırabilir (HTTP veya HTTPS uç noktalarını çağırmak gibi), ayrıca Azure Depolama kuyruklarıyla Azure Service Bus kuyruklarına veya konularına iletiler gönderebilirsiniz. İşleri hemen çalıştırabileceğiniz gibi daha sonra da çalıştırabilirsiniz. Scheduler kolayca [karmaşık zamanlamalar ve gelişmiş yineleme oluşturmayı](../scheduler/scheduler-advanced-complexity.md) destekler. Scheduler işlerin ne zaman çalıştırılacağını belirtir, gözden geçirebilmeniz için iş sonuçlarının geçmişini tutar ve ardından iş yüklerini çalıştırmak için öngörülebilir ve güvenilir zamanlamalar yapar.

Scheduler'ı kullanarak zamanlanmış iş yüklerini oluşturabilir, koruyabilir ve çalıştırabilirsiniz ama Scheduler iş yüklerini barındırmaz veya kod çalıştırmaz. Hizmet yalnızca başka bir yerde (örneğin Azure’da, şirket içinde veya başka sağlayıcıda) barındırılan hizmetleri veya kodu *çağırır*. Scheduler HTTP, HTTPS, Depolama kuyruğu, Service Bus kuyruğu veya Service Bus konusu aracılığıyla çağırabilir. İşleri oluşturmak, yönetmek ve zamanlamak için [Azure portalını](../scheduler/scheduler-get-started-portal.md), kodu, [Scheduler REST API](https://docs.microsoft.com/rest/api/scheduler/)'yi veya [Azure Scheduler PowerShell cmdlet'leri başvurusunu](scheduler-powershell-reference.md) kullanabilirsiniz. Örneğin, Azure portalında ve betikleri kullanarak program aracılığıyla işleri ve [iş koleksiyonlarını](../scheduler/scheduler-concepts-terms.md) oluşturabilir, görüntüleyebilir, güncelleştirebilir, yönetebilir veya silebilirsiniz.

Diğer Azure zamanlama özellikleri, örneğin Azure App Service'te bir [Web Apps](https://azure.microsoft.com/services/app-service/web/) özelliği olan [Azure Web İşleri](../app-service/webjobs-create.md) de arka planda Scheduler'ı kullanır. [Scheduler REST API](https://docs.microsoft.com/rest/api/scheduler/)'yi kullanarak bu eylemler için iletişimi yönetebilirsiniz. bu eylemler için iletişimi yönetmeye yardımcı olur.

Burada Scheduler'ın size yardımcı olabileceği bazı senaryolar verilmiştir:

* **Yinelenen uygulama eylemleri çalıştırmak**: Örneğin, düzenli aralıklarla veri Twitter'dan akışa toplayın.

* **Günlük bakımının**: Günlük günlükleri temizleme, yedekleme ve diğer bakım görevleri gerçekleştirme gibi. 

  Örneğin, yönetici olarak önümüzdeki dokuz ay boyunca veritabanınızı her gün saat 1:00'da yedeklemek isteyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure portalında Scheduler’ı kullanmaya başlama](scheduler-get-started-portal.md)
* [Azure Scheduler için planlar ve faturalandırma](scheduler-plans-billing.md) hakkında bilgi edinin
* [Azure Scheduler ile karmaşık zamanlamalar ve gelişmiş yineleme oluşturmayı](scheduler-advanced-complexity.md) öğrenin