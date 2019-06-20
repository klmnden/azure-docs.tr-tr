---
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: include
ms.date: 06/18/2019
ms.author: alkohli
ms.openlocfilehash: 0975df8ee9da4a239dbd72e468b967c88ab3feed
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67277499"
---
Depolama hesabına kopyalanır veri boyutu sınırları şunlardır. Karşıya yüklediğiniz veriler için limitler uyduğundan emin olun. Bu sınırlar en güncel bilgiler için Git [Azure blob depolama ölçek hedefleri](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets#azure-blob-storage-scale-targets) ve [Azure dosyaları ölçeklendirme hedeflerini](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets#azure-files-scale-targets).

| Azure depolama hesabına kopyalanan verileri boyutu                      | Varsayılan limit          |
|---------------------------------------------------------------------|------------------------|
| Blok blobu ve sayfa blobu                                            | 2 PB Amerika ve Avrupa'da için.<br>UK içerir 500 TB diğer tüm bölgeler için.  <br> Bu Data Box dahil olmak üzere tüm kaynaklardan gelen verileri içerir.|
| Azure Dosyaları                                                          | Paylaşım başına 5 TB.<br> Altındaki tüm klasörleri *StorageAccount_AzureFiles* bu sınıra uymalıdır.       |
