---
title: Görüntü dizini, Azure SaaS SQL uygulama | Microsoft Docs
description: Bu makalede çeşitli zaman noktası 11 Ekim 2017 tutulan Ignite konferansından SaaS DB kiralama uygulama tasarımı ile ilgili bir video bizim 81 dakikalar içinde bir dizin oluşturur. Önceden ilginizi çeken bölümüne atlayabilirsiniz. En az 3 desenler açıklanmaktadır. Geliştirme ve yönetim sürecini basitleştirmek azure özellikleri açıklanmaktadır.
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MightyPen
ms.author: genemi
ms.reviewer: billgib, sstein
manager: craigg
ms.date: 12/18/2018
ms.openlocfilehash: bbe220780a3c21e7bfb15d0568904af4ed47f765
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61487299"
---
# <a name="video-indexed-and-annotated-for-multi-tenant-saas-app-using-azure-sql-database"></a>Video dizini oluşturulan ve Azure SQL veritabanı kullanan çok kiracılı SaaS uygulaması için açıklama

Bu makalede, SaaS kiralama modelleri veya düzenleri hakkında video 81 bir dakikalık zaman konumlara ek açıklamalı bir dizindir. Bu makalede, geriye doğru atlamayı sağlar veya videonun hangi bölümünün için İleri ilginizi çeken. Videoda, Azure SQL veritabanı'nda bir çok kiracılı veritabanı uygulaması için önemli tasarım seçenekleri açıklanmaktadır. Video, tanıtımlar, yönetim kod Kılavuzlar ve yazılı belgelerimizde olabilir daha deneyimi hakkında bilgi sahibi olmak zamanlarda daha fazla ayrıntı içerir.

Video bulunan belgelerimize yazılı olarak bilgi büyüten: 
- *Kavramsal:* [Çok kiracılı SaaS veritabanı kiracılı desenleri][saas-concept-design-patterns-563e]
- *Öğretici:* [Wingtip bilet SaaS uygulaması][saas-how-welcome-wingtip-app-679t]

Video ve makale, Azure SQL veritabanı'nda bulut üzerinde çok kiracılı bir uygulama oluşturmanın birçok aşamaları açıklanmaktadır. Azure SQL veritabanı'nın özel özellikler geliştirmek ve çok kiracılı uygulamalar, her ikisi de daha kolay ve güvenilir bir şekilde yüksek performanslı uygulamak kolaylaştırır.

Düzenli olarak yazılmış Belgelerimizi güncelleştiriyoruz. Video düzenleme ya da güncelleştirilmiş, bu nedenle sonunda daha fazla ayrıntı, eski olabilir.



## <a name="sequence-of-38-time-indexed-screenshots"></a>38 zaman dizine ekran görüntüleri bir dizi

Bu bölümde 38 tartışmalar boyunca 81 dakikalık video saat konumunu dizinler. Her zaman dizin, bir ekran görüntüsü, video ve bazen ek bilgilerle açıklanıyor.

Her zaman dizindir biçiminde *hh*. Örneğin, ikinci etiketli saat konumunu, dizine **oturum hedefleri**, yaklaşık süreyi konumunda başlayan **0:03:11**.


### <a name="compact-links-to-video-indexed-time-locations"></a>Video dizinli zaman konumlara Compact bağlantıları

Aşağıdaki konu başlıklarını bağlantılar için bu makalenin devamındaki ek açıklamalı kendi ilgili bölümleri şunlardır:

