---
title: include dosyası
description: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 02/19/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 9402c4b24c9d64b4b69d750fbd19de40cda396f3
ms.sourcegitcommit: c712cb5c80bed4b5801be214788770b66bf7a009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57218099"
---
Azure depolama verilerine erişim için aşağıdaki yerleşik RBAC rolleri sağlar:

- [Depolama Blob verileri sahibi (Önizleme)](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-owner-preview): Sahipliği ayarlamak ve POSIX erişim denetimi için Azure Data Lake depolama Gen2 yönetmek için kullanın (Önizleme). Daha fazla bilgi için [Azure Data Lake depolama Gen2'deki erişim denetimi](../articles/storage/blobs/data-lake-storage-access-control.md).
- [Depolama Blob verileri katkıda bulunan (Önizleme)](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-contributor-preview): Blob depolama kaynaklarını okuma/yazma/silme izni vermek için kullanın.
- [Depolama Blob verileri Okuyucu (Önizleme)](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-reader-preview): Blob depolama kaynaklarını salt okunur izinler vermek için kullanın.
- [Depolama kuyruk verileri katkıda bulunan (Önizleme)](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-contributor-preview): Azure Kuyrukları için okuma/yazma/silme izinleri vermek için bu seçeneği kullanın.
- [Depolama kuyruk verileri Okuyucu (Önizleme)](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-reader-preview): Azure kuyrukları salt okunur izinler vermek için kullanın.

Hakkında daha fazla bilgi için Azure depolama için yerleşik roller tanımlanır, bkz: [rol tanımlarını anlamak](../articles/role-based-access-control/role-definitions.md#management-and-data-operations-preview). Özel bir RBAC rollerini oluşturma hakkında daha fazla bilgi için bkz: [Azure rol tabanlı erişim denetimi için özel roller oluşturma](../articles/role-based-access-control/custom-roles.md). 
