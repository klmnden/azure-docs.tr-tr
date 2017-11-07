---
title: "Azure Machine Learning veri hazırlık için veri dönüşümler kullanma | Microsoft Docs"
description: "Bu makale, Azure Machine Learning veri hazırlığı kullanılabilir dönüşümleri tam bir listesini sağlar."
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: 
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 10/09/2017
ms.openlocfilehash: d0b62a63d381239e17b7dccceb89171bca15a68e
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2017
---
# <a name="use-data-transforms-for-data-preparation-in-azure-machine-learning"></a>Azure Machine Learning veri hazırlık için veri Dönüşümleri kullanma

A *dönüştürme* Azure Machine Learning ile verileri belirli bir biçimde kullanır, (örneğin, veri türünü değiştirme) veriler üzerinde bir işlemi gerçekleştirir ve verileri yeni biçiminde üretir. Her dönüştürme kendi arabirimi ve davranışı vardır. Birkaç dönüşümler birlikte veri akışı adımlarda aracılığıyla zincirleme tarafından verilerinizde karmaşık ve tekrarlanabilir dönüştürmeleri gerçekleştirebilirsiniz. Veri hazırlama işlevlerini çekirdek budur.

Azure Machine Learning kullanılabilir dönüşümler verilmiştir. 

## <a name="column-selection"></a>Sütun Seçimi 
Bu listedeki dönüşümler birçoğu, tek bir sütun ya da birçok çalışır. Birden çok sütun seçmek için Ctrl tuşunu kullanın. Sütunların bir aralık seçmek için SHIFT tuşunu kullanın.

## <a name="transforms-from-the-main-menu-or-the-grid-header"></a>Ana menü veya kılavuz başlığından dönüştürür 
Ana menüde dönüşümler seçeneğinden erişim dönüştürür. Dönüşümler veri kılavuzunda sütun adı sağ tıklayarak da seçebilirsiniz. Birden çok sütun seçiliyse, bunlardan herhangi birinin sağ tıklayarak bir dönüşümler menü sağlar.

Sağ tıklatma menüsünden yalnızca seçili veri türü için geçerli dönüşümler gösterir. Ana menü tüm dönüşümler sunar, ancak seçilen sütunlara ilgili olmayan olanlar devre dışı bırakır.

Bağlamsal dönüşümler küçük bir kısmı bir hücre sağ tıklayarak kullanılabilir. Bu dönüşümler kopyalama, Değiştir ve filtreleyebilirsiniz. Sayı sütununa seçeneklerini farklı bir dize sütunu için veri türünü algılayan bunlar olduğundan.

## <a name="derive-column-by-example"></a>Sütun örneğe göre türetilen
Bu dönüştürme, bir veya daha fazla var olan sütun bir türevi yeni bir sütun oluşturmak için kullanın. Dönüştürme giriş (Seçili) sütunları ve verilen örnek arar ve yeni bir sütun istenen çıktıda belirler. 

Bu dönüştürme kullanmak için bir veya daha fazla sütun seçin. Yeni bir (boş) türetilen sütun örnekle ekleyin. (Diğer sütunlardan türetilen varsayılarak) türetilen sütun ve "tarafından teknolojisi sütunda diğer tüm hücrelere doldurun girişiminde örnek" görmek istediğiniz bir örnek yazın. 

Karmaşık örnekler için birden fazla örnek sağlamanız gerekebilir. Bunu yapmak için başka bir hücre seçin ve başka bir örnek yazın.

Örnek anlamını belirlemek denemek için seçilen sütunların "tarafından örnek" teknolojisini kullanır. Bu dönüştürme çağrıldığında hiçbir sütun seçtiyseniz, tüm hücreleri geçerli satır için kullanılır. Yalnızca gerekli sütunları seçerek daha doğru sonuçlar verecektir.

Dönüştürme çağırmadan önce sütunları seçebilirsiniz. Dönüştürme Düzenleyici açıldığında onay kutuları her sütunun üstünde girdi olarak hangi sütunların seçildiği gösterir. Ekleyebilir veya sütunları sütun başlıklarının onay kutularını kullanarak seçimden kaldırın.

