---
title: Azure Active Directory Uygulama proxy'si ve Tableau | Microsoft Docs
description: Tableau dağıtımınız için uzaktan erişim sağlamak için Azure Active Directory (Azure AD) uygulama proxy'si kullanmayı öğrenin.  .
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/24/2018
ms.author: barbkess
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 7cace1af527c1c7c80bf0e23f7a88aa9ac9f9d03
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2018
ms.locfileid: "39365031"
---
# <a name="azure-active-directory-application-proxy-and-tableau"></a>Azure Active Directory Uygulama proxy'si ve Tableau 

Azure Active Directory Uygulama proxy'si ve Tableau, Tableau dağıtımınız için uzaktan erişim sağlamak için uygulama proxy'si kullanmak kolayca emin olmak için iş ortaklığı kurdu. Bu makalede, bu senaryo yapılandırma açıklanmaktadır.  

## <a name="prerequisites"></a>Önkoşullar 

Bu makaledeki senaryoda, olduğunu varsayar:

- [Tableau](https://onlinehelp.tableau.com/current/server/en-us/proxy.htm#azure) yapılandırılmış. 

- Bir [uygulama ara sunucusu bağlayıcısını](application-proxy-enable.md) yüklü. 

 

## <a name="enabling-application-proxy-for-tableau"></a>Tableau için uygulama ara sunucusunu etkinleştirme 

Uygulama proxy'si için Tableau kullanmak istiyorsanız, bir e-posta göndermek gereken [ aadapfeedback@microsoft.com ](mailto:aadapfeedback@microsoft.com) etkin bu senaryo alınamıyor.
E-postanızda:

-   Uygulama Ara sunucusunu etkinleştirme Tableau için konu olarak kullanın.
-   Kiracı Kimliğinizi gövdesinde içerir    

Uygulamayı kullanmaya hazır olduğunuzda, bir onay alırsınız. Onay beklenirken yapılandırmaları tamamlayabilir.


 

## <a name="publish-your-applications-in-azure"></a>Uygulamalarınızı Azure'da yayımlayın 

Tableau yayımlamak için Azure Portalı'nda bir uygulama yayımlamak gerekir.

İçin:

- 1-8, bkz: adım yönergeleri ayrıntılı [Azure AD uygulama ara sunucusu kullanarak uygulama yayımlama](application-proxy-publish-azure-portal.md). 
- Tableau değerleri bulmak için uygulama proxy'si alanları hakkında bilgi, lütfen Tableau belgelerine bakın.  

**Uygulamanızı yayımlamak için**: 


1. [Azure portalında](https://portal.azure.com) genel yönetici olarak oturum açın. 

2. Seçin **Azure Active Directory > Kurumsal uygulamalar**. 

3. Seçin **Ekle** dikey penceresinin üstünde. 

4. Seçin **şirket içi uygulama**. 

5. Yeni uygulamanız hakkındaki bilgilerle gerekli alanları doldurun. Ayarları için aşağıdaki yönergeleri kullanın: 

    - **İç URL**: Bu uygulama, Tableau URL bir iç URL olması gerekir. Örneğin, `https://adventure-works.tableau.com`. 

    - **Ön kimlik doğrulama yöntemi**: Azure Active (önerilir ancak gerekli değildir) dizin. 

6. Seçin **Ekle** dikey penceresinin üstünde. Uygulamanızı eklenir ve Hızlı Başlangıç menüsü açılır. 

7. Hızlı Başlangıç menüde **test etmek için kullanıcı atama**, ve en az bir kullanıcı uygulamaya ekleyin. Bu test hesabı şirket içi uygulamaya erişimi olduğundan emin olun. 

8. Seçin **atama** test kullanıcı atama kaydetmek için. 

9. (İsteğe bağlı) Uygulama Yönetimi sayfasında **çoklu oturum açma**. Seçin **tümleşik Windows kimlik doğrulaması** açılan menüsünden ve Tableau yapılandırmanıza göre gerekli alanları doldurun. **Kaydet**’i seçin. 

 

## <a name="testing"></a>Test Etme 

Uygulamanızı şimdi test etmek hazırdır. Tableau, yayımlamak için kullanılan dış URL'sine erişin ve her iki uygulama için atanan bir kullanıcı olarak oturum açın.



## <a name="next-steps"></a>Sonraki adımlar

Azure AD uygulama proxy'si hakkında daha fazla bilgi için bkz: [güvenli uzaktan erişim sağlamak şirket içi uygulamalara](application-proxy.md).

