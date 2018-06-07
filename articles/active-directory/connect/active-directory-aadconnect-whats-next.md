---
title: "Azure AD Connect: Sonraki adımlar ve Azure AD Connect'i yönetme | Microsoft Docs"
description: Varsayılan yapılandırma ve işletimsel görevler için Azure AD Connect genişletmeyi öğrenin.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: curtand
ms.assetid: c18bee36-aebf-4281-b8fc-3fe14116f1a5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: f8b73e70606adc2b1fa593745b3ac426c679f417
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34592094"
---
# <a name="next-steps-and-how-to-manage-azure-ad-connect"></a>Sonraki adımlar ve Azure AD Connect'i yönetme
İşlem yordamlarını bu makalede, kuruluşunuzun ihtiyaçları ve gereksinimleri karşılamak için Azure Active Directory (Azure AD) Bağlan özelleştirmek için kullanın.  

## <a name="add-additional-sync-admins"></a>Ek eşitleme yöneticileri ekleyin
Varsayılan olarak, yükleme ve yerel Yöneticiler sürümlerdeki kullanıcı yüklü eşitleme altyapısı yönetebilirsiniz. Erişmek ve eşitleme altyapısı yönetmek ek kişiler için yerel sunucuda ADSyncAdmins adlı grubunu bulun ve bu gruba ekleyin.

## <a name="assign-licenses-to-azure-ad-premium-and-enterprise-mobility-suite-users"></a>Azure AD Premium ve Enterprise Mobility Suite kullanıcılara lisans atayın
Kullanıcılarınızın buluta eşitlenmiş olduğundan, bunlar Office 365 gibi bulut uygulamalarıyla kullanmaya başlayabilirsiniz için bunları bir lisans atamanız gerekir.

### <a name="to-assign-an-azure-ad-premium-or-enterprise-mobility-suite-license"></a>Bir Azure AD Premium veya Enterprise Mobility Suite lisansı atamak için

1. Azure portalında bir yönetici olarak oturum açın
2. Sol taraftaki **Active Directory** öğesini seçin.
3. Üzerinde **Active Directory** sayfasında, ayarlamak istediğiniz kullanıcıları içeren dizine çift tıklayın.
4. Dizin sayfasının en üst kısmındaki **Lisanslar** öğesini seçin.
5. Üzerinde **lisansları** sayfasında, **Active Directory Premium** veya **Enterprise Mobility Suite**ve ardından **atamak**.
6. İletişim kutusunda, lisans atamak istediğiniz kullanıcıları seçin ve ardından değişiklikleri kaydetmek için onay işareti simgesine tıklayın.

## <a name="verify-the-scheduled-synchronization-task"></a>Zamanlanan eşitleme görevi doğrulayın
Bir eşitleme durumunu denetlemek için Azure Portalı'nı kullanın.

### <a name="to-verify-the-scheduled-synchronization-task"></a>Zamanlanan eşitleme görevi doğrulamak için
1. Azure portalında bir yönetici olarak oturum açın
2. Sol taraftaki **Active Directory** öğesini seçin.
3. Üzerinde **Active Directory** sayfasında, ayarlamak istediğiniz kullanıcıları içeren dizine çift tıklayın.
4. Dizin sayfanın en üstünde seçin **dizin tümleştirme**.
5. Altında **yerel active directory ile tümleştirme**, son eşitleme zamanı unutmayın.

<center>![Dizin eşitleme zamanı](./media/active-directory-aadconnect-whats-next/verify.png)</center>

## <a name="start-a-scheduled-synchronization-task"></a>Zamanlanan eşitleme görevi Başlat
Eşitleme görevi çalıştırmanız gerekiyorsa, Azure AD Connect Sihirbazı yeniden çalıştırarak bunu yapabilirsiniz.  Azure AD kimlik bilgilerinizi sağlamanız gerekir.  Sihirbazı'nda seçin **eşitleme seçeneklerini özelleştirme** tıklayın ve görev **sonraki** sihirbazda taşımak için. Sonunda, emin **ilk yapılandırma tamamlandıktan hemen sonra eşitleme işlemini başlatmak** kutusu seçilidir.

<center>![Eşitlemeyi Başlat](./media/active-directory-aadconnect-whats-next/startsynch.png)</center>

Azure AD Connect eşitleme Zamanlayıcı hakkında daha fazla bilgi için bkz: [Azure AD Connect Zamanlayıcı](active-directory-aadconnectsync-feature-scheduler.md).

## <a name="additional-tasks-available-in-azure-ad-connect"></a>Azure AD Connect kullanılabilir ek görevler
Azure AD Connect ilk, yüklemeden sonra her zaman Sihirbazı'nı yeniden Azure AD Connect başlangıç sayfası veya Masaüstü kısayoldan başlatabilirsiniz.  Sihirbazı kullanarak yeniden giderek ek görevleri biçiminde yeni bazı seçenekler sunar fark edeceksiniz.  

Aşağıdaki tabloda, bu görevleri özetini ve her görevin kısa bir açıklamasını sağlar.

![Ek görevler listesi](./media/active-directory-aadconnect-whats-next/addtasks.png)

| Ek görevi | Açıklama |
| --- | --- |
| **Görünüm Seçili senaryosu** |Geçerli Azure AD Connect çözümünüzü görüntüleyin.  Bu eşitlenmiş genel ayarları içeren dizinler ve eşitleme ayarları. |
| **Eşitleme seçeneklerini özelleştirme** |Ek Active Directory ormanı yapılandırmasına ekleme ya da kullanıcı, Grup, cihaz veya parola geri yazma gibi eşitleme seçeneklerini etkinleştirme gibi geçerli yapılandırmasını değiştirin. |
| **Hazırlama modunu etkinleştir** |Hemen eşitlenmemiş ve Azure AD'ye aktarılan değil veya şirket içi Active Directory aşama bilgileri.  Bu özellik ile ortaya çıkmadan önce eşitlemeleri önizleyebilirsiniz. |

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinmek [şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md).
