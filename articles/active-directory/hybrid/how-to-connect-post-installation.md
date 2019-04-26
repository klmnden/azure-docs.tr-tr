---
title: "Azure AD Connect: Sonraki adımlar ve Azure AD Connect'i yönetme | Microsoft Docs"
description: Azure AD Connect için işletimsel görevler ve varsayılan yapılandırmayı genişletmeyi öğrenin.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: curtand
ms.assetid: c18bee36-aebf-4281-b8fc-3fe14116f1a5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/12/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 291b3d506993cfea89be072684835c0d4efe75f6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60243120"
---
# <a name="next-steps-and-how-to-manage-azure-ad-connect"></a>Sonraki adımlar ve Azure AD Connect'i yönetme
İşletimsel yordamları bu makalede, Azure Active Directory (Azure AD) Connect kuruluşunuzun ihtiyaçları ve gereksinimleri karşılayacak şekilde özelleştirmek için kullanın.  

## <a name="add-additional-sync-admins"></a>Ek eşitleme yöneticileri ekleyin
Varsayılan olarak, yükleme ve yerel yönetici kullanıcı yüklü eşitleme altyapısı yönetebilir. Erişmek ve eşitleme altyapısını yönetmek ek kişiler için yerel sunucuda ADSyncAdmins adlı grubunu bulun ve onları bu gruba ekleyin.

## <a name="assign-licenses-to-azure-ad-premium-and-enterprise-mobility-suite-users"></a>Azure AD Premium ve Enterprise Mobility Suite kullanıcılara lisans atama
Kullanıcılarınız için bulut eşitlendikten sonra kullanıcılar Office 365 gibi bulut uygulamaları ile kullanmaya başlayabilirsiniz şekilde bunları bir lisans atamanız gerekir.

### <a name="to-assign-an-azure-ad-premium-or-enterprise-mobility-suite-license"></a>Bir Azure AD Premium veya Enterprise Mobility Suite lisansı atamak için

1. Azure portalında bir yönetici olarak oturum açın
2. Sol taraftaki **Active Directory** öğesini seçin.
3. Üzerinde **Active Directory** sayfasında, ayarlamak istediğiniz kullanıcıları içeren dizine çift tıklayın.
4. Dizin sayfasının en üst kısmındaki **Lisanslar** öğesini seçin.
5. Üzerinde **lisansları** sayfasında **Active Directory Premium** veya **Enterprise Mobility Suite**ve ardından **atama**.
6. İletişim kutusunda, lisans atamak istediğiniz kullanıcıları seçin ve ardından değişiklikleri kaydetmek için onay işareti simgesine tıklayın.

## <a name="verify-the-scheduled-synchronization-task"></a>Zamanlanan eşitleme görevi doğrulayın
Bir eşitleme durumunu denetlemek için Azure portalını kullanın.

### <a name="to-verify-the-scheduled-synchronization-task"></a>Zamanlanan eşitleme görevi doğrulamak için
1. Azure portalında bir yönetici olarak oturum açın
2. Sol taraftaki **Active Directory** öğesini seçin.
3. Üzerinde **Active Directory** sayfasında, ayarlamak istediğiniz kullanıcıları içeren dizine çift tıklayın.
4. Dizin sayfasının en üstünde seçin **dizin tümleştirme**.
5. Altında **yerel active directory ile tümleştirme**, son eşitleme zamanı unutmayın.

<center>

![Dizin eşitleme zamanı](./media/how-to-connect-post-installation/verify.png)</center>

## <a name="start-a-scheduled-synchronization-task"></a>Zamanlanan eşitleme görevi Başlat
Bir eşitleme görevi çalıştırmanız gerekiyorsa, Azure AD Connect Sihirbazı yeniden çalıştırarak bunu yapabilirsiniz.  Azure AD kimlik bilgilerinizi sağlamanız gerekir.  Sihirbazda **eşitleme seçeneklerini özelleştirme** görev ve tıklayın **sonraki** sihirbazda taşımak için. Sonunda, emin **ilk yapılandırma tamamlandıktan hemen sonra eşitleme işlemini başlatmak** kutusu seçilidir.

<center>

![Eşitlemeyi başlatma](./media/how-to-connect-post-installation/startsynch.png)</center>

Azure AD Connect Eşitleme Zamanlayıcısı hakkında daha fazla bilgi için bkz. [Azure AD Connect Zamanlayıcı](how-to-connect-sync-feature-scheduler.md).

## <a name="additional-tasks-available-in-azure-ad-connect"></a>Azure AD Connect kullanılabilir ek görevler
Azure AD Connect, ilk yüklemeden sonra size her zaman yeniden Azure AD Connect başlangıç sayfası ya da Masaüstü kısayoldan başlatabilirsiniz.  Sihirbazda yeniden giderek ek görevler şeklinde bazı yeni seçenekler sağlar fark edeceksiniz.  

Aşağıdaki tabloda, bu görevlerin bir özeti bulunur ve her bir görevin kısa bir açıklamasını sağlar.

![Ek görevler listesi](./media/how-to-connect-post-installation/addtasks.png)

| Ek görev | Açıklama |
| --- | --- |
| **Seçilen senaryo görüntüleyin** |Geçerli Azure AD Connect çözümünüzü görüntüleyin.  Bu eşitlenmiş genel ayarlar içeren dizinler ve eşitleme ayarları. |
| **Eşitleme seçeneklerini özelleştirme** |Ek Active Directory ormanı yapılandırmasına ekleme veya eşitleme seçeneklerini gibi kullanıcı, Grup, cihaz veya parola geri yazma özelliğini etkinleştirme gibi geçerli yapılandırmasını değiştirin. |
| **Hazırlama modunu etkinleştirme** |Hemen eşitlenmez ve Azure AD'ye aktarılan değil veya şirket içi Active Directory'nin aşama bilgileri.  Bu özellik ile ortaya çıkmadan önce eşitlemeleri önizlemesini görebilirsiniz. |

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinin [şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md).
