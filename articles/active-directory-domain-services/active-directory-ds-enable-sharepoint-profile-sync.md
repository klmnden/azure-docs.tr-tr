---
title: 'Azure Active Directory etki alanı Hizmetleri: SharePoint kullanıcı profili hizmeti için desteği etkinleştirme | Microsoft Docs'
description: SharePoint Server Profil eşitleme desteklemek için Azure Active Directory etki alanı Hizmetleri yönetilen etki alanlarını yapılandırın
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory
ms.component: domains
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: 936d760b05fcf48586739bd7538cac3dae2b6b77
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34587734"
---
# <a name="configure-a-managed-domain-to-support-profile-synchronization-for-sharepoint-server"></a>SharePoint Server Profil eşitleme desteklemek için yönetilen bir etki alanını yapılandırın
SharePoint Server kullanıcı profili eşitleme için kullanılan bir kullanıcı profili hizmeti içerir. Kullanıcı profili hizmetini kurma, uygun izinleri bir Active Directory etki alanında verilmiş olması gerekir. Daha fazla bilgi için bkz: [SharePoint Server 2013'te profil eşitleme için Active Directory etki alanı Hizmetleri izinleri](https://technet.microsoft.com/library/hh296982.aspx).

Bu makalede SharePoint Server kullanıcı profili eşitleme hizmeti dağıtmak için kullanılan Azure AD etki alanı Hizmetleri yönetilen etki alanları nasıl yapılandırabileceğiniz açıklanmaktadır.

## <a name="the-aad-dc-service-accounts-group"></a>'AAD DC hizmet hesapları' grubu
Bir güvenlik grubu olarak adlandırılan '**AAD DC hizmet hesaplarını**' 'Kullanıcılar' kuruluş birimi, yönetilen etki alanınızda içinde kullanılabilir. Bu gruptaki görebilirsiniz **Active Directory Kullanıcıları ve Bilgisayarları** MMC ek bileşenini, yönetilen etki alanınızda.

![AAD DC hizmet hesaplarını güvenlik grubu](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts.png)

Bu güvenlik grubunun üyesi bir temsilci aşağıdaki ayrıcalıklara:
- Kök DSE yönetilen etki alanının 'Dizin değişiklikleri Çoğalt' ayrıcalığı.
- Yapılandırma adlandırma bağlamında 'Dizin değişiklikleri Çoğalt' ayrıcalığı (cn = configuration kapsayıcısı) yönetilen etki alanı.

Bu güvenlik grubunu ayrıca yerleşik grubunun üyesi olan **Pre-Windows 2000 Compatible Access**.

![AAD DC hizmet hesaplarını güvenlik grubu](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-properties.png)


## <a name="enable-your-managed-domain-to-support-sharepoint-server-user-profile-sync"></a>SharePoint Server kullanıcı profili eşitleme desteklemek, yönetilen etki alanınızın
SharePoint kullanıcı profili eşitleme için kullanılan hizmet hesabı ekleyebilirsiniz **AAD DC hizmet hesaplarını** grubu. Sonuç olarak, eşitleme hesabı dizinine değişiklikleri çoğaltmak için yeterli ayrıcalıkları alır. Bu yapılandırma adımı düzgün çalışması SharePoint Server kullanıcı profili eşitleme sağlar.

![AAD DC hizmet hesapları - üyeleri Ekle](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-add-member.png)

![AAD DC hizmet hesapları - üyeleri Ekle](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-add-member2.png)

## <a name="related-content"></a>İlgili İçerik
* [Teknik başvuru - SharePoint Server 2013'te profil eşitleme izinlerini verin Active Directory etki alanı Hizmetleri](https://technet.microsoft.com/library/hh296982.aspx)
