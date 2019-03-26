---
title: Azure veri fabrikası veri akışı havuz dönüştürme eşlemesi
description: Azure veri fabrikası veri akışı havuz dönüştürme eşlemesi
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: a39fa0949276b7e86c7fdd0d0861492a9a0b723e
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58438641"
---
# <a name="mapping-data-flow-sink-transformation"></a>Veri akışı havuz dönüştürme eşlemesi

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

![Havuz seçenekleri](media/data-flow/sink1.png "havuzu 1")

Veri akışı dönüşümünüzü tamamlandığında, bir hedef veri kümesine dönüştürülmüş veri havuzu. Havuz dönüşümünde hedef çıktı verileri için kullanmak istediğiniz veri kümesi tanımı seçebilirsiniz. Veri akışınızı gerektirdiği sayıda havuz dönüştürme olabilir.

Bir ortak gelen verileri değiştirmek için hesap ve şema kayması için hesap tanımlı bir şeması çıkış veri kümesinde olmayan bir klasör için çıktı verilerini havuz uygulamadır. Ayrıca tüm sütun değişikliklerini kaynaklarınızı "İzin ver şema değişikliklerini" kaynak ve sonra eşleme seçerek havuz içindeki tüm alanlarda hesabının.

Üzerine, ekleme veya bir veri kümesine yönelik veri akışı başarısız seçebilirsiniz.

Tüm gelen alanları havuz "eşleme" de seçebilirsiniz. Hedef havuz istediğiniz alanları seçin istiyorsanız veya hedefteki alanların adlarını değiştirmek istiyorsanız, "eşleme" için "Kapat"'i seçin ve sonra çıktı alanlarını eşlemek için eşleme sekmesine tıklayın:

![Havuz seçenekleri](media/data-flow/sink2.png "2 Havuz")

## <a name="output-to-one-file"></a>Çıktıyı bir dosyaya
Azure depolama blobu veya Data Lake havuz türlerinde bir klasöre dönüştürülmüş verileri çıkarır. Spark, kullanılan bölümleme düzenini temel alarak bölümlenmiş çıkış veri dosyaları oluşturacağını havuz Dönüştür. Bölümleme düzeni "En iyi duruma getir" sekmesinde tıklayarak ayarlayabilirsiniz. Çıkış tek bir dosyada birleştirmek için ADF esnetmek istiyorsanız "Tek bölüm" radyo düğmesine tıklayın.

![Havuz seçenekleri](media/data-flow/opt001.png "havuz seçenekleri")

## <a name="field-mapping"></a>Alan eşleme

Havuz dönüşümünüzü eşleme sekmesinde hedefe (sağ taraf) gelen (sol taraf) sütun eşleyebilirsiniz. Bir veri akışı dosyalarını havuz, ADF yeni dosyaları bir klasöre her zaman yazın. Veritabanı veri kümesine eşlediğinizde, ya da bu şema ("üzerine yazmak için" ilke kaydetme ayarlanır) ile yeni bir tablo oluşturmak seçebilir veya mevcut bir yeni satır Ekle tablo ve mevcut şemaya alanları eşleyin.

Tek tıklamayla birden çok sütun bağlantısı, birden çok sütun delink veya birden çok satır aynı sütun adını eşleştirmek için eşleme tablosunda çoklu seçim kullanabilirsiniz.

Her zaman gelen alan kümesini alıp bunları hedef olarak eşleştirmek istediğiniz zaman-"Şema değişikliklerini izin ver" ayarı ise.

![Alan eşleme](media/data-flow/multi1.png "birden fazla seçenek")

Sütunları eşlemelerinizi sıfırlamak istiyorsanız, eşlemeleri sıfırlamak için "Yeniden eşleme" düğmesine basın.

![Havuz seçenekleri](media/data-flow/sink1.png "bir havuz")

![Havuz seçenekleri](media/data-flow/sink2.png "iç havuzları")

* Şema değişikliklerini ve şema doğrulama seçenekleri artık havuzunda kullanılabilir izin verir. Bu, esnek şema tanımlarını (şema değişikliklerini) kabul edin veya tamamen (doğrulama şema) şema değişirse havuz başarısız için ADF bildirmesine olanak tanır.

* Klasör temizleyin. ADF havuz klasör içeriğini hedef dosyaların hedef klasörde yazmadan önce keser.

## <a name="file-name-options"></a>Dosya adı seçenekleri

   * Varsayılan: Spark bölümü varsayılanlara dayanan adı dosyalara izin ver
   * Desen: Bir desen çıktı dosyalarınızı girin. Örneğin, "KREDİLERİ [n]" oluşturacak loans1.csv, loans2.csv,...
   * Bölüm başına: Bölüm başına bir dosya adı girin
   * Sütundaki verilerin: Çıkış dosyası bir sütunun değerine ayarlayın.

> [!NOTE]
> Yürütme veri akışı etkinliği olmayan modda veri akışı hata ayıklama çalıştırırken dosya işlemleri yalnızca yürütme

## <a name="database-options"></a>Veritabanı seçenekleri

* INSERT, update, delete, upsert eder izin verir. Eklemeleri izin vermek için varsayılandır. Güncelleştirme, upsert veya silmek istiyorsanız, etiketi satırlara bu belirli eylemler için öncelikle bir alter satır dönüştürme eklemeniz gerekir. "İzin Ekle" ADF kaynağınızdan yeni satır ekleyerek gelen durdurur.
* Truncate tablo (tüm satırları hedef tablonuzdan veri akışı tamamlamadan önce kaldırır)
* (Açılan/oluşturma, hedef tablo veri akışı tamamlamadan önce gerçekleştirir) tabloyu yeniden oluşturun
* Büyük veri için toplu iş boyutu yükler. Öbeklere demet yazma işlemleri için bir sayı girin
* Hazırlamayı etkinleştir: Bu ADF Polybase, Azure veri ambarı, havuz veri kümesi olarak yüklerken kullanılacak yenilemelerini ister

> [!NOTE]
> Veri akışı havuz dönüşümü yeni bir tablo adı olan bir veri kümesi ayarlayarak, hedef veritabanında yeni bir tablo tanımı oluşturmak için ADF sorabilirsiniz. SQL veri kümesini tablo adının altında "Düzenle"'ye tıklayın ve yeni bir tablo adı girin. Ardından, havuz dönüşümünde "İzin ver şema değişikliklerini zamanlı şekilde" açın. Seth "Şemayı içeri aktar" ayarı yok.

![Kaynak dönüştürme şema](media/data-flow/dataset2.png "SQL şeması")

![SQL havuz seçenekleri](media/data-flow/alter-row2.png "SQL seçenekleri")

> [!NOTE]
> Güncelleştirme veya veritabanı havuzunuzu satır silme, anahtar sütunu ayarlamanız gerekir. Bu şekilde DML benzersiz satırda belirlemek Alter satır kuramıyor.

## <a name="next-steps"></a>Sonraki adımlar

Veri akışınızı oluşturduğunuza göre eklemek bir [yürütme veri akışı etkinliği ardışık](concepts-data-flow-overview.md).
