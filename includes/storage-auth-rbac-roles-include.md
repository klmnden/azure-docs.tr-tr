---
title: include dosyası
description: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 03/21/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: c7fa81f6d23962eedb3dfeafdd397b62a83d130e
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58372684"
---
Azure depolama verilerine erişim için aşağıdaki yerleşik RBAC rolleri sağlar:

- [Depolama Blob verileri sahibi](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-owner-preview): Sahipliği ayarlamak ve POSIX erişim denetimi için Azure Data Lake depolama Gen2 yönetmek için kullanın (Önizleme). Daha fazla bilgi için [Azure Data Lake depolama Gen2'deki erişim denetimi](../articles/storage/blobs/data-lake-storage-access-control.md).
- [Depolama Blob verileri katkıda bulunan](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-contributor-preview): Blob depolama kaynaklarını okuma/yazma/silme izni vermek için kullanın.
- [Depolama Blob verileri okuyucu](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-reader-preview): Blob depolama kaynaklarını salt okunur izinler vermek için kullanın.
- [Depolama kuyruk verileri katkıda bulunan](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-contributor-preview): Azure Kuyrukları için okuma/yazma/silme izinleri vermek için bu seçeneği kullanın.
- [Depolama kuyruk verileri okuyucu](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-reader-preview): Azure kuyrukları salt okunur izinler vermek için kullanın.
- [Depolama kuyruk verileri ileti İşlemci](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-message-processor-preview): Grant gözlem, alma ve silme izinlerine Azure depolama kuyruklarına iletileri kullanın.
- [Depolama kuyruk verileri ileti gönderen](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-message-sender-preview): Vermek Azure depolama kuyruklarına iletilerinde izinleri ekleyin.

Hakkında daha fazla bilgi için Azure depolama için yerleşik roller tanımlanır, bkz: [rol tanımlarını anlamak](../articles/role-based-access-control/role-definitions.md#management-and-data-operations-preview). Özel bir RBAC rollerini oluşturma hakkında daha fazla bilgi için bkz: [Azure rol tabanlı erişim denetimi için özel roller oluşturma](../articles/role-based-access-control/custom-roles.md). 
