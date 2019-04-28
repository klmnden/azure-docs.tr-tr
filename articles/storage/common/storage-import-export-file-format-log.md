---
title: Azure içeri/dışarı aktarma günlük dosyası biçimi | Microsoft Docs
description: Adımlar için bir içeri/dışarı aktarma hizmeti işi çalıştırıldığında oluşturulan günlük dosyalarının biçimi hakkında bilgi edinin.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.subservice: common
ms.openlocfilehash: 00e226134039d29efd744290c4bc63abd50adc89
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61478615"
---
# <a name="azure-importexport-service-log-file-format"></a>Azure içeri/dışarı aktarma hizmeti günlük dosyası biçimi
Microsoft Azure içeri/dışarı aktarma hizmeti içeri aktarma işi veya dışarı aktarma işi bir parçası olarak bir sürücüde bir eylem gerçekleştirdiğinde, günlükleri blok blobları, işi ile ilişkili depolama hesabına yazılır.  
  
İçeri/dışarı aktarma hizmeti tarafından yazılabilir iki günlükleri şunlardır:  
  
-   Hata günlüğü, her zaman bir hata olması durumunda oluşturulur.  
  
-   Ayrıntılı günlük varsayılan olarak etkin değildir, ancak ayarlayarak etkinleştirilebilir `EnableVerboseLog` özelliği bir [Put işlemini](/rest/api/storageimportexport/jobs) veya [güncelleştirme işi özellikleri](/rest/api/storageimportexport/jobs) işlemi.  
  
## <a name="log-file-location"></a>Günlük dosyası konumu  
Blok blobları kapsayıcı veya sanal dizin tarafından belirtilen günlüklere yazılır `ImportExportStatesPath` üzerinde ayarlanan ayarı bir `Put Job` işlemi. Günlüklere yazılır konumu nasıl kimlik doğrulaması için belirtilen değer ile birlikte bir iş için belirtilen üzerinde bağlıdır `ImportExportStatesPath`. İş için kimlik doğrulaması, bir depolama hesabı anahtarı veya SAS (paylaşılan erişim imzası) bir kapsayıcı aracılığıyla belirtilebilir.  
  
Kapsayıcı veya sanal dizin adını ya da varsayılan adı olabilir. `waimportexport`, veya başka bir kapsayıcı veya belirttiğiniz sanal dizin adı.  
  
Aşağıdaki tabloda, olası seçeneklerin gösterilmiştir:  
  
|Kimlik Doğrulama Yöntemi|Değeri `ImportExportStatesPath`öğesi|Günlük Bloblarını konumu|  
|---------------------------|----------------------------------------------|---------------------------|  
|Depolama hesabı anahtarı|Varsayılan değer|Adlı bir kapsayıcı `waimportexport`, varsayılan kapsayıcı olduğu. Örneğin:<br /><br /> `https://myaccount.blob.core.windows.net/waimportexport`|  
|Depolama hesabı anahtarı|Kullanıcı tarafından belirtilen değeri|Kullanıcı tarafından adlı bir kapsayıcı. Örneğin:<br /><br /> `https://myaccount.blob.core.windows.net/mylogcontainer`|  
|Kapsayıcı SAS|Varsayılan değer|Adlı bir sanal dizin `waimportexport`, varsayılan adı, SAS içinde belirtilen kapsayıcısı altında.<br /><br /> Örneğin, SAS için belirtilen iş ise `https://myaccount.blob.core.windows.net/mylogcontainer?sv=2012-02-12&se=2015-05-22T06%3A54%3A55Z&sr=c&sp=wl&sig=sigvalue`, günlük konumu şu şekilde olacaktır `https://myaccount.blob.core.windows.net/mylogcontainer/waimportexport`|  
|Kapsayıcı SAS|Kullanıcı tarafından belirtilen değeri|SAS içinde belirtilen kapsayıcısı altında kullanıcı tarafından adlandırılan bir sanal dizin.<br /><br /> Örneğin, SAS için belirtilen iş ise `https://myaccount.blob.core.windows.net/mylogcontainer?sv=2012-02-12&se=2015-05-22T06%3A54%3A55Z&sr=c&sp=wl&sig=sigvalue`, ve belirtilen sanal dizin adlı `mylogblobs`, günlük konumu şu şekilde olacaktır `https://myaccount.blob.core.windows.net/mylogcontainer/waimportexport/mylogblobs`.|  
  
