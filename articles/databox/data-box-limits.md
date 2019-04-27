---
title: Azure Data Box sınırlar | Microsoft Docs
description: Sistem sınırlarını ve Microsoft Azure Data Box bileşenleri ve bağlantıları için önerilen boyutları açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: article
ms.date: 02/22/2019
ms.author: alkohli
ms.openlocfilehash: 147cf61dcd36edc75a936cf9b467fd89c8d8a965
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60326607"
---
# <a name="azure-data-box-limits"></a>Azure Data Box sınırları

Limitler, dağıtmanıza ve Microsoft Azure Data Box'ınızı gibi düşünün. Aşağıdaki tabloda Data Box için limitler açıklanmaktadır.


## <a name="data-box-service-limits"></a>Veri kutusu hizmeti sınırları

 - Data Box hizmeti ile birden çok depolama hesabı kullanıyorsanız, tüm depolama hesapları aynı Azure bölgesine ait olmalıdır.
 - En fazla üç depolama hesabı kullanmanızı öneririz. Daha fazla depolama hesaplarının kullanılması, potansiyel olarak performansı etkileyebilir.

## <a name="data-box-limits"></a>Veri kutusu sınırları

- Veri kutusu en fazla 500 milyon dosya depolayabilir.

## <a name="azure-storage-limits"></a>Azure depolama sınırları

Bu bölümde, Azure dosyaları, Azure blok BLOB'ları ve Data Box hizmeti uygunsa Azure sayfa blobları için Azure depolama hizmeti için sınırlar ve gerekli adlandırma kurallarını açıklar. Depolama sınırları dikkatle gözden geçirin ve tüm önerilere uyun.

Azure depolama hizmet sınırları ve adlandırma paylaşımları, kapsayıcılar ve dosyalar için en iyi yöntemler üzerinde en son bilgiler için bkz:

- [Adlandırma ve kapsayıcıları başvurma](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata)
- [Paylaşımları adlandırma ve onlara başvurma](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-shares--directories--files--and-metadata)
- [Blok blobları ve sayfa blob kuralları](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs)

> [!IMPORTANT]
> Herhangi bir dosya ya da Azure depolama hizmeti limitlerin ya da Azure dosya/Blob adlandırma kurallarına uygun olmayan dizin varsa, ardından bu dosyaları veya dizinleri Data Box hizmeti aracılığıyla Azure Depolama'ya alınan değildir.

## <a name="data-upload-caveats"></a>Uyarılar karşıya veri yükleme

- Dosyaları doğrudan precreated paylaşımları birine bulundurmanıza gerek yoktur. Paylaşım altında bir klasör oluşturun ve bu klasöre kopyalayın gerekir.
- Bir klasörü altında *StorageAccount_BlockBlob* ve *StorageAccount_PageBlob* bir kapsayıcıdır. Örneğin, kapsayıcıları olarak oluşturulur *StorageAccount_BlockBlob/kapsayıcı* ve *StorageAccount_PageBlob/kapsayıcı*.
- Doğrudan altında oluşturulan her klasörün *StorageAccount_AzureFiles* bir Azure dosya paylaşımına çevrilir.
- Data Box, kopyalanan nesne olarak aynı ada sahip bir bulutta (örneğin, bir bloba veya bir dosya) var olan bir Azure nesne varsa, bulutta dosyanın üzerine yazar.
- Her dosyanın içine yazılmış *StorageAccount_BlockBlob* ve *StorageAccount_PageBlob* paylaşımları yüklendiği bir blok blobu ve sayfa blobu olarak sırasıyla.
- Azure blob Depolama dizinleri desteklememektedir. Bir klasörü altında oluşturursanız *StorageAccount_BlockBlob* klasörünü ve ardından sanal klasörler blob adı oluşturulur. Azure dosyaları için gerçek dizin yapısı korunur.
- Herhangi bir boş (olmadan tüm dosyaları) dizin hiyerarşisi altında oluşturulan *StorageAccount_BlockBlob* ve *StorageAccount_PageBlob* değil klasörleri karşıya yüklendi.
- Herhangi bir hata varsa verileri Azure'a karşıya yüklenirken bir hata günlüğü hedef depolama hesabında oluşturulur. Bu hata günlük yolunu karşıya yükleme tamamlandıktan ve düzeltme eylemi için günlüğü gözden geçirebilirsiniz kullanılabilir. Veri kaynağından karşıya yüklenen veriler doğrulamadan silmeyin.

## <a name="azure-storage-account-size-limits"></a>Azure depolama hesabı boyut sınırları

