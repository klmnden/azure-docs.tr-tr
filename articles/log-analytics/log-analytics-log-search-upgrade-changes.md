---
title: Azure günlük analizi Değiştirilenler? | Microsoft Belgeleri
description: Bu makale sık sorulan soruları yükseltilmesiyle ilgili günlük analizi, yeni sorgu dili sağlar.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.author: bwren
ms.openlocfilehash: 017a1da233827f19489a99b234ee9009fd9f6fe3
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2017
ms.locfileid: "26039868"
---
# <a name="whats-changed-in-azure-log-analytics"></a>Azure günlük analizi Değiştirilenler?
Sorgu dili yanı sıra kendi vardır çeşitli iyileştirmeler ve günlük analizi çalışma alanınız olduğunda farkında olmanız gereken değişiklikleri [yeni sorgu dili yükseltilmiş](log-analytics-log-search-new.md).  Bu makalede, eski ve yükseltilmiş çalışma her ayrıntılı içerik için bağlantılar ile birlikte arasındaki değişiklikleri kısaca açıklanmaktadır. 

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Whats-changed-in-Azure-Log-Analytics/player]

Bkz: [yeni günlük analizi günlük arama SSS ve bilinen sorunlar](log-analytics-log-search-faq.md) ortak sorusunun yanıtlarını ve yükseltme ile ilgili bilinen sorunlar açıklaması.  

## <a name="query-language"></a>Sorgu dili
Günlük analizi yükseltme birincil değişikliği önceki dil önemli geliştirmeler olan yeni bir sorgu dilidir.  

Eski dil ve yeni dil desteğini arasındaki ortak işlemleri doğrudan karşılaştırmasını get [Azure günlük analizi yeni sorgu dili geçiş](log-analytics-log-search-transition.md).  Yeni dil kapsamlı belgeler, şu adreste [Azure günlük analizi sorgu dili](https://docs.loganalytics.io).


## <a name="computer-groups"></a>Bilgisayar grupları
Bir bilgisayar grubu oluşturmak için yöntem değişmediğinden, bu rağmen şimdi benzersiz bir diğer ad her biri için sağlamanız gerekir.  Bilgisayar grupları günlük aramaları dayalı yeni dil sözdizimini kullanmanız gerekir.

Bir günlük aramada bilgisayar gruplarını kullanmak için yeni bir sözdizimi yoktur.  $ComputerGroups işlevi kullanmak yerine, bilgisayar gruplarını her benzersiz bir diğer ada sahip bir işlevi olarak uygulanan sunulmuştur.  Diğer bir işlev gibi günlük arama diğer adı kullanın.  

Ayrıntıları şurada bulunabilir [günlük analizi bilgisayar gruplarında oturum aramaları](log-analytics-computer-groups.md).


## <a name="log-search-portals"></a>Günlük araması portalları
Oluşturmak ve günlük aramaları çalıştırmak için günlük arama portal yanı sıra, önemli ölçüde geliştirilmiş düzenleme özellikleri sağlayan Advanced Analytics portalı artık kullanabilirsiniz.

İki portalları kısa bir açıklamasını şu adresten edinilebilir [oluşturma ve Azure günlük analizi günlük sorgularda düzenleme için portalları](log-analytics-log-search-portals.md).  Yeni Portal bir öğretici yol [Analytics portalı ile çalışmaya başlama](https://docs.loganalytics.io/docs/Learn/Getting-Started/Getting-started-with-the-Analytics-portal).

## <a name="alerts"></a>Uyarılar
Uyarılar aynı yükseltilmiş çalışma alanlarında çalışır, ancak çalıştırdıkları sorgu yeni dilinde yazılmış olmalıdır.  Yükseltmeden önce sahip olduğunuz var olan tüm uyarı kuralları otomatik olarak yeni dil dönüştürülür.  Daha fazla bilgi alabilirsiniz [günlük analizi anlama uyarıları](log-analytics-alerts.md).

Runbook'ları ve Web kancalarını uyarılardan gönderilen yükünün biçimi değişmiştir.  Yeni Yük biçiminde için ayrıntılara [günlük analizi uyarı kurallarında eylemleri eklemek](log-analytics-alerts-actions.md).

## <a name="dashboards"></a>Panolar
[Panolar](log-analytics-dashboards.md) kullanım dışı sürecinde olan.  Pano için çalışma alanınızı yükseltilmişse ancak bu kutucuklar düzenleyemez veya yenilerini ekleyebileceğiniz önce eklenen tüm kutucukları kullanmaya devam edebilirsiniz.  Görünüm Tasarımcısı daha zengin bir özellik kümesinden panolar daha olan özel görünümler oluşturmak için kullanın.

Görünüm Tasarımcısı hakkında ayrıntılar kullanılabilir [günlük analizi özel görünümler oluşturmak için kullanım Görünüm Tasarımcısı](log-analytics-view-designer.md).

## <a name="power-bi"></a>Power BI
Power BI için günlük analizi verileri dışarı aktarma işlemi yükseltilmiş çalışma alanları için değişti, yükseltmeden önce oluşturulan tüm var olan zamanlamalar devre dışı bırakılacaktır.  

Ayrıntıları şurada bulunabilir [Power BI için günlük analizi dışarı veri](log-analytics-powerbi.md).

## <a name="powershell"></a>PowerShell
Get-Powershell'den günlük aramaları çalıştırma AzureRmOperationalInsightsSearchResults yükseltilmiş bir çalışma alanı ile çalışmaz.  Invoke-LogAnalyticsQuery yükseltilmiş bir çalışma alanı ile bu işlevi kullanın.

Ayrıntıları şurada bulunabilir [Azure günlük analizi REST API - PowerShell cmdlet'leri](https://dev.loganalytics.io/documentation/Tools/PowerShell-Cmdlets).

## <a name="log-search-api"></a>Günlük arama API
Günlük arama REST API değişti ve eski sürüm kullanan çözümleri API yeni sürümünü kullanacak şekilde güncelleştirilmesi gerekir.   

API yeni sürüm ayrıntıları şurada bulunabilir [Azure günlük analizi REST API](https://dev.loganalytics.io/).

## <a name="next-steps"></a>Sonraki adımlar

- Bkz: [yeni günlük analizi günlük arama SSS ve bilinen sorunlar](log-analytics-log-search-faq.md) ortak sorusunun yanıtlarını ve yükseltme ile ilgili bilinen sorunlar açıklaması.