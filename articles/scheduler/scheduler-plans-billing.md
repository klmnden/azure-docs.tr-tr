---
title: Planlar ve faturalama içinde Azure Zamanlayıcı
description: Planlar ve faturalama içinde Azure Zamanlayıcı
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: ''
ms.assetid: 13a2be8c-dc14-46cc-ab7d-5075bfd4d724
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/18/2016
ms.author: deli
ms.openlocfilehash: b25e97b0f0d0b6f63134a774856eb7ec8f77b679
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="plans-and-billing-in-azure-scheduler"></a>Planlar ve faturalama içinde Azure Zamanlayıcı
## <a name="job-collection-plans"></a>İş koleksiyonu planları
İş, Azure Scheduler Faturalanabilir varlıkta koleksiyonlarıdır. İş koleksiyonları ve aşağıda açıklanan üç planlarda – Standard, P10 Premium ve P20 Premium – gelen işleri sayısını içerir.

| **İş koleksiyonu planı** | **İşler iş koleksiyonu başına maksimum sayısı** | **En fazla yineleme** | **Abonelik başına en fazla iş koleksiyonları** | **Limitler** |
|:--- |:--- |:--- |:--- |:--- |
| **Standart** |İş koleksiyonu başına 50 işleri |Dakikada bir kez. İşlerini bir dakika daha sık yürütülemiyor |Bir abonelik en fazla 100 standart iş koleksiyonları izin verilir |Zamanlayıcı tam özellik kümesini erişimi |
| **P10 Premium** |İş koleksiyonu başına 50 işleri |Dakikada bir kez. İşlerini bir dakika daha sık yürütülemiyor |Bir abonelik 10.000 P10 Premium iş koleksiyonları izin verilir. <a href="mailto:wapteams@microsoft.com">Bize Ulaşın</a> daha fazla bilgi için. |Zamanlayıcı tam özellik kümesini erişimi |
| **P20 Premium** |İş koleksiyonu başına 1000 işleri |Dakikada bir kez. İşlerini bir dakika daha sık yürütülemiyor |Bir abonelik 10.000 P20 Premium iş koleksiyonları izin verilir. <a href="mailto:wapteams@microsoft.com">Bize Ulaşın</a> daha fazla bilgi için. |Zamanlayıcı tam özellik kümesini erişimi |

## <a name="upgrades-and-downgrades-of-job-collection-plans"></a>Yükseltmeleri ve Downgrades iş koleksiyonu planı
Yükseltme veya standart, P10 Premium ve P20 Premium planları arasında dilediğiniz zaman bir iş koleksiyonu planı düşürmek.

## <a name="billing-and-azure-plans"></a>Faturalama ve Azure planları
Ardından 100'den fazla standart iş koleksiyonları (standart 10 fatura birimleri) varsa, tüm iş koleksiyonları premium plana sağlamak için daha iyi bir bölümünü olur.

Bir standart iş koleksiyonu ve bir premium iş koleksiyonu varsa, faturalanan bir standart fatura birimi olan *ve* bir premium faturalandırma birimidir. İçin standart veya premium ayarlamak etkin iş koleksiyonları sayısına dayalı Zamanlayıcı hizmeti faturaları; Bu açıklanmıştır sonraki iki bölümde ilgili daha fazla.

## <a name="standard-billable-units"></a>Standart Faturalanabilir birimleri
Standart Faturalanabilir birim en fazla 10 standart iş koleksiyonları dahil edebilirsiniz. Standart iş koleksiyonu iş koleksiyonu başına en fazla 50 iş olabileceği için bir standart fatura birimi en fazla 500 işler – neredeyse 22 milyon iş yürütmeleri aylık kadar sağlamak bir abonelik sağlar.

1 ile 10 standart iş koleksiyonları arasında varsa, 1 standart Fatura birim için faturalandırılırsınız. 11-20 standart iş koleksiyonları arasında varsa, 2 standart fatura birimler için faturalandırılırsınız. 21 ve 30 standart iş koleksiyonları arasında varsa, 3 standart fatura birimler için faturalandırılırsınız ve benzeri.

