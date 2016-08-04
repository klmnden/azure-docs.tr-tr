<properties
   pageTitle="Azure AD Privileged Identity Management ile çalışmaya başlama | Microsoft Azure"
   description="Azure portalında Azure Active Directory Privileged Identity Management uygulaması ile ayrıcalıklı kimlikleri nasıl yöneteceğinizi öğrenin."
   services="active-directory"
   documentationCenter=""
   authors="kgremban"
   manager="stevenpo"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="05/19/2016"
   ms.author="kgremban"/>

# Azure AD Privileged Identity Management ile çalışmaya başlama


Azure Active Directory (AD) Privileged Identity Management, ayrıcalıklı kimliklerinizi ve Azure AD'deki kaynakların yanı sıra, Office 365 veya Microsoft Intune diğer Microsoft çevrimiçi hizmetlerinin içerdiği kaynaklara yönelik erişimi yönetmenize, denetlemenize ve izlemenize olanak tanır.  

Bu makalede Azure AD PIM uygulamasını Azure portalı panonuza nasıl ekleyeceğiniz anlatılmaktadır.

## Privileged Identity Management uygulamasını ekleme

Azure AD Privileged Identity Management'ı kullanmadan önce uygulamayı Azure portalı panonuza eklemeniz gerekir.

1. [Azure portalında](https://portal.azure.com/) dizininizin genel yöneticisi olarak oturum açın.
2. Kuruluşunuz birden fazla dizine sahipse Azure portalının sağ üst köşesinde kullanıcı adınıza tıklayın ve PIM'yi kullanacağı dizini seçin.
3. Sol gezinti bölmesinde **Yeni**'yi seçin.
4. **Güvenlik + Kimlik** seçeneğini belirleyin.
5. **Azure AD Privileged Identity Management**'ı seçin.
6. **Panoya sabitle**'yi işaretleyin ve ardından **Oluştur** düğmesine tıklayın. Privileged Identity Management uygulaması açılır.


Dizininizde Azure AD Privileged Identity Management'ı kullanan ilk kişiyseniz [güvenlik sihirbazı](active-directory-privileged-identity-management-security-wizard.md) ilk atama deneyimi boyunca size yol gösterir. Bunun ardından, otomatik olarak dizinin ilk **güvenlik yöneticisi** ve **ayrıcalıklı rol yöneticisi** haline gelirsiniz. Yalnızca bir ayrıcalıklı rol yöneticisi diğer yöneticiler için erişimi yönetmek üzere bu uygulamaya erişebilir.  

Bunun aksine başka bir ayrıcalıklı yol yöneticisi tarafından bir veya daha fazla role atandıysanız hangi rolü etkinleştireceğinizi seçebilirsiniz. Ayrıcalıklı rol yöneticisi rolünde olmanız durumunda **Kimlikleri Yönet** seçeneğini de görürsünüz.  


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## Sonraki adımlar

[Azure AD Privileged Identity Management'a genel bakış](active-directory-privileged-identity-management-configure.md) kuruluşunuzda yönetim erişimini nasıl yönetebileceğiniz konusunda daha fazla ayrıntı içerir.

[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]



<!----HONumber=Jun16_HO2-->


