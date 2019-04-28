---
title: Azure İzleyici günlük sorguları dizelerle çalışma | Microsoft Docs
description: Düzenleme, karşılaştırın, içinde arama ve çeşitli diğer işlemleri dizeler Azure İzleyici günlük sorguları gerçekleştirmek nasıl açıklar.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/16/2018
ms.author: bwren
ms.openlocfilehash: 4b2763629a3036551cb3d362e609c72737436f4a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61424712"
---
# <a name="work-with-strings-in-azure-monitor-log-queries"></a>Azure İzleyici günlük sorguları dizelerle çalışma


> [!NOTE]
> Tamamlamanız gereken [Azure İzleyici Log Analytics ile çalışmaya başlama](get-started-portal.md) ve [Azure İzleyici günlük sorguları ile çalışmaya başlama](get-started-queries.md) Bu öğreticiyi tamamlamadan önce.

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]

Bu makalede, Düzenle, karşılaştırın, içinde arama ve çeşitli dizelerle ilgili diğer işlemleri gerçekleştirmek açıklar.

Bir dizedeki her karakterin konumuna göre bir dizin numarası var. Dizin 0 ilk karakter, 1 ve bu nedenle bir sonraki karakteri. Aşağıdaki bölümlerde gösterildiği gibi farklı dize işlevleri dizin numaralarını kullanın. Çok sayıda Aşağıdaki örnekler **yazdırma** belirli bir veri kaynağına kullanmadan dize düzenlemesi göstermek için komutu.


## <a name="strings-and-escaping-them"></a>Dizeler ve kaçış
Dize değerleri, tek veya çift tırnaklı karakterle ya da ile sarılır. Ters eğik çizgi (\) , \t \n yeni satır için sekmesinde gibi izleyen karaktere karakterleri kaçış için kullanılır ve \" tırnak karakteri.

```Kusto
print "this is a 'string' literal in double \" quotes"
```

Önlemek için "\\"çıkış karakteri olarak yaratmasını, Ekle"\@" dize öneki olarak:

```Kusto
print @"C:\backslash\not\escaped\with @ prefix"
```


## <a name="string-comparisons"></a>Dize karşılaştırmaları

