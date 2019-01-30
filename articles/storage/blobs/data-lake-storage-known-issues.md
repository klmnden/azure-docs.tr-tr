---
title: Azure Data Lake depolama Gen2 ile'ilgili bilinen sorunlar | Microsoft Docs
description: Azure Data Lake depolama Gen2 ile'ilgili bilinen sorunlar ve sınırlamalar hakkında bilgi edinin
services: storage
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 12/05/2018
ms.author: normesta
ms.openlocfilehash: cbd58c0873a4a46d175c6d7cbdf2d004da304c06
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55247247"
---
# <a name="known-issues-with-azure-data-lake-storage-gen2"></a>Azure Data Lake depolama Gen2 ile'ilgili bilinen sorunlar

Bu makale, Azure Data Lake depolama Gen2 geçici ilişkin sınırlamalar ve bilinen sorunları içerir.

## <a name="api-interoperability"></a>API birlikte çalışabilirlik

BLOB Depolama API'leri ve Azure Data Lake Gen2 API'lerini birbirleri ile birlikte çalışabilir değil.

Tüm hesabınıza yüklediğiniz içerik çalışmak için aynı aracı kullanmanız gerekiyorsa, bu API'leri birbirleri ile birlikte çalışabilen duruma kadar Blob Depolama hesabınızda hiyerarşik ad alanları etkinleştirmeyin. Hiyerarşik ad alanı olmayan bir depolama hesabı kullanarak erişim denetimi listeleri ardından dizin ve dosya sistemi gibi Data Lake depolama Gen2 belirli özelliklere erişiminiz yoksa anlamına gelir.

## <a name="blob-storage-apis"></a>BLOB Depolama API'leri

BLOB Depolama API'leri henüz Azure Data Lake depolama Gen2 hesaplarına kullanıma sunulmaz.

Bu API'ler Blob Depolama API'leri henüz Azure Data Lake Gen2 API'leri ile birlikte çalışabilen olmadığından oluşabilecek yanlışlıkla veri erişim sorunları önlemek için devre dışı bırakılır.

Önce devre dışı bırakıldı ve bu verilere erişmek için bir üretim ihtiyacı olan verileri yüklemek için bu API'leri kullandıysanız, lütfen Microsoft Support aşağıdaki bilgilerle başvurun:

* Abonelik kimliği (GUID, adı değil)

* Depolama hesabı adları

* Üretimde etkin bir şekilde etkilendiğini işlenmeyeceğini ve işlenecekse hangi depolama hesapları için?

* Bu verileri başka bir depolama hesabına herhangi bir nedenden dolayı kopyalanması gerekip gerekmediğini ve bu durumda, üretimde etkin bir şekilde etkilenmez olsa bile, bize neden?

Böylece bu verileri hiyerarşik ad alanları etkin olmayan bir depolama hesabına kopyalayabilirsiniz Bu koşullar altında biz erişim Blob API için sınırlı bir süre için geri yükleyebilirsiniz.

Yönetilmeyen sanal makine (VM) diskleri devre dışı Blob Depolama API'leri bağlıdır, hiyerarşik ad alanları bir depolama hesabı üzerinde etkinleştirmek istiyorsanız, bu nedenle yönetilmeyen VM disklerini hiyerarşik ad alanları etkin olmayan bir depolama hesabına yerleştirmeyi düşünün.

## <a name="azure-storage-explorer"></a>Azure Depolama Gezgini

Azure Depolama Gezgini'ni kullanarak Data Lake depolama Gen2 hesapları yönetmek veya görüntülemek için en az olmalıdır sürüm `1.6.0` olarak kullanılabilir olan Aracı'nın bir [ücretsiz](https://azure.microsoft.com/features/storage-explorer/).

Depolama Gezgini, Azure Portalı'na katıştırılmış sürümü şu anda mu Not görüntüleme veya etkin hiyerarşik ad alanları ile Data Lake depolama Gen2 hesaplarını yönetme desteği.

## <a name="blob-viewing-tool"></a>BLOB görüntüleme aracı

Azure portalında görüntüleme aracıyla blob yalnızca Azure Data Lake depolama Gen2 desteği sınırlıdır.

## <a name="third-party-applications"></a>Üçüncü taraf uygulamalar

Üçüncü taraf uygulamaları, Azure Data Lake depolama Gen2 desteklemiyor olabilir.

Destek her üçüncü taraf uygulama sağlayıcısının takdirinizdedir. Şu anda, Blob Depolama API'leri ve aynı içeriği yönetmek için Azure Data Lake depolama Gen2 API'lerini kullanılamaz. Bu birlikte çalışabilirliği sağlamak için çalışıyoruz gibi çok sayıda üçüncü taraf araçları otomatik olarak Azure Data Lake depolama Gen2 destekleyecek mümkündür.

## <a name="azcopy-support"></a>AzCopy desteği

Sürüm 8 AzCopy, Azure Data Lake depolama Gen2 desteklemiyor.

Bunun yerine, en güncel AzCopy önizleme sürümünü kullanın ( [AzCopy v10](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10?toc=%2fazure%2fstorage%2ftables%2ftoc.json) ) olarak Azure Data Lake depolama Gen2 uç noktalarını destekler.

## <a name="azure-event-grid"></a>Azure Event Grid

[Azure Event Grid](https://azure.microsoft.com/services/event-grid/) hesaplar bunları henüz üretme çünkü olayları Azure Data Lake Gen2 hesaplarından almaz.  

## <a name="soft-delete-and-snapshots"></a>Geçici silme ve anlık görüntüleri

Geçici silme ve anlık görüntüleri, Azure Data Lake depolama Gen2 hesaplarında kullanılamaz.

Dahil tüm sürüm oluşturma özellikleri [anlık görüntüleri](https://docs.microsoft.com/rest/api/storageservices/creating-a-snapshot-of-a-blob) ve [geçici silme](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete) hiyerarşik ad alanları etkin olduğu depolama hesaplarında için henüz kullanılamamaktadır.

## <a name="object-level-storage-tiers"></a>Nesne düzeyinde depolama katmanları

Nesne düzeyinde depolama katmanları (sık erişimli, soğuk ve Arşiv) Azure Data Lake depolama Gen 2 hesapları için henüz kullanılamıyor, ancak etkin hiyerarşik boşluk olmayan depolama hesapları için kullanılabilir.

## <a name="azure-blob-storage-lifecycle-management-preview-policies"></a>Azure Blob Depolama yaşam döngüsü yönetimi (Önizleme) İlkeleri

Azure Blob Depolama yaşam döngüsü yönetimi (Önizleme) ilkeleri, Azure Data Lake depolama Gen2 hesapları için henüz kullanılamamaktadır.

Bu ilkeler, hiyerarşik alanları etkin olmayan depolama hesapları için kullanılabilir.

## <a name="diagnostic-logs"></a>Tanılama günlükleri

Tanılama günlükleri, Azure Data Lake depolama Gen2 hesaplarında kullanılamaz.

Tanılama günlükleri istemek için Azure desteğine başvurun. Bunları, hesabınızın adını ve günlükleri duyduğunuz süre sağlar.
