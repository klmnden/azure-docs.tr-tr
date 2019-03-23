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
ms.openlocfilehash: 50217829be7aff3bcdbf417f19fff9f1fd7c6901
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58372683"
---
> [!NOTE]
> - RBAC rolü atamalarını yaymak için iki dakika sürebilir.
>
> Bir OAuth belirteci ile BLOB ve kuyruk işlemlerini yetkilendirmek için HTTPS kullanmalıdır.
>
> - Azure portalı, artık Azure AD kimlik bilgilerini kullanarak okuma ve yazma blob destekler ve sıra veri önizlemesinin bir parçası bırakın. Azure portalında BLOB ve kuyruk verilere erişmek için Azure Resource Manager bir kullanıcı atanmalıdır **okuyucu** rol, blob veya kuyruk erişim için uygun rolü yanı sıra. Daha fazla bilgi için [Azure kapsayıcıları ve Azure portalında RBAC ile kuyruk için erişim verin](../articles/storage/common/storage-auth-aad-rbac.md). 
> 
> - [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/) şu anda depolama hesabı anahtarınızı blob ve kuyruk verilerine erişmek için kullanır. Anahtar mevcut değilse, Azure AD yetkilendirme bloblara erişim için kullanılır. Azure AD yetkilendirme kuyruklar için şu anda desteklenmiyor. 
>
> - Azure dosyaları SMB üzerinden Azure AD ile kimlik doğrulaması etki alanına katılmış sanal makineleri için yalnızca (Önizleme) destekler. Azure dosyaları için SMB üzerinden Azure AD kullanma hakkında bilgi edinmek için [genel bakış, Azure Active Directory kimlik doğrulaması SMB üzerinden Azure dosyaları (Önizleme)](../articles/storage/files/storage-files-active-directory-overview.md).