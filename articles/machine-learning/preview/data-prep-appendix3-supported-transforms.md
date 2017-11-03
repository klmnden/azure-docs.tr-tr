---
title: "Azure Machine Learning veri hazırlık için veri dönüşümler kullanma | Microsoft Docs"
description: "Bu makalede Azure Machine Learning veri hazırlığı için kullanılabilir dönüşümleri tam bir listesini sağlar"
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
ms.openlocfilehash: 4331bb4a16d56f027dd63a97868822fa6ecc92d0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="use-data-transforms-for-data-preparation-in-azure-machine-learning"></a>Azure Machine Learning veri hazırlık için veri Dönüşümleri kullanma

A *dönüştürme* Azure Machine Learning ile verileri belirli bir biçimde kullanır, (örneğin, veri türünü değiştirme) veriler üzerinde bir işlemi gerçekleştirir ve verileri yeni biçiminde üretir. Her dönüştürme kendi arabirimi ve davranışı vardır. Verilerinizi karmaşık ve tekrarlanabilir dönüşümleri gerçekleştirmenize olanak sağlayan birkaç dönüşümler birlikte veri akışı adımlarda aracılığıyla zincir. Veri hazırlama işlevlerini çekirdek budur.

Azure Machine Learning kullanılabilir dönüşümler listesi verilmiştir. 

## <a name="column-selection"></a>Sütun Seçimi 
Aşağıdaki listelenen dönüşümler çoğunu, tek bir sütun ya da birden çok üzerinde çalışır. Birden çok sütun seçmek için kullanın **Ctrl** anahtar; veya bir dizi sütunları seçmek için kullanın **Shift** anahtarı.

## <a name="transforms-from-main-menu-andor-grid-header"></a>Ana menü ve/veya kılavuz başlığından dönüştürür 
Dönüşümler ana menüdeki dönüşümler seçeneğinden erişebilir. Dönüşümler veri kılavuzunda sütun adı sağ tıklayarak da seçilmiş olabilir. Birden çok sütun seçiliyse, bunlardan herhangi birinin sağ dönüşümler menü sağlar.

Yalnızca seçili veri türü için geçerli dönüşümler sağ tıklatma menüsünden sunulur. Ana menü tüm dönüşümler sunar, ancak seçilen sütunlara ilgili olmayan dönüşümler devre dışı bırakır.

Bağlamsal dönüşümler küçük bir kısmı bir hücre sağ tıklayarak kullanılabilir. Bu dönüşümler kopyalama, Değiştir ve filtreleyebilirsiniz. Bu dönüşümler seçenekleri sayı sütun için farklı bir dize sütunu için veri türü kullanan olduğundan.

## <a name="derive-column-by-example"></a>Sütun örneğe göre türetilen
Bu dönüştürme bir veya daha fazla var olan sütunlar türevi olarak yeni bir sütun oluşturulmasını sağlar. Dönüşümler giriş (Seçili) sütunları ve verilen örnek arar ve yeni bir sütun desire çıktısında belirler. 

Bu dönüştürme kullanmak için bir veya daha fazla sütun seçin. Yeni bir (boş) türetilen sütun örnekle ekleyin. (Diğer sütunlardan türetilen varsayılarak) türetilen sütun ve "tarafından teknolojisi sütunda diğer tüm hücrelere doldurun girişiminde örnek" görmek istediğiniz bir örnek yazın. 

Karmaşık örnekler için birden fazla örnek sağlamak gerekli olabilir. Bunu yapmak için başka bir hücre seçin ve başka bir örnek yazın.

Örnek anlamını belirlemek denemek için seçilen sütunların "tarafından örnek" teknolojisini kullanır. Bu dönüştürme çağrıldığında hiçbir sütun seçiliyse, geçerli satırın tüm hücreleri kullanılır. Yalnızca gerekli sütunları seçerek daha doğru sonuçlar verecektir.

Dönüştürme çağırmadan önce sütunları seçebilirsiniz. Dönüştürme Düzenleyicisi'ni başlatıldıktan sonra girdi olarak hangi sütunların seçildiği onay kutuları her sütunun üstünde gösterir. Ekleyebilir veya sütunları sütun başlıklarının onay kutularını kullanarak seçimden kaldırın.

