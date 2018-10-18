---
title: Azure Data Box Disk sınırlar | Microsoft Docs
description: Sistem sınırlarını ve önerilen boyut için Microsoft Azure Data Box Disk açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: article
ms.date: 09/04/2018
ms.author: alkohli
ms.openlocfilehash: 1a4fe30881f06d8af851a67f389a6faafbe3dfef
ms.sourcegitcommit: f20e43e436bfeafd333da75754cd32d405903b07
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49389471"
---
# <a name="azure-data-box-disk-limits-preview"></a>Azure Data Box Disk sınırları (Önizleme)


Limitler, dağıtmanıza ve Microsoft Azure Data Box Disk çözümünüz olarak düşünün. 

> [!IMPORTANT] 
> Azure Data Box Disk Önizleme aşamasındadır. Bu çözümü dağıtmadan önce [önizleme için kullanım koşullarını](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) gözden geçirin. 


## <a name="data-box-service-limits"></a>Veri kutusu hizmeti sınırları

 - Veri kutusu hizmeti yalnızca ABD, AB, Kanada ve Avustralya'da Azure genel bulutu için tüm Azure bölgelerinde kullanılabilir durumdadır.
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

- Veri diskleri doğrudan kopyalamayın. Önceden oluşturulmuş veri kopyalama *BlockBlob* ve *PageBlob* klasörleri.
- Bir klasörü altında *BlockBlob* ve *PageBlob* bir kapsayıcıdır. Örneğin, kapsayıcıları olarak oluşturulur *BlockBlob/kapsayıcı* ve *PageBlob/kapsayıcı*.
- Data Box Disk, kopyalanan nesne olarak aynı ada sahip bir bulutta mevcut bir Azure nesne (örneğin, bir blobu) varsa, bulutta dosyanın üzerine yazar.
- Her dosyanın içine yazılmış *BlockBlob* ve *PageBlob* paylaşımları yüklendiği bir blok blobu ve sayfa blobu olarak sırasıyla.
- Herhangi bir boş (olmadan tüm dosyaları) dizin hiyerarşisi altında oluşturulan *BlockBlob* ve *PageBlob* klasörleri karşıya.
- Herhangi bir hata varsa verileri Azure'a karşıya yüklenirken bir hata günlüğü hedef depolama hesabında oluşturulur. Bu hata günlük yolunu, karşıya yükleme tamamlandıktan ve düzeltme eylemi için günlüğü gözden geçirebilirsiniz portalda kullanılabilir. Veri kaynağından karşıya yüklenen veriler doğrulamadan silmeyin.

## <a name="azure-storage-account-size-limits"></a>Azure depolama hesabı boyut sınırları

Depolama hesabına kopyalanır veri boyutu sınırları şunlardır. Karşıya yüklediğiniz veriler için limitler uyduğundan emin olun. Bu sınırlar en güncel bilgiler için Git [Azure blob depolama ölçek hedefleri](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets#azure-blob-storage-scale-targets) ve [Azure dosyaları ölçeklendirme hedeflerini](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets#azure-files-scale-targets).

| Azure depolama hesabına kopyalanan verileri boyutu                      | Varsayılan limit          |
|---------------------------------------------------------------------|------------------------|
| Blok blobu ve sayfa blob                                            | Depolama hesabı başına 500 TB. <br> Bu Data Box Disk dahil olmak üzere tüm kaynaklardan gelen verileri içerir.|


## <a name="azure-object-size-limits"></a>Azure nesne boyutu sınırları

Azure nesnelerin yazılabilir boyutları aşağıda verilmiştir. Yüklenen tüm dosyalar bu sınırlara uyacak emin olun.

| Azure nesne türü | Varsayılan limit                                             |
|-------------------|-----------------------------------------------------------|
| Blok Blobu        | ~ 8 TB                                                 |
| Sayfa Blobu         | 1 TB <br> (Sayfa blobu biçiminde yüklenen her dosya, 512 bayt hizalı (tamsayı katı) olmalıdır, aksi takdirde yükleme başarısız olur. <br> VHD ve VHDX 512 bayt hizalı var.) |


## <a name="azure-block-blob-and-page-blob-naming-conventions"></a>Azure blok blobu ve sayfa blob'u adlandırma kuralları

| Varlık                                       | Kurallar                                                                                                                                                                                                                                                                                                               |
|----------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Kapsayıcı adları için blok blobu ve sayfa blobu | 3-63 karakter uzunluğunda olan geçerli bir DNS adı olmalıdır. <br>  Bir harf veya sayı ile başlamalıdır. <br> Yalnızca küçük harf, sayı ve tire (-) içerebilir. <br> Kısa çizgiden (-) hemen önce ve sonra bir harf veya rakam gelmelidir. <br> Adlarda kısa çizgiler art arda kullanılamaz. |
| Blok blobu ve sayfa blobu için blob adları      | Blob adları büyük/küçük harfe duyarlıdır ve karakterler herhangi bir düzende sıralanabilir. <br> Blob adı 1 ila 1024 karakter uzunluğunda olmalıdır. <br> Ayrılmış URL karakterleri doğru şekilde atlanmalıdır. <br>Blob adını oluşturan yolun bölümleri 254 karakterden uzun olamaz. Yol bölümü, arka arkaya gelen sınırlayıcı karakterlerinin (örneğin eğik çizgi "/") arasında yer alan ve bir sanal dizinin adına karşılık gelen dizedir. |


## <a name="next-steps"></a>Sonraki adımlar
* Gözden geçirme [Data Box sistem gereksinimleri](data-box-system-requirements.md)
