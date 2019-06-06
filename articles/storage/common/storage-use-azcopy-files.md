---
title: AzCopy v10 kullanarak Azure dosyaları veri aktarma | Microsoft Docs
description: AzCopy ve dosya depolama ile veri aktarımı.
services: storage
author: normesta
ms.service: storage
ms.topic: article
ms.date: 05/14/2019
ms.author: normesta
ms.subservice: common
ms.openlocfilehash: 69d7136396c3d989e63b8956d3e703cc7f9666c8
ms.sourcegitcommit: 6932af4f4222786476fdf62e1e0bf09295d723a1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2019
ms.locfileid: "66687940"
---
# <a name="transfer-data-with-azcopy-and-file-storage"></a>AzCopy ve dosya depolama ile veri aktarma 

AzCopy, BLOB veya dosyalar için veya bir depolama hesabına kopyalamak için kullanabileceğiniz bir komut satırı yardımcı programıdır. Bu makale, Azure dosyaları ile çalışma örnek komutlar içerir.

Başlamadan önce bkz: [Azcopy'i kullanmaya başlama](storage-use-azcopy-v10.md) makale AzCopy indirip aracı ile kendinizi alıştırın.

## <a name="create-file-shares"></a>Dosya paylaşımları oluşturma

AzCopy kullanabilirsiniz `make` bir dosya paylaşımı oluşturmak için komutu. Bu bölümdeki örnek adlı bir dosya paylaşımı oluşturur `myfileshare`.

|    |     |
|--------|-----------|
| **Söz dizimi** | `azcopy make "https://<storage-account-name>.file.core.windows.net/<file-share-name>?<SAS-token>"` |
| **Örnek** | `azcopy make "https://mystorageaccount.file.core.windows.net/myfileshare?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D"` |

## <a name="upload-files"></a>Dosyaları karşıya yükleme

AzCopy kullanabilirsiniz `copy` dosyaları ve dizinleri yerel bilgisayarınızdan yüklemek için komutu.

Bu bölüm aşağıdaki örnekleri içerir:

> [!div class="checklist"]
> * Dosyayı karşıya yükleme
> * Bir dizin karşıya yükleme
> * Joker karakterler kullanarak karşıya dosya yükleme

> [!NOTE]
> AzCopy otomatik olarak hesaplar ve dosya md5 karma kod saklamak değil. Bunu yapmak için AzCopy istiyorsanız, ardından ekleme `--put-md5` bayrağı her kopyalama komutu. Dosya indirildiğinde, bu şekilde, AzCopy bir MD5 karma değeri veri indirilir ve MD5 karma değeri dosyanın içinde depolanan doğrular hesaplar `Content-md5` özelliği hesaplanan karma eşleşir.

### <a name="upload-a-file"></a>Dosyayı karşıya yükleme

|    |     |
|--------|-----------|
| **Söz dizimi** | `azcopy cp "<local-file-path>" "https://<storage-account-name>.file.core.windows.net/<file-share-name>/<file-name>?<SAS-token>"` |
| **Örnek** | `azcopy copy "C:\myDirectory\myTextFile.txt" "https://mystorageaccount.file.core.windows.net/myfileshare/myTextFile.txt?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D"` |

### <a name="upload-a-directory"></a>Bir dizin karşıya yükleme

Bu örnek, bir dosya paylaşımı için bir dizin (ve tüm dosyaları dizindeki) kopyalar. Bir dizin aynı adı taşıyan bir dosya paylaşımındaki sonucudur.

|    |     |
|--------|-----------|
| **Söz dizimi** | `azcopy copy "<local-directory-path>" "https://<storage-account-name>.file.core.windows.net/<file-share-name>?<SAS-token>" --recursive` |
| **Örnek** | `azcopy copy "C:\myDirectory" "https://mystorageaccount.file.core.windows.net/myfileshare?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D" --recursive` |

Dosya paylaşımı içinde bir dizine kopyalamak için yalnızca bu dizinin adını komut dizenizi belirtin.

|    |     |
|--------|-----------|
| **Örnek** | `azcopy copy "C:\myDirectory" "https://mystorageaccount.file.core.windows.net/myfileshare/myFileShareDirectory?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D" --recursive` |

Dosya paylaşımı var olmayan bir dizin adı belirtirseniz, AzCopy bu adla yeni bir dizin oluşturur.

### <a name="upload-the-contents-of-a-directory"></a>Bir dizinin içeriklerini karşıya yükleme

(*) Joker karakter sembolünü kullanarak içeren dizin kopyalama olmadan bir dizinin içeriklerini karşıya yükleyebilirsiniz.

|    |     |
|--------|-----------|
| **Söz dizimi** | `azcopy copy "<local-directory-path>/*" "https://<storage-account-name>.file.core.windows.net/<file-share-name>/<directory-path>?<SAS-token>` |
| **Örnek** | `azcopy copy "C:\myDirectory\*" "https://mystorageaccount.file.core.windows.net/myfileshare/myFileShareDirectory?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D"` |

> [!NOTE]
> Append `--recursive` tüm alt dizinlerde dosyaları karşıya yüklemek için bayrak.

## <a name="download-files"></a>Dosyaları indirme

AzCopy kullanabilirsiniz `copy` komut dosyaları, dizinleri ve dosya indirmek için yerel bilgisayarınıza paylaşır.

Bu bölüm aşağıdaki örnekleri içerir:

> [!div class="checklist"]
> * Dosya indirme
> * Bir dizine indirin
> * Joker karakterler kullanarak dosyaları indirme

> [!NOTE]
> Varsa `Content-md5` dosyasının özellik değerini içeren bir karma, AzCopy indirilen veriler için bir MD5 karma değeri hesaplar ve MD5 karma değeri dosyanın içinde depolanan doğrular `Content-md5` özelliği hesaplanan karma eşleşir. Bu değerler eşleşmiyorsa, ekleyerek bu davranışı geçersiz kılma sürece indirme başarısız `--check-md5=NoCheck` veya `--check-md5=LogOnly` kopyalama komutu.

### <a name="download-a-file"></a>Dosya indirme

|    |     |
|--------|-----------|
| **Söz dizimi** | `azcopy copy "https://<storage-account-name>.file.core.windows.net/<file-share-name>/<file-path>?<SAS-token>" "<local-file-path>"` |
| **Örnek** | `azcopy copy "https://mystorageaccount.file.core.windows.net/myfileshare/myTextFile.txt?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D" "C:\myDirectory\myTextFile.txt"` |

### <a name="download-a-directory"></a>Bir dizine indirin

|    |     |
|--------|-----------|
| **Söz dizimi** | `azcopy copy "https://<storage-account-name>.file.core.windows.net/<file-share-name>/<directory-path>?<SAS-token>" "<local-directory-path>" --recursive` |
| **Örnek** | `azcopy copy "https://mystorageaccount.file.core.windows.net/myfileshare/myFileShareDirectory?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D" "C:\myDirectory"  --recursive` |

Bu örnek adlı bir dizinde sonuçlanır `C:\myDirectory\myFileShareDirectory` indirilen dosyaların tümünü içerir.

### <a name="download-the-contents-of-a-directory"></a>Bir dizinin içeriğini indirin

Bir dizinin içeriklerini içeren dizin kopyalamadan (*) joker karakter sembolünü kullanarak indirebilirsiniz.

|    |     |
|--------|-----------|
| **Söz dizimi** | `azcopy copy "https://<storage-account-name>.file.core.windows.net/<file-share-name>/*?<SAS-token>" "<local-directory-path>/"` |
| **Örnek** | `azcopy copy "https://mystorageaccount.file.core.windows.net/myfileshare/myFileShareDirectory/*?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D" "C:\myDirectory"` |

> [!NOTE]
> Append `--recursive` tüm alt dizinlerde dosyaları indirmek için bayrak.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla örnek, tüm bu makaleler bulun:

- [Azcopy'i kullanmaya başlama](storage-use-azcopy-v10.md)

- [AzCopy ve blob depolama ile veri aktarma](storage-use-azcopy-blobs.md)

- [AzCopy ve Amazon S3 demetleri ile veri aktarma](storage-use-azcopy-s3.md)

- [Yapılandırma, en iyi duruma getirmek ve AzCopy sorunlarını giderme](storage-use-azcopy-configure.md)