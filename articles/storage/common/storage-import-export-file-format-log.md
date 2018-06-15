---
title: Azure içeri/dışarı aktarma günlük dosyası biçimi | Microsoft Docs
description: İçeri/dışarı aktarma hizmeti işi için adımları çalıştırıldığında oluşturulan günlük dosyalarını biçimi hakkında bilgi edinin.
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: ''
ms.assetid: 38cc16bd-ad55-4625-9a85-e1726c35fd1b
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: 16234ccaf13ce1d85cfd207ed4734e683070faa6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23874104"
---
# <a name="azure-importexport-service-log-file-format"></a>Azure içeri/dışarı aktarma hizmeti günlük dosyası biçimi
Microsoft Azure içeri/dışarı aktarma hizmeti bir alma işi veya bir dışarı aktarma işinin parçası olarak bir sürücüde bir eylem gerçekleştirdiğinde, bu işle ilişkili depolama hesabındaki BLOB engellemek için günlüklerine yazılır.  
  
İçeri/dışarı aktarma hizmeti tarafından yazılan iki günlükleri şunlardır:  
  
-   Hata günlüğü her zaman bir hata durumunda oluşturulur.  
  
-   Ayrıntılı günlük varsayılan olarak etkin değildir, ancak ayarlayarak etkinleştirilebilir `EnableVerboseLog` özelliği bir [Put işlemini](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) veya [güncelleştirme işi özellikleri](/rest/api/storageimportexport/jobs#Jobs_Update) işlemi.  
  
## <a name="log-file-location"></a>Günlük dosyası konumu  
Günlükleri kapsayıcı veya sanal dizini tarafından belirtilen blok yazılan `ImportExportStatesPath` üzerinde ayarlayabilirsiniz ayarı bir `Put Job` işlemi. Günlüklere yazılır konumu nasıl kimlik doğrulaması için belirtilen değer birlikte iş için belirtilen üzerinde bağlıdır `ImportExportStatesPath`. Kimlik doğrulama iş için bir depolama hesabı anahtarı ya da bir kapsayıcı SAS (paylaşılan erişim imzası) belirtilebilir.  
  
Kapsayıcı veya sanal dizin adını ya da varsayılan adı olabilir. `waimportexport`, veya başka bir kapsayıcı veya sanal dizin adı.  
  
Aşağıdaki tabloda olası seçenekleri gösterir:  
  
|Kimlik Doğrulama Yöntemi|Değeri `ImportExportStatesPath`öğesi|Günlük BLOB'lar konumu|  
|---------------------------|----------------------------------------------|---------------------------|  
|Depolama hesabı anahtarı|Varsayılan değer|Adlı bir kapsayıcı `waimportexport`, varsayılan kapsayıcı olduğu. Örneğin:<br /><br /> `https://myaccount.blob.core.windows.net/waimportexport`|  
|Depolama hesabı anahtarı|Kullanıcı tarafından belirtilen değeri|Kullanıcı tarafından adlı bir kapsayıcı. Örneğin:<br /><br /> `https://myaccount.blob.core.windows.net/mylogcontainer`|  
|Kapsayıcı SAS|Varsayılan değer|Adlı bir sanal dizini `waimportexport`, SAS belirtilen kapsayıcısı altında varsayılan adı.<br /><br /> Örneğin, SAS belirtilen iş ise `https://myaccount.blob.core.windows.net/mylogcontainer?sv=2012-02-12&se=2015-05-22T06%3A54%3A55Z&sr=c&sp=wl&sig=sigvalue`, sonra da günlük konumunu olacaktır`https://myaccount.blob.core.windows.net/mylogcontainer/waimportexport`|  
|Kapsayıcı SAS|Kullanıcı tarafından belirtilen değeri|SAS belirtilen kapsayıcısı altında kullanıcı tarafından adlı bir sanal dizini.<br /><br /> Örneğin, SAS belirtilen iş ise `https://myaccount.blob.core.windows.net/mylogcontainer?sv=2012-02-12&se=2015-05-22T06%3A54%3A55Z&sr=c&sp=wl&sig=sigvalue`, ve belirtilen sanal dizin adlı `mylogblobs`, sonra da günlük konumunu olacaktır `https://myaccount.blob.core.windows.net/mylogcontainer/waimportexport/mylogblobs`.|  
  
Çağırarak URL'sini hata ve ayrıntılı günlükleri alabilirsiniz [alma işi](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) işlemi. Günlükleri, sürücünün işlem tamamlandıktan sonra kullanılabilir.  
  
## <a name="log-file-format"></a>Günlük dosyası biçimi  
Her iki günlük için biçim aynıdır: sabit sürücü ve müşterinin hesabına arasında BLOB'ları kopyalanırken oluşan olaylarla XML açıklamaları içeren blob.  
  
Hata günlüğü yalnızca BLOB veya içe veya dışa aktarma işi sırasında hatalarla karşılaştı dosyaları bilgileri içerirken ayrıntılı günlüğü her blob (için içeri aktarma işi için) veya (bir dışa aktarma işi için), dosya kopyalama işlemi durumu hakkında tam bilgi içerir.  
  
Ayrıntılı günlük biçimi aşağıda gösterilmiştir. Hata günlüğü aynı yapıya sahip, ancak başarılı işlemleri filtreler.  

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

Aşağıdaki tabloda günlük dosyasının öğelerini açıklar.  
  
|XML öğesi|Tür|Açıklama|  
|-----------------|----------|-----------------|  
|`DriveLog`|XML öğesi|Bir sürücü günlük temsil eder.|  
|`Version`|Öznitelik, dize|Günlük biçimi sürümü.|  
|`DriveId`|Dize|Sürücünün donanım seri numarası.|  
|`Status`|Dize|Sürücü işlem durumu. Bkz: `Drive Status Codes` daha fazla bilgi için tablo aşağıda.|  
|`Blob`|İç içe geçmiş XML öğesi|Bir blob temsil eder.|  
|`Blob/BlobPath`|Dize|Blob URI'si.|  
|`Blob/FilePath`|Dize|Sürücüsündeki dosyanın göreli yolu.|  
|`Blob/Snapshot`|Tarih saat|Yalnızca bir dışa aktarma işi için blob anlık görüntü sürümü.|  
|`Blob/Length`|Tamsayı|Blob bayt cinsinden uzunluğu.|  
|`Blob/LastModified`|Tarih saat|Blob son değiştirildiği, tarih yalnızca bir dışa aktarma işi için.|  
|`Blob/ImportDisposition`|Dize|Yalnızca bir içeri aktarma işi için blob alma değerlendirme.|  
|`Blob/ImportDisposition/@Status`|Öznitelik, dize|İçeri aktarma değerlendirme durumu.|  
|`PageRangeList`|İç içe geçmiş XML öğesi|Bir sayfa blob'u için sayfa aralıklarını listesini temsil eder.|  
|`PageRange`|XML öğesi|Sayfa aralığını temsil eder.|  
|`PageRange/@Offset`|Öznitelik, tamsayı|Blob sayfa aralığında başlangıç uzaklığı.|  
|`PageRange/@Length`|Öznitelik, tamsayı|Sayfa aralığının bayt cinsinden uzunluğu.|  
|`PageRange/@Hash`|Öznitelik, dize|Base16 kodlu MD5 karması sayfa aralığı.|  
|`PageRange/@Status`|Öznitelik, dize|Sayfa aralığı işlem durumu.|  
|`BlockList`|İç içe geçmiş XML öğesi|Bir blok blobu için blok listesini temsil eder.|  
|`Block`|XML öğesi|Bloğunu temsil eder.|  
|`Block/@Offset`|Öznitelik, tamsayı|Blob bloğunda başlangıç uzaklığı.|  
|`Block/@Length`|Öznitelik, tamsayı|Bloğun bayt cinsinden uzunluğu.|  
|`Block/@Id`|Öznitelik, dize|Blok kimliği.|  
|`Block/@Hash`|Öznitelik, dize|Base16 kodlu MD5 karma değeri bloğunun.|  
|`Block/@Status`|Öznitelik, dize|Blok işlem durumu.|  
|`Metadata`|İç içe geçmiş XML öğesi|Blob'un meta verileri temsil eder.|  
|`Metadata/@Status`|Öznitelik, dize|Blob verilerinin işlenmesini durumu.|  
|`Metadata/GlobalPath`|Dize|Genel meta veri dosyası göreli yolu.|  
|`Metadata/GlobalPath/@Hash`|Öznitelik, dize|Base16 kodlu MD5 karma değeri genel meta veri dosyasının.|  
|`Metadata/Path`|Dize|Meta veri dosyası göreli yolu.|  
|`Metadata/Path/@Hash`|Öznitelik, dize|Base16 kodlu MD5 karması meta veri dosyası.|  
|`Properties`|İç içe geçmiş XML öğesi|Blob özellikleri temsil eder.|  
|`Properties/@Status`|Öznitelik, dize|Blob özellikleri, örneğin dosya bulunamadı, işleme durumunu tamamlandı.|  
|`Properties/GlobalPath`|Dize|Genel Özellikler dosyanın göreli yolu.|  
|`Properties/GlobalPath/@Hash`|Öznitelik, dize|Base16 kodlu MD5 karma değeri genel özellikleri dosyasının.|  
|`Properties/Path`|Dize|Özellikler dosyanın göreli yolu.|  
|`Properties/Path/@Hash`|Öznitelik, dize|Base16 kodlu MD5 karma özellikleri dosyasının.|  
|`Blob/Status`|Dize|Blob işlem durumu.|  
  
# <a name="drive-status-codes"></a>Sürücü durum kodları  
Aşağıdaki tabloda bir sürücü işlemek için durum kodları listelenmektedir.  
  
|Durum kodu|Açıklama|  
|-----------------|-----------------|  
|`Completed`|Sürücü hatasız işleme tamamlandı.|  
|`CompletedWithWarnings`|Sürücü işleme BLOB'lar için belirtilen içeri aktarma değerlendirmeleri başına bir veya daha fazla BLOB uyarılarla tamamlandı.|  
|`CompletedWithErrors`|Sürücü, bir veya daha fazla BLOB veya öbekleri hatalarla tamamlandı.|  
|`DiskNotFound`|Disk sürücüsünde bulunur.|  
|`VolumeNotNtfs`|Disk üzerindeki ilk veri birimi NTFS biçiminde değil.|  
|`DiskOperationFailed`|Sürücü üzerindeki işlemleri gerçekleştirirken bilinmeyen bir hata oluştu.|  
|`BitLockerVolumeNotFound`|BitLocker encryptable birim bulunamadı.|  
|`BitLockerNotActivated`|Birimde BitLocker etkin değil.|  
|`BitLockerProtectorNotFound`|Sayısal parola anahtar koruyucusu birim üzerinde yok.|  
|`BitLockerKeyInvalid`|Sağlanan sayısal parola, birimin kilidi açılamıyor.|  
|`BitLockerUnlockVolumeFailed`|Birimin kilidini açmak çalışılırken bilinmeyen bir hata oluştu.|  
|`BitLockerFailed`|BitLocker işlem gerçekleştirilirken bilinmeyen bir hata oluştu.|  
|`ManifestNameInvalid`|Yayılma dosyası adı geçersiz.|  
|`ManifestNameTooLong`|Yayılma dosyası adı çok uzun.|  
|`ManifestNotFound`|Bildirim dosyası bulunamadı.|  
|`ManifestAccessDenied`|Bildirim dosyası için erişim reddedildi.|  
|`ManifestCorrupted`|Bildirim dosyası bozuk (içerik karması eşleşmiyor).|  
|`ManifestFormatInvalid`|Bildirim içerik gerekli biçime uymuyor.|  
|`ManifestDriveIdMismatch`|Sürücü Kimliği bildirim dosyasındaki sürücüden bir okuma eşleşmiyor.|  
|`ReadManifestFailed`|Bildirimden okunurken bir disk g/ç hatası oluştu.|  
|`BlobListFormatInvalid`|Dışarı aktarma blob listesi blob gerekli biçime uymuyor.|  
|`BlobRequestForbidden`|Depolama hesabındaki BLOB'ları erişmesine izin verilmiyor. Bu geçersiz depolama hesabı anahtarı veya kapsayıcı SAS nedeniyle olabilir.|  
|`InternalError`|Ve sürücü işlenirken iç hata oluştu.|  
  
## <a name="blob-status-codes"></a>BLOB durum kodları  
Aşağıdaki tabloda bir blob işlemek için durum kodları listelenmektedir.  
  
|Durum kodu|Açıklama|  
|-----------------|-----------------|  
|`Completed`|Blob hatasız işleme tamamlandı.|  
|`CompletedWithErrors`|Blob işleme bir veya daha fazla sayfa aralıklarını veya blokları, meta verileri veya özellikler hatalarla tamamlandı.|  
|`FileNameInvalid`|Dosya adı geçersiz.|  
|`FileNameTooLong`|Dosya adı çok uzun.|  
|`FileNotFound`|Dosyası bulunamadı.|  
|`FileAccessDenied`|Dosyaya erişim reddedildi.|  
|`BlobRequestFailed`|Blob erişmek için Blob hizmeti isteği başarısız oldu.|  
|`BlobRequestForbidden`|Blob erişmek için Blob hizmeti isteği izin verilmiyor. Bu geçersiz depolama hesabı anahtarı veya kapsayıcı SAS nedeniyle olabilir.|  
|`RenameFailed`|Blob (için içeri aktarma işi için) veya dosyası (bir dışarı aktarma işinin) yeniden adlandırmak başarısız oldu.|  
|`BlobUnexpectedChange`|Beklenmedik bir değişikliği (için bir dışarı aktarma işinin) blob ile oluştu.|  
|`LeasePresent`|Blob üzerindeki mevcut bir kira var.|  
|`IOFailed`|Blob işlenirken bir disk veya ağ g/ç hatası oluştu.|  
|`Failed`|Blob işlenirken bilinmeyen bir hata oluştu.|  
  
## <a name="import-disposition-status-codes"></a>İçeri aktarma değerlendirme durum kodları  
Aşağıdaki tabloda bir alma değerlendirme çözmek için durum kodları listelenmektedir.  
  
|Durum kodu|Açıklama|  
|-----------------|-----------------|  
|`Created`|Blob oluşturuldu.|  
|`Renamed`|Blob adlandırma alma değerlendirme adlandırıldı. `Blob/BlobPath` Öğe yeniden adlandırılmış blob URI'sini içeriyor.|  
|`Skipped`|Blob başına atlandı `no-overwrite` değerlendirme içeri aktarın.|  
|`Overwritten`|Blob başına bir blob geçersiz kıldı `overwrite` değerlendirme içeri aktarın.|  
|`Cancelled`|Önceki bir hata alma eğilimini işlenmesi durduruldu.|  
  
## <a name="page-rangeblock-status-codes"></a>Sayfa aralık/blok durum kodları  
Aşağıdaki tabloda bir sayfa aralığı veya bir blok işlemek için durum kodları listelenmektedir.  
  
|Durum kodu|Açıklama|  
|-----------------|-----------------|  
|`Completed`|Sayfa aralık veya blok hatasız işleme tamamlandı.|  
|`Committed`|Blok kaydedildi, ancak değil tam bloğunda listesi diğer blokları başarısız oldu veya tam engelleme listesi kendisini put başarısız oldu.|  
|`Uncommitted`|Blok karşıya ancak edilmemiş.|  
|`Corrupted`|Sayfa aralık veya blok bozuk (içerik karması eşleşmiyor).|  
|`FileUnexpectedEnd`|Bir beklenmeyen dosya sonu karşılaşıldı.|  
|`BlobUnexpectedEnd`|Blob beklenmeyen dosya sonuyla karşılaşıldı.|  
|`BlobRequestFailed`|Sayfa aralık veya blok erişmek için Blob hizmeti isteği başarısız oldu.|  
|`IOFailed`|Bir disk veya ağ g/ç hatası sayfası aralık veya blok işlenirken hata oluştu.|  
|`Failed`|Sayfa aralık veya blok işlenirken bilinmeyen bir hata oluştu.|  
|`Cancelled`|Önceki bir hata sayfası aralık veya blok işlenmesi durduruldu.|  
  
## <a name="metadata-status-codes"></a>Meta veriler durum kodları  
Aşağıdaki tabloda blob meta verileri işlemek için durum kodları listelenmektedir.  
  
|Durum kodu|Açıklama|  
|-----------------|-----------------|  
|`Completed`|Meta veri işleme hatasız bitirdi.|  
|`FileNameInvalid`|Meta veri dosyası adı geçersiz.|  
|`FileNameTooLong`|Meta veri dosya adı çok uzun.|  
|`FileNotFound`|Meta veri dosyası bulunamadı.|  
|`FileAccessDenied`|Meta veri dosyası için erişim reddedildi.|  
|`Corrupted`|Meta veri dosyası bozuk (içerik karması eşleşmiyor).|  
|`XmlReadFailed`|Meta veri içeriği gerekli biçime uymuyor.|  
|`XmlWriteFailed`|Meta veri yazma XML başarısız oldu.|  
|`BlobRequestFailed`|Meta verilerine erişmek için Blob hizmeti isteği başarısız oldu.|  
|`IOFailed`|Meta veriler işlenirken bir disk veya ağ g/ç hatası oluştu.|  
|`Failed`|Meta verileri işlenirken bilinmeyen bir hata oluştu.|  
|`Cancelled`|Önceki bir hata meta işlenmesi durduruldu.|  
  
## <a name="properties-status-codes"></a>Özellikler durum kodları  
Aşağıdaki tabloda blob özellikleri işlemek için durum kodları listelenmektedir.  
  
|Durum kodu|Açıklama|  
|-----------------|-----------------|  
|`Completed`|Özellikleri işleme hatasız bitti.|  
|`FileNameInvalid`|Özellikler dosya adı geçersiz.|  
|`FileNameTooLong`|Özellikler dosya adı çok uzun.|  
|`FileNotFound`|Özellikler dosyası bulunamadı.|  
|`FileAccessDenied`|Özellikler dosyaya erişim reddedildi.|  
|`Corrupted`|Özellikler dosya bozulmuş (içerik karması eşleşmiyor).|  
|`XmlReadFailed`|Özellikler içerik gerekli biçime uymuyor.|  
|`XmlWriteFailed`|Özellikleri yazma XML başarısız oldu.|  
|`BlobRequestFailed`|Özelliklerine erişmek için Blob hizmeti isteği başarısız oldu.|  
|`IOFailed`|Bir disk veya ağ g/ç hatası özellikleri işlenirken hata oluştu.|  
|`Failed`|Özellikler işlenirken bilinmeyen bir hata oluştu.|  
|`Cancelled`|Önceki bir hata özelliklerini işlenmesi durduruldu.|  
  
## <a name="sample-logs"></a>Örnek günlükleri  
Ayrıntılı günlük bir örnek verilmiştir.  
  
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
  
Karşılık gelen hata günlüğü aşağıda gösterilmiştir.  
  
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

 İçe aktarma işi için izleme hata günlüğünü bir dosya içeri aktarma sürücüsünde bulunamadı hakkında bir hata içeriyor. Sonraki bileşenlerinin durumunu olduğuna dikkat edin `Cancelled`.  
  
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

Aşağıdaki hata günlüğü dışarı aktarma işi için blob içeriğinin diske yazıldı, ancak blob'un özellikleri dışarı aktarılırken bir hata oluştu gösterir.  
  
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
