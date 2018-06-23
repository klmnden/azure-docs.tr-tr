---
title: LogDownloader - Azure Bilişsel hizmetler | Microsoft Docs
description: Azure özel karar hizmeti tarafından oluşturulan günlük dosyalarını indirin.
services: cognitive-services
author: marco-rossi29
manager: marco-rossi29
ms.service: cognitive-services
ms.topic: article
ms.date: 05/09/2018
ms.author: marossi
ms.openlocfilehash: 783b534b3b3f4bb7f5d9f073f491690759edfea5
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354551"
---
# <a name="logdownloader"></a>LogDownloader

Azure özel karar hizmeti tarafından üretilen ve Oluştur günlük dosyalarını indirme *.gz* deneme tarafından kullanılan dosyalar.

## <a name="prerequisites"></a>Önkoşullar

- Python 3: Yüklü ve yolunuzu. Büyük dosyaları işlemek için 64-bit sürümü öneririz.
- *Mwt/Microsoft-ds* deposu: [depoyu kopyalama](https://github.com/Microsoft/mwt-ds).
- *Azure storage blobuna* paket: yükleme ayrıntılarını Git [Python için Microsoft Azure depolama Kitaplığı](https://github.com/Azure/azure-storage-python#option-1-via-pypi).
- Azure depolama bağlantı dizenizi girin *mwt-ds/DataScience/ds.config*: izleyin *my_app_id: my_connectionString* şablonu. Birden çok belirtebilirsiniz `app_id`. Çalıştırdığınızda `LogDownloader.py`, giriş `app_id` içinde bulunamadı `ds.config`, `LogDownloader.py` kullanan `$Default` bağlantı dizesi.

## <a name="usage"></a>Kullanım

Git `mwt-ds/DataScience` çalıştırıp `LogDownloader.py` aşağıdaki kodda ayrıntılı olarak ilgili bağımsız değişkenlere sahip:

```cmd
python LogDownloader.py [-h] -a APP_ID -l LOG_DIR [-s START_DATE]
                        [-e END_DATE] [-o OVERWRITE_MODE] [--dry_run]
                        [--create_gzip] [--delta_mod_t DELTA_MOD_T]
                        [--verbose] [-v VERSION]
```

### <a name="parameters"></a>Parametreler

| Girdi | Açıklama | Varsayılan |
| --- | --- | --- |
| `-h`, `--help` | Yardım iletisi ve çıkış gösterir. | |
| `-a APP_ID`, `--app_id APP_ID` | Uygulama Kimliği (diğer bir deyişle, Azure Storage blob kapsayıcı adı). | Gerekli |
| `-l LOG_DIR`, `--log_dir LOG_DIR` | (Bir alt klasör oluşturulur) veri indirme için temel dizin.  | Gerekli |
| `-s START_DATE`, `--start_date START_DATE` | Başlangıç tarihi (dahil), indirme *YYYY-AA-GG* biçimi. | `None` |
| `-e END_DATE`, `--end_date END_DATE` | (Dahil) yükleme son tarihi, *YYYY-AA-GG* biçimi. | `None` |
| `-o OVERWRITE_MODE`, `--overwrite_mode OVERWRITE_MODE` | Kullanmak için üzerine yazma modu. | |
| | `0`: Hiçbir zaman üzerine; BLOB'ları şu anda kullanılmakta olan kullanıcı isteyin. | Varsayılan | |
| | `1`: Kullanıcı dosyaları farklı boyutlarda veya ne zaman BLOB şu anda kullanılmakta olan devam etme isteyin. | |
| | `2`: Her zaman üzerine; şu anda kullanılan BLOB'lar indirin. | |
| | `3`: Hiçbir zaman üzerine ve boyutu büyükse, istemeden append; şu anda kullanılan BLOB'lar indirin. | |
| | `4`: Hiçbir zaman üzerine ve boyutu büyükse, istemeden append; şu anda kullanılan BLOB'lar atlayın. | |
| `--dry_run` | Yazdırma hangi BLOB'lar, indirmeden indirilmiş olan. | `False` |
| `--create_gzip` | Oluşturma bir *gzip* Vowpal Wabbit dosyası. | `False` |
| `--delta_mod_t DELTA_MOD_T` | Bir dosyanın şu anda kullanımda olup olmadığını saptamak için saniye olarak zaman penceresi. | `3600` saniye (`1` saat) |
| `--verbose` | Daha fazla ayrıntı yazdırın. | `False` |
| `-v VERSION`, `--version VERSION` | Kullanılacak günlük yükleyici sürümü. | |
| | `1`: Uncooked günlüklerini (yalnızca geriye dönük uyumluluk için). | Kullanım Dışı |
| | `2`: Hazırlanan günlüklerini. | Varsayılan |

### <a name="examples"></a>Örnekler

Kuru Azure Storage blob kapsayıcısında tüm verileri indirilmesi Çalıştır için aşağıdaki kodu kullanın:
```cmd
python LogDownloader.py -a your_app_id -l d:\data --dry_run
```

Yalnızca 1 Ocak 2018 ile bu yana oluşturulan günlükleri indirmek için `overwrite_mode=4`, aşağıdaki kodu kullanın:
```cmd
python LogDownloader.py -a your_app_id -l d:\data -s 2018-1-1 -o 4
```

Oluşturmak için bir *gzip* dosya indirilen tüm dosyaları birleştirme, aşağıdaki kodu kullanın:
```cmd
python LogDownloader.py -a your_app_id -l d:\data -s 2018-1-1 -o 4 --create_gzip
```