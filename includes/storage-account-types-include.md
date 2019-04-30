---
title: include dosyası
description: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 03/23/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: d4f57eca89cbb68d61546c6d5ce5bcd04f9256e7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61477984"
---
Azure Storage birkaç depolama hesabı türü sunar. Her tür farklı özellikleri destekler ve kendi fiyatlandırma modeline sahiptir. Uygulamalarınız için en iyi olan hesap türünü belirlemek için bir depolama hesabı oluşturmadan önce bu farklılıkları göz önünde bulundurun. Depolama hesabı türleri şunlardır:

- **Genel amaçlı v2 hesapları**: Bloblar, dosyalar, kuyruklar ve tablolar için temel bir depolama hesabı türü. Azure depolama kullanan çoğu senaryo için önerilir.
- **Genel amaçlı v1 hesapları**: Bloblar, dosyalar, kuyruklar ve tablolar için eski hesap türü. Genel amaçlı v2 hesapları mümkün olduğunda kullanın.
- **Blob depolama hesapları Block**: Premium performans özellikleri olan yalnızca BLOB Depolama hesapları. Yüksek işlem hızı, daha küçük nesneleri kullanarak ya da depolama tutarlı düşük gecikme süresi gerektiren senaryolar için önerilir.
- **Dosya deposundan (Önizleme) depolama hesapları**: Premium performans özellikleri olan dosyaları yalnızca depolama hesabı. Kurumsal veya yüksek performans ölçekli uygulamalar için önerilir.
- **BLOB Depolama hesapları**: Yalnızca BLOB Depolama hesapları. Genel amaçlı v2 hesapları mümkün olduğunda kullanın.

Aşağıdaki tabloda, depolama hesabı türleri ve yeteneklerini açıklar:

| Depolama hesabı türü | Desteklenen hizmetler                       | Desteklenen performans katmanları      | Desteklenen erişim katmanları         | Çoğaltma seçenekleri               | Dağıtım modeli<sup>1</sup> | Şifreleme<sup>2</sup> |
|----------------------|------------------------------------------|-----------------------------|--------------------------------|-----------------------------------|------------------------------|------------------------|
| Genel amaçlı V2   | BLOB, dosya, kuyruk, tablo ve diski       | Standart, Premium<sup>5</sup> | Sık, seyrek arşiv<sup>3</sup> | LRS, ZRS<sup>4</sup>, GRS, RA-GRS | Resource Manager             | Şifreli              |
| Genel amaçlı V1   | BLOB, dosya, kuyruk, tablo ve diski       | Standart, Premium<sup>5</sup> | Yok                            | LRS, GRS, RA-GRS                  | Resource Manager, Klasik    | Şifreli              |
| Blok blobu depolama   | BLOB (blok blobları ve ekleme blobları yalnızca) | Premium                       | Yok                            | LRS                               | Resource Manager             | Şifreli              |
| Dosya deposundan (Önizleme)   | Yalnızca dosyaları | Premium                       | Yok                            | LRS                               | Resource Manager             | Şifreli              |
| Blob depolama         | BLOB (blok blobları ve ekleme blobları yalnızca) | Standart                      | Sık, seyrek arşiv<sup>3</sup> | LRS, GRS, RA-GRS                  | Resource Manager             | Şifreli              |

<sup>1</sup>Azure Resource Manager dağıtım modelini kullanarak önerilir. Depolama hesapları Klasik dağıtım modeli kullanılarak yine de bazı yerlerde oluşturulabilir ve mevcut Klasik hesapları desteklemeye devam eder. Daha fazla bilgi için [Azure Resource Manager ve klasik dağıtım: Dağıtım modellerini ve kaynaklarınızın durumunu anlama](../articles/azure-resource-manager/resource-manager-deployment-model.md).

<sup>2</sup>tüm depolama hesapları, bekleyen veriler için depolama hizmeti şifrelemesi (SSE) kullanılarak şifrelenir. Daha fazla bilgi için [bekleyen veriler için Azure depolama hizmeti şifrelemesi](../articles/storage/common/storage-service-encryption.md).

<sup>3</sup>arşiv katmanı değil yalnızca tek bir blob düzeyinde kullanılabilir depolama hesap düzeyinde. Yalnızca blok blobları ve ekleme blobları arşivlenen. Daha fazla bilgi için [Azure Blob Depolama: Seyrek erişimli, seyrek ve Arşiv depolama katmanları](../articles/storage/blobs/storage-blob-storage-tiers.md).

<sup>4</sup>bölgesel olarak yedekli depolama (ZRS) yalnızca standart genel amaçlı v2 depolama hesapları için kullanılabilir. ZRS hakkında daha fazla bilgi için bkz: [bölgesel olarak yedekli depolama (ZRS): Azure depolama yüksek kullanılabilirliğe sahip uygulamalar](../articles/storage/common/storage-redundancy-zrs.md). Diğer çoğaltma seçenekleri hakkında daha fazla bilgi için bkz. [Azure depolama çoğaltma](../articles/storage/common/storage-redundancy.md).

<sup>5</sup> premium performans için genel amaçlı v2 ve genel amaçlı v1 hesapları yalnızca disk ve sayfa blob'u için kullanılabilir.