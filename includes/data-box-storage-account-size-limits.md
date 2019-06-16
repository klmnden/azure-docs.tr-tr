---
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: include
ms.date: 05/21/2019
ms.author: alkohli
ms.openlocfilehash: 7b46360886387bc4ab48f764edb5d01dc52dc812
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66244635"
---
Depolama hesabına kopyalanır veri boyutu sınırları şunlardır. Karşıya yüklediğiniz veriler için limitler uyduğundan emin olun. Bu sınırlar en güncel bilgiler için Git [Azure blob depolama ölçek hedefleri](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets#azure-blob-storage-scale-targets) ve [Azure dosyaları ölçeklendirme hedeflerini](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets#azure-files-scale-targets).

| Azure depolama hesabına kopyalanan verileri boyutu                      | Varsayılan limit          |
|---------------------------------------------------------------------|------------------------|
| Blok blobu ve sayfa blobu                                            | Depolama hesabı başına 500 TiB. <br> Bu Data Box dahil olmak üzere tüm kaynaklardan gelen verileri içerir.|
| Azure dosya                                                          | Paylaşım başına 5 TiB.<br> Altındaki tüm klasörleri *StorageAccount_AzureFiles* bu sınıra uymalıdır.       |
