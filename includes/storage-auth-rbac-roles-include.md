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
ms.openlocfilehash: 9b8418dba12748915666c6a91ee65b37c0f59ace
ms.sourcegitcommit: 3341598aebf02bf45a2393c06b136f8627c2a7b8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58808009"
---
Azure depolama verilerine erişim için aşağıdaki yerleşik RBAC rolleri sağlar:

- [Depolama Blob verileri sahibi](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-owner): Sahipliği ayarlamak ve POSIX erişim denetimi için Azure Data Lake depolama Gen2 yönetmek için kullanın (Önizleme). Daha fazla bilgi için [Azure Data Lake depolama Gen2'deki erişim denetimi](../articles/storage/blobs/data-lake-storage-access-control.md).
- [Depolama Blob verileri katkıda bulunan](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-contributor): Blob depolama kaynaklarını okuma/yazma/silme izni vermek için kullanın.
- [Depolama Blob verileri okuyucu](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-reader): Blob depolama kaynaklarını salt okunur izinler vermek için kullanın.
- [Depolama kuyruk verileri katkıda bulunan](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-contributor): Azure Kuyrukları için okuma/yazma/silme izinleri vermek için bu seçeneği kullanın.
- [Depolama kuyruk verileri okuyucu](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-reader): Azure kuyrukları salt okunur izinler vermek için kullanın.
- [Depolama kuyruk verileri ileti İşlemci](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-message-processor): Grant gözlem, alma ve silme izinlerine Azure depolama kuyruklarına iletileri kullanın.
- [Depolama kuyruk verileri ileti gönderen](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-message-sender): Vermek Azure depolama kuyruklarına iletilerinde izinleri ekleyin.

> [!IMPORTANT]
> RBAC rolü atamalarını yayılması için beş dakika sürebilir.

Hakkında daha fazla bilgi için Azure depolama için yerleşik roller tanımlanır, bkz: [rol tanımlarını anlamak](../articles/role-based-access-control/role-definitions.md#management-and-data-operations-preview). Özel bir RBAC rollerini oluşturma hakkında daha fazla bilgi için bkz: [Azure rol tabanlı erişim denetimi için özel roller oluşturma](../articles/role-based-access-control/custom-roles.md). 