Daha ayrıntılı bir açıklaması için **sütun örneği tarafından türetilmiş** yanı sıra daha fazla örnekleri, bkz: [türetilen sütun örnek başvuruya göre](data-prep-derive-column-by-example.md)  

## <a name="split-column-by-example"></a>Örneğe göre bölünmüş sütun
Varolan bir sütunla bu dönüşüm alır ve "Örnek tarafından" altyapısını kullanarak çalışır, sütuna bölmek  *n*  diğer sütunları. Sonraki oluşturulan sütunlarda otomatik bölme çalıştırmak mümkündür.

Daha ayrıntılı bir açıklaması için **tarafından bölünmüş sütun örneği** yanı sıra daha fazla örnekleri, bkz: [bölünmüş sütun örnek başvuruya göre](data-prep-split-column-by-example.md)

## <a name="expand-json"></a>JSON’ı genişletme

Bu dönüştürme, birden çok sütun geçerli JSON metin içeren bir sütun genişleterek eklemenize olanak tanır.

Daha ayrıntılı bir açıklaması için **genişletin JSON** yanı sıra daha fazla örnekleri, bkz: [genişletin JSON başvurusu](data-prep-expand-json.md)


## <a name="combine-columns-by-example"></a>Sütunları örneğe göre birleştirin

Bu dönüştürme birden çok sütun değerlerinden birleştiren yeni bir sütun eklemenizi sağlar. 

Daha ayrıntılı bir açıklaması için **birleştirmek sütunları örneğe göre** yanı sıra daha fazla örnekleri, bkz: [birleştirmek sütunları örnek başvuruya göre](data-prep-combine-columns-by-example.md)


## <a name="duplicate-column"></a>Yinelenen sütun
Bu dönüştürme, bir veya daha fazla Seçili sütunlar ve yeni bir ad özgün sütun adından türetilen verir her tam bir kopyasını oluşturur.

## <a name="text-clustering"></a>Kümeleme metin 
Bu dönüştürme tutarsız değeri aynı olması ve birlikte gruplamak yararlanacak şekilde tasarlanmıştır.  

Bu dönüştürme kullandığınızda, tek bir sütundaki değerlerin benzerlik için analiz ve kümeler halinde gruplandırılır. Her küme için kümedeki tüm örneklerini değiştirir değeri ve örnek değerleri bir kurallı değer yoktur. Tam küme kaldırılabilir ve kurallı değer düzenlenebilir. Belirli bir kümeden örnekleri kaldırılabilir. Ayrıca, Grup örnekleri bir küme içine kullandı benzerlik puan eşik filtre değiştirilebilir.

Varsayılan olarak bu dönüşüm yeni değerleri içeren yeni bir sütun oluşturma, bu küme için kurallı değerine sahip tüm küme örneği değerleri değiştirir. Her örneği için benzerlik puanı (adlandırılabilir) yeni bir sütun için daha sonra veri akışında kullanılmasına izin eklemek mümkündür.

## <a name="replace-values"></a>Değerleri değiştirin
Bu dönüştürme başkası tarafından değiştirilmesi bir dize verir. Kaynak dizesi kısmi dize veya tam bir hücre olabilir; değiştirme, tek bir sütun veya birçok uygulayabilirsiniz. Arama dizesi normal karakter yanı sıra özel karakterleri aramayı destekler. 

## <a name="replace-na-values"></a>BELİRTİLMEYEN değerlerini değiştirin
Bu dönüştürme NA çeşitli farklı biçimlerdeki sağlar (yok, ad, null, NaN, vb.) veya boş dizeler tutarlı hale getirmek için tek bir değer ile değiştirilir. Bu dönüştürme, bir veya daha çok sütun destekler. Bu dönüştürme, bir sütun seçildiğinde ve hiç sütun yok seçildiğinde ana dönüştürme menüsünden mevcut değil yalnızca listelenir.

