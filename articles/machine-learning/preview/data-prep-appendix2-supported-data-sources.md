---
title: "Veri kaynakları ile Azure Machine Learning veri hazırlığı kullanılabilir desteklenen | Microsoft Docs"
description: "Bu belgede Azure Machine Learning veri hazırlığı kullanılabilir desteklenen veri kaynaklarının tam bir listesi sağlanmaktadır."
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
ms.date: 09/12/2017
ms.openlocfilehash: 458338cd23c704c40c512dd96b22a4790f27d017
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2017
---
# <a name="supported-data-sources-for-azure-machine-learning-data-preparation"></a>Azure Machine Learning veri hazırlığı için desteklenen veri kaynakları 
Bu makalede, Azure Machine Learning veri hazırlığı için şu anda desteklenen veri kaynakları özetlenmektedir.

Bu sürüm için desteklenen veri kaynakları aşağıda gösterilmektedir.

## <a name="types"></a>Türler 

### <a name="sql-server"></a>SQL Server
Şirket içi SQL server veya Azure SQL veritabanından okuyun.

#### <a name="options"></a>Seçenekler
- Sunucu Adresi
- Sunucu (sunucu üzerindeki sertifika geçerli olmadığında bile. güven Dikkatli kullanın)
- Kimlik doğrulama türü (Windows, sunucu)
- User Name
- Parola
- Veritabanına bağlanmak için
- SQL sorgusu

#### <a name="notes"></a>Notlar
- SQL variant sütunları desteklenmiyor
- Saat sütunu datetime için tarih 1970'ten / 1/1 veritabanından süresi eklenerek dönüştürülür
- Spark kümesi üzerinde çalıştırıldığında, tüm sütunlar (date, datetime, datetime2, datetimeoffset) 1583 önceki tarihler için hatalı değerler değerlendirecek ilgili verileri
- Ondalık sütunlardaki değerleri ondalık dönüştürme nedeniyle duyarlık kaybedebileceği

### <a name="directory-vs-file"></a>Dosya ve dizin
Tek bir dosya seçin ve veri hazırlığı okuyun. Sonraki ekranda gösterilen dosya bağlantı için varsayılan parametreleri belirlemek için dosya türü ayrıştırılır.

Bir dizin veya dosyaları (dosya seçiciyi çoklu) bir dizin içinde kümesini seçin. Her iki yaklaşım ile dosyalar tek bir veri akış içinde okuyun ve birbirlerine gerekirse çıkarılır üstbilgileri ile eklenir.

Desteklenen dosya türleri şunlardır:
- Ayrılmış (.csv, .tsv, .txt, vb.)
- Sabit Genişlik
- Düz metin
- JSON dosyası

### <a name="csv-file"></a>CSV dosyası
Bir virgülle ayrılmış değer dosyası depolama alanından okuyun.

#### <a name="options"></a>Seçenekler
- ayırıcı
- Açıklama
- Üst bilgiler
- Ondalık simgesi
- Dosya kodlama
- Atlanacak satır

### <a name="tsv-file"></a>TSV dosyası
Sekme ayrılmış değer dosyası depolama alanından okuyun.

#### <a name="options"></a>Seçenekler
- Açıklama
- Üst bilgiler
- Dosya kodlama
- Atlanacak satır

### <a name="excel-xlsxlsx"></a>Excel (.xls/.xlsx)
Bir Excel dosyası bir sayfa, sayfa adı veya numarası belirterek aynı anda okuyun.

#### <a name="options"></a>Seçenekler
- Sayfa adı veya numarası
- Üst bilgiler
- Atlanacak satır

### <a name="json-file"></a>JSON dosyası
Bir JSON dosyası depolama alanından okuyun. Dosyanın "üzerinde okuma düzleştirilmiş".

#### <a name="options"></a>Seçenekler
- None

### <a name="parquet"></a>Parquet
Bir Parquet veri kümesi ya da tek bir dosya veya klasör okuyun.

Parquet gibi bir biçim çeşitli depolama biçimlerde olabilir. Daha küçük veri kümeleri için tek .parquet dosyası bazen kullanılır. Çeşitli Python kitaplıkları okunurken veya yazılırken tek .parquet dosyalarını destekler. Şu anda, Azure Machine Learning veri hazırlığı sırasında yerel etkileşimli kullanım Parquet okumak için PyArrow Python kitaplığı kullanır. Tek .parquet dosyaları (büyük veri kümesinin bir parçası olarak değil de, örneğin olarak yazılmış sürece), destekler Parquet veri kümelerini yanı sıra.

Parquet veri kümesi, her biri daha büyük bir veri kümesi daha küçük bir bölümünü temsil eder, birden fazla .parquet dosya koleksiyonudur. Veri kümeleri, genellikle bir klasörde yer alan ve Spark ve Hive gibi platformlar için varsayılan parquet çıktı biçimi.

>[!NOTE]
>Bir klasördeki birden çok .parquet dosyalarla Parquet verileri okurken, okuma, dizin seçmek en güvenli ve **Parquet veri kümesi** seçeneği. Bu, tek tek dosyalar yerine tüm klasörü okumak PyArrow hale getirir. Bu diskte klasör bölümlendirme gibi Parquet depolama daha karmaşık yolları okumak için destek sağlar.

Genişleme yürütme üzerinde Spark'ın Parquet özellikleri okuma güvenir ve klasörler, yerel etkileşimli kullanım için benzer yanı sıra tek dosyalarını destekler.

#### <a name="options"></a>Seçenekler
- Veri kümesi parquet. Bu seçenek Azure Machine Learning veri hazırlığı belirli bir dizin genişletir ve tek tek her dosyasını okumaya çalışır belirler (seçili olmayan mod) veya olup dizini tam veri kümesi (Seçilen modu) olarak işler. Seçili moduyla PyArrow dosyaları yorumlamak için en iyi yolu seçer.


## <a name="locations"></a>Konumlar
### <a name="local"></a>Yerel
Yerel sabit diske veya bir eşlenen ağ depolama konumu.

### <a name="sql-server"></a>SQL Server
Şirket içi SQL Server, veya Azure SQL veritabanı.

### <a name="azure-blob-storage"></a>Azure Blob depolama
Bir Azure aboneliği gerektirir azure Blob Depolama birimi.

