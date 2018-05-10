---
title: Azure Active Directory Uygulama proxy'si ve Tableau | Microsoft Docs
description: Azure Active Directory (Azure AD) uygulama proxy'si Tableau dağıtımınız için uzaktan erişim sağlamak için nasıl kullanılacağını öğrenin.  .
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/02/2018
ms.author: barbkess
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: c79689ae7527a715266bad62ec50fddaab90129d
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="azure-active-directory-application-proxy-and-tableau"></a>Azure Active Directory Uygulama proxy'si ve Tableau 

Azure Active Directory Uygulama proxy'si ve Tableau kolayca Tableau dağıtımınız için uzaktan erişim sağlamak için uygulama proxy'si kullanmanız mümkün olduğundan emin olmak için ortaklık. Bu makalede, bu senaryoyu yapılandırmak açıklanmaktadır.  

## <a name="prerequisites"></a>Önkoşullar 

Bu makalede senaryosunda, sahibi olduğunuzu varsayar:

- [Tableau](https://onlinehelp.tableau.com/current/server/en-us/proxy.htm#azure) yapılandırılmış. 

- Bir [uygulama ara sunucusu Bağlayıcısı](active-directory-application-proxy-enable.md) yüklü. 

 

## <a name="enabling-application-proxy-for-tableau"></a>Uygulama proxy'si Tableau için etkinleştirme 

Uygulama proxy'si Tableau için kullanmak istiyorsanız, bir e-posta göndermek gereken [ aadapfeedback@microsoft.com ](mailto:aadapfeedback@microsoft.com) etkin bu senaryo alınamıyor.
E-postanızda:

-   Uygulama Ara sunucusunu etkinleştirme için Tableau konu olarak kullanın.
-   Kiracı Kimliğinizi gövdesinde içerir    

Uygulamayı kullanmak hazır olduğunuzda, bir onay iletisi. Genellikle, bu hakkında bir hafta sürer. Ancak, yapılandırmaları için onay beklenirken bitirebilirsiniz.


 

## <a name="publish-your-applications-in-azure"></a>Uygulamalarınızı Azure yayımlama 

Tableau yayımlamak için Azure Portalı'nda bir uygulama yayımlama olmanız gerekir.

İçin:

- Ayrıntılı yönergeler adım 1-8, bkz: [Azure AD uygulama proxy'si ile uygulama yayımlama](application-proxy-publish-azure-portal.md). 
- Uygulama proxy'si alanlar için Tableau değerleri bulma hakkında bilgi Lütfen Tableau belgelerine bakın.  

**Uygulamanızı yayımlamak için**: 


1. [Azure portalında](https://portal.azure.com) genel yönetici olarak oturum açın. 

2. Seçin **Azure Active Directory > Kurumsal uygulamalar**. 

3. Seçin **Ekle** dikey pencerenin üstündeki. 

4. Seçin **şirket içi uygulama**. 

5. Yeni uygulamanızı hakkındaki bilgilerle gerekli alanları doldurun. Ayarları için aşağıdaki yönergeleri kullanın: 

    - **İç URL**: Bu uygulamayı Tableau URL bir iç URL olması gerekir. Örneğin, `https://adventure-works.tableau.com`. 

    - **Ön kimlik doğrulama yöntemi**: Azure Active (önerilir ancak gerekli değildir) dizini. 

6. Seçin **Ekle** dikey pencerenin üstündeki. Uygulamanızı eklenir ve Hızlı Başlat menüsü açılır. 

7. Hızlı Başlangıç menüsünde seçin **test etmek için bir kullanıcı atamak**, ve uygulama için en az bir kullanıcı ekleyebilirsiniz. Bu test hesabının şirket içi uygulama erişimi olduğundan emin olun. 

8. Seçin **atamak** test kullanıcı ataması kaydetmek için. 

9. (İsteğe bağlı) Uygulama Yönetimi sayfasında, seçin **çoklu oturum açma**. Seçin **tümleşik Windows kimlik doğrulaması** açılır menüsünden ve Tableau yapılandırmanızı temel alarak gerekli alanlar doldururken. **Kaydet**’i seçin. 

 

## <a name="testing"></a>Test Etme 

Artık uygulamanızı test etmek hazırdır. Tableau, yayımlamak için kullanılan dış URL erişmek ve her iki uygulamalarına atanan bir kullanıcı olarak oturum açın.



## <a name="next-steps"></a>Sonraki adımlar

Azure AD uygulama proxy'si hakkında daha fazla bilgi için bkz: [güvenli uzaktan erişim sağlamak şirket içi uygulamalar](active-directory-application-proxy-get-started.md).

