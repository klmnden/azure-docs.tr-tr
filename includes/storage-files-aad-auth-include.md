---
title: include dosyası
description: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 06/18/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: ff2ed5abbf2ce67a0b96a5da450b83832403db1f
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67269374"
---
[Azure dosyaları](../articles/storage/files/storage-files-introduction.md) kimlik tabanlı kimlik doğrulaması üzerinden SMB üzerinden (sunucu ileti bloğu) (Önizleme) destekler. [Azure Active Directory (Azure AD) etki alanı Hizmetleri](../articles/active-directory-domain-services/overview.md). Etki alanına katılmış Windows sanal makinelerinizi (VM) kullanarak Azure dosya paylaşımlarını erişip [Azure AD'ye](../articles/active-directory/fundamentals/active-directory-whatis.md) kimlik bilgileri. 

Azure dosya paylaşımı düzeyinde erişimi ile Azure AD grubundaki bir kullanıcı gibi kimlik yönetebileceğiniz [rol tabanlı erişim denetimi (RBAC)](../articles/role-based-access-control/overview.md). Yaygın Azure dosyalarına erişmek için kullanılan izin kümelerini kapsayacak özel RBAC rollerini tanımlayabilirsiniz. Ne zaman kimlik bu izinlere göre bir Azure dosya paylaşımına erişim izni verilir bir Azure AD kimlik için özel bir RBAC rolü atayın.

Önizlemenin bir parçası olarak, Azure dosyaları ayrıca koruma, devralma ve zorlama destekler [NTFS DACL](https://technet.microsoft.com/library/2006.01.howitworksntfs.aspx) tüm dosyaları ve dizinleri bir dosya paylaşımındaki. Bir dosya paylaşımından Azure dosyaları'na veri kopyalama ya da tam tersi belirtebilirsiniz NTFS DACL sürdürülür. Bu şekilde yedekleme senaryoları kullanarak Azure dosyaları, şirket içi dosya paylaşımınızı ve bulut dosya paylaşımınızı arasında NTFS DACL koruma uygulayabilirsiniz. 

> [!NOTE]
> - Azure AD etki alanı hizmeti kimlik doğrulaması SMB erişimi için Linux Vm'leri için desteklenmiyor. Yalnızca Windows sanal makineleri desteklenir.
> - SMB erişimi için Azure AD etki alanı hizmeti kimlik doğrulaması için Active Directory etki alanına katılmış makinedeki desteklenmiyor. Bu arada, yararlanarak düşünebilirsiniz [Azure dosya eşitleme](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning) Azure dosyaları'na veri geçişini Başlat ve AD kimlik bilgileri gerektirip, şirket içinden kullanarak erişim denetimini zorunlu tutmak devam etmek için AD etki alanına katılmış, makineleri. 
> - Azure AD etki alanı hizmeti kimlik doğrulaması için SMB erişimini yalnızca 24 Eylül 2018'den sonra oluşturulan depolama hesapları için kullanılabilir.
> - Azure dosya paylaşımlarını Azure dosya eşitleme hizmeti tarafından yönetilen Azure AD etki alanı hizmeti kimlik doğrulama için SMB erişimini ve NTFS DACL kalıcı desteklenmiyor. 
