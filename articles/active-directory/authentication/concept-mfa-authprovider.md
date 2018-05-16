---
title: Azure Multi-Factor Auth Sağlayıcısı’nı kullanmaya başlama | Microsoft Belgeleri
description: Azure Multi-Factor Auth Sağlayıcısı oluşturma hakkında bilgi edinin.
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: get-started-article
ms.date: 12/08/2017
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: richagi
ms.openlocfilehash: 453b8cc399c78ddb26ae601abf64626d2a6bf36f
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
---
# <a name="getting-started-with-an-azure-multi-factor-authentication-provider"></a>Azure Multi-Factor Authentication Sağlayıcısı kullanmaya başlama
İki adımlı doğrulama, Azure Active Directory’ye sahip genel yöneticiler ve Office 365 kullanıcıları için varsayılan olarak kullanılabilir durumdadır. Ancak, [gelişmiş özelliklerden](howto-mfa-mfasettings.md) yararlanmak isterseniz Azure Multi-Factor Authentication’ın (MFA) tam sürümünü satın almanız gerekir.

Azure Multi-Factor Auth Sağlayıcısı Azure MFA tam sürümünün sağladığı özelliklerden yararlanmak için kullanılır. **Azure MFA, Azure AD Premium veya Enterprise Mobility + Security (EMS) lisansı olmayan** kullanıcılara yöneliktir.  Azure MFA, Azure AD Premium ve EMS varsayılan olarak Azure MFA’nın tam sürümünü içerir. Lisanslarınız varsa bir Azure Multi-Factor Auth Sağlayıcısına ihtiyacınız yoktur.

SDK’yı indirmek için Azure Multi-Factor Auth sağlayıcısı gerekir.

> [!IMPORTANT]
> Azure Multi-Factor Authentication Yazılım Geliştirme Seti’nin (SDK) kullanımdan kaldırılacağı duyurulmuştur. Bu özellik yeni müşteriler için artık desteklenmemektedir. Mevcut müşteriler 14 Kasım 2018’e kadar SDK'yı kullanmaya devam edebilir. O tarihten sonra SDK çağrıları başarısız olacaktır.

> [!IMPORTANT]
>SDK’yı indirmek için Azure MFA, AAD Premium veya EMS lisanslarınız olsa bile bir Azure Multi-Factor Auth Sağlayıcısı oluşturmanız gerekir.  Bu amaçla Azure Multi-Factor Auth Sağlayıcısı oluşturursanız ve zaten lisanslarınız varsa Sağlayıcıyı **Etkin Kullanıcı Başına** modeliyle oluşturduğunuzdan emin olun. Ardından, Sağlayıcıyı Azure MFA, Azure AD Premium veya EMS lisansları içeren dizine bağlayın. Bu yapılandırma, sizden yalnızca iki aşamalı doğrulama kullanan benzersiz kullanıcılarınızın sayısı sahip olduğunuz lisanslardan daha fazlaysa ücret alınmasını sağlar. 

## <a name="what-is-an-mfa-provider"></a>MFA Sağlayıcısı nedir?

Azure Multi-Factor Authentication lisansınız yoksa kullanıcılarınız için iki aşamalı doğrulamayı gerekli kılmak üzere kimlik doğrulama sağlayıcısı oluşturabilirsiniz.

Azure aboneliğinizin ücretlendirilme biçimi konusunda farklılık gösteren iki tür kimlik doğrulama sağlayıcısı vardır. Kimlik doğrulaması başına seçeneğinde, kiracınızda bir ay içinde gerçekleştirilen kimlik doğrulaması sayısı hesaplanır. Bu, yalnızca gereken durumlarda kimlik doğrulamasını kullanan birkaç kullanıcınızın olması halinde en iyi seçenektir. Kullanıcı başına seçeneğinde, kiracınızda bir ayda iki aşamalı doğrulama gerçekleştiren kişi sayısı hesaplanır. Bu, lisansı bulunan bazı kullanıcılarınızın olması ancak MFA'yı, lisanslama sınırlarınızı aşacak sayıda kullanıcıya genişletmeniz gereken durumlarda en iyi seçenektir.

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

Geçerli Multi-Factor Auth Sağlayıcısı bir Azure AD dizini (aynı zamanda Azure AD kiracısı olarak bilinir) ile ilişkili ise, MFA sağlayıcısını güvenli bir şekilde silebilir ve aynı Azure AD kiracısına bağlı bir sağlayıcı oluşturabilirsiniz. Alternatif olarak, MFA için etkinleştirilen tüm kullanıcıları kapsayacak sayıda MFA, Azure AD Premium veya Enterprise Mobility + Security (EMS) lisansı satın aldıysanız, MFA sağlayıcısını tamamen silebilirsiniz.

MFA sağlayıcınız bir Azure AD kiracısına bağlı değilse veya yeni MFA sağlayıcısını farklı bir Azure AD kiracısına bağlarsanız, kullanıcı ayarları ve yapılandırma seçenekleri aktarılmaz. Ayrıca, yeni MFA Sağlayıcısı ile oluşturulan etkinleştirme kimlik bilgileri kullanılarak mevcut Azure MFA Sunucularının yeniden etkinleştirilmesi gerekir. MFA Sunucularını yeni MFA Sağlayıcısına bağlamak için yeniden etkinleştirmek, telefon çağrısı ve kısa mesaj kimlik doğrulamasını etkilemez, ancak mobil uygulama etkinleştirilinceye kadar tüm kullanıcılar için mobil uygulama bildirimleri çalışmaz.

## <a name="next-steps"></a>Sonraki adımlar

[Multi-Factor Authentication ayarlarını yapılandırma](howto-mfa-mfasettings.md)

[Providers]: ./media/concept-mfa-authprovider/add-providers.png "MFA Sağlayıcıları Ekle"