Çağırarak ayrıntılı günlükleri ve hata URL'sini alabilirsiniz [alma işi](/rest/api/storageimportexport/jobs) işlemi. Günlükleri, sürücünün işleme tamamlandıktan sonra kullanılabilir.  
  
## <a name="log-file-format"></a>Günlük dosyası biçimi  
Her iki günlük için'biçim aynıdır: XML açıklamaları sabit sürücü ve müşterinin hesap arasında BLOB kopyalanırken oluşan olaylar içeren bir blob.  
  
Hata günlüğü yalnızca bloblar veya içeri aktarma sırasında hatalarla karşılaştı dosyaları bilgisini içerirken ayrıntılı günlüğü her blob (için içeri aktarma işine) veya (dışarı aktarma işi için), dosya kopyalama işleminin durumunu ilgili eksiksiz bilgi içeriyor veya dışarı aktarma işi.  
  
Ayrıntılı günlük biçimi, aşağıda gösterilmiştir. Hata günlüğü aynı yapıya sahiptir, ancak başarılı işlemleri filtreler.  

```xml
<DriveLog Version="2014-11-01">  
  <DriveId>drive-id</DriveId>  
  [<Blob Status="blob-status">  
   <BlobPath>blob-path</BlobPath>  
   <FilePath>file-path</FilePath>  
   [<Snapshot>snapshot</Snapshot>]  
   <Length>length</Length>  
   [<LastModified>last-modified</LastModified>]  
   [<ImportDisposition Status="import-disposition-status">import-disposition</ImportDisposition>]  
   [page-range-list-or-block-list]  
   [metadata-status]  
   [properties-status]  
  </Blob>]  
  [<Blob>  
    . . .  
  </Blob>]  
  <Status>drive-status</Status>  
</DriveLog>  
  
page-range-list-or-block-list ::= 
  page-range-list | block-list  
  
page-range-list ::=   
<PageRangeList>  
      [<PageRange Offset="page-range-offset" Length="page-range-length"   
       [Hash="md5-hash"] Status="page-range-status"/>]  
      [<PageRange Offset="page-range-offset" Length="page-range-length"   
       [Hash="md5-hash"] Status="page-range-status"/>]  
</PageRangeList>  
  
block-list ::=  
<BlockList>  
      [<Block Offset="block-offset" Length="block-length" [Id="block-id"]  
       [Hash="md5-hash"] Status="block-status"/>]  
      [<Block Offset="block-offset" Length="block-length" [Id="block-id"]   
       [Hash="md5-hash"] Status="block-status"/>]  
</BlockList>  
  
metadata-status ::=  
<Metadata Status="metadata-status">  
   [<GlobalPath Hash="md5-hash">global-metadata-file-path</GlobalPath>]  
   [<Path Hash="md5-hash">metadata-file-path</Path>]  
</Metadata>  
  
properties-status ::=  
<Properties Status="properties-status">  
   [<GlobalPath Hash="md5-hash">global-properties-file-path</GlobalPath>]  
   [<Path Hash="md5-hash">properties-file-path</Path>]  
</Properties>  
```

Aşağıdaki tabloda, günlük dosyasının öğeleri açıklar.  
  
