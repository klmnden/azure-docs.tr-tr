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
ms.date: 04/26/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: c204029557a73dc3f02015afb92c0fdbf0d4d50e
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64571339"
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
3. Sol tarafta, seçin **Azure AD Connect**
4. Sayfanın üst kısmında, son eşitlemeden unutmayın.

![Dizin eşitleme zamanı](./media/how-to-connect-post-installation/verify2.png)

## <a name="start-a-scheduled-synchronization-task"></a>Zamanlanan eşitleme görevi Başlat
Bir eşitleme görevi çalıştırmak ihtiyacınız varsa, bunu yapabilirsiniz:

1. Sihirbazı başlatmak için Azure AD Connect masaüstü kısayolu çift tıklayın.
2. **Yapılandır**'a tıklayın.
3. Görevleri ekranında seçin **eşitleme seçeneklerini özelleştirme** tıklatıp **İleri**
4. Azure AD kimlik bilgilerinizi girin
5. **İleri**’ye tıklayın. **İleri**’ye tıklayın.  **İleri**’ye tıklayın.
5.  Üzerinde **yapılandırmaya hazır** ekranında, emin **Yapılandırma tamamlandığında eşitleme işlemini başlatmak** kutusu seçilidir.
6.  **Yapılandır**'a tıklayın.

Azure AD Connect Eşitleme Zamanlayıcısı hakkında daha fazla bilgi için bkz. [Azure AD Connect Zamanlayıcı](how-to-connect-sync-feature-scheduler.md).

## <a name="additional-tasks-available-in-azure-ad-connect"></a>Azure AD Connect kullanılabilir ek görevler
Azure AD Connect, ilk yüklemeden sonra size her zaman yeniden Azure AD Connect başlangıç sayfası ya da Masaüstü kısayoldan başlatabilirsiniz.  Sihirbazda yeniden giderek ek görevler şeklinde bazı yeni seçenekler sağlar fark edeceksiniz.  

Aşağıdaki tabloda, bu görevlerin bir özeti bulunur ve her bir görevin kısa bir açıklamasını sağlar.

![Ek görevler listesi](./media/how-to-connect-post-installation/addtasks2.png)

| Ek görev | Açıklama |
| --- | --- |
|**Gizlilik ayarları**|Hangi telemetri verilerini Microsoft ile paylaşılacağını görüntüleyin.|
|**Geçerli yapılandırmayı görüntüleme**|Geçerli Azure AD Connect çözümünüzü görüntüleyin.  Bu eşitlenmiş genel ayarlar içeren dizinler ve eşitleme ayarları. |
| **Eşitleme seçeneklerini özelleştirme** |Ek Active Directory ormanı yapılandırmasına ekleme veya eşitleme seçeneklerini gibi kullanıcı, Grup, cihaz veya parola geri yazma özelliğini etkinleştirme gibi geçerli yapılandırmasını değiştirin. |
|**Cihaz seçeneklerini yapılandır**|Eşitleme için kullanılabilir cihaz seçenekleri|
|**Dizin şemasını Yenile**|Eşitleme için yeni şirket içi dizin nesnelerini eklemenizi sağlar|
|**Hazırlama modunu yapılandırma** |Hemen eşitlenmez ve Azure AD'ye aktarılan değil veya şirket içi Active Directory'nin aşama bilgileri.  Bu özellik ile ortaya çıkmadan önce eşitlemeleri önizlemesini görebilirsiniz. |
|**Kullanıcı oturum açma**|Kullanıcılar oturum açmak için kullandığınız kimlik doğrulama yöntemini değiştirme|
|**Federasyon yönetme**|AD FS altyapınızı yönetin, sertifikaları yenileme ve AD FS sunucuları ekleyin|
|**Sorun giderme**|Azure AD Connect sorunlarını gidermeye yardım|

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinin [şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md).
