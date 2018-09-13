---
title: Azure Machine Learning veri hazırlama için veri dönüşümler kullanma | Microsoft Docs
description: Bu makalede, Azure Machine Learning veri hazırlama için kullanılabilir dönüştürmeler tam bir listesini sağlar.
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: ''
ms.devlang: ''
ms.topic: article
ms.date: 02/01/2018
ms.openlocfilehash: c47d9bc72ad1d197b5030076456f9dc9efc422bc
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "35647220"
---
# <a name="use-data-transforms-for-data-preparation-in-azure-machine-learning"></a>Azure Machine Learning veri hazırlama için veri dönüşümler kullanma

A *dönüştürme* verileri belirli bir biçimde Azure Machine Learning'de kullanır (örneğin, veri türünü değiştirme) veriler üzerinde bir işlem gerçekleştirir ve ardından yeni biçimindeki verileri oluşturur. Her dönüştürme, kendi arabirimi ve davranışı vardır. Dönüşümlerden birbirine veri akışı'ndaki adımları aracılığıyla zincirleme olarak, verileriniz üzerinde karmaşık ve tekrarlanabilir dönüştürmeleri gerçekleştirebilirsiniz. Veri hazırlama işlevi setinin budur.

Azure Machine Learning'de kullanılabilen dönüşümler verilmiştir. 

## <a name="column-selection"></a>Sütun Seçimi 
Bu listedeki dönüşümleri birçoğu, tek bir sütun veya birçok çalışır. Birden çok sütun seçmek için Ctrl tuşunu kullanın. Bir dizi sütunları seçmek için Shift tuşunu kullanın.

## <a name="transforms-from-the-main-menu-or-the-grid-header"></a>Ana menü veya kılavuz başlığından dönüştürür 
Ana menüsündeki dönüşümler seçeneğinden erişim dönüştürür. Dönüşümler veri kılavuzunda sütun adı sağ tıklayarak da seçebilirsiniz. Birden çok sütun seçiliyse, bunlardan herhangi birini sağ dönüşümler menüsü sağlar.

Sağ tıklama menüsünün yalnızca seçili veri türü için geçerli dönüşümler gösterir. Ana menü tüm dönüşümler sunar ancak seçilen sütunlara ilgisi olmayan bu devre dışı bırakır.

Bağlamsal dönüştürmeleri küçük bir kısmı, bir hücreye sağ tıklayarak kullanılabilir. Bu dönüştürmeler, kopyalama, değiştirme ve filtreleyin. Bir sayı sütununun seçenekleri bir dize sütunu için farklı şekilde veri türünü algılayan şunlardır.

## <a name="derive-column-by-example"></a>Sütunu Örneğe Göre Türet
Bu dönüşüm, bir veya daha fazla var olan sütun bir türev yeni bir sütun oluşturmak için kullanın. Dönüştürme giriş (Seçili) sütunları ve verilen örneği arar ve ardından yeni bir sütun istenen çıktıda belirler. 

Bu dönüşüm kullanmak için bir veya daha fazla sütun seçin. Yeni (boş) türetilen bir sütunu örneğe göre ekleyin. (Diğer sütunlardan türetilir varsayılarak) türetilmiş sütun ve "tarafından teknoloji sütunda diğer tüm hücrelere doldurun dener örnek" görmek istediğiniz bir örnek yazın. 

Karmaşık örnekler için birden fazla örnek sağlamanız gerekebilir. Bunu yapmak için başka bir hücreyi seçin ve başka bir örnek yazın.

"Örnek olarak" teknoloji Seçili sütunları örneği anlamını belirlemek denemek için kullanır. Daha sonra bu dönüşüm çağrıldığında hiçbir sütun seçtiyseniz, tüm hücreleri geçerli satır için kullanılır. Yalnızca gerekli sütunlar seçilmesi daha doğru sonuçlar verecektir.

Dönüştürme çağırmadan önce sütunları seçebilirsiniz. Dönüştürme Düzenleyici açıldığında sütunları girdi olarak seçilen her bir sütunun üstündeki onay kutularını gösterir. Ekleyebilir veya sütun başlıklarını onay kutularını kullanarak sütunları seçimden kaldırın.

