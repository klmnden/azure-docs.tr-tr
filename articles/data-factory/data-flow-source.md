---
title: Kaynak dönüştürme Azure Data Factory veri akışı eşleme özelliğini ayarlama
description: Eşleme veri akışı kaynak Dönüşümde ayarlama konusunda bilgi edinin.
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/12/2019
ms.openlocfilehash: 51c1ea7b554178f7fb3f264bf731ffd5872ceea2
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66234556"
---
# <a name="source-transformation-for-mapping-data-flow"></a>Eşleme veri akışı kaynak dönüşümü 

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Bir kaynak dönüşüm veri akışı için veri kaynağı yapılandırır. Birden fazla kaynak dönüşüm veri akışı içerebilir. Her zaman veri tasarlama akışlarınız çalıştığında, bir kaynak dönüştürme ile başlar.

Her veri akışı en az bir kaynak dönüştürme gerektiriyor. Kadar kaynakları, veri Bağlantılarınızdaki tamamlamak için gerektiği gibi ekleyin. Bu kaynakları bir birleşim dönüştürme veya birleşim dönüştürme birlikte katılabilirsiniz.

> [!NOTE]
> Veri akışınızı hata ayıklaması yaparken, veriler kaynaktan örnekleme ayarı veya hata ayıklama kaynak sınırları kullanarak okunur. Bir havuz için veri yazmak için bir işlem hattını veri akış etkinliğini veri akışınız çalıştırmanız gerekir. 

![Dönüştürme seçenekleri kaynak ayarlar sekmesindeki kaynak](media/data-flow/source.png "kaynak")

Veri akışı kaynak dönüşümünüzü tam olarak bir Data Factory veri kümesi ile ilişkilendirin. Veri kümesi, yazma veya okuma istediğiniz veri konumunu ve şekli tanımlar. Aynı anda birden fazla dosyayla çalışmak için kaynak kodunuzda joker karakterler ve dosya listeleri kullanın.

## <a name="data-flow-staging-areas"></a>Veri akışı hazırlama alanları

Veri akışı ile birlikte çalışır *hazırlama* Azure'a tüm olan veri kümeleri. Bu veri kümeleri, verilerinizi dönüştürürken hazırlama için kullanın. 

Veri Fabrikası yaklaşık 80 yerel bağlayıcıları erişebilir. Veri akışınızı bu diğer kaynaklardan verileri dahil etmek için bu verileri bir veri akışı veri kümesi hazırlama alanları hazırlamak için kopyalama etkinliği aracını kullanın.

## <a name="options"></a>Seçenekler

Verileriniz için şema ve örnekleme Seçenekleri'ni seçin.

### <a name="allow-schema-drift"></a>Şema değişikliklerini izin ver
Seçin **şema değişikliklerini izin** kaynak sütunları genellikle değiştirirseniz. Bu ayar, tüm gelen kaynak alanları havuz dönüşümleri akışına sağlar.

### <a name="validate-schema"></a>Şema doğrulama

Tanımlı bir şeması kaynak verilerin gelen sürüm eşleşmiyorsa, veri akışı çalıştırmak başarısız olur.

![Doğrulama şeması, şema değişikliklerini izin ver ve örnekleme seçeneklerini gösteren genel kaynak ayarlarını](media/data-flow/source1.png "genel kaynağı 1")

### <a name="sample-the-data"></a>Örnek veriler
Etkinleştirme **örnekleme** kaynağınızdan alınan satır sayısını sınırlamak için. Test veya hata ayıklama amacıyla kaynağınızdan alınan veri örneği, bu ayarı kullanın.

## <a name="define-schema"></a>Şema tanımlayın

Kaynak dosyalarını (örneğin, düz dosyalar yerine için Parquet dosyalarını) kesin değil, kaynak dönüşümünde burada her bir alan için veri türlerini tanımlayın.  

![Kaynağı tanımlama şema sekmesinde dönüştürme ayarları](media/data-flow/source2.png "kaynak 2")

Bir select dönüştürme sütun adları daha sonra değiştirebilirsiniz. Bir türetilmiş sütun dönüşümü, veri türlerini değiştirmek için kullanın. Kesin olarak belirlenmiş kaynaklar için daha sonra seçin dönüştürme veri türlerini değiştirebilirsiniz. 

![Veri türleri seçme dönüşümünde](media/data-flow/source003.png "veri türleri")

### <a name="optimize-the-source-transformation"></a>Kaynak dönüşümü en iyi duruma getirme

