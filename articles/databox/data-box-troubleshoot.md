---
title: Azure Data Box'ınızı üzerinde sorunlarını giderme | Microsoft Docs
description: Azure Data Box verileri Azure'a karşıya yükleme sırasında görülen sorunların nasıl giderileceği açıklanmaktadır.
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: article
ms.date: 04/26/2019
ms.author: alkohli
ms.openlocfilehash: 329ea6ac52e972c42e0e2aab247a7ad8e04c76e7
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64927365"
---
# <a name="troubleshoot-issues-related-to-azure-data-box"></a>Azure Data Box için ilgili sorunları giderme

Bu makalede, Azure Data Box'ı kullanırken görebilirsiniz sorunlarını giderme konusunda bilgi ayrıntılı olarak açıklanmaktadır.

## <a name="errors-during-data-copy"></a>Veri kopyalama sırasında karşılaşılan hatalar

Aşağıdaki bölümlerde, veri kopyalama sırasında görülen tüm hataları özetlenmiştir.

### <a name="errorcontainerorsharenamelength"></a>ERROR_CONTAINER_OR_SHARE_NAME_LENGTH 

**Hata açıklaması:** Kapsayıcı veya paylaşım adı 3 ile 63 karakter arasında olmalıdır.

**Önerilen çözünürlük:** Klasörü altında veri kopyaladığınız Data Box (SMB/NFS) paylaşımı, depolama hesabınızdaki bir Azure kapsayıcı haline gelir. 

