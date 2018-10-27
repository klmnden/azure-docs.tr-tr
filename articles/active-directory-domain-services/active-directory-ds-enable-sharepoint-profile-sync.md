---
title: 'Azure Active Directory Domain Services: SharePoint kullanıcı profili hizmeti için desteği etkinleştirme | Microsoft Docs'
description: SharePoint Server için profil eşitleme desteklemek için Azure Active Directory Domain Services yönetilen etki alanlarını yapılandırma
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: mtillman
editor: curtand
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/22/2018
ms.author: ergreenl
ms.openlocfilehash: 982984912494598e9791968e5ee3ccee52c5a7c9
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2018
ms.locfileid: "50154956"
---
# <a name="configure-a-managed-domain-to-support-profile-synchronization-for-sharepoint-server"></a>SharePoint Server için profil eşitleme desteklemek için yönetilen bir etki alanı yapılandırma
SharePoint Server, kullanıcı profili eşitleme için kullanılan kullanıcı profili hizmeti içerir. Kullanıcı profili hizmeti'kurmak için uygun izinlere bir Active Directory etki alanına sahip olmanız gerekir. Daha fazla bilgi için [Active Directory Domain Services için SharePoint Server 2013'te profil eşitleme izinleri](https://technet.microsoft.com/library/hh296982.aspx).

Bu makalede, SharePoint Server kullanıcı profili eşitleme hizmeti dağıtmak için kullanılan Azure AD Domain Services yönetilen etki alanlarını nasıl yapılandırabileceğiniz açıklanmaktadır.

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="the-aad-dc-service-accounts-group"></a>'AAD DC hizmeti hesapları' grubuna
Bir güvenlik grubu adı '**AAD DC hizmet hesapları**', yönetilen etki alanınızda 'Kullanıcılar' kuruluş birimi içerisinde kullanılabilir. Bu grupta gördüğünüz **Active Directory Kullanıcıları ve Bilgisayarları** MMC ek bileşeninde, yönetilen etki alanınızda.

![AAD DC hizmet hesapları güvenlik grubu](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts.png)

Bu güvenlik grubunun üyesi bir temsilci aşağıdaki ayrıcalıklara:
- Kök DSE yönetilen etki alanının 'Dizin değişikliklerini çoğaltma' ayrıcalığı.
- Yapılandırma adlandırma bağlamında 'Dizin değişikliklerini çoğaltma' ayrıcalığı (cn = yapılandırma kapsayıcısı), yönetilen etki alanı.

Bu güvenlik grubunu ayrıca yerleşik grubunun bir üyesi olan **Pre-Windows 2000 Compatible Access**.

![AAD DC hizmet hesapları güvenlik grubu](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-properties.png)


## <a name="enable-your-managed-domain-to-support-sharepoint-server-user-profile-sync"></a>SharePoint Server kullanıcı profili eşitleme desteklemek yönetilen etki alanınıza etkinleştir
SharePoint kullanıcı profili eşitleme için kullanılan hizmet hesabı ekleyebilirsiniz **AAD DC hizmet hesapları** grubu. Sonuç olarak, eşitleme hesabı dizine değişiklikleri çoğaltmak için yeterli ayrıcalıkları alır. Bu yapılandırma adımı düzgün çalışması SharePoint Server kullanıcı profili eşitleme sağlar.

![AAD DC hizmet hesapları - üyeleri Ekle](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-add-member.png)

![AAD DC hizmet hesapları - üyeleri Ekle](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-add-member2.png)

## <a name="related-content"></a>İlgili İçerik
* [Teknik başvuru - SharePoint Server 2013'te profil eşitleme izinleri verme Active Directory etki alanı Hizmetleri](https://technet.microsoft.com/library/hh296982.aspx)
