---
title: Azure Machine Learning Workbench'i kullanarak örnek dönüştürme tarafından sütunu Böl
description: Başvuru belgesini 'Sütunu örneğe göre Böl' dönüştürme için
services: machine-learning
author: ranvijaykumar
ms.author: ranku
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: mvc, reference
ms.topic: article
ms.date: 09/14/2017
ROBOTS: NOINDEX
ms.openlocfilehash: 3edf49484e5bc05a297b8d8969632fb902aa1714
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46953744"
---
# <a name="split-column-by-example-transformation"></a>Örnek dönüştürme tarafından sütunu Böl

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 


Bu dönüşüm, kullanıcı girişi gerektirmeden bir sütunun anlamlı sınırlarındaki içeriğini predictively böler. Bölünmüş algoritması, sütunun içerik çözümledikten sonra sınırları seçer. Bu sınırları tarafından tanımlanan
* Sabit bir sınırlayıcı
* Birden çok, rastgele sınırlayıcılar bağlamı, özellikle görüntülenmesini veya
* Veri modelleri veya belirli bir varlık türleri

Kullanıcılar ayrıca nerede bunlar sınırlayıcılar belirtebilir veya istenen bölme örnekleri sağladığı gelişmiş modda bölme davranışını kontrol edebilirsiniz.

Teorik olarak, bölme işlemleri ayrıca Workbench'teki bir dizi kullanarak gerçekleştirilebilir *sütunu örneğe göre türet* dönüştürür. Birden çok sütun varsa, ancak bu hatta tek tek örnek tarafından yaklaşımı kullanarak türetme çok zaman alabilir. Tahmine dayalı bir bölme, kullanıcının her sütun için örnek sağlamaya gerek olmadan kolayca bölme sağlar. 

## <a name="how-to-perform-this-transformation"></a>Bu dönüşüm gerçekleştirme

1. Bölmek istediğiniz sütunu seçin. 
2. Seçin **sütunu örneğe göre Böl** gelen **dönüştüren** menüsü. Veya, seçili sütunun başlığına sağ tıklayıp **sütunu örneğe göre Böl** bağlam menüsünden. Dönüştürme Düzenleyicisi açılır ve yeni sütunlar yanında seçili olan sütunda eklenir. Bu noktada, Workbench, giriş sütunundaki analiz eder, bölünmüş sınırları belirler ve kılavuzunda görüntülenen sütununu bölmek için bir program sentezler. Sentezlenen programın sütundaki tüm satırlar karşı yürütülür. Sınırlayıcı, varsa, nihai sonucu hariç tutulur.
3. Tıklayabilirsiniz **Gelişmiş mod** bölme dönüştürme üzerinde daha hassas bir denetim. 
4. ' A tıklayın ve çıkış gözden **Tamam** dönüştürmeyi kabul etmek için. 

Sonuç sütunları tüm satırların aynı sayıda üretmek için dönüştürme amaçlar. Herhangi bir satır belirlenen sınırları bölünemez, ürettiği *null* varsayılan olarak tüm sütunlar için. Bu davranış değiştirilebilir **Gelişmiş mod**.

### <a name="transform-editor-advanced-mode"></a>Düzenleyici dönüştürme: Gelişmiş mod
**Gelişmiş mod** sütunları bölme için daha zengin bir deneyim sağlar. 

Seçme **sınırlayıcı sütunları tutun** sınırlayıcıları içinde nihai sonucu içerir. Sınırlayıcı varsayılan olarak dışlanır.

Belirtme **sınırlayıcılar** otomatik sınırlayıcı seçim mantığını geçersiz kılar. Her satırda birden çok sınırlayıcıya olarak belirlenebilir **sınırlayıcılar**. Bu karakterleri sınırlayıcı olarak sütununu bölmek için kullanılır.

Bazı durumlarda belirlenen sınırlar oluşturan farklı sayıda sütuna diğerleri çoğunu değerinden bir değere bölerek. Bu durumlarda **dolgu yönü** içinde sütun doldurulmalıdır sırası karar vermek için kullanılır.

Tıklayarak **önerilen örnekler** görüntüler bölünmüş bir örneği temsil eden satırlar için hangi kullanıcı sağlamalıdır. Kullanıcı tıklayabilirsiniz **yukarı** satır örnek olarak yükseltmek için önerilen satırın sağ tarafındaki oku. 

Kullanıcı **Sütun Sil** veya **yeni sütun ekleme** başlığında sağ tıklanarak **örnekler tablo**.

