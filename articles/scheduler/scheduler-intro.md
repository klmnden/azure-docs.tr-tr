<properties
 pageTitle="Azure Scheduler nedir? | Microsoft Azure"
 description="Azure Scheduler, bulutta çalıştırmak üzere eylemleri bildirimli olarak tanımlamanızı sağlar. Sonra, bu eylemleri otomatik olarak zamanlar ve çalıştırır."
 services="scheduler"
 documentationCenter=".NET"
 authors="krisragh"
 manager="dwrede"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="hero-article"
 ms.date="03/09/2016"
 ms.author="krisragh"/>

# Azure Scheduler nedir?

Azure Scheduler, bulutta çalıştırmak üzere eylemleri bildirimli olarak tanımlamanızı sağlar. Sonra, bu eylemleri otomatik olarak zamanlar ve çalıştırır.  Scheduler bunu [Azure portal](scheduler-get-started-portal.md), kod, [REST API](https://msdn.microsoft.com/library/mt629143.aspx) ya da Azure PowerShell kullanarak yapar.

Scheduler zamanlanan iş oluşturur, korur ve çağırır.  Scheduler iş yükü barındırmaz ya da kod çalıştırmaz. Yalnızca başka bir yerde (Azure’da, şirket içi ya da başka sağlayıcıda) barındırılan kodu _çağırır_. HTTP, HTTPS, depolama kuyruğu, hizmet veri yolu kuyruğu ya da hizmet veri yolu konusu aracılığıyla çağırır.

Scheduler [işleri](scheduler-concepts-terms.md) zamanlar, birinin gözden geçirebileceği iş yürütme sonuçlarının geçmişini tutar ve yürütülecek iş yüklerini kesin ve güvenilir bir şekilde çalışması zamanlar. Azure WebJobs (Azure App Service’te Web Apps özelliğini bir parçası) ve diğer Azure zamanlama özellikleri arka planda Scheduler kullanır. [Scheduler REST API](https://msdn.microsoft.com/library/mt629143.aspx) bu eylemler için iletişimi yönetmeye yardımcı olur. Bu şekilde, Scheduler kolayca [karmaşık zamanlamalar ve gelişmiş yineleme oluşturmayı](scheduler-advanced-complexity.md) destekler.

Kendilerini Scheduler kullanımına ödünç veren çeşitli senaryolar vardır. Örneğin:

+ _Yinelenen uygulama eylemleri:_ Twitter’dan akışa düzenli aralıklarla veri toplama.
+ _Günlük bakım:_ Günlüklerin günlük ayıklanması, yedeklemelerin ve diğer bakım görevlerinin gerçekleştirilmesi. Örneğin, bir yönetici, sonraki dokuz ay boyunca her gün saat 01: 00'da veritabanını yedeklemeyi seçebilir.

Scheduler, programlı olarak, betikleri kullanarak ve portalda işler ve [iş koleksiyonları](scheduler-concepts-terms.md) oluşturmanızı, silmenizi, görüntülemenizi ve yönetmenizi sağlar.

## Ayrıca bkz.

 [Azure Scheduler kavramları, terminolojisi ve varlık hiyerarşisi](scheduler-concepts-terms.md)

 [Azure portalında Scheduler’ı kullanmaya başlayın](scheduler-get-started-portal.md)

 [Azure Scheduler’da planlar ve faturalama](scheduler-plans-billing.md)

 [Azure Scheduler ile karmaşık zamanlamalar ve gelişmiş yineleme oluşturma](scheduler-advanced-complexity.md)

 [Azure Scheduler REST API başvurusu](https://msdn.microsoft.com/library/mt629143)

 [Azure Scheduler PowerShell cmdlet’leri başvurusu](scheduler-powershell-reference.md)

 [Yüksek Azure Scheduler kullanılabilirliği ve güvenilirliği](scheduler-high-availability-reliability.md)

 [Azure Scheduler sınırları, varsayılanları ve hata kodları](scheduler-limits-defaults-errors.md)

 [Azure Scheduler giden bağlantı kimlik doğrulaması](scheduler-outbound-authentication.md)



<!--HONumber=Jun16_HO2-->


