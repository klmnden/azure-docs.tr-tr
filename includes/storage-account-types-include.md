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
ms.openlocfilehash: d96e69fb526cff633c78e9ac8a1679762014cd4b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67133567"
---
Azure Storage birkaç depolama hesabı türü sunar. Her tür farklı özellikleri destekler ve kendi fiyatlandırma modeline sahiptir. Uygulamalarınız için en iyi olan hesap türünü belirlemek için bir depolama hesabı oluşturmadan önce bu farklılıkları göz önünde bulundurun. Depolama hesabı türleri şunlardır:

- **Genel amaçlı v2 hesapları**: Bloblar, dosyalar, kuyruklar ve tablolar için temel bir depolama hesabı türü. Azure depolama kullanan çoğu senaryo için önerilir.
- **Genel amaçlı v1 hesapları**: Bloblar, dosyalar, kuyruklar ve tablolar için eski hesap türü. Genel amaçlı v2 hesapları mümkün olduğunda kullanın.
- **Blob depolama hesapları Block**: Premium performans özellikleri olan yalnızca BLOB Depolama hesapları. Yüksek işlem hızı, daha küçük nesneleri kullanarak ya da depolama tutarlı düşük gecikme süresi gerektiren senaryolar için önerilir.
- **Dosya deposundan (Önizleme) depolama hesapları**: Premium performans özellikleri olan dosyaları yalnızca depolama hesabı. Kurumsal veya yüksek performans ölçekli uygulamalar için önerilir.
- **BLOB Depolama hesapları**: Yalnızca BLOB Depolama hesapları. Genel amaçlı v2 hesapları mümkün olduğunda kullanın.

Aşağıdaki tabloda, depolama hesabı türleri ve yeteneklerini açıklar:

| Depolama hesabı türü | Desteklenen hizmetler                       | Desteklenen performans katmanları      | Desteklenen erişim katmanları         | Çoğaltma seçenekleri               | Dağıtım modeli<div role="complementary" aria-labelledby="deployment-model"><sup>1</sup></div> | Şifreleme<div role="complementary" aria-labelledby="encryption"><sup>2</sup></div> |
|----------------------|------------------------------------------|-----------------------------|--------------------------------|-----------------------------------|------------------------------|------------------------|
| Genel amaçlı V2   | BLOB, dosya, kuyruk, tablo ve diski       | Standart, Premium<div role="complementary" aria-labelledby="premium-performance"><sup>5</sup></div> | Sık erişimli, seyrek erişimli ve Arşiv<div role="complementary" aria-labelledby="archive"><sup>3</sup></div> | LRS, GRS, RA-GRS, ZRS<div role="complementary" aria-labelledby="zone-redundant-storage"><sup>4</sup></div> | Resource Manager             | Şifreli              |
| Genel amaçlı V1   | BLOB, dosya, kuyruk, tablo ve diski       | Standart, Premium<div role="complementary" aria-labelledby="premium-performance"><sup>5</sup></div> | Yok                            | LRS, GRS, RA-GRS                  | Resource Manager, Klasik    | Şifreli              |
| Blok blobu depolama   | BLOB (blok blobları ve ekleme blobları yalnızca) | Premium                       | Yok                            | LRS                               | Resource Manager             | Şifreli              |
| Dosya deposundan (Önizleme)   | Yalnızca dosyaları | Premium                       | Yok                            | LRS                               | Resource Manager             | Şifreli              |
| Blob depolama         | BLOB (blok blobları ve ekleme blobları yalnızca) | Standart                      | Sık erişimli, seyrek erişimli ve Arşiv<div role="complementary" aria-labelledby="archive"><sup>3</sup></div> | LRS, GRS, RA-GRS                  | Resource Manager             | Şifreli              |

<div id="deployment-model"><sup>1</sup>Azure Resource Manager dağıtım modelini kullanarak önerilir. Depolama hesapları Klasik dağıtım modeli kullanılarak yine de bazı yerlerde oluşturulabilir ve mevcut Klasik hesapları desteklemeye devam eder. Daha fazla bilgi için <a href="https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model">Azure Resource Manager ve klasik dağıtım: Dağıtım modellerini ve kaynaklarınızın durumunu anlama</a>.</div>

<div id="encryption"><sup>2</sup>tüm depolama hesapları, bekleyen veriler için depolama hizmeti şifrelemesi (SSE) kullanılarak şifrelenir. Daha fazla bilgi için <a href="https://docs.microsoft.com/azure/storage/common/storage-service-encryption">bekleyen veriler için Azure depolama hizmeti şifrelemesi</a>.</div>

<div id="archive"><sup>3</sup>arşiv katmanı değil yalnızca tek bir blob düzeyinde kullanılabilir depolama hesap düzeyinde. Yalnızca blok blobları ve ekleme blobları arşivlenen. Daha fazla bilgi için <a href="https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers">Azure Blob Depolama: Seyrek erişimli, seyrek ve Arşiv depolama katmanları</a>.</div>

<div id="zone-redundant-storage"><sup>4</sup>bölgesel olarak yedekli depolama (ZRS) yalnızca standart genel amaçlı v2 depolama hesapları için kullanılabilir. ZRS hakkında daha fazla bilgi için bkz: <a href="https://docs.microsoft.com/azure/storage/common/storage-redundancy-zrs">bölgesel olarak yedekli depolama (ZRS): Azure depolama yüksek kullanılabilirliğe sahip uygulamalar</a>. Diğer çoğaltma seçenekleri hakkında daha fazla bilgi için bkz. <a href="https://docs.microsoft.com/azure/storage/common/storage-redundancy">Azure depolama çoğaltma</a>.</div>

<div id="premium-performance"><sup>5</sup>premium performans için genel amaçlı v2 ve genel amaçlı v1 hesapları yalnızca disk ve sayfa blob'u için kullanılabilir.</div>
