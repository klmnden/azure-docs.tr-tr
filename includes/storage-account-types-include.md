---
title: include dosyası
description: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 01/02/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: b278cce8f885f31f9d59fd389261812e04e58405
ms.sourcegitcommit: 3ab534773c4decd755c1e433b89a15f7634e088a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/07/2019
ms.locfileid: "54066999"
---
Azure depolama üç tür depolama hesabı sunuyor. Her tür farklı özellikleri destekler ve kendi fiyatlandırma modeline sahiptir. Uygulamalarınız için en iyi olan hesap türünü belirlemek için bir depolama hesabı oluşturmadan önce bu farklılıkları göz önünde bulundurun. Depolama hesapları üç tür şunlardır:

* **Genel amaçlı v2 hesapları**: Bloblar, dosyalar, kuyruklar ve tablolar için temel bir depolama hesabı türü. Azure depolama kullanan çoğu senaryo için önerilir.
* **Genel amaçlı v1 hesapları**: Bloblar, dosyalar, kuyruklar ve tablolar için eski hesap türü. Genel amaçlı v2 hesapları mümkün olduğunda kullanın.
* **BLOB Depolama hesapları**: Yalnızca BLOB Depolama hesapları. Genel amaçlı v2 hesapları mümkün olduğunda kullanın. 

Aşağıdaki tabloda, depolama hesabı türleri ve yeteneklerini açıklar:

| Depolama hesabı türü | Desteklenen hizmetler                       | Desteklenen performans katmanları | Desteklenen erişim katmanları               | Çoğaltma seçenekleri                                                | Dağıtım modeli<sup>1</sup>  | Şifreleme<sup>2</sup> |
|----------------------|------------------------------------------|-----------------------------|--------------------------------------|--------------------------------------------------------------------|-------------------|------------|
| Genel amaçlı V2   | BLOB, dosya, kuyruk, tablo ve diski       | Standart, Premium           | Sık, seyrek arşiv<sup>3</sup> | LRS, ZRS<sup>4</sup>, GRS, RA-GRS | Resource Manager | Şifreli  |
| Genel amaçlı V1   | BLOB, dosya, kuyruk, tablo ve diski       | Standart, Premium           | Yok                                  | LRS, GRS, RA-GRS                                                   | Resource Manager, Klasik  | Şifreli  |
| Blob depolama         | BLOB (blok blobları ve ekleme blobları yalnızca) | Standart                    | Sık, seyrek arşiv<sup>3</sup>                            | LRS, GRS, RA-GRS                                                   | Resource Manager  | Şifreli  |

<sup>1</sup>Azure Resource Manager dağıtım modelini kullanarak önerilir. Depolama hesapları Klasik dağıtım modeli kullanılarak yine de bazı yerlerde oluşturulabilir ve mevcut Klasik hesapları desteklemeye devam eder. Daha fazla bilgi için [Azure Resource Manager ve klasik dağıtım: Dağıtım modellerini ve kaynaklarınızın durumunu anlama](../articles/azure-resource-manager/resource-manager-deployment-model.md).

<sup>2</sup>tüm depolama hesapları, bekleyen veriler için depolama hizmeti şifrelemesi (SSE) kullanılarak şifrelenir. Daha fazla bilgi için [bekleyen veriler için Azure depolama hizmeti şifrelemesi](../articles/storage/common/storage-service-encryption.md).

<sup>3</sup>arşiv katmanı değil yalnızca tek bir blob düzeyinde kullanılabilir depolama hesap düzeyinde. Yalnızca blok blobları ve ekleme blobları arşivlenen. Daha fazla bilgi için [Azure Blob Depolama: Seyrek erişimli, seyrek ve Arşiv depolama katmanları](../articles/storage/blobs/storage-blob-storage-tiers.md).

<sup>4</sup>bölgesel olarak yedekli depolama (ZRS) yalnızca standart genel amaçlı v2 depolama hesapları için kullanılabilir. ZRS hakkında daha fazla bilgi için bkz: [bölgesel olarak yedekli depolama (ZRS): Azure depolama yüksek kullanılabilirliğe sahip uygulamalar](../articles/storage/common/storage-redundancy-zrs.md). Diğer çoğaltma seçenekleri hakkında daha fazla bilgi için bkz. [Azure depolama çoğaltma](../articles/storage/common/storage-redundancy.md).
