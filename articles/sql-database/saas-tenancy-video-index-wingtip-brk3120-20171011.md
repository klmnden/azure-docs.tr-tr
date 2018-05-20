---
title: Video dizine, Azure SaaS SQL uygulama | Microsoft Docs
description: Bu makalede çeşitli zaman noktaları bizim 81 dakika video 11 Ekim 2017 tutulan Ignite konferans gelen SaaS DB kiralama uygulama tasarımı ile ilgili dizinler. Şimdi sizi ilgilendiren bölümüne atlayabilirsiniz. En az 3 desenleri açıklanmaktadır. Geliştirme ve yönetimini basitleştirmenize azure özellikleri açıklanmaktadır.
services: sql-database
ms.date: 05/14/2018
ms.service: sql-database
ms.reviewer: billgib
ms.topic: article
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 4ea62855b61cb7439b19204564cbc2d7fcdbd0fa
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="video-indexed-and-annotated-for-multi-tenant-saas-app-using-azure-sql-database"></a>Video dizine ve Azure SQL veritabanı kullanarak çok kiracılı SaaS uygulaması için açıklama

Bu makalede bir SaaS kiralama modelleri veya desenleri hakkında video 81 dakika zaman konumlarını içine açıklamalı dizinidir. Bu makalede, geriye doğru atlamayı sağlar veya hangi bölümünün videoya İleri ilginizi çeken. Video Azure SQL veritabanındaki bir çok Kiracı veritabanı uygulama için önemli tasarım seçenekleri açıklanmaktadır. Video gösterileri, izlenecek yollar yönetim kod ve deneyimi tarafından yazılmış Belgelerimizdeki olabilir daha haberdar zamanlarda daha fazla ayrıntı içerir.

Video bulunan Belgelerimizi yazılı bilgileri büyüten: 
- *Kavramsal:* [çok kiracılı SaaS veritabanı kiralama desenleri][saas-concept-design-patterns-563e]
- *Öğreticiler:* [Wingtip biletleri SaaS uygulaması][saas-how-welcome-wingtip-app-679t]

Video ve makaleleri bir çok kiracılı uygulama bulutta Azure SQL veritabanı oluşturma birçok aşamalar açıklanmaktadır. Azure SQL veritabanı'nın özel özellikleri geliştirmek ve her ikisi de daha kolay çok kiracılı uygulamalara ve güvenilir bir şekilde kullanıcı uygulamak kolaylaştırır.

Düzenli olarak yazılmış Belgelerimizi güncelleştiriyoruz. Video düzenlenen ya da güncelleştirilmiş, bu nedenle sonunda daha fazla kendi ayrıntı güncelliğini olabilir.



## <a name="sequence-of-38-time-indexed-screenshots"></a>38 zaman dizine ekran görüntüleri bir dizi

Bu bölümde video 81 dakika boyunca 38 tartışmalara saat konumunu dizinler. Her zaman dizin video bir ekran görüntüsü ile ve bazen ek bilgilerle işaretiyle gösterilir.

Her zaman dizinidir biçiminde *dd:*. Örneğin, ikinci etiketli zaman konumu dizine **oturum hedefleri**, yaklaşık zaman konumunda başlayan **0:03:11**.


### <a name="compact-links-to-video-indexed-time-locations"></a>Video dizinli zaman konumlarına Compact bağlantılar

Aşağıdaki konu başlıklarını bu makalenin sonraki bölümlerinde karşılık gelen açıklamalı bölümlerinin bağlantılarını şunlardır:

- [1. **(Başlangıç)**  Hoş Geldiniz slayt, 0:00:03](#anchor-image-wtip-min00001)
- [2. Oturum hedefleri, 0:03:11](#anchor-image-wtip-min00311)
- [3. Gündem, 0:04:17](#anchor-image-wtip-min00417)
- [4. Çok kiracılı web uygulaması, 0:05:05](#anchor-image-wtip-min00505)
- [5. Eylem, 0 App web formunda: 05:55](#anchor-image-wtip-min00555)
- [6. Kiracı başına maliyet (ölçekli, yalıtım, Kurtarma), 0:09:31](#anchor-image-wtip-min00931)
- [7. Veritabanı modelleri için çok kiracılı: Artıları ve eksileri, 0:11:59](#anchor-image-wtip-min01159)
- [8. Karma modeli karıştırır MT/ST, 0 yararları: 13:01](#anchor-image-wtip-min01301)
- [9. Tek Kiracı vs çok kiracılı: Artıları ve eksileri, 0:16:44](#anchor-image-wtip-min01644)
- [10. Havuzları öngörülemeyen iş yükleri için 0 uygun maliyetli: 19:36](#anchor-image-wtip-min01936)
- [11. Kiracı başına veritabanı karma ST/MT, 0 ve Demo: 20:08](#anchor-image-wtip-min02008)
- [12. Canlı Dojo, 0 gösteren uygulama formu: 20:29](#anchor-image-wtip-min02029)
- [13. MYOB ve DBA görüş, 0'ın değil: 28:54](#anchor-image-wtip-min02854)
- [14. MYOB esnek Havuz Kullanım örneği, 0:29:40](#anchor-image-wtip-min02940)
- [15. MYOB ve diğer ISV'ler 0 öğrenme: 31:36](#anchor-image-wtip-min03136)
- [16. Desenler oluşturan 0 E2E SaaS senaryo: 43:15](#anchor-image-wtip-min04315)
- [17. Kurallı karma çok kiracılı SaaS uygulaması, 0:47:33](#anchor-image-wtip-min04733)
- [18. Wingtip SaaS örnek uygulaması, 0:48:10](#anchor-image-wtip-min04810)
- [19. Senaryolar ve öğreticiler, 0 incelediniz desenleri: 49:10](#anchor-image-wtip-min04910)
- [20. Öğreticiler ve Github deposunu, 0 Demo: 50:18](#anchor-image-wtip-min05018)
- [21. Github depo Microsoft/WingtipSaaS, 0:50:38](#anchor-image-wtip-min05038)
- [22. 0 desenleri keşfetme: 56:20](#anchor-image-wtip-min05620)
- [23. Kiracılar ve ekleme, 0 sağlama: 57:44](#anchor-image-wtip-min05744)
- [24. Kiracılar ve uygulama bağlantısı, 0 sağlama: 58:58](#anchor-image-wtip-min05858)
- [25. Tanıtım yönetim komut dosyaları 0 tek bir kiracı sağlama: 59:43](#anchor-image-wtip-min05943)
- [26. PowerShell sağlamak ve Katalog, 1:00:02](#anchor-image-wtip-min10002)
- [27. T-SQL SELECT * TenantsExtended, 1:03: 30'dan](#anchor-image-wtip-min10330)
- [28. Öngörülemeyen Kiracı İş yükleri, 1:04:36 yönetme](#anchor-image-wtip-min10436)
- [29. İzleme, esnek Havuz 1:06:39](#anchor-image-wtip-min10639)
- [30. Yükleme oluşturma ve performans izleme, 1:09:42](#anchor-image-wtip-min10942)
- [31. Şema Yönetimi ölçekte 1:10:33](#anchor-image-wtip-min11033)
- [32. Kiracı veritabanları, 1:12:21 arasında dağıtılmış sorgu](#anchor-image-wtip-min11221)
- [33. Bilet oluşturma, 1:12:32 Tanıtımı](#anchor-image-wtip-min11232)
- [34. SSMS geçici analizi, 1:12:46](#anchor-image-wtip-min11246)
- [35. 1:16:32 SQL DW Kiracı veri ayıklamak](#anchor-image-wtip-min11632)
- [36. Grafik günlük satış dağıtım, 1:16:48](#anchor-image-wtip-min11648)
- [37. Kaydırma ve eylem, 1:19:52 çağırın](#anchor-image-wtip-min11952)
- [38. Daha fazla bilgi için 1:20:42 kaynakları](#anchor-image-wtip-min12042)


&nbsp;

### <a name="annotated-index-time-locations-in-the-video"></a>Açıklamalı dizin zaman konumlarını video

Herhangi bir ekran görüntü tıklamak video tam zaman konuma götürür.


&nbsp; <a name="anchor-image-wtip-min00001"/>
#### <a name="1-start-welcome-slide-00001"></a>1. *(Başlangıç)*  Hoş Geldiniz slayt, 0:00:01

*MYOB öğrenme: Tasarım desenleri Azure SQL veritabanı - BRK3120 SaaS uygulamaları için*

[![Hoş Geldiniz slayt][image-wtip-min00003-brk3120-whole-welcome]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=1)

- Başlık: MYOB öğrenme: Tasarım desenleri Azure SQL veritabanı SaaS uygulamaları için
- Bill.Gibson@microsoft.com
- Baş Program Yöneticisi, Azure SQL veritabanı
- Microsoft Ignite oturum BRK3120, FL in konak İlçesindeki, ABD, 11 Ekim 2017


&nbsp; <a name="anchor-image-wtip-min00311"/>
#### <a name="2-session-objectives-00153"></a>2. Oturum hedefleri, 0:01:53
[![Oturum hedefleri][image-wtip-min00311-session]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=113)

- Çok kiracılı uygulamalarla Artıları ve eksileri için alternatif modeller.
- Geliştirme, yönetim ve kaynak maliyetlerini azaltmak için SaaS desenleri.
- Örnek bir uygulama + komut dosyaları.
- PaaS özellikleri + SaaS desenleri SQL veritabanı yüksek düzeyde ölçeklenebilir, maliyeti verimli bir veri platformu çok kiracılı SaaS olun.


&nbsp; <a name="anchor-image-wtip-min00417"/>
#### <a name="3-agenda-00409"></a>3. Gündem, 0:04:09
[![Gündem][image-wtip-min00417-agenda]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=249)


&nbsp; <a name="anchor-image-wtip-min00505"/>
#### <a name="4-multi-tenant-web-app-00500"></a>4. Çok kiracılı web uygulaması, 0:05:00
[![Wingtip SaaS uygulama: çok kiracılı web uygulaması][image-wtip-min00505-web-app]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=300)


&nbsp; <a name="anchor-image-wtip-min00555"/>
#### <a name="5-app-web-form-in-action-00539"></a>5. Eylem, 0 App web formunda: 05:39
[![Uygulama web formunda eylemi][image-wtip-min00555-app-web-form]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=339)


&nbsp; <a name="anchor-image-wtip-min00931"/>
#### <a name="6-per-tenant-cost-scale-isolation-recovery-00658"></a>6. Kiracı başına maliyet (ölçekli, yalıtım, Kurtarma), 0:06:58
[![Her Kiracı maliyet, Ölçek, yalıtım, Kurtarma][image-wtip-min00931-per-tenant-cost]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=418)


&nbsp; <a name="anchor-image-wtip-min01159"/>
#### <a name="7-database-models-for-multi-tenant-pros-and-cons-00952"></a>7. Veritabanı modelleri için çok kiracılı: Artıları ve eksileri, 0:09:52
[![Veritabanı modelleri için çok kiracılı: Artıları ve eksileri][image-wtip-min01159-db-models-pros-cons]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=592)


&nbsp; <a name="anchor-image-wtip-min01301"/>
#### <a name="8-hybrid-model-blends-benefits-of-mtst-01229"></a>8. Karma modeli karıştırır MT/ST, 0 yararları: 12:29
[![Karma modeli MT/ST yararları karıştırır][image-wtip-min01301-hybrid]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=749)


&nbsp; <a name="anchor-image-wtip-min01644"/>
#### <a name="9-single-tenant-vs-multi-tenant-pros-and-cons-01311"></a>9. Tek Kiracı vs çok kiracılı: Artıları ve eksileri, 0:13:11
[![Tek Kiracı vs çok kiracılı: Artıları ve eksileri][image-wtip-min01644-st-vs-mt]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=791)


&nbsp; <a name="anchor-image-wtip-min01936"/>
#### <a name="10-pools-are-cost-effective-for-unpredictable-workloads-01749"></a>10. Havuzları öngörülemeyen iş yükleri için 0 uygun maliyetli: 17:49
[![Havuzları öngörülemeyen iş yükleri için düşük maliyetli][image-wtip-min01936-pools-cost]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=1069)


&nbsp; <a name="anchor-image-wtip-min02008"/>
#### <a name="11-demo-of-database-per-tenant-and-hybrid-stmt-01959"></a>11. Kiracı başına veritabanı karma ST/MT, 0 ve Demo: 19:59
[![Kiracı başına veritabanı ve karma ST/MT Tanıtımı][image-wtip-min02008-demo-st-hybrid]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=1199)


&nbsp; <a name="anchor-image-wtip-min02029"/>
#### <a name="12-live-app-form-showing-dojo-02010"></a>12. Canlı Dojo, 0 gösteren uygulama formu: 20:10
[![Dojo gösteren dinamik uygulama formu][image-wtip-min02029-live-app-form-dojo]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=1210)

&nbsp; <a name="anchor-image-wtip-min02854"/>
#### <a name="13-myob-and-not-a-dba-in-sight-02506"></a>13. MYOB ve DBA görüş, 0'ın değil: 25:06
[![MYOB ve DBA görüş içinde değil][image-wtip-min02854-myob-no-dba]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=1506)


&nbsp; <a name="anchor-image-wtip-min02940"/>
#### <a name="14-myob-elastic-pool-usage-example-02930"></a>14. MYOB esnek Havuz Kullanım örneği, 0:29:30
[![MYOB esnek Havuz Kullanım örneği][image-wtip-min02940-myob-elastic]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=1770)


&nbsp; <a name="anchor-image-wtip-min03136"/>
#### <a name="15-learning-from-myob-and-other-isvs-03125"></a>15. MYOB ve diğer ISV'ler 0 öğrenme: 31:25
[![MYOB ve diğer ISV'ler öğrenme][image-wtip-min03136-learning-isvs]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=1885)


&nbsp; <a name="anchor-image-wtip-min04315"/>
#### <a name="16-patterns-compose-into-e2e-saas-scenario-03142"></a>16. Desenler oluşturan 0 E2E SaaS senaryo: 31:42
[![E2E SaaS senaryo düzenlerini oluşturma][image-wtip-min04315-patterns-compose]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=1902)


&nbsp; <a name="anchor-image-wtip-min04733"/>
#### <a name="17-canonical-hybrid-multi-tenant-saas-app-04604"></a>17. Kurallı karma çok kiracılı SaaS uygulaması, 0:46:04
[![Kurallı karma çok kiracılı SaaS uygulama][image-wtip-min04733-canonical-hybrid]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=2764)


&nbsp; <a name="anchor-image-wtip-min04810"/>
#### <a name="18-wingtip-saas-sample-app-04801"></a>18. Wingtip SaaS örnek uygulaması, 0:48:01
[![Wingtip SaaS örnek uygulaması][image-wtip-min04810-wingtip-saas-app]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=2881)


&nbsp; <a name="anchor-image-wtip-min04910"/>
#### <a name="19-scenarios-and-patterns-explored-in-the-tutorials-04900"></a>19. Senaryolar ve öğreticiler, 0 incelediniz desenleri: 49:00
[![Senaryolar ve eğitimlerine incelediniz desenleri][image-wtip-min04910-scenarios-tutorials]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=2940)


&nbsp; <a name="anchor-image-wtip-min05018"/>
#### <a name="20-demo-of-tutorials-and-github-repository-05012"></a>20. Öğreticiler ve Github deposunu, 0 Demo: 50:12
[![Tanıtım öğreticiler ve Github depo][image-wtip-min05018-demo-tutorials-github]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=3012)


&nbsp; <a name="anchor-image-wtip-min05038"/>
#### <a name="21-github-repo-microsoftwingtipsaas-05032"></a>21. Github depo Microsoft/WingtipSaaS, 0:50:32
[![Github depo Microsoft/WingtipSaaS][image-wtip-min05038-github-wingtipsaas]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=3032)


&nbsp; <a name="anchor-image-wtip-min05620"/>
#### <a name="22-exploring-the-patterns-05615"></a>22. 0 desenleri keşfetme: 56:15
[![Desenler keşfetme][image-wtip-min05620-exploring-patterns]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=3375)


&nbsp; <a name="anchor-image-wtip-min05744"/>
#### <a name="23-provisioning-tenants-and-onboarding-05619"></a>23. Kiracılar ve ekleme, 0 sağlama: 56:19
[![Sağlama kiracılar ve ekleme][image-wtip-min05744-provisioning-tenants-onboarding-1]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=3379)


&nbsp; <a name="anchor-image-wtip-min05858"/>
#### <a name="24-provisioning-tenants-and-application-connection-05752"></a>24. Kiracılar ve uygulama bağlantısı, 0 sağlama: 57:52
[![Kiracılar ve uygulama bağlantı sağlama][image-wtip-min05858-provisioning-tenants-app-connection-2]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=3472)


&nbsp; <a name="anchor-image-wtip-min05943"/>
#### <a name="25-demo-of-management-scripts-provisioning-a-single-tenant-05936"></a>25. Tanıtım yönetim komut dosyaları 0 tek bir kiracı sağlama: 59:36
[![Tek bir kiracı sağlama yönetim komut dosyaları Tanıtımı][image-wtip-min05943-demo-management-scripts-st]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=3576)


&nbsp; <a name="anchor-image-wtip-min10002"/>
#### <a name="26-powershell-to-provision-and-catalog-05956"></a>26. PowerShell sağlamak ve Katalog, 0:59:56
[![Sağlama ve Katalog için PowerShell][image-wtip-min10002-powershell-provision-catalog]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=3596)


&nbsp; <a name="anchor-image-wtip-min10330"/>
#### <a name="27-t-sql-select--from-tenantsextended-10325"></a>27. T-SQL SELECT * TenantsExtended, 1:03:25 gelen
[![T-SQL SELECT * TenantsExtended gelen][image-wtip-min10330-sql-select-tenantsextended]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=3805)


&nbsp; <a name="anchor-image-wtip-min10436"/>
#### <a name="28-managing-unpredictable-tenant-workloads-10334"></a>28. Öngörülemeyen Kiracı İş yükleri, 1:03:34 yönetme
[![Öngörülemeyen Kiracı İş yükleri yönetme][image-wtip-min10436-managing-unpredictable-workloads]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=3814)


&nbsp; <a name="anchor-image-wtip-min10639"/>
#### <a name="29-elastic-pool-monitoring-10632"></a>29. İzleme, esnek Havuz 1:06:32
[![Esnek Havuz izleme][image-wtip-min10639-elastic-pool-monitoring]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=3992)


&nbsp; <a name="anchor-image-wtip-min10942"/>
#### <a name="30-load-generation-and-performance-monitoring-10937"></a>30. Oluşturma ve performans izleme, 1:09:37 yükle
[![Yükleme oluşturma ve performans izlemesi][image-wtip-min10942-load-generation]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=4117)


&nbsp; <a name="anchor-image-wtip-min11033"/>
#### <a name="31-schema-management-at-scale-10940"></a>31. Şema Yönetimi ölçekte 1:09:40
[![Şema Yönetimi][image-wtip-min11033-schema-management-scale]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=34120)


&nbsp; <a name="anchor-image-wtip-min11221"/>
#### <a name="32-distributed-query-across-tenant-databases-11118"></a>32. Kiracı veritabanları, 1:11:18 arasında dağıtılmış sorgu
[![Kiracı veritabanları arasında dağıtılmış sorgu][image-wtip-min11221-distributed-query]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=4278)


&nbsp; <a name="anchor-image-wtip-min11232"/>
#### <a name="33-demo-of-ticket-generation-11228"></a>33. Bilet oluşturma, 1:12:28 Tanıtımı
[![Bilet oluşturma Tanıtımı][image-wtip-min11232-demo-ticket-generation]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=4348)


&nbsp; <a name="anchor-image-wtip-min11246"/>
#### <a name="34-ssms-adhoc-analytics-11235"></a>34. SSMS geçici analizi, 1:12:35
[![SSMS geçici analizi][image-wtip-min11246-ssms-adhoc-analytics]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=4355)


&nbsp; <a name="anchor-image-wtip-min11632"/>
#### <a name="35-extract-tenant-data-into-sql-dw-11546"></a>35. 1:15:46 SQL DW Kiracı veri ayıklamak
[![SQL DW Kiracı veri ayıklamak][image-wtip-min11632-extract-tenant-data-sql-dw]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=4546)


&nbsp; <a name="anchor-image-wtip-min11648"/>
#### <a name="36-graph-of-daily-sale-distribution-11638"></a>36. Grafik günlük satış dağıtım, 1:16:38
[![Günlük satış dağıtım grafiği][image-wtip-min11648-graph-daily-sale-distribution]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=4598)


&nbsp; <a name="anchor-image-wtip-min11952"/>
#### <a name="37-wrap-up-and-call-to-action-11743"></a>37. Kaydırma ve eylem, 1:17:43 çağırın
[![Kaydırma ve eyleme çağırın][image-wtip-min11952-wrap-up-call-action]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=4663)


&nbsp; <a name="anchor-image-wtip-min12042"/>
#### <a name="38-resources-for-more-information-12035"></a>38. Daha fazla bilgi için 1:20:35 kaynakları
[![Daha fazla bilgi için kaynaklar][image-wtip-min12042-resources-more-info]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=4835)

- [Blog gönderisi, 22 May 2017][resource-blog-saas-patterns-app-dev-sql-db-768h]

- *Kavramsal:* [çok kiracılı SaaS veritabanı kiralama desenleri][saas-concept-design-patterns-563e]

- *Öğreticiler:* [Wingtip biletleri SaaS uygulaması][saas-how-welcome-wingtip-app-679t]

- Github depoları Wingtip biletleri SaaS kiralama uygulama özellikleri için:
    - [Github depo için - tek başına uygulama modeli][github-wingtip-standaloneapp].
    - [Github depo - DB başına Kiracı modeli için][github-wingtip-dbpertenant].
    - [Github depo - çok Kiracılı DB modeli için][github-wingtip-multitenantdb].





## <a name="next-steps"></a>Sonraki adımlar

- [İlk öğretici makalesi][saas-how-welcome-wingtip-app-679t]




<!-- Image link reference IDs. -->

[image-wtip-min00003-brk3120-whole-welcome]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min00003-brk3120-welcome-myob-design-saas-app-sql-db.png "Hoş Geldiniz slayt"

[image-wtip-min00311-session]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min00311-session-objectives-takeaway.png "Oturum hedefler."

[image-wtip-min00417-agenda]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min00417-agenda-app-management-models-patterns.png "Gündem."

[image-wtip-min00505-web-app]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min00505-wingtip-saas-app-mt-web.png "Wingtip SaaS uygulama: çok kiracılı web uygulaması"

[image-wtip-min00555-app-web-form]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min00555-app-form-contoso-concert-hall-night-opera.png "Uygulama web formunda eylemi"

[image-wtip-min00931-per-tenant-cost]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min00931-saas-data-management-concerns.png "Her Kiracı maliyet, Ölçek, yalıtım, Kurtarma"

[image-wtip-min01159-db-models-pros-cons]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min01159-db-models-multi-tenant-saas-apps.png "Veritabanı modelleri için çok kiracılı: Artıları ve eksileri"

[image-wtip-min01301-hybrid]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min01301-hybrib-model-blends-benefits-mt-st.png "Karma modeli MT/ST yararları karıştırır"

[image-wtip-min01644-st-vs-mt]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min01644-st-mt-pros-cons.png "Tek Kiracı vs çok kiracılı: Artıları ve eksileri"

[image-wtip-min01936-pools-cost]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min01936-pools-cost-effective-unpredictable-workloads.png "Havuzları öngörülemeyen iş yükleri için düşük maliyetli"

[image-wtip-min02008-demo-st-hybrid]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min02008-demo-st-hybrid.png "Kiracı başına veritabanı ve karma ST/MT Tanıtımı"

[image-wtip-min02029-live-app-form-dojo]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min02029-app-form-dogwwod-dojo.png "Dojo gösteren dinamik uygulama formu"

[image-wtip-min02854-myob-no-dba]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min02854-myob-no-dba.png "MYOB ve DBA görüş içinde değil"

[image-wtip-min02940-myob-elastic]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min02940-myob-elastic-pool-usage-example.png "MYOB esnek Havuz Kullanım örneği"

[image-wtip-min03136-learning-isvs]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min03136-myob-isv-saas-patterns-design-scale.png "MYOB ve diğer ISV'ler öğrenme"

[image-wtip-min04315-patterns-compose]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min04315-patterns-compose-into-e2e-saas-scenario-st-mt.png "E2E SaaS senaryo düzenlerini oluşturma"

[image-wtip-min04733-canonical-hybrid]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min04733-canonical-hybrid-mt-saas-app.png "Kurallı karma çok kiracılı SaaS uygulama"

[image-wtip-min04810-wingtip-saas-app]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min04810-saas-sample-app-descr-of-modules-links.png "Wingtip SaaS örnek uygulaması"

[image-wtip-min04910-scenarios-tutorials]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min04910-scenarios-patterns-explored-tutorials.png "Senaryolar ve eğitimlerine incelediniz desenleri"

[image-wtip-min05018-demo-tutorials-github]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min05018-demo-saas-tutorials-github-repo.png "Öğreticiler ve Github deposuna Tanıtımı"

[image-wtip-min05038-github-wingtipsaas]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min05038-github-repo-wingtipsaas.png "Github depo Microsoft/WingtipSaaS"

[image-wtip-min05620-exploring-patterns]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min05620-exploring-patterns-tutorials.png "Desenler keşfetme"

[image-wtip-min05744-provisioning-tenants-onboarding-1]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min05744-provisioning-tenants-connecting-run-time-1.png "Sağlama kiracılar ve ekleme"

[image-wtip-min05858-provisioning-tenants-app-connection-2]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min05858-provisioning-tenants-connecting-run-time-2.png "Kiracılar ve uygulama bağlantı sağlama"

[image-wtip-min05943-demo-management-scripts-st]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min05943-demo-management-scripts-provisioning-st.png "Tek bir kiracı sağlama yönetim komut dosyaları Tanıtımı"

[image-wtip-min10002-powershell-provision-catalog]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min10002-powershell-code.png "Sağlama ve Katalog için PowerShell"

[image-wtip-min10330-sql-select-tenantsextended]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min10330-ssms-tenantcatalog.png "T-SQL SELECT * TenantsExtended gelen"

[image-wtip-min10436-managing-unpredictable-workloads]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min10436-managing-unpredictable-tenant-workloads.png "Öngörülemeyen Kiracı İş yükleri yönetme"

[image-wtip-min10639-elastic-pool-monitoring]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min10639-elastic-pool-monitoring.png "Esnek Havuz izleme"

[image-wtip-min10942-load-generation]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min10942-schema-management-scale.png "Yükleme oluşturma ve performans izlemesi"

[image-wtip-min11033-schema-management-scale]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min11033-schema-manage-1000s-dbs-one.png "Şema Yönetimi"

[image-wtip-min11221-distributed-query]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min11221-distributed-query-all-tenants-asif-single-db.png "Kiracı veritabanları arasında dağıtılmış sorgu"

[image-wtip-min11232-demo-ticket-generation]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min11232-demo-ticket-generation-distributed-query.png "Bilet oluşturma Tanıtımı"

[image-wtip-min11246-ssms-adhoc-analytics]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min11246-tsql-adhoc-analystics-db-elastic-query.png "SSMS geçici analizi"

[image-wtip-min11632-extract-tenant-data-sql-dw]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min11632-extract-tenant-data-analytics-db-dw.png "SQL DW Kiracı veri ayıklamak"

[image-wtip-min11648-graph-daily-sale-distribution]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min11648-graph-daily-sale-contoso-concert-hall.png "Günlük satış dağıtım grafiği"

[image-wtip-min11952-wrap-up-call-action]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min11952-wrap-call-action-saasfeedback.png "Kaydırma ve eyleme çağırın"

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