Depolama hesabına kopyalanır veri boyutu sınırları şunlardır. Karşıya yüklediğiniz veriler için limitler uyduğundan emin olun. Bu sınırlar en güncel bilgiler için Git [Azure blob depolama ölçek hedefleri](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets#azure-blob-storage-scale-targets) ve [Azure dosyaları ölçeklendirme hedeflerini](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets#azure-files-scale-targets).

| Azure depolama hesabına kopyalanan verileri boyutu                      | Varsayılan Sınır          |
|---------------------------------------------------------------------|------------------------|
| Blok blobu ve sayfa blobu                                            | Depolama hesabı başına 500 TiB. <br> Bu Data Box dahil olmak üzere tüm kaynaklardan gelen verileri içerir.|
| Azure dosya                                                          | Paylaşım başına 5 TiB.<br> Altındaki tüm klasörleri *StorageAccount_AzureFiles* bu sınıra uymalıdır.       |

## <a name="azure-object-size-limits"></a>Azure nesne boyutu sınırları

Azure nesnelerin yazılabilir boyutları aşağıda verilmiştir. Yüklenen tüm dosyalar bu sınırlara uyacak emin olun.

| Azure nesne türü | Varsayılan Sınır                                             |
|-------------------|-----------------------------------------------------------|
| Blok blobu        | ~ 4,75 TiB                                                 |
| Sayfa blobu         | 8 TiB <br> Sayfa blob biçiminde yüklenen her dosya 512 bayt hizalı (tamsayı katı) olmalıdır, aksi takdirde yükleme başarısız olur. <br> VHD ve VHDX 512 bayt hizalı var. |
| Azure Dosyaları        | 1 TiB                                                      |
| Yönetilen diskler     | 4 TiB <br> Boyutu ve sınırları hakkında daha fazla bilgi için bkz: <li>[Standart SSD ölçeklenebilirlik hedefleri](../virtual-machines/windows/disks-types.md#standard-ssd)</li><li>[Premium SSD ölçeklenebilirlik hedefleri](../virtual-machines/windows/disks-types.md#standard-hdd)</li><li>[Standart hdd'lerin ölçeklenebilirlik hedefleri](../virtual-machines/windows/disks-types.md#premium-ssd)</li><li>[Fiyatlandırma ve faturalama yönetilen diskleri](../virtual-machines/windows/disks-types.md#billing)</li>                                                     |

## <a name="azure-block-blob-page-blob-and-file-naming-conventions"></a>Azure blok blobu, sayfa blobu ve dosya adlandırma kuralları

| Varlık                                       | Kurallar                                                                                                                                                                                                                                                                                                               |
|----------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Kapsayıcı adları için blok blobu ve sayfa blobu | 3-63 karakter uzunluğunda olan geçerli bir DNS adı olmalıdır. <br>  Bir harf veya sayı ile başlamalıdır. <br> Yalnızca küçük harf, sayı ve tire (-) içerebilir. <br> Kısa çizgiden (-) hemen önce ve sonra bir harf veya rakam gelmelidir. <br> Art arda kısa çizgi adlarında izin. |
| Azure dosyaları için paylaşım adları                  | Yukarıdakiyle aynı                                                                                                                                                                                                                                                                                                             |
| Azure dosyaları için dizin ve dosya adları     |<li> Durum koruma, büyük/küçük harfe ve 255 karakterden uzun olmamalıdır. </li><li> (/) İleri eğik çizgiyle bitemez. </li><li>Sağlanırsa, otomatik olarak kaldırılacak. </li><li> Şu karakterler izin verilmez: <code>" \\ / : \| < > * ?</code></li><li> Ayrılmış URL karakterleri doğru şekilde atlanmalıdır. </li><li> Geçersiz URL yolu karakterler izin verilmez. Kod noktaları gibi \\uE000 Unicode karakterler geçerli değildir. Bazı ASCII veya Unicode karakterleri gibi denetim karakterleri (0x00 için 0x1F \\u0081, vb.), izin verilmeyen de. Unicode yöneten kurallar için HTTP/1.1 dizelerde RFC 2616 ', bölüm 2.2 bakın: Temel kurallar ve RFC 3987. </li><li> Şu dosya adlarına izin verilmez: LPT1, LPT2, LPT3, LPT4, LPT5, LPT6, LPT7, LPT8, LPT9, COM1, COM2, COM3, COM4, COM5, COM6, COM7, COM8, COM9, PRN, AUX, NUL, CON, CLOCK$, nokta karakteri (.) ve iki nokta karakteri (..).</li>|
| Blok blobu ve sayfa blobu için blob adları      | </li><li>Blob adları büyük/küçük harfe duyarlıdır ve karakterler herhangi bir düzende sıralanabilir. </li><li>Blob adı 1 ila 1024 karakter uzunluğunda olmalıdır. </li><li>Ayrılmış URL karakterleri doğru şekilde atlanmalıdır. </li><li>Blob adını oluşturan yolun bölümleri 254 karakterden uzun olamaz. Yol bölümü, arka arkaya gelen sınırlayıcı karakterlerinin (örneğin eğik çizgi "/") arasında yer alan ve bir sanal dizinin adına karşılık gelen dizedir.</li> |