|XML öğesi|Tür|Açıklama|  
|-----------------|----------|-----------------|  
|`DriveLog`|XML öğesi|Bir sürücü günlük temsil eder.|  
|`Version`|Öznitelik, dize|Günlük biçimi sürümü.|  
|`DriveId`|String|Sürücünün donanım seri numarası.|  
|`Status`|String|Sürücü işlem durumu. Bkz: `Drive Status Codes` daha fazla bilgi için tablo aşağıda.|  
|`Blob`|İç içe geçmiş XML öğesi|Bir blob temsil eder.|  
|`Blob/BlobPath`|String|Blob URI'si.|  
|`Blob/FilePath`|String|Sürücüdeki dosyaya göreli yol.|  
|`Blob/Snapshot`|DateTime|Yalnızca bir dışarı aktarma işi için blob anlık görüntü sürümü.|  
|`Blob/Length`|Tamsayı|Blob bayt cinsinden uzunluğu.|  
|`Blob/LastModified`|DateTime|Blob son değiştirildiği, tarih yalnızca bir dışarı aktarma işi için.|  
|`Blob/ImportDisposition`|String|Yalnızca içeri aktarma işi için blob alma eğilimini.|  
|`Blob/ImportDisposition/@Status`|Öznitelik, dize|İçeri aktarma değerlendirme durumu.|  
|`PageRangeList`|İç içe geçmiş XML öğesi|Bir sayfa blobu için sayfa aralıklarını listesini temsil eder.|  
|`PageRange`|XML öğesi|Sayfa aralığını temsil eder.|  
|`PageRange/@Offset`|Öznitelik, bir tamsayı|Blob sayfa aralığında başlangıç uzaklığı.|  
|`PageRange/@Length`|Öznitelik, bir tamsayı|Sayfa aralığı bayt cinsinden uzunluğu.|  
|`PageRange/@Hash`|Öznitelik, dize|Base16 kodlu MD5 karması sayfa aralığı.|  
|`PageRange/@Status`|Öznitelik, dize|Sayfa aralığı işlem durumu.|  
|`BlockList`|İç içe geçmiş XML öğesi|Bir blok blobu için blok listesini temsil eder.|  
|`Block`|XML öğesi|Bir blok temsil eder.|  
|`Block/@Offset`|Öznitelik, bir tamsayı|Blok BLOB başlangıç uzaklığı.|  
|`Block/@Length`|Öznitelik, bir tamsayı|Blok bayt cinsinden uzunluğu.|  
|`Block/@Id`|Öznitelik, dize|Blok kimliği.|  
|`Block/@Hash`|Öznitelik, dize|Bloğun Base16 kodlu MD5 karması.|  
|`Block/@Status`|Öznitelik, dize|Blok işlem durumu.|  
|`Metadata`|İç içe geçmiş XML öğesi|Blob meta verileri temsil eder.|  
|`Metadata/@Status`|Öznitelik, dize|Blob meta verilerini işlenmesini durumu.|  
|`Metadata/GlobalPath`|String|Genel meta veri dosyası için göreli yol.|  
|`Metadata/GlobalPath/@Hash`|Öznitelik, dize|Base16 kodlu MD5 karması genel meta veri dosyası.|  
|`Metadata/Path`|String|Meta veri dosyası için göreli yol.|  
|`Metadata/Path/@Hash`|Öznitelik, dize|Base16 kodlu MD5 karması meta veri dosyası.|  
|`Properties`|İç içe geçmiş XML öğesi|Blob özelliklerini temsil eder.|  
|`Properties/@Status`|Öznitelik, dize|Blob özelliklerini, örneğin dosya bulunamadı, işleme durumu tamamlandı.|  
|`Properties/GlobalPath`|String|Genel Özellikler dosyasının göreli yolu.|  
|`Properties/GlobalPath/@Hash`|Öznitelik, dize|Base16 kodlu MD5 karması genel özellikler dosyası.|  
|`Properties/Path`|String|Özellikler dosyasına göreli yol.|  
|`Properties/Path/@Hash`|Öznitelik, dize|Base16 kodlu MD5 karması özellikler dosyası.|  
|`Blob/Status`|String|Blob işlem durumu.|  
  
