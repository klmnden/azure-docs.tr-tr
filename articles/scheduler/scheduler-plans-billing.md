---
title: Planlar ve faturalandırma Azure scheduler'da
description: Planlar ve faturalandırma Azure scheduler'da
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
ms.openlocfilehash: 03f335634b7ce1fe4aa6251d6ec21922ed9b84c8
ms.sourcegitcommit: 11321f26df5fb047dac5d15e0435fce6c4fde663
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37887496"
---
# <a name="plans-and-billing-in-azure-scheduler"></a>Planlar ve faturalandırma Azure scheduler'da
## <a name="job-collection-plans"></a>İş koleksiyonu planları
İş, Faturalanabilir Azure Scheduler varlık koleksiyonlarıdır. İş koleksiyonları içeren iş sayısı ve aşağıda açıklanan üç planlara – standart, Premium P10 ve P20 Premium – gelir.

| **İş koleksiyonu planı** | **En fazla iş koleksiyonu başına iş sayısı** | **En fazla yinelenme** | **Abonelik başına en fazla iş koleksiyonu** | **Limitler** |
|:--- |:--- |:--- |:--- |:--- |
| **Standart** |İş koleksiyonu başına 50 iş |Dakikada bir kez. İş bir dakikadan daha sık yürütülemiyor |Bir aboneliğin 100'e kadar standart iş koleksiyonu izin verilir |Zamanlayıcı tam özellik kümesi erişimi |
| **P10 Premium** |İş koleksiyonu başına 50 iş |Dakikada bir kez. İş bir dakikadan daha sık yürütülemiyor |Bir abonelik en fazla 10.000 P10 Premium iş koleksiyonu izin verilir. <a href="mailto:wapteams@microsoft.com">Bizimle iletişime geçin</a> daha fazla bilgi için. |Zamanlayıcı tam özellik kümesi erişimi |
| **P20 Premium** |İş koleksiyonu başına 1000 iş |Dakikada bir kez. İş bir dakikadan daha sık yürütülemiyor |Bir abonelik en fazla 10.000 P20 Premium iş koleksiyonu izin verilir. <a href="mailto:wapteams@microsoft.com">Bizimle iletişime geçin</a> daha fazla bilgi için. |Zamanlayıcı tam özellik kümesi erişimi |

## <a name="upgrades-and-downgrades-of-job-collection-plans"></a>Yükseltme ve eski sürümü yükleme işlemleri iş koleksiyonu planı
Yükseltme veya düşürme standart, Premium P10 ve P20 Premium planlar arasında dilediğiniz zaman bir iş koleksiyonu planı.

## <a name="billing-and-azure-plans"></a>Faturalandırma ve Azure planları
Ardından, 100'den fazla standart iş koleksiyonu (standart 10 faturalandırma birimi) varsa, tüm proje koleksiyonlarında premium plana sahip daha iyi bir fırsat olduğu.

Bir standart iş koleksiyonu ve bir premium iş koleksiyonu varsa, faturalandırılan bir standart faturalandırma birimi olan *ve* bir premium faturalandırma birimidir. Zamanlayıcı hizmeti, standart veya premium ayarlayın etkin iş koleksiyonu sayısına bağlı faturalar; Bu sonraki iki bölümde daha ayrıntılı açıklanmıştır.

## <a name="standard-billable-units"></a>Standart Faturalanabilir birimler
Standart Faturalanabilir birim, en fazla 10 standart iş koleksiyonu içerebilir. İş koleksiyonu başına en fazla 50 iş standart iş koleksiyonu olabileceğinden, bir standart faturalandırma birimi – neredeyse 22 milyondan iş yürütmeleri ayda en fazla 500'e kadar işleri bir abonelik sağlar.

