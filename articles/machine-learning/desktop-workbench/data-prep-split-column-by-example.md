---
title: Bölünmüş sütuna göre örnek Azure Machine Learning çalışma ekranı kullanarak dönüştürme
description: Başvuru belge 'Örneğe göre Bölünmüş Sütun' dönüştürme için
services: machine-learning
author: ranvijaykumar
ms.author: ranku
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc, reference
ms.topic: article
ms.date: 09/14/2017
ms.openlocfilehash: 497c1725fc4554792add11c0ec069d1628a89fbd
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="split-column-by-example-transformation"></a>Bölünmüş sütuna göre örnek dönüştürme
Bu dönüştürme kullanıcı girişi gerektirmeden anlamlı sınırlarındaki bir sütunun içerik predictively ayırır. Bölünmüş algoritmasını sütunun içeriğini çözümledikten sonra sınırları seçer. Bu sınırları tarafından tanımlanan
* Bir sabit ayırıcısı
* Özellikle bağlamı, görünen birden çok, rasgele sınırlayıcıları veya
* Veri desenleri veya belirli varlık türleri

Kullanıcılar ayrıca sınırlayıcı belirtin açabilecekleri tarafından istenen bölme örnekleri sağlayın Gelişmiş modda bölme davranışını kontrol edebilirsiniz.

Teorik olarak bölme işlemleri aynı zamanda bir dizi kullanarak çalışma ekranı gerçekleştirilebilir *türetilen sütun örneğe göre* dönüştürür. Birkaç sütun varsa, ancak bu örnek tarafından yaklaşımı ayrı ayrı bile kullanarak her türetme çok zaman alabilir. Tahmine dayalı bölünmüş kullanıcının sütunların her biri için örnekler sağlamaya gerek olmadan kolayca bölme sağlar. 

## <a name="how-to-perform-this-transformation"></a>Bu dönüştürme gerçekleştirme

1. Bölmek istediğiniz sütunu seçin. 
2. Seçin **örneğe göre bölünmüş sütun** gelen **dönüştüren** menüsü. Veya, seçili sütun başlığına sağ tıklatın ve seçin **örneğe göre bölünmüş sütun** ve bağlam menüsünden. Dönüştürme Düzenleyicisi açılır ve yeni sütunlar yanındaki seçilen sütuna eklenir. Bu noktada, çalışma ekranı giriş sütununu çözümler, bölünmüş sınırları belirler ve kılavuzunda gösterilen sütununu bölmek için bir program ölçekli harfleri sentezler. Sütundaki tüm satırları karşı birleştirilen program yürütülür. Sınırlayıcılar, varsa, Nihai sonuç hariç tutulur.
3. Tıklatabilirsiniz **Gelişmiş mod** bölme dönüşümünü üzerinde daha hassas denetim için. 
4. ' I tıklatın ve çıkış gözden **Tamam** dönüştürme kabul etmek için. 

Tüm satırlar sonuç sütunların aynı sayıda üretmek için dönüştürme amaçlar. Herhangi bir satırın belirlenen sınırları bölünemez, üreten *null* varsayılan olarak tüm sütunlar için. Bu davranış değiştirilebilir **Gelişmiş mod**.

### <a name="transform-editor-advanced-mode"></a>Düzenleyici Dönüştür: Gelişmiş mod
**Gelişmiş mod** sütunları bölme için daha zengin bir deneyim sağlar. 

Seçme **sınırlayıcı sütunları tutun** nihai sonucu sınırlayıcıları içerir. Sınırlayıcılar varsayılan olarak dışarıda bırakılır.

Belirtme **sınırlayıcıları** otomatik sınırlayıcı seçme mantığı geçersiz kılar. Her satırın birden çok ayırıcısı olarak belirtilen **sınırlayıcıları**. Bu karakterleri sütununu bölmek için sınırlayıcı olarak kullanılır.

Bazı durumlarda, belirlenen sınırlar üretir farklı sayıda sütuna başkalarının çoğunluğu daha değerine bölme. Bu durumlarda, **doldurun yönü** içinde sütun doldurulmalıdır sırası karar vermek için kullanılır.

Tıklayarak **önerilen örnekler** hangi kullanıcı için temsili Satırları bölme örneği sağlayacağını görüntüler. Kullanıcı tıklatabilirsiniz **yukarı** satır bir örnek olarak yükseltmek için önerilen satır sağındaki oku. 

Kullanıcı **Sütun Sil** veya **eklenecek yeni sütunlar** başlığında sağ tıklanarak **örnekler tablo**.

Kullanıcı kopyalayabilir ve Yapıştır bir hücreden diğerine bölünmüş örneği sağlamak için değerleri.

Kullanıcı arasında geçiş yapmak **temel mod** ve **Gelişmiş mod** bağlantıları dönüştürme Düzenleyicisi'ni tıklatarak.

### <a name="transform-editor-send-feedback"></a>Düzenleyici Dönüştür: geri bildirim gönder

Tıklayarak **geri bildirim gönder** bağlantı açar **geri bildirim** parametre seçimleri ve örnekler kullanıcı ile önceden doldurulmaz açıklamaları kutusuyla iletişim sağlanan. Kullanıcı, açıklama kutusunun içeriğini gözden geçirin ve sorunu anlamasına yardımcı olmak için daha fazla ayrıntı sağlar. Kullanıcı, verileri Microsoft ile paylaşmak istiyorsanız değil, kullanıcı önceden doldurulmuş örnek veri tıklatmadan önce silmelisiniz **geri bildirim gönder** düğmesi. 


### <a name="editing-an-existing-transformation"></a>Varolan bir dönüşüm düzenleme

Var olan bir kullanıcı düzenleyebilir **tarafından bölünmüş sütun örneği** dönüştürme seçerek **Düzenle** dönüştürme adımının seçeneği. Tıklayarak **Düzenle** dönüştürme Düzenleyicisi'nde açar **Gelişmiş mod**, ve dönüştürme oluşturma sırasında sağlanan tüm örnekler gösterilmektedir.

## <a name="examples-of-splitting-on-a-fixed-single-character-delimiter"></a>Bir sabit, tek karakter ayırıcısı bölme örnekleri

Tek bir sabit sınırlayıcı gibi bir CSV biçiminde bir virgülle ayrılmış veri alanları için yaygın bir durumdur. Bölünmüş dönüştürme bu sınırlayıcılar otomatik olarak Infer dener. Örneğin, aşağıdaki senaryoda, otomatik olarak algılar "." sınırlayıcı olarak.

### <a name="splitting-ip-addresses"></a>Bölme IP adresleri

Birinci sütundaki değerleri predictively dört sütunlara bölünür.

|IP|IP_1|IP_2|IP_3|IP_4|
|:-----|:-----|:-----|:-----|:-----|
|192.168.0.102|192|168|0|102|
|192.138.0.101|192|138|0|101|
|192.168.0.102|192|168|0|102|
|192.158.1.202|192|158|1|202|
|192.168.0.102|192|168|0|102|
|192.169.1.102|192|169|1|102|

## <a name="examples-of-splitting-on-multiple-delimiters-within-particular-contexts"></a>Birden çok belirli bağlamları sınırlayıcıları bölme örnekleri

Kullanıcı verilerinin farklı alanları ayırmak birçok farklı sınırlayıcıları içerebilir. Ayrıca, yalnızca bazı dizesinin yineleme sayısını sınırlandırma sınırlayıcı ancak tüm olabilir. Örneğin, aşağıdaki durumda gerekli sınırlayıcıları kümesidir "-",","ve":" istenen çıkış üretmek için. Bununla birlikte, her örneğini ":" biz zaman bölme ancak tek bir sütunda tutmak istemiyorsanız bu yana bir bölme noktası olması gerekir. Bölünmüş dönüştürme içinde sınırlayıcısı olası olayı yerine giriş verilerini, gerçekleşme bağlamları sınırlayıcıları oluşturur. Dönüştürme, tarih ve saat gibi ortak veri türleri, ayrıca farkındadır.   

### <a name="splitting-store-opening-timings"></a>Mağaza açılış zamanlamaları bölme

Aşağıdaki değerleri *zamanlamaları* sütunu predictively altındaki tabloda gösterilen dokuz sütunlara bölme.

|Zamanlamaları|
|:-----|
|Pazartesi - Cuma: 7: 00'da - 6:00 pm Cumartesi: 09:00:00 - 17: 00'dan, Pazar: kapalı|
|Pazartesi - Cuma: 09:00:00 - 6:00 pm Cumartesi: 04:00 ' - 4:00 pm, Pazar: kapalı|
|Pazartesi - Cuma: 8:30:00 - 7:00 pm Cumartesi: 03: 00'da - 2:30 şöyle, Pazar: kapalı|
|Pazartesi - Cuma: 8:00:00 - 6:00 pm Cumartesi: 2: 00'da - 3:00 pm Pazar: kapalı|
|Pazartesi - Cuma: 4: 00'da - 7:00 pm Cumartesi: 09:00:00 - 4:00 pm, Pazar: kapalı|
|Pazartesi - Cuma: 8:30:00 - 4:30 pm Cumartesi: 09:00:00 - 17: 00'dan, Pazar: kapalı|
|Pazartesi - Cuma: 5: 30'da - 6:30 pm Cumartesi: 5: 00'da - 4:00 pm, Pazar: kapalı|
|Pazartesi - Cuma: 8:30:00 - 8:30 pm Cumartesi: 6:00 am - 5:00 pm, Pazar: kapalı|
|Pazartesi - Cuma: 8:00:00 - 9:00 pm Cumartesi: 09:00:00 - 8:00 pm Pazar: kapalı|
|Pazartesi - Cuma: 10:00:00 - 9:30 pm Cumartesi: 09:30:00 - 3:00 pm Pazar: kapalı|