Daha ayrıntılı bir açıklaması için **örnek tarafından türetilmeyen sütun** daha fazla örnekleri birlikte bkz [türetilen sütun örnek başvuruya göre](data-prep-derive-column-by-example.md).  

## <a name="split-column-by-example"></a>Örneğe göre bölünmüş sütun
Bu dönüştürme varolan bir sütunla alır ve, "Örnek tarafından" altyapısını kullanarak, bu sütuna bölme girişiminde  *n*  diğer sütunları. Sonraki oluşturulan sütunlarda otomatik bölme çalıştırabilirsiniz.

Daha ayrıntılı bir açıklaması için **örneğe göre bölünmüş sütun** daha fazla örnekleri birlikte bkz [örnek başvuruya göre bölünmüş sütun](data-prep-split-column-by-example.md).

## <a name="expand-json"></a>JSON’ı genişletme

Bu dönüştürme, birden çok sütun geçerli JSON metin içeren bir sütun genişleterek eklemenize olanak tanır.

Daha ayrıntılı bir açıklaması için **genişletin JSON** daha fazla örnekleri birlikte bkz [genişletin JSON başvurusu](data-prep-expand-json.md).


## <a name="combine-columns-by-example"></a>Sütunları örneğe göre birleştirin

Bu dönüştürme, birden çok sütun değerlerinden birleştiren yeni bir sütun ekler. 

Daha ayrıntılı bir açıklaması için **birleştirmek sütunları örneğe göre** daha fazla örnekleri birlikte bkz [birleştirmek sütunları örnek başvuruya göre](data-prep-combine-columns-by-example.md).


## <a name="duplicate-column"></a>Yinelenen sütun
Bu dönüştürme, bir veya daha fazla Seçili sütunlar ve yeni bir ad özgün sütun adından türetilen verir her tam bir kopyasını oluşturur.

## <a name="text-clustering"></a>Kümeleme metin 
Bu dönüştürme tutarsız değeri aynı olması ve birlikte gruplamak yararlanacak şekilde tasarlanmıştır.  

Bu dönüşüm ile tek bir sütundaki değerlerin benzerlik için analiz ve kümeler halinde gruplandırılır. Her küme için kümedeki tüm örneklerini değiştirir değer kurallı bir değer ve tüm örnek değerleri yoktur. Tam küme kaldırılabilir ve kurallı değer düzenlenebilir. Belirli bir kümeden örnekleri kaldırılabilir. Grup örnekleri bir küme içine kullandı benzerlik puan eşik filtre değiştirilebilir.

Varsayılan olarak, bu dönüştürme yeni değerleri içeren yeni bir sütun oluşturma, bu küme için kurallı değerine sahip tüm küme örneği değerleri değiştirir. Veri akışı daha sonra kullanmak için (adlandırılabilir) yeni bir sütun için benzerlik puan her örneği için de ekleyebilirsiniz.

## <a name="replace-values"></a>Değerleri değiştirin
Başka bir dizesini değiştirmek için bu dönüşüm kullanın. Kaynak dizesi kısmi dize veya tam bir hücre olabilir ve tek bir sütun veya birçok değiştirme uygulayabilirsiniz. Arama dizesi normal karakterler için olduğu gibi özel karakterler de arama destekler. 

## <a name="replace-na-values"></a>BELİRTİLMEYEN değerlerini değiştirin
BELİRTİLMEYEN çeşitli biçimlerde değiştirmek için bu dönüşüm kullanın (yok, ad, null, NaN, vb.), ya da boş dizeler tutarlı hale getirmek için tek bir değer ile değiştirmek için. Bir veya daha çok sütun bu dönüşüm destekler ve yalnızca bir sütun seçildiğinde listelenir. Hiç sütun yok seçildiğinde ana dönüştürme menüsünden mevcut değil.