- Üzerinde **Bağlan ve Kopyala** sayfasında Data Box yerel web kullanıcı Arabirimi, indirme ve klasörü belirlemek için hata dosyalarının adları ile ilgili sorunları gözden geçirin.
- Data Box paylaşım emin olmak için klasör adıyla değiştirin:

    - Adı 3 ila 63 karakter uzunluğunda olabilir.
    - Adları yalnızca harf, rakam ve kısa çizgi olabilir.
    - Adları başlatmak veya kısa çizgi ile bitmelidir.
    - Adlarında art arda kısa çizgi olamaz.
    - Geçerli adlar örnekleri: `my-folder-1`, `my-really-extra-long-folder-111`</li><li>
    - Geçerli olmayan adları örnekleri: `my-folder_1`, `my`, `--myfolder`, `myfolder--`, `myfolder!`

    Daha fazla bilgi için bkz. Azure adlandırma kuralları için [kapsayıcı adları](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata#container-names) ve [paylaşım adları](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-shares--directories--files--and-metadata#share-names).

### <a name="errorcontainerorsharenamelength"></a>ERROR_CONTAINER_OR_SHARE_NAME_LENGTH     

**Hata açıklaması:** Kapsayıcı veya paylaşım adı 3 ile 63 karakter arasında olmalıdır. 

**Önerilen çözünürlük:** Klasörü altında veri kopyaladığınız Data Box (SMB/NFS) paylaşımı, depolama hesabınızdaki bir Azure kapsayıcı haline gelir. 

- Üzerinde **Bağlan ve Kopyala** sayfasında Data Box yerel web kullanıcı Arabirimi, indirme ve klasörü belirlemek için hata dosyalarının adları ile ilgili sorunları gözden geçirin.
- Data Box paylaşım emin olmak için klasör adıyla değiştirin:

    - Adı 3 ila 63 karakter uzunluğunda olabilir.
    - Adları yalnızca harf, rakam ve kısa çizgi olabilir.
    - Adları başlatmak veya kısa çizgi ile bitmelidir.
    - Adlarında art arda kısa çizgi olamaz.
    - Geçerli adlar örnekleri: `my-folder-1`, `my-really-extra-long-folder-111`
    - Geçerli olmayan adları örnekleri: `my-folder_1`, `my`, `--myfolder`, `myfolder--`, `myfolder!`

    Daha fazla bilgi için bkz. Azure adlandırma kuralları için [kapsayıcı adları](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata#container-names) ve [paylaşım adları](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-shares--directories--files--and-metadata#share-names).


### <a name="errorcontainerorsharenamealphanumericdash"></a>ERROR_CONTAINER_OR_SHARE_NAME_ALPHA_NUMERIC_DASH

**Hata açıklaması:** Kapsayıcı veya paylaşım adında yalnızca harf, rakam veya kısa çizgi bulunmalıdır.

**Önerilen çözünürlük:** Klasörü altında veri kopyaladığınız Data Box (SMB/NFS) paylaşımı, depolama hesabınızdaki bir Azure kapsayıcı haline gelir. 

- Üzerinde **Bağlan ve Kopyala** sayfasında Data Box yerel web kullanıcı Arabirimi, indirme ve klasörü belirlemek için hata dosyalarının adları ile ilgili sorunları gözden geçirin.
- Data Box paylaşım emin olmak için klasör adıyla değiştirin:

    - Adı 3 ila 63 karakter uzunluğunda olabilir.
    - Adları yalnızca harf, rakam ve kısa çizgi olabilir.
    - Adları başlatmak veya kısa çizgi ile bitmelidir.
    - Adlarında art arda kısa çizgi olamaz.
    - Geçerli adlar örnekleri: `my-folder-1`, `my-really-extra-long-folder-111`
    - Geçerli olmayan adları örnekleri: `my-folder_1`, `my`, `--myfolder`, `myfolder--`, `myfolder!`

    Daha fazla bilgi için bkz. Azure adlandırma kuralları için [kapsayıcı adları](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata#container-names) ve [paylaşım adları](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-shares--directories--files--and-metadata#share-names).

### <a name="errorcontainerorsharenameimproperdash"></a>ERROR_CONTAINER_OR_SHARE_NAME_IMPROPER_DASH

**Hata açıklaması:** Kapsayıcı adları ve paylaşım adları başlatılamıyor veya kısa çizgi ile bitemez ve art arda kısa çizgi olamaz.

**Önerilen çözünürlük:** Klasörü altında veri kopyaladığınız Data Box (SMB/NFS) paylaşımı, depolama hesabınızdaki bir Azure kapsayıcı haline gelir. 

- Üzerinde **Bağlan ve Kopyala** sayfasında Data Box yerel web kullanıcı Arabirimi, indirme ve klasörü belirlemek için hata dosyalarının adları ile ilgili sorunları gözden geçirin.
- Data Box paylaşım emin olmak için klasör adıyla değiştirin:

    - Adı 3 ila 63 karakter uzunluğunda olabilir.
    - Adları yalnızca harf, rakam ve kısa çizgi olabilir.
    - Adları başlatmak veya kısa çizgi ile bitmelidir.
    - Adlarında art arda kısa çizgi olamaz.
    - Geçerli adlar örnekleri: `my-folder-1`, `my-really-extra-long-folder-111`
    - Geçerli olmayan adları örnekleri: `my-folder_1`, `my`, `--myfolder`, `myfolder--`, `myfolder!`

    Daha fazla bilgi için bkz. Azure adlandırma kuralları için [kapsayıcı adları](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata#container-names) ve [paylaşım adları](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-shares--directories--files--and-metadata#share-names).

### <a name="errorcontainerorsharenamedisallowedfortype"></a>ERROR_CONTAINER_OR_SHARE_NAME_DISALLOWED_FOR_TYPE

**Hata açıklaması:** Hatalı kapsayıcı adları için yönetilen disk paylaşımları belirtilir.

**Önerilen çözünürlük:** Her paylaşımı içinde yönetilen diskler için depolama hesabınızdaki kapsayıcılar, karşılık gelen aşağıdaki klasörler oluşturulur: Premium SSD, standart HDD ve SSD standart. Bu klasörler için yönetilen disk performans katmanı karşılık gelir.

- Sayfa blobu verileriniz (VHD) mevcut klasörlerden birine kopyaladığınızdan emin olun. Yalnızca bu mevcut kapsayıcılar verileri Azure'a karşıya yüklendi.
- Standart SSD Premium SSD ve HDD standart olarak aynı düzeyde oluşturulan herhangi bir klasör geçerli performans katmanına karşılık gelmesi gerekmez ve kullanılamaz.
- Performans katmanları dışında oluşturulan dosya ve klasörleri kaldırın.

Daha fazla bilgi için [kopyalama yönetilen disklere](data-box-deploy-copy-data-from-vhds.md#connect-to-data-box).

### <a name="errorcontainerorsharecapacityexceeded"></a>ERROR_CONTAINER_OR_SHARE_CAPACITY_EXCEEDED

**Hata açıklaması:** Azure dosya paylaşımı, 5 TB veri için bir paylaşım sınırlar. Bu sınır için bazı paylaşımları eşiğini aştı.

**Önerilen çözünürlük:** Üzerinde **Bağlan ve Kopyala** sayfa Data Box yerel Web kullanıcı Arabirimi, indirin ve hata dosyalarını gözden geçirin.

Bu sorundan Hata günlüklerini ve söz konusu klasördeki dosyalar 5 TB'altında olduğundan emin olun klasörleri tanımlayın.

### <a name="errorbloborfilenamecharactercontrol"></a>ERROR_BLOB_OR_FILE_NAME_CHARACTER_CONTROL

**Hata açıklaması:** Blob veya dosya adları, desteklenmeyen denetim karakterlerini içeremez.

**Önerilen çözünürlük:** Desteklenmeyen karakterleri içeren BLOB veya kopyaladığınız dosyaları içerir.

Üzerinde **Bağlan ve Kopyala** sayfasında yerel web kullanıcı Arabirimi, indirin ve hata dosyalarını gözden geçirin.
Kaldırın veya desteklenmeyen karakterler kaldırmak için dosyalarını yeniden adlandırın.

Daha fazla bilgi için bkz. Azure adlandırma kuralları için [blob adları](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata#blob-names) ve [dosya adları](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-shares--directories--files--and-metadata#directory-and-file-names).

### <a name="errorbloborfilenamecharacterillegal"></a>ERROR_BLOB_OR_FILE_NAME_CHARACTER_ILLEGAL

**Hata açıklaması:** Blob veya dosya adları geçersiz karakterler içeriyor.

**Önerilen çözünürlük:** Desteklenmeyen karakterleri içeren BLOB veya kopyaladığınız dosyaları içerir.

Üzerinde **Bağlan ve Kopyala** sayfasında yerel web kullanıcı Arabirimi, indirin ve hata dosyalarını gözden geçirin.
Kaldırın veya desteklenmeyen karakterler kaldırmak için dosyalarını yeniden adlandırın.

Daha fazla bilgi için bkz. Azure adlandırma kuralları için [blob adları](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata#blob-names) ve [dosya adları](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-shares--directories--files--and-metadata#directory-and-file-names).


### <a name="errorbloborfilenameending"></a>ERROR_BLOB_OR_FILE_NAME_ENDING

**Hata açıklaması:** Blob veya dosya adı geçersiz karakter ile sona erecektir.

**Önerilen çözünürlük:** Desteklenmeyen karakterleri içeren BLOB veya kopyaladığınız dosyaları içerir.

Üzerinde **Bağlan ve Kopyala** sayfasında yerel web kullanıcı Arabirimi, indirin ve hata dosyalarını gözden geçirin.
Kaldırın veya desteklenmeyen karakterler kaldırmak için dosyalarını yeniden adlandırın.

Daha fazla bilgi için bkz. Azure adlandırma kuralları için [blob adları](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata#blob-names) ve [dosya adları](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-shares--directories--files--and-metadata#directory-and-file-names).


### <a name="errorbloborfilenamesegmentcount"></a>ERROR_BLOB_OR_FILE_NAME_SEGMENT_COUNT

**Hata açıklaması:** Blob veya dosya adı çok fazla sayıda yol kesimi içeriyor.

**Önerilen çözünürlük:** BLOB'ları veya kopyaladığınız dosyalar en fazla yol bölümlerinin sayısını aşıyor. Bir yol kesimi dizedir arka arkaya sınırlayıcı karakter, örneğin, eğik /.

- Üzerinde **Bağlan ve Kopyala** sayfasında yerel web kullanıcı Arabirimi, indirin ve hata dosyalarını gözden geçirin.
- Emin olun [blob adları](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata#blob-names) ve [dosya adları](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-shares--directories--files--and-metadata#directory-and-file-names) Azure adlandırma kurallarına uygun.

### <a name="errorbloborfilenameaggregatelength"></a>ERROR_BLOB_OR_FILE_NAME_AGGREGATE_LENGTH

**Hata açıklaması:** Blob veya dosya adı çok uzun.

**Önerilen çözünürlük:** Blob veya dosya adları en fazla uzunluğu aşıyor.

- Üzerinde **Bağlan ve Kopyala** sayfasında yerel web kullanıcı Arabirimi, indirin ve hata dosyalarını gözden geçirin.
- Blob adı 1024 karakteri aşmamalıdır.
- Kaldırın veya adlarını 1024 karakteri aşmayan blob veya dosyalar yeniden adlandırın.

Daha fazla bilgi için Azure adlandırma kurallarına blob adları ve dosya adları için bkz.

### <a name="errorbloborfilenamecomponentlength"></a>ERROR_BLOB_OR_FILE_NAME_COMPONENT_LENGTH

**Hata açıklaması:** Blob veya dosya adı bölümlerinden biri çok uzun.

**Önerilen çözünürlük:** Yolun yol kesimleri blob veya dosya adında biri en fazla karakter sayıları aşıyor. Bir yol kesimi dizedir arka arkaya sınırlayıcı karakter, örneğin, eğik /.

- Üzerinde **Bağlan ve Kopyala** sayfasında yerel web kullanıcı Arabirimi, indirin ve hata dosyalarını gözden geçirin.
- Emin olun [blob adları](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata#blob-names) ve [dosya adları](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-shares--directories--files--and-metadata#directory-and-file-names) Azure adlandırma kurallarına uygun.

### <a name="errorbloborfilesizelimit"></a>ERROR_BLOB_OR_FILE_SIZE_LIMIT

**Hata açıklaması:** Dosya boyutu karşıya yüklenebilecek maksimum dosya boyutunu aşıyor.

**Önerilen çözünürlük:** Blob veya dosya boyutları karşıya yükleme için izin verilen en yüksek sınırı aşıyor.

- Üzerinde **Bağlan ve Kopyala** sayfasında yerel web kullanıcı Arabirimi, indirin ve hata dosyalarını gözden geçirin.
- Blob ve dosya boyutları Azure nesne boyutu sınırları aşmadığından emin olun.

### <a name="errorbloborfilesizealignment"></a>ERROR_BLOB_OR_FILE_SIZE_ALIGNMENT

**Hata açıklaması:** Blob veya dosya hatalı hizalanmış.

**Önerilen çözünürlük:** Sayfa blob paylaşımında 512 bayt Data Box yalnızca destekleyen dosyaları (örneğin, VHD/VHDX) hizalanır. Sayfa blob paylaşımına kopyaladığınız herhangi bir veri için Azure sayfa blobları yüklenir.

VHD/VHDX olmayan veriler sayfa blobu paylaşımından kaldırın. Blok blobu veya genel verileri için Azure dosya paylaşımlarını kullanabilirsiniz.

Daha fazla bilgi için [genel bakış, sayfa blobları](../storage/blobs/storage-blob-pageblob-overview.md).

### <a name="errorbloborfiletypeunsupported"></a>ERROR_BLOB_OR_FILE_TYPE_UNSUPPORTED

**Hata açıklaması:** Desteklenmeyen dosya türü bir yönetilen disk paylaşımına mevcuttur. Yalnızca sabit VHD'lerin izin verilir.

**Önerilen çözünürlük:**

- Yalnızca sabit VHD'lerin yönetilen disk oluşturmak için karşıya emin olun.
- VHDX dosyaları veya **dinamik** ve **fark kayıt** VHD'ler desteklenmez.

### <a name="errordirectorydisallowedfortype"></a>ERROR_DIRECTORY_DISALLOWED_FOR_TYPE

**Hata açıklaması:** Bir dizin önceden mevcut olan klasörlerin hiçbirini yönetilen diskler için izin verilmiyor. Bu klasörleri yalnızca sabit VHD'lerin izin verilir.

**Önerilen çözünürlük:** Her paylaşımı içinde yönetilen diskler için depolama hesabınızdaki kapsayıcılar, karşılık gelen, aşağıdaki üç klasör oluşturulur: Premium SSD, standart HDD ve SSD standart. Bu klasörler için yönetilen disk performans katmanı karşılık gelir.

- Sayfa blobu verileriniz (VHD) mevcut klasörlerden birine kopyaladığınızdan emin olun.
- Bir klasör veya dizin var olan bu klasörlerde izin verilmiyor. Önceden var olan bir klasör içinde oluşturduğunuz herhangi bir klasörde kaldırın.

Daha fazla bilgi için [kopyalama yönetilen disklere](data-box-deploy-copy-data-from-vhds.md#connect-to-data-box).


## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [veri kutusu Blob Depolama sistem gereksinimleri](data-box-system-requirements-rest.md).