Kullanıcı kopyalayabilir ve Yapıştır bir hücreden başka bir bölünmüş bir örneğini sağlamak için değerler.

Kullanıcı arasında geçiş yapmak **temel mod** ve **Gelişmiş mod** dönüştürme Düzenleyicisi bağlantıları tıklayarak.

### <a name="transform-editor-send-feedback"></a>Düzenleyici dönüştürme: geri bildirim gönder

Tıklayarak **geri bildirim gönder** bağlantı açar **geri bildirim** parametre seçimleri ve örnek kullanıcı ile önceden doldurulmuş Açıklama kutusuna bir iletişim kutusunda sağlanan. Kullanıcı yorumları kutunun içeriğini gözden geçirmeniz ve sorunu anlamanıza yardımcı olmak için daha fazla ayrıntı sağlanır. Kullanıcı verilerini Microsoft ile paylaşmak istemediği, kullanıcı önceden doldurulmuş bir örnek veri tıklamadan önce silmelisiniz **geri bildirim gönder** düğmesi. 


### <a name="editing-an-existing-transformation"></a>Mevcut bir dönüşüm düzenleme

Varolan bir kullanıcı düzenleyebilir **göre bölünmüş sütun örneği** Dönüştür seçip **Düzenle** dönüştürme adımı seçeneği. Tıklayarak **Düzenle** dönüştürme Düzenleyicisi'nde açılır **Gelişmiş mod**, ve dönüştürme, oluşturma sırasında sağlanan tüm örnekler gösterilmektedir.

## <a name="examples-of-splitting-on-a-fixed-single-character-delimiter"></a>Bir sabit, tek karakterli sınırlayıcı bölme örnekleri

Veri alanı bir CSV biçiminde virgül gibi tek bir sabit sınırlayıcıyla ayrılmış yaygındır. Bölme dönüştürme bu sınırlayıcıları otomatik olarak tanım Çıkarsama dener. Örneğin, aşağıdaki senaryoda, otomatik olarak algılar "." ayırıcı olarak.

### <a name="splitting-ip-addresses"></a>Bölme IP adresleri

İlk sütunda bulunan değerlerin predictively dört sütuna bölünür.

|IP|IP_1|IP_2|IP_3|IP_4|
|:-----|:-----|:-----|:-----|:-----|
|192.168.0.102|192|168|0|102|
|192.138.0.101|192|138|0|101|
|192.168.0.102|192|168|0|102|
|192.158.1.202|192|158|1|202|
|192.168.0.102|192|168|0|102|
|192.169.1.102|192|169|1|102|

## <a name="examples-of-splitting-on-multiple-delimiters-within-particular-contexts"></a>Belirli bağlamı içinde birden çok sınırlayıcıya üzerinde bölme örnekleri

Kullanıcı verileri, farklı alanları ayırmak için birçok farklı sınırlayıcı içerebilir. Ayrıca, bazı örnekleri yalnızca sınırlayıcı bir dizenin tümünü değil ancak bir sınırlayıcı olabilir. Örneğin, aşağıdaki örnekte gerekli sınırlayıcılar kümesidir "-",","ve":" istenen çıkış oluşturmak için. Ancak, her geçtiği ":" biz zaman bölme, ancak tek bir sütunda tutmak istemediğiniz bu yana bir ayırma noktasıyla olmalıdır. Bölme dönüştürme, olası tüm oluşumlarıyla sınırlayıcı yerine girdi verilerini ortaya bağlamları sınırlayıcıları çıkarır. Dönüştürme de tarihler ve saatler gibi ortak veri türleri farkındadır.   

### <a name="splitting-store-opening-timings"></a>Depolama açılış zamanlamaları bölme

Aşağıdaki değerler *zamanlamaları* sütunu predictively altındaki tabloda gösterilen dokuz sütunlara bölme.

