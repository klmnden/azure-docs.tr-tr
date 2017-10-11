---
title: "Bir Azure içeri/dışarı aktarma alma işi - v1 sabit sürücülerini prep için örnek iş akışı | Microsoft Docs"
description: "Sürücüleri Azure içeri/dışarı aktarma hizmetindeki bir içeri aktarma işi hazırlama tam işlemi için bkz."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 6eb1b1b7-c69f-4365-b5ef-3cd5e05eb72a
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: 179c6bac9a2d9509baa0007a7008d75d0874a25e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="sample-workflow-to-prepare-hard-drives-for-an-import-job"></a>Sabit sürücüleri içeri aktarma işine hazırlamak için örnek iş akışı
Bu konu, sürücüler için içeri aktarma işi hazırlama tam işlemi açıklanmaktadır.  
  
Bu örnek olarak aşağıdaki verileri adlı bir Windows Azure depolama hesabı alır `mystorageaccount`:  
  
|Konum|Açıklama|  
|--------------|-----------------|  
|H:\Video|Bir koleksiyonu videoları, 5 TB toplam.|  
|H:\Photo|Bir koleksiyonu fotoğrafları, toplam 30 GB.|  
|K:\Temp\FavoriteMovie.ISO|A Blu-ray™ disk görüntüsü, 25 GB.|  
|\\\bigshare\john\music|Müzik dosyalarının toplam 10 GB bir ağ paylaşımında koleksiyonu.|  
  
İçe aktarma işi bu veri depolama hesabındaki aşağıdaki hedefleri aktarır:  
  
|Kaynak|Hedef sanal dizin veya blob|  
|------------|-------------------------------------------|  
|H:\Video|https://mystorageaccount.BLOB.Core.Windows.NET/video|  
|H:\Photo|https://mystorageaccount.BLOB.Core.Windows.NET/Photo|  
|K:\Temp\FavoriteMovie.ISO|https://mystorageaccount.BLOB.Core.Windows.NET/favorite/FavoriteMovies.ISO|  
|\\\bigshare\john\music|https://mystorageaccount.BLOB.Core.Windows.NET/Music|  
  
Bu eşleme, dosya ile `H:\Video\Drama\GreatMovie.mov` blob içe `https://mystorageaccount.blob.core.windows.net/video/Drama/GreatMovie.mov`.  
  
Ardından, kaç tane sabit sürücüler gerekli belirlemek için verilerin boyutunu hesaplama:  
  
`5TB + 30GB + 25GB + 10GB = 5TB + 65GB`  
  
Bu örnekte, iki 3 TB sabit sürücü yeterli olacaktır. Ancak, kaynak dizin itibaren `H:\Video` 5 TB'lık veriyi varsa ve yalnızca 3 TB, tek sabit diskin kapasitesini ayırmak gerekli olan `H:\Video` iki küçük dizini içine: `H:\Video1` ve `H:\Video2`, Microsoft Azure çalıştırmadan önce İçeri/dışarı aktarma aracı. Bu adımı şu kaynak dizinlerini verir:  
  
|Konum|Boyut|Hedef sanal dizin veya blob|  
|--------------|----------|-------------------------------------------|  
|H:\Video1|2,5 TB|https://mystorageaccount.BLOB.Core.Windows.NET/video|  
|H:\Video2|2,5 TB|https://mystorageaccount.BLOB.Core.Windows.NET/video|  
|H:\Photo|30 GB|https://mystorageaccount.BLOB.Core.Windows.NET/Photo|  
|K:\Temp\FavoriteMovies.ISO|25 GB|https://mystorageaccount.BLOB.Core.Windows.NET/favorite/FavoriteMovies.ISO|  
|\\\bigshare\john\music|10 GB|https://mystorageaccount.BLOB.Core.Windows.NET/Music|  
  
 Olsa bile `H:\Video`dizin bölme iki dizini için depolama hesabı aynı hedef sanal dizine gidin. Bu şekilde tüm video dosyaları altında tek bir korunur `video` depolama hesabındaki kapsayıcı.  
  
 Ardından, önceki kaynak dizinleri iki sabit sürücü olacak şekilde eşit dağıtılır:  
  
||||  
|-|-|-|  
|Sabit sürücü|Kaynak dizinler|Toplam boyutu|  
|İlk sürücü|H:\Video1|2.5 TB + 30 GB|  
||H:\Photo||  
|İkinci sürücü|H:\Video2|2.5 TB + 35 GB|  
||K:\Temp\BlueRay.ISO||  
||\\\bigshare\john\music||  
  
Ayrıca, aşağıdaki meta verileri tüm dosyalar için ayarlayabilirsiniz:  
  
