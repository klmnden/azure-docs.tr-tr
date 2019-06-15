---
title: Havuz dönüştürme Azure Data Factory veri akışı eşleme özelliğini ayarlama
description: Bir havuz dönüşümünde eşleme veri akışı ayarlama konusunda bilgi edinin.
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: 4341cbb0e24330d535f5211c088f0068eab33af7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65596270"
---
# <a name="sink-transformation-for-a-data-flow"></a>Dönüşüm veri akışı için havuz

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Veri akışınızı dönüştürdükten sonra bir hedef veri kümesine veri havuzu. Havuz dönüşümünde hedef çıktı verileri için bir veri kümesi tanımı seçin. Veri akışınızı gerektirdiği çok dönüşümleri havuz olabilir.

Şema değişikliklerini ve gelen verilerdeki değişiklikleri hesaba tanımlı bir şeması çıkış veri kümesinde olmayan bir klasör için çıktı verilerini havuz. Ayrıca sütun değişikliklerini kaynaklarınızı seçerek hesabının **şema değişikliklerini izin** kaynak. Ardından tüm eşleme havuzunda alanları.

![Otomatik eşleme seçeneğiyle birlikte havuz sekmesindeki seçenekler](media/data-flow/sink1.png "havuzu 1")

Tüm gelen alanları havuz için açma **otomatik eşleme**. Hedef havuz veya hedefteki alanların adlarını değiştirmek için alanları seçin için devre dışı **otomatik eşleme**. Açılacağını **eşleme** çıkış alanları eşleştirmek için sekmesinde.

![Eşleme sekmesindeki seçenekler](media/data-flow/sink2.png "2 Havuz")

## <a name="output"></a>Çıktı 
Azure Blob Depolama veya Data Lake depolama havuzu türleri için bir klasöre dönüştürülmüş verileri çıktı. Spark havuz dönüştürme kullanan bölümleme düzenini temel alarak bölümlenmiş çıkış veri dosyalarını oluşturur. 

Bölümleme düzeninden ayarlayabilirsiniz **İyileştir** sekmesi. Data Factory, çıkış tek bir dosyada birleştirmek istiyorsanız belirleyin **tek bölüm**.

![En iyi duruma getir sekmesindeki seçenekler](media/data-flow/opt001.png "havuz seçenekleri")

## <a name="field-mapping"></a>Alan eşleme

Üzerinde **eşleme** sekmesi, havuz dönüşümü, sağdaki hedeflere sol gelen sütunlarda eşleyebilirsiniz. Bir veri akışı dosyalarını havuz, Data Factory yeni dosyaları bir klasöre her zaman yazın. Bir veritabanı veri kümesine eşlediğinizde, ayarlayarak bu şemayı kullanan yeni bir tablo oluşturabilir **ilkeyi Kaydet** için **üzerine yaz**. Veya var olan bir tabloya yeni satır eklemek ve ardından mevcut şemaya alanları eşleyin. 

![Eşleşme sekmesini](media/data-flow/sink2.png "iç havuzları")

Eşleme tablosunda birden çok sütun bağlantısı, birden çok sütun delink veya birden çok satır aynı sütun adını eşleştirmek için çoklu seçim yapabilirsiniz.

Her zaman olan ve esnek bir şema tanımları, tam olarak kabul etmek için işaretleyin gelen alan kümesini hedef eşlemek için **şema değişikliklerini izin**.

![Veri kümesindeki sütunları eşlenen alanlar gösteren eşleme sekmesinde](media/data-flow/multi1.png "birden fazla seçenek")

Seçin, sütun eşlemelerini sıfırlamak için **yeniden eşlemeniz**.

![Havuz sekmesi](media/data-flow/sink1.png "bir havuz")

Seçin **doğrulama şeması** şema değişirse havuz başarısız.

Seçin **klasörünü temizleme** havuz klasörünün içeriğini hedef dosyaların hedef klasörde yazmadan önce kesemez.

## <a name="file-name-options"></a>Dosya adı seçenekleri

Dosya adlandırma yukarı ayarlayın: 

   * **Varsayılan**: Spark bölümü varsayılanlara dayanan adı dosyalarına izin verir.
   * **Desen**: Bir desen çıktı dosyalarınızı girin. Örneğin, **KREDİLERİ [n]** loans1.csv loans2.csv ve benzeri oluşturacaksınız.
   * **Bölüm başına**: Bölüm başına bir dosya adı girin.
   * **Veri sütunu olarak**: Çıkış dosyası, bir sütunun değerine ayarlayın.
   * **Tek bir dosyaya çıktı**: Bu seçenek ile ADF tek bir dosya adlı bölümlenmiş çıkış dosyalarının birleştirecek. Bu seçeneği kullanmak için veri kümenizde bir klasör adı için çözümlenmelidir. Ayrıca, lütfen bu birleştirme işlemi muhtemelen düğüm boyutu temel alınarak yük devredebildiğini dikkat edin.

> [!NOTE]
> Yürütme veri akışı etkinliği çalıştırırken operations başlangıç dosyası. Bunlar, veri akışı hata ayıklama modunda başlamaz.

## <a name="database-options"></a>Veritabanı seçenekleri

Veritabanı ayarlarını seçin:

* **Güncelleştirme yöntemi**: Eklemeleri izin vermek için varsayılandır. NET **izin INSERT** kaynağınızdan yeni satır ekleyerek durdurmak istiyorsanız. , Upsert, güncelleştirmek veya satırları silmek için önce bu eylemler için etiket satırlara alter satır dönüştürme ekleyin. 
* **Tablo yeniden**: Bırakın veya hedef tablonuz tamamlanmadan veri akışı önce oluşturun.
* **Truncate tablo**: Tamamlandığında veri akışı önce hedef tablosundan tüm satırları kaldırın.
* **Yığın boyutu**: Bir sayı demetine Yazar öbeklere girin. Büyük veri yüklerine için bu seçeneği kullanın. 
* **Hazırlamayı etkinleştirme**: PolyBase, Azure veri ambarı, havuz veri kümesi olarak yüklediğinizde kullanın.

![SQL havuz seçeneklerini gösteren Ayarlar sekmesinde,](media/data-flow/alter-row2.png "SQL seçenekleri")

> [!NOTE]
> Veri akışı hedef veritabanınızda yeni bir tablo tanımı oluşturmak için Data Factory yönlendirebilir. Tablo tanımı oluşturmak için yeni bir tablo adı olan havuz dönüşümü bir veri kümesi ayarlayın. Aşağıdaki tablo adı, SQL veri kümesi seçin **Düzenle** ve yeni bir tablo adı girin. Daha sonra havuz dönüşümünde açma **şema değişikliklerini izin**. Ayarlama **Şemayı içeri aktar** için **hiçbiri**.

![SQL veri kümesi ayarları, tablo adını düzenlemek nereye gösteren](media/data-flow/dataset2.png "SQL şeması")

> [!NOTE]
> Güncelleştirme veya veritabanı havuzunuzu satırları silmek, anahtar sütunu ayarlamanız gerekir. Bu ayar için benzersiz satırındaki veri hareketi kitaplığı (DML) belirlemek alter satır dönüştürme sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Veri akışınızı oluşturduğunuza göre eklemek bir [ardışık veri akış etkinliğini](concepts-data-flow-overview.md).