## <a name="replace-missing-values"></a>Eksik değerleri değiştirin
Bu dönüşüm eksik verileri tek bir değeri ile değiştirir. Bu dönüştürme, bir veya daha çok sütun destekler. Bu dönüştürme, bir sütun seçildiğinde ve hiç sütun yok seçildiğinde ana dönüştürme menüsünden mevcut değil yalnızca listelenir.

## <a name="replace-error-values"></a>Hata değerlerini değiştirme
Bu dönüştürme hataları tek bir değeri ile değiştirir. Bu dönüştürme, bir veya daha çok sütun destekler. Bu dönüştürme, bir sütun seçildiğinde ve hiç sütun yok seçildiğinde ana dönüştürme menüsünden mevcut değil yalnızca listelenir.

## <a name="trim-string"></a>Dize kırpma
Bu dönüştürme baştaki ve sondaki "boşluk" karakterleri kaldırır (boşluk, sekme, içeren *vb.*), bir veya daha fazla sütunlarından.

## <a name="adjust-precision"></a>Duyarlık Ayarla
Bu dönüştürme, sayısal bir sütun için ondalık basamak sayısını ayarlamanızı sağlar.

## <a name="rename-column"></a>Sütunu yeniden adlandırma
Bu dönüştürme seçili sütunun adını değiştirir. Bu dönüştürme, sütun adına tıklayarak çağrılan satır içinde sütun başlığına da olabilir.

## <a name="remove-column"></a>Sütun Kaldır
Bu dönüştürme seçili sütunların kaldırır. Bu dönüştürme, tek bir sütun veya birçok çalışır. 

## <a name="keep-column"></a>Sütunun koruyun
Bu dönüşüm yalnızca seçili sütunların tutar. Bu dönüştürme, tek bir sütun veya birçok çalışır.

## <a name="handle-path-column"></a>İşleyici yolu sütun
Bir dosyayı içeri aktarma sırasında yolu sütun kümesine veri kaynağı Ekle Sihirbazı tarafından otomatik olarak eklenir. Veri kümesi yoluna forms tam dosya adını içerir. Bu dönüştürme ekler veya bu ek sütun kümesinden kaldırır.

## <a name="convert-field-type-to-numeric"></a>Alan türü sayısal Dönüştür
Bu dönüştürme için sayısal sütun türünü değiştirir. Tamsayı olmayan veri ise, ayırıcı belirtebilirsiniz. Varsayılan olarak, bu dönüştürme için ayırıcı isteminde değil, kullanın **Düzenle** Düzenleyicisi çağrılacak menü öğesi. Bu dönüştürme, tek bir sütun veya birçok çalışır.

## <a name="convert-field-type-to-date"></a>Alan türü Date olarak Dönüştür
Bu dönüştürme sütun türü date olarak değiştirir. Varsayılan tarih/saat biçimi kullanılır, ancak kullanılarak geçersiz kılınabilir `strftime` yönergeleri. Saat değerleri bir sabit tarihi ile önüne eklediğinizden mümkündür.

Bu dönüştürme için biçim sor değil varsayılan olarak, **Düzenle** Düzenleyicisi çağırmak için sonuç adımında menü öğesi. Bu dönüştürme, tek bir sütun veya birçok çalışır.

Yalnızca, 9-22-1677 tarihleri ve 4 11 2262 tarih türüne dönüştürülebilir.

## <a name="convert-field-type-to-boolean"></a>Alan türü Boolean değerine dönüştürme
Bu dönüştürme sütun türü true/false olarak değiştirir. Birden çok değer true veya false eşleme, bu dönüştürme destekler ve bu eşlemeleri düzenleme mümkündür. Ayrıca doğru/yanlış eşleme tablolarda yok değerleri eşleyebilirsiniz sabiti destekler. Bu dönüştürme, tek bir sütun veya birçok çalışır.

## <a name="convert-field-type-to-string"></a>Alan türü dizeye dönüştürme
Bu dönüştürme sütun türü dize olarak değiştirir. Bu dönüştürme, tek bir sütun veya birçok çalışır.

## <a name="convert-unix-timestamp-to-datetime"></a>UNIX zaman damgası DateTime olarak dönüştürme
Verilerdeki dize olarak temsil edilir ve doğru veri hazırlığı içinde tarih türüne dönüştürür olsa bile bu dönüşüm UNIX zaman damgası biçimi bilir.

