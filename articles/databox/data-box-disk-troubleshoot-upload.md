---
title: Azure Data Box Disk sorun giderme verilerini karşıya yükleme günlüklerini kullanarak sorunları | Microsoft Docs
description: Günlükleri ve Azure Data Box Disk için karşıya veri yükleme sırasında görülen sorunlarını nasıl kullanılacağını açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: article
ms.date: 06/14/2019
ms.author: alkohli
ms.openlocfilehash: 6f3ac38c3eac968bd2f7ec2aada435466d3ff279
ms.sourcegitcommit: 72f1d1210980d2f75e490f879521bc73d76a17e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/14/2019
ms.locfileid: "67148315"
---
# <a name="understand-logs-to-troubleshoot-data-upload-issues-in-azure-data-box-disk"></a>Azure Data Box Disk verileri karşıya yükleme sorunlarını gidermek için günlükleri anlama

Bu makale, Microsoft Azure Data Box Disk için geçerlidir ve verileri Azure'a karşıya yüklediğinizde gördüğünüz sorunları açıklar.

## <a name="data-upload-logs"></a>Günlükleri karşıya veri yükleme

Veri merkezinde Azure'a yüklendiğinde `_error.xml` ve `_verbose.xml` dosyalar oluşturulur. Bu günlükler, verileri karşıya yüklemek için kullanılan aynı depolama hesabına yüklenir. Bir örnek `_error.xml` aşağıda gösterilmiştir.
    
```xml
 <?xml version="1.0" encoding="utf-8"?>
    <DriveLog Version="2018-10-01">
      <DriveId>18041C582D7E</DriveId>
      <Summary>
     <!--Summary for validation and data copy to Azure -->
        <ValidationErrors>
          <None Count="8" />
        </ValidationErrors>
        <CopyErrors>
          <Completed Count="5" Description="No errors encountered" />
          <ContainerRenamed Count="3" Description="Renamed the container as the original container name does not follow Azure conventions." />
        </CopyErrors>
      </Summary>
    <!--List of renamed containers with the new names, new file path in Azure, original invalid file path, and size -->
      <Blob Status="ContainerRenamed">
        <BlobPath>databox-c2073fd1cc379d83e03d6b7acce23a6cf29d1eef/private.vhd</BlobPath>
        <OriginalFilePath>\PageBlob\pageblob test\private.vhd</OriginalFilePath>
        <SizeInBytes>10490880</SizeInBytes>
      </Blob>
      <Blob Status="ContainerRenamed">
        <BlobPath>databox-c2073fd1cc379d83e03d6b7acce23a6cf29d1eef/resource.vhd</BlobPath>
        <OriginalFilePath>\PageBlob\pageblob test\resource.vhd</OriginalFilePath>
        <SizeInBytes>71528448</SizeInBytes>
      </Blob>
      <Blob Status="ContainerRenamed">
        <BlobPath>databox-c2073fd1cc379d83e03d6b7acce23a6cf29d1eef/role.vhd</BlobPath>
        <OriginalFilePath>\PageBlob\pageblob test\role.vhd</OriginalFilePath>
        <SizeInBytes>10490880</SizeInBytes>
      </Blob>
      <Status>CompletedWithErrors</Status>
    </DriveLog>
```

## <a name="download-logs"></a>İndirme günlükleri

Bulma ve tanılama günlükleri indirmek için iki yolu vardır.

