---
title: Azure günlük analizi sorgu dili kopya sayfası | Microsoft Docs
description: Bu makale, zaten eski diline alışık değilseniz, yeni sorgu dili için günlük analizi geçiş hakkında Yardım sağlar.
services: operations-management-suite
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: log-analytics
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/28/2017
ms.author: bwren
ms.component: na
ms.openlocfilehash: 7c2158d8e6f64c7c356ba40b3bf56684f00cb8c0
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37133124"
---
# <a name="transitioning-to-azure-log-analytics-new-query-language"></a>Azure günlük analizi yeni sorgu dili geçiş
Günlük analizi yakın zamanda yeni bir sorgu dili uygulanmadı.  Bu makale, zaten eski dili ile tanıdık ve hala yardıma ihtiyacınız varsa, bu dil için günlük analizi için geçiş hakkında Yardım sağlar.

## <a name="resources"></a>Kaynaklar


## <a name="language-converter"></a>Dil dönüştürücü

Eski günlük analizi sorgu dili ile sahibiyseniz, yeni dilde aynı sorgu oluşturmak için en kolay yolu çalışma alanınızı dönüştürüldüğünde günlük arama Portalı'nda yüklü dil Dönüştürücüsü kullanmaktır.  Dönüştürücü kullanarak eski sorgu üst metin kutusuna yazarak ve ardından olarak basit **dönüştürme**.  Ya da kopyalama veya sorguyu çalıştırmak ve başka bir yere kullanacak şekilde yapıştırmak için arama düğmesini tıklatabilirsiniz.

![Dil dönüştürücü](media/log-analytics-log-search-upgrade/language-converter.png)


## <a name="resources"></a>Kaynaklar
[Belgeleri sitesi için günlük analizi sorgu dili](https://docs.loganalytics.io) yeni dil hızlıca gelen gereken tüm kaynakları içerir.  Bu öğreticiler, örnekler ve tam bir dil başvurusu içerir.


## <a name="cheat-sheet"></a>Kopya kağıdı

Aşağıdaki tabloda Azure günlük analizi yeni ve eski sorgu dilinde arasında eşdeğer komutları için genel sorgular çeşitli arasında bir karşılaştırma sağlanmaktadır.

| Açıklama | Eski | yeni |
|:--|:--|:--|
| Tüm tabloları arama      | error | "error" (büyük/küçük harfe duyarlı değildir) arama |
| Tablodan veri seçin | Tür olay = |  Olay |
|                        | Tür olay = &#124; kaynak, olay günlüğüne, EventID seçin | Olay &#124; kaynak, olay günlüğüne, EventID proje |
|                        | Tür olay = &#124; ilk 100 | Olay &#124; 100 alın |
| Dize karşılaştırması      | Tür olay Computer=srv01.contoso.com =   | Olay &#124; burada bilgisayar "srv01.contoso.com" == |
|                        | Tür olay Computer=contains("contoso") = | Olay &#124; , bilgisayar içerdiği "contoso" (büyük/küçük harfe duyarlı değildir)<br>Olay &#124; burada bilgisayar contains_cs "Contoso" (büyük küçük harf duyarlı) |
|                        | Tür = olay bilgisayarı RegEx = ("\@contoso @")  | Olay &#124; bilgisayar regex eşleştiği ". *contoso*" |
| Tarih karşılaştırması        | Tür olay TimeGenerated = > şimdi 1DAYS | Olay &#124; burada TimeGenerated > ago(1d) |
|                        | Tür olay TimeGenerated = > 2017-05-01 TimeGenerated < 2017-05-31 | Olay & #124; Burada TimeGenerated (datetime(2017-05-01) arasında .. DateTime(2017-05-31)) |
| Boolean karşılaştırması     | Tür sinyal IsGatewayInstalled = = false  | Sinyal \| nerede IsGatewayInstalled == false |
| Sırala                   | Tür olay = &#124; sıralama bilgisayarı asc, olay günlüğüne desc, EventLevelName asc | Olay \| bilgisayar asc, olay günlüğüne desc, EventLevelName asc göre sırala |
| Farklı               | Tür olay = &#124; yinelenenleri kaldırma bilgisayar \| bilgisayarı seçin | Olay &#124; EventLog bilgisayar tarafından özetler |
| Sütunları Genişlet         | Türü Perf CounterName = "% işlemci zamanı" = &#124; GENİŞLET if(map(CounterValue,0,50,0,1),"HIGH","LOW") kullanımı olarak | Perf &#124; CounterName burada "% işlemci zamanı" == \| kullanımı genişletmek olur = (> 50, "Yüksek", "Düşük" CounterValue) |
| Toplama            | Tür olay = &#124; bilgisayar bazında sayı olarak count() ölçme | Olay &#124; sayısı özetlemek bilgisayar tarafından count() = |
|                                | Türü Perf ObjectName = işlemci CounterName = "% işlemci zamanı" = &#124; avg(CounterValue) 5 dakika bilgisayar aralığına göre ölçün | Perf &#124; burada ObjectName "İşlemci" ve CounterName == "% işlemci zamanı" == &#124; avg(CounterValue) bilgisayar, bin (TimeGenerated, 5 dk.) tarafından özetler |
| Sınır toplama | Tür olay = &#124; bilgisayar tarafından count() ölçmek &#124; ilk 10 | Olay &#124; AggregatedValue özetlemek bilgisayar tarafından count() = &#124; 10 sınırla |
| birleşim                  | Tür olay veya türü = Syslog = | UNION olay, Syslog |
| Birleştir                   | Türü NetworkMonitoring = &#124; iç AgentIP katılma (tür = sinyal) ComputerIP | NetworkMonitoring &#124; birleştirme türü iç = (türünü arama "Sinyal" ==) $left üzerinde. AgentIP $right.ComputerIP == |



## <a name="next-steps"></a>Sonraki adımlar
- Kullanıma bir [sorgu yazmakla ilgili öğretici](https://go.microsoft.com/fwlink/?linkid=856078) yeni sorgu dilini kullanma.
- Başvurmak [sorgu dili başvurusu](https://go.microsoft.com/fwlink/?linkid=856079) tüm komutu, işleçler ve yeni sorgu dili için işlevleri hakkında ayrıntılar için.  