|Zamanlamaları|
|:-----|
|Pazartesi - Cuma: 7: 00'da - 18:00:00, Cumartesi: 09:00:00 - 17:00:00, Pazar: kapalı|
|Pazartesi - Cuma: 09:00:00 - 18:00:00, Cumartesi: 4: 00'da - 4:00 pm, Pazar: kapalı|
|Pazartesi - Cuma: 8:30:00 - 7:00 pm Cumartesi: 03: 00'da - 2:30 pm, Pazar: kapalı|
|Pazartesi - Cuma: 8:00 am - 6:00 pm Cumartesi: 02:00:00 - 3:00 pm, Pazar: kapalı|
|Pazartesi - Cuma: 4: 00'da - 7:00 pm Cumartesi: 09:00:00 - 4:00 pm, Pazar: kapalı|
|Pazartesi - Cuma: 8:30:00 - 4:30 pm, Cumartesi: 09:00:00 - 17:00:00, Pazar: kapalı|
|Pazartesi - Cuma: 5:30 am - 6:30 pm, Cumartesi: 5: 00'da - 4:00 pm, Pazar: kapalı|
|Pazartesi - Cuma: 8:30:00 - 8:30 pm, Cumartesi: 6: 00'da - 17:00:00, Pazar: kapalı|
|Pazartesi - Cuma: 8: 00'da - 9:00 pm Cumartesi: 09:00:00 - 8:00 pm, Pazar: kapalı|
|Pazartesi - Cuma: 10: 00'da - 9:30 pm, Cumartesi: 09:30:00 - 3:00 pm, Pazar: kapalı|

|Timings_1|Timings_2|Timings_3|Timings_4|Timings_5|Timings_6|Timings_7|Timings_8|Timings_9|
|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|
|Pazartesi|Cuma|7: 00'da|18:00:00|Cumartesi|09.00|-17:00|Pazar|Kapatıldı|
|Pazartesi|Cuma|09.00|18:00:00|Cumartesi|4: 00'da|4:00 pm|Pazar|Kapatıldı|
|Pazartesi|Cuma|8:30:00|19:00:00|Cumartesi|3: 00'da|2:30 pm|Pazar|Kapatıldı|
|Pazartesi|Cuma|8: 00'da|18:00:00|Cumartesi|2: 00'da|3:00 pm|Pazar|Kapatıldı|
|Pazartesi|Cuma|4: 00'da|19:00:00|Cumartesi|09.00|4:00 pm|Pazar|Kapatıldı|
|Pazartesi|Cuma|8:30:00|4:30 pm|Cumartesi|09.00|-17:00|Pazar|Kapatıldı|
|Pazartesi|Cuma|5: 30'da|6:30 pm|Cumartesi|5: 00'da|4:00 pm|Pazar|Kapatıldı|
|Pazartesi|Cuma|8:30:00|8:30 pm|Cumartesi|6: 00'da|-17:00|Pazar|Kapatıldı|
|Pazartesi|Cuma|8: 00'da|saat 21:00|Cumartesi|09.00|8:00 pm|Pazar|Kapatıldı|
|Pazartesi|Cuma|10: 00'da|9:30 pm|Cumartesi|09:30:00|3:00 pm|Pazar|Kapatıldı|

### <a name="splitting-iis-log"></a>Bölme IIS günlüğü

Birden çok rastgele sınırlayıcılar başka bir örneği aşağıda verilmiştir. Bu örnek ayrıca bağlamsal sınırlayıcı içerir "/", içindeki URL'leri bölmemelisiniz veya dosya yolları. Bu bölme birçok kullanarak gerçekleştirmeyi yorucu bir süreç *sütunu örneğe göre türet* dönüştürmeler ve her alan için dair örnekler verir. Bölme dönüştürme kullanarak Tahmine dayalı örnekleri vermeden bölme gerçekleştirebiliriz.

|logtext|
|:-----|
|192.128.138.20--[eki/16/2016 16:22:33-0200] "GET /images/picture.gif HTTP/1.1" 234 343 www.yahoo.com "http://www.example.com/" "Mozilla/4.0 (uyumlu; MSIE 4)""-"|
|10.128.72.213--[eki/17/2016 12:43:12 +0300] "GET /news/stuff.html HTTP/1.1" 200 6233 www.aol.com "http://www.sample.com/" "Mozilla/5.0 (MSIE)" "-"|
|192.165.71.165--[Kasım/12/2016 14:22:44 -0500] "GET /sample.ico HTTP/1.1" 342 7342 www.facebook.com "-" "Mozilla/5.0 (Windows; U; Windows NT 5.1; RV:1.7.3) ""-"|
|10.166.64.165--[Kasım/23/2016 01:52:45 -0800] "GET /style.css HTTP/1.1" 200 2552 www.google.com "http://www.test.com/index.html" "Mozilla/5.0 (Windows)" "-"|
|192.167.1.193--[Oca/16/2017 22:34:56 + 0200] "GET /js/ads.js HTTP/1.1" 200 23462 www.microsoft.com "http://www.illustration.com/index.html" "Mozilla/5.0 (Windows)" "-"|
|192.147.76.193--[Oca/28/2017 26:36:16 +0800] "GET /search.php HTTP/1.1" 400 1777 www.bing.com "-" "Mozilla/4.0 (uyumlu; MSIE 6.0; Windows NT 5.1)""-"|
|192.166.64.165--[Mar/23/2017 01:55:25 -0800] "GET /style.css HTTP/1.1" 200 2552 www.google.com "http://www.test.com/index.html" "Mozilla/5.0 (Windows)" "-"|
|11.167.1.193--[Apr/16/2017 11:34:36 + 0200] "GET /js/ads.js HTTP/1.1" 200 23462 www.microsoft.com "http://www.illustration.com/index.html" "Mozilla/5.0 (Windows)" "-"|

