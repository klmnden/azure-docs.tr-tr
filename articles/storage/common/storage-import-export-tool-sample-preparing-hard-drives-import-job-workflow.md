---
title: "Bir Azure içeri/dışarı aktarma alma işi sabit sürücülerini prep için örnek iş akışı | Microsoft Docs"
description: "Sürücüleri Azure içeri/dışarı aktarma hizmetindeki bir içeri aktarma işi hazırlama tam işlemi için bkz."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/07/2017
ms.author: muralikk
ms.openlocfilehash: 60139ff36b66432620591ceaf201e046ad30217f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="sample-workflow-to-prepare-hard-drives-for-an-import-job"></a>Sabit sürücüleri içeri aktarma işine hazırlamak için örnek iş akışı

Bu makalede, sürücüler için içeri aktarma işi hazırlama tam işlemi açıklanmaktadır.

## <a name="sample-data"></a>Örnek veriler

Bu örnek olarak aşağıdaki verileri adlı bir Azure storage hesabınıza aktarır `mystorageaccount`:

|Konum|Açıklama|Veri boyutu|
|--------------|-----------------|-----|
|H:\Video\ |Videolar koleksiyonu|12 TB|
|H:\Photo\ |Fotoğraf koleksiyonu|30 GB|
|K:\Temp\FavoriteMovie.ISO|A Blu-ray™ disk görüntüsü|25 GB|
|\\\bigshare\john\music\ |Müzik dosyalarını bir ağ paylaşımına koleksiyonu|10 GB|

## <a name="storage-account-destinations"></a>Depolama hesabı hedefleri

İçe aktarma işi verileri depolama hesabındaki aşağıdaki hedefleri alın:

|Kaynak|Hedef sanal dizin veya blob|
|------------|-------------------------------------------|
|H:\Video\ |Video /|
|H:\Photo\ |Fotoğraf /|
|K:\Temp\FavoriteMovie.ISO|favorite/FavoriteMovies.ISO|
|\\\bigshare\john\music\ |Müzik|

Bu eşleme, dosya ile `H:\Video\Drama\GreatMovie.mov` blob içeri aktarılacak `https://mystorageaccount.blob.core.windows.net/video/Drama/GreatMovie.mov`.

## <a name="determine-hard-drive-requirements"></a>Sabit sürücü gereksinimlerini belirleyin

Ardından, kaç tane sabit sürücüler gerekli belirlemek için verilerin boyutunu hesaplama:

`12TB + 30GB + 25GB + 10GB = 12TB + 65GB`

Bu örnekte, iki 8TB sabit sürücü yeterli olacaktır. Ancak, kaynak dizin itibaren `H:\Video` yalnızca 8 TB, tek sabit diskin kapasitesini 12 TB'lık veriyi sahipse ve bu yolla da aşağıdakileri belirtin kuramaz **driveset.csv** dosyası:

```
DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
X,Format,SilentMode,Encrypt,
Y,Format,SilentMode,Encrypt,
```
Aracın en iyi duruma getirilmiş bir şekilde iki sabit sürücüde veri dağıtın.

## <a name="attach-drives-and-configure-the-job"></a>Sürücüleri bağlayın ve iş yapılandırın
Her iki diskin makineye ekleyebilir ve birimler oluşturabilir. Ardından Yazar **dataset.csv** dosyası:
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
* **CreationDate:** 1/10/2013

İçeri aktarılan dosyalar için meta veri ayarlamak için bir metin dosyası oluşturun `c:\WAImportExport\SampleMetadata.txt`, aşağıdaki içeriğe sahip:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Metadata>
    <UploadMethod>Windows Azure Import/Export service</UploadMethod>
    <DataSetName>SampleData</DataSetName>
    <CreationDate>10/1/2013</CreationDate>
</Metadata>
```

Bazı özellikler için de ayarlayabilirsiniz `FavoriteMovie.ISO` blob:

* **Content-Type:** application/octet-stream,
* **Content-MD5:** Q2hlY2sgSW50ZWdyaXR5IQ ==
* **Cache-Control:** no cache

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

Şimdi iki sabit sürücü hazırlamak için Azure içeri/dışarı aktarma aracını çalıştırmak hazırsınız.

**İlk oturum için:**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#1  /sk:************* /InitialDriveSet:driveset-1.csv /DataSet:dataset-1.csv /logdir:F:\logs
```

Daha fazla veri eklenmesi gerekiyorsa, başka bir veri kümesi dosyasını (Initialdataset ile aynı biçimi) oluşturun.

**İkinci oturum için:**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2  /DataSet:dataset-2.csv
```

Kopyalama oturumları tamamladıktan sonra iki sürücü kopyalama bilgisayardan bağlantısını kesmek ve uygun Azure veri merkezine sevk. İki günlük dosyalarını yükleyeceğiniz `<FirstDriveSerialNumber>.xml` ve `<SecondDriveSerialNumber>.xml`, Azure portalında alma işi oluşturduğunuzda.

## <a name="next-steps"></a>Sonraki adımlar

* [Sabit sürücüleri içeri aktarma işine hazırlama](../storage-import-export-tool-preparing-hard-drives-import.md)
* [Sık kullanılan komutlar için hızlı başvuru](../storage-import-export-tool-quick-reference.md)