İşleç       |Açıklama                         |Büyük küçük harfe duyarlı|Örnek (verir `true`)
---------------|------------------------------------|--------------|-----------------------
`==`           |Eşittir                              |Evet           |`"aBc" == "aBc"`
`!=`           |Eşit değildir                          |Evet           |`"abc" != "ABC"`
`=~`           |Eşittir                              |Hayır            |`"abc" =~ "ABC"`
`!~`           |Eşit değildir                          |Hayır            |`"aBc" !~ "xyz"`
`has`          |Sağ taraftaki tam bir sol taraftaki terimdir |Hayır|`"North America" has "america"`
`!has`         |Sağ taraftaki tam bir sol taraftaki terimi değil       |Hayır            |`"North America" !has "amer"` 
`has_cs`       |Sağ taraftaki tam bir sol taraftaki terimdir |Evet|`"North America" has_cs "America"`
`!has_cs`      |Sağ taraftaki tam bir sol taraftaki terimi değil       |Evet            |`"North America" !has_cs "amer"` 
`hasprefix`    |Sağ taraftaki terimi, sol taraftaki önekidir         |Hayır            |`"North America" hasprefix "ame"`
`!hasprefix`   |Sağ taraftaki sol taraftaki terimi ön eki değil     |Hayır            |`"North America" !hasprefix "mer"` 
`hasprefix_cs`    |Sağ taraftaki terimi, sol taraftaki önekidir         |Evet            |`"North America" hasprefix_cs "Ame"`
`!hasprefix_cs`   |Sağ taraftaki sol taraftaki terimi ön eki değil     |Evet            |`"North America" !hasprefix_cs "CA"` 
`hassuffix`    |Sağ taraftaki olan sol taraftaki bir terim soneki         |Hayır            |`"North America" hassuffix "ica"`
`!hassuffix`   |Sol taraftaki bir terim soneki sağ taraftaki değil     |Hayır            |`"North America" !hassuffix "americ"`
`hassuffix_cs`    |Sağ taraftaki olan sol taraftaki bir terim soneki         |Evet            |`"North America" hassuffix_cs "ica"`
`!hassuffix_cs`   |Sol taraftaki bir terim soneki sağ taraftaki değil     |Evet            |`"North America" !hassuffix_cs "icA"`
`contains`     |Sağ taraftaki bir alt dizi sol taraftaki gerçekleşir.  |Hayır            |`"FabriKam" contains "BRik"`
`!contains`    |Sağ taraftaki sol taraftaki içinde oluşmaz.           |Hayır            |`"Fabrikam" !contains "xyz"`
`contains_cs`   |Sağ taraftaki bir alt dizi sol taraftaki gerçekleşir.  |Evet           |`"FabriKam" contains_cs "Kam"`
`!contains_cs`  |Sağ taraftaki sol taraftaki içinde oluşmaz.           |Evet           |`"Fabrikam" !contains_cs "Kam"`
`startswith`   |Sağ taraftaki bir ilk alt sol taraftaki olduğu|Hayır            |`"Fabrikam" startswith "fab"`
`!startswith`  |Sağ taraftaki bir ilk alt sol taraftaki değil|Hayır        |`"Fabrikam" !startswith "kam"`
`startswith_cs`   |Sağ taraftaki bir ilk alt sol taraftaki olduğu|Evet            |`"Fabrikam" startswith_cs "Fab"`
`!startswith_cs`  |Sağ taraftaki bir ilk alt sol taraftaki değil|Evet        |`"Fabrikam" !startswith_cs "fab"`
`endswith`     |Sağ taraftaki bir kapanış alt sol taraftaki olduğu|Hayır             |`"Fabrikam" endswith "Kam"`
`!endswith`    |Sağ taraftaki bir kapanış alt sol taraftaki değil|Hayır         |`"Fabrikam" !endswith "brik"`
`endswith_cs`     |Sağ taraftaki bir kapanış alt sol taraftaki olduğu|Evet             |`"Fabrikam" endswith "Kam"`
`!endswith_cs`    |Sağ taraftaki bir kapanış alt sol taraftaki değil|Evet         |`"Fabrikam" !endswith "brik"`
`matches regex`|Sol taraftaki bir eşleşme için sağ taraftaki içerir        |Evet           |`"Fabrikam" matches regex "b.*k"`
`in`           |Öğelerden birine eşit       |Evet           |`"abc" in ("123", "345", "abc")`
`!in`          |Öğeleri birine eşit değildir   |Evet           |`"bca" !in ("123", "345", "abc")`


## <a name="countof"></a>countof

Bir alt dizenin bir dize içinde yineleme sayar. Düz dizeleri Eşleştir veya regex kullan kullanabilirsiniz. Regex eşleşme yoksa ancak düz dize eşleşmeleri çakışabilir.

### <a name="syntax"></a>Sözdizimi
```
countof(text, search [, kind])
```

### <a name="arguments"></a>Bağımsız Değişkenler:
- `text` -Giriş dizesi 
- `search` -Düz dize veya içindeki metnin eşleştirmek için normal bir ifade.
- `kind` - _Normal_ | _regex_ (varsayılan: normal).

### <a name="returns"></a>Şunu döndürür:

Arama dizesi kapsayıcıda eşleştirilebildiği sayısı. Düz dize eşleşmeleri desteklerken Regex eşleşme çakışabilir.

### <a name="examples"></a>Örnekler

#### <a name="plain-string-matches"></a>Düz dize eşleşmeleri

```Kusto
print countof("The cat sat on the mat", "at");  //result: 3
print countof("aaa", "a");  //result: 3
print countof("aaaa", "aa");  //result: 3 (not 2!)
print countof("ababa", "ab", "normal");  //result: 2
print countof("ababa", "aba");  //result: 2
```

#### <a name="regex-matches"></a>Normal ifade ile eşleşir

```Kusto
print countof("The cat sat on the mat", @"\b.at\b", "regex");  //result: 3
print countof("ababa", "aba", "regex");  //result: 1
print countof("abcabc", "a.c", "regex");  // result: 2
```


## <a name="extract"></a>Extract

Normal bir ifade için bir eşleşme, belirli bir dizeden alır. İsteğe bağlı olarak ayrıca dönüştürür ayıklanan belirtilen tür alt dize.

### <a name="syntax"></a>Sözdizimi

```Kusto
extract(regex, captureGroup, text [, typeLiteral])
```

### <a name="arguments"></a>Bağımsız Değişkenler