Daha ayrıntılı bir açıklaması için **türetilmiş sütunu örneğe göre** birlikte daha fazla örnek için bkz [sütunu örnek başvuruya göre türet](data-prep-derive-column-by-example.md).  

## <a name="split-column-by-example"></a>Sütunu örneğe göre Böl
Bu dönüşüm varolan bir sütunla alır ve "Örnek tarafından" Altyapısı'nı kullanarak bu sütuna bölmek çalışır *n* diğer sütunları. Sonraki oluşturulan sütunlarda otomatik bölme çalıştırabilirsiniz.

Daha ayrıntılı bir açıklaması için **sütunu örneğe göre Böl** birlikte daha fazla örnek için bkz [örnek başvuru sütunu Böl](data-prep-split-column-by-example.md).

## <a name="expand-json"></a>JSON’ı genişletme

Bu dönüşüm, geçerli JSON metnine sahip bir sütunu genişleterek birden çok sütun eklemenize olanak sağlar.

Daha ayrıntılı bir açıklaması için **genişletin JSON** birlikte daha fazla örnek için bkz [genişletin JSON başvurusu](data-prep-expand-json.md).


## <a name="combine-columns-by-example"></a>Sütunu örneğe göre birleştirme

Bu dönüşüm, birden fazla sütundaki değerleri birleştiren yeni bir sütun ekler. 

Daha ayrıntılı bir açıklaması için **birleştirmek sütunları örneğe göre** birlikte daha fazla örnek için bkz [birleştirmek sütunları örnek başvuruya göre](data-prep-combine-columns-by-example.md).


## <a name="duplicate-column"></a>Sütunu Çoğalt
Bu dönüşüm, bir veya daha fazla seçili sütunları ve yeni bir ad özgün sütun adından türetilen verir her tam bir kopyasını getirir.

## <a name="text-clustering"></a>Kümeleme metin 
Bu dönüşüm, aynı olması ve gruplandırabilirsiniz gereken değerleri tutarsız yararlanacak şekilde tasarlanmıştır.  

Bu dönüşüm ile tek bir sütundaki değerleri için benzerlik analiz ve kümeler halinde gruplandırılır. Her küme için küme içindeki tüm örnekleri yerini alan değer olan kurallı bir değer ve tüm örnek değerleri yok. Tam küme kaldırılabilir ve kurallı değer düzenlenemez. Belirtilen kümeden örnekleri kaldırılabilir. Bir küme içine Grup örneklerine kullandı benzerlik puanı eşiği filtre değiştirilebilir.

Varsayılan olarak, bu dönüşüm kurallı değer yeni değerleri içeren yeni bir sütun oluşturma, bu küme için tüm küme örneği değerlerini değiştirir. Daha sonra veri akışı kullanmak için (yani adlandırılabilir) yeni bir sütun için her örneği için benzerlik puanı da ekleyebilirsiniz.

## <a name="replace-values"></a>Değerleri Değiştir
Bu dönüşüm, başka bir dizede değiştirilecek kullanın. Kaynak dizesi kısmi dize veya tam bir hücre olabilir ve tek bir sütun veya birçok değişiklik uygulayabilirsiniz. Arama dizesinin normal karakterler hem de özel karakterler için aramayı destekler. 

## <a name="replace-na-values"></a>DI değerleri Değiştir
Bu dönüşüm NA çeşitli biçimlerdeki değiştirmek için kullanın (yok, ad, null, NaN, vb.), ya da boş dizeleri tutarlı hale getirmek için tek bir değerle değiştirmek için. Bir veya daha çok sütun bu dönüşüm destekler ve yalnızca bir sütun seçildiğinde listelenir. Hiç sütun yok seçildiğinde ana dönüştürme menüsünde mevcut değil.

## <a name="replace-missing-values"></a>Eksik değerleri Değiştir
Bu dönüşüm eksik verileri tek bir değer ile değiştirir ve bir veya daha çok sütun destekler. Bu dönüşüm, yalnızca bir sütun seçildiğinde listelenir. Hiç sütun yok seçildiğinde ana dönüştürme menüsünde mevcut değil.

