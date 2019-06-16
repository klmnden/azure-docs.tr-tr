---
title: Sabit sürücüleri için Azure içeri/dışarı aktarma içeri aktarma işine hazırlama için örnek iş akışı | Microsoft Docs
description: Tam sürücüleri için Azure içeri/dışarı aktarma hizmeti, içeri aktarma işine Hazırlama işlemi için bkz.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 04/07/2017
ms.author: muralikk
ms.subservice: common
ms.openlocfilehash: 42da285fbb55df43959506996bcde9cf547c2a22
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60320576"
---
# <a name="sample-workflow-to-prepare-hard-drives-for-an-import-job"></a>Sabit sürücüleri içeri aktarma işine hazırlamak için örnek iş akışı

Bu makalede, tüm sürücüleri için içeri aktarma işine hazırlama işleminin adım adım anlatılmaktadır.

## <a name="sample-data"></a>Örnek veriler

Bu örnek olarak aşağıdaki verileri, adlı bir Azure depolama hesabına içeri aktarır `mystorageaccount`:

|Location|Açıklama|Veri boyutu|
|--------------|-----------------|-----|
|H:\Video\ |Videoları koleksiyonu|12 TB|
|H:\Photo\ |Fotoğraf koleksiyonu|30 GB|
|K:\Temp\FavoriteMovie.ISO|A Blu-ray™ disk görüntüsü|25 GB|
|\\\bigshare\john\music\ |Bir ağ paylaşımındaki Müzik dosyalarımı koleksiyonu|10 GB|

## <a name="storage-account-destinations"></a>Depolama hesabını hedefler

İçeri aktarma işi depolama hesabındaki aşağıdaki hedefleri veri aktarma:

|source|Hedef sanal dizin veya blob|
|------------|-------------------------------------------|
|H:\Video\ |Video /|
|H:\Photo\ |Fotoğraf /|
|K:\Temp\FavoriteMovie.ISO|favorite/FavoriteMovies.ISO|
|\\\bigshare\john\music\ |Müzik|

Bu eşleme, dosya ile `H:\Video\Drama\GreatMovie.mov` blob olarak içeri aktarılacak `https://mystorageaccount.blob.core.windows.net/video/Drama/GreatMovie.mov`.

## <a name="determine-hard-drive-requirements"></a>Sabit sürücü gereksinimlerini belirleme

Ardından, kaç sabit sürücüler gerektiğini belirlemek için veri boyutu işlem:

`12TB + 30GB + 25GB + 10GB = 12TB + 65GB`

Bu örnekte, iki 8TB sabit sürücü yeterli olur. Ancak, kaynak dizin beri `H:\Video` 12 TB veri varsa ve yalnızca 8 TB, tek sabit sürücü kapasite olduğundan, bunu aşağıdaki şekilde giriş belirtmek mümkün olmayacak **driveset.csv** dosya:

```
DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
X,Format,SilentMode,Encrypt,
Y,Format,SilentMode,Encrypt,
```
Aracı bir en iyi duruma getirilmiş şekilde iki sabit sürücülerde veri dağıtın.

## <a name="attach-drives-and-configure-the-job"></a>Sürücüleri bağlayın ve iş yapılandırma
Makineye hem diski ve birimler oluşturun. Ardından Yazar **dataset.csv** dosyası:
```
BasePath,DstBlobPathOrPrefix,BlobType,Disposition,MetadataFile,PropertiesFile
H:\Video\,video/,BlockBlob,rename,None,H:\mydirectory\properties.xml
H:\Photo\,photo/,BlockBlob,rename,None,H:\mydirectory\properties.xml
K:\Temp\FavoriteVideo.ISO,favorite/FavoriteVideo.ISO,BlockBlob,rename,None,H:\mydirectory\properties.xml
\\myshare\john\music\,music/,BlockBlob,rename,None,H:\mydirectory\properties.xml
```

Ayrıca, aşağıdaki meta verileri tüm dosyalar için ayarlayabilirsiniz:

* **UploadMethod:** Windows Azure içeri/dışarı aktarma hizmeti
* **DataSetName:** SampleData
* **CreationDate:** 10/1/2013

İçeri aktarılan dosyaları için meta veri kümesi için bir metin dosyası oluşturun. `c:\WAImportExport\SampleMetadata.txt`, aşağıdaki içeriğe sahip:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Metadata>
    <UploadMethod>Windows Azure Import/Export service</UploadMethod>
    <DataSetName>SampleData</DataSetName>
    <CreationDate>10/1/2013</CreationDate>
</Metadata>
```

Bazı özellikler için de ayarlayabilirsiniz `FavoriteMovie.ISO` blob:

* **İçerik türü:** uygulama/octet-akış
* **İçerik MD5:** Q2hlY2sgSW50ZWdyaXR5IQ==
* **Cache-Control:** no-cache

Bu özellikleri ayarlamak için bir metin dosyası oluşturun `c:\WAImportExport\SampleProperties.txt`:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Properties>
    <Content-Type>application/octet-stream</Content-Type>
    <Content-MD5>Q2hlY2sgSW50ZWdyaXR5IQ==</Content-MD5>
    <Cache-Control>no-cache</Cache-Control>
</Properties>
```

## <a name="run-the-azure-importexport-tool-waimportexportexe"></a>Azure içeri/dışarı aktarma Aracı (WAImportExport.exe) çalıştırın

Şimdi iki sabit sürücüleri hazırlamak için Azure içeri/dışarı aktarma aracı çalıştırmak hazır olursunuz.

**İlk oturum için:**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#1  /sk:************* /InitialDriveSet:driveset-1.csv /DataSet:dataset-1.csv /logdir:F:\logs
```

Daha fazla veri eklenmesi gerekiyorsa, başka bir veri kümesi dosyası (ilk veri kümesi olarak aynı biçimi) oluşturun.

**İkinci oturum için:**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2  /DataSet:dataset-2.csv
```

Kopyalama oturumları tamamladıktan sonra sürücüleri kopya bilgisayar bağlantısını kesebilmeniz ve bunları en uygun Azure veri merkezine gönderin. İki günlük dosyalarını karşıya yükleyelim `<FirstDriveSerialNumber>.xml` ve `<SecondDriveSerialNumber>.xml`, Azure portalında içeri aktarma işi oluşturma.

## <a name="next-steps"></a>Sonraki adımlar

* [Sabit sürücüleri içeri aktarma işine hazırlama](../storage-import-export-tool-preparing-hard-drives-import.md)
* [Sık kullanılan komutlar için hızlı başvuru](../storage-import-export-tool-quick-reference.md)