Böl:

|logtext_1|logtext_2|logtext_3|logtext_4|logtext_5|logtext_6|logtext_7|logtext_8|logtext_9|logtext_10|logtext_11|logtext_12|logtext_13|logtext_14|logtext_15|
|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|
|192.128.138.20|Eki/16/2016|16:22:33|-0200|GET|images/Picture.gif|HTTP|1.1|234|343|www.yahoo.com|http://www.example.com/|Mozilla|4.0|uyumlu; MSIE 4|
|10.128.72.213|Eki/17/2016|12:43:12|+0300|GET|News/STUFF.HTML|HTTP|1.1|200|6233|www.aol.com|http://www.sample.com/|Mozilla|5.0|MSIE|
|192.165.71.165|Kasım/12/2016|14:22:44|-0500|GET|Sample.ico|HTTP|1.1|342|7342|www.facebook.com|-|Mozilla|5.0|Windows; U; Windows NT 5.1; RV:1.7.3|
|10.166.64.165|Kasım/23/2016|01:52:45|-0800|GET|Style.css|HTTP|1.1|200|2552|www.Google.com|http://www.test.com/index.html|Mozilla|5.0|Windows|
|192.167.1.193|Jan/16/2017|22:34:56|+0200|GET|js/ADs.js|HTTP|1.1|200|23462|www.microsoft.com|http://www.illustration.com/index.html|Mozilla|5.0|Windows|
|192.147.76.193|Jan/28/2017|26:36:16|+0800|GET|Search.php|HTTP|1.1|400|1777|www.Bing.com|-|Mozilla|4.0|uyumlu; MSIE 6.0; Windows NT 5.1|
|192.166.64.165|Mart/23/2017|01:55:25|-0800|GET|Style.css|HTTP|1.1|200|2552|www.Google.com|http://www.test.com/index.html|Mozilla|5.0|Windows|
|11.167.1.193|Apr/16/2017|11:34:36|+0200|GET|js/ADs.js|HTTP|1.1|200|23462|www.microsoft.com|http://www.illustration.com/index.html|Mozilla|5.0|Windows|

## <a name="examples-of-splitting-without-delimiters"></a>Sınırlayıcıları bölme örnekleri
Bazı durumlarda, hiçbir gerçek sınırlayıcılar vardır ve veri alanlarını bitişik birbirinin yanına oluşabilir. Bu durumda, bölünmüş dönüştürme bölme noktalarını büyük olasılıkla çıkarsanacak verilerdeki desenleri otomatik olarak algılar. Örneğin, aşağıdaki senaryoda miktarı para birimi türünden ayrı istiyoruz ve bölünmüş sınır bölünmüş noktası olarak sayısal ve sayısal olmayan veriler arasında otomatik olarak algılar.

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

Ölçü birimlerindeki ağırlık değerlerini ayırmak aşağıdaki örnekte, istiyoruz. Yeniden bölünmüş çıkarımı anlamlı sınır otomatik olarak algılar ve diğer olası sınırlayıcılar gibi tercih "." karakter. 

### <a name="splitting-weights-with-units"></a>Ağırlıklar birimleri ile bölme

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

Bölme dönüştürme özelliği dayanır **Tahmine dayalı Program sentezi** tekniği. Bu teknikte veri dönüştürme programları girdi verilerini göre otomatik olarak öğrenilen. Program bir etki alanına özgü dili oluşturulan. DSL sınırlayıcıları ve normal ifade bağlamları özellikle oluşan alanları temel alır. Bu teknoloji hakkında daha fazla bilgi bulunabilir bir [bu konuyla ilgili en son yayını](https://www.microsoft.com/research/publication/automated-data-extraction-using-predictive-program-synthesis/). 
