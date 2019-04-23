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
ms.openlocfilehash: 026717dff2b6883eb643497dec91226e4afe8133
ms.sourcegitcommit: c884e2b3746d4d5f0c5c1090e51d2056456a1317
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60150226"
---
Azure, Azure AD kullanarak blob ve kuyruk verilere erişim yetkisi vermek için aşağıdaki yerleşik RBAC rolleri sağlar ve OAuth:

- [Depolama Blob verileri sahibi](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-owner): Sahipliği ayarlamak ve POSIX erişim denetimi için Azure Data Lake depolama Gen2 yönetmek için kullanın (Önizleme). Daha fazla bilgi için [Azure Data Lake depolama Gen2'deki erişim denetimi](../articles/storage/blobs/data-lake-storage-access-control.md).
- [Depolama Blob verileri katkıda bulunan](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-contributor): Blob depolama kaynaklarını okuma/yazma/silme izni vermek için kullanın.
- [Depolama Blob verileri okuyucu](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-reader): Blob depolama kaynaklarını salt okunur izinler vermek için kullanın.
- [Depolama kuyruk verileri katkıda bulunan](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-contributor): Azure Kuyrukları için okuma/yazma/silme izinleri vermek için bu seçeneği kullanın.
- [Depolama kuyruk verileri okuyucu](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-reader): Azure kuyrukları salt okunur izinler vermek için kullanın.
- [Depolama kuyruk verileri ileti İşlemci](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-message-processor): Grant gözlem, alma ve silme izinlerine Azure depolama kuyruklarına iletileri kullanın.
- [Depolama kuyruk verileri ileti gönderen](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-message-sender): Vermek Azure depolama kuyruklarına iletilerinde izinleri ekleyin.

> [!NOTE]
> RBAC rolü atamalarını yayılması için beş dakika sürebilir göz önünde bulundurun.