## <a name="replace-error-values"></a>Hata değerlerini değiştirin
Bu dönüşüm hataları tek bir değer ile değiştirir ve bir veya daha çok sütun destekler. Bu dönüşüm, yalnızca bir sütun seçildiğinde listelenir. Hiç sütun yok seçildiğinde ana dönüştürme menüsünde mevcut değil.

## <a name="trim-string"></a>Dize Kırp
Bu dönüşüm, baştaki ve sondaki "boşluk" karakterlerini (boşluklar, sekmeler, vb. dahil) bir veya daha fazla sütun kaldırır.

## <a name="adjust-precision"></a>Duyarlık Ayarla
Bu dönüşüm, sayısal bir sütun için ondalık basamak sayısını belirler.

## <a name="rename-column"></a>Sütunu yeniden adlandırma
Bu dönüşüm, seçili sütunun adını değiştirir. Ayrıca, satır içi sütun üst bilgisindeki sütunun adını tıklatarak çağırabilirsiniz.

## <a name="remove-column"></a>Odebrat Sloupec
Seçili sütunları bu dönüşüm kaldırır ve tek bir sütun veya birçok çalışır. 

## <a name="keep-column"></a>Sütunu tut
Bu dönüşüm yalnızca seçili sütunları tutar ve tek bir sütun veya birçok çalışır.

## <a name="handle-path-column"></a>Tanıtıcı yol sütunu
Bir dosyayı içeri aktarma sırasında bir yol sütunu otomatik olarak veri kümesi tarafından eklenen **veri kaynağı Ekle** özelliği. Bu veri kümesi yolu forms tam olarak nitelenmiş dosya adını içerir. Bu dönüşüm ekler veya ek sütuna veri kümesinden kaldırır.

## <a name="convert-field-type-to-numeric"></a>Alan türünü sayısala dönüştürme
Bu dönüştürme, sütun türü sayısal değiştirir. Tamsayı olmayan veriler için ayırıcı belirtebilirsiniz. Varsayılan olarak, bu dönüştürme için ayırıcı sor değil, bu nedenle kullanın **Düzenle** Düzenleyici çağırmak için menü öğesi. Bu dönüşüm, tek bir sütun veya birçok çalışır.

## <a name="convert-field-type-to-date"></a>Alan türü Date'e Dönüştür
Bu dönüşüm sütun türünü tarih olarak değiştirir. Varsayılan tarih/saat biçimi kullanılır, ancak kullanarak kılınabilir `strftime` yönergeleri. Bir sabit tarih ile zaman değerlerini önüne ekleyin.

Varsayılan olarak, bu dönüştürme biçimi sor değil, bu nedenle kullanın **Düzenle** Düzenleyici çağırmak için sonuç adımında menü öğesi. Bu dönüşüm, tek bir sütun veya birçok çalışır.

Yalnızca, 9-22-1677 tarihleri ve tarih türü için 4-11-2262 dönüştürülebilir.

## <a name="convert-field-type-to-boolean"></a>Alan türü Boole değerine dönüştürür.
Bu dönüştürme, sütun türü true/false olarak değiştirir. Birden çok değer true veya false eşleme destekler ve bu eşlemelerin düzenleyebilirsiniz. Ayrıca, doğru/yanlış eşleme tablolarında mevcut olmayan değerleri eşleyebilirsiniz sabit destekler. Bu dönüşüm, tek bir sütun veya birçok çalışır.

## <a name="convert-field-type-to-string"></a>Alan türü dizeye Dönüştür
Bu dönüşüm sütun türü dize olarak değiştirir ve tek bir sütun veya birçok çalışır.

## <a name="convert-unix-timestamp-to-datetime"></a>Unix zaman damgası DateTime olarak dönüştürme
Verilerdeki dize olarak temsil edilir olsa bile bu dönüşüm Unix zaman damgası biçimi farkındadır. Zaman damgası veri hazırlık tarih türü doğru şekilde dönüştürür.