## <a name="filter"></a>Filtre
Bu dönüştürme, bir veya daha çok sütunlardaki değerlere göre satır filtreleme destekler. Filtre koşulları için filtre dizesini sütunları "içerir" veya "içeremez" olası kolaylaştırarak, her bir sütunun veri türünü bağlıdır. Sayısal sütunlar "büyüktür" tarafından filtre "değerinden küçük" koşulları.

Filtre dönüştürme ana menüden veya bir sütun başlığına sağ tıklayarak çağrıldığında, başarısız olan satırları başka bir veri akışına çatallaştırma seçeneği kullanılabilir. Ana veri akışının filtrelenmiş ile devam eder sonra **içinde** satır ve yeni bir veri akışı oluşturulur filtre uygulanmış tüm satırları içeren **çıkışı**.

## <a name="use-first-row-as-headers"></a>İlk satırı başlığı olarak kullan
Bu dönüştürme ilk satırı sütun adlarının olması için veri kümesinden yükseltir. Bazı sütunları verilerin ilk satırında yoksa, otomatik olarak oluşturulan bir adı değil. Bu dönüşüm kullanarak veri aktarımını 2 satırındaki en kısa zamanda başladığını anlamına gelir. Atla satırları ayar bağlı olarak, alma veri kümesindeki bile aşağıya başlayabilir. Ayrıca yükseltme kaldırmak için ve yalnızca adlarını otomatik olarak oluşturulan sağlamak için kullanılabilir.

## <a name="join"></a>Birleştir
Bu dönüştürme, iki veri akışı birlikte eklemek için kullanılır. Hangi çıktısı birleştirmenin sonucu olmalıdır seçebilirsiniz: başarılı satırları birleştirme, "sol" katılma veya "sağ" Katıl satırları başarısız olan satırları başarısız.

Birleştirme Sihirbazı'nı tek bir veri akışından başlatılır ve bu veri akışı katılma sütunlardan seçer. Ardından katılma diğer taraf için başka bir veri akışı için ister. İki akışı seçtikten sonra sihirbaz her katılım için seçilecek Eklem tarafında tek bir sütun gerektirir. Birleşim birden fazla sütun gerekirse, yalnızca birleştirme için kullanılacak yeni bir (birleştirilmiş) sütun oluşturmak için Sihirbazı'nı başlatmadan önce türetilmiş bir sütun oluşturun. Birleşim anahtarları önermek ve mümkünse türetilmiş sütunları otomatik olarak oluşturmak Sihirbazı çalışır.

Birleştirme tamamlandığında Sankey diyagram görünümü katılma sunulur. Birbirine çizgilerin genişliğini birleştirme akış kısmı taşıma satır sayısını temsil eder. Sağdaki panelde başarılı satırları, başarısız olan sola veya özel olarak başarısız sağa seçmenize olanak sağlar. Yalnızca bir dal seçmek mümkündür.

## <a name="append-rows"></a>Satır Ekle
Bu dönüştürme için geçerli bir başka bir veri akışından veri ekler. Sütunları sonunda yeni satır eklemek için konumuna göre eşler.

## <a name="append-columns"></a>Sütunlar ekleme
Bu dönüştürme başka bir veri akışı yeni sütunlarından geçerli bir ekler. Yeni sütunlar sağa ekler; veri satırı hizalamak girişimi değil.

## <a name="summarize"></a>Özetle
Bu dönüştürme için bir veya daha fazla seçili sütunlardaki benzersiz değerlerin birleşimi toplamalar hesaplar. 

Desteklenen toplamalar şunlardır: sayısı, Topla, MIN, MAX, ortalama, SAPMASI ve standart sapma. Veri türü için geçerli olanlara Toplamalar için belirli bir sütun listesi filtrelenir. Geçerli toplamalar devre dışı bırakılır. Örneğin, bir dize sütunu ortalaması işlem mümkün değildir, ancak bu işlem min ve max mümkündür.