1 ile 10 standart iş koleksiyonu arasında varsa, bir standart faturalandırma birimi için faturalandırılırsınız. 20 standart iş koleksiyonu ile 11 arasında varsa, iki standart faturalandırma birimi için faturalandırılırsınız. 21 arasında 30 standart iş koleksiyonu varsa, standart üç faturalandırma birimi için faturalandırılırsınız ve benzeri.

## <a name="p10-premium-billable-units"></a>P10 Premium Faturalanabilir birimler
P10 premium Faturalanabilir birim, en fazla 10.000 P10 premium iş koleksiyonu içerebilir. İş koleksiyonu başına en fazla 50 iş P10 premium iş koleksiyonu olabileceğinden, bir premium faturalandırma birimi en fazla 500.000 işler – kadar her ay yaklaşık 22 milyar iş yürütmeleri bir abonelik sağlar.

1 ila 10.000 premium iş koleksiyonu arasında varsa, bir P10 premium faturalandırma birimi için faturalandırılırsınız. 20.000 premium iş koleksiyonu 10,001 arasında varsa, 2 P10 premium faturalandırma birimi için faturalandırılırsınız ve benzeri.

Bu nedenle, P10 premium iş koleksiyonu standart iş koleksiyonu ile aynı işlevlere sahip ancak birçok iş koleksiyonları uygulamanızın gerektirdiği durumlarda bir fiyat kesmesinden kaçınılmasını sağlar.

## <a name="p20-premium-billable-units"></a>P20 Premium Faturalanabilir birimler
P20 premium Faturalanabilir birim, en fazla 5000 P20 premium iş koleksiyonu içerebilir. İş koleksiyonu başına en fazla 1.000 iş P20 premium iş koleksiyonu olabileceğinden, bir premium faturalandırma birimi – neredeyse 220 milyar aylık iş yürütmeleri kadar varan 5,000,000 işleri bir abonelik sağlar.

P20 premium iş koleksiyonu P10 premium iş koleksiyonu aynı özellikleri sağlar, ancak iş koleksiyonu başına daha yüksek bir sayı iş ve bir büyük toplam iş sayısıyla genel P10 premium daha fazla ölçeklendirilebilirliğe sahip olmasını sağlayarak daha da destekler.

## <a name="billing-and-active-status"></a>Faturalandırma ve etkin durumu
Tüm aboneliğiniz fatura sorunları nedeniyle bazı geçici devre dışı duruma geçti sürece iş koleksiyonları her zaman etkindir. İş koleksiyonu faturalandırılmaz sağlamanın tek yolu, iş koleksiyonu silmektir.

Tek bir işlemde bir iş koleksiyonundaki tüm işlerin devre dışı bırakabilirsiniz ancak iş koleksiyonu fatura durumunu değiştirmez – iş koleksiyonu olacak *hala* gibi faturalandırılır. Benzer şekilde, boş iş koleksiyonu etkin olarak kabul edilir ve faturalandırılır.

## <a name="pricing"></a>Fiyatlandırma
Fiyatlandırma ayrıntıları için bkz: [Zamanlayıcı fiyatlandırma](https://azure.microsoft.com/pricing/details/scheduler/).

## <a name="see-also"></a>Ayrıca Bkz.
 [Scheduler nedir?](scheduler-intro.md)

 [Azure Scheduler kavramları, terminolojisi ve varlık hiyerarşisi](scheduler-concepts-terms.md)

 [Azure portalında Scheduler’ı kullanmaya başlama](scheduler-get-started-portal.md)

 [Azure Scheduler REST API başvurusu](https://msdn.microsoft.com/library/mt629143)

 [Azure Scheduler PowerShell cmdlet’leri başvurusu](scheduler-powershell-reference.md)

 [Yüksek Azure Scheduler kullanılabilirliği ve güvenilirliği](scheduler-high-availability-reliability.md)

 [Azure Scheduler sınırları, varsayılanları ve hata kodları](scheduler-limits-defaults-errors.md)

 [Azure Scheduler giden bağlantı kimlik doğrulaması](scheduler-outbound-authentication.md)

