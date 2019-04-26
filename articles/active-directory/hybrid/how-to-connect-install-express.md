---
title: 'Azure AD Connect: Hızlı ayarlar ile çalışmaya başlama | Microsoft Docs'
description: Azure AD Connect'i indirme, yükleme ve kurulum sihirbazını çalıştırma hakkında bilgi edinin.
services: active-directory
author: billmath
manager: daveba
editor: curtand
ms.assetid: b6ce45fd-554d-4f4d-95d1-47996d561c9f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/28/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 24143f8c94a294da90be84bacfe633db0cd24f85
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60244554"
---
# <a name="getting-started-with-azure-ad-connect-using-express-settings"></a>Hızlı ayarları kullanarak Azure AD Connect ile çalışmaya başlama
Kimlik doğrulaması için [parola karması eşitleme](how-to-connect-password-hash-synchronization.md) özelliğine ve tek ormanlı bir topolojiye sahipseniz Azure AD Connect **Hızlı Ayarları** kullanılır. **Hızlı Ayarlar** varsayılan seçenek olup yaygın olarak dağıtılan senaryo için kullanılır. Şirket içi dizininizi buluta genişletmek için yalnızca birkaç tıklama yapmanız yeterli.

Azure AD Connect'i yüklemeye başlamadan önce emin olun [Azure AD Connect'i indirdiğinizden](https://go.microsoft.com/fwlink/?LinkId=615771) ve adımları tamamlayın önkoşul [Azure AD Connect: Donanım ve Önkoşullar](how-to-connect-install-prerequisites.md).

Hızlı ayarlar, topolojinizle eşleşmiyorsa diğer senaryolar için [ilgili belgelere](#related-documentation) göz atın.

## <a name="express-installation-of-azure-ad-connect"></a>Azure AD Connect'i hızlı yükleme
Bu adımların nasıl gerçekleştirildiğini [videolar](#videos) bölümünden görebilirsiniz.

1. Azure AD Connect'i yüklemek istediğiniz sunucuda yerel yönetici olarak oturum açın. Bu işlemi eşitleme sunucusu olmasını istediğiniz sunucuda yapmanız gerekir.
2. **AzureADConnect.msi** öğesine gidin ve çift tıklayın.
3. Hoş Geldiniz ekranında, lisans koşullarını kabul ettiğinizi belirten kutuyu seçin ve **Devam**'a tıklayın.  
4. Hızlı ayarlar ekranında **Hızlı ayarları kullan**'a tıklayın.  
   ![Azure AD Connect'e Hoş Geldiniz](./media/how-to-connect-install-express/express.png)
5. Azure AD'ye Bağlanma ekranında Azure AD'niz için genel yönetici kullanıcı adını ve parolasını girin. **İleri**’ye tıklayın.  
   ![Azure AD'ye Bağlanma](./media/how-to-connect-install-express/connectaad.png)  
   Bir hatayla karşılaştıysanız ve bağlantı sorunlarınız varsa bkz. [Bağlantı sorunlarını giderme](tshoot-connect-connectivity.md).
6. AD DS'ye Bağlanma ekranında kuruluş yöneticisi hesabına ilişkin kullanıcı adını ve parolayı girin. Etki alanı bölümünü NetBIOS veya FQDN biçiminde (örneğin, FABRIKAM\yönetici veya fabrikam.com\yönetici) girebilirsiniz. **İleri**’ye tıklayın.  
   ![AD DS'ye Bağlanma](./media/how-to-connect-install-express/connectad.png)
7. [**Azure AD oturum açma yapılandırması**](plan-connect-user-signin.md#azure-ad-sign-in-configuration) sayfası, yalnızca [önkoşullar](how-to-connect-install-prerequisites.md) bölümündeki [etki alanlarınızı doğrulama](../active-directory-domains-add-azure-portal.md) adımını tamamlamamış olmanız halinde görüntülenir.
   ![Doğrulanmamış etki alanları](./media/how-to-connect-install-express/unverifieddomain.png)  
   Bu sayfayı görüyorsanız **Eklenmedi** ve **Doğrulanmadı** olarak işaretlenen tüm etki alanlarını gözden geçirin. Kullandığınız etki alanlarının Azure AD'de doğrulanmış olduğundan emin olun. Etki alanlarınızı doğruladıktan sonra Yenile simgesine tıklayın.
8. Yapılandırma için hazır ekranında **Yükle**'ye tıklayın.
   * İsteğe bağlı olarak, Yapılandırma için hazır sayfasında **Start the synchronization process as soon as configuration completes (Yapılandırma tamamlanınca eşitlemeyi başlat)** onay kutusunun işaretini kaldırabilirsiniz. Başka yapılandırmalar (örneğin, [filtreleme](how-to-connect-sync-configure-filtering.md)) gerçekleştirmek istiyorsanız bu onay kutusunun işaretini kaldırmanız gerekir. Bu seçeneğin işaretini kaldırırsanız sihirbaz eşitlemeyi yapılandırır ancak zamanlayıcıyı devre dışı bırakır. [Yükleme sihirbazını yeniden çalıştırarak](how-to-connect-installation-wizard.md) elle etkinleştirmediğiniz sürece çalışmaz.
   * **Yapılandırma tamamlandıktan hemen sonra eşitleme işlemini başlat** onay kutusunu etkin bırakmak, hemen tüm kullanıcıları, grupları ve kişileri Azure AD’ye tam eşitleme işlemini tetikler.
   * Şirket içi Active Directory'nizde Exchange varsa [**Exchange Karma dağıtımını**](https://technet.microsoft.com/library/jj200581.aspx) da etkinleştirebilirsiniz. Aynı anda hem bulutta ve hem de şirket içinde Exchange posta kutunuzun olmasını istiyorsanız bu seçeneği etkinleştirin.
     ![Azure AD Connect yapılandırma için hazır](./media/how-to-connect-install-express/readytoconfigure.png)
9. Yükleme tamamlandığında **Çıkış**'a tıklayın.
10. Yükleme tamamlandıktan sonra Synchronization Service Manager'ı veya Synchronization Rule Editor'ı kullanmadan önce oturumunuzu kapatıp tekrar açın.

## <a name="videos"></a>Videolar
Hızlı yüklemeyi kullanmaya ilişkin video için bkz.

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-Active-Directory-Connect-Express-Settings/player]
>
>

## <a name="next-steps"></a>Sonraki adımlar
Azure AD Connect'i yüklediniz, artık [yüklemeyi doğrulayabilir ve lisans atayabilirsiniz](how-to-connect-post-installation.md).

Yüklemeyle etkinleştirilen özellikler hakkında daha fazla bilgi edinin: [Otomatik yükseltme](how-to-connect-install-automatic-upgrade.md), [yanlışlıkla silmeleri engelleme](how-to-connect-sync-feature-prevent-accidental-deletes.md), ve [Azure AD Connect Health](how-to-connect-health-sync.md).

Şu genel konu başlıkları hakkında daha fazla bilgi edinin: [Zamanlayıcı ve eşitleme tetikleme](how-to-connect-sync-feature-scheduler.md).

[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.

## <a name="related-documentation"></a>İlgili belgeler

| Konu | Bağlantı |
| --- | --- |
| Azure AD Connect'e genel bakış | [Şirket içi dizinlerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)
| Özelleştirilmiş ayarları kullanarak yükleme | [Azure AD Connect özel yüklemesi](how-to-connect-install-custom.md) |
| DirSync'ten yükseltme | [Azure AD eşitleme aracından (DirSync) yükseltme](how-to-dirsync-upgrade-get-started.md)|
| Yükleme için kullanılan hesaplar | [Azure AD Connect kimlik bilgileri ve izinleri hakkında daha fazla bilgi](reference-connect-accounts-permissions.md) |
