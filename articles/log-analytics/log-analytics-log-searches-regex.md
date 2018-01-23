---
title: "Normal ifadelerde Azure günlük analizi oturum aramaları | Microsoft Docs"
description: "Sonuçları normal ifadeye göre filtre uygulamak için günlük analizi günlük aramaları RegEx anahtar sözcüğü kullanabilirsiniz.  Bu makalede, bu ifadelerle çeşitli örnekler için sözdizimi sağlar."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/18/2018
ms.author: bwren
ms.openlocfilehash: 8915e0e35951871ff10fd84453d55bd5102e97df
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2018
---
# <a name="using-regular-expressions-to-filter-log-searches-in-log-analytics"></a>Günlük analizi günlük filtrelemek için normal ifadeler kullanarak arar

>[!NOTE]
> Bu makalede eski sorgu dili günlük analizi kullanarak normal ifadeler açıklanmaktadır.  Çalışma alanınız için yükseltildiyse [yeni günlük analizi sorgu dili](log-analytics-log-search-upgrade.md), için başvurmalıdır sonra [dil belgelerinde normal ifadeler](https://docs.loganalytics.io/docs/Language-Reference/References/Regular-Expressions-syntax).


[Oturum aramaları](log-analytics-log-searches.md) günlük analizi çalışma alanından bilgi ayıklamak olanak sağlar.  [Filtre ifadelerinde](log-analytics-search-reference.md#filter-expressions) belirli ölçütlere göre arama sonuçlarını filtre olanak sağlar.  **RegEx** anahtar sözcüğü Bu filtre için normal bir ifade belirtmenize olanak verir.  

Bu makalede, günlük analizi tarafından kullanılan normal ifade sözdizimi hakkında ayrıntılar sağlar.

> [!NOTE]
> Bu gibi durumlarda, RegEx yalnızca aranabilir alanlara ile kullanabilirsiniz.  Aranabilir alanları hakkında daha fazla bilgi için bkz: **alan türleri** içinde [Bul günlük aramaları günlük analizi kullanarak verileri](log-analytics-log-searches.md#use-additional-filters).


## <a name="regex-keyword"></a>RegEx anahtar sözcüğü

Kullanmak için aşağıdaki sözdizimini kullanın **RegEx** günlük arama anahtar sözcük.  Bu makalede diğer bölümlerinde, normal ifade sözdizimini belirlemek için kullanabilirsiniz.

    field:Regex("Regular Expression")
    field=Regex("Regular Expression")

Örneğin, normal bir ifade türü olan uyarı kayıtları döndürmek için kullanılacak *uyarı* veya *hata*, aşağıdaki günlük arama kullanırsınız.

    Type=Alert AlertSeverity=RegEx("Warning|Error")

## <a name="partial-matches"></a>Kısmi eşleşmeler
Normal ifade özelliğinin tüm metni eşleşmesi gerektiğini unutmayın.  Kısmi eşleşmeler herhangi bir kayıt döndürmez.  Örneğin, aşağıdaki günlük arama srv01.contoso.com adlı bir bilgisayardan kayıtları döndürmek çalışıyorsanız misiniz **değil** herhangi bir kayıt döndürür.

    Computer=RegEx("srv..")

Ad yalnızca ilk bölümü normal ifadeyle eşleşen olmasıdır.  Adın tamamını eşleştiğinden aşağıdaki iki günlük aramaları bu bilgisayardan kayıtları döndürecektir.

    Computer=RegEx("srv..@")
    Computer=RegEx("srv...contoso.com")

## <a name="characters"></a>Karakterler
Farklı karakterlere belirtin.

| Karakter | Açıklama | Örnek | Örnek eşleşiyor |
|:--|:--|:--|:--|
| a | Karakterin bir oluşumu. | Computer=RegEx("srv01.contoso.com") | srv01.contoso.com |
| . | Herhangi bir tek karakteri. | Computer=RegEx("srv...contoso.com") | srv01.contoso.com<br>srv02.contoso.com<br>srv03.contoso.com |
| bir? | Sıfır veya bir oluşumu karakter. | Computer=RegEx("srv01?.contoso.com") | srv0.contoso.com<br>srv01.contoso.com |
| a* | Sıfır veya daha fazla karakterin yineleme sayısını. | Computer=RegEx("srv01*.contoso.com") | srv0.contoso.com<br>srv01.contoso.com<br>srv011.contoso.com<br>srv0111.contoso.com |
| a+ | Bir veya daha fazla karakterin yineleme sayısını. | Computer=RegEx("srv01+.contoso.com") | srv01.contoso.com<br>srv011.contoso.com<br>srv0111.contoso.com |
| [*abc*] | Köşeli ayraçlar içinde herhangi bir tek karakterle eşleşen | Computer=RegEx("srv0[123].contoso.com") | srv01.contoso.com<br>srv02.contoso.com<br>srv03.contoso.com |
| [*a*-*z*] | Tek bir karakter aralığı eşleşmesi.  Birden çok aralık içerebilir. | Computer=RegEx("srv0[1-3].contoso.com") | srv01.contoso.com<br>srv02.contoso.com<br>srv03.contoso.com |
| [^*abc*] | Köşeli ayraçlar içinde karakterleri hiçbiri | Computer=RegEx("srv0[^123].contoso.com") | srv05.contoso.com<br>srv06.contoso.com<br>srv07.contoso.com |
| [^*a*-*z*] | Hiçbir karakter aralığı. | Computer=RegEx("srv0[^1-3].contoso.com") | srv05.contoso.com<br>srv06.contoso.com<br>srv07.contoso.com |
| [*n*-*m*] | Sayısal karakter aralığı eşleşir. | Computer=RegEx("srv[01-03].contoso.com") | srv01.contoso.com<br>srv02.contoso.com<br>srv03.contoso.com |
| @ | Bir karakter dizesi. | Computer=RegEx("srv@.contoso.com") | srv01.contoso.com<br>srv02.contoso.com<br>srv03.contoso.com |


## <a name="multiple-occurences-of-character"></a>Birden çok örnek karakter
Belirli bir karakter birden çok tekrarı belirtin.

| Karakter | Açıklama | Örnek | Örnek eşleşiyor |
|:--|:--|:--|:--|
| a{n} |  *n*bir karakterin tekrarı. | Computer=RegEx("bw-win-sc01{3}.bwren.lab") | bw-win-sc0111.bwren.lab |
| bir {n} |  *n*veya daha fazla karakterin yineleme sayısını. | Computer=RegEx("bw-win-sc01{3,}.bwren.lab") | bw-win-sc0111.bwren.lab<br>bw-win-sc01111.bwren.lab<br>bw-win-sc011111.bwren.lab<br>bw-win-sc0111111.bwren.lab |
| {n, m} |  *n*için *m* karakterin yineleme sayısını. | Computer=RegEx("bw-win-sc01{3,5}.bwren.lab") | bw-win-sc0111.bwren.lab<br>bw-win-sc01111.bwren.lab<br>bw-win-sc011111.bwren.lab |


## <a name="logical-expressions"></a>Mantıksal ifadeler
Birden çok değerlerden birini seçin.

| Karakter | Açıklama | Örnek | Örnek eşleşiyor |
|:--|:--|:--|:--|
| &#124; | Mantıksal OR.  Sonuç döndürür üzerinde ya da ifadeyle eşleşir. | Tür uyarı AlertSeverity = RegEx = ("uyarısı &#124; "Error") | Uyarı<br>Hata |
| & | Mantıksal and  Sonuç döndürür eşleşme üzerinde hem ifadeleri | EventData=regex("(Security.\*&.\*success.\*)") | Güvenlik denetimi başarılı |


## <a name="literals"></a>Değişmez değerler
Özel karakterler değişmez değer karakterlerine dönüştürmek.  Bu işlevsellik için normal ifadeler gibi sağlamak karakterler içeren?-\*^\[\]{}\(\)+\|. &.

| Karakter | Açıklama | Örnek | Örnek eşleşiyor |
|:--|:--|:--|:--|
| \\ | Bir özel karakter, bir hazır değer dönüştürür. | Status_CF =\\[Hata\\] @<br>Status_CF hata =\\-@ | [Hata] Dosyası bulunamadı.<br>Hata-dosya bulunamadı. |


## <a name="next-steps"></a>Sonraki Adımlar

* İle tanışın [oturum aramaları](log-analytics-log-searches.md) görüntülemek ve günlük analizi çalışma alanındaki verileri çözümlemek için.