|Timings_1|Timings_2|Timings_3|Timings_4|Timings_5|Timings_6|Timings_7|Timings_8|Timings_9|
|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|
|Pazartesi|Cuma|7: 00'da|6:00 pm|Cumartesi|09.00|5:00 pm|Pazar|Kapalı|
|Pazartesi|Cuma|09.00|6:00 pm|Cumartesi|4: 00'da|4:00 pm|Pazar|Kapalı|
|Pazartesi|Cuma|8:30:00|7:00 pm|Cumartesi|3: 00'da|2:30 pm|Pazar|Kapalı|
|Pazartesi|Cuma|8:00:00|6:00 pm|Cumartesi|2: 00'da|3:00 pm|Pazar|Kapalı|
|Pazartesi|Cuma|4: 00'da|7:00 pm|Cumartesi|09.00|4:00 pm|Pazar|Kapalı|
|Pazartesi|Cuma|8:30:00|4:30 pm|Cumartesi|09.00|5:00 pm|Pazar|Kapalı|
|Pazartesi|Cuma|5: 30'da|6:30 pm|Cumartesi|5: 00'da|4:00 pm|Pazar|Kapalı|
|Pazartesi|Cuma|8:30:00|8:30 pm|Cumartesi|6: 00'da|5:00 pm|Pazar|Kapalı|
|Pazartesi|Cuma|8:00:00|9:00 pm|Cumartesi|09.00|8:00 pm|Pazar|Kapalı|
|Pazartesi|Cuma|10:00:00|9:30 pm|Cumartesi|09:30:00|3:00 pm|Pazar|Kapalı|

### <a name="splitting-iis-log"></a>Bölme IIS günlüğü

Birden çok rasgele sınırlayıcıları başka bir örneği burada verilmiştir. Bu örnek ayrıca bağlamsal sınırlayıcı içerir "/", içindeki URL'leri ayrılmalıdır değil veya dosya yolları. Bu bölme birçok kullanarak gerçekleştirmek yorucu *türetilen sütun örneğe göre* dönüştürmeler ve örnekler için her bir alan verir. Bölünmüş transform kullanılarak biz Tahmine dayalı tüm örnekler vermeden bölme gerçekleştirebilirsiniz.

|logtext|
|:-----|
|192.128.138.20--[eki/16/2016 16:22:33-0200] "GET /images/picture.gif HTTP/1.1" 234 343 www.yahoo.com "http://www.example.com/" "Mozilla/4.0 (uyumlu; MSIE 4)""-"|
|10.128.72.213--[eki/17/2016 12:43:12 +0300] "GET /news/stuff.html HTTP/1.1" 200 6233 www.aol.com "http://www.sample.com/" "Mozilla/5.0 (MSIE)" "-"|
|192.165.71.165--[Kas/12/2016 14:22:44 -0500] "GET /sample.ico HTTP/1.1" 342 7342 www.facebook.com "-" "Mozilla/5.0 (Windows; U; Windows NT 5.1; RV:1.7.3) ""-"|
|10.166.64.165--[Kas/23/2016 01:52:45 -0800] "GET /style.css HTTP/1.1" 200 2552 www.google.com "http://www.test.com/index.html" "Mozilla/5.0 (Windows)" "-"|
|192.167.1.193--[Oca/16/2017 22:34:56 +0200] "GET /js/ads.js HTTP/1.1" 200 23462 www.microsoft.com "http://www.illustration.com/index.html" "Mozilla/5.0 (Windows)" "-"|
|192.147.76.193--[Oca/28/2017 26:36:16 +0800] "GET /search.php HTTP/1.1" 400 1777 www.bing.com "-" "Mozilla/4.0 (uyumlu; MSIE 6.0; Windows NT 5.1)""-"|
|192.166.64.165--[Mar/23/2017 01:55:25 -0800] "GET /style.css HTTP/1.1" 200 2552 www.google.com "http://www.test.com/index.html" "Mozilla/5.0 (Windows)" "-"|
|11.167.1.193--[Apr/16/2017 11:34:36 +0200] "GET /js/ads.js HTTP/1.1" 200 23462 www.microsoft.com "http://www.illustration.com/index.html" "Mozilla/5.0 (Windows)" "-"|

