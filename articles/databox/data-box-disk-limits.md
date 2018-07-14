---
title: Azure Data Box Disk sınırlar | Microsoft Docs
description: Sistem sınırlarını ve önerilen boyut için Microsoft Azure Data Box Disk açıklar.
services: databox
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/12/2018
ms.author: alkohli
ms.custom: ''
ms.openlocfilehash: 4db70fa93914ba0544d9beb8e523241513a2e5ce
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39009304"
---
# <a name="azure-data-box-disk-limits-preview"></a>Azure Data Box Disk sınırları (Önizleme)


Limitler, dağıtmanıza ve Microsoft Azure Data Box Disk çözümünüz olarak düşünün. 

> [!IMPORTANT] 
> Azure Data Box Disk Önizleme aşamasındadır. Gözden geçirme [Önizleme kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) bu çözümü dağıtmadan önce. 


## <a name="data-box-service-limits"></a>Veri kutusu hizmeti sınırları

 - Veri kutusu hizmeti yalnızca ABD ve AB Azure genel bulutu için tüm Azure bölgelerinde kullanılabilir durumdadır.
 - Tek bir depolama hesabında Data Box Disk ile desteklenir.

## <a name="data-box-disk-performance"></a>Veri kutusu Disk performansı

USB 3.0 üzerinden bağlı disklere sahip test edildiğinde, disk performansını kadar 430 MB/sn oluştu. Gerçek sayıları, kullanılan dosya boyutuna bağlı olarak değişir. Daha küçük dosyalar için daha düşük performans görebilirsiniz.

## <a name="azure-storage-limits"></a>Azure depolama sınırları

Bu bölümde, Azure dosyaları, Azure blok BLOB'ları ve Data Box hizmeti uygunsa Azure sayfa blobları için Azure depolama hizmeti için sınırlar ve gerekli adlandırma kurallarını açıklar. Depolama sınırları dikkatle gözden geçirin ve tüm önerilere uyun.

Azure depolama hizmet sınırları ve adlandırma paylaşımları, kapsayıcılar ve dosyalar için en iyi yöntemler üzerinde en son bilgiler için bkz:

- [Adlandırma ve kapsayıcıları başvurma](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata)
- [Adlandırma ve paylaşımları başvurma](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-shares--directories--files--and-metadata)
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

Depolama hesabına kopyalanır veri boyutu sınırları şunlardır. Karşıya yüklediğiniz veriler için limitler uyduğundan emin olun. Bu sınırlar en güncel bilgiler için Git [Azure blob depolama ölçek hedefleri](https://docs.microsoft.com/en-us/azure/storage/cstorage-scalability-targets#azure-blob-storage-scale-targets) ve [Azure dosyaları ölçeklendirme hedeflerini](https://docs.microsoft.com/en-us/azure/storage/common/storage-scalability-targets#azure-files-scale-targets).

| Azure depolama hesabına kopyalanan verileri boyutu                      | Varsayılan Sınır          |
|---------------------------------------------------------------------|------------------------|
| Blok blobu ve sayfa blob                                            | Depolama hesabı başına 500 TB. <br> Bu Data Box Disk dahil olmak üzere tüm kaynaklardan gelen verileri içerir.|


## <a name="azure-object-size-limits"></a>Azure nesne boyutu sınırları

Azure nesnelerin yazılabilir boyutları aşağıda verilmiştir. Yüklenen tüm dosyalar bu sınırlara uyacak emin olun.

| Azure nesne türü | Varsayılan Sınır                                             |
|-------------------|-----------------------------------------------------------|
| Blok blobu        | ~ 8 TB                                                 |
| Sayfa blobu         | 1 TB <br> (Sayfa blobu biçiminde yüklenen her dosya, 512 bayt hizalı (tamsayı katı) olmalıdır, aksi takdirde yükleme başarısız olur. <br> VHD ve VHDX 512 bayt hizalı var.) |


## <a name="azure-block-blob-and-page-blob-naming-conventions"></a>Azure blok blobu ve sayfa blob'u adlandırma kuralları

| Varlık                                       | Kuralları                                                                                                                                                                                                                                                                                                               |
|----------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Kapsayıcı adları için blok blobu ve sayfa blobu | 3-63 karakter uzunluğunda olan geçerli bir DNS adı olmalıdır. <br>  Bir harf veya sayı ile başlamalıdır. <br> Yalnızca küçük harf, sayı ve tire (-) içerebilir. <br> Her tire (-) hemen önce ve bir harf veya rakam olmalıdır. <br> Art arda kısa çizgi adlarında izin verilmez. |
| Blok blobu ve sayfa blobu için BLOB adları      | BLOB adları büyük küçük harfe duyarlıdır ve herhangi bir karakter birleşimini içerebilir. <br> Blob adı 1-1024 karakter uzunluğunda olmalıdır. <br> Ayrılmış URL karakterleri için doğru kaçış gerekir. <br>Blob adı kapsayan yol bölümlerinin sayısını 254 aşamaz. Bir yol kesimi dizedir arka arkaya sınırlayıcı karakter arasında (örneğin, eğik '/') bir sanal dizin adına karşılık gelir. |