## <a name="filter"></a>Filtre
Bu dönüşüm, bir veya daha çok sütunlardaki değerlere göre satır filtrelemeyi destekler. "İçerir" veya "içermiyor." dizesi sütunları filtrelemek için filtre koşulları, her bir sütunun veri türüne bağlıdır. Sayısal sütunları, "büyüktür" veya "az" koşulları tarafından filtrelenebilir.

Zaman **filtre** dönüştürme, ana menüden çağrılır ve bir sütunun başlığına sağ tıklayarak alanından başka bir veri akışına başarısız olan satırları çatal seçeneği kullanılabilir. Ana veri akışı devam eder ile filtre **içinde** satırları ve yeni bir veri akışı oluşturuldu filtre uygulanmış tüm satırları içeren **kullanıma**.

## <a name="use-first-row-as-headers"></a>İlk satırı başlık olarak kullanma
Bu dönüştürme, sütun adlarının olması için veri kümesinden ilk satırını yükseltir. İlk satırın bazı sütunları veri yoksa, bir otomatik olarak oluşturulan addır. Bu dönüşüm kullanarak, verilerin içeri aktarılması satır 2 sırasında en kısa sürede başlayacağı anlamına gelir. Yapılandırmanıza bağlı olarak **Atla satırları** ayarlama, veri kümesinde alma daha da aşağı başlayabilirsiniz. Bu promosyon kaldırın ve yalnızca otomatik olarak oluşturulan ad kullanmak için de kullanabilirsiniz.

## <a name="join"></a>Birleştir
Bu dönüşüm, iki veri akışı birbirine birleştirmek için kullanılır. Sonuç olarak birleştirme işleminin hangi çıkış seçebilirsiniz: "sol" satır birleştirme veya "doğru" satır birleştirme, başarısız, başarısız birleştirme başarılı satırların.

**Birleştirme** dönüştürme bir tek veri akışından başlar ve bu veri akışı bir birleşim tarafı seçer. Daha sonra başka bir veri akışı için birleştirme diğer tarafında seçmenizi ister. İki akışı seçtikten sonra dönüşüm katılın seçilecek birleşimin her iki taraftaki tek bir sütun gerektiriyor. Birden fazla sütun birleştirme erişmesi gerekiyorsa, yalnızca birleştirme için kullanılacak yeni ve birleştirilmiş bir sütun oluşturmak için dönüştürme başlatılmadan önce türetilmiş bir sütun oluşturun. Join anahtarlar önermek ve mümkün olduğunda, türetilen sütunu otomatik olarak oluşturmak dönüştürme çalışır.

Birleştirme tamamlandıktan sonra birleşimin bir Sankey diyagram görünümü sunulur. Birbirine çizgilerin genişliğini birleştirme akışın bu bölümü taşıma satır sayısını temsil eder. Sağ tarafta Masası'nı kullanarak başarılı bir satır, satır veya satır yalnızca başarısız sağ başarısız sol seçebilirsiniz. Ayrıca, tek bir dalı seçebilirsiniz.

## <a name="append-rows"></a>Satır Ekle
Bu dönüşüm, geçerli bir başka bir veri akışı verileri ekler. Bu, yeni satırları sona eklemek için konuma göre sütunları eşler.

## <a name="append-columns"></a>Sütunlar ekleme
Bu dönüşüm, geçerli bir başka bir veri akışı yeni sütunları ekler. Sağda yeni bir sütun ekler. Verileri satır, satır çalışmaz.

## <a name="summarize"></a>Özetleme
Bu dönüşüm Toplamalar için bir veya daha fazla seçilen sütunlardaki benzersiz değerlerin birleşimini hesaplar. 

Desteklenen toplamaları olan: sayısı, Topla, MIN, MAX, ortalama, varyans ve standart sapma. Belirtilen sütun için toplamalar listesinde, veri türü için geçerli toplamalara filtrelenir. Geçerli toplamaları devre dışı bırakıldı. Örneğin, bir dize sütunu ORTALAMASINI hesaplamak mümkün değildir, ancak minimum ve maksimum işlem mümkündür.

