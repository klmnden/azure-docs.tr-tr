---
title: Azure Scheduler nedir? | Microsoft Belgeleri
description: "Azure Scheduler, bulutta çalıştırmak üzere eylemleri bildirimli olarak tanımlamanızı sağlar. Sonra, bu eylemleri otomatik olarak zamanlar ve çalıştırır."
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: 52aa6ae1-4c3d-43fb-81b0-6792c84bcfae
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/18/2016
ms.author: deli
ms.translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: a3bf1aacd6978499d7ef77cbcb451a06b857ac38
ms.contentlocale: tr-tr
ms.lasthandoff: 12/07/2016

---
# <a name="what-is-azure-scheduler"></a>Azure Scheduler nedir?
Azure Scheduler, bulutta çalıştırmak üzere eylemleri bildirimli olarak tanımlamanızı sağlar. Sonra, bu eylemleri otomatik olarak zamanlar ve çalıştırır.  Scheduler bunu [Azure portal](scheduler-get-started-portal.md), kod, [REST API](https://msdn.microsoft.com/library/mt629143.aspx) ya da Azure PowerShell kullanarak yapar.

Scheduler zamanlanan iş oluşturur, korur ve çağırır.  Scheduler iş yükü barındırmaz ya da kod çalıştırmaz. Yalnızca başka bir yerde (Azure’da, şirket içi ya da başka sağlayıcıda) barındırılan kodu *çağırır*. HTTP, HTTPS, depolama kuyruğu, hizmet veri yolu kuyruğu ya da hizmet veri yolu konusu aracılığıyla çağırır.

Scheduler [işleri](scheduler-concepts-terms.md) zamanlar, birinin gözden geçirebileceği iş yürütme sonuçlarının geçmişini tutar ve yürütülecek iş yüklerini kesin ve güvenilir bir şekilde çalışması zamanlar. Azure WebJobs (Azure App Service’te Web Apps özelliğini bir parçası) ve diğer Azure zamanlama özellikleri arka planda Scheduler kullanır. [Scheduler REST API](https://msdn.microsoft.com/library/mt629143.aspx) bu eylemler için iletişimi yönetmeye yardımcı olur. Bu şekilde, Scheduler kolayca [karmaşık zamanlamalar ve gelişmiş yineleme oluşturmayı](scheduler-advanced-complexity.md) destekler.

Kendilerini Scheduler kullanımına ödünç veren çeşitli senaryolar vardır. Örneğin:

* *Yinelenen uygulama eylemleri:* Twitter’dan akışa düzenli aralıklarla veri toplama.
* *Günlük bakım:* Günlüklerin günlük ayıklanması, yedeklemelerin ve diğer bakım görevlerinin gerçekleştirilmesi. Örneğin, bir yönetici, sonraki dokuz ay boyunca her gün saat 01: 00'da veritabanını yedeklemeyi seçebilir.

Scheduler, programlı olarak, betikleri kullanarak ve portalda işler ve [iş koleksiyonları](scheduler-concepts-terms.md) oluşturmanızı, silmenizi, görüntülemenizi ve yönetmenizi sağlar.

## <a name="see-also"></a>Ayrıca bkz.
 [Azure Scheduler kavramları, terminolojisi ve varlık hiyerarşisi](scheduler-concepts-terms.md)

 [Azure portalında Scheduler’ı kullanmaya başlama](scheduler-get-started-portal.md)

 [Azure Scheduler’da planlar ve faturalama](scheduler-plans-billing.md)

 [Azure Scheduler ile karmaşık zamanlamalar ve gelişmiş yineleme oluşturma](scheduler-advanced-complexity.md)

 [Azure Scheduler REST API başvurusu](https://msdn.microsoft.com/library/mt629143)

 [Azure Scheduler PowerShell cmdlet’leri başvurusu](scheduler-powershell-reference.md)

 [Yüksek Azure Scheduler kullanılabilirliği ve güvenilirliği](scheduler-high-availability-reliability.md)

 [Azure Scheduler sınırları, varsayılanları ve hata kodları](scheduler-limits-defaults-errors.md)

 [Azure Scheduler giden bağlantı kimlik doğrulaması](scheduler-outbound-authentication.md)