- Verileri Azure'a karşıya yüklerken herhangi bir hata varsa portalı tanılama günlüklerinin bulunduğu klasöre bir yol görüntüler.

    ![Portal'da günlüklerini bağlantı](./media/data-box-disk-troubleshoot-upload/data-box-disk-portal-logs.png)

- Data Box Siparişiniz ile ilişkilendirilmiş depolama hesabına gidin. **Blob hizmeti > Blob'lara göz atın** sayfasına gidin ve depolama hesabına karşılık gelen blobu bulun. Git **waies**.

    ![Günlükleri kopyalama 2](./media/data-box-disk-troubleshoot/data-box-disk-copy-logs2.png)

Her durumda, hata günlüklerini ve ayrıntılı günlüklere bakın. Her günlük seçin ve yerel bir kopyasını indirin.


## <a name="data-upload-errors"></a>Verileri karşıya yükleme hataları

Verileri Azure'a karşıya yüklenirken oluşturulan hatalar aşağıdaki tabloda özetlenmiştir.

| Hata kodu | Açıklama                        |
|-------------|------------------------------|
|`None` |  Başarıyla tamamlandı.           |
|`Renamed` | Başarılı bir şekilde blob'olarak yeniden adlandırıldı.  |                                                            |
|`CompletedWithErrors` | Karşıya yükleme hatalarla tamamlandı. Dosyaların hata ayrıntıları günlük dosyasında dahil edilir.  |
|`Corrupted`|Veri alımı sırasında hesaplanan CRC karşıya yükleme sırasında hesaplanan CRC'yle eşleşmiyor.  |  
|`StorageRequestFailed` | Azure depolama isteği başarısız oldu.   |     |
|`LeasePresent` | Bu öğe kiralanmış ve başka bir kullanıcı tarafından kullanılıyor. |
|`StorageRequestForbidden` |Kimlik doğrulama sorunları nedeniyle karşıya yüklenemedi. |
|`ManagedDiskCreationTerminalFailure` | Yönetilen diskler olarak karşıya yüklenemedi. Dosyaları hazırlama depolama hesabında sayfa blobları kullanılabilir. Bu gibi durumlarda, sayfa BLOB'ları el ile yönetilen disklere dönüştürebilirsiniz.  |
|`DiskConversionNotStartedTierInfoMissing` | Yönetilen disk, VHD dosyasının dışında precreated katmanı klasörleri kopyaladığınızdan oluşturulmadı. Dosya varsayılan olarak, sipariş oluşturma sırasında belirtilen Hazırlama depolama hesabına sayfa blobu olarak yüklenir. Bunu el ile bir yönetilen diskin dönüştürebilirsiniz.|
|`InvalidWorkitem` | Azure adlandırma için uygun değil ve kuralları sınırlar verileri karşıya yüklenemedi.|
|`InvalidPageBlobUploadAsBlockBlob` | Ön eki içeren bir kapsayıcıya blok blobları olarak karşıya `databoxdisk-invalid-pb-`.|
|`InvalidAzureFileUploadAsBlockBlob` | Ön eki içeren bir kapsayıcıya blok blobları olarak karşıya `databoxdisk-invalid-af`-.|
|`InvalidManagedDiskUploadAsBlockBlob` | Ön eki içeren bir kapsayıcıya blok blobları olarak karşıya `databoxdisk-invalid-md`-.|
|`InvalidManagedDiskUploadAsPageBlob` |Sayfa blobları bir kapsayıcıda ön eki olarak karşıya `databoxdisk-invalid-md-`. |
|`MovedToOverflowShare` |Yüklenen dosyalar için yeni bir paylaşım özgün paylaşım boyutu olarak maksimum Azure boyut sınırını aştı. Yeni dosya paylaşımı adı ile ve sonra özgün adına sahip `-2`.   |
|`MovedToDefaultAzureShare` |Varsayılan Paylaşım herhangi bir klasöre bir parçası olmayan yüklenen dosyalar. Paylaşım adı ile başlayan `databox-`. |
|`ContainerRenamed` |Bu dosyalar için kapsayıcı Azure adlandırma kurallarına uygun olmadı ve yeniden adlandırılır. Yeni adı ile başlayan `databox-` ve özgün adı SHA1 karması ile soneki |
|`ShareRenamed` |Paylaşım için bu dosyalar Azure adlandırma kurallarına uygun olmadı ve adlandırılır. Yeni adı ile başlayan `databox-` ve özgün adı SHA1 karması ile soneki. |
|`BlobRenamed` |Bu dosyalar Azure adlandırma kurallarına uygun olmadı ve değiştirilmiştir. Denetleme `BlobPath` yeni ad alanı. |
|`FileRenamed` |Bu dosyalar Azure adlandırma kurallarına uygun olmadı ve değiştirilmiştir. Denetleme `FileStoragePath` yeni ad alanı. |
|`DiskRenamed` |Bu dosyalar Azure adlandırma kurallarına uygun olmadı ve değiştirilmiştir. Denetleme `BlobPath` yeni ad alanı. |


## <a name="next-steps"></a>Sonraki adımlar

- [Data Box Disk sorunları için bir destek bileti](data-box-disk-contact-microsoft-support.md).