## <a name="drive-status-codes"></a>Sürücü durum kodları  
Aşağıdaki tablo, bir sürücü işlemek için durum kodları listeler.  
  
|Durum kodu|Açıklama|  
|-----------------|-----------------|  
|`Completed`|Sürücü işleme herhangi bir hata olmadan tamamlandığı anlamına gelir.|  
|`CompletedWithWarnings`|Sürücü işleme bloblar için belirtilen içeri aktarma değerlendirmeleri başına bir veya daha fazla blob'larda uyarılarla tamamlandı.|  
|`CompletedWithErrors`|Sürücü, bir veya daha fazla bloblar veya öbekleri, hatalarla tamamlandı.|  
|`DiskNotFound`|Hiçbir disk sürücüsünde bulunur.|  
|`VolumeNotNtfs`|Disk üzerindeki ilk veri birimi NTFS biçimde değil.|  
|`DiskOperationFailed`|Sürücüde işlemleri gerçekleştirilirken bilinmeyen bir hata oluştu.|  
|`BitLockerVolumeNotFound`|BitLocker encryptable birim bulunamadı.|  
|`BitLockerNotActivated`|Birimde BitLocker etkin değil.|  
|`BitLockerProtectorNotFound`|Sayısal parola anahtar koruyucusu birimde yok.|  
|`BitLockerKeyInvalid`|Sağlanan sayısal parola, birimin kilidi açılamıyor.|  
|`BitLockerUnlockVolumeFailed`|Birimin kilidini açmanız çalışılırken bilinmeyen bir hata oluştu.|  
|`BitLockerFailed`|BitLocker işlemleri gerçekleştirilirken bilinmeyen bir hata oluştu.|  
|`ManifestNameInvalid`|Bildirim dosyası adı geçersiz.|  
|`ManifestNameTooLong`|Bildirim dosyası adı çok uzun.|  
|`ManifestNotFound`|Bildirim dosyası bulunamadı.|  
|`ManifestAccessDenied`|Bildirim dosyasına erişim reddedildi.|  
|`ManifestCorrupted`|Bildirim dosyası bozuk (içerik karması eşleşmiyor).|  
|`ManifestFormatInvalid`|Bildirim içeriği gerekli biçime uymuyor.|  
|`ManifestDriveIdMismatch`|Sürücü Kimliği bildirimi dosyasındaki sürücüden bir okuma eşleşmiyor.|  
|`ReadManifestFailed`|Bildirimi okunurken bir disk g/ç hatası oluştu.|  
|`BlobListFormatInvalid`|Dışarı aktarma blob listesi blob gerekli biçime uymuyor.|  
|`BlobRequestForbidden`|BLOB Depolama hesabındaki erişim engellendi. Bu, geçersiz depolama hesabı anahtarı veya SAS kapsayıcı nedeniyle olabilir.|  
|`InternalError`|Ve sürücü işlenirken iç hata oluştu.|  
  
## <a name="blob-status-codes"></a>BLOB durum kodları  
Aşağıdaki tablo, bir blob işlemek için durum kodları listeler.  
  
|Durum kodu|Açıklama|  
|-----------------|-----------------|  
|`Completed`|Blob işleme hata olmadan tamamlandığı anlamına gelir.|  
|`CompletedWithErrors`|Blob işleme bir veya daha fazla sayfa aralıklarını veya blokları, meta verileri veya özellikleri hatalarla tamamlandı.|  
|`FileNameInvalid`|Dosya adı geçersiz.|  
|`FileNameTooLong`|Dosya adı çok uzun.|  
|`FileNotFound`|Dosya bulunamadı.|  
|`FileAccessDenied`|Dosyaya erişim reddedildi.|  
|`BlobRequestFailed`|Bloba erişmek için Blob hizmeti isteği başarısız oldu.|  
|`BlobRequestForbidden`|Blob hizmeti isteği bloba erişmek yasaktır. Bu, geçersiz depolama hesabı anahtarı veya SAS kapsayıcı nedeniyle olabilir.|  
|`RenameFailed`|Blob (için içeri aktarma işine) veya dosyası (dışarı aktarma işi) yeniden adlandırmak başarısız oldu.|  
|`BlobUnexpectedChange`|Blob (için dışarı aktarma işi) ile beklenmedik bir değişikliği oluştu.|  
|`LeasePresent`|Mevcut blob üzerinde kiralama yoktur.|  
|`IOFailed`|Blob işlenirken bir disk veya ağ g/ç hatası oluştu.|  
|`Failed`|Blob işlenirken bilinmeyen bir hata oluştu.|  
  
## <a name="import-disposition-status-codes"></a>Alma eğilimi durum kodları  
Aşağıdaki tabloda, bir içeri aktarma değerlendirme çözmek için durum kodları listeler.  
  
|Durum kodu|Açıklama|  
|-----------------|-----------------|  
|`Created`|Blob oluşturuldu.|  
|`Renamed`|Blob yeniden adlandırma alma eğilimi yeniden adlandırıldı. `Blob/BlobPath` Öğe yeniden adlandırılmış blob URI'si içeriyor.|  
|`Skipped`|Blob başına atlandı `no-overwrite` değerlendirme içeri aktarın.|  
|`Overwritten`|Blob başına mevcut bir bloba geçersiz kıldı `overwrite` değerlendirme içeri aktarın.|  
|`Cancelled`|Önceki hata alma eğilimini başka işlem durduruldu.|  
  
## <a name="page-rangeblock-status-codes"></a>Sayfa aralık/blok durum kodları  
Aşağıdaki tablo, bir sayfa aralığını veya bloğunu işlemek için durum kodları listeler.  
  
|Durum kodu|Açıklama|  
|-----------------|-----------------|  
|`Completed`|Blok ve sayfa aralığı işleme herhangi bir hata olmadan tamamlandığı anlamına gelir.|  
|`Committed`|Blok kaydedildi, ancak değil tam bloğunda listesi diğer bloklardan başarısız oldu veya tam bir Engellenenler listesi kendisini koymak için başarısız oldu.|  
|`Uncommitted`|Blok karşıya ancak edilmemiş.|  
|`Corrupted`|Blok ve sayfa aralığı bozuk (içerik karması eşleşmiyor).|  
|`FileUnexpectedEnd`|Bir beklenmeyen dosya sonu oluştu.|  
|`BlobUnexpectedEnd`|Blob beklenmeyen dosya sonuyla karşılaşıldı.|  
|`BlobRequestFailed`|Blok ve sayfa aralığı erişmek için Blob hizmeti isteği başarısız oldu.|  
|`IOFailed`|Blok ve sayfa aralığı işlenirken bir disk veya ağ g/ç hatası oluştu.|  
|`Failed`|Blok ve sayfa aralığı işlenirken bilinmeyen bir hata oluştu.|  
|`Cancelled`|Önceki bir hata sayfası aralık veya blok başka işlem durduruldu.|  
  
## <a name="metadata-status-codes"></a>Meta veri durum kodları  
Aşağıdaki tablo, blob meta verilerini işlemek için durum kodları listeler.  
  
|Durum kodu|Açıklama|  
|-----------------|-----------------|  
|`Completed`|Meta veri işleme hata olmadan tamamlandığı anlamına gelir.|  
|`FileNameInvalid`|Meta veri dosyası adı geçersiz.|  
|`FileNameTooLong`|Meta veri dosyası adı çok uzun.|  
|`FileNotFound`|Meta veri dosyası bulunamadı.|  
|`FileAccessDenied`|Meta veri dosyasına erişim reddedildi.|  
|`Corrupted`|Meta veri dosyası bozuk (içerik karması eşleşmiyor).|  
|`XmlReadFailed`|Meta veri içeriği gerekli biçime uymuyor.|  
|`XmlWriteFailed`|Meta verileri yazarken XML başarısız oldu.|  
|`BlobRequestFailed`|Meta verilerine erişmek için Blob hizmeti isteği başarısız oldu.|  
|`IOFailed`|Meta verileri işlenirken bir disk veya ağ g/ç hatası oluştu.|  
|`Failed`|Meta verileri işlenirken bilinmeyen bir hata oluştu.|  
|`Cancelled`|Önceki hata meta verileri başka işlem durduruldu.|  
  
