---
title: "Normal ifadelerde OMS günlük analizi oturum aramaları | Microsoft Docs"
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
ms.date: 08/08/2017
ms.author: bwren
ms.openlocfilehash: 9746170f157ed5065adc953a31687ff18bd73708
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2017
---
# <a name="using-regular-expressions-to-filter-log-searches-in-log-analytics"></a>Günlük analizi günlük filtrelemek için normal ifadeler kullanarak arar

[Oturum aramaları](log-analytics-log-searches.md) günlük analizi depodan bilgi ayıklamak olanak sağlar.  [Filtre ifadelerinde](log-analytics-search-reference.md#filter-expressions) belirli ölçütlere göre arama sonuçlarını filtre olanak sağlar.  **RegEx** anahtar sözcüğü Bu filtre için normal bir ifade belirtmenize olanak verir.  

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

## <a name="characters"></a>Karakterleri
Farklı karakterlere belirtin.

| Karakter | Açıklama | Örnek | Örnek eşleşiyor |
|:--|:--|:--|:--|
| bir | Karakterin bir oluşumu. | Computer=Regex("Srv01.contoso.com") | Srv01.contoso.com |
| . | Herhangi bir tek karakteri. | Computer=Regex("Srv...contoso.com") | Srv01.contoso.com<br>SRV02.contoso.com<br>srv03.contoso.com |
| bir? | Sıfır veya bir oluşumu karakter. | Bilgisayar RegEx = ("srv01?. "contoso.com") | srv0.contoso.com<br>Srv01.contoso.com |
| bir * | Sıfır veya daha fazla karakterin yineleme sayısını. | Computer=Regex("Srv01*.contoso.com") | srv0.contoso.com<br>Srv01.contoso.com<br>srv011.contoso.com<br>srv0111.contoso.com |
| bir + | Bir veya daha fazla karakterin yineleme sayısını. | Computer=Regex("Srv01+.contoso.com") | Srv01.contoso.com<br>srv011.contoso.com<br>srv0111.contoso.com |
| [*abc*] | Köşeli ayraçlar içinde herhangi bir tek karakterle eşleşen | Computer=Regex("srv0[123].contoso.com") | Srv01.contoso.com<br>SRV02.contoso.com<br>srv03.contoso.com |
| [*bir*-*z*] | Tek bir karakter aralığı eşleşmesi.  Birden çok aralık içerebilir. | Computer=Regex("srv0[1-3].contoso.com") | Srv01.contoso.com<br>SRV02.contoso.com<br>srv03.contoso.com |
| [^*abc*] | Köşeli ayraçlar içinde karakterleri hiçbiri | Computer=Regex("srv0[^123].contoso.com") | srv05.contoso.com<br>SRV06.contoso.com<br>srv07.contoso.com |
| [^*bir*-*z*] | Hiçbir karakter aralığı. | Computer=Regex("srv0[^1-3].contoso.com") | srv05.contoso.com<br>SRV06.contoso.com<br>srv07.contoso.com |
| [*n*-*m*] | Sayısal karakter aralığı eşleşir. | Computer=Regex("SRV[01-03].contoso.com") | Srv01.contoso.com<br>SRV02.contoso.com<br>srv03.contoso.com |
| @ | Bir karakter dizesi. | Bilgisayar RegEx = ("srv@.contoso.com") | Srv01.contoso.com<br>SRV02.contoso.com<br>srv03.contoso.com |


## <a name="multiple-occurences-of-character"></a>Birden çok örnek karakter
Belirli bir karakter birden çok tekrarı belirtin.

| Karakter | Açıklama | Örnek | Örnek eşleşiyor |
|:--|:--|:--|:--|
| {n} |  *n*bir karakterin tekrarı. | Computer=Regex("BW-Win-sc01{3}.bwren.Lab") | BW win sc0111.bwren.lab |
| bir {n} |  *n*veya daha fazla karakterin yineleme sayısını. | Computer=Regex("BW-Win-sc01{3,}.bwren.Lab") | BW win sc0111.bwren.lab<br>BW win sc01111.bwren.lab<br>BW win sc011111.bwren.lab<br>BW win sc0111111.bwren.lab |
| {n, m} |  *n*için *m* karakterin yineleme sayısını. | Computer=Regex("BW-Win-sc01{3,5}.bwren.Lab") | BW win sc0111.bwren.lab<br>BW win sc01111.bwren.lab<br>BW win sc011111.bwren.lab |


## <a name="logical-expressions"></a>Mantıksal ifadeler
Birden çok değerlerden birini seçin.

| Karakter | Açıklama | Örnek | Örnek eşleşiyor |
|:--|:--|:--|:--|
| &#124; | Mantıksal OR.  Sonuç döndürür üzerinde ya da ifadeyle eşleşir. | Tür uyarı AlertSeverity = RegEx = ("uyarısı &#124; "Error") | Uyarı<br>Hata |
| & | Mantıksal and  Sonuç döndürür eşleşme üzerinde hem ifadeleri | EventData regex = ("(güvenlik.\* &. \*başarılı. \*)") | Güvenlik denetimi başarılı |


## <a name="literals"></a>Değişmez değerler
Özel karakterler değişmez değer karakterlerine dönüştürmek.  Bu işlevsellik için normal ifadeler gibi sağlamak karakterler içeren?-\*^\[\]{}\(\)+\|. &.

| Karakter | Açıklama | Örnek | Örnek eşleşiyor |
|:--|:--|:--|:--|
| \\ | Bir özel karakter, bir hazır değer dönüştürür. | Status_CF =\\[Hata\\] @<br>Status_CF hata =\\-@ | [Hata] Dosyası bulunamadı.<br>Hata-dosya bulunamadı. |


## <a name="next-steps"></a>Sonraki Adımlar

* İle tanışın [oturum aramaları](log-analytics-log-searches.md) görüntülemek ve günlük analizi deposundaki verileri çözümlemek için.
