---
title: Azure veri fabrikası veri akışı kaynak dönüştürme eşlemesi
description: Azure veri fabrikası veri akışı kaynak dönüştürme eşlemesi
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/12/2019
ms.openlocfilehash: 54302f97913fd01dc8f8e4a8d987a407c8bdf9a7
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58369182"
---
# <a name="mapping-data-flow-source-transformation"></a>Veri akışı kaynak dönüştürme eşlemesi

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Kaynak dönüşümü, veri akışınız verileri getirmek için kullanmak istediğiniz bir veri kaynağı yapılandırır. Tek bir akış veri birden fazla kaynak dönüştürme olabilir. Her zaman, veri akışları ile bir kaynak dönüştürme tasarlama başlar.

> [!NOTE]
> Her veri akışı en az bir kaynak dönüştürme gerektiriyor. Veri Bağlantılarınızdaki tamamlamak için ihtiyaç duyduğunuz kadar ek kaynakları ekleyin. Bu kaynakları JOIN veya birleşim dönüştürme birlikte katılabilirsiniz. Hata ayıklama oturumlarında, veri akışı hata ayıklaması yaparken veri örnekleme ayarı veya hata ayıklama kaynak sınırları kullanarak kaynağından okur. Ancak, bir işlem hattı veri akışı etkinliği, veri akışını yürütecek kadar havuza veri yazılır. 

![Dönüştürme seçenekleri kaynak](media/data-flow/source.png "kaynak")

Her veri akışı kaynak dönüştürme ile tam olarak bir Data Factory veri kümesi ilişkilendirilmelidir. Veri kümesini, verilerinizin okuma veya yazma konumunu ve şekli tanımlar. Aynı anda birden fazla dosyayla çalışmak için kaynak kodunuzda joker karakterler ve dosya listeleri kullanabilir dosya kaynakları kullanırken.

## <a name="data-flow-staging-areas"></a>Hazırlama alanları veri akışı

Veri akışı, tüm Azure üzerinde olan "Hazırlama" veri kümeleri ile birlikte çalışır. Bu veri akışı veri kümeleri için verileri hazırlama, veri dönüşümlerini gerçekleştirmek için kullanılır. Veri Fabrikası yaklaşık 80 farklı yerel bağlayıcıları erişebilir. Bu diğer kaynaklardan verileri, veri akışı dahil etmek için önce bu verileri bu veri akışı veri kümesi hazırlama alanların birini kopyalama etkinliği'ni kullanarak hazırlayın.

## <a name="options"></a>Seçenekler

### <a name="allow-schema-drift"></a>Şema değişikliklerini izin ver
Kaynak sütunları genellikle değiştirirseniz, şema değişikliklerini izin seçin. Bu ayar, kaynak havuzuna dönüştürmeleri akışına gelen tüm alanları izin verir.

### <a name="validate-schema"></a>Şema doğrulama

![Genel kaynak](media/data-flow/source1.png "genel kaynağı 1")

Gelen sürümü kaynak veriler, tanımlı bir şeması eşleşmiyorsa veri akışının yürütme başarısız olur.

### <a name="sampling"></a>Örnekleme
Örnekleme kaynağınızdan alınan satır sayısını sınırlamak için kullanın.  Bu test veya hata ayıklama amacıyla kaynağınızdan veri örnekleme olduğunda yararlıdır.

## <a name="define-schema"></a>Şema tanımlayın

![Dönüştürme kaynağı](media/data-flow/source2.png "kaynak 2")

Kesin türü belirtilmiş (yani düz dosyalar Parquet dosyalarını aksine) olmayan bir kaynak dosya türleri için kaynak dönüşümünde burada her bir alan için veri türlerini tanımlamanız gerekir. Daha sonra bir Select dönüştürme sütun adları ve veri türleri, bir sütunu türetilmiş dönüşümünde da değiştirebilirsiniz. 

![Dönüştürme kaynağı](media/data-flow/source003.png "veri türleri")

Kesin olarak belirlenmiş kaynakları için bir sonraki Select dönüştürme veri türlerini değiştirebilirsiniz. 

### <a name="optimize"></a>İyileştirme