## <a name="properties-status-codes"></a>Özellikler durum kodları  
Aşağıdaki tablo, blob özelliklerini işlemek için durum kodları listeler.  
  
|Durum kodu|Açıklama|  
|-----------------|-----------------|  
|`Completed`|Özellikleri işleme herhangi bir hata olmadan tamamlandı.|  
|`FileNameInvalid`|Özellikleri dosya adı geçersiz.|  
|`FileNameTooLong`|Özellikleri dosya adı çok uzun.|  
|`FileNotFound`|Özellikler dosyası bulunamadı.|  
|`FileAccessDenied`|Özellikler dosyasına erişim reddedildi.|  
|`Corrupted`|Özellikler dosyası bozuk (içerik karması eşleşmiyor).|  
|`XmlReadFailed`|Özellikleri içeriği gerekli biçime uymuyor.|  
|`XmlWriteFailed`|Özellikleri yazma XML başarısız oldu.|  
|`BlobRequestFailed`|Özelliklerine erişmek için Blob hizmeti isteği başarısız oldu.|  
|`IOFailed`|Özellikleri işlenirken bir disk veya ağ g/ç hatası oluştu.|  
|`Failed`|Özellikleri işlenirken bilinmeyen bir hata oluştu.|  
|`Cancelled`|Önceki hata özelliklerini başka işlem durduruldu.|  
  
## <a name="sample-logs"></a>Örnek günlükler  
Ayrıntılı günlüğü örneği verilmiştir.  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<DriveLog Version="2014-11-01">  
    <DriveId>WD-WMATV123456</DriveId>  
    <Blob Status="Completed">  
       <BlobPath>pictures/bob/wild/desert.jpg</BlobPath>  
       <FilePath>\Users\bob\Pictures\wild\desert.jpg</FilePath>  
       <Length>98304</Length>  
       <ImportDisposition Status="Created">overwrite</ImportDisposition>  
       <BlockList>  
          <Block Offset="0" Length="65536" Id="AAAAAA==" Hash=" 9C8AE14A55241F98533C4D80D85CDC68" Status="Completed"/>  
          <Block Offset="65536" Length="32768" Id="AQAAAA==" Hash=" DF54C531C9B3CA2570FDDDB3BCD0E27D" Status="Completed"/>  
       </BlockList>  
       <Metadata Status="Completed">  
          <GlobalPath Hash=" E34F54B7086BCF4EC1601D056F4C7E37">\Users\bob\Pictures\wild\metadata.xml</GlobalPath>  
       </Metadata>  
    </Blob>  
    <Blob Status="CompletedWithErrors">  
       <BlobPath>pictures/bob/animals/koala.jpg</BlobPath>  
       <FilePath>\Users\bob\Pictures\animals\koala.jpg</FilePath>  
       <Length>163840</Length>  
       <ImportDisposition Status="Overwritten">overwrite</ImportDisposition>  
       <PageRangeList>  
          <PageRange Offset="0" Length="65536" Hash="19701B8877418393CB3CB567F53EE225" Status="Completed"/>  
          <PageRange Offset="65536" Length="65536" Hash="AA2585F6F6FD01C4AD4256E018240CD4" Status="Corrupted"/>  
          <PageRange Offset="131072" Length="4096" Hash="9BA552E1C3EEAFFC91B42B979900A996" Status="Completed"/>  
       </PageRangeList>  
       <Properties Status="Completed">  
          <Path Hash="38D7AE80653F47F63C0222FEE90EC4E7">\Users\bob\Pictures\animals\koala.jpg.properties</Path>  
       </Properties>  
    </Blob>  
    <Status>CompletedWithErrors</Status>  
