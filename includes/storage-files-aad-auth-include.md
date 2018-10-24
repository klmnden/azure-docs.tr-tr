---
title: include dosyası
description: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/22/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: b7682b83ca415ebff2be1c9dbb5581e8da1bb7fb
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2018
ms.locfileid: "49805029"
---
[Azure dosyaları](../articles/storage/files/storage-files-introduction.md) kimlik tabanlı kimlik doğrulaması üzerinden SMB üzerinden (sunucu ileti bloğu) (Önizleme) destekler. [Azure Active Directory (Azure AD) etki alanı Hizmetleri](../articles/active-directory-domain-services/active-directory-ds-overview.md). Etki alanına katılmış Windows sanal makinelerinizi (VM) kullanarak Azure dosya paylaşımlarını erişip [Azure AD'ye](../articles/active-directory/fundamentals/active-directory-whatis.md) kimlik bilgileri. 

Azure AD kimlik doğrulaması gibi bir kullanıcı, Grup veya hizmet sorumlusu ile kimlik [rol tabanlı erişim denetimi (RBAC)](../articles/role-based-access-control/overview.md). Yaygın Azure dosyalarına erişmek için kullanılan izin kümelerini kapsayacak özel RBAC rollerini tanımlayabilirsiniz. Ne zaman kimlik bu izinlere göre bir Azure dosya paylaşımına erişim izni verilir bir Azure AD kimlik için özel bir RBAC rolü atayın.

Önizlemenin bir parçası olarak, Azure dosyaları ayrıca koruma, devralma ve zorlama destekler [NTFS DACL](https://technet.microsoft.com/library/2006.01.howitworksntfs.aspx) tüm dosyaları ve dizinleri bir dosya paylaşımındaki. Bir dosya paylaşımından Azure dosyaları'na veri kopyalama ya da tam tersi belirtebilirsiniz NTFS DACL sürdürülür. Bu şekilde yedekleme senaryoları kullanarak Azure dosyaları, şirket içi dosya paylaşımınızı ve bulut dosya paylaşımınızı arasında NTFS DACL koruma uygulayabilirsiniz. 

> [!NOTE]
> - SMB üzerinden Azure AD kimlik doğrulaması için Linux Vm'lerini Önizleme sürümü için desteklenmiyor. Yalnızca Windows Server sanal makineleri desteklenir.
> - SMB üzerinden Azure AD kimlik doğrulaması, şirket içi makineleri Azure dosyalara erişmek için desteklenmiyor.
> - Azure AD kimlik doğrulaması, yalnızca, 24 Eylül 2018'den sonra oluşturulan depolama hesapları için kullanılabilir.
