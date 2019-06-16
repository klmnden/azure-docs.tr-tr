---
title: İzleme ve Azure Data Box, Azure veri kutusu ağır olayları günlüğe | Microsoft Docs
description: İzleme ve Azure Data Box'ı ve Azure veri kutusu ağır siparişinizi çeşitli aşamalarında olayları günlüğe açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: article
ms.date: 06/03/2019
ms.author: alkohli
ms.openlocfilehash: 108d17d3e0ca5f32648f9d4f6cf4b5f9a2984d0c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66495817"
---
# <a name="tracking-and-event-logging-for-your-azure-data-box-and-azure-data-box-heavy"></a>İzleme ve Azure Data Box ve Azure veri kutusu ağır için olay günlüğü

Aşağıdaki adımları izleyerek bir Data Box veya yoğun veri kutusu siparişi gider:, ayarlama, verileri kopyalama, döndürmek, Azure'a karşıya yükleme ve doğrulama, sipariş ve verileri silme işlemi. Adımları sırayla karşılık gelen, siparişin erişimi denetleme olaylarını, sırayla izlemek için birden çok eyleme ve oluşturulan çeşitli günlüklerinizi yorumlama.

Aşağıdaki tabloda Data Box veya yoğun veri kutusu siparişi adımları ve izlemek ve her adımı sırasında sırasını denetlemek kullanılabilen bir özetini gösterir.

