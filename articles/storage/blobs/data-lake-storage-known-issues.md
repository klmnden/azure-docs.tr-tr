---
title: Azure Data Lake depolama Gen2 ile'ilgili bilinen sorunlar | Microsoft Docs
description: Azure Data Lake depolama Gen2 ile'ilgili bilinen sorunlar ve sınırlamalar hakkında bilgi edinin
services: storage
author: normesta
ms.component: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 11/26/2018
ms.author: normesta
ms.openlocfilehash: 83e9dfbe18dd79e8547e6b48daef39a5aed2cced
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52975287"
---
# <a name="known-issues-with-azure-data-lake-storage-gen2"></a>Azure Data Lake depolama Gen2 ile'ilgili bilinen sorunlar

Bu makale, Azure Data Lake depolama Gen2 geçici ilişkin sınırlamalar ve bilinen sorunları içerir.

## <a name="api-interoperability"></a>API birlikte çalışabilirlik

BLOB Depolama API'leri ve Azure Data Lake Gen2 API'lerini birbirleri ile birlikte çalışabilir değil.

Tüm hesabınıza yüklediğiniz içerik çalışmak için aynı aracı kullanmanız gerekiyorsa, bu API'leri birbirleri ile birlikte çalışabilen duruma kadar Blob Depolama hesabınızda hiyerarşik ad alanları etkinleştirmeyin. Hiyerarşik ad alanı olmayan bir depolama hesabı kullanarak erişim denetimi listeleri ardından dizin ve dosya sistemi gibi Data Lake depolama Gen2 belirli özelliklere erişiminiz yoksa anlamına gelir.

## <a name="blob-storage-apis"></a>BLOB Depolama API'leri

BLOB Depolama API'leri henüz Azure Data Lake depolama Gen 2 hesaplarına kullanıma sunulmaz.

Bu API'ler Blob Depolama API'leri henüz Azure Data Lake Gen2 API'leri ile birlikte çalışabilen olmadığından oluşabilecek yanlışlıkla veri erişim sorunları önlemek için devre dışı bırakılır.

Yönetilmeyen sanal makine (VM) disk üzerinde bu API'leri bağlıdır, hiyerarşik ad alanları bir depolama hesabı üzerinde etkinleştirmek istiyorsanız, bu nedenle yönetilmeyen VM disklerini hiyerarşik ad alanları etkin olmayan bir depolama hesabına yerleştirmeyi düşünün.

## <a name="azure-storage-explorer"></a>Azure Depolama Gezgini

Depolama Gezgini'nde bazı özellikler henüz Azure Data Lake depolama Gen2'ye dosya sistemleri ile çalışmaz. Bu kısıtlamalar için her ikisinin de geçerli [tek başına sürüm](https://azure.microsoft.com/features/storage-explorer/) Azure Depolama Gezgini ek olarak, Azure portalında görünen sürüm.

## <a name="blob-viewing-tool"></a>BLOB görüntüleme aracı

Azure portalında görüntüleme aracıyla blob yalnızca Azure Data Lake depolama Gen2 desteği sınırlıdır.

## <a name="third-party-applications"></a>Üçüncü taraf uygulamalar

Üçüncü taraf uygulamaları, Azure Data Lake depolama Gen2 desteklemiyor olabilir.

Destek her üçüncü taraf uygulama sağlayıcısının takdirinizdedir. Şu anda, Blob Depolama API'leri ve aynı içeriği yönetmek için Azure Data Lake depolama Gen2 API'lerini kullanılamaz. Bu birlikte çalışabilirliği sağlamak için çalışıyoruz gibi çok sayıda üçüncü taraf araçları otomatik olarak Azure Data Lake depolama Gen2 destekleyecek mümkündür.

## <a name="azcopy-support"></a>AzCopy desteği

Sürüm 8 AzCopy, Azure Data Lake depolama Gen2 desteklemiyor.

Bunun yerine, en güncel AzCopy önizleme sürümünü kullanın ( [AzCopy v10](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10?toc=%2fazure%2fstorage%2ftables%2ftoc.json) ) olarak Azure Data Lake depolama Gen2 uç noktalarını destekler.

## <a name="oauth-authentication"></a>OAuth kimlik doğrulaması

Azure Databricks, HDInsight ve Azure Data Factory gibi hizmetleri, henüz Azure Active Directory (Azure AD) OAuth taşıyıcı belirteci kimlik doğrulaması ile tümleştirme yoktur.

## <a name="access-control-lists-acl"></a>Erişim denetim listeleri (ACL)

Dizin ve dosya düzeyinde erişim denetim listeleri (ACL) yönetmek zordur. Almak ve bu erişim denetim listeleri ayarlamak için kullanabileceğiniz UI tabanlı aracı yoktur.

## <a name="azure-event-grid"></a>Azure Event Grid

[Azure Event Grid](https://azure.microsoft.com/services/event-grid/) hesaplar bunları henüz üretme çünkü olayları Azure Data Lake Gen2 hesaplarından almaz.  

## <a name="role-based-access-control"></a>Rol tabanlı erişim denetimi

Rol tabanlı erişim denetimi, bir Azure Data Lake depolama Gen2 hesaptaki dosya sistemi nesnelere uygulayamazsınız.

## <a name="sql-data-warehouse-polybase"></a>SQL veri ambarı PolyBase

Bir Azure depolama hesabı, SQL veri ambarı depolama güvenlik duvarlarını etkinleştirildiğinde [Polybase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide?view=sql-server-2017) hesaplar erişemez.

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
