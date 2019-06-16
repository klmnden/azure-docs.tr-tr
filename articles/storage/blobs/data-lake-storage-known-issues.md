---
title: Azure Data Lake depolama Gen2 ile'ilgili bilinen sorunlar | Microsoft Docs
description: Azure Data Lake depolama Gen2 ile'ilgili bilinen sorunlar ve sınırlamalar hakkında bilgi edinin
services: storage
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 04/26/2019
ms.author: normesta
ms.openlocfilehash: 446b49cbf3fdf3d4cde37b2a7c4ac2d9f0a811b1
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67061341"
---
# <a name="known-issues-with-azure-data-lake-storage-gen2"></a>Azure Data Lake depolama Gen2 ile'ilgili bilinen sorunlar

Bu makalede, henüz desteklenen veya hiyerarşik ad alanı (Azure Data Lake depolama Gen2) sahip olan depolama hesapları ile yalnızca kısmen desteklenen araçları ve özellikleri listelenmektedir.

<a id="blob-apis-disabled" />

## <a name="blob-storage-apis"></a>BLOB Depolama API'leri

Blob depolama API'leri, Blob Depolama API'leri henüz Azure Data Lake Gen2 API'leri ile birlikte çalışabilen olmadığından oluşabilecek özellik çalışabilirlik sorunları önlemek için devre dışıdır.

### <a name="what-to-do-with-existing-tools-applications-and-services"></a>Mevcut araçları, uygulamalar ve hizmetler ile yapmanız gerekenler

Bu kullanım Blob API'leri ve bunları tüm hesabınıza yüklediğiniz içerik çalışmak için kullanmak istiyorsanız, bir hiyerarşik ad alanı Blob Depolama hesabınızda Blob API'leri, Azure Data Lake Gen2 API'leri ile birlikte çalışabilir duruma kadar etkinleştirmeyin.

Hiyerarşik ad alanı olmayan bir depolama hesabı kullanarak, dizin ve dosya sistemi erişim denetim listeleri gibi Data Lake depolama Gen2'ye özgü özelliklere erişim ardından gerekmediği anlamına gelir.

### <a name="what-to-do-with-unmanaged-virtual-machine-vm-disks"></a>Yönetilmeyen sanal makine (VM) disklerle yapmanız gerekenler

Bu, devre dışı Blob Depolama API'leri bağlıdır, hiyerarşik bir ad alanı bir depolama hesabı üzerinde etkinleştirmek istiyorsanız, bu nedenle bunları hiyerarşik ad alanı özelliğinin etkin olmayan bir depolama hesabına yerleştirmeyi düşünün.

### <a name="what-to-do-if-you-used-blob-apis-to-load-data-before-blob-apis-were-disabled"></a>Verileri Blob API'leri önce yüklemek için Blob API'leri kullandıysanız yapmanız gerekenler devre dışı bırakıldı

Önce devre dışı bırakıldı ve bu verilere erişmek için bir üretim ihtiyacı olan verileri yüklemek için bu API'leri kullandıysanız, lütfen Microsoft Support aşağıdaki bilgilerle başvurun:

> [!div class="checklist"]
> * Abonelik kimliği (GUID, adı değil).
> * Depolama hesabı adları.
> * Üretimde etkin bir şekilde etkilendiğini işlenmeyeceğini ve işlenecekse hangi depolama hesapları için?
> * Bu verileri başka bir depolama hesabına herhangi bir nedenden dolayı kopyalanması gerekip gerekmediğini ve bu durumda, üretimde etkin bir şekilde etkilenmez olsa bile, bize neden?

Böylece bu verileri hiyerarşik ad alanı özelliğinin etkin olmayan bir depolama hesabına kopyalayabilirsiniz Bu koşullar altında biz erişim Blob API için sınırlı bir süre için geri yükleyebilirsiniz.

## <a name="all-other-features-and-tools"></a>Diğer tüm özellikler ve Araçlar

