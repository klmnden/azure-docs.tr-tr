---
title: "Veri kaynakları kullanılabilir Azure Machine Learning veri hazırlığı ile desteklenen | Microsoft Docs"
description: "Bu belge desteklenen veri kaynaklarının tam listesi için Azure Machine Learning veri hazırlığı sağlar."
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
ms.openlocfilehash: 1ef4c5c33d98cfeb566e8fe23bda9e0d3f041781
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2017
---
# <a name="supported-data-sources-for-this-release"></a>Bu sürüm için desteklenen veri kaynakları 
Aşağıdaki belge Azure Machine Learning veri hazırlık şu anda desteklenen veri kaynaklarının listesini özetler.

Bu sürüm için desteklenen veri kaynakları aşağıda gösterilmektedir.

## <a name="types"></a>Türler 
### <a name="directory-versus-file"></a>Dosya ve dizin
*Dosya veya dizinlerin*: tek bir dosya seçin ve veri hazırlığı okuyun. Sonraki ekranda dosya bağlantı için varsayılan parametreleri belirlemek için dosya türü ayrıştırılır. Bir dizin veya dosyaları (dosya seçiciyi çoklu) bir dizin içinde kümesini seçin. Her iki yaklaşım sonuçları tek bir veri akışı birbirlerine (gerekirse çıkarılır üstbilgiler ile) eklenen dosyalarla okunan dosyalarında.

Dosya türleri aşağıdaki gibidir:
- Ayrılmış (.csv, .tsv, .txt vb.) 
- Sabit Genişlik
- Düz metin
- JSON dosyası

### <a name="csv-file"></a>CSV dosyası
Bir CSV dosyası depolama biriminden okur.

#### <a name="options"></a>Seçenekler
- ayırıcı
- Açıklama
- Üstbilgileri
- Ondalık simgesi
- Dosya kodlama
- Atlanacak satır

### <a name="tsv-file"></a>TSV dosyası
Depolama biriminden bir TSV değer dosyasını okur.

#### <a name="options"></a>Seçenekler
- Açıklama
- Üstbilgileri
- Dosya kodlama
- Atlanacak satır

### <a name="excel-xlsxlsx"></a>Excel (.xls/.xlsx)
Sayfa adı veya numarası belirterek, her seferinde bir sayfa bir Excel dosyasını okur.

#### <a name="options"></a>Seçenekler
- Sayfa adı/numarası
- Üstbilgileri
- Atlanacak satır

### <a name="json-file"></a>JSON dosyası
Bir JSON dosyası depolama alanından okuyun. Dosyanın "düzleştirilmiş," okuma unutmayın.

#### <a name="options"></a>Seçenekler
None

### <a name="parquet"></a>Parquet
Parquet dataset ya da tek bir dosya veya klasör okuyun.

Parquet gibi bir biçim çeşitli depolama biçimlerde olabilir. Daha küçük veri kümeleri için tek .parquet dosyası bazen kullanılır. Çeşitli Python kitaplıkları okuma veya tek .parquet dosyasına yazmayı destekler. Şu anda, Azure Machine Learning çalışma sırasında yerel etkileşimli kullanım Parquet okumak için PyArrow Python kitaplığı kullanır. (Bunlar, bu nedenle daha büyük bir veri kümesinin bir parçası değil yazılmış sürece) tek .parquet dosyalarını destekler. Ayrıca, Parquet veri kümelerini destekler. 

Parquet veri kümesi, her biri daha büyük bir veri kümesi daha küçük bir bölümünü temsil eder, birden fazla .parquet dosya koleksiyonudur. Veri kümeleri, genellikle bir klasörde yer alır. Spark ve Hive gibi ortak platformlar için varsayılan Parquet çıktı biçimi oldukları.

>[!NOTE]
>Bir klasördeki birden çok .parquet dosyalarla Parquet verileri okurken okuma ve değer için dizini seçmek en güvenli olanıdır **Parquet Dataset** seçeneği. Bu, tek tek dosyalar yerine tüm klasörü okumak PyArrow hale getirir. Bu Parquet (örneğin, klasör bölümlendirme.) disk depolama, daha karmaşık yolları okuma desteği sağlar

Genişleme yürütme üzerinde Spark'ın Parquet özellikleri okuma güvenir ve klasörleri yanı sıra tek dosyalarını destekler.

#### <a name="options"></a>Seçenekler
*Veri kümesi parquet*: Bu seçenek, Azure Machine Learning çalışma ekranı unticked modu veya ticked modu kullanıp kullanmadığını belirler. Unticked modu belirli bir dizin genişletir ve tek tek her dosyada okumaya çalışır. Ticked modu dizin tam veri kümesi olarak değerlendirir ve PyArrow olanak tanır dosyaları yorumlamak için en iyi şekilde şekil.


## <a name="locations"></a>Konumlar
### <a name="local"></a>Yerel
Yerel sabit diske veya eşlenen ağ depolama konumu.

### <a name="azure-blob-storage"></a>Azure Blob depolama
Bir Azure aboneliği gerektirir.