-   **UploadMethod:** Windows Azure içeri/dışarı aktarma hizmeti  
  
-   **DataSetName:** SampleData  
  
-   **CreationDate:** 1/10/2013  
  
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
  
-   **Content-Type:** application/octet-stream,  
  
-   **Content-MD5:** Q2hlY2sgSW50ZWdyaXR5IQ ==  
  
-   **Cache-Control:** no cache  
  
Bu özellikleri ayarlamak için bir metin dosyası oluşturun `c:\WAImportExport\SampleProperties.txt`:  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Properties>  
    <Content-Type>application/octet-stream</Content-Type>  
    <Content-MD5>Q2hlY2sgSW50ZWdyaXR5IQ==</Content-MD5>  
    <Cache-Control>no-cache</Cache-Control>  
</Properties>  
```
  
Şimdi iki sabit sürücü hazırlamak için Azure içeri/dışarı aktarma aracını çalıştırmak hazırsınız. Şunlara dikkat edin:  
  
-   İlk sürücü X sürücü olarak bağlı.  
  
-   İkinci sürücüyü Y sürücü olarak bağlı.  
  
-   Depolama hesabı anahtarı `mystorageaccount` olan `8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg==`.  

## <a name="preparing-disk-for-import-when-data-is-pre-loaded"></a>Veri önceden yüklendiğinde disk alma işlemi için hazırlanıyor
 
 İçeri aktarılacak veri disk üzerinde zaten bayrağı /skipwrite kullanın. /T ve /srcdir değerini alma için hazırlanan diske hem noktası gerekir. İçeri aktarılacak verilerin tümünü yok olduğuna aynı hedef sanal dizin veya kök depolama hesabının, her hedef dizini için aynı komutu ayrı ayrı /ID değerini tüm çalıştırmaları arasında aynı kalmasını çalıştırın.

>[!NOTE] 
>Bu diskte verileri silme şekilde Format belirtmeyin. Belirtin / şifrelemek veya olup disk zaten veya şifrelenmiş bağlı olarak /bk. 
>

```
    When data is already present on the disk for each drive run the following command.
    WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Video1 /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:x:\Video1 /dstdir:video/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt /skipwrite
```

## <a name="copy-sessions---first-drive"></a>Oturumlarını - Kopyala ilk sürücü

İlk sürücüsü için iki kez iki kaynak dizinleri kopyalamak için Azure içeri/dışarı aktarma aracını çalıştırın:  

**İlk kopya oturumu**
  
```
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Video1 /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:H:\Video1 /dstdir:video/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt  
```

**İkinci kopya oturumu**

```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Photo /srcdir:H:\Photo /dstdir:photo/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt
```

## <a name="copy-sessions---second-drive"></a>Oturumlarını - Kopyala ikinci sürücü
 
İkinci sürücüsü için Azure içeri/dışarı aktarma aracı üç kez, bir kez her kaynak dizinler için ve bir kez tek başına Blu-Ray™ görüntü dosyası için):  
  
**İlk kopya oturumu** 

```
WAImportExport.exe PrepImport /j:SecondDrive.jrn /id:Video2 /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:y /format /encrypt /srcdir:H:\Video2 /dstdir:video/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt  
```
  
**İkinci kopya oturumu**

```
WAImportExport.exe PrepImport /j:SecondDrive.jrn /id:Music /srcdir:\\bigshare\john\music /dstdir:music/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt  
```  
  
**Üçüncü kopya oturumu**  

```
WAImportExport.exe PrepImport /j:SecondDrive.jrn /id:BlueRayIso /srcfile:K:\Temp\BlueRay.ISO /dstblob:favorite/BlueRay.ISO /MetadataFile:c:\WAImportExport\SampleMetadata.txt /PropertyFile:c:\WAImportExport\SampleProperties.txt  
```

## <a name="copy-session-completion"></a>Kopya oturum tamamlama

Kopyalama oturumları tamamladıktan sonra iki sürücüsü kopyalama bilgisayardan bağlantısını kesmek ve uygun Windows Azure veri merkezine sevk. İki günlük dosyalarını karşıya `FirstDrive.jrn` ve `SecondDrive.jrn`, içeri aktarma işi oluşturduğunuzda [Windows Azure portalında](https://manage.windowsazure.com/).  
  
## <a name="next-steps"></a>Sonraki adımlar

* [Sabit sürücüleri içeri aktarma işine hazırlama](../storage-import-export-tool-preparing-hard-drives-import-v1.md)   
* [Sık kullanılan komutlar için hızlı başvuru](../storage-import-export-tool-quick-reference-v1.md) 