Böl:

|logtext_1|logtext_2|logtext_3|logtext_4|logtext_5|logtext_6|logtext_7|logtext_8|logtext_9|logtext_10|logtext_11|logtext_12|logtext_13|logtext_14|logtext_15|
|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|
|192.128.138.20|Eki/16/2016|16:22:33|-0200|AL|images/Picture.gif|HTTP|1.1|234|343|www.yahoo.com|http://www.example.com/|Mozilla|4.0|uyumlu; MSIE 4|
|10.128.72.213|Eki/17/2016|12:43:12|+0300|AL|News/STUFF.HTML|HTTP|1.1|200|6233|www.aol.com|http://www.sample.com/|Mozilla|5.0|MSIE|
|192.165.71.165|Kas/12/2016|14:22:44|-0500|AL|Sample.ico|HTTP|1.1|342|7342|www.facebook.com|-|Mozilla|5.0|Windows; U; Windows NT 5.1; RV:1.7.3|
|10.166.64.165|Kas/23/2016|01:52:45|-0800|AL|Style.css|HTTP|1.1|200|2552|www.Google.com|http://www.test.com/index.html|Mozilla|5.0|Windows|
|192.167.1.193|Oca/16/2017|22:34:56|+0200|AL|js/ADs.js|HTTP|1.1|200|23462|www.microsoft.com|http://www.illustration.com/index.html|Mozilla|5.0|Windows|
|192.147.76.193|Oca/28/2017|26:36:16|+0800|AL|Search.php|HTTP|1.1|400|1777|www.Bing.com|-|Mozilla|4.0|uyumlu; MSIE 6.0; Windows NT 5.1|
|192.166.64.165|Mar/23/2017|01:55:25|-0800|AL|Style.css|HTTP|1.1|200|2552|www.Google.com|http://www.test.com/index.html|Mozilla|5.0|Windows|
|11.167.1.193|Apr/16/2017|11:34:36|+0200|AL|js/ADs.js|HTTP|1.1|200|23462|www.microsoft.com|http://www.illustration.com/index.html|Mozilla|5.0|Windows|

## <a name="examples-of-splitting-without-delimiters"></a>Sınırlayıcıları bölme örnekleri
Bazı durumlarda, hiçbir gerçek ayırıcısı vardır ve veri alanları bitişik birbirinin yanına oluşabilir. Bu durumda, bölünmüş dönüştürme büyük olasılıkla bölme noktalarını anlamak için verileri desenleri otomatik olarak algılar. Örneğin, aşağıdaki senaryoda para birimi türünden tutar ayrı istiyoruz ve bölünmüş sınır bölünmüş noktası olarak sayısal ve sayısal olmayan veriler arasında otomatik olarak oluşturur.

### <a name="splitting-amount-with-currency-symbol"></a>Para birimi simgesi ile miktarını bölme

|Miktar|Amount_1|Amount_2|
|:-----|:-----|:-----|
|\$14|$|14|
|£9|£|9|
|\$34|$|34|
|€ 18|€ |18|
|\$42|$|42|
|\$7|$|7|
|£42|£|42|
|\$16|$|16|
|€ 16|€ |16|
|\$15|$|15|
|\$16|$|16|
|€ 64|€ |64|

Aşağıdaki örnekte, ölçü birimlerinden ağırlık değerleri ayırmak isteriz. Yeniden bölünmüş çıkarım anlamlı sınır otomatik olarak algılar ve diğer olası sınırlayıcıları gibi tercih "." karakter. 

### <a name="splitting-weights-with-units"></a>Birimleri ile ağırlıkları bölme

|Ağırlık|Weight_1|Weight_2|
|:-----|:-----|:-----|
|2,27 KG|2.27|KG|
|1 M|1|L|
|2.5 KG|2.5|KG|
|2KG|2|KG|
|1.7KGA|1.7|KGA|
|3KG|3|KG|
|2KG|2|KG|
|125G|125|G|
|500G|500|G|
|1.5KGA|1,5|KGA|

## <a name="technical-notes"></a>Teknik notlar

Bölünmüş dönüştürme özelliği dayanır **Tahmine dayalı Program Birleştirici** tekniği. Bu yöntem, veri dönüştürme programları giriş verilerini göre otomatik olarak öğrenilen. Program bir etki alanına özgü dil oluşturulan. DSL sınırlayıcıları ve özellikle normal ifade bağlamları ortaya alanları temel alır. Bu teknoloji hakkında daha fazla bilgi bulunabilir bir [konunun son yayınında](https://www.microsoft.com/en-us/research/publication/automated-data-extraction-using-predictive-program-synthesis/). 
