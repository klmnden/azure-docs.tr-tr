---
title: Azure Machine Learning veri hazırlama ile kullanılabilen veri kaynakları desteklenen | Microsoft Docs
description: Bu belge, Azure Machine Learning veri hazırlama için kullanılabilir desteklenen veri kaynaklarının tam listesi sağlar.
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
ROBOTS: NOINDEX
ms.openlocfilehash: eb84638e2996b0ee9bd5a580b7e827ea30d0ab21
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2018
ms.locfileid: "48855995"
---
# <a name="supported-data-sources-for-azure-machine-learning-data-preparation"></a>Azure Machine Learning veri hazırlama için desteklenen veri kaynakları 

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 

Bu makalede, Azure Machine Learning veri hazırlama için şu anda desteklenen veri kaynakları özetlenmektedir.

Bu sürüm için desteklenen veri kaynakları aşağıdaki gibidir.

## <a name="types"></a>Türler 

### <a name="sql-server"></a>SQL Server
Şirket içi SQL server veya Azure SQL veritabanı'nı okuyun.

#### <a name="options"></a>Seçenekler
- Sunucu Adresi
- Sunucu (sunucu üzerindeki sertifika geçerli olmadığında bile. güven Dikkatli kullanın)
- Kimlik doğrulama türü (Windows, Server)
- User Name
- Parola
- Bağlanmak için veritabanı
- SQL sorgusu

#### <a name="notes"></a>Notlar
- SQL varyantı sütunlar desteklenmez
- Saat sütununu tarih 1/1970/1 veritabanı süre eklenerek datetime türüne dönüştürülür
- Spark kümesinde yürütüldüğünde, tüm sütunları (tarih, datetime, datetime2, datetimeoffset) hatalı değerler tarihlerden önce 1583 değerlendirecek ilgili verileri
- Ondalık sütunlardaki değerleri ondalık dönüştürülmesi nedeniyle duyarlık kaybedebileceği

### <a name="directory-vs-file"></a>Dosya ve dizin
Tek bir dosya seçin ve veri hazırlama okuyun. Dosya türü, sonraki ekranda gösterilen dosya bağlantısı için varsayılan parametreleri belirlemek için ayrıştırılır.

Bir dizin ya da bir dizi içinde bir dizin (dosya Seçici çoklu seçim) dosyası seçin. Her iki yöntemle dosyaları tek bir veri akış içinde okuyun ve gerekirse çıkarılır üst bilgileri ile birbirine eklenir.

Desteklenen dosya türleri şunlardır:
- Ayrılmış (.csv, .tsv, .txt, vb.)
- Sabit Genişlik
- Düz metin
- JSON dosyası

### <a name="csv-file"></a>CSV dosyası
Bir virgülle ayrılmış değer dosyasını depolama alanından okuyun.

#### <a name="options"></a>Seçenekler
- Ayırıcı
- Açıklama
- Üst bilgiler
- Ondalık simgesi
- Dosya kodlama
- Atlanacak satır

### <a name="tsv-file"></a>TSV dosyası
Bir sekme ayrılmış değer dosyasını depolama alanından okuyun.

#### <a name="options"></a>Seçenekler
- Açıklama
- Üst bilgiler
- Dosya kodlama
- Atlanacak satır

### <a name="excel-xlsxlsx"></a>Excel (.xls/.xlsx)
Bir Excel dosyası bir sayfa sayfa adı veya numarası belirterek birer okuyun.

#### <a name="options"></a>Seçenekler
- Sayfa adı veya numarası
- Üst bilgiler
- Atlanacak satır

### <a name="json-file"></a>JSON dosyası
Bir JSON dosyasını depolama alanından okuyun. Dosya "okuma sırasında düzleştirilir".

#### <a name="options"></a>Seçenekler
- None

### <a name="parquet"></a>Parquet
Parquet veri kümesi, tek bir dosya veya klasör ya da okuyun.

Bir biçim çeşitli depolama alanında sürebilir, parquet. Daha küçük veri kümeleri için bazen tek .parquet dosya kullanılır. Çeşitli Python kitaplıkları, okuma veya tek .parquet dosyalara yazma desteği. Şu anda, Azure Machine Learning veri hazırlama sırasında yerel etkileşimli kullanım Parquet okumak için PyArrow Python kitaplığı kullanır. (Daha büyük bir veri kümesinin bir parçası olarak değil de, örneğin olarak yazılmış sürece), tek .parquet dosyaları destekler Parquet veri kümelerinin yanı sıra.

Parquet veri kümesi, birden fazla .parquet dosyası, her biri daha büyük bir veri kümesini daha küçük bir bölümünü temsil eden bir koleksiyonudur. Veri kümeleri, genellikle bir klasörde yer alır ve Spark ve Hive gibi platformlar için varsayılan parquet çıkış biçimi.

>[!NOTE]
>Bir klasördeki birden çok .parquet dosyalarla Parquet verileri okurken dizini için okuma, seçmek güvenli ve **Parquet veri kümesi** seçeneği. Bu, tek tek dosyalar yerine tam klasörü okumak PyArrow hale getirir. Bu diskteki klasör bölümleme gibi Parquet depolamak daha karmaşık yöntemler okumak için destek sağlar.

Ölçeği genişletilmiş yürütme, üzerinde Spark'ın Parquet yetenekleri okuma kullanır ve klasörleri yerel etkileşimli kullanımına benzer yanı sıra tek dosya destekler.

#### <a name="options"></a>Seçenekler
- Parquet veri kümesi. Bu seçenek, Azure Machine Learning veri hazırlama belirli bir dizin genişletir ve tek tek her dosyayı okumayı dener belirler (seçili olmayan mod) veya olup dizini tüm veri kümesi (Seçili modu) işler. Seçili moduyla PyArrow dosyaları yorumlamak için en iyi yolu seçer.


## <a name="locations"></a>Konumlar
### <a name="local"></a>Yerel
Bir yerel sabit sürücüye veya bir eşlenen ağ depolama konumu.

### <a name="sql-server"></a>SQL Server
Şirket içi SQL server veya Azure SQL veritabanı.

### <a name="azure-blob-storage"></a>Azure Blob depolama
Bir Azure aboneliği gerektiren azure Blob Depolama.