</DriveLog>  
```  
  
Karşılık gelen hata günlüğünü aşağıda gösterilmiştir.  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<DriveLog Version="2014-11-01">  
    <DriveId>WD-WMATV6965824</DriveId>  
    <Blob Status="CompletedWithErrors">  
       <BlobPath>pictures/bob/animals/koala.jpg</BlobPath>  
       <FilePath>\Users\bob\Pictures\animals\koala.jpg</FilePath>  
       <Length>163840</Length>  
       <ImportDisposition Status="Overwritten">overwrite</ImportDisposition>  
       <PageRangeList>  
          <PageRange Offset="65536" Length="65536" Hash="AA2585F6F6FD01C4AD4256E018240CD4" Status="Corrupted"/>  
       </PageRangeList>  
    </Blob>  
    <Status>CompletedWithErrors</Status>  
</DriveLog>  
```

 İzleme günlükleri içeri aktarma işi için bir dosya içeri aktarma sürücüsünde bulunamadı hakkında bir hata içeriyor. Sonraki bileşenlerinin durumu olduğuna dikkat edin `Cancelled`.  
  
```xml
<?xml version="1.0" encoding="utf-8"?>  
<DriveLog Version="2014-11-01">  
  <DriveId>9WM35C2V</DriveId>  
  <Blob Status="FileNotFound">  
    <BlobPath>pictures/animals/koala.jpg</BlobPath>  
    <FilePath>\animals\koala.jpg</FilePath>  
    <Length>30310</Length>  
    <ImportDisposition Status="Cancelled">rename</ImportDisposition>  
    <BlockList>  
      <Block Offset="0" Length="6062" Id="MD5/cAzn4h7VVSWXf696qp5Uaw==" Hash="700CE7E21ED55525977FAF7AAA9E546B" Status="Cancelled" />  
      <Block Offset="6062" Length="6062" Id="MD5/PEnGwYOI8LPLNYdfKr7kAg==" Hash="3C49C6C18388F0B3CB35875F2ABEE402" Status="Cancelled" />  
      <Block Offset="12124" Length="6062" Id="MD5/FG4WxqfZKuUWZ2nGTU2qVA==" Hash="146E16C6A7D92AE5166769C64D4DAA54" Status="Cancelled" />  
      <Block Offset="18186" Length="6062" Id="MD5/ZzibNDzr3IRBQENRyegeXQ==" Hash="67389B343CEBDC8441404351C9E81E5D" Status="Cancelled" />  
      <Block Offset="24248" Length="6062" Id="MD5/ZzibNDzr3IRBQENRyegeXQ==" Hash="67389B343CEBDC8441404351C9E81E5D" Status="Cancelled" />  
    </BlockList>  
  </Blob>  
  <Status>CompletedWithErrors</Status>  
</DriveLog>  
```

Aşağıdaki hata günlüğünü dışarı aktarma işi için blob içeriğini diske yazıldı, ancak blobun özellikleri dışarı aktarılırken bir hata oluştu gösterir.  
  
```xml
<?xml version="1.0" encoding="utf-8"?>  
<DriveLog Version="2014-11-01">  
  <DriveId>9WM35C3U</DriveId>  
  <Blob Status="CompletedWithErrors">  
    <BlobPath>pictures/wild/canyon.jpg</BlobPath>  
    <FilePath>\pictures\wild\canyon.jpg</FilePath>  
    <LastModified>2012-09-18T23:47:08Z</LastModified>  
    <Length>163840</Length>  
    <BlockList />  
    <Properties Status="Failed" />  
  </Blob>  
  <Status>CompletedWithErrors</Status>  
</DriveLog>  
```
  
## <a name="next-steps"></a>Sonraki adımlar
 
* [Depolama içeri/dışarı aktarma REST API'si](/rest/api/storageimportexport/)