| Veri kutusu siparişi aşaması       | İzleme ve denetim için aracı                                                                        |
|----------------------------|------------------------------------------------------------------------------------------------|
| Sipariş oluşturma               | [RBAC aracılığıyla sipariş erişim denetimini ayarlama](#set-up-access-control-on-the-order)                                                    |
| Sipariş işleme            | [Sipariş İzleme](#track-the-order) aracılığıyla <ul><li> Azure portal </li><li> Sevkiyat taşıyıcısı Web sitesi </li><li>E-posta bildirimleri</ul> |
| Cihaz ayarlama              | Cihaz kimlik bilgileri, oturum açmış erişim [etkinlik günlükleri](#query-activity-logs-during-setup)                                              |
| Cihaz verileri Kopyala        | [Görünüm *error.xml* dosyaları](#view-error-log-during-data-copy) veri kopyalama                                                             |
| Göndermeye hazırlama            | [BOM dosyalarını İnceleme](#inspect-bom-during-prepare-to-ship) veya cihaz üzerinde bildirim dosyaları                                      |
| Azure için verileri karşıya yükleme       | [Gözden geçirme *copylogs* ](#review-copy-log-during-upload-to-azure) veri sırasında hatalar için Azure veri merkezinde karşıya yükleyin                         |
| CİHAZDAN verileri silinme   | [Günlükleri gözetim zinciri görüntülemek](#get-chain-of-custody-logs-after-data-erasure) denetim günlükleri ve siparişi geçmişi gibi                                                   |

Bu makalede, çeşitli mekanizmalar veya izlemek ve Data Box veya yoğun veri kutusu siparişi denetlemek kullanılabilen araçlar ayrıntılı olarak açıklanır. Bu makaledeki bilgiler hem Data Box hem de veri kutusu ağır için geçerlidir. Sonraki bölümlerde Data Box yönelik tüm başvuruları de veri kutusu ağır için geçerlidir.

## <a name="set-up-access-control-on-the-order"></a>Siparişi üzerindeki erişim denetimi ayarlama

Siparişinizi sırasını ilk oluşturulduğunda kimlerin erişebileceğini kontrol edebilirsiniz. Rol tabanlı erişim denetimi (RBAC) rolleri, Data Box Siparişiniz erişimi denetlemek için çeşitli kapsamların ayarlama. Bir RBAC rolü – salt okunur, salt okunur, okuma-yazma işlemleri bir kısmına erişim türünü belirler.

Azure Data Box hizmeti için tanımlı iki rol vardır:

- **Veri kutusu okuyucu** -salt okunur bir oluşturulmamasını kapsamı tarafından tanımlandığı şekilde erişebilir. Yalnızca bir sipariş ayrıntılarını görüntüleyebilirler. Depolama hesapları için ilgili diğer ayrıntıları erişim olamaz veya vb. adresi gibi sipariş ayrıntılarını düzenleyin.
- **Veri kutusu katkıda bulunan** -yalnızca belirli bir depolama hesabına veri aktarmak için bir sipariş oluşturabilirsiniz *zaten bir depolama hesabına yazma erişimi varsa*. Bunlar bir depolama hesabına erişim yoksa bunlar bile veri hesabına kopyalamak için bir veri kutusu siparişi oluşturulamıyor. Bu rol, herhangi bir depolama hesabı tanımlamıyor depolama hesaplarına ilgili erişim izinleri ne de izin verir.  

Sipariş için erişimi kısıtlamak için şunları yapabilirsiniz:

- Bir sipariş düzeyinde bir rol atayın. Kullanıcının yalnızca bu belirli veri kutusu siparişi yalnızca ve başka bir şey ile etkileşim kurmak için rolleri tarafından tanımlandığı şekilde bu izinleri vardır.
- Kaynak grubu düzeyinde bir role atama, kullanıcının bir kaynak grubu içindeki tüm Data Box sipariş erişebilir.

Önerilen RBAC kullanma hakkında daha fazla bilgi için bkz. [en iyi uygulamalar için RBAC](../role-based-access-control/overview.md#best-practice-for-using-rbac).

## <a name="track-the-order"></a>Siparişi izleme

Sipariş Sevkiyat taşıyıcısı Web sitesi ve Azure Portalı aracılığıyla izleyebilirsiniz. Data Box Siparişiniz herhangi bir zamanda izlemek üzere aşağıdaki mekanizmalardan şunlardır:

- Cihaz Azure veri merkezinde veya şirket içinde olduğunda sırasını izlemek için Git, **Data Box Siparişiniz > Genel Bakış** Azure portalında.

    ![Sipariş Durumu görüntüle ve Hayır'ı izleme](media/data-box-logs/overview-view-status-1.png)

- Cihaz esnasında sırasını izlemek için örneğin, UPS Web sitesinde ABD bölgesel taşıyıcı Web sitesine gidin. Numaralı Siparişiniz ile ilişkili izleme numarası sağlayın.
- Sipariş durumu değişiklikleri siparişi oluşturulduğunda sağlanan e-postaları üzerinde bağlı herhangi bir zamanda veri kutusu ayrıca e-posta bildirimleri gönderir. Tüm Data Box sipariş durumların bir listesi için bkz. [sırası durumunu görüntüleme](data-box-portal-admin.md#view-order-status). Siparişle ilişkili bildirim ayarlarını değiştirmek için bkz: [bildirim ayrıntılarını Düzenle](data-box-portal-admin.md#edit-notification-details).

## <a name="query-activity-logs-during-setup"></a>Kurulum sırasında sorgu etkinlik günlükleri

- Şirket içinde kilitli bir durumda Data Box'ınızı ulaşır. Numaralı Siparişiniz için Azure portalında kullanılabilir cihaz kimlik bilgilerini kullanabilirsiniz.  

    Data Box ayarlarken, cihaz kimlik bilgilerini kullanan tüm erişilen bilmeniz gerekebilir. Kimin eriştiği anlamaya **cihaz kimlik bilgilerini** dikey penceresinde, etkinlik günlükleri sorgulayabilir.  Erişim içeren herhangi bir eylem **cihaz ayrıntıları > kimlik bilgileri** dikey etkinlik günlüklerini içine kaydedilir `ListCredentials` eylem.

    ![Etkinlik günlüklerini sorgulama](media/data-box-logs/query-activity-log-1.png)

- Data Box her oturum, gerçek zamanlı olarak günlüğe kaydedilen ' dir. Ancak, bu bilgiler yalnızca kullanılabilir [denetim günlükleri](#audit-logs) sırasını başarıyla tamamlandıktan sonra.

## <a name="view-error-log-during-data-copy"></a>Veri kopyalama sırasında hata günlüğü görüntüle

Kopyalanan verileri içeren herhangi bir sorun varsa Data Box veya veri kutusu yoğun veri kopyalama sırasında bir hata dosyası oluşturulur.

### <a name="errorxml-file"></a>Error.xml dosyası

Kopyası işleri hatasız tamamlandığını olduğundan emin olun. Kopyalama işlemi sırasında bir hata varsa, günlükleri indirmek **Bağlan ve Kopyala** sayfası.

- Data Box'ınızı bir yönetilen disk klasöre hizalı değil 512 bayt olan dosya kopyaladıysanız, dosya hazırlama depolama hesabınıza sayfa blobu olarak karşıya değil. Günlüklerindeki bir hata görürsünüz. Dosyayı kaldırın ve 512 bayt hizalı bir dosyaya kopyalayın.
- Bir VHDX veya dinamik bir VHD ya da bir fark kayıt VHD (Bu dosyaları desteklenmez) kopyaladıysanız, günlüklerindeki bir hata görürsünüz.

İşte bir örnek *error.xml* yönetilen diskleri kopyalama farklı hataları için.

```xml
<file error="ERROR_BLOB_OR_FILE_TYPE_UNSUPPORTED">\StandardHDD\testvhds\differencing-vhd-022019.vhd</file>
<file error="ERROR_BLOB_OR_FILE_TYPE_UNSUPPORTED">\StandardHDD\testvhds\dynamic-vhd-022019.vhd</file>
<file error="ERROR_BLOB_OR_FILE_TYPE_UNSUPPORTED">\StandardHDD\testvhds\insidefixedvhdx-022019.vhdx</file>
<file error="ERROR_BLOB_OR_FILE_TYPE_UNSUPPORTED">\StandardHDD\testvhds\insidediffvhd-022019.vhd</file>
```

İşte bir örnek *error.xml* sayfa blobları için kopyalarken farklı hataları.

```xml
<file error="ERROR_BLOB_OR_FILE_SIZE_ALIGNMENT">\PageBlob512NotAligned\File100Bytes</file>
<file error="ERROR_BLOB_OR_FILE_SIZE_ALIGNMENT">\PageBlob512NotAligned\File786Bytes</file>
<file error="ERROR_BLOB_OR_FILE_SIZE_ALIGNMENT">\PageBlob512NotAligned\File513Bytes</file>
<file error="ERROR_BLOB_OR_FILE_SIZE_ALIGNMENT">\PageBlob512NotAligned\File10Bytes</file>
<file error="ERROR_BLOB_OR_FILE_SIZE_ALIGNMENT">\PageBlob512NotAligned\File500Bytes</file>
```


İşte bir örnek *error.xml* blok blobları için kopyalarken farklı hataları.

```xml
<file error="ERROR_CONTAINER_OR_SHARE_NAME_LENGTH">\ab</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_ALPHA_NUMERIC_DASH">\invalid dns name</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_LENGTH">\morethan63charactersfortestingmorethan63charactersfortestingmorethan63charactersfortestingmorethan63charactersfortestingmorethan63charactersfortestingmorethan63charactersfortesting</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_ALPHA_NUMERIC_DASH">\testdirectory-~!@#$%^&amp;()_+{}</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_ALPHA_NUMERIC_DASH">\test__doubleunderscore</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_IMPROPER_DASH">\-startingwith-hyphen</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_ALPHA_NUMERIC_DASH">\Starting with Capital</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_ALPHA_NUMERIC_DASH">\_startingwith_underscore</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_IMPROPER_DASH">\55555555--4444--3333--2222--111111111111</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_LENGTH">\1</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_ALPHA_NUMERIC_DASH">\11111111-_2222-_3333-_4444-_555555555555</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_IMPROPER_DASH">\test--doublehyphen</file>
<file error="ERROR_BLOB_OR_FILE_NAME_CHARACTER_ILLEGAL" name_encoding="Base64">XEludmFsaWRVbmljb2RlRmlsZXNcU3BjQ2hhci01NTI5Ni3vv70=</file>
<file error="ERROR_BLOB_OR_FILE_NAME_CHARACTER_ILLEGAL" name_encoding="Base64">XEludmFsaWRVbmljb2RlRmlsZXNcU3BjQ2hhci01NTMwMS3vv70=</file>
<file error="ERROR_BLOB_OR_FILE_NAME_CHARACTER_ILLEGAL" name_encoding="Base64">XEludmFsaWRVbmljb2RlRmlsZXNcU3BjQ2hhci01NTMwMy3vv70=</file>
<file error="ERROR_BLOB_OR_FILE_NAME_CHARACTER_CONTROL">\InvalidUnicodeFiles\Ã.txt</file>
<file error="ERROR_BLOB_OR_FILE_NAME_CHARACTER_ILLEGAL" name_encoding="Base64">XEludmFsaWRVbmljb2RlRmlsZXNcU3BjQ2hhci01NTMwNS3vv70=</file>
<file error="ERROR_BLOB_OR_FILE_NAME_CHARACTER_ILLEGAL" name_encoding="Base64">XEludmFsaWRVbmljb2RlRmlsZXNcU3BjQ2hhci01NTI5OS3vv70=</file>
<file error="ERROR_BLOB_OR_FILE_NAME_CHARACTER_ILLEGAL" name_encoding="Base64">XEludmFsaWRVbmljb2RlRmlsZXNcU3BjQ2hhci01NTMwMi3vv70=</file>
<file error="ERROR_BLOB_OR_FILE_NAME_CHARACTER_ILLEGAL" name_encoding="Base64">XEludmFsaWRVbmljb2RlRmlsZXNcU3BjQ2hhci01NTMwNC3vv70=</file>
<file error="ERROR_BLOB_OR_FILE_NAME_CHARACTER_ILLEGAL" name_encoding="Base64">XEludmFsaWRVbmljb2RlRmlsZXNcU3BjQ2hhci01NTI5OC3vv70=</file>
<file error="ERROR_BLOB_OR_FILE_NAME_CHARACTER_ILLEGAL" name_encoding="Base64">XEludmFsaWRVbmljb2RlRmlsZXNcU3BjQ2hhci01NTMwMC3vv70=</file>
<file error="ERROR_BLOB_OR_FILE_NAME_CHARACTER_ILLEGAL" name_encoding="Base64">XEludmFsaWRVbmljb2RlRmlsZXNcU3BjQ2hhci01NTI5Ny3vv70=</file>
```

İşte bir örnek *error.xml* Azure dosyaları kopyalarken farklı hataları.

```xml
<file error="ERROR_BLOB_OR_FILE_SIZE_LIMIT">\AzFileMorethan1TB\AzFile1.2TB</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_ALPHA_NUMERIC_DASH">\testdirectory-~!@#$%^&amp;()_+{}</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_IMPROPER_DASH">\55555555--4444--3333--2222--111111111111</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_IMPROPER_DASH">\-startingwith-hyphen</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_ALPHA_NUMERIC_DASH">\11111111-_2222-_3333-_4444-_555555555555</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_IMPROPER_DASH">\test--doublehyphen</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_LENGTH">\ab</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_ALPHA_NUMERIC_DASH">\invalid dns name</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_ALPHA_NUMERIC_DASH">\test__doubleunderscore</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_LENGTH">\morethan63charactersfortestingmorethan63charactersfortestingmorethan63charactersfortestingmorethan63charactersfortestingmorethan63charactersfortestingmorethan63charactersfortesting</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_ALPHA_NUMERIC_DASH">\_startingwith_underscore</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_LENGTH">\1</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_ALPHA_NUMERIC_DASH">\Starting with Capital</file>
```

Sonraki adıma devam etmeden önce her yukarıdaki durumları, hataları giderin. Data Box SMB veya NFS protokolü aracılığıyla veri kopyalama sırasında karşılaşılan hataları ile ilgili daha fazla bilgi için Git [Data Box sorun giderme ve veri kutusu ağır sorunları](data-box-troubleshoot.md). Data Box REST aracılığıyla veri kopyalama sırasında karşılaşılan hataları hakkında daha fazla bilgi için Git [sorun giderme veri kutusu Blob Depolama sorunları](data-box-troubleshoot-rest.md).

## <a name="inspect-bom-during-prepare-to-ship"></a>BOM İnceleme sırasında göndermeye hazırlama

Sırasında göndermeye, fatura, ürün reçetesi (BOM) veya bildirim dosyası oluşturulurken bilinen dosyaların listesini hazırlayın.

- Gerçek adlarını ve sayısı Kutusu'na veri kopyalanan dosyalar karşı doğrulamak için bu dosyayı kullanın.
- Dosyaların gerçek boyutları karşı doğrulamak için bu dosyayı kullanın.
- Doğrulayın *crc64* sıfır olmayan dizesine karşılık gelir. <!--A null value for crc64 indicates that there was a reparse point error)-->

Sırasında karşılaşılan hataları hakkında daha fazla bilgi için hazırlama gönderin, Git [Data Box sorun giderme ve veri kutusu ağır sorunları](data-box-troubleshoot.md).

### <a name="bom-or-manifest-file"></a>BOM ya da bildirim dosyası

Bildirim dosyası ve BOM Data Box cihaza kopyalanır tüm dosyaların listesini içerir. BOM dosya sağlama toplamı yanı sıra dosya adlarını ve karşılık gelen boyutları vardır. Blok blobları, sayfa blobları, Azure dosyaları, yönetilen disklere kopyalayın ve REST API'leri aracılığıyla kopyalama için Data Box için ayrı bir ürün reçetesi dosya oluşturulur. Göndermeye hazırlama sırasında yerel web kullanıcı Arabiriminden cihazın BOM dosyalarını indirebilirsiniz.

Bu dosyalar Data Box cihazda bulunan ve Azure veri merkezinde ilişkili depolama hesabına yüklenir.

### <a name="bom-file-format"></a>BOM dosya biçimi

BOM ya da bildirim dosyası, genel biçimi aşağıdaki gibidir:

`<file size = "file-size-in-bytes" crc64="cyclic-redundancy-check-string">\folder-path-on-data-box\name-of-file-copied.md</file>`

Verileri Data Box blok blob paylaşımında kopyalanmış olduğu zaman oluşturulan bir bildirimin örneği aşağıdadır.

```
<file size="10923" crc64="0x51c78833c90e4e3f">\databox\media\data-box-deploy-copy-data\connect-shares-file-explorer1.png</file>
<file size="15308" crc64="0x091a8b2c7a3bcf0a">\databox\media\data-box-deploy-copy-data\get-share-credentials2.png</file>
<file size="53486" crc64="0x053da912fb45675f">\databox\media\data-box-deploy-copy-data\nfs-client-access.png</file>
<file size="6093" crc64="0xadb61d0d7c6d4deb">\databox\data-box-cable-options.md</file>
<file size="6499" crc64="0x080add29add367d9">\databox\data-box-deploy-copy-data-via-nfs.md</file>
<file size="11089" crc64="0xc3ce6b13a4fe3001">\databox\data-box-deploy-copy-data-via-rest.md</file>
<file size="7749" crc64="0xd2e346a4588e307a">\databox\data-box-deploy-ordered.md</file>
<file size="14275" crc64="0x260296e5d1b1608a">\databox\data-box-deploy-copy-data.md</file>
<file size="4077" crc64="0x2bb0a170225bceec">\databox\data-box-deploy-picked-up.md</file>
<file size="15447" crc64="0xcec0ca8527720b3c">\databox\data-box-portal-admin.md</file>
<file size="9126" crc64="0x820856b5a54321ad">\databox\data-box-overview.md</file>
<file size="10963" crc64="0x5e9a14f9f4784fd8">\databox\data-box-safety.md</file>
<file size="5941" crc64="0x8631d62fbc038760">\databox\data-box-security.md</file>
<file size="12536" crc64="0x8c8ff93e73d665ec">\databox\data-box-system-requirements-rest.md</file>
<file size="3220" crc64="0x7257a263c434839a">\databox\data-box-system-requirements.md</file>
```

Ayrıca ürün reçetesi veya bildirim dosyaları Azure depolama hesabına kopyalanır. BOM kullanın veya bildirim dosyaları Data Box için kopyalanan verileri Azure'a karşıya yüklenen dosyaların eşleştiğini doğrulayın.

## <a name="review-copy-log-during-upload-to-azure"></a>Azure için karşıya yükleme sırasında kopya günlüğünü gözden geçirin

Azure, veri yükleme sırasında bir *copylog* oluşturulur.

### <a name="copylog"></a>Copylog

Data Box hizmeti işlenen her sipariş oluşturur *copylog* ilişkili depolama hesabında. *Copylog* yüklenen dosyaları toplam sayısı ve Azure depolama hesabınızı veri kutusundaki kopyalayıp veri sırasında hatalı çıkış dosya sayısı.

Karşıya yükleme sırasında Döngüsel artıklık denetleyin (CRC) hesaplama Azure'a gerçekleştirilir. CRC veri kopyalama ve sonra veriler karşıya karşılaştırılır. CRC uyuşmazlığı, karşılık gelen dosyalar karşıya yüklemek başarısız olduğunu gösterir.

Varsayılan olarak, copylog adlı bir kapsayıcı için günlükleri yazılır. Günlükleri aşağıdaki adlandırma kuralını ile depolanır:

`storage-account-name/databoxcopylog/ordername_device-serial-number_CopyLog_guid.xml`.

Copylog yolunu da görüntülenen **genel bakış** portal dikey penceresinde.

![Tamamlandığında genel bakış dikey penceresinde copylog yolu](media/data-box-logs/copy-log-path-1.png)

Aşağıdaki örnek, Data Box'ı yüklemek için başarıyla tamamlandı copylog dosyasının genel biçimini tanımlar:

```
<?xml version="1.0"?>
-<CopyLog Summary="Summary">
<Status>Succeeded</Status>
<TotalFiles>45</TotalFiles>
<FilesErrored>0</FilesErrored>
</CopyLog>
```

Azure'a karşıya yükleme hataları da tamamlanabilir.

![Hatalarla tamamlandı, genel bakış dikey penceresinde copylog yolu](media/data-box-logs/copy-log-path-2.png)

Burada karşıya yükleme hatalarla tamamlandı bir copylog örneği aşağıdadır:

```xml
<ErroredEntity Path="iso\samsungssd.iso">
  <Category>UploadErrorCloudHttp</Category>
  <ErrorCode>409</ErrorCode>
  <ErrorMessage>The blob type is invalid for this operation.</ErrorMessage>
  <Type>File</Type>
</ErroredEntity><ErroredEntity Path="iso\iSCSI_Software_Target_33.iso">
  <Category>UploadErrorCloudHttp</Category>
  <ErrorCode>409</ErrorCode>
  <ErrorMessage>The blob type is invalid for this operation.</ErrorMessage>
  <Type>File</Type>
</ErroredEntity><CopyLog Summary="Summary">
  <Status>Failed</Status>
  <TotalFiles_Blobs>72</TotalFiles_Blobs>
  <FilesErrored>2</FilesErrored>
</CopyLog>
```


## <a name="get-chain-of-custody-logs-after-data-erasure"></a>Günlükleri gözetim zinciri sonra veri silinme Al

Data Box disklerinin NIST SP 800-88 düzeltme 1 yönergelere göre gelen verileri silindikten sonra günlükleri gözetim zinciri kullanılabilir. Bu günlükler, denetim günlüklerini ve siparişi geçmişi içerir. BOM veya bildirim dosyalarını da Denetim günlükleriyle kopyalanır.

### <a name="audit-logs"></a>Denetim günlükleri

Denetim günlükleri çalıştırma hakkında bilgi içerir ve Azure veri merkezi dışında olduğunda Data Box veya veri kutusu ağır erişimini paylaşın. Bu günlükler şu adreste bulunabilir: `storage-account/azuredatabox-chainofcustodylogs`

Bir veri kutusu denetim günlüğünden bir örneği aşağıda verilmiştir:

```
9/10/2018 8:23:01 PM : The operating system started at system time ‎2018‎-‎09‎-‎10T20:23:01.497758400Z.
9/10/2018 8:23:42 PM : An account was successfully logged on.
Subject:
    Security ID:        S-1-5-18
    Account Name:       WIN-DATABOXADMIN
    Account Domain: Workgroup
    Logon ID:       0x3E7
Logon Information:
    Logon Type:     3
    Restricted Admin Mode:  -
    Virtual Account:        No
    Elevated Token:     No
Impersonation Level:        Impersonation
New Logon:
    Security ID:        S-1-5-7
    Account Name:       ANONYMOUS LOGON
    Account Domain: NT AUTHORITY
    Logon ID:       0x775D5
    Linked Logon ID:    0x0
    Network Account Name:   -
    Network Account Domain: -
    Logon GUID:     {00000000-0000-0000-0000-000000000000}
Process Information:
    Process ID:     0x4
    Process Name:       
Network Information:
    Workstation Name:   -
    Source Network Address: -
    Source Port:        -
Detailed Authentication Information:
    Logon Process:      NfsSvr
    Authentication Package:MICROSOFT_AUTHENTICATION_PACKAGE_V1_0
    Transited Services: -
    Package Name (NTLM only):   -
    Key Length:     0
This event is generated when a logon session is created. It is generated on the computer that was accessed. 
The subject fields indicate the account on the local system which requested the logon. This is most commonly a service such as the Server service, or a local process such as Winlogon.exe or Services.exe. 
The logon type field indicates the kind of logon that occurred. The most common types are 2 (interactive) and 3 (network).
The New Logon fields indicate the account for whom the new logon was created, i.e. the account that was logged on.
The network fields indicate where a remote logon request originated. Workstation name is not always available and may be left blank in some cases.
The impersonation level field indicates the extent to which a process in the logon session can impersonate.
The authentication information fields provide detailed information about this specific logon request.
    - Logon GUID is a unique identifier that can be used to correlate this event with a KDC event.
    - Transited services indicate which intermediate services have participated in this logon request.
    - Package name indicates which sub-protocol was used among the NTLM protocols.
    - Key length indicates the length of the generated session key. This will be 0 if no session key was requested.
9/10/2018 8:25:58 PM : An account was successfully logged on.
```


## <a name="download-order-history"></a>Sipariş geçmişi indirme

Siparişi geçmişi, Azure portalında kullanılabilir. Sırası tamamlandıktan ve cihaz temizleme (veri silinme disklerden) tamamlandıktan, sonra cihaz siparişinizi gidip için **sipariş ayrıntıları**. ** Siparişi geçmişi indirme** seçeneği kullanılabilir. Daha fazla bilgi için [indirme siparişi geçmişi](data-box-portal-admin.md#download-order-history).

Sipariş geçmişinde kaydırırsanız, görürsünüz:

- İzleme bilgileri için Cihazınızı taşıyıcı.
- Olaylar ile *SecureErase* etkinlik. Bu olaylar diskteki verilerin silinmesini karşılık gelir.
- Veri kutusu günlük bağlantıları. Yollarını *denetim günlükleri*, *copylogs*, ve *BOM* dosyaları sunulur.

Azure portalından sipariş geçmiş günlüğü örneği aşağıdadır:

```
-------------------------------
Microsoft Data Box Order Report
-------------------------------
Name                                               : gus-poland                              
StartTime(UTC)                              : 9/19/2018 8:49:23 AM +00:00                       
DeviceType                                     : DataBox                                           
-------------------
Data Box Activities
-------------------
Time(UTC)                 | Activity                       | Status          | Description

9/19/2018 8:49:26 AM      | OrderCreated                   | Completed       |
10/2/2018 7:32:53 AM      | DevicePrepared                 | Completed       |
10/3/2018 1:36:43 PM      | ShippingToCustomer             | InProgress      | Shipment picked up. Local Time : 10/3/2018 1:36:43 PM at AMSTERDAM-NLD                                                                                
10/4/2018 8:23:30 PM      | ShippingToCustomer             | InProgress      | Processed at AMSTERDAM-NLD. Local Time : 10/4/2018 8:23:30 PM at AMSTERDAM-NLD                                                                        
10/4/2018 11:43:34 PM     | ShippingToCustomer             | InProgress      | Departed Facility in AMSTERDAM-NLD. Local Time : 10/4/2018 11:43:34 PM at AMSTERDAM-NLD
10/5/2018 8:13:49 AM      | ShippingToCustomer             | InProgress      | Arrived at Delivery Facility in BRIGHTON-GBR. Local Time : 10/5/2018 8:13:49 AM at LAMBETH-GBR                                                         
10/5/2018 9:13:24 AM      | ShippingToCustomer             | InProgress      | With delivery courier. Local Time : 10/5/2018 9:13:24 AM at BRIGHTON-GBR                                                                               
10/5/2018 12:03:04 PM     | ShippingToCustomer             | Completed       | Delivered - Signed for by. Local Time : 10/5/2018 12:03:04 PM at BRIGHTON-GBR                                                                          
1/25/2019 3:19:25 PM      | ShippingToDataCenter           | InProgress      | Shipment picked up. Local Time : 1/25/2019 3:19:25 PM at BRIGHTON-GBR                                                                                       
1/25/2019 8:03:55 PM      | ShippingToDataCenter           | InProgress      | Processed at BRIGHTON-GBR. Local Time : 1/25/2019 8:03:55 PM at LAMBETH-GBR                                                                            
1/25/2019 8:04:58 PM      | ShippingToDataCenter           | InProgress      | Departed Facility in BRIGHTON-GBR. Local Time : 1/25/2019 8:04:58 PM at BRIGHTON-GBR                                                                    
1/25/2019 9:06:09 PM      | ShippingToDataCenter           | InProgress      | Arrived at Sort Facility LONDON-HEATHROW-GBR. Local Time : 1/25/2019 9:06:09 PM at LONDON-HEATHROW-GBR                                                
1/25/2019 9:48:54 PM      | ShippingToDataCenter           | InProgress      | Processed at LONDON-HEATHROW-GBR. Local Time : 1/25/2019 9:48:54 PM at LONDON-HEATHROW-GBR                                                            
1/25/2019 10:30:20 PM     | ShippingToDataCenter           | InProgress      | Departed Facility in LONDON-HEATHROW-GBR. Local Time : 1/25/2019 10:30:20 PM at LONDON-HEATHROW-GBR
1/28/2019 7:11:35 AM      | ShippingToDataCenter           | InProgress      | Arrived at Delivery Facility in AMSTERDAM-NLD. Local Time : 1/28/2019 7:11:35 AM at AMSTERDAM-NLD                                                     
1/28/2019 9:07:57 AM      | ShippingToDataCenter           | InProgress      | With delivery courier. Local Time : 1/28/2019 9:07:57 AM at AMSTERDAM-NLD                                                                             
1/28/2019 1:35:56 PM      | ShippingToDataCenter           | InProgress      | Scheduled for delivery. Local Time : 1/28/2019 1:35:56 PM at AMSTERDAM-NLD                                                                            
1/28/2019 2:57:48 PM      | ShippingToDataCenter           | Completed       | Delivered - Signed for by. Local Time : 1/28/2019 2:57:48 PM at AMSTERDAM-NLD
1/29/2019 2:18:43 PM      | PhysicalVerification           | Completed       |
1/29/2019 3:49:50 PM      | DeviceBoot                     | Completed       | Appliance booted up successfully.
1/29/2019 3:49:51 PM      | AnomalyDetection               | Completed       | No anomaly detected.
2/12/2019 10:37:03 PM     | DataCopy                       | Resumed         |
2/13/2019 12:05:15 AM     | DataCopy                       | Resumed         |
2/15/2019 7:07:34 PM      | DataCopy                       | Completed       | Copy Completed.
2/15/2019 7:47:32 PM      | SecureErase                    | Started         |
2/15/2019 8:01:10 PM      | SecureErase                    | Completed       | Azure Data Box:<Device-serial-no> has been sanitized according to NIST 800-88 Rev 1.
------------------
Data Box Log Links
------------------
Account Name         : gusacct
Copy Logs Path       : databoxcopylog/gus-poland_<Device-serial-no>_CopyLog_<GUID>.xml
Audit Logs Path      : azuredatabox-chainofcustodylogs\<GUID>\<Device-serial-no>
BOM Files Path       : azuredatabox-chainofcustodylogs\<GUID>\<Device-serial-no>
```

## <a name="next-steps"></a>Sonraki adımlar

- Bilgi edinmek için nasıl [Data Box ve veri kutusu ağır sorun giderme](data-box-troubleshoot.md).
