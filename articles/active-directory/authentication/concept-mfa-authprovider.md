---
title: Ne zaman ve nasıl bir Azure multi-Factor Auth sağlayıcısı kullanılır?
description: Ne zaman bir Auth sağlayıcısı Azure MFA ile kullanmalısınız?
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 09/01/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: michmcla
ms.openlocfilehash: b601a3d23b23faa16925881a54e2ceba85c800f8
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2018
ms.locfileid: "43669074"
---
# <a name="when-to-use-an-azure-multi-factor-authentication-provider"></a>Azure multi-Factor Authentication sağlayıcısı kullanıldığı durumlar

İki adımlı doğrulama, Azure Active Directory’ye sahip genel yöneticiler ve Office 365 kullanıcıları için varsayılan olarak kullanılabilir durumdadır. Ancak, [gelişmiş özelliklerden](howto-mfa-mfasettings.md) yararlanmak isterseniz Azure Multi-Factor Authentication’ın (MFA) tam sürümünü satın almanız gerekir.

Kullanıcılar için Azure multi-Factor Authentication tarafından sağlanan özelliklerden yararlanmak için Azure multi-Factor Auth sağlayıcısı kullanılan kimin **lisansı olmayan**. 

Ardından, kuruluşunuzdaki tüm kullanıcılar kapsayan lisanslarınız varsa, Azure multi-Factor Auth sağlayıcısı gerekmez. Yalnızca, ayrıca lisansınız yoksa kullanıcılarınız için iki aşamalı doğrulamayı sağlamanız gerekiyorsa, Azure multi-Factor Authentication sağlayıcısı oluşturun.

> [!NOTE]
> 1 Eylül 2018'e yeni etkin yetki sağlayıcılar artık oluşturulabilir. Mevcut kimlik doğrulama sağlayıcıları güncelleştirildi ve kullanılabilmesi devam edebilir. Çok faktörlü kimlik doğrulaması, Azure AD Premium lisansınız kullanılabilir bir özellik olmaya devam edecektir.

## <a name="caveats-related-to-the-azure-mfa-sdk"></a>Azure MFA SDK'sı için ilgili uyarılar

SDK’yı indirmek için Azure Multi-Factor Auth sağlayıcısı gerekir. SDK, kullanım dışı bırakıldı ve yeni müşteriler için artık desteklenmiyor ve yalnızca 14 Kasım 2018'e kadar çalışmaya devam edecektir unutmayın. O tarihten sonra SDK çağrıları başarısız olacaktır.

Azure MFA, AAD Premium veya ile birlikte gelen diğer lisans olsa bile SDK'yı indirmek için Azure multi-Factor Auth sağlayıcısı oluşturun. Bu amaçla Azure Multi-Factor Auth Sağlayıcısı oluşturursanız ve zaten lisanslarınız varsa Sağlayıcıyı **Etkin Kullanıcı Başına** modeliyle oluşturduğunuzdan emin olun. Ardından, sağlayıcıyı Azure MFA, Azure AD Premium veya ile birlikte gelen diğer lisans içeren dizine bağlayın. Bu yapılandırma, sizden yalnızca iki aşamalı doğrulama kullanan benzersiz kullanıcılarınızın sayısı sahip olduğunuz lisanslardan daha fazlaysa ücret alınmasını sağlar.

## <a name="what-is-an-mfa-provider"></a>MFA Sağlayıcısı nedir?

Azure multi-Factor Authentication için lisans yoksa, kullanıcılarınız için iki aşamalı doğrulama gerektirecek şekilde kimlik doğrulama sağlayıcısı oluşturabilirsiniz.

İki tür kimlik doğrulama sağlayıcısı vardır ve Azure aboneliğinize nasıl ücretlendirilir etrafında ayrım ilgilidir. Kimlik doğrulaması başına seçeneğinde, kiracınızda bir ay içinde gerçekleştirilen kimlik doğrulaması sayısı hesaplanır. Bu, yalnızca gereken durumlarda kimlik doğrulamasını kullanan birkaç kullanıcınızın olması halinde en iyi seçenektir. Kullanıcı başına seçeneğinde, kiracınızda bir ayda iki aşamalı doğrulama gerçekleştiren kişi sayısı hesaplanır. Bu, lisansı bulunan bazı kullanıcılarınızın olması ancak MFA'yı, lisanslama sınırlarınızı aşacak sayıda kullanıcıya genişletmeniz gereken durumlarda en iyi seçenektir.

