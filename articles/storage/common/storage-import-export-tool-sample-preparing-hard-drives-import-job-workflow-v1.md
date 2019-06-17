---
title: Azure içeri/dışarı aktarma içeri aktarma işine - v1 sabit disk hazırlama için örnek iş akışı | Microsoft Docs
description: Tam sürücüleri için Azure içeri/dışarı aktarma hizmeti, içeri aktarma işine Hazırlama işlemi için bkz.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.subservice: common
ms.openlocfilehash: b80ba1cbe168270ec591bdd38859408eae387bbf
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60320593"
---
# <a name="sample-workflow-to-prepare-hard-drives-for-an-import-job"></a>Sabit sürücüleri içeri aktarma işine hazırlamak için örnek iş akışı
Bu konuda, sürücüleri için içeri aktarma işine hazırlama tam işleminde size yol gösterir.  
  
Bu örnek olarak aşağıdaki verileri, adlı bir Windows Azure depolama hesabına içeri aktarır `mystorageaccount`:  
  
|Location|Açıklama|  
|--------------|-----------------|  
|H:\Video|Koleksiyonu videosu, 5 TB toplam.|  
|H:\Photo|Bir koleksiyonu fotoğrafları, toplamda 30 GB.|  
|K:\Temp\FavoriteMovie.ISO|A Blu-ray™ disk görüntüsü, 25 GB.|  
|\\\bigshare\john\music|Müzik dosyalarımı toplam 10 GB bir ağ paylaşımındaki bir koleksiyonu.|  
  
İçeri aktarma işi bu veriler depolama hesabında aşağıdaki hedefleri aktarır:  
  
|source|Hedef sanal dizin veya blob|  
|------------|-------------------------------------------|  
|H:\Video|https:\//mystorageaccount.blob.core.windows.net/video|  
|H:\Photo|https:\//mystorageaccount.blob.core.windows.net/photo|  
|K:\Temp\FavoriteMovie.ISO|https:\//mystorageaccount.blob.core.windows.net/favorite/FavoriteMovies.ISO|  
|\\\bigshare\john\music|https:\//mystorageaccount.blob.core.windows.net/music|  
  
Bu eşleme, dosya ile `H:\Video\Drama\GreatMovie.mov` blob HTTPS'ye alınır:\//mystorageaccount.blob.core.windows.net/video/Drama/GreatMovie.mov.  
  
Ardından, kaç sabit sürücüler gerektiğini belirlemek için veri boyutu işlem:  
  
`5TB + 30GB + 25GB + 10GB = 5TB + 65GB`  
  
Bu örnekte, iki 3 TB sabit sürücü yeterli olur. Ancak, kaynak dizin beri `H:\Video` 5 TB'lık veriniz varsa ve, tek sabit diskin kapasitesi yalnızca 3 TB ise kırmanız gereklidir `H:\Video` iki küçük dizini içine: `H:\Video1` ve `H:\Video2`, Microsoft Azure çalıştırmadan önce İçeri/dışarı aktarma aracı. Bu adım, aşağıdaki kaynak dizinleri verir:  
  
|Location|Boyut|Hedef sanal dizin veya blob|  
|--------------|----------|-------------------------------------------|  
|H:\Video1|2,5 TB|https:\//mystorageaccount.blob.core.windows.net/video|  
|H:\Video2|2,5 TB|https:\//mystorageaccount.blob.core.windows.net/video|  
|H:\Photo|30 GB|https:\//mystorageaccount.blob.core.windows.net/photo|  
|K:\Temp\FavoriteMovies.ISO|25 GB|https:\//mystorageaccount.blob.core.windows.net/favorite/FavoriteMovies.ISO|  
|\\\bigshare\john\music|10 GB|https:\//mystorageaccount.blob.core.windows.net/music|  
  
 Olsa da `H:\Video`dizin bölündü iki dizin için depolama hesabı aynı hedef sanal dizinine işaret edecek. Bu şekilde tüm video dosyaları tek bir korunur `video` depolama hesabında bir kapsayıcı.  
  
 Ardından, önceki kaynak dizinleri iki sabit sürücüler için eşit olarak dağıtılır:  
  
