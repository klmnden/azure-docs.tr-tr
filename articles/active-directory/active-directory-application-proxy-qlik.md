---
title: Azure AD uygulama proxy'si ve Qlik algılama | Microsoft Docs
description: Azure portalında uygulama ara sunucusunu etkinleştirmek ve bağlayıcıları için ters proxy yükleyin.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 03/26/2018
ms.author: markvi
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 331f8ed2e77a076dd8969dc37add1cdeafc852dc
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="application-proxy-and-qlik-sense"></a>Uygulama proxy'si ve Qlik algılama 
Azure Active Directory Uygulama proxy'si ve Qlik algılama kolayca Qlik algılama dağıtımınız için uzaktan erişim sağlamak için uygulama proxy'si kullanmanız mümkün olduğundan emin olmak için birlikte ortaklık.  

## <a name="prerequisites"></a>Önkoşullar 
Bu senaryo geri kalanı, aşağıdaki bitti varsayılır:
 
- Yapılandırılmış [Qlik algılama](https://community.qlik.com/docs/DOC-19822). 
- Bir uygulama Proxy Bağlayıcısı yüklü 

## <a name="install-an-application-proxy-connector"></a>Bir uygulama ara sunucusu Bağlayıcısı'nı yüklemek 
Zaten uygulama Proxy etkin olması ve yüklü bir bağlayıcı varsa, bu bölüm atlayın ve üzerinde gitme [Azure AD uygulama proxy'si ile uygulamanızı eklemek](application-proxy-ping-access.md). 

Uygulama Ara sunucusu Bağlayıcısı'nı uzaktan çalışanlarınızın trafiği yayımlanan uygulamalarınızı yönlendiren bir Windows Server hizmetidir. Daha ayrıntılı yükleme yönergeleri için bkz: [Azure portalında uygulama ara sunucusunu etkinleştirme](active-directory-application-proxy-enable.md). 


1. [Azure portalında](https://portal.azure.com/) genel yönetici olarak oturum açın. 
2. Azure Active Directory'yi seçin > uygulama proxy'si. 
3. Uygulama Proxy Bağlayıcısı yüklemeyi başlatmak için indirme Bağlayıcısı'nı seçin. Yükleme yönergelerini izleyin. 
 
>[!NOTE]
>Bağlayıcı yükleme otomatik olarak etkinleştirmelisiniz uygulama proxy'si değil, seçebilirsiniz, ancak dizininiz için **uygulama ara sunucusunu etkinleştirme**. 
 
## <a name="publish-your-applications-in-azure"></a>Uygulamalarınızı Azure yayımlama 
QlikSense yayımlamak için iki uygulama Azure'da yayımlamak gerekir.  

### <a name="application-1"></a>Uygulama #1: 
Uygulamanızı yayımlamak için aşağıdaki adımları izleyin. 1-8, bkz: izlenecek adımların daha ayrıntılı için [Azure AD uygulama proxy'si ile uygulama yayımlama](application-proxy-publish-azure-portal.md). 


1. Son bölümde almadıysanız, Azure portalına genel yönetici olarak oturum açın. 
2. Seçin **Azure Active Directory** > **kurumsal uygulamalar**. 
3. Seçin **Ekle** dikey pencerenin üstündeki. 
4. Seçin **şirket içi uygulama**. 
5.       Yeni uygulamanızı hakkındaki bilgilerle gerekli alanları doldurun. Ayarları için aşağıdaki yönergeleri kullanın: 
    - **İç URL**: Bu uygulamayı QlikSense URL bir iç URL olması gerekir. Örneğin, **https&#58;//demo.qlikemm.com** 
    - **Ön kimlik doğrulama yöntemi**: Azure Active Directory (ancak bu zorunlu önerilir) 
1.       Seçin **Ekle** dikey pencerenin altındaki. Uygulamanızı eklenir ve Hızlı Başlat menüsü açılır. 
2.       Hızlı Başlangıç menüsünde seçin **test etmek için bir kullanıcı atamak**, ve uygulama için en az bir kullanıcı ekleyebilirsiniz. Bu test hesabının şirket içi uygulama erişimi olduğundan emin olun. 
3.       Seçin **atamak** test kullanıcı ataması kaydetmek için. 
4.       (İsteğe bağlı) Uygulama Yönetimi dikey penceresinde, çoklu oturum açma seçin. Seçin **Kerberos Kısıtlı temsilci** açılır menüsünden ve Qlik yapılandırmanızı temel alarak gerekli alanlar doldururken. **Kaydet**’i seçin. 

### <a name="application-2"></a>Uygulama #2: 
Aşağıdaki istisnalar dışında uygulama # 1'için olduğu gibi aynı adımları izleyin: 

**#5. adım**: İç URL QlikSense URL uygulama tarafından kullanılan kimlik doğrulama bağlantı noktası ile artık olması gerekir. Varsayılan değer **4244** HTTPS ve HTTP için 4248. EX: **https&#58;//demo.qlik.com:4244** 
**adım #10:** yoksa SSO'yu ayarlamak ve bırakın **çoklu oturum devre dışı açma**
 
 
## <a name="testing"></a>Test Etme 
Artık uygulamanızı test etmek hazırdır. Her iki uygulamalarına atanan bir kullanıcı olarak uygulama #1 ve oturum açma QlikSense yayımlamak için kullanılan dış URL erişin.  

## <a name="next-steps"></a>Sonraki Adımlar

- [Uygulama proxy'si ile uygulama yayımlama](application-proxy-publish-azure-portal.md)
- [Uygulama proxy'si bağlayıcıları ile çalışma](active-directory-application-proxy-connectors-azure-portal.md).
