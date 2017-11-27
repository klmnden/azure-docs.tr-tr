---
title: "Azure Multi-Factor Auth Sağlayıcısı’nı kullanmaya başlama | Microsoft Belgeleri"
description: "Azure Multi-Factor Auth Sağlayıcısı oluşturma hakkında bilgi edinin."
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.assetid: a7dd5030-7d40-4654-8fbd-88e53ddc1ef5
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/02/2017
ms.author: joflore
ms.reviewer: richagi
ms.custom: it-pro
ms.openlocfilehash: dc1664d382c6e59c125ef00d02a8848079d8bf8d
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/23/2017
---
# <a name="getting-started-with-an-azure-multi-factor-authentication-provider"></a>Azure Multi-Factor Authentication Sağlayıcısı kullanmaya başlama
İki adımlı doğrulama, Azure Active Directory’ye sahip genel yöneticiler ve Office 365 kullanıcıları için varsayılan olarak kullanılabilir durumdadır. Ancak, [gelişmiş özelliklerden](multi-factor-authentication-whats-next.md) yararlanmak isterseniz Azure Multi-Factor Authentication’ın (MFA) tam sürümünü satın almanız gerekir.

Azure Multi-Factor Auth Sağlayıcısı Azure MFA tam sürümünün sağladığı özelliklerden yararlanmak için kullanılır. **Azure MFA, Azure AD Premium veya Enterprise Mobility + Security (EMS) lisansı olmayan** kullanıcılara yöneliktir.  Azure MFA, Azure AD Premium ve EMS varsayılan olarak Azure MFA’nın tam sürümünü içerir. Lisanslarınız varsa bir Azure Multi-Factor Auth Sağlayıcısına ihtiyacınız yoktur.

SDK’yı indirmek için Azure Multi-Factor Auth sağlayıcısı gerekir.

> [!IMPORTANT]
> Azure Multi-Factor Authentication Yazılım Geliştirme Seti’nin (SDK) kullanımdan kaldırılacağı duyurulmuştur. Bu özellik yeni müşteriler için artık desteklenmemektedir. Mevcut müşteriler 14 Kasım 2018’e kadar SDK'yı kullanmaya devam edebilir. O tarihten sonra SDK çağrıları başarısız olacaktır.
> [!IMPORTANT]
>SDK’yı indirmek için Azure MFA, AAD Premium veya EMS lisanslarınız olsa bile bir Azure Multi-Factor Auth Sağlayıcısı oluşturmanız gerekir.  Bu amaçla Azure Multi-Factor Auth Sağlayıcısı oluşturursanız ve zaten lisanslarınız varsa Sağlayıcıyı **Etkin Kullanıcı Başına** modeliyle oluşturduğunuzdan emin olun. Ardından, Sağlayıcıyı Azure MFA, Azure AD Premium veya EMS lisansları içeren dizine bağlayın. Bu yapılandırma, sizden yalnızca iki aşamalı doğrulama kullanan benzersiz kullanıcılarınızın sayısı sahip olduğunuz lisanslardan daha fazlaysa ücret alınmasını sağlar. 

## <a name="what-is-an-mfa-provider"></a>MFA Sağlayıcısı nedir?

Azure Multi-Factor Authentication lisansınız yoksa kullanıcılarınız için iki aşamalı doğrulamayı gerekli kılmak üzere kimlik doğrulama sağlayıcısı oluşturabilirsiniz.

Azure aboneliğinizin ücretlendirilme biçimi konusunda farklılık gösteren iki tür kimlik doğrulama sağlayıcısı vardır. Kimlik doğrulaması başına seçeneğinde, kiracınızda bir ay içinde gerçekleştirilen kimlik doğrulaması sayısı hesaplanır. Bu, yalnızca gereken durumlarda kimlik doğrulamasını kullanan birkaç kullanıcınızın olması halinde en iyi seçenektir. Kullanıcı başına seçeneğinde, kiracınızda bir ayda iki aşamalı doğrulama gerçekleştiren kişi sayısı hesaplanır. Bu, lisansı bulunan bazı kullanıcılarınızın olması ancak MFA'yı, lisanslama sınırlarınızı aşacak sayıda kullanıcıya genişletmeniz gereken durumlarda en iyi seçenektir.

## <a name="create-an-mfa-provider---public-preview"></a>MFA Sağlayıcısı oluşturma - Genel önizleme

Azure portalında Azure Multi-Factor Authentication Sağlayıcısı oluşturmak için aşağıdaki adımları izleyin:

