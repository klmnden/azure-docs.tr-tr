---
title: include dosyası
description: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/09/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 2d641287bcf095f13719667a91b7f61295309b5d
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2018
ms.locfileid: "49430366"
---
> [!NOTE]
> - Azure AD kimlik doğrulamasını BLOB'lar ve Kuyruklar önizlemesi yalnızca üretim dışı kullanması için tasarlanmıştır. Üretim hizmet düzeyi sözleşmeleri (SLA'lar) şu anda kullanılamıyor. Azure AD kimlik doğrulaması senaryonuz için henüz desteklenmiyor, uygulamalarınızda paylaşılan anahtar yetkilendirme veya SAS belirteçlerini kullanmaya devam.
>
> - Önizleme sırasında RBAC rolü atamalarını yayılması için beş dakika sürebilir.
>
> - Azure AD kimlik doğrulaması için HTTPS kullanmalıdır blob ve kuyruk işlemlerini çağırırken.
>
> - Azure portalı artık okuyup önizleme sürümünün bir parçası blob veri yazabilmesi için Azure AD kimlik bilgilerini kullanarak destekler.
> 
> - Azure portalı, sıranın veri okuma ve yazma için Azure AD kimlik bilgilerini kullanarak şu anda desteklemiyor. Kuyruk veri depolama hesap anahtarlarınızı erişilir.
>
> - [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/) şu anda depolama hesabı anahtarınızı blob ve kuyruk verilerine erişmek için kullanır.
>
> - Azure dosyaları SMB üzerinden Azure AD ile kimlik doğrulaması etki alanına katılmış sanal makineleri için yalnızca (Önizleme) destekler. Azure dosyaları için SMB üzerinden Azure AD kullanma hakkında bilgi edinmek için [genel bakış, Azure Active Directory kimlik doğrulaması SMB üzerinden Azure dosyaları (Önizleme)](../articles/storage/files/storage-files-active-directory-overview.md).



