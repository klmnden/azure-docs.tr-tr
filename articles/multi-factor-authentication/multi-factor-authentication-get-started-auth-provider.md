---
title: "Azure Multi-Factor Auth Sağlayıcısı’nı kullanmaya başlama | Microsoft Belgeleri"
description: "Azure Multi-Factor Auth Sağlayıcısı oluşturma hakkında bilgi edinin."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: a7dd5030-7d40-4654-8fbd-88e53ddc1ef5
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/28/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ms.translationtype: Human Translation
ms.sourcegitcommit: 3716c7699732ad31970778fdfa116f8aee3da70b
ms.openlocfilehash: 977640041f4b58a751848c96e2aa48eb2b284154
ms.contentlocale: tr-tr
ms.lasthandoff: 06/30/2017

---

# <a name="getting-started-with-an-azure-multi-factor-auth-provider"></a>Azure Multi-Factor Auth Sağlayıcısını kullanmaya başlama
İki adımlı doğrulama, Azure Active Directory’ye sahip genel yöneticiler ve Office 365 kullanıcıları için varsayılan olarak kullanılabilir durumdadır. Ancak, [gelişmiş özelliklerden](multi-factor-authentication-whats-next.md) yararlanmak isterseniz Azure Multi-Factor Authentication’ın (MFA) tam sürümünü satın almanız gerekir.

Azure Multi-Factor Auth Sağlayıcısı Azure MFA tam sürümünün sağladığı özelliklerden yararlanmak için kullanılır. **Azure MFA, Azure AD Premium veya Enterprise Mobility + Security (EMS) lisansı olmayan** kullanıcılara yöneliktir.  Azure MFA, Azure AD Premium ve EMS varsayılan olarak Azure MFA’nın tam sürümünü içerir. Lisanslarınız varsa bir Azure Multi-Factor Auth Sağlayıcısına ihtiyacınız yoktur.

SDK’yı indirmek için Azure Multi-Factor Auth sağlayıcısı gerekir.

> [!IMPORTANT]
> SDK’yı indirmek için Azure MFA, AAD Premium veya EMS lisanslarınız olsa bile bir Azure Multi-Factor Auth Sağlayıcısı oluşturun.  Bu amaçla Azure Multi-Factor Auth Sağlayıcısı oluşturursanız ve zaten lisanslarınız varsa Sağlayıcıyı **Etkin Kullanıcı Başına** modeliyle oluşturduğunuzdan emin olun. Ardından, Sağlayıcıyı Azure MFA, Azure AD Premium veya EMS lisansları içeren dizine bağlayın. Bu yapılandırma, sizden yalnızca iki aşamalı doğrulama kullanan benzersiz kullanıcılarınızın sayısı sahip olduğunuz lisanslardan daha fazlaysa ücret alınmasını sağlar.

## <a name="what-is-an-azure-multi-factor-auth-provider"></a>Azure Multi-Factor Auth Sağlayıcısı nedir?

Azure Multi-Factor Authentication lisansınız yoksa kullanıcılarınız için iki aşamalı doğrulamayı gerekli kılmak üzere kimlik doğrulama sağlayıcısı oluşturabilirsiniz. Özel bir uygulama geliştiriyorsanız ve Azure MFA'yı etkinleştirmek istiyorsanız bir kimlik doğrulama sağlayıcısı oluşturun ve [SDK'yı indirin](multi-factor-authentication-sdk.md).

Azure aboneliğinizin ücretlendirilme biçimi konusunda farklılık gösteren iki tür kimlik doğrulama sağlayıcısı vardır. Kimlik doğrulaması başına seçeneğinde, kiracınızda bir ay içinde gerçekleştirilen kimlik doğrulaması sayısı hesaplanır. Bu, yalnızca gereken durumlarda (örneğin, özel bir uygulama için MFA'yı gerekli kıldıysanız) kimlik doğrulamasını kullanan belirli sayıda kullanıcınızın olması halinde en iyi seçenektir. Kullanıcı başına seçeneğinde, kiracınızda bir ayda iki aşamalı doğrulama gerçekleştiren kişi sayısı hesaplanır. Bu, lisansı bulunan bazı kullanıcılarınızın olması ancak MFA'yı, lisanslama sınırlarınızı aşacak sayıda kullanıcıya genişletmeniz gereken durumlarda en iyi seçenektir.

## <a name="create-a-multi-factor-auth-provider"></a>Multi-Factor Auth Sağlayıcısı oluşturma
Azure Multi-Factor Auth Sağlayıcısı oluşturmak için aşağıdaki adımları kullanın.

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
   1. **Ad** – Multi-Factor Auth Sağlayıcısının adı.
   2. **Kullanım Modeli**: İki seçenekten birini belirleyin:
      * Kimlik Doğrulaması Başına – kimlik doğrulaması başına ücretlendirilen satın alma modeli. Genellikle tüketiciyle karşılaşan uygulamada Azure Multi-Factor Authentication kullanan senaryolar için kullanılır.
      * Etkin Kullanıcı Başına - etkin kullanıcı başına ücretlendirilen satın alma modeli. Genellikle Office 365 gibi uygulamalara çalışan erişimi için kullanılır. Azure MFA için zaten lisansı olan kullanıcılarınız varsa bu seçeneği belirleyin.
   3. **Dizin** – Multi-Factor Authentication Sağlayıcısının ilişkili olduğu Azure Active Directory kiracısı. Lütfen aşağıdakilere dikkat edin:
      * Bir Multi-Factor Auth Sağlayıcısı oluşturmak için Azure AD dizini gerekli değildir. Yalnızca Azure Multi-Factor Authentication Sunucusunu veya SDK’yı kullanmayı planlıyorsanız kutuyu boş bırakın.
      * Gelişmiş özelliklerden yararlanmak için Multi-Factor Auth Sağlayıcısının bir Azure AD dizini ile ilişkili olması gerekir.
      * Azure AD Connect, AAD Sync veya DirSync yalnızca şirket içi Active Directory ortamınızı bir Azure AD diziniyle eşitliyorsanız gereklidir.  Yalnızca eşitlenmemiş bir Azure AD dizini kullanıyorsanız bunun yapılması gerekli değildir.
        
        ![MFA Sağlayıcısı oluşturma](./media/multi-factor-authentication-get-started-auth-provider/authprovider5.png)

8. Oluştur’a tıkladıktan sonra Multi-Factor Authentication Sağlayıcısı oluşturulur ve şu iletiyi görmeniz gerekir: **Multi-Factor Authentication Sağlayıcısı başarıyla oluşturuldu**. **Tamam**’a tıklayın.
   
   ![MFA Sağlayıcısı oluşturma](./media/multi-factor-authentication-get-started-auth-provider/authprovider6.png)  