- `regex` -Bir normal ifade.
- `captureGroup` -Bir pozitif tamsayı ayıklamak için yakalama grubu belirten sabit değer. Tüm eşleşmeyi, 1, 2 veya daha fazla sonraki parantezler için normal ifadede ilk '(' parantez')' tarafından eşleştirilen değeri için 0.
- `text` -Aranacak bir dize.
- `typeLiteral` -Bir isteğe bağlı tür değişmez değeri (örneğin, typeof(long)). Sağlanırsa, ve çıkartılan alt dizenin bu türe dönüştürülür.

### <a name="returns"></a>Şunu döndürür:
Belirtilen yakalama grubu captureGroup karşı eşleştirilen alt dizenin typeLiteral için isteğe bağlı olarak dönüştürülür.
Eşleşme yok, veya tür dönüştürme başarısız olursa null döndürür.

### <a name="examples"></a>Örnekler

Aşağıdaki örnekte, son sekizliğini ayıklar *Computerıp* sinyal kaydı:
```Kusto
Heartbeat
| where ComputerIP != "" 
| take 1
| project ComputerIP, last_octet=extract("([0-9]*$)", 1, ComputerIP) 
```

Aşağıdaki örnek son sekizli ayıklar, kendisine bıraktığı bir *gerçek* (sayı) yazın ve İleri IP değeri hesaplar
```Kusto
Heartbeat
| where ComputerIP != "" 
| take 1
| extend last_octet=extract("([0-9]*$)", 1, ComputerIP, typeof(real)) 
| extend next_ip=(last_octet+1)%255
| project ComputerIP, last_octet, next_ip
```

Dize aşağıdaki örnekte *izleme* "Süresi" için bir tanım aranır. Eşleşme türüne dönüştürülür *gerçek* ve bir zaman sabit tarafından çarpılan (1 s) *timespan türü için süre bıraktığı*.
```Kusto
let Trace="A=12, B=34, Duration=567, ...";
print Duration = extract("Duration=([0-9.]+)", 1, Trace, typeof(real));  //result: 567
print Duration_seconds =  extract("Duration=([0-9.]+)", 1, Trace, typeof(real)) * time(1s);  //result: 00:09:27
```


## <a name="isempty-isnotempty-notempty"></a>IsEmpty, isnotempty, notempty

- *IsEmpty* null ya da bağımsız değişken boş bir dize ise true verir (Ayrıca bkz: *IsNull*).
- *isnotempty* bağımsız değişken boş bir dize veya boş değilse true değerini döndürür (Ayrıca bkz: *isnotnull*). diğer ad: *notempty*.

### <a name="syntax"></a>Sözdizimi

```Kusto
isempty(value)
isnotempty(value)
```

### <a name="examples"></a>Örnekler

```Kusto
print isempty("");  // result: true

print isempty("0");  // result: false

print isempty(0);  // result: false

print isempty(5);  // result: false

Heartbeat | where isnotempty(ComputerIP) | take 1  // return 1 Heartbeat record in which ComputerIP isn't empty
```


## <a name="parseurl"></a>parseurl

Bir URL parçasına (protokol, konak, bağlantı noktası, vb.) ayırır ve olarak dizeleri parçalarını içeren bir sözlük nesnesi döndürür.

### <a name="syntax"></a>Sözdizimi

```
parseurl(urlstring)
```

### <a name="examples"></a>Örnekler

```Kusto
print parseurl("http://user:pass@contoso.com/icecream/buy.aspx?a=1&b=2#tag")
```

Sonucu şöyle olacaktır:
```
{
    "Scheme" : "http",
    "Host" : "contoso.com",
    "Port" : "80",
    "Path" : "/icecream/buy.aspx",
    "Username" : "user",
    "Password" : "pass",
    "Query Parameters" : {"a":"1","b":"2"},
    "Fragment" : "tag"
}
```


## <a name="replace"></a>Değiştir

Tüm normal ifade eşleşen başka bir dizeyle değiştirir. 

### <a name="syntax"></a>Sözdizimi

```
replace(regex, rewrite, input_text)
```

### <a name="arguments"></a>Bağımsız Değişkenler

- `regex` -Tarafından eşleştirilecek normal ifade. Bu, yakalama gruplarının '('parantez')' içerebilir.
- `rewrite` -Değiştirme için normal ifade ile eşleşen normal ifade yapılan herhangi bir eşleşme. Başvurmak için tam eşleşenlerin, ilk yakalama grubunun \1, vb. sonraki yakalama grupları için bir \2 \0 kullanın.
- `input_text` -İçinde arama yapmak giriş dizesi.

