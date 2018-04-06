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
ms.date: 04/03/2018
ms.author: markvi
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: e10fefdd3bd46aeb90fd2cfc82d4fee3b17d867b
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="application-proxy-and-qlik-sense"></a>Uygulama proxy'si ve Qlik algılama 
Azure Active Directory Uygulama proxy'si ve Qlik algılama kolayca Qlik algılama dağıtımınız için uzaktan erişim sağlamak için uygulama proxy'si kullanmanız mümkün olduğundan emin olmak için birlikte ortaklık.  

## <a name="prerequisites"></a>Önkoşullar 
Bu senaryo geri kalanı, aşağıdaki bitti varsayılır:
 
- Yapılandırılmış [Qlik algılama](https://community.qlik.com/docs/DOC-19822). 
- [Bir uygulama Proxy Bağlayıcısı yüklü](active-directory-application-proxy-enable.md#install-and-register-a-connector) 
 
## <a name="publish-your-applications-in-azure"></a>Uygulamalarınızı Azure yayımlama 
QlikSense yayımlamak için iki uygulama Azure'da yayımlamak gerekir.  

### <a name="application-1"></a>Uygulama #1: 
Uygulamanızı yayımlamak için aşağıdaki adımları izleyin. 1-8, bkz: izlenecek adımların daha ayrıntılı için [Azure AD uygulama proxy'si ile uygulama yayımlama](application-proxy-publish-azure-portal.md). 


1. Azure portalına genel yönetici olarak oturum açın. 
2. Seçin **Azure Active Directory** > **kurumsal uygulamalar**. 
3. Seçin **Ekle** dikey pencerenin üstündeki. 
4. Seçin **şirket içi uygulama**. 
5.       Yeni uygulamanızı hakkındaki bilgilerle gerekli alanları doldurun. Ayarları için aşağıdaki yönergeleri kullanın: 
    - **İç URL**: Bu uygulamayı QlikSense URL bir iç URL olması gerekir. Örneğin, **https&#58;//demo.qlikemm.com:4244** 
    - **Ön kimlik doğrulama yöntemi**: Azure Active Directory (ancak bu zorunlu önerilir) 
1.       Seçin **Ekle** dikey pencerenin altındaki. Uygulamanızı eklenir ve Hızlı Başlat menüsü açılır. 
2.       Hızlı Başlangıç menüsünde seçin **test etmek için bir kullanıcı atamak**, ve uygulama için en az bir kullanıcı ekleyebilirsiniz. Bu test hesabının şirket içi uygulama erişimi olduğundan emin olun. 
3.       Seçin **atamak** test kullanıcı ataması kaydetmek için. 
4.       (İsteğe bağlı) Uygulama Yönetimi dikey penceresinde, çoklu oturum açma seçin. Seçin **Kerberos Kısıtlı temsilci** açılır menüsünden ve Qlik yapılandırmanızı temel alarak gerekli alanlar doldururken. **Kaydet**’i seçin. 

### <a name="application-2"></a>Uygulama #2: 
Aşağıdaki istisnalar dışında uygulama # 1'için olduğu gibi aynı adımları izleyin: 

**#5. adım**: İç URL QlikSense URL uygulama tarafından kullanılan kimlik doğrulama bağlantı noktası ile artık olması gerekir. Varsayılan değer **4244** HTTPS ve HTTP için 4248. EX: **https&#58;//demo.qlik.com:4244**</br></br> 
**#10. adım:** yoksa SSO'yu ayarlamak ve bırakın **çoklu oturum devre dışı açma**
 
 
## <a name="testing"></a>Test Etme 
Artık uygulamanızı test etmek hazırdır. Her iki uygulamalarına atanan bir kullanıcı olarak uygulama #1 ve oturum açma QlikSense yayımlamak için kullanılan dış URL erişin.  

## <a name="next-steps"></a>Sonraki Adımlar

- [Uygulama proxy'si ile uygulama yayımlama](application-proxy-publish-azure-portal.md)
- [Uygulama proxy'si bağlayıcıları ile çalışma](active-directory-application-proxy-connectors-azure-portal.md).
