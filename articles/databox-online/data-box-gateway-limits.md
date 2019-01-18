---
title: Azure veri kutusu ağ geçidi sınırlar | Microsoft Docs
description: Microsoft Azure veri kutusu ağ geçidi için sistem sınırlarını ve önerilen boyut açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 01/15/2019
ms.author: alkohli
ms.openlocfilehash: f01fb88bf0ea726b421111a262bdfdd68cd3d38c
ms.sourcegitcommit: 9f07ad84b0ff397746c63a085b757394928f6fc0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2019
ms.locfileid: "54388417"
---
# <a name="azure-data-box-gateway-limits-preview"></a>Azure veri kutusu ağ geçidi sınırları (Önizleme)


Limitler, dağıtmanıza ve Microsoft Azure veri kutusu ağ geçidi çözümünüz olarak düşünün. 

> [!IMPORTANT] 
> Data Box Gateway, Önizleme aşamasındadır. Bu çözümü dağıtmadan önce [önizleme için kullanım koşullarını](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) gözden geçirin. 


## <a name="data-box-gateway-service-limits"></a>Veri kutusu Gateway hizmet limitleri

- Bu sürümde, hizmet yalnızca ABD, AB ve Asya Pasifik belirli bölgelerde kullanılabilir durumdadır. Daha fazla bilgi için [bölge kullanılabilirliği](#data-box-gateway-overview#region-availability)’ne gidin. Depolama hesabı bölgeye cihazın dağıtıldığı fiziksel olarak yakın olmalıdır (hizmet coğrafi bölgesinden farklı olabilir).
- Farklı bir abonelik veya kaynak grubu için bir veri kutusu ağ geçidi kaynağı taşıma desteklenmiyor. Daha fazla ayrıntı için [kaynakları yeni kaynak grubuna veya aboneliğe taşıma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources).

## <a name="data-box-gateway-device-limits"></a>Veri kutusu ağ geçidi cihaz sınırları

Veri kutusu ağ geçidi cihazı için sınırlar aşağıdaki tabloda açıklanmaktadır.

| Açıklama | Değer |
|---|---|
|Hayır. cihaz başına dosyaları |100 milyon <br> Sınır ~ her 2 TB disk alanı ile en fazla 100 milyon sınırında 25 milyon dosya |
|Hayır. cihaz başına paylaşılma sayısı |24 |
|Bir paylaşıma yazılan en büyük dosya boyutu|2 TB sanal cihaz için en büyük dosya boyutu 500 GB'dir. <br> En çok 5 TB ulaşana kadar önceki oranını veri disk boyutu ile maksimum dosya boyutu artar. |

## <a name="azure-storage-limits"></a>Azure depolama sınırları

Bu bölümde, Azure dosyaları, Azure blok BLOB'ları ve veri kutusu ağ geçidi/veri kutusu Edge hizmetine uygunsa Azure sayfa blobları için Azure depolama hizmeti için sınırlar ve gerekli adlandırma kurallarını açıklar. Depolama sınırları dikkatle gözden geçirin ve tüm önerilere uyun.

Azure depolama hizmet sınırları ve adlandırma paylaşımları, kapsayıcılar ve dosyalar için en iyi yöntemler üzerinde en son bilgiler için bkz:

- [Adlandırma ve kapsayıcıları başvurma](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata)
- [Paylaşımları adlandırma ve onlara başvurma](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-shares--directories--files--and-metadata)
- [Blok blobları ve sayfa blob kuralları](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs)

> [!IMPORTANT]
> Herhangi bir dosya ya da Azure depolama hizmeti limitlerin ya da Azure dosya/Blob adlandırma kurallarına uygun olmayan dizin varsa, ardından bu dosyaları veya dizinleri veri kutusu ağ geçidi/veri kutusu Edge hizmeti aracılığıyla Azure Depolama'ya alınan değil.

## <a name="data-upload-caveats"></a>Uyarılar karşıya veri yükleme

Azure'a taşınırken aşağıdaki uyarılar için veri uygulayın.

- Birden fazla cihaz olanla aynı kapsayıcıya yazamaz öneririz.
- Kopyalanan nesne olarak aynı ada sahip bir bulutta (örneğin, bir bloba veya bir dosya) var olan bir Azure nesne varsa, cihaz bulut dosyanın üzerine yazar.
- Paylaşım klasör altında oluşturulan bir boş dizin hiyerarşisi (olmadan tüm dosyaları) için blob kapsayıcılar karşıya yüklenmemiş.
- Cihaz boyutundan büyük dosyalar kopyalıyorsanız kullanmak için önerilir *Robocopy* veya *rsync* herhangi bir hata olmadığından emin olun.

## <a name="azure-storage-account-size-and-object-size-limits"></a>Azure depolama hesabı boyut ve nesne boyutu sınırları

Depolama hesabına kopyalanır veri boyutu sınırları şunlardır. Karşıya yüklediğiniz veriler için limitler uyduğundan emin olun. Bu sınırlar en güncel bilgiler için Git [Azure blob depolama ölçek hedefleri](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets#azure-blob-storage-scale-targets) ve [Azure dosyaları ölçeklendirme hedeflerini](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets#azure-files-scale-targets).

| Azure depolama hesabına kopyalanan verileri boyutu                      | Varsayılan Sınır          |
|---------------------------------------------------------------------|------------------------|
| Blok blobu ve sayfa blob                                            | Depolama hesabı başına 500 TB|


## <a name="azure-object-size-limits"></a>Azure nesne boyutu sınırları

Azure nesnelerin yazılabilir boyutları aşağıda verilmiştir. Yüklenen tüm dosyalar bu sınırlara uyacak emin olun.

| Azure nesne türü | Varsayılan limit                                             |
|-------------------|-----------------------------------------------------------|
| Blok Blobu        | ~ 8 TB                                                 |
| Sayfa Blobu         | 1 TB <br> Sayfa blobu biçiminde yüklenen her dosya 512 bayt hizalı (tamsayı katı) olmalıdır, aksi takdirde yükleme başarısız olur. <br> VHD ve VHDX 512 bayt hizalı var. |


## <a name="next-steps"></a>Sonraki adımlar

- [Azure Data Box Gateway'i dağıtmaya hazırlanma](data-box-gateway-deploy-prep.md)