- [1. **(Başlangıç)**  Hoş Geldiniz slayt, 0:00:03](#anchor-image-wtip-min00001)
- [2. Oturum hedefleri, 0:03:11](#anchor-image-wtip-min00311)
- [3. Gündem, 0:04:17](#anchor-image-wtip-min00417)
- [4. Çok kiracılı web uygulaması, 0:05:05](#anchor-image-wtip-min00505)
- [5. Uygulama web formu uygulamada, 0:05:55](#anchor-image-wtip-min00555)
- [6. Kiracı başına maliyet (ölçekli, yalıtım kurtarma), 0:09:31](#anchor-image-wtip-min00931)
- [7. Modelleri için çok kiracılı veritabanı: Artıları ve eksileri, 0:11:59](#anchor-image-wtip-min01159)
- [8. Karma modeli karıştırır MT/ST, 0'ın avantajları: 13:01](#anchor-image-wtip-min01301)
- [9. Tek kiracılı vs çok kiracılı: Artıları ve eksileri, 0:16:44](#anchor-image-wtip-min01644)
- [10. Havuzları öngörülemeyen iş yükleri için 0 uygun maliyetli: 19:36](#anchor-image-wtip-min01936)
- [11. Kiracı başına veritabanı karma ST/MT, 0 ve Demo: 20:08](#anchor-image-wtip-min02008)
- [12. Canlı Dojo, 0'ı gösteren uygulama formu: 20:29](#anchor-image-wtip-min02029)
- [13. MYOB ve DBA görme, 0 içinde değil: 28:54](#anchor-image-wtip-min02854)
- [14. MYOB elastik Havuz Kullanım örneği, 0:29:40](#anchor-image-wtip-min02940)
- [15. MYOB ve diğer ISV'ler, 0'ı Öğrenme: 31:36](#anchor-image-wtip-min03136)
- [16. Desenler compose E2E SaaS senaryoya, 0:43:15](#anchor-image-wtip-min04315)
- [17. Kurallı karma çok kiracılı SaaS uygulaması, 0:47:33](#anchor-image-wtip-min04733)
- [18. Wingtip SaaS örnek uygulaması, 0:48:10](#anchor-image-wtip-min04810)
- [19. Senaryoları ve düzenleri öğreticiler, 0 incelediniz: 49:10](#anchor-image-wtip-min04910)
- [20. Öğreticiler ve GitHub deposu, 0'ın Tanıtımı: 50:18](#anchor-image-wtip-min05018)
- [21. GitHub deposunu Microsoft/WingtipSaaS, 0:50:38](#anchor-image-wtip-min05038)
- [22. Desenler, 0'ı keşfetme: 56:20](#anchor-image-wtip-min05620)
- [23. Kiracılar ve ekleme, 0 sağlama: 57:44](#anchor-image-wtip-min05744)
- [24. Kiracılar ve uygulama bağlantısı, 0 sağlama: 58:58](#anchor-image-wtip-min05858)
- [25. Yönetim tanıtım betikleri 0 tek bir kiracı sağlama: 59:43](#anchor-image-wtip-min05943)
- [26. PowerShell için sağlama ve kataloğa kaydetme, 1:00:02](#anchor-image-wtip-min10002)
- [27. T-SQL SELECT * TenantsExtended, 1:03: 30'dan](#anchor-image-wtip-min10330)
- [28. 1:04:36 öngörülemez Kiracı iş yüklerini yönetme](#anchor-image-wtip-min10436)
- [29. Elastik Havuz izleme, 1:06:39](#anchor-image-wtip-min10639)
- [30. Yük oluşturma ve performans izleme, 1:09:42](#anchor-image-wtip-min10942)
- [31. Uygun ölçekte, 1:10:33 Şema Yönetimi](#anchor-image-wtip-min11033)
- [32. 1:12:21 Kiracı veritabanlarında dağıtılmış sorgu](#anchor-image-wtip-min11221)
- [33. Bilet oluşturma, 1:12:32 Tanıtımı](#anchor-image-wtip-min11232)
- [34. SSMS geçici analiz, 1:12:46](#anchor-image-wtip-min11246)
- [35. SQL DW'ye, 1:16:32 Kiracı verilerini ayıklama](#anchor-image-wtip-min11632)
- [36. Grafik günlük satış dağıtım, 1:16:48](#anchor-image-wtip-min11648)
- [37. Kaydır ve eylem, 1:19:52 çağırın](#anchor-image-wtip-min11952)
- [38. 1:20:42 hakkında daha fazla bilgi için kaynaklar](#anchor-image-wtip-min12042)


&nbsp;

### <a name="annotated-index-time-locations-in-the-video"></a>Videoda açıklamalı dizin zaman konumları

Herhangi bir ekran görüntüsünü tıklamak videoyu kesin zaman konumuna götürür.


&nbsp;<a name="anchor-image-wtip-min00001"/>
#### <a name="1-start-welcome-slide-00001"></a>1. *(Başlangıç)*  Hoş Geldiniz slayt, 0:00:01

*MYOB öğrenme: Azure SQL veritabanı - BRK3120 SaaS uygulamaları için Tasarım desenleri*

[![Hoş Geldiniz slayt][image-wtip-min00003-brk3120-whole-welcome]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=1)

- Başlık: MYOB öğrenme: Azure SQL veritabanı SaaS uygulamaları için Tasarım desenleri
- Bill.Gibson@microsoft.com
- Baş Program Yöneticisi, Azure SQL veritabanı
- Microsoft Ignite oturumu BRK3120, Orlando, FL ABD, 11 Ekim 2017


&nbsp;<a name="anchor-image-wtip-min00311"/>
#### <a name="2-session-objectives-00153"></a>2. Oturum hedefleri, 0:01:53
[![Oturum hedefleri][image-wtip-min00311-session]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=113)

- Alternatif modeller Artıları ve eksileri ile çok kiracılı uygulamalar için.
- Geliştirme, yönetim ve kaynak maliyetleri azaltmak için SaaS düzenlerinin.
- Örnek bir uygulama + betikler.
- PaaS özellikler + SaaS düzenlerinin SQL veritabanı yüksek düzeyde ölçeklenebilir ve ekonomik veri platformu çok kiracılı SaaS olun.


&nbsp;<a name="anchor-image-wtip-min00417"/>
#### <a name="3-agenda-00409"></a>3. Gündem, 0:04:09
[![Gündem][image-wtip-min00417-agenda]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=249)


&nbsp;<a name="anchor-image-wtip-min00505"/>
#### <a name="4-multi-tenant-web-app-00500"></a>4. Çok kiracılı web uygulaması, 0:05:00
[![Wingtip SaaS uygulaması: Çok kiracılı web uygulaması][image-wtip-min00505-web-app]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=300)


&nbsp;<a name="anchor-image-wtip-min00555"/>
#### <a name="5-app-web-form-in-action-00539"></a>5. Uygulama web formu uygulamada, 0:05:39
[![Uygulamada uygulama web formu][image-wtip-min00555-app-web-form]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=339)


&nbsp;<a name="anchor-image-wtip-min00931"/>
#### <a name="6-per-tenant-cost-scale-isolation-recovery-00658"></a>6. Kiracı başına maliyet (ölçekli, yalıtım kurtarma), 0:06:58
[![Kiracı başına maliyeti, Ölçek, yalıtım, Kurtarma][image-wtip-min00931-per-tenant-cost]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=418)


&nbsp;<a name="anchor-image-wtip-min01159"/>
#### <a name="7-database-models-for-multi-tenant-pros-and-cons-00952"></a>7. Modelleri için çok kiracılı veritabanı: Artıları ve eksileri, 0:09:52
[![Modelleri için çok kiracılı veritabanı: avantajları ve dezavantajları][image-wtip-min01159-db-models-pros-cons]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=592)


&nbsp;<a name="anchor-image-wtip-min01301"/>
#### <a name="8-hybrid-model-blends-benefits-of-mtst-01229"></a>8. Karma modeli karıştırır MT/ST, 0'ın avantajları: 12:29
[![MT/ST avantajlarını karma modeli karıştırır][image-wtip-min01301-hybrid]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=749)


&nbsp;<a name="anchor-image-wtip-min01644"/>
#### <a name="9-single-tenant-vs-multi-tenant-pros-and-cons-01311"></a>9. Tek kiracılı vs çok kiracılı: Artıları ve eksileri, 0:13:11
[![Tek kiracılı vs çok kiracılı: avantajları ve dezavantajları][image-wtip-min01644-st-vs-mt]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=791)


&nbsp;<a name="anchor-image-wtip-min01936"/>
#### <a name="10-pools-are-cost-effective-for-unpredictable-workloads-01749"></a>10. Havuzları öngörülemeyen iş yükleri için 0 uygun maliyetli: 17:49
[![Havuzları öngörülemeyen iş yükleri için düşük maliyetli][image-wtip-min01936-pools-cost]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=1069)


&nbsp;<a name="anchor-image-wtip-min02008"/>
#### <a name="11-demo-of-database-per-tenant-and-hybrid-stmt-01959"></a>11. Kiracı başına veritabanı karma ST/MT, 0 ve Demo: 19:59
[![Kiracı başına veritabanı ve karma ST/MT Tanıtımı][image-wtip-min02008-demo-st-hybrid]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=1199)


&nbsp;<a name="anchor-image-wtip-min02029"/>
#### <a name="12-live-app-form-showing-dojo-02010"></a>12. Canlı Dojo, 0'ı gösteren uygulama formu: 20:10
[![Dojo gösteren Canlı uygulama formu][image-wtip-min02029-live-app-form-dojo]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=1210)

&nbsp;<a name="anchor-image-wtip-min02854"/>
#### <a name="13-myob-and-not-a-dba-in-sight-02506"></a>13. MYOB ve DBA görme, 0 içinde değil: 25:06
[![MYOB ve DBA görüş içinde değil][image-wtip-min02854-myob-no-dba]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=1506)


&nbsp;<a name="anchor-image-wtip-min02940"/>
#### <a name="14-myob-elastic-pool-usage-example-02930"></a>14. MYOB elastik Havuz Kullanım örneği, 0:29:30
[![MYOB elastik Havuz Kullanım örneği][image-wtip-min02940-myob-elastic]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=1770)


&nbsp;<a name="anchor-image-wtip-min03136"/>
#### <a name="15-learning-from-myob-and-other-isvs-03125"></a>15. MYOB ve diğer ISV'ler, 0'ı Öğrenme: 31:25
[![MYOB ve diğer ISV'ler öğrenme][image-wtip-min03136-learning-isvs]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=1885)


&nbsp;<a name="anchor-image-wtip-min04315"/>
#### <a name="16-patterns-compose-into-e2e-saas-scenario-03142"></a>16. Desenler compose E2E SaaS senaryoya, 0:31:42
[![E2E SaaS senaryoya desenleri oluşturma][image-wtip-min04315-patterns-compose]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=1902)


&nbsp;<a name="anchor-image-wtip-min04733"/>
#### <a name="17-canonical-hybrid-multi-tenant-saas-app-04604"></a>17. Kurallı karma çok kiracılı SaaS uygulaması, 0:46:04
[![Kurallı karma çok kiracılı SaaS uygulaması][image-wtip-min04733-canonical-hybrid]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=2764)


&nbsp;<a name="anchor-image-wtip-min04810"/>
#### <a name="18-wingtip-saas-sample-app-04801"></a>18. Wingtip SaaS örnek uygulaması, 0:48:01
[![Wingtip SaaS örnek uygulaması][image-wtip-min04810-wingtip-saas-app]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=2881)


&nbsp;<a name="anchor-image-wtip-min04910"/>
#### <a name="19-scenarios-and-patterns-explored-in-the-tutorials-04900"></a>19. Senaryoları ve düzenleri öğreticiler, 0 incelediniz: 49:00
[![Senaryolar ve öğreticilerde incelediniz desenleri][image-wtip-min04910-scenarios-tutorials]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=2940)


&nbsp;<a name="anchor-image-wtip-min05018"/>
#### <a name="20-demo-of-tutorials-and-github-repository-05012"></a>20. Öğreticiler ve GitHub deposu, 0'ın Tanıtımı: 50:12
[![Tanıtım öğreticilerine ve GitHub deposu][image-wtip-min05018-demo-tutorials-github]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=3012)


&nbsp;<a name="anchor-image-wtip-min05038"/>
#### <a name="21-github-repo-microsoftwingtipsaas-05032"></a>21. GitHub deposunu Microsoft/WingtipSaaS, 0:50:32
[![GitHub deposunu Microsoft/WingtipSaaS][image-wtip-min05038-github-wingtipsaas]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=3032)


&nbsp;<a name="anchor-image-wtip-min05620"/>
#### <a name="22-exploring-the-patterns-05615"></a>22. Desenler, 0'ı keşfetme: 56:15
[![Desenler keşfetme][image-wtip-min05620-exploring-patterns]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=3375)


&nbsp;<a name="anchor-image-wtip-min05744"/>
#### <a name="23-provisioning-tenants-and-onboarding-05619"></a>23. Kiracılar ve ekleme, 0 sağlama: 56:19
[![Kiracılar sağlama ve ekleme][image-wtip-min05744-provisioning-tenants-onboarding-1]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=3379)


&nbsp;<a name="anchor-image-wtip-min05858"/>
#### <a name="24-provisioning-tenants-and-application-connection-05752"></a>24. Kiracılar ve uygulama bağlantısı, 0 sağlama: 57:52
[![Kiracılar ve uygulama bağlantısı sağlama][image-wtip-min05858-provisioning-tenants-app-connection-2]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=3472)


&nbsp;<a name="anchor-image-wtip-min05943"/>
#### <a name="25-demo-of-management-scripts-provisioning-a-single-tenant-05936"></a>25. Yönetim tanıtım betikleri 0 tek bir kiracı sağlama: 59:36
[![Tek bir kiracı sağlama yönetim komut dosyaları Tanıtımı][image-wtip-min05943-demo-management-scripts-st]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=3576)


&nbsp;<a name="anchor-image-wtip-min10002"/>
#### <a name="26-powershell-to-provision-and-catalog-05956"></a>26. PowerShell için sağlama ve kataloğa kaydetme, 0:59:56
[![PowerShell için sağlama ve kataloğa kaydetme][image-wtip-min10002-powershell-provision-catalog]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=3596)


&nbsp;<a name="anchor-image-wtip-min10330"/>
#### <a name="27-t-sql-select--from-tenantsextended-10325"></a>27. T-SQL SELECT * ÖĞESİNDEN TenantsExtended, 1:03:25
[![T-SQL SELECT * TenantsExtended gelen][image-wtip-min10330-sql-select-tenantsextended]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=3805)


&nbsp;<a name="anchor-image-wtip-min10436"/>
#### <a name="28-managing-unpredictable-tenant-workloads-10334"></a>28. 1:03:34 öngörülemez Kiracı iş yüklerini yönetme
[![Öngörülemez Kiracı iş yüklerini yönetme][image-wtip-min10436-managing-unpredictable-workloads]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=3814)


&nbsp;<a name="anchor-image-wtip-min10639"/>
#### <a name="29-elastic-pool-monitoring-10632"></a>29. Elastik Havuz izleme, 1:06:32
[![Esnek Havuz izleme][image-wtip-min10639-elastic-pool-monitoring]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=3992)


&nbsp;<a name="anchor-image-wtip-min10942"/>
#### <a name="30-load-generation-and-performance-monitoring-10937"></a>30. Yük oluşturma ve performans izleme, 1:09:37
[![Yük oluşturma ve performans izleme][image-wtip-min10942-load-generation]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=4117)


&nbsp;<a name="anchor-image-wtip-min11033"/>
#### <a name="31-schema-management-at-scale-10940"></a>31. Şema Yönetimi uygun ölçekte, 1:09:40
[![Uygun ölçekte Şema Yönetimi][image-wtip-min11033-schema-management-scale]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=34120)


&nbsp;<a name="anchor-image-wtip-min11221"/>
#### <a name="32-distributed-query-across-tenant-databases-11118"></a>32. 1:11:18 Kiracı veritabanlarında dağıtılmış sorgu
[![Kiracı veritabanlarında dağıtılmış sorgu][image-wtip-min11221-distributed-query]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=4278)


&nbsp;<a name="anchor-image-wtip-min11232"/>
#### <a name="33-demo-of-ticket-generation-11228"></a>33. Bilet oluşturma, 1:12:28 Tanıtımı
[![Tanıtım bilet oluşturma][image-wtip-min11232-demo-ticket-generation]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=4348)


&nbsp;<a name="anchor-image-wtip-min11246"/>
#### <a name="34-ssms-adhoc-analytics-11235"></a>34. SSMS geçici analiz, 1:12:35
[![SSMS geçici analiz][image-wtip-min11246-ssms-adhoc-analytics]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=4355)


&nbsp;<a name="anchor-image-wtip-min11632"/>
#### <a name="35-extract-tenant-data-into-sql-dw-11546"></a>35. SQL DW'ye, 1:15:46 Kiracı verilerini ayıklama
[![SQL DW'ye Kiracı verilerini ayıklama][image-wtip-min11632-extract-tenant-data-sql-dw]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=4546)


&nbsp;<a name="anchor-image-wtip-min11648"/>
#### <a name="36-graph-of-daily-sale-distribution-11638"></a>36. Grafik günlük satış dağıtım, 1:16:38
[![Günlük satış dağılım grafiği][image-wtip-min11648-graph-daily-sale-distribution]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=4598)


&nbsp;<a name="anchor-image-wtip-min11952"/>
#### <a name="37-wrap-up-and-call-to-action-11743"></a>37. Kaydır ve arama eylemi, 1:17:43
[![Kaydır ve eylem çağrısı][image-wtip-min11952-wrap-up-call-action]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=4663)


&nbsp;<a name="anchor-image-wtip-min12042"/>
#### <a name="38-resources-for-more-information-12035"></a>38. 1:20:35 daha fazla bilgi için kaynaklar
[![Daha fazla bilgi için kaynaklar][image-wtip-min12042-resources-more-info]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=4835)

- [Blog gönderisi, 22 Mayıs 2017][resource-blog-saas-patterns-app-dev-sql-db-768h]

- *Kavramsal:* [Çok kiracılı SaaS veritabanı kiracılı desenleri][saas-concept-design-patterns-563e]

- *Öğretici:* [Wingtip bilet SaaS uygulaması][saas-how-welcome-wingtip-app-679t]

- GitHub depoları için Wingtip bilet SaaS kiralama uygulama türü:
    - [-Tek başına uygulama modeli için GitHub deposunu][github-wingtip-standaloneapp].
    - [-DB başına Kiracı modeli için GitHub deposunu][github-wingtip-dbpertenant].
    - [-Çok Kiracılı DB modeli için GitHub deposunu][github-wingtip-multitenantdb].





## <a name="next-steps"></a>Sonraki adımlar

- [İlk öğretici makalesi][saas-how-welcome-wingtip-app-679t]




<!-- Image link reference IDs. -->

[image-wtip-min00003-brk3120-whole-welcome]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min00003-brk3120-welcome-myob-design-saas-app-sql-db.png "Hoş Geldiniz slayt"

[image-wtip-min00311-session]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min00311-session-objectives-takeaway.png "Oturum hedefler."

[image-wtip-min00417-agenda]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min00417-agenda-app-management-models-patterns.png "Gündem."

[image-wtip-min00505-web-app]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min00505-wingtip-saas-app-mt-web.png "Wingtip SaaS uygulaması: Çok kiracılı web uygulaması"

[image-wtip-min00555-app-web-form]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min00555-app-form-contoso-concert-hall-night-opera.png "Uygulamada uygulama web formu"

[image-wtip-min00931-per-tenant-cost]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min00931-saas-data-management-concerns.png "Kiracı başına maliyeti, Ölçek, yalıtım, Kurtarma"

[image-wtip-min01159-db-models-pros-cons]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min01159-db-models-multi-tenant-saas-apps.png "Modelleri için çok kiracılı veritabanı: avantajları ve dezavantajları"

[image-wtip-min01301-hybrid]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min01301-hybrib-model-blends-benefits-mt-st.png "MT/ST avantajlarını karma modeli karıştırır"

[image-wtip-min01644-st-vs-mt]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min01644-st-mt-pros-cons.png "Tek kiracılı vs çok kiracılı: avantajları ve dezavantajları"

[image-wtip-min01936-pools-cost]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min01936-pools-cost-effective-unpredictable-workloads.png "Havuzları öngörülemeyen iş yükleri için düşük maliyetli"

[image-wtip-min02008-demo-st-hybrid]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min02008-demo-st-hybrid.png "Kiracı başına veritabanı ve karma ST/MT Tanıtımı"

[image-wtip-min02029-live-app-form-dojo]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min02029-app-form-dogwwod-dojo.png "Dojo gösteren Canlı uygulama formu"

[image-wtip-min02854-myob-no-dba]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min02854-myob-no-dba.png "MYOB ve DBA görüş içinde değil"

[image-wtip-min02940-myob-elastic]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min02940-myob-elastic-pool-usage-example.png "MYOB elastik Havuz Kullanım örneği"

[image-wtip-min03136-learning-isvs]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min03136-myob-isv-saas-patterns-design-scale.png "MYOB ve diğer ISV'ler öğrenme"

[image-wtip-min04315-patterns-compose]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min04315-patterns-compose-into-e2e-saas-scenario-st-mt.png "E2E SaaS senaryoya desenleri oluşturma"

[image-wtip-min04733-canonical-hybrid]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min04733-canonical-hybrid-mt-saas-app.png "Kurallı karma çok kiracılı SaaS uygulaması"

[image-wtip-min04810-wingtip-saas-app]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min04810-saas-sample-app-descr-of-modules-links.png "Wingtip SaaS örnek uygulaması"

[image-wtip-min04910-scenarios-tutorials]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min04910-scenarios-patterns-explored-tutorials.png "Senaryolar ve öğreticilerde incelediniz desenleri"

[image-wtip-min05018-demo-tutorials-github]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min05018-demo-saas-tutorials-github-repo.png "Öğreticiler ve GitHub deposunun Tanıtımı"

[image-wtip-min05038-github-wingtipsaas]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min05038-github-repo-wingtipsaas.png "GitHub deposunu Microsoft/WingtipSaaS"

[image-wtip-min05620-exploring-patterns]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min05620-exploring-patterns-tutorials.png "Desenler keşfetme"

[image-wtip-min05744-provisioning-tenants-onboarding-1]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min05744-provisioning-tenants-connecting-run-time-1.png "Kiracılar sağlama ve ekleme"

[image-wtip-min05858-provisioning-tenants-app-connection-2]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min05858-provisioning-tenants-connecting-run-time-2.png "Kiracılar ve uygulama bağlantısı sağlama"

[image-wtip-min05943-demo-management-scripts-st]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min05943-demo-management-scripts-provisioning-st.png "Tek bir kiracı sağlama yönetim komut dosyaları Tanıtımı"

[image-wtip-min10002-powershell-provision-catalog]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min10002-powershell-code.png "PowerShell için sağlama ve kataloğa kaydetme"

[image-wtip-min10330-sql-select-tenantsextended]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min10330-ssms-tenantcatalog.png "T-SQL SELECT * TenantsExtended gelen"

[image-wtip-min10436-managing-unpredictable-workloads]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min10436-managing-unpredictable-tenant-workloads.png "Öngörülemez Kiracı iş yüklerini yönetme"

[image-wtip-min10639-elastic-pool-monitoring]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min10639-elastic-pool-monitoring.png "Esnek Havuz izleme"

[image-wtip-min10942-load-generation]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min10942-schema-management-scale.png "Yük oluşturma ve performans izleme"

[image-wtip-min11033-schema-management-scale]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min11033-schema-manage-1000s-dbs-one.png "Uygun ölçekte Şema Yönetimi"

[image-wtip-min11221-distributed-query]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min11221-distributed-query-all-tenants-asif-single-db.png "Kiracı veritabanlarında dağıtılmış sorgu"

[image-wtip-min11232-demo-ticket-generation]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min11232-demo-ticket-generation-distributed-query.png "Tanıtım bilet oluşturma"

[image-wtip-min11246-ssms-adhoc-analytics]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min11246-tsql-adhoc-analystics-db-elastic-query.png "SSMS geçici analiz"

[image-wtip-min11632-extract-tenant-data-sql-dw]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min11632-extract-tenant-data-analytics-db-dw.png "SQL DW'ye Kiracı verilerini ayıklama"

[image-wtip-min11648-graph-daily-sale-distribution]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min11648-graph-daily-sale-contoso-concert-hall.png "Günlük satış dağılım grafiği"

[image-wtip-min11952-wrap-up-call-action]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min11952-wrap-call-action-saasfeedback.png "Kaydır ve eylem çağrısı"

[image-wtip-min12042-resources-more-info]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min12042-resources-blog-github-tutorials-get-started.png "Daha fazla bilgi için kaynaklar"




<!-- Article link reference IDs. -->

[saas-concept-design-patterns-563e]: saas-tenancy-app-design-patterns.md

[saas-how-welcome-wingtip-app-679t]: saas-tenancy-welcome-wingtip-tickets-app.md


[video-on-youtube-com-478y]: https://www.youtube.com/watch?v=jjNmcKBVjrc&t=1

[video-on-channel9-479c]: https://channel9.msdn.com/Events/Ignite/Microsoft-Ignite-Orlando-2017/BRK3120


[resource-blog-saas-patterns-app-dev-sql-db-768h]: https://azure.microsoft.com/blog/saas-patterns-accelerate-saas-application-development-on-sql-database/


[github-wingtip-standaloneapp]: https://github.com/Microsoft/WingtipTicketsSaaS-StandaloneApp/

[github-wingtip-dbpertenant]: https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant/

[github-wingtip-multitenantdb]: https://github.com/Microsoft/WingtipTicketsSaaS-MultiTenantDB/