### <a name="returns"></a>Şunu döndürür:
Yeniden yazma değerlendirmelerde regex tüm eşleşmeleri değiştirdikten sonra metin. Eşleşme çakışmadığından.

### <a name="examples"></a>Örnekler

```Kusto
SecurityEvent
| take 1
| project Activity 
| extend replaced = replace(@"(\d+) -", @"Activity ID \1: ", Activity) 
```

Aşağıdaki sonuçları olabilir:

Etkinlik                                        |değiştirildi
------------------------------------------------|----------------------------------------------------------
4663 - bir nesneye erişmek için bir girişimde bulunuldu  |Etkinlik Kimliği 4663: Bir nesneye erişmek için girişimde bulunuldu.


## <a name="split"></a>split

Belirtilen sınırlayıcıya göre belirli bir dizeyi böler ve sonuçta elde edilen alt dizelerin dizisini döndürür.

### <a name="syntax"></a>Sözdizimi
```
split(source, delimiter [, requestedIndex])
```

### <a name="arguments"></a>Bağımsız Değişkenler:

- `source` -Belirtilen sınırlayıcıya göre bölüneceği dize.
- `delimiter` -Kaynak dizeyi bölmek için kullanılan sınırlayıcı.
- `requestedIndex` İsteğe bağlı sıfır tabanlı dizini. Sağlanırsa, döndürülen dize dizisi bu öğe yalnızca tutacaktır (varsa var).


### <a name="examples"></a>Örnekler

```Kusto
print split("aaa_bbb_ccc", "_");    // result: ["aaa","bbb","ccc"]
print split("aa_bb", "_");          // result: ["aa","bb"]
print split("aaa_bbb_ccc", "_", 1); // result: ["bbb"]
print split("", "_");               // result: [""]
print split("a__b", "_");           // result: ["a","","b"]
print split("aabbcc", "bb");        // result: ["aa","cc"]
```

## <a name="strcat"></a>strcat

Dize bağımsız değişkenleri (1-16 destekler bağımsız değişkenler) art arda ekler.

### <a name="syntax"></a>Sözdizimi
```
strcat("string1", "string2", "string3")
```

### <a name="examples"></a>Örnekler
```Kusto
print strcat("hello", " ", "world") // result: "hello world"
```


## <a name="strlen"></a>strlen

Bir dizenin uzunluğunu döndürür.

### <a name="syntax"></a>Sözdizimi
```
strlen("text_to_evaluate")
```

### <a name="examples"></a>Örnekler
```Kusto
print strlen("hello")   // result: 5
```


## <a name="substring"></a>alt dize

Belirtilen kaynak dizeden belirtilen dizinden başlayarak, bir alt dizeyi ayıklar. İsteğe bağlı olarak, istenen alt dizenin uzunluğu belirtilebilir.

### <a name="syntax"></a>Sözdizimi
```
substring(source, startingIndex [, length])
```

### <a name="arguments"></a>Bağımsız Değişkenler:

- `source` -Alt dizenin alındığı kaynak dizesi.
- `startingIndex` -Sıfır tabanlı başlangıç karakteri konumunu istenen alt dize.
- `length` -Döndürülen substring istenen uzunluğunu belirtmek üzere kullanılan isteğe bağlı bir parametre.

### <a name="examples"></a>Örnekler
```Kusto
print substring("abcdefg", 1, 2);   // result: "bc"
print substring("123456", 1);       // result: "23456"
print substring("123456", 2, 2);    // result: "34"
print substring("ABCD", 0, 2);  // result: "AB"
```


## <a name="tolower-toupper"></a>tolower, toupper

Belirli bir dize, tüm küçük veya büyük harfe dönüştürür.

### <a name="syntax"></a>Sözdizimi
```
tolower("value")
toupper("value")
```

### <a name="examples"></a>Örnekler
```Kusto
print tolower("HELLO"); // result: "hello"
print toupper("hello"); // result: "HELLO"
```



## <a name="next-steps"></a>Sonraki adımlar
Gelişmiş öğreticiler ile devam edin:
* [Toplama işlevleri](aggregations.md)
* [Gelişmiş toplamaları](advanced-aggregations.md)
* [Grafikleri ve diyagramları](charts.md)
* [JSON ve veri yapıları ile çalışma](json-data-structures.md)
* [Gelişmiş sorgu yazma](advanced-query-writing.md)
* [-Çözümleme arası birleştirmeler](joins.md)
