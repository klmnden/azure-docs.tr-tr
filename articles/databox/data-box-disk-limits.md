---
title: Azure Data Box Disk sınırlar | Microsoft Docs
description: Sistem sınırlarını ve önerilen boyut için Microsoft Azure Data Box Disk açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: article
ms.date: 04/01/2019
ms.author: alkohli
ms.openlocfilehash: 32445e3f6859a6161eb2fae20233c598234f18a0
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60400635"
---
# <a name="azure-data-box-disk-limits"></a>Azure Data Box Disk limitleri


Limitler, dağıtmanıza ve Microsoft Azure Data Box Disk çözümünüz olarak düşünün.

## <a name="data-box-service-limits"></a>Veri kutusu hizmeti sınırları

 - Veri kutusu hizmeti kullanılabilir listelenen Azure bölgelerinde [bölge kullanılabilirliği](data-box-disk-overview.md#region-availability).
 - Tek bir depolama hesabında Data Box Disk ile desteklenir.

## <a name="data-box-disk-performance"></a>Veri kutusu Disk performansı

USB 3.0 bağlantısıyla yapılan testlerde disk performansının 430 MB/sn seviyesine çıkabildiği görülmüştür. Gerçek performans kullanılan dosya boyutuna göre değişiklik gösterecektir. Daha küçük dosyalarda performans daha düşük olabilir.

## <a name="azure-storage-limits"></a>Azure depolama sınırları

Bu bölümde, Azure dosyaları, Azure blok BLOB'ları ve Data Box hizmeti uygunsa Azure sayfa blobları için Azure depolama hizmeti için sınırlar ve gerekli adlandırma kurallarını açıklar. Depolama sınırları dikkatle gözden geçirin ve tüm önerilere uyun.

Azure depolama hizmet sınırları ve adlandırma paylaşımları, kapsayıcılar ve dosyalar için en iyi yöntemler üzerinde en son bilgiler için bkz:

- [Adlandırma ve kapsayıcıları başvurma](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata)
- [Paylaşımları adlandırma ve onlara başvurma](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-shares--directories--files--and-metadata)
- [Blok blobları ve sayfa blob kuralları](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs)

> [!IMPORTANT]
> Herhangi bir dosya ya da Azure depolama hizmeti limitlerin ya da Azure dosya/Blob adlandırma kurallarına uygun olmayan dizin varsa, ardından bu dosyaları veya dizinleri Data Box hizmeti aracılığıyla Azure Depolama'ya alınan değil.

## <a name="data-upload-caveats"></a>Uyarılar karşıya veri yükleme

- Veri diskleri doğrudan kopyalamayın. Önceden oluşturulmuş veri kopyalama *BlockBlob*,*PageBlob*, ve *AzureFile* klasörleri.
- Bir klasörü altında *BlockBlob* ve *PageBlob* bir kapsayıcıdır. Örneğin, kapsayıcıları olarak oluşturulur *BlockBlob/kapsayıcı* ve *PageBlob/kapsayıcı*.
- Data Box Disk kopyalanan nesne olarak aynı ada sahip bir bulutta mevcut bir Azure nesne (örneğin, bir blobu) varsa, dosyanın file(1) bulutta olarak adlandırır.
- Her dosyanın içine yazılmış *BlockBlob* ve *PageBlob* paylaşımları yüklendiği bir blok blobu ve sayfa blobu olarak sırasıyla.
- Herhangi bir boş (olmadan tüm dosyaları) dizin hiyerarşisi altında oluşturulan *BlockBlob* ve *PageBlob* klasörleri karşıya.
- Herhangi bir hata varsa verileri Azure'a karşıya yüklenirken bir hata günlüğü hedef depolama hesabında oluşturulur. Bu hata günlük yolunu, karşıya yükleme tamamlandıktan ve düzeltme eylemi için günlüğü gözden geçirebilirsiniz portalda kullanılabilir. Veri kaynağından karşıya yüklenen veriler doğrulamadan silmeyin.
- Yönetilen diskler talepte belirtilen aşağıdaki ek konuları gözden geçirin:

    - Belirli bir ada sahip bir yönetilen diski yalnızca bir kaynak grubunda ve tüm Data Box Disk precreated tüm klasörler arasında olabilir. Başka bir gelir precreated klasörlere karşıya VHD'ler benzersiz adlara sahip olmalıdır. Belirtilen ada, bir kaynak grubunda zaten mevcut bir yönetilen disk eşleşmiyor emin olun. VHD'leri aynı ada sahipse, bu ada sahip yönetilen disk için yalnızca bir VHD dönüştürülür. Diğer VHD'ler sayfa BLOB'ları hazırlama depolama hesabına yüklenir.
    - Her zaman VHD'lerin precreated klasörlerden birine kopyalayın. VHD'ler, bu klasörleri dışında veya oluşturduğunuz bir klasöre kopyalamak, VHD'ler sayfa blobları Azure depolama hesabına yüklenir ve yönetilen diskler değil.
    - Yönetilen disk oluşturmak için yalnızca sabit VHD'lerin karşıya yüklenebilir. Dinamik VHD, fark kayıt VHD veya VHDX dosyaları desteklenmez.

## <a name="azure-storage-account-size-limits"></a>Azure depolama hesabı boyut sınırları

Depolama hesabına kopyalanır veri boyutu sınırları şunlardır. Karşıya yüklediğiniz veriler için limitler uyduğundan emin olun. Bu sınırlar en güncel bilgiler için Git [Azure blob depolama ölçek hedefleri](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets#azure-blob-storage-scale-targets) ve [Azure dosyaları ölçeklendirme hedeflerini](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets#azure-files-scale-targets).

| Azure depolama hesabına kopyalanan verileri boyutu                      | Varsayılan limit          |
|---------------------------------------------------------------------|------------------------|
| Blok blobu ve sayfa blob                                            | Depolama hesabı başına 500 TB. <br> Bu Data Box Disk dahil olmak üzere tüm kaynaklardan gelen verileri içerir.|


## <a name="azure-object-size-limits"></a>Azure nesne boyutu sınırları

Azure nesnelerin yazılabilir boyutları aşağıda verilmiştir. Yüklenen tüm dosyalar bu sınırlara uyacak emin olun.

| Azure nesne türü | Varsayılan limit                                             |
|-------------------|-----------------------------------------------------------|
| Blok blobu        | ~ 4,75 TiB                                                 |
| Sayfa blobu         | 8 TiB <br> (Sayfa blobu biçiminde yüklenen her dosya, 512 bayt hizalı olmalıdır, aksi takdirde yükleme başarısız olur. <br> VHD ve VHDX 512 bayt hizalı var.) |
|Azure Dosyaları        | 1 TiB <br> En çok, Paylaşım 5 TiB boyutudur     |
| Yönetilen diskler     |4 TiB <br> Boyutu ve sınırları hakkında daha fazla bilgi için bkz: <li>[Yönetilen diskler için ölçeklenebilirlik hedefleri](../virtual-machines/windows/disk-scalability-targets.md#managed-virtual-machine-disks)</li>|


## <a name="azure-block-blob-page-blob-and-file-naming-conventions"></a>Azure blok blobu, sayfa blobu ve dosya adlandırma kuralları

| Varlık                                       | Kurallar                                                                                                                                                                                                                                                                                                               |
|----------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Kapsayıcı adları için blok blobu ve sayfa blobu <br> Azure dosyaları için dosya paylaşımı adları | 3-63 karakter uzunluğunda olan geçerli bir DNS adı olmalıdır. <br>  Bir harf veya sayı ile başlamalıdır. <br> Yalnızca küçük harf, sayı ve tire (-) içerebilir. <br> Kısa çizgiden (-) hemen önce ve sonra bir harf veya rakam gelmelidir. <br> Adlarda kısa çizgiler art arda kullanılamaz. |
| Azure dosyaları için dizin ve dosya adları     |<li> Durum koruma, büyük/küçük harfe ve 255 karakterden uzun olmamalıdır. </li><li> (/) İleri eğik çizgiyle bitemez. </li><li>Sağlanırsa, otomatik olarak kaldırılacak. </li><li> Aşağıdaki karakterlere izin verilmez: <code>" \\ / : \| < > * ?</code></li><li> Ayrılmış URL karakterleri doğru şekilde atlanmalıdır. </li><li> Geçersiz URL yolu karakterlere izin verilmez. Kod noktaları gibi \\uE000 Unicode karakterler geçerli değildir. Bazı ASCII veya Unicode karakterleri gibi denetim karakterleri (0x00 için 0x1F \\u0081, vb.), izin verilmeyen de. Unicode yöneten kurallar için HTTP/1.1 dizelerde RFC 2616 ', bölüm 2.2 bakın: Temel kurallar ve RFC 3987. </li><li> Şu dosya adlarına izin verilmez: LPT1, LPT2, LPT3, LPT4, LPT5, LPT6, LPT7, LPT8, LPT9, COM1, COM2, COM3, COM4, COM5, COM6, COM7, COM8, COM9, PRN, AUX, NUL, CON, CLOCK$, nokta karakteri (.) ve iki nokta karakteri (..).</li>|
| Blok blobu ve sayfa blobu için blob adları      | Blob adları büyük/küçük harfe duyarlıdır ve karakterler herhangi bir düzende sıralanabilir. <br> Blob adı 1 ila 1024 karakter uzunluğunda olmalıdır. <br> Ayrılmış URL karakterleri doğru şekilde atlanmalıdır. <br>Blob adını oluşturan yolun bölümleri 254 karakterden uzun olamaz. Yol bölümü, arka arkaya gelen sınırlayıcı karakterlerinin (örneğin eğik çizgi "/") arasında yer alan ve bir sanal dizinin adına karşılık gelen dizedir. |

## <a name="managed-disk-naming-conventions"></a>Yönetilen disk adlandırma kuralları

| Varlık | Kurallar                                             |
|-------------------|-----------------------------------------------------------|
| Yönetilen disk adları       | <li> Ad 1 ila 80 karakter uzunluğunda olmalıdır. </li><li> Ad, harf veya sayı ile başlamak, bir harf, sayı veya alt çizgi ile bitmelidir. </li><li> Ad yalnızca harf, sayı, alt çizgi, nokta veya kısa çizgi içerebilir. </li><li>   Ad boşluk olmamalıdır veya `/`.                                              |

## <a name="next-steps"></a>Sonraki adımlar

- Gözden geçirme [Data Box Disk sistem gereksinimleri](data-box-disk-system-requirements.md)