||||  
|-|-|-|  
|Sabit sürücü|Kaynak dizinleri|Toplam boyutu|  
|İlk sürücü|H:\Video1|2,5 TB + 30 GB|  
||H:\Photo||  
|İkinci sürücü|H:\Video2|2,5 TB + 35 GB|  
||K:\Temp\BlueRay.ISO||  
||\\\bigshare\john\music||  
  
Ayrıca, aşağıdaki meta verileri tüm dosyalar için ayarlayabilirsiniz:  
  
-   **UploadMethod:** Windows Azure içeri/dışarı aktarma hizmeti  
  
-   **DataSetName:** SampleData  
  
-   **CreationDate:** 10/1/2013  
  
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
  
-   **İçerik türü:** uygulama/octet-akış  
  
-   **İçerik MD5:** Q2hlY2sgSW50ZWdyaXR5IQ==  
  
-   **Cache-Control:** no-cache  
  
Bu özellikleri ayarlamak için bir metin dosyası oluşturun `c:\WAImportExport\SampleProperties.txt`:  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Properties>  
    <Content-Type>application/octet-stream</Content-Type>  
    <Content-MD5>Q2hlY2sgSW50ZWdyaXR5IQ==</Content-MD5>  
    <Cache-Control>no-cache</Cache-Control>  
</Properties>  
```
  
Şimdi iki sabit sürücüleri hazırlamak için Azure içeri/dışarı aktarma aracı çalıştırmak hazır olursunuz. Aşağıdakilere dikkat edin:  
  
-   İlk sürücü X sürücü olarak bağlanmıştır.  
  
-   İkinci sürücüsü, sürücü Y'olarak bağlanmıştır.  
  
-   Depolama hesabı anahtarı `mystorageaccount` olduğu `8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg==`.  

## <a name="preparing-disk-for-import-when-data-is-pre-loaded"></a>Verileri önceden yüklü değilse, diski alma işlemi için hazırlanıyor
 
 İçeri aktarılacak veri zaten diskte mevcut ise, bayrak /skipwrite kullanın. /T ve /srcdir değerini alma işlemi için hazırlanıyor diske hem noktası gerekir. Tüm verileri içeri aktarılacak düşülmemesini varsa aynı hedef sanal dizin veya depolama hesabının kök, her hedef dizini için aynı komutu /id değerini tüm çalıştırmaları arasında aynı kalmasını ayrı ayrı çalıştırın.

>[!NOTE] 
>Bu verileri disk üzerinde tekrarlamak gerekir Özetteki belirtmeyin. Belirtin / şifrelemek veya /bk olup disk zaten şifrelenmiş olup olmadığına bağlı olarak. 
>

```
    When data is already present on the disk for each drive run the following command.
    WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Video1 /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:x:\Video1 /dstdir:video/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt /skipwrite
```

## <a name="copy-sessions---first-drive"></a>Oturumlarının - kopyalama ilk sürücü

İlk sürücü için iki kez iki kaynak dizinleri kopyalamak için Azure içeri/dışarı aktarma aracı çalıştırın:  

**İlk kopya oturumu**
  
```
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Video1 /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:H:\Video1 /dstdir:video/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt  
```

**İkinci kopya oturumu**

```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Photo /srcdir:H:\Photo /dstdir:photo/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt
```

## <a name="copy-sessions---second-drive"></a>Oturumlarının - kopyalama ikinci sürücü
 
İkinci sürücüsü için Azure içeri/dışarı aktarma aracı üç kez, bir kez her kaynak dizinlerinin ve bir kez Blu-Ray™ tek başına resim dosyası için):  
  
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

## <a name="copy-session-completion"></a>Oturum tamamlama kopyalayın

Kopyalama oturumları tamamladıktan sonra sürücüleri kopya bilgisayar bağlantısını kesebilmeniz ve bunları uygun Windows Azure veri merkezine gönderin. İki günlük dosyaları karşıya yükleme `FirstDrive.jrn` ve `SecondDrive.jrn`, içeri aktarma işinin oluşturduğunuzda [Azure portalında](https://portal.azure.com).  
  
## <a name="next-steps"></a>Sonraki adımlar

* [Sabit sürücüleri içeri aktarma işine hazırlama](../storage-import-export-tool-preparing-hard-drives-import-v1.md)   
* [Sık kullanılan komutlar için hızlı başvuru](../storage-import-export-tool-quick-reference-v1.md) 