## <a name="create-an-mfa-provider"></a>MFA Sağlayıcısı oluşturma

Azure portalında Azure Multi-Factor Authentication Sağlayıcısı oluşturmak için aşağıdaki adımları izleyin:

1. [Azure portalında](https://portal.azure.com) genel yönetici olarak oturum açın.
2. **Azure Active Directory** > **MFA Sunucusu** > **Sağlayıcılar**’ı seçin.

   ![Sağlayıcılar][Providers]

3. **Add (Ekle)** seçeneğini belirleyin.
4. Aşağıdaki alanları doldurun ve **Ekle**'yi seçin:
   - **Ad** – Sağlayıcının adı.
   - **Kullanım Modeli**: İki seçenekten birini belirleyin:
      * Kimlik Doğrulaması Başına – kimlik doğrulaması başına ücretlendirilen satın alma modeli. Genellikle tüketiciyle karşılaşan uygulamada Azure Multi-Factor Authentication kullanan senaryolar için kullanılır.
      * Etkin Kullanıcı Başına - etkin kullanıcı başına ücretlendirilen satın alma modeli. Genellikle Office 365 gibi uygulamalara çalışan erişimi için kullanılır. Azure MFA için zaten lisansı olan kullanıcılarınız varsa bu seçeneği belirleyin.
   - **Abonelik** – Sağlayıcı üzerinden gerçekleştirilen iki aşamalı doğrulama etkinliği için ücretlendirilen Azure aboneliği.
   - **Dizin** – Sağlayıcının ilişkili olduğu Azure Active Directory kiracısı.
      * Bir Sağlayıcı oluşturmak için Azure AD dizini gerekli değildir. Yalnızca Azure Multi-Factor Authentication Sunucusunu indirmeyi planlıyorsanız bu kutuyu boş bırakın.
      * Gelişmiş özelliklerden yararlanmak için Sağlayıcının bir Azure AD dizini ile ilişkili olması gerekir.
      * Herhangi bir Azure AD dizini ile yalnızca bir Sağlayıcı ilişkili olabilir.

## <a name="manage-your-mfa-provider"></a>MFA Sağlayıcınızı yönetme

Bir MFA sağlayıcısı oluşturulduktan sonra kullanım modelini (etkin kullanıcı başına veya kimlik doğrulaması başına) değiştiremezsiniz. Ancak, MFA sağlayıcısı silip daha sonra farklı bir kullanım modeliyle bir sağlayıcı oluşturabilirsiniz.

Geçerli Multi-Factor Auth Sağlayıcısı bir Azure AD dizini (aynı zamanda Azure AD kiracısı olarak bilinir) ile ilişkili ise, MFA sağlayıcısını güvenli bir şekilde silebilir ve aynı Azure AD kiracısına bağlı bir sağlayıcı oluşturabilirsiniz. Alternatif olarak, MFA için etkinleştirilen tüm kullanıcıları kapsayacak yeterli sayıda lisans satın aldıysanız, MFA sağlayıcısını tamamen silebilirsiniz.

MFA sağlayıcınız bir Azure AD kiracısına bağlı değilse veya yeni MFA sağlayıcısını farklı bir Azure AD kiracısına bağlarsanız, kullanıcı ayarları ve yapılandırma seçenekleri aktarılmaz. Ayrıca, yeni MFA Sağlayıcısı ile oluşturulan etkinleştirme kimlik bilgileri kullanılarak mevcut Azure MFA Sunucularının yeniden etkinleştirilmesi gerekir. MFA sunucularını yeni MFA sağlayıcısına bağlamak telefon aramasıyla kısa mesaj kimlik doğrulamasını etkilemez, ancak bunlar mobil uygulamayı yeniden kadar tüm kullanıcılar için mobil uygulama bildirimleri durdurun.

## <a name="next-steps"></a>Sonraki adımlar

[Multi-Factor Authentication ayarlarını yapılandırma](howto-mfa-mfasettings.md)

[Providers]: ./media/concept-mfa-authprovider/add-providers.png "MFA Sağlayıcıları Ekle"
