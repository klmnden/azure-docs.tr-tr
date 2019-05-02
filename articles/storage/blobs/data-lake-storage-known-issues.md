---
title: Azure Data Lake depolama Gen2 ile'ilgili bilinen sorunlar | Microsoft Docs
description: Azure Data Lake depolama Gen2 ile'ilgili bilinen sorunlar ve sınırlamalar hakkında bilgi edinin
services: storage
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 02/28/2019
ms.author: normesta
ms.openlocfilehash: 51230fe050c67253dd5b2bb3f23d03512df64ef6
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64939240"
---
# <a name="known-issues-with-azure-data-lake-storage-gen2"></a>Azure Data Lake depolama Gen2 ile'ilgili bilinen sorunlar

Bu makale, Data Lake depolama Gen2 geçici ilişkin sınırlamalar ve bilinen sorunları içerir.

## <a name="sdk-support-for-data-lake-storage-gen2-accounts"></a>Data Lake depolama Gen2 hesapları için SDK desteği

Data Lake depolama Gen2 hesaplarıyla çalışır SDK'lar değildir.

## <a name="blob-storage-apis"></a>BLOB Depolama API'leri

BLOB Depolama API'leri henüz Data Lake depolama Gen2 hesaplarına kullanıma sunulmaz.

Bu API'ler Blob Depolama API'leri henüz Azure Data Lake Gen2 API'leri ile birlikte çalışabilen olmadığından oluşabilecek yanlışlıkla veri erişim sorunları önlemek için devre dışı bırakılır.

Önce devre dışı bırakıldı ve bu verilere erişmek için bir üretim ihtiyacı olan verileri yüklemek için bu API'leri kullandıysanız, lütfen Microsoft Support aşağıdaki bilgilerle başvurun:

* Abonelik kimliği (GUID, adı değil)

* Depolama hesabı adları

* Üretimde etkin bir şekilde etkilendiğini işlenmeyeceğini ve işlenecekse hangi depolama hesapları için?

* Bu verileri başka bir depolama hesabına herhangi bir nedenden dolayı kopyalanması gerekip gerekmediğini ve bu durumda, üretimde etkin bir şekilde etkilenmez olsa bile, bize neden?

Böylece bu verileri hiyerarşik ad alanı özelliğinin etkin olmayan bir depolama hesabına kopyalayabilirsiniz Bu koşullar altında biz erişim Blob API için sınırlı bir süre için geri yükleyebilirsiniz.

Yönetilmeyen sanal makine (VM) diskleri devre dışı Blob Depolama API'leri bağlıdır, hiyerarşik bir ad alanı bir depolama hesabı üzerinde etkinleştirmek istiyorsanız, bu nedenle yönetilmeyen VM disklerini hiyerarşik ad alanı özelliği olmayan bir depolama hesabına yerleştirmeyi düşünün etkin.

## <a name="api-interoperability"></a>API birlikte çalışabilirlik

BLOB Depolama API'leri ve Azure Data Lake Gen2 API'lerini birbirleri ile birlikte çalışabilir değil.

BLOB API'leri duruma kadar araçları, uygulamaları, hizmetleri veya Blob API'leri kullanan betikler vardır ve bunları tüm hesabınıza yüklediğiniz içerik çalışmak için kullanmak istiyorsanız, ardından bir hiyerarşik ad alanı Blob Depolama hesabınızda etkinleştirme Azure Data Lake Gen2 API'leri ile birlikte çalışabilir. Hiyerarşik ad alanı olmayan bir depolama hesabı kullanarak, dizin ve dosya sistemi erişim denetim listeleri gibi Data Lake depolama Gen2'ye özgü özelliklere erişim ardından gerekmediği anlamına gelir.

## <a name="azure-storage-explorer"></a>Azure Depolama Gezgini

Azure Depolama Gezgini'ni kullanarak Data Lake depolama Gen2 hesapları yönetmek veya görüntülemek için en az olmalıdır sürüm `1.6.0` olarak kullanılabilir olan Aracı'nın bir [ücretsiz](https://azure.microsoft.com/features/storage-explorer/).

Depolama Gezgini, Azure portalında katıştırılmış sürümü şu anda mu Not görüntüleme veya etkin hiyerarşik ad alanı özelliği ile Data Lake depolama Gen2 hesapları yönetme desteği.

## <a name="blob-viewing-tool"></a>BLOB görüntüleme aracı

Azure portalında görüntüleme aracıyla blob yalnızca Data Lake depolama Gen2 desteği sınırlıdır.

## <a name="third-party-applications"></a>Üçüncü taraf uygulamalar

Üçüncü taraf uygulamaları Data Lake depolama Gen2 desteklemiyor olabilir.

Destek her üçüncü taraf uygulama sağlayıcısının takdirinizdedir. Şu anda, Blob Depolama API'lerini ve Data Lake depolama Gen2 API'lerini aynı içeriği yönetmek için kullanılamaz. Bu birlikte çalışabilirliği sağlamak için çalışıyoruz gibi çok sayıda üçüncü taraf araçları otomatik olarak Data Lake depolama Gen2 destekleyecek mümkündür.

## <a name="azcopy-support"></a>AzCopy desteği

Data Lake depolama Gen2 AzCopy 8 sürümünü desteklemiyor.

Bunun yerine, en güncel AzCopy önizleme sürümünü kullanın ( [AzCopy v10](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10?toc=%2fazure%2fstorage%2ftables%2ftoc.json) ) olarak Data Lake depolama Gen2 uç noktalarını destekler.

## <a name="azure-event-grid"></a>Azure Event Grid

[Azure Event Grid](https://azure.microsoft.com/services/event-grid/) hesaplar bunları henüz üretme çünkü olayları Azure Data Lake Gen2 hesaplarından almaz.  

## <a name="soft-delete-and-snapshots"></a>Geçici silme ve anlık görüntüleri

Geçici silme ve anlık görüntüleri, Data Lake depolama Gen2 hesaplarında kullanılamaz.

Dahil tüm sürüm oluşturma özellikleri [anlık görüntüleri](https://docs.microsoft.com/rest/api/storageservices/creating-a-snapshot-of-a-blob) ve [geçici silme](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete) hiyerarşik ad alanı özelliğinin etkin olduğu depolama hesaplarında için henüz kullanılamamaktadır.

## <a name="object-level-storage-tiers"></a>Nesne düzeyinde depolama katmanları

Nesne düzeyinde depolama katmanları (sık erişimli, soğuk ve Arşiv) Azure Data Lake depolama Gen 2 hesapları için henüz kullanılamıyor, ancak hiyerarşik ad alanı özelliğinin etkin olmayan depolama hesapları için kullanılabilir.

## <a name="azure-blob-storage-lifecycle-management-policies"></a>Azure Blob Depolama yaşam döngüsü yönetimi ilkeleri

Azure Blob Depolama yaşam döngüsü yönetimi ilkeleri, Data Lake depolama Gen2 hesapları için henüz kullanılamamaktadır.

Bu ilkeler, hiyerarşik ad alanı özelliğinin etkin olmayan depolama hesapları için kullanılabilir.

## <a name="diagnostic-logs"></a>Tanılama günlükleri

Tanılama günlükleri, Data Lake depolama Gen2 hesaplarında kullanılamaz.
