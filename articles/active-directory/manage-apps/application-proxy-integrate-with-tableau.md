---
title: Azure Active Directory Uygulama proxy'si ve Tableau | Microsoft Docs
description: Tableau dağıtımınız için uzaktan erişim sağlamak için Azure Active Directory (Azure AD) uygulama proxy'si kullanmayı öğrenin.
services: active-directory
author: barbkess
manager: daveba
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 08/20/2018
ms.author: barbkess
ms.reviewer: japere
ms.custom: it-pro
ms.openlocfilehash: b775d9377b95c3c63e3c38cd477ce47209a6f419
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55149978"
---
# <a name="azure-active-directory-application-proxy-and-tableau"></a>Azure Active Directory Uygulama proxy'si ve Tableau 

Azure Active Directory Uygulama proxy'si ve Tableau, Tableau dağıtımınız için uzaktan erişim sağlamak için uygulama proxy'si kullanmak kolayca emin olmak için iş ortaklığı kurdu. Bu makalede, bu senaryo yapılandırma açıklanmaktadır.  

## <a name="prerequisites"></a>Önkoşullar 

Bu makaledeki senaryoda, olduğunu varsayar:

- [Tableau](https://onlinehelp.tableau.com/current/server/en-us/proxy.htm#azure) yapılandırılmış. 

- Bir [uygulama ara sunucusu bağlayıcısını](application-proxy-add-on-premises-application.md) yüklü. 

 
## <a name="enabling-application-proxy-for-tableau"></a>Tableau için uygulama ara sunucusunu etkinleştirme 

Uygulama Ara sunucusu OAuth 2.0 yetki akışı, düzgün çalışması Tableau için gerekli olduğu destekler. Başka bir deyişle, artık, yayımlama adımları izleyerek yapılandırma dışında bu uygulama, etkinleştirmek için gereken tüm özel adım vardır.


## <a name="publish-your-applications-in-azure"></a>Uygulamalarınızı Azure'da yayımlayın 

Tableau yayımlamak için Azure Portalı'nda bir uygulama yayımlamak gerekir.

İçin:

- 1-8, bkz: adım yönergeleri ayrıntılı [Azure AD uygulama ara sunucusu kullanarak uygulama yayımlama](application-proxy-add-on-premises-application.md). 
- Tableau değerleri bulmak için uygulama proxy'si alanları hakkında bilgi, lütfen Tableau belgelerine bakın.  

**Uygulamanızı yayımlamak için**: 


1. [Azure portalında](https://portal.azure.com) genel yönetici olarak oturum açın. 

2. Seçin **Azure Active Directory > Kurumsal uygulamalar**. 

3. Seçin **Ekle** dikey penceresinin üstünde. 

4. Seçin **şirket içi uygulama**. 

5. Yeni uygulamanız hakkındaki bilgilerle gerekli alanları doldurun. Ayarları için aşağıdaki yönergeleri kullanın: 

    - **İç URL**: Bu uygulama, Tableau URL bir iç URL olması gerekir. Örneğin, `https://adventure-works.tableau.com`. 

    - **Ön kimlik doğrulama yöntemi**: Azure Active (önerilir ancak gerekli değildir) dizini. 

6. Seçin **Ekle** dikey penceresinin üstünde. Uygulamanızı eklenir ve Hızlı Başlangıç menüsü açılır. 

7. Hızlı Başlangıç menüde **test etmek için kullanıcı atama**, ve en az bir kullanıcı uygulamaya ekleyin. Bu test hesabı şirket içi uygulamaya erişimi olduğundan emin olun. 

8. Seçin **atama** test kullanıcı atama kaydetmek için. 

9. (İsteğe bağlı) Uygulama Yönetimi sayfasında **çoklu oturum açma**. Seçin **tümleşik Windows kimlik doğrulaması** açılan menüsünden ve Tableau yapılandırmanıza göre gerekli alanları doldurun. **Kaydet**’i seçin. 

 

## <a name="testing"></a>Test Etme 

Uygulamanızı şimdi test etmek hazırdır. Tableau, yayımlamak için kullanılan dış URL'sine erişin ve her iki uygulama için atanan bir kullanıcı olarak oturum açın.



## <a name="next-steps"></a>Sonraki adımlar

Azure AD uygulama proxy'si hakkında daha fazla bilgi için bkz: [güvenli uzaktan erişim sağlamak şirket içi uygulamalara](application-proxy.md).