1. [Azure Portal](https://portal.azure.com)’da yönetici olarak oturum açın. 
2. **Azure Active Directory** > **MFA Sunucusu**'nu seçin.
3. **Sağlayıcılar**'ı seçin.
4. **Add (Ekle)** seçeneğini belirleyin.
5. Aşağıdaki alanları doldurun ve **Ekle**'yi seçin:
   - **Ad** – Sağlayıcının adı.
   - **Kullanım Modeli**: İki seçenekten birini belirleyin:
      * Kimlik Doğrulaması Başına – kimlik doğrulaması başına ücretlendirilen satın alma modeli. Genellikle tüketiciyle karşılaşan uygulamada Azure Multi-Factor Authentication kullanan senaryolar için kullanılır.
      * Etkin Kullanıcı Başına - etkin kullanıcı başına ücretlendirilen satın alma modeli. Genellikle Office 365 gibi uygulamalara çalışan erişimi için kullanılır. Azure MFA için zaten lisansı olan kullanıcılarınız varsa bu seçeneği belirleyin.
   - **Abonelik** – Sağlayıcı üzerinden gerçekleştirilen iki aşamalı doğrulama etkinliği için ücretlendirilen Azure aboneliği. 
   - **Dizin** – Sağlayıcının ilişkili olduğu Azure Active Directory kiracısı.
      * Bir Sağlayıcı oluşturmak için Azure AD dizini gerekli değildir. Yalnızca Azure Multi-Factor Authentication Sunucusunu indirmeyi planlıyorsanız bu kutuyu boş bırakın.
      * Gelişmiş özelliklerden yararlanmak için Sağlayıcının bir Azure AD dizini ile ilişkili olması gerekir.
      * Herhangi bir Azure AD dizini ile yalnızca bir Sağlayıcı ilişkili olabilir.

## <a name="create-an-mfa-provider"></a>MFA Sağlayıcısı oluşturma
Klasik portalda Azure Multi-Factor Authentication Sağlayıcısı oluşturmak için aşağıdaki adımları izleyin:

1. [Klasik Azure portalında](https://manage.windowsazure.com) yönetici olarak oturum açın.
2. Sol taraftaki **Active Directory** öğesini seçin.
3. Active Directory sayfasının en üst kısmındaki **Multi-Factor Authentication Sağlayıcıları**’na tıklayın.
   
   ![MFA Sağlayıcısı oluşturma](./media/multi-factor-authentication-get-started-auth-provider/authprovider1.png)

4. Alt kısımda **Yeni**’ye tıklayın.
   
   ![MFA Sağlayıcısı oluşturma](./media/multi-factor-authentication-get-started-auth-provider/authprovider2.png)

5. Uygulama Hizmetleri altında **Multi-Factor Auth Sağlayıcısı**'nı seçin.
   
   ![MFA Sağlayıcısı oluşturma](./media/multi-factor-authentication-get-started-auth-provider/authprovider3.png)

6. **Hızlı Oluştur**’u seçin.
   
   ![MFA Sağlayıcısı oluşturma](./media/multi-factor-authentication-get-started-auth-provider/authprovider4.png)

7. Aşağıdaki alanları doldurun ve **Oluştur**’u seçin.
   1. **Ad** – Sağlayıcının adı.
   2. **Kullanım Modeli**: İki seçenekten birini belirleyin:
      * Kimlik Doğrulaması Başına – kimlik doğrulaması başına ücretlendirilen satın alma modeli. Genellikle tüketiciyle karşılaşan uygulamada Azure Multi-Factor Authentication kullanan senaryolar için kullanılır.
      * Etkin Kullanıcı Başına - etkin kullanıcı başına ücretlendirilen satın alma modeli. Genellikle Office 365 gibi uygulamalara çalışan erişimi için kullanılır. Azure MFA için zaten lisansı olan kullanıcılarınız varsa bu seçeneği belirleyin.
   3. **Dizin** – Sağlayıcının ilişkili olduğu Azure Active Directory kiracısı.
      * Bir Sağlayıcı oluşturmak için Azure AD dizini gerekli değildir. Yalnızca Azure Multi-Factor Authentication Sunucusunu indirmeyi planlıyorsanız bu kutuyu boş bırakın.
      * Gelişmiş özelliklerden yararlanmak için Sağlayıcının bir Azure AD dizini ile ilişkili olması gerekir.
      * Herhangi bir Azure AD dizini ile yalnızca bir Sağlayıcı ilişkili olabilir.  
      ![MFA Sağlayıcısı oluşturma](./media/multi-factor-authentication-get-started-auth-provider/authprovider5.png)

8. Oluştur’a tıkladıktan sonra Multi-Factor Authentication Sağlayıcısı oluşturulur ve şu iletiyi görmeniz gerekir: **Multi-Factor Authentication Sağlayıcısı başarıyla oluşturuldu**. **Tamam**’a tıklayın.  
   
   ![MFA Sağlayıcısı oluşturma](./media/multi-factor-authentication-get-started-auth-provider/authprovider6.png)  

## <a name="manage-your-mfa-provider"></a>MFA Sağlayıcınızı yönetme

Bir MFA sağlayıcısı oluşturulduktan sonra kullanım modelini (etkin kullanıcı başına veya kimlik doğrulaması başına) değiştiremezsiniz. Ancak, MFA sağlayıcısı silip daha sonra farklı bir kullanım modeliyle bir sağlayıcı oluşturabilirsiniz.

Geçerli Multi-Factor Auth Sağlayıcısı bir Azure AD dizini (aynı zamanda Azure AD kiracısı olarak bilinir) ile ilişkili ise, MFA sağlayıcısını güvenli bir şekilde silebilir ve aynı Azure AD kiracısına bağlı bir sağlayıcı oluşturabilirsiniz. Alternatif olarak, MFA için etkinleştirilen tüm kullanıcıları kapsayacak sayıda MFA, Azure AD Premium veya Enterprise Mobility + Security (EMS) lisansı satın aldıysanız, MFA sağlayıcısını tamamen silebilirsiniz.

MFA sağlayıcınız bir Azure AD kiracısına bağlı değilse veya yeni MFA sağlayıcısını farklı bir Azure AD kiracısına bağlarsanız, kullanıcı ayarları ve yapılandırma seçenekleri aktarılmaz. Ayrıca, yeni MFA Sağlayıcısı ile oluşturulan etkinleştirme kimlik bilgileri kullanılarak mevcut Azure MFA Sunucularının yeniden etkinleştirilmesi gerekir. MFA Sunucularını yeni MFA Sağlayıcısına bağlamak için yeniden etkinleştirmek, telefon çağrısı ve kısa mesaj kimlik doğrulamasını etkilemez, ancak mobil uygulama etkinleştirilinceye kadar tüm kullanıcılar için mobil uygulama bildirimleri çalışmaz.

## <a name="next-steps"></a>Sonraki adımlar

[Multi-Factor Authentication ayarlarını yapılandırma](multi-factor-authentication-whats-next.md)