![Kaynak bölümler](media/data-flow/sourcepart.png "bölümleme")

Bir kaynak dönüştürme için İyileştir sekmesinde, "Kaynak" adlı ek bir bölümleme türü görürsünüz. Bu yalnızca açık kaynak Azure SQL DB seçtiğinizde yukarı. ADF bağlantılar, Azure SQL DB kaynağına karşı büyük sorgularını yürütmek için paralel hale getirmek istediğiniz olmasıdır.

SQL DB kaynak verileri bölümleme isteğe bağlıdır, ancak büyük sorgular için yararlıdır. İki seçeneğiniz vardır:

### <a name="column"></a>Sütun

Bir sütun bölüm için kaynak tablosundan seçin. Maksimum bağlantı sayısını de ayarlamanız gerekir.

### <a name="query-condition"></a>Sorgu koşulu

İsteğe bağlı bir sorgu üzerindeki bağlantıları bölümlemek seçebilirsiniz. Bu seçenek için WHERE koşulu içerikte basitçe. I.e. year > 1980

## <a name="source-file-management"></a>Kaynak dosya yönetimi
![Yeni kaynak ayarları](media/data-flow/source2.png "yeni ayarlar")

* Bir desenle eşleşen dosyaları kaynak klasöründen bir dizi seçmek için joker karakter yolu. Bu, veri kümesi tanımında, ayarladığınız herhangi bir dosyayı geçersiz kılar.
* Dosyaların listesi. Bir dosya kümesi ile aynıdır. İşlemek için göreli yol dosyaların listesini ile oluşturduğunuz bir metin dosyasına işaret.
* Dosya adı depolamak için sütun verilerinizi sütununda kaynak dosyasının adını depolar. Dosya adı dizesi depolamak için yeni bir ad girin.
* Tamamlandıktan sonra (veri akışı yürütüldükten sonra kaynak dosya ile hiçbir şey yapma, kaynak dosyaları silin veya kaynak dosyalarını taşımak için seçebilirsiniz. Taşıma için göreli yolların yollardır.

### <a name="sql-datasets"></a>SQL veri kümeleri

Azure SQL DB veya Azure SQL DW, kaynağı olarak kullanırken, ek seçenekler gerekir.

* Sorgu: Kaynağınız için bir SQL sorgusunu girin. Bir sorgu ayarlama kümesinde seçtiğiniz herhangi bir tabloda geçersiz kılar. Order By yan tümcesi burada desteklenmediğini unutmayın. Ancak, tam bir SELECT FROM deyimi Burada ayarlanan olabilir.

* Toplu iş boyutu: Toplu iş boyutu okur ile büyük verileri öbek için bir toplu iş boyutu girin.

> [!NOTE]
> Veri akışı işlem hattı çalıştırmasını (işlem hattı hata ayıklama veya Çalıştır yürütme) yürütüldüğünde dosya işlemi ayarları yalnızca yürütecek bir işlem hattı yürütme veri akışı etkinliği kullanarak. Dosya işlemleri, veri akışı hata ayıklama modunda yürütün değil.

### <a name="projection"></a>Yansıtma

![Projeksiyon](media/data-flow/source3.png "projeksiyonu")

Benzer şemalara veri kümelerinde, veri sütunları, veri türleri ve veri biçimleri veriler kaynak veri kaynağındaki projeksiyon tanımlar. Bir metin dosyasında tanımlı bir şeması varsa, "Veri türü örneği ve veri türlerini çıkarması denemek için ADF sormak algıla" tıklayın. Varsayılan veri ayarlayabilirsiniz biçimleri için otomatik algıla "Varsayılan biçim tanımlama" düğmesini kullanarak. Bir sonraki sütun türetilmiş dönüştürme sütun veri türlerini değiştirebilirsiniz. Sütun adlarını seçme dönüştürme kullanılarak değiştirilebilir.

![Varsayılan biçimleri](media/data-flow/source2.png "varsayılan biçimleri")

## <a name="next-steps"></a>Sonraki adımlar

Veri dönüşümünüzü oluşturmaya başlamak [türetilmiş sütun](data-flow-derived-column.md) ve [seçin](data-flow-select.md).
