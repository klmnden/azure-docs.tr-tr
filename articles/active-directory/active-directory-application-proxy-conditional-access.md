---
title: "Şirket içi uygulamalara - Azure AD koşullu erişim | Microsoft Docs"
description: "Azure AD uygulama proxy'si kullanarak uzaktan erişilmesine yayımlama uygulamalar için koşullu erişimi ayarlama alınmaktadır."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 2e97722b-eb4e-4078-b607-9fed210d8a0f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro; oldportal
ms.openlocfilehash: 463946256f9e335fa6d98fc904835e5c3dc2725e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="working-with-conditional-access-in-azure-ad-application-proxy"></a>Azure AD uygulama proxy'si koşullu erişim ile çalışma

>[!NOTE]
>Bu makale, devre dışı bırakıldığını Klasik Azure portalı için geçerlidir. Kullanmanızı öneririz [Azure portal](https://portal.azure.com). Azure portalında uygulama proxy'si uygulamaların başka bir SaaS uygulaması ile aynı koşullu erişim özellikleri vardır. Koşullu erişim hakkında daha fazla bilgi için bkz: [Azure Active Directory'de koşullu erişimi kullanmaya başlama](active-directory-conditional-access-azure-portal-get-started.md).

Uygulama proxy'si kullanarak yayımlanan uygulamalar için koşullu erişim vermek üzere erişim kurallarını yapılandırabilirsiniz. Şunları yapmanızı sağlar:

* Uygulama başına çok faktörlü kimlik doğrulaması gerektirir
* Yalnızca kullanıcıların iş olmadığında çok faktörlü kimlik doğrulaması gerektirir
* İşte olmadıkları zaman kullanıcıların uygulamaya erişmeyi engelle

Bu kurallar, tüm kullanıcılar ve gruplar için veya yalnızca belirli kullanıcılar ve gruplar için uygulanabilir. Varsayılan kural uygulamaya erişimi olan tüm kullanıcıları için geçerlidir. Ancak kural belirtilen güvenlik gruplarına üye olan kullanıcılara kısıtlanabilir.  

Erişim kuralları, bir kullanıcı OAuth 2.0, Openıd Connect, SAML veya WS-Federasyon kullanan bir federasyon uygulaması eriştiğinde değerlendirilir. Ayrıca, bir yenileme belirteci bir erişim belirteci almak için kullanıldığında, erişim kuralları OAuth 2.0 ve Openıd Connect ile değerlendirilir.

## <a name="conditional-access-prerequisites"></a>Koşullu erişim önkoşulları
* Azure Active Directory Premium aboneliği
* Federe veya yönetilen bir Azure Active Directory kiracısı
* Federasyon kiracıları multi-Factor authentication (MFA) gerekli  
    ![Erişim kuralları yapılandırın - çok faktörlü kimlik doğrulaması gerektirir](./media/active-directory-application-proxy-conditional-access/application-proxy-conditional-access.png)

## <a name="configure-per-application-multi-factor-authentication"></a>Uygulama başına çok faktörlü kimlik doğrulamasını yapılandırma
1. Klasik Azure portalında yönetici olarak oturum açın.
2. Active Directory'ye gidip Uygulama Ara Sunucusunu etkinleştirmek istediğiniz dizini seçin.
3. Tıklatın **uygulamaları** ve ekranı aşağı kaydırarak **erişim kuralları** bölümü. Erişim kuralları bölümünde yalnızca federe kimlik doğrulaması kullanan uygulama proxy'si kullanılarak yayımlanan uygulamalar için görüntülenir.
4. Kural seçerek etkinleştirin **erişim kurallarını etkinleştirme** için **üzerinde**.
5. Kullanıcılar ve gruplar Kime kuralları uygula belirtin. Kullanım **Grup Ekle** düğmesine tıklayarak erişim kuralının uygulanacağı bir veya daha fazla grup seçin. Bu iletişim kutusu, seçili grupların kaldırmak için de kullanılabilir.  Gruplara uygulanacak kuralları seçildiğinde erişim kuralları yalnızca belirtilen güvenlik gruplarının birine ait kullanıcılar için uygulanır.  

   * Güvenlik grupları kuraldan açıkça dışlamak için kontrol **dışında** ve bir veya daha fazla gruplarını belirtin. Except listedeki bir grubun üyeleri olan kullanıcılar çok faktörlü kimlik doğrulaması gerçekleştirmek için gerekli değildir.  
   * Bir kullanıcı kullanıcı başına çok faktörlü kimlik doğrulama özelliği kullanılarak yapılandırıldıysa, bu ayarı uygulama çok faktörlü kimlik doğrulaması kurallardan önceliklidir. Kullanıcı başına çok faktörlü kimlik doğrulaması için yapılandırılmış bir kullanıcı, uygulamanın çok faktörlü kimlik doğrulaması kurallardan bırakılan bile, çok faktörlü kimlik doğrulaması yapmak için gereklidir. Daha fazla bilgi edinmek [çok faktörlü kimlik doğrulama ve kullanıcı başına ayarları](../multi-factor-authentication/multi-factor-authentication.md).
6. Ayarlamak istediğiniz erişim kuralını seçin:

   * **Çok faktörlü kimlik doğrulaması gerektiren**: kuralın uygulandığı uygulama erişmeden önce tam çok faktörlü kimlik doğrulaması için kendisine erişim kuralları geçerli gereklidir.
   * **İş olduğunda değil, çok faktörlü kimlik doğrulaması gerektiren**: kullanıcıların uygulamanın güvenilir bir IP adresinden erişmelerine değil gerekecek çok faktörlü kimlik doğrulaması gerçekleştirin. Güvenilen IP adres aralıkları çok faktörlü kimlik doğrulama ayarları sayfasında yapılandırılabilir.
   * **Çalışma zaman değil, erişimi engelleme**: kullanıcıların şirket ağınızın dışındaki uygulamadan erişmelerine edemeyecek uygulamaya erişmek.

## <a name="configuring-mfa-for-federation-services"></a>MFA için Federasyon Hizmetleri Yapılandırılıyor
Federasyon kiracıları için çok faktörlü kimlik doğrulaması (MFA) şirket içi veya Azure Active Directory tarafından işlem yapılabilir AD FS sunucusu. Varsayılan olarak, MFA Azure Active Directory tarafından barındırılan herhangi bir sayfada gerçekleşir. MFA şirket içi yapılandırmak için Windows PowerShell'i çalıştırın ve Azure AD modülünü ayarlamak için – SupportsMFA özelliğini kullanın.

Aşağıdaki örnek, şirket içi MFA kullanarak etkinleştirmek gösterilmiştir [Set-MsolDomainFederationSettings cmdlet'i](https://msdn.microsoft.com/library/azure/dn194088.aspx) contoso.com Kiracı üzerinde:`Set-MsolDomainFederationSettings -DomainName contoso.com -SupportsMFA $true `

Bu bayrak ayarlamaya ek olarak, Federasyon Kiracı AD FS örneği çok faktörlü kimlik doğrulaması gerçekleştirmek için yapılandırılmış olması gerekir. Yönergeleri izleyin [Microsoft Azure çok faktörlü kimlik doğrulaması şirket içi dağıtma](../multi-factor-authentication/multi-factor-authentication-get-started-server.md).

## <a name="see-also"></a>Ayrıca bkz.
* [Talepleri kullanan uygulamalarla çalışma](active-directory-application-proxy-claims-aware-apps.md)
* [Uygulama proxy'si ile uygulama yayımlama](active-directory-application-proxy-publish.md)
* [Çoklu oturum açmayı etkinleştirme](active-directory-application-proxy-sso-using-kcd.md)
* [Kendi etki alanı adınızı kullanarak uygulama yayımlama](active-directory-application-proxy-custom-domains.md)

En yeni haberler ve güncelleştirmeler için [Uygulama Ara Sunucusu bloguna](http://blogs.technet.com/b/applicationproxyblog/) göz atın
