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
ms.openlocfilehash: 0182df40a4e7815560a85e60fe9062ccd8001c18
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52978707"
---
> [!NOTE]
> - Azure AD kimlik doğrulamasını BLOB'lar ve Kuyruklar önizlemesi yalnızca üretim dışı kullanması için tasarlanmıştır. Üretim hizmet düzeyi sözleşmeleri (SLA'lar) şu anda kullanılamıyor. Azure AD kimlik doğrulaması senaryonuz için henüz desteklenmiyor, uygulamalarınızda paylaşılan anahtar yetkilendirme veya SAS belirteçlerini kullanmaya devam.
>
> - Önizleme sırasında RBAC rolü atamalarını yayılması için beş dakika sürebilir.
>
> - Bir OAuth belirteci ile BLOB ve kuyruk işlemlerini yetkilendirmek için HTTPS kullanmalıdır.
>
> - Azure portalı, artık Azure AD kimlik bilgilerini kullanarak okuma ve yazma blob destekler ve sıra veri önizlemesinin bir parçası bırakın.
> 
> - [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/) şu anda depolama hesabı anahtarınızı blob ve kuyruk verilerine erişmek için kullanır. OAuth erişim, BLOB'ları için desteklenir.
>
> - Azure dosyaları SMB üzerinden Azure AD ile kimlik doğrulaması etki alanına katılmış sanal makineleri için yalnızca (Önizleme) destekler. Azure dosyaları için SMB üzerinden Azure AD kullanma hakkında bilgi edinmek için [genel bakış, Azure Active Directory kimlik doğrulaması SMB üzerinden Azure dosyaları (Önizleme)](../articles/storage/files/storage-files-active-directory-overview.md).