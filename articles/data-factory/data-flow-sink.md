---
title: Azure veri fabrikası veri akışı havuz dönüştürme eşlemesi
description: Azure veri fabrikası veri akışı havuz dönüştürme eşlemesi
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: dba043721c2d81b7fe2c254f62328e54bb959cdc
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2019
ms.locfileid: "56729381"
---
# <a name="mapping-data-flow-sink-transformation"></a>Veri akışı havuz dönüştürme eşlemesi

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

![Havuz seçenekleri](media/data-flow/windows1.png "havuzu 1")

Veri akışı dönüşümünüzü tamamlandığında, bir hedef veri kümesine dönüştürülmüş veri havuzu. Havuz dönüşümünde hedef çıktı verileri için kullanmak istediğiniz veri kümesi tanımı seçebilirsiniz. Veri akışınızı gerektirdiği sayıda havuz dönüştürme olabilir.

Bir ortak gelen verileri değiştirmek için hesap ve şema kayması için hesap tanımlı bir şeması çıkış veri kümesinde olmayan bir klasör için çıktı verilerini havuz uygulamadır. Ayrıca tüm sütun değişikliklerini kaynaklarınızı "İzin ver şema değişikliklerini" kaynak ve sonra eşleme seçerek havuz içindeki tüm alanlarda hesabının.

Üzerine, ekleme veya bir veri kümesine yönelik veri akışı başarısız seçebilirsiniz.

Tüm gelen alanları havuz "eşleme" de seçebilirsiniz. Hedef havuz istediğiniz alanları seçin istiyorsanız veya hedefteki alanların adlarını değiştirmek istiyorsanız, "eşleme" için "Kapat"'i seçin ve sonra çıktı alanlarını eşlemek için eşleme sekmesine tıklayın:

![Havuz seçenekleri](media/data-flow/sink2.png "2 Havuz")

## <a name="output-to-one-file"></a>Çıktıyı bir dosyaya
Azure depolama blobu veya Data Lake havuz türlerinde bir klasöre dönüştürülmüş verileri çıkarır. Spark, kullanılan bölümleme düzenini temel alarak bölümlenmiş çıkış veri dosyaları oluşturacağını havuz Dönüştür. Bölümleme düzeni "En iyi duruma getir" sekmesinde tıklayarak ayarlayabilirsiniz. Çıkış tek bir dosyada birleştirmek için ADF esnetmek istiyorsanız "Tek bölüm" radyo düğmesine tıklayın.

![Havuz seçenekleri](media/data-flow/opt001.png "havuz seçenekleri")

### <a name="output-settings"></a>Çıkış ayarları

Üzerine yazma varsa tabloyu kesmek, ardından yeniden oluşturun ve verileri yükleme. Ekleme yeni satır ekleyin. Veri kümesini tablo adını tabloda hiç ADW hedefte mevcut değilse, veri akışı tablosu oluşturun ve sonra veri yükleme.

"Otomatik eşleme" işaretini kaldırırsanız, hedef tablonuz alanları el ile eşleyebilirsiniz.

![Havuz ADW seçenekleri](media/data-flow/adw2.png "adw havuz")

#### <a name="field-mapping"></a>Alan eşleme

Havuz dönüşümünüzü eşleme sekmesinde hedefe (sağ taraf) gelen (sol taraf) sütun eşleyebilirsiniz. Bir veri akışı dosyalarını havuz, ADF yeni dosyaları bir klasöre her zaman yazın. Veritabanı veri kümesine eşlediğinizde, ya da bu şema ("üzerine yazmak için" ilke kaydetme ayarlanır) ile yeni bir tablo oluşturmak seçebilir veya mevcut bir yeni satır Ekle tablo ve mevcut şemaya alanları eşleyin.

Tek tıklamayla birden çok sütun bağlantısı, birden çok sütun Delink veya birden çok satır aynı sütun adını eşleştirmek için eşleme tablosunda çoklu seçim kullanabilirsiniz.

![Alan eşleme](media/data-flow/multi1.png "birden fazla seçenek")

Sütunları eşlemelerinizi sıfırlamak istiyorsanız, eşlemeleri sıfırlamak için "Yeniden eşleme" düğmesine basın.

![Bağlantıları](media/data-flow/maxcon.png "bağlantıları")

### <a name="updates-to-sink-transformation-for-adf-v2-ga-version"></a>ADF V2 genel kullanım sürümü için dönüştürme havuz güncelleştirmeleri

![Havuz seçenekleri](media/data-flow/sink1.png "bir havuz")

![Havuz seçenekleri](media/data-flow/sink2.png "iç havuzları")

* Şema değişikliklerini ve şema doğrulama seçenekleri artık havuzunda kullanılabilir izin verir. Bu, esnek şema tanımlarını (şema değişikliklerini) kabul edin veya tamamen (doğrulama şema) şema değişirse havuz başarısız için ADF bildirmesine olanak tanır.

* Klasör temizleyin. ADF havuz klasör içeriğini hedef dosyaların hedef klasörde yazmadan önce keser.

* Dosya adı seçenekleri

   * Varsayılan: Spark bölümü varsayılanlara dayanan adı dosyalara izin ver
   * Desen: Çıkış dosyaları için bir ad girin
   * Bölüm başına: Bölüm başına bir dosya adı girin
   * Sütundaki verilerin: Çıkış dosyası bir sütunun değerine ayarlayın.

> [!NOTE]
> Yürütme veri akışı etkinliği olmayan modda veri akışı hata ayıklama çalıştırırken dosya işlemleri yalnızca yürütme

SQL havuz türleriyle ayarlayabilirsiniz:

* Tablo Kes
* (Açılan/oluşturma gerçekleştirir) tabloyu yeniden oluşturun
* Büyük veri için toplu iş boyutu yükler. Bir sayı demetine Yazar öbeklere girin.

![Alan eşleme](media/data-flow/sql001.png "SQL seçenekleri")

## <a name="next-steps"></a>Sonraki adımlar

Veri akışınızı oluşturduğunuza göre eklemek bir [yürütme veri akışı etkinliği ardışık](https://docs.microsoft.com/azure/data-factory/concepts-data-flow-overview).