## <a name="p10-premium-billable-units"></a>P10 Premium Faturalanabilir birimleri
P10 premium Faturalanabilir birim 10.000 P10 premium iş koleksiyonları dahil edebilirsiniz. İş koleksiyonu başına en fazla 50 iş P10 premium iş koleksiyonu olabileceği için bir premium faturalama birimi kadar 500.000 işler – neredeyse 22 milyar iş yürütmeleri aylık kadar sağlamak bir abonelik sağlar.

1 ila 10.000 premium iş koleksiyonları arasında varsa, 1 P10 premium faturalama birim için faturalandırılırsınız. 10,001 ve 20.000 premium iş koleksiyonları arasında varsa, 2 P10 premium faturalama birimleri için faturalandırılırsınız ve benzeri.

Bu nedenle, P10 premium iş koleksiyonları standart iş koleksiyonları aynı işlevselliğe sahiptir ancak çok sayıda iş koleksiyonları uygulamanızın gerektirdiği durumunda bir fiyat sonu sağlar.

## <a name="p20-premium-billable-units"></a>P20 Premium Faturalanabilir birimleri
P20 premium Faturalanabilir birim en fazla 5000 P20 premium iş koleksiyonları dahil edebilirsiniz. P20 premium iş koleksiyonu iş koleksiyonu başına en çok 1.000 iş olduğundan bir premium faturalama birimi kadar 5,000,000 işler – neredeyse 220 milyar iş yürütmeleri aylık kadar sağlamak bir abonelik sağlar.

P20 premium iş koleksiyonları P10 premium iş koleksiyonları olarak aynı yetenekleri sunar, ancak her bir iş koleksiyonu büyük bir sayı işi ve bir büyük işlerin toplam sayısı genel P10 premium daha fazla ölçeklenebilirlik olmasını sağlayarak daha da destekler.

## <a name="billing-and-active-status"></a>Faturalama ve etkin durumu
Tüm abonelik faturalama sorunları nedeniyle bazı geçici devre dışı duruma geçti sürece iş koleksiyonları her zaman etkindir. Ya da ayarlamak için bir iş koleksiyonu değil faturalandırılır emin olmak için tek yolu olduğundan *serbest* plan veya iş koleksiyonu silmek için.

Tek bir işlemde bir iş koleksiyonundaki tüm işleri devre dışı bırakabilir rağmen iş koleksiyonu fatura durumunu değiştirmez – iş koleksiyonu olacak *hala* faturalandırılacaksınız. Benzer şekilde, boş iş koleksiyonları etkin olarak kabul edilir ve Fatura edilecek.

## <a name="pricing"></a>Fiyatlandırma
Fiyatlandırma ayrıntıları için lütfen bkz [Zamanlayıcı fiyatlandırma](https://azure.microsoft.com/pricing/details/scheduler/).

## <a name="see-also"></a>Ayrıca Bkz.
 [Scheduler nedir?](scheduler-intro.md)

 [Azure Scheduler kavramları, terminolojisi ve varlık hiyerarşisi](scheduler-concepts-terms.md)

 [Azure portalında Scheduler’ı kullanmaya başlama](scheduler-get-started-portal.md)

 [Azure Scheduler REST API başvurusu](https://msdn.microsoft.com/library/mt629143)

 [Azure Scheduler PowerShell cmdlet’leri başvurusu](scheduler-powershell-reference.md)

 [Yüksek Azure Scheduler kullanılabilirliği ve güvenilirliği](scheduler-high-availability-reliability.md)

 [Azure Scheduler sınırları, varsayılanları ve hata kodları](scheduler-limits-defaults-errors.md)

 [Azure Scheduler giden bağlantı kimlik doğrulaması](scheduler-outbound-authentication.md)

