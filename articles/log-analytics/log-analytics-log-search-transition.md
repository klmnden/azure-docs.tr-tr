---
title: "Azure günlük analizi sorgu dili kopya sayfası | Microsoft Docs"
description: "Bu makale, zaten eski diline alışık değilseniz, yeni sorgu dili için günlük analizi geçiş hakkında Yardım sağlar."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/06/2017
ms.author: bwren
ms.openlocfilehash: 38cb11befe844178572981abb29fe5439286dbc1
ms.sourcegitcommit: 5735491874429ba19607f5f81cd4823e4d8c8206
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2017
---
# <a name="transitioning-to-azure-log-analytics-new-query-language"></a>Azure günlük analizi yeni sorgu dili geçiş
Günlük analizi yakın zamanda yeni bir sorgu dili uygulanmadı.  Bu makale, zaten eski dili ile tanıdık ve hala yardıma ihtiyacınız varsa, bu dil için günlük analizi için geçiş hakkında Yardım sağlar.

## <a name="language-converter"></a>Dil dönüştürücü

Eski günlük analizi sorgu dili ile sahibiyseniz, yeni dilde aynı sorgu oluşturmak için en kolay yolu çalışma alanınızı dönüştürüldüğünde günlük arama Portalı'nda yüklü dil Dönüştürücüsü kullanmaktır.  Dönüştürücü kullanarak eski sorgu üst metin kutusuna yazarak ve ardından olarak basit **dönüştürme**.  Ya da kopyalama veya sorguyu çalıştırmak ve başka bir yere kullanacak şekilde yapıştırmak için arama düğmesini tıklatabilirsiniz.

![Dil dönüştürücü](media/log-analytics-log-search-upgrade/language-converter.png)


## <a name="cheat-sheet"></a>Kopya kağıdı

Aşağıdaki tabloda Azure günlük analizi yeni ve eski sorgu dilinde arasında eşdeğer komutları için genel sorgular çeşitli arasında bir karşılaştırma sağlanmaktadır.

| Açıklama | Eski | Yeni |
|:--|:--|:--|
| Tüm tabloları arama      | error | "error" (büyük/küçük harfe duyarlı değildir) arama |
| Tablodan veri seçin | Tür olay = |  Olay |
|                        | Tür = olay &#124; Kaynak, olay günlüğüne olay kimliği seçin | Olay &#124; Kaynak, olay günlüğüne, EventID proje |
|                        | Tür = olay &#124; en iyi 100 | Olay &#124; 100 alın |
| Dize karşılaştırması      | Tür olay Computer=srv01.contoso.com =   | Olay &#124; Burada bilgisayar "srv01.contoso.com" == |
|                        | Tür olay Computer=contains("contoso") = | Olay &#124; Burada "contoso" (büyük/küçük harfe duyarlı değildir) bilgisayar içeriyor<br>Olay &#124; Burada bilgisayar contains_cs "Contoso" (büyük küçük harf duyarlı) |
|                        | Tür = olay bilgisayarı RegEx = ("@contoso@")  | Olay &#124; Bilgisayar regex eşleştiği ". *contoso*" |
| Tarih karşılaştırması        | Tür olay TimeGenerated = > şimdi 1DAYS | Olay &#124; Burada TimeGenerated > ago(1d) |
|                        | Tür olay TimeGenerated = > 2017-05-01 TimeGenerated < 2017-05-31 | Olay &#124; Burada TimeGenerated (datetime(2017-05-01) arasında .. DateTime(2017-05-31)) |
| Boolean karşılaştırması     | Tür sinyal IsGatewayInstalled = = false  | Sinyal \| Burada IsGatewayInstalled == false |
| Sırala                   | Tür = olay &#124; Sıralama bilgisayarı asc, olay günlüğüne desc, EventLevelName asc | Olay \| Bilgisayar asc, olay günlüğüne desc, EventLevelName asc göre sıralama |
| Farklı               | Tür = olay &#124; Yinelenenleri kaldırma bilgisayar \| Bilgisayar seçin | Olay &#124; Bilgisayar, olay günlüğüne özetler |
| Sütunları Genişlet         | Türü Perf CounterName = = "% işlemci zamanı" &#124; GENİŞLETME if(map(CounterValue,0,50,0,1),"HIGH","LOW") kullanımı olarak | Perf &#124; CounterName burada "% işlemci zamanı" == \| Kullanımı genişletmek olur = (> 50, "Yüksek", "Düşük" CounterValue) |
| Toplama            | Tür = olay &#124; Bilgisayar bazında sayı olarak ölçü Count() işlevi | Olay &#124; Count özetlemek bilgisayar tarafından count() = |
|                                | Türü Perf ObjectName = işlemci CounterName = = "% işlemci zamanı" &#124; Ölçü avg(CounterValue) bilgisayar aralığı 5 dakika | Perf &#124; Burada ObjectName "İşlemci" ve CounterName == "% işlemci zamanı" &#124; == Bilgisayar, bin (TimeGenerated, 5 dk.) tarafından AVG(CounterValue) özetler |
| Sınır toplama | Tür = olay &#124; Bilgisayar &#124;tarafından ölçü count(); İlk 10 | Olay &#124; AggregatedValue özetlemek bilgisayar &#124; tarafından count() = Sınırı 10 |
| birleşim                  | Tür olay veya türü = Syslog = | UNION olay, Syslog |
| Birleştir                   | Tür = NetworkMonitoring &#124; İç AgentIP katılma (tür = sinyal) ComputerIP | NetworkMonitoring &#124; Birleştirme türü iç = (türünü arama "Sinyal" ==) $left üzerinde. AgentIP $right.ComputerIP == |



## <a name="next-steps"></a>Sonraki adımlar
- Kullanıma bir [sorgu yazmakla ilgili öğretici](https://go.microsoft.com/fwlink/?linkid=856078) yeni sorgu dilini kullanma.
- Başvurmak [sorgu dili başvurusu](https://go.microsoft.com/fwlink/?linkid=856079) tüm komutu, işleçler ve yeni sorgu dili için işlevleri hakkında ayrıntılar için.  