Üzerinde **İyileştir** kaynak dönüştürme için sekmesinde görebileceğiniz bir **kaynak** lüm türü. Bu seçenek, yalnızca Azure SQL veritabanı kaynağınızın olduğunda kullanılabilir. Data Factory bağlantıları SQL veritabanı kaynağınızın karşı büyük sorguları çalıştırmak için paralel hale getirmeyi denediğinden budur.

![Kaynak bölüm ayarları](media/data-flow/sourcepart2.png "bölümleme")

Verileri bölümlemek için SQL veritabanı kaynağınız yoksa, ancak bölümleri büyük sorgular için kullanışlıdır. Bir sütun veya bir sorgu bölümünüz temel alabilir.

### <a name="use-a-column-to-partition-data"></a>Sütun bölümü veri kullanın

Kaynak tablonuzdan bölüme bir sütun seçin. Ayrıca en fazla bağlantı sayısını ayarlayın.

### <a name="use-a-query-to-partition-data"></a>Verileri bölümleme için bir sorgu kullanın

Bölüm bir sorguya dayalı bağlantılar seçebilirsiniz. WHERE koşulu içeriğini girmeniz yeterlidir. Örneğin, Yıl > 1980 girin.

## <a name="source-file-management"></a>Kaynak dosya yönetimi

Kaynak dosyaları yönetmek için Ayarlar'ı seçin. 

![Yeni kaynak ayarları](media/data-flow/source2.png "yeni ayarlar")

* **Joker karakter yolu**: Kaynak klasörünüzden bir desenle eşleşen dosyaları bir dizi seçin. Bu ayar, herhangi bir dosyada, veri kümesi tanımı geçersiz kılar.
* **Dosyaların listesini**: Bu dosya kümesidir. İşlemek için göreli yol dosyaların listesini içeren bir metin dosyası oluşturun. Bu metin dosyasını göstermek.
* **Dosya adı depolamak için sütun**: Kaynak dosyanın adı, bir sütunda, verilerinizi Store. Dosya adı dizesi depolamak için yeni bir ad girin.
* **Tamamlandıktan sonra**: Veri akış çalıştırmaları, kaynak dosyayı silin veya kaynak dosyayı taşıma sonra kaynak dosya ile yapmamak seçin. Göreli taşımak yollardır.

### <a name="sql-datasets"></a>SQL veri kümeleri

Kaynağınızda SQL veritabanı veya SQL veri ambarı ise, kaynak dosya yönetimi için ek seçenekleri vardır.

* **Sorgu**: Kaynağınız için bir SQL sorgusunu girin. Bu ayar kümesinde seçtiğiniz herhangi bir tabloda geçersiz kılar. Unutmayın **Order By** yan tümceleri burada desteklenmez, ancak tam bir SELECT FROM deyimi ayarlayabilirsiniz. Kullanıcı tanımlı tablo işlevleri de kullanabilirsiniz. **seçin * udfGetData() gelen** olduğu bir UDF SQL'de bir tablo döndürür. Bu sorgu, veri akışında kullanabileceğiniz bir kaynak tablo oluşturur.
* **Yığın boyutu**: Okur ile büyük verileri öbek için bir toplu iş boyutu girin.

> [!NOTE]
> Dosya işlemleri, yalnızca yürütme veri akışı etkinliği kullanan bir işlem hattı (işlem hattı hata ayıklama veya yürütme Çalıştır) bir işlem hattı çalıştırması'ndan veri akışı başladığında çalıştırın. Dosya işlemleri *olmayan* veri akışı hata ayıklama modunda çalıştırın.

### <a name="projection"></a>Yansıtma

Veri kümelerinde şemalar gibi veri sütunları, türlerini ve biçimlerini veriler kaynak veri kaynağındaki projeksiyon tanımlar. 

![Projeksiyon sekmesindeki ayarlar](media/data-flow/source3.png "projeksiyonu")

Metin dosyanızı tanımlı bir şeması varsa, seçin **veri türünü Algıla** Data Factory örneği ve veri türleri çıkarılamıyor. Seçin **tanımla varsayılan biçimi** otomatik algılama için varsayılan verileri biçimlendirir. 

Daha sonra türetilmiş sütun dönüştürme sütun veri türlerini değiştirebilirsiniz. Bir select dönüştürme, sütun adlarını değiştirmek için kullanın.

![Varsayılan veri biçimleri için ayarları](media/data-flow/source2.png "varsayılan biçimleri")

## <a name="next-steps"></a>Sonraki adımlar

Yapı başlamak bir [türetilmiş sütun dönüştürme](data-flow-derived-column.md) ve [seçin dönüştürme](data-flow-select.md).