Düzenleyici kullanılabilir olduğunda, sütun başlığından paneline sol üst üzerinde yukarı sürükleyin. Toplanacak sütunları burada görüntülenir. Bu panoyu hiyerarşik olduğundan iç içe toplamalar yapmak da mümkündür. Sağ üstteki Düzenleyicisi Masası bir sütuna uygulamak için hangi toplama seçmek için kullanılır. Tek bir sütun bir veya birden çok kez kümelenebilir. En az bir toplama seçtikten sonra sağ alt Izgarayı veri toplama biçimde önizlemeleri sağlanır. 

Bu dönüştürme için benzer bir `ANSI-SQL GROUP BY`.

## <a name="remove-duplicates"></a>Yinelenenleri kaldırma
Bu dönüştürme bulunduğu yinelenen değerler birinde veya daha fazla sütun seçili tüm satırı kaldırır. Hiçbir sütun seçilirse, sonra kaldırılan yalnızca satırları olanları tüm sütun değerlerinin aynı olduğu edilir.

## <a name="sort"></a>Sırala
Bu dönüştürme verileri sıralar. Tek bir sütun veya birçok sıralama yapılabilir ve her sütunu (varsayılan) artan veya azalan düzende sıralanır (Düzenleyicisi'nden değiştirilebilir).

Bu dönüştürme için benzer bir `ANSI-SQL ORDER BY`.

## <a name="output-transforms"></a>Çıktı dönüşümler
Aşağıdaki dönüşümler veri çıkışı. Verileri farklı noktalarda dışarıda yazabilmesi için tek bir akış içinde birden çok yazma blokları olması mümkündür.

### <a name="write-to-csv"></a>CSV'ye yazma
Bu dönüştürme veri akışında geçerli noktasından CSV formundaki verileri çıkışı yazar. Dosyanın geçici bir çözüm denetimi (yerel veya uzak) konumun ve çeşitli ayarları sağlar.

### <a name="write-to-parquet"></a>Parquet için yazma
Bu dönüştürme veri akışında geçerli noktasından Parquet formundaki verileri çıkışı yazar. Dosyanın geçici bir çözüm denetimi (yerel veya uzak) konumun ve çeşitli ayarları sağlar.

## <a name="script-based-transforms"></a>Betik tabanlı dönüşümler
Aşağıdaki dönüşümler çekirdek ürüne eksik işlevleri gerçekleştirmek için komut dosyası (Python) kullanın. Bu dönüşümler birini kullanmadan önce okumanız [kullanarak Python genişletilebilirlik](data-prep-python-extensibility-overview.md).

### <a name="add-column-script"></a>Sütun (komut) ekleyin
Bu dönüştürme Python ifade kullanarak verileri eklenecek sütunun sağlar.
Daha fazla bilgi için bkz: [yeni sütunlar türetme için örnek Python kodu](data-prep-appendix10-sample-custom-column-transforms-python.md).

### <a name="advanced-filter-script"></a>Gelişmiş Filtre (komut)
Bu dönüştürme yazılacak bir Python satır düzeyi filtre izin verir.
Daha fazla bilgi için bkz: [örnek filtre ifadeleri](data-prep-appendix6-sample-filter-expressions-python.md).

### <a name="transform-dataflow-script"></a>Veri akışı (komut) dönüştürme
Bu dönüştürme Python tüm veri kümesinin uygulanmasına izin verir.
Daha fazla bilgi için bkz: [örnek dönüştürme veri akışını dönüşümleri](data-prep-appendix7-sample-transform-data-flow-python.md).

### <a name="transform-dataflow-script"></a>Veri akışı (komut) dönüştürme
Bu dönüştürme için tüm veri bölümü uygulanacak Python sağlar.
Daha fazla bilgi için bkz: [örnek dönüştürme veri akışını dönüşümleri](data-prep-appendix7-sample-transform-data-flow-python.md).

### <a name="write-dataflow-script"></a>Veri akışı (komut) yazma
Bu dönüştürme Python yazma ve tüm veri kümesinin uygulanmasına izin verir.
Daha fazla bilgi için bkz: [yeni sütunlar türetme için örnek Python](data-prep-appendix9-sample-destination-connections-python.md).