## <a name="replace-missing-values"></a>Eksik değerleri değiştirin
Bu dönüştürme eksik verileri tek bir değeri ile değiştirir ve bir veya daha çok sütun destekler. Bu dönüştürme, yalnızca bir sütun seçildiğinde listelenir. Hiç sütun yok seçildiğinde ana dönüştürme menüsünden mevcut değil.

## <a name="replace-error-values"></a>Hata değerlerini değiştirme
Bu dönüştürme hataları tek bir değeri ile değiştirir ve bir veya daha çok sütun destekler. Bu dönüştürme, yalnızca bir sütun seçildiğinde listelenir. Hiç sütun yok seçildiğinde ana dönüştürme menüsünden mevcut değil.

## <a name="trim-string"></a>Dize kırpma
Bu dönüştürme, baştaki ve sondaki "boşluk" karakterlerinden (boşluk, sekme, vb. dahil) bir veya daha fazla sütun kaldırır.

## <a name="adjust-precision"></a>Duyarlık Ayarla
Bu dönüştürme için sayısal bir sütun ondalık basamak sayısını ayarlar.

## <a name="rename-column"></a>Sütunu yeniden adlandırma
Bu dönüştürme seçili sütunun adını değiştirir. Ayrıca, satır içi sütun üstbilgisindeki sütun adına tıklayarak çağırabilirsiniz.

## <a name="remove-column"></a>Sütun Kaldır
Seçili sütunları bu dönüşüm kaldırır ve tek bir sütun veya birçok çalışır. 

## <a name="keep-column"></a>Sütunun koruyun
Yalnızca Seçili sütunları bu dönüşüm tutar ve tek bir sütun veya birçok çalışır.

## <a name="handle-path-column"></a>İşleyici yolu sütun
Bir dosyayı içe aktarma sırasında bir yol sütun otomatik olarak veri kümesi tarafından eklenen **veri kaynağı Ekle** özelliği. Veri kümesi yoluna forms tam dosya adını içerir. Bu dönüştürme ekler veya ek sütunun veri kümesinden kaldırır.

## <a name="convert-field-type-to-numeric"></a>Alan türü sayısal Dönüştür
Bu dönüştürme için sayısal sütun türünü değiştirir. Tamsayı olmayan veriler için ayırıcı belirtebilirsiniz. Varsayılan olarak, bu dönüştürme için ayırıcı sor değil, bu nedenle kullanın **Düzenle** Düzenleyicisi çağrılacak menü öğesi. Bu dönüştürme, tek bir sütun veya birçok çalışır.

## <a name="convert-field-type-to-date"></a>Alan türü Date olarak Dönüştür
Bu dönüştürme sütun türü date olarak değiştirir. Varsayılan tarih/saat biçimi kullanılır, ancak kullanarak kılınabilir `strftime` yönergeleri. Saat değerleri bir sabit tarihi ile önüne ekleyin.

Varsayılan olarak, bu dönüştürme için biçim sor değil, bu nedenle kullanın **Düzenle** Düzenleyicisi çağırmak için sonuç adımında menü öğesi. Bu dönüştürme, tek bir sütun veya birçok çalışır.

Yalnızca, 9-22-1677 tarihleri ve 4 11 2262 tarih türüne dönüştürülebilir.

## <a name="convert-field-type-to-boolean"></a>Alan türü Boolean değerine dönüştürme
Bu dönüştürme sütun türü true/false olarak değiştirir. Birden çok değer true veya false eşleme destekler ve bu eşlemeler düzenleyebilirsiniz. Ayrıca doğru/yanlış eşleme tablolarda yok değerleri eşleyebilirsiniz sabiti destekler. Bu dönüştürme, tek bir sütun veya birçok çalışır.

## <a name="convert-field-type-to-string"></a>Alan türü dizeye dönüştürme
Bu dönüştürme sütun türü dize olarak değiştirir ve tek bir sütun ya da birden çok üzerinde çalışır.

## <a name="convert-unix-timestamp-to-datetime"></a>UNIX zaman damgası DateTime olarak dönüştürme
Verilerdeki dize olarak temsil edilir olsa bile bu dönüşüm UNIX zaman damgası biçimi bilir. Zaman damgası tarih türü veri hazırlık doğru dönüştürür.