Aşağıdaki tablo, diğer tüm özellikler ve henüz desteklenen veya hiyerarşik ad alanı (Azure Data Lake depolama Gen2) sahip olan depolama hesapları ile yalnızca kısmen desteklenen araçları listeler.

| Özellik / aracı    | Daha fazla bilgi    |
|--------|-----------|
| **Data Lake depolama Gen2'ye depolama hesapları için API'leri** | Kısmen destekleniyor <br><br>Data Lake depolama Gen2 kullanabileceğiniz **REST** API'leri, ancak .NET, Java, Python SDK'ları gibi diğer Blob SDK API'leri henüz mevcut değildir.|
| **AzCopy** | Sürüme özgü desteği <br><br>Yalnızca en güncel AzCopy sürümünü kullanın ([AzCopy v10](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10?toc=%2fazure%2fstorage%2ftables%2ftoc.json)). AzCopy v8.1 gibi AzCopy'nın önceki sürümlerinde desteklenmez.|
| **Azure Blob Depolama yaşam döngüsü yönetimi ilkeleri** | Henüz desteklenmiyor |
| **Azure Content Delivery Network'te (CDN)** | Henüz desteklenmiyor|
| **Azure Event Grid** | Henüz desteklenmiyor |
| **Azure arama** |Henüz desteklenmiyor|
| **Azure Depolama Gezgini** | Sürüme özgü desteği <br><br>Yalnızca sürümünü kullanın `1.6.0` veya üzeri. <br>Sürüm `1.6.0` olarak kullanılabilir bir [ücretsiz](https://azure.microsoft.com/features/storage-explorer/).|
| **BLOB kapsayıcısı ACL'leri** |Henüz desteklenmiyor|
| **Blobfuse** |Henüz desteklenmiyor|
| **Özel etki alanları** |Henüz desteklenmiyor|
| **Tanılama günlükleri** |Henüz desteklenmiyor|
| **Dosya sistemi gezgininin** | Sınırlı destek |
| **Sabit depolama** |Henüz desteklenmiyor <br><br>Sabit depolama verileri depolama olanağı sağlar bir [(yazma birçok okuma bir kez) SOLUCAN](https://docs.microsoft.com/azure/storage/blobs/storage-blob-immutable-storage) durumu.|
| **Nesne düzeyinde katmanları** |Henüz desteklenmiyor <br><br>Örneğin: Premium, sık erişimli, soğuk ve Arşiv katmanları.|
| **PowerShell ve CLI desteği** | Sınırlı işlevsellik <br><br>Powershell veya CLI kullanarak bir hesap oluşturabilirsiniz. İşlemleri veya dosya sistemleri, dizinler ve dosyalar üzerinde erişim denetim listeleri ayarlayın.|
| **Statik Web siteleri** |Henüz desteklenmiyor <br><br>Özellikle, dosyaları sunabilme özelliğini [statik Web sitelerine](https://docs.microsoft.com/azure/storage/blobs/storage-blob-static-website).|
| **Üçüncü taraf uygulamalar** | Sınırlı destek <br><br>İş için REST API'lerini kullanma üçüncü taraf uygulamaları ile Data Lake depolama Gen2 kullanmanız durumunda çalışmaya devam eder. <br>Blob API'leri kullanan bir uygulamanız varsa, bu uygulama ile Data Lake depolama Gen2 kullanırsanız uygulama sorunlarını büyük olasılıkla sahip. Daha fazla bilgi için bkz. [Blob Depolama, Data Lake depolama Gen2'ye depolama hesapları için API devre dışı](#blob-apis-disabled) bu makalenin.|
| **Sürüm oluşturma özellikleri** |Henüz desteklenmiyor <br><br>Bu içerir [anlık görüntüleri](https://docs.microsoft.com/rest/api/storageservices/creating-a-snapshot-of-a-blob) ve [geçici silme](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete).|
|

