---
title: Planlar ve faturalandırma - Azure Zamanlayıcı
description: Planlar ve faturalandırma için Azure Zamanlayıcı hakkında bilgi edinin
services: scheduler
ms.service: scheduler
author: derek1ee
ms.author: deli
ms.reviewer: klam
ms.assetid: 13a2be8c-dc14-46cc-ab7d-5075bfd4d724
ms.topic: article
ms.date: 08/18/2016
ms.openlocfilehash: 3a8664497d3d082ec1c7f584188854991e872d50
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64720437"
---
# <a name="plans-and-billing-for-azure-scheduler"></a>Planlar ve faturalandırma için Azure Zamanlayıcı

> [!IMPORTANT]
> Kullanımdan kaldırılan Azure Scheduler uygulamasının yerini [Azure Logic Apps](../logic-apps/logic-apps-overview.md) alacaktır. İş zamanlamak için [Azure Logic Apps'ı deneyebilirsiniz](../scheduler/migrate-from-scheduler-to-logic-apps.md). 

## <a name="job-collection-plans"></a>İş koleksiyonu planları

Azure Scheduler'da işleri belirli bir dizi iş koleksiyonu içerir. İş koleksiyonu, Faturalanabilir bir varlıktır ve burada açıklanan standart, Premium P10 ve P20 Premium planlarını içinde sunulur: 

| İş koleksiyonu planı | Koleksiyon başına en fazla iş | En fazla yinelenme | Abonelik başına en fazla iş koleksiyonu | Limits | 
|:--- |:--- |:--- |:--- |:--- |
| **Standart** | Koleksiyon başına 50 iş | Bir dakika başına. İşleri dakikada birden daha sık çalıştırılamaz. | Her Azure aboneliği, en fazla 100 standart iş koleksiyonu olabilir. | Zamanlayıcı tam özellik kümesi erişimi | 
| **P10 Premium** | Koleksiyon başına 50 iş | Bir dakika başına. İşleri dakikada birden daha sık çalıştırılamaz. | Her Azure aboneliği, en fazla 10.000 P10 Premium iş koleksiyonu olabilir. Daha fazla koleksiyon <a href="mailto:wapteams@microsoft.com">bizimle</a>. | Zamanlayıcı tam özellik kümesi erişimi |
| **P20 Premium** | Koleksiyon başına 1000 iş | Bir dakika başına. İşleri dakikada birden daha sık çalıştırılamaz. | Her Azure aboneliği, en fazla 5000 P20 Premium iş koleksiyonu olabilir. Daha fazla koleksiyon <a href="mailto:wapteams@microsoft.com">bizimle</a>. | Zamanlayıcı tam özellik kümesi erişimi |
|||||| 

## <a name="pricing"></a>Fiyatlandırma

Fiyatlandırma ayrıntıları için bkz: [Zamanlayıcı fiyatlandırma](https://azure.microsoft.com/pricing/details/scheduler/).

## <a name="upgrade-or-downgrade-plans"></a>Yükseltme veya düşürme planları

Herhangi bir zamanda, yükseltebilir veya bir iş koleksiyonu planı arasında standart, Premium P10 ve P20 Premium planlar düşürebilirsiniz.

## <a name="active-status-and-billing"></a>Etkin durumu ve faturalandırma

Tüm Azure aboneliğiniz geçici devre dışı durumuna faturalandırma sorunları nedeniyle girmeyeceğini sürece iş koleksiyonları her zaman etkindir. Ve tek bir işlem aracılığıyla bir iş koleksiyonundaki tüm işlerin, devre dışı bırakabilirsiniz ancak iş koleksiyonu, bu nedenle bu eylemi proje koleksiyonunun faturalama durumu değişmez *hala* faturalandırılır. Boş iş koleksiyonu etkin olarak kabul edilir ve faturalandırılır.

İş koleksiyonu faturalandırılır olmadığından emin olmak için iş koleksiyonu silmeniz gerekir.

## <a name="standard-billable-units"></a>Standart Faturalanabilir birimler

Standart Faturalanabilir birim, en fazla 10 standart iş koleksiyonu olabilir. Standart iş koleksiyonu koleksiyon başına en fazla 50 iş olabileceğinden, 500 projelere veya neredeyse 22 kadar sahip Azure Aboneliğinize bir standart faturalandırma birimi sağlar *milyon* iş yürütme / ay. Bu listeyi nasıl faturalandırılırsınız standart iş koleksiyonu, çeşitli rakamlara dayanan açıklanmaktadır:

* 1 ile 10 standart iş koleksiyonu arasında varsa, bir standart faturalandırma birimi için faturalandırılırsınız. 

* 20 standart iş koleksiyonu ile 11 arasında varsa, iki standart faturalandırma birimi için faturalandırılırsınız. 

* 21 arasında 30 standart iş koleksiyonu varsa, standart üç faturalandırma birimi için faturalandırılırsınız ve benzeri.

## <a name="p10-premium-billable-units"></a>P10 premium Faturalanabilir birimler

P10 premium Faturalanabilir birim, en fazla 10.000 P10 Premium iş koleksiyonu olabilir. Koleksiyon başına en fazla 50 iş P10 Premium iş koleksiyonu olabileceğinden, 500.000 projelere veya neredeyse 22 kadar sahip Azure Aboneliğinize bir P10 premium faturalandırma birimi sağlar *milyar* iş yürütme / ay. 

P10 Premium iş koleksiyonu standart iş koleksiyonu aynı özellikleri sağlar, ancak daha fazla ölçeklenebilirlik sağlar ve birçok iş koleksiyonları gerektiren uygulamalar için bir fiyat sonu sunar. Bu listeyi nasıl faturalandırılırsınız P10 Premium iş koleksiyonu, çeşitli rakamlara dayanan açıklanmaktadır:

* 1 ila 10.000 P10 Premium iş koleksiyonu arasında varsa, bir P10 premium faturalandırma birimi için faturalandırılırsınız. 

* 10,001 ve 20.000 P10 Premium iş koleksiyonu arasında varsa, 2 P10 premium faturalandırma birimi için faturalandırılırsınız ve benzeri.

## <a name="p20-premium-billable-units"></a>P20 premium Faturalanabilir birimler

P20 premium Faturalanabilir birim, en fazla 5000 P20 Premium iş koleksiyonu olabilir. İş koleksiyonu başına en fazla 1.000 iş P20 Premium iş koleksiyonu olabileceğinden, 5,000,000 projelere veya neredeyse 220 kadar sahip Azure Aboneliğinize bir P20 premium faturalandırma birimi sağlar *milyar* iş yürütme / ay.

P20 Premium iş koleksiyonu P10 Premium iş koleksiyonu aynı özellikleri sağlar, ancak daha fazla sayıda koleksiyon başına iş ve bir büyük toplam iş sayısıyla genel P10 Premium, daha fazla ölçeklenebilirlik sağlayarak daha da destekler.

## <a name="plan-comparison"></a>Plan karşılaştırması

* 100'den fazla standart iş koleksiyonu (standart 10 faturalandırma birimi) varsa, bir Premium planı tüm iş koleksiyonları sağlayarak daha iyi bir fırsat alabilirsiniz.

* Bir standart iş koleksiyonu ve bir Premium iş koleksiyonu olması durumunda bir standart faturalandırma birimi için faturalandırılırsınız *ve* bir premium faturalandırma birimidir.

  Zamanlayıcı hizmeti, standart veya premium etkin iş koleksiyonu sayısına göre faturalandırır.

## <a name="see-also"></a>Ayrıca bkz.

* [Azure Scheduler nedir?](scheduler-intro.md)
* [Azure Scheduler kavramları, terminolojisi ve varlık hiyerarşisi](scheduler-concepts-terms.md)
* [Azure Scheduler sınırları, varsayılanları ve hata kodları](scheduler-limits-defaults-errors.md)