## <a name="filter"></a>Filtre
Bu dönüştürme, bir veya daha çok sütunlardaki değerlere göre satır filtreleme destekler. "İçerir" veya "içermiyor." dizesi sütunları filtrelemek için filtre koşulları her bir sütunun veri türüne bağlıdır. Sayısal sütunlar "büyüktür" veya "küçüktür" koşullara göre filtre uygulanabilir.

Zaman **filtre** dönüştürme, ana menüden çağrıldığında ya da bir sütunun başlığına sağ tıklayarak gelen başarısız olan satırları başka bir veri akışına çatallaştırma seçeneği mevcuttur. İle devam filtre ana veri akışı **içinde** satır ve yeni bir veri akışı oluşturulur filtre uygulanmış tüm satırları içeren **çıkışı**.

## <a name="use-first-row-as-headers"></a>İlk satırı başlığı olarak kullan
Bu dönüştürme ilk satırı sütun adlarının olması için veri kümesinden yükseltir. Bazı sütunları verilerin ilk satırında yoksa, otomatik olarak oluşturulan bir adı değil. Bu dönüşüm kullanarak veri aktarımını 2 satırındaki en kısa zamanda başladığını anlamına gelir. Bağlı olarak **Atla satırları** ayarı veri kümesinde alma bile aşağıya başlatabilirsiniz. Bu yükseltme kaldırmak ve yalnızca adlarını otomatik olarak oluşturulan kullanmak için de kullanabilirsiniz.

## <a name="join"></a>Birleştir
Bu dönüştürme, iki veri akışı birlikte eklemek için kullanılır. Hangi çıktı sonucu olarak katılma seçebilirsiniz: başarılı satırları birleştirme, "sol" katılma veya "sağ" Katıl satırları başarısız olan satırları başarısız.

**Birleştirme** dönüştürme tek bir veri akışından başlar ve bu veri akışı katılma sütunlardan seçer. Ardından katılma diğer taraf için başka bir veri akışı seçmenizi ister. İki akışı seçtikten sonra dönüşüm katılım için seçilecek katılma her iki taraftaki tek bir sütun gerektiriyor. Birleşim birden fazla sütun gerekirse, yalnızca birleştirme için kullanılacak yeni, birleştirilmiş bir sütun oluşturmak için dönüştürme başlatmadan önce türetilmiş bir sütun oluşturun. Birleşim anahtarları önermek ve mümkünse, türetilmiş sütunları otomatik olarak oluşturmak için dönüştürme çalışır.

Birleştirme tamamlandıktan sonra bir Sankey diyagram görünümü katılma sunulur. Birbirine çizgilerin genişliğini birleştirme akış kısmı taşıma satır sayısını temsil eder. Sağ tarafta Masası'nı kullanarak başarılı satır, satırları veya satır özel olarak başarısız sağ başarısız sol seçebilirsiniz. Tek bir şube de seçebilirsiniz.

## <a name="append-rows"></a>Satır Ekle
Bu dönüştürme için geçerli bir başka bir veri akışından veri ekler. Sütunları sonunda yeni satır eklemek için konumuna göre eşler.

## <a name="append-columns"></a>Sütunlar ekleme
Bu dönüştürme başka bir veri akışı yeni sütunlarından geçerli bir ekler. Yeni sütunlar sağa ekler. Veri satırı hizalamak girişimi değil.

## <a name="summarize"></a>Özetle
Bu dönüştürme için bir veya daha fazla seçili sütunlardaki benzersiz değerlerin birleşimi toplamalar hesaplar. 

Desteklenen toplamalar şunlardır: sayısı, Topla, MIN, MAX, ortalama, SAPMASI ve standart sapma. Veri türü için geçerli toplamalara Toplamalar için belirli bir sütun listesi filtrelenir. Geçerli toplamalar devre dışı bırakılır. Örneğin, bir dize sütunu ORTALAMASI işlem mümkün değildir, ancak bu işlem MIN ve MAX mümkündür.

Düzenleyici kullanılabilir olduğunda, sütun başlığından üst sol paneline toplanacak sütunları görüntülendiği yukarı sürükleyin. Bu panoyu hiyerarşik olduğundan iç içe toplamalar yapabilirsiniz. Sağ üst köşedeki Düzenleyicisi Masası bir sütuna uygulamak için hangi toplama seçmek için kullanılır. Tek bir sütun bir veya birden çok kez kümelenebilir. En az bir toplama seçildikten sonra sağ alt köşede kılavuz veri toplama biçimde önizlemeleri sağlanır. 

Bu dönüştürme için benzer bir `ANSI-SQL GROUP BY`.

## <a name="remove-duplicates"></a>Yinelenenleri kaldırma
Bir veya daha fazla seçili sütunda yinelenen değerler olduğunda bu dönüşüm tüm satırı kaldırır. Hiçbir sütun seçildi, kaldırılan yalnızca satır olanları tüm sütun değerlerinin aynı olduğu demektir.

## <a name="sort"></a>Sırala
Bu dönüştürme verileri sıralar. Tek bir sütun veya birçok sıralama yapılabilir ve her bir sütunun (varsayılan) artan veya azalan (hangi Düzenleyicisi'nden değiştirilebilir) sıralanabilir.

Bu dönüştürme için benzer bir `ANSI-SQL ORDER BY`.

## <a name="output-transforms"></a>Çıktı dönüşümler
Aşağıdaki dönüşümler veri çıkışı. Verileri farklı noktalarda dışarıda yazmak için tek bir akış içinde birden çok yazma blokları olabilir.

### <a name="write-to-csv"></a>CSV'ye yazma
Bu dönüştürme veri akışında geçerli noktasından CSV formundaki verileri çıkışı yazar. (Yerel veya uzak) konumu ve dosya geçici çeşitli ayarları denetler.

### <a name="write-to-parquet"></a>Parquet için yazma
Bu dönüştürme veri akışında geçerli noktasından Parquet formundaki verileri çıkışı yazar. (Yerel veya uzak) konumu ve dosya geçici çeşitli ayarları denetler.

## <a name="script-based-transforms"></a>Betik tabanlı dönüşümler
Aşağıdaki dönüşümler çekirdek ürüne eksik işlevleri gerçekleştirmek için komut dosyası (Python) kullanın. 

>[!NOTE]
>Bu dönüşümler birini kullanmadan önce okuyun [kullanarak Python genişletilebilirlik](data-prep-python-extensibility-overview.md).

### <a name="add-column-script"></a>Sütun (komut) ekleyin
Bu dönüştürme Python ifade kullanarak verileri bir sütun ekler.
Daha fazla bilgi için bkz: [yeni sütunlar türetme için örnek Python kodu](data-prep-appendix10-sample-custom-column-transforms-python.md).

### <a name="advanced-filter-script"></a>Gelişmiş Filtre (komut)
Bu dönüştürme Python satır düzeyi filtresi yazmak için kullanın.
Daha fazla bilgi için bkz: [örnek filtre ifadeleri](data-prep-appendix6-sample-filter-expressions-python.md).

### <a name="transform-dataflow-script"></a>Veri akışı (komut) dönüştürme
Bu dönüşüm Python tüm veri kümesi uygular.
Daha fazla bilgi için bkz: [örnek dönüştürme veri akışını dönüşümleri](data-prep-appendix7-sample-transform-data-flow-python.md).

Bu dönüştürme için tüm veri bölümü de Python uygulayabilirsiniz.
Daha fazla bilgi için bkz: [örnek dönüştürme veri akışını dönüşümleri](data-prep-appendix7-sample-transform-data-flow-python.md).

### <a name="write-dataflow-script"></a>Veri akışı (komut) yazma
Bu dönüştürme Python bütün bir veri kümesi yazmak için kullanır.
Daha fazla bilgi için bkz: [yeni sütunlar türetme için örnek Python](data-prep-appendix9-sample-destination-connections-python.md).