Düzenleyici kullanılabilir olduğunda, sütun üst bilgisinden sol üstteki paneline toplanacak sütunları görüntülendiği yukarı sürükleyin. Bu panelde hiyerarşik olduğundan iç içe toplamalar bunu yapabilirsiniz. Düzenleyici bölmenin sağ üst köşedeki bir sütuna uygulamak için hangi toplama seçmek için kullanılır. Bir veya daha fazla kez tek bir sütunu toplanabilir. Sağ alt köşede kılavuz en az bir toplama seçtikten sonra veri toplama biçimde önizlemeleri sağlanır. 

Bu dönüştürme için benzer bir `ANSI-SQL GROUP BY`.

## <a name="remove-duplicates"></a>Yinelenenleri Kaldır
Bir veya daha fazla seçili olan sütunlardaki yinelenen değerler olduğunda bu dönüşüm tüm satırı kaldırır. Hiçbir sütun seçilir, yalnızca satırlar kaldırıldı olanları tüm sütun değerlerinin aynı olduğu demektir.

## <a name="sort"></a>Sırala
Bu dönüşüm verileri sıralar. Tek bir sütun veya birçok sıralama yapılabilir ve her sütun (varsayılan) artan veya azalan düzende (hangi Düzenleyicisi'nden değiştirilebilir) sıralanabilir.

Bu dönüştürme için benzer bir `ANSI-SQL ORDER BY`.

## <a name="output-transforms"></a>Çıkış dönüşümler
Aşağıdaki dönüşümlerden veri çıkışı. Tek bir akışta veri farklı noktalarda kullanıma yazmak için birden fazla yazma blokları olabilir.

### <a name="write-to-csv"></a>CSV için yazma
Bu dönüşüm veri akışı geçerli noktadaki CSV formdaki verileri dışarı yazar. Konum (yerel veya uzak) ve dosyayı geçici çeşitli ayarları kontrol eder.

### <a name="write-to-parquet"></a>Parquet yazma
Bu dönüşüm veri akışı geçerli noktadaki Parquet formdaki verileri dışarı yazar. Konum (yerel veya uzak) ve dosyayı geçici çeşitli ayarları kontrol eder.

## <a name="script-based-transforms"></a>Betik tabanlı dönüşümler
Aşağıdaki Dönüşümleri (Python) komut çekirdek ürüne eksik işlevleri gerçekleştirmek için kullanın. 

>[!NOTE]
>Bu dönüştürmeler birini kullanmadan önce okuma [kullanarak Python genişletilebilirlik](data-prep-python-extensibility-overview.md).

### <a name="add-column-script"></a>Sütun (betik) Ekle
Bu dönüşüm, bir Python ifadesi kullanarak verilere bir sütun ekler.
Daha fazla bilgi için [yeni bir sütun türetmek için örnek Python kodu](data-prep-appendix10-sample-custom-column-transforms-python.md).

### <a name="advanced-filter-script"></a>Gelişmiş Filtre (betik)
Bu dönüşüm, bir Python satır düzeyi filtresi yazmak için kullanın.
Daha fazla bilgi için [örnek filtre ifadeleri](data-prep-appendix6-sample-filter-expressions-python.md).

### <a name="transform-dataflow-script"></a>Veri akışı (betik) dönüştürme
Bu dönüşüm Python veri kümesinin tamamı için geçerlidir.
Daha fazla bilgi için [örnek dönüşüm veri akışı dönüşümleri](data-prep-appendix7-sample-transform-data-flow-python.md).

Bu dönüşüm, Python için bir tüm veri bölümü de uygulayabilirsiniz.
Daha fazla bilgi için [örnek dönüşüm veri akışı dönüşümleri](data-prep-appendix7-sample-transform-data-flow-python.md).

### <a name="write-dataflow-script"></a>Veri akışı (betik) yazma
Bu dönüşüm, bütün bir veri kümesini yazmak için Python kullanır.
Daha fazla bilgi için [örnek Python'ın yeni bir sütun türetmek için](data-prep-appendix9-sample-destination-connections-python.md).



