---
title: "Öğretici: Azure Active Directory Tümleştirme ile Cisco Webex | Microsoft Docs"
description: "Cisco Webex Azure Active Directory ile çoklu oturum açma, otomatik sağlama ve daha fazlasını sağlamak için nasıl kullanılacağını öğrenin!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 26704ca7-13ed-4261-bf24-fd6252e2072b
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/10/2017
ms.author: jeedes
ms.openlocfilehash: b44b1a5b3e988a51db3325ec8a181651fa84e768
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cisco-webex"></a>Öğretici: Cisco Webex Azure Active Directory Tümleştirme
Bu öğreticinin amacı, Azure ve Cisco Webex tümleştirmesini göstermektir.  
Bu öğreticide gösterilen senaryo, aşağıdaki öğeleri zaten sahip olduğunuzu varsayar:

* Geçerli bir Azure aboneliği
* Cisco Webex Kiracı

Bu öğreticiyi tamamladıktan sonra Cisco Webex atamış Azure AD kullanıcıları çoklu oturum açma, Cisco Webex şirket sitenizdeki (servis sağlayıcı tarafından başlatılan oturum açma) uygulamaya veya kullanarak okuyamayacak [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md).

Bu öğreticide gösterilen senaryo, aşağıdaki yapı taşlarını oluşur:

* Cisco Webex uygulama tümleştirmesi etkinleştirme
* Çoklu oturum açma (SSO) yapılandırma
* Kullanıcı hazırlama işleminin yapılandırılması
* Kullanıcılar atama

![Senaryo](./media/active-directory-saas-cisco-webex-tutorial/IC777614.png "senaryosu")

## <a name="enable-the-application-integration-for-cisco-webex"></a>Cisco Webex uygulama tümleştirmeyi etkinleştir
Bu bölümün amacı, Cisco Webex için uygulama tümleştirme sağlamak üzere nasıl anahat sağlamaktır.

**Cisco Webex uygulama tümleştirmesini etkinleştirmek için aşağıdaki adımları gerçekleştirin:**

1. Azure Klasik portalında, sol gezinti bölmesinde tıklatın **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-cisco-webex-tutorial/IC700993.png "Active Directory")
2. Gelen **Directory** listesinde, directory tümleştirmesini etkinleştirmek istediğiniz dizini seçin.
3. Dizin görünümünde uygulamaları görünümü açmak için **uygulamaları** üst menüde.
   
   ![Uygulamaları](./media/active-directory-saas-cisco-webex-tutorial/IC700994.png "uygulamalar")
4. Tıklatın **Ekle** sayfanın sonundaki.
   
   ![Uygulama ekleme](./media/active-directory-saas-cisco-webex-tutorial/IC749321.png "uygulama ekleme")
5. Üzerinde **ne yapmak istiyorsunuz** iletişim kutusunda, tıklatın **Galeriden bir uygulama eklemek**.
   
   ![Galeriden bir uygulama eklemek](./media/active-directory-saas-cisco-webex-tutorial/IC749322.png "Galeriden bir uygulama ekleme")
6. İçinde **arama kutusu**, türü **Cisco Webex**.
   
   ![Uygulama Galerisi](./media/active-directory-saas-cisco-webex-tutorial/IC777615.png "uygulama Galerisi")
7. Sonuçlar bölmesinde seçin **Cisco Webex**ve ardından **tam** uygulama eklemek için.
   
   ![Cisco Webex](./media/active-directory-saas-cisco-webex-tutorial/IC777616.png "Cisco Webex")
   
## <a name="configure-single-sign-on"></a>Çoklu oturum açmayı yapılandırın

Bu bölümün amacı kullanıcıların Cisco Webex kendi hesabıyla SAML protokolünü temel Federasyon kullanarak Azure AD kimlik doğrulaması sağlamak nasıl anahat sağlamaktır.  

Bu yordam bir parçası olarak, bir base-64 kodlanmış sertifika oluşturmak için gereklidir. Bu yordama bilmiyorsanız bkz [ikili bir sertifika bir metin dosyasına dönüştürme](http://youtu.be/PlgrzUZ-Y1o)

**Çoklu oturum açma yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure Klasik portalında üzerinde **Cisco Webex** uygulama tümleştirmesi sayfasında, tıklatın **çoklu oturum açma özelliğini yapılandırın** açmak için **tek oturum yapılandırma üzerinde aktar** iletişim.
   
   ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-cisco-webex-tutorial/IC777617.png "çoklu oturum açmayı yapılandırın")
2. Üzerinde **Cisco Webex oturum açmasını nasıl istiyorsunuz** sayfasında, **Microsoft Azure AD çoklu oturum açma**ve ardından **sonraki**.
   
   ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-cisco-webex-tutorial/IC777618.png "çoklu oturum açmayı yapılandırın")
3. Üzerinde **uygulama URL'sini Yapılandır** sayfasında, aşağıdaki adımları uygulayın ve ardından **sonraki**.
   
   ![Uygulama URL'sini Yapılandır](./media/active-directory-saas-cisco-webex-tutorial/IC777619.png "uygulama URL'sini yapılandırın")   
   1. İçinde **URL SING** metin kutusuna, Cisco Webex Kiracı URL'nizi yazın (örneğin: *http://contoso.webex.com*).
   2. İçinde **Cisco Webex yanıt URL'si** metin kutusuna, türü, **Cisco Webex AssertionConsumerService URL** (örn: *https://company.webex.com/dispatcher/SAML2AuthService?siteurl=company*).
4. Üzerinde **çoklu oturum açma sırasında Cisco Webex yapılandırma** , sertifika indirmek için sayfasını tıklatın **indirme sertifika**ve ardından sertifika dosyayı bilgisayarınıza kaydedin.
   
   ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-cisco-webex-tutorial/IC777620.png "çoklu oturum açmayı yapılandırın")
5. Farklı web tarayıcısı penceresinde Cisco Webex şirket sitenize yönetici olarak oturum açın.
6. Üstteki menüde tıklatın **Site Yönetimi**.
   
   ![Site Yönetimi](./media/active-directory-saas-cisco-webex-tutorial/IC777621.png "Site Yönetimi")
7. İçinde **sitesi yönetme** 'yi tıklatın **SSO yapılandırma**.
   
   ![SSO yapılandırma](./media/active-directory-saas-cisco-webex-tutorial/IC777622.png "SSO yapılandırma")
8. Federe Web SSO yapılandırma bölümünde aşağıdaki adımları gerçekleştirin:
   
   ![SSO yapılandırma Federasyon](./media/active-directory-saas-cisco-webex-tutorial/IC777623.png "Federasyon SSO yapılandırma")  
   1. Gelen **Federasyon Protokolü** listesinde **SAML 2.0**.
   2. Oluşturma bir **Base-64 kodlanmış** indirilen sertifikanızı dosyasından.  
    >[!TIP]
    >Daha fazla ayrıntı için bkz: [ikili bir sertifika bir metin dosyasına dönüştürme](http://youtu.be/PlgrzUZ-Y1o)
    >  
   3. Base-64 kodlanmış sertifikanızı Not Defteri'nde açın ve içeriğini kopyalayın.
   4. Tıklatın **SAML meta verileri içeri aktarma**ve ardından, base-64 kodlanmış sertifika yapıştırın.
   5. Azure Klasik portalında üzerinde **çoklu oturum açma sırasında Cisco Webex yapılandırma** iletişim sayfası, kopya **veren URL'si** değer ve ardından yapıştırın **veren SAML (IDP kimliği) için** metin kutusu.
   6. Azure Klasik portalında üzerinde **çoklu oturum açma sırasında Cisco Webex yapılandırma** iletişim sayfası, kopya **uzaktan oturum açma URL'si** değer ve ardından yapıştırın **müşteri SSO hizmeti oturum açma URL'si** metin kutusu.
   7. Gelen **NameID biçimi** listesinde **e-posta adresi**.
   8. İçinde **AuthnContextClassRef** metin kutusuna, türü **urn: OASIS: adları: tc: SAML:2.0:ac:classes:Password**.
   9. Azure Klasik portalında üzerinde **çoklu oturum açma sırasında Cisco Webex yapılandırma** iletişim sayfası, kopya **uzak oturum kapatma URL'si** değer ve ardından yapıştırın **müşteri SSO hizmet oturum kapatma URL'si** metin kutusu.
   10. Tıklatın **güncelleştirme**.
9. Azure Klasik portalında üzerinde **çoklu oturum açma sırasında Cisco Webex yapılandırma** iletişim sayfasında, tek oturum açma yapılandırması onay seçin ve ardından **tam**.
   
   ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-cisco-webex-tutorial/IC777624.png "çoklu oturum açmayı yapılandırın")
   
## <a name="configure-user-provisioning"></a>Kullanıcı sağlamayı Yapılandır

Azure AD kullanıcıların Cisco Webex oturum etkinleştirmek için bunların Cisco Webex sağlanmalıdır.  

* Cisco Webex söz konusu olduğunda, sağlama bir el ile bir görevdir.

**Kullanıcı hesaplarını sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum, **Cisco Webex** Kiracı.
2. Git **kullanıcıları yönetme \> kullanıcı ekleme**.
   
   ![Kullanıcıları ekleme](./media/active-directory-saas-cisco-webex-tutorial/IC777625.png "kullanıcı ekleme")
3. Kullanıcı Ekle bölüm üzerinde aşağıdaki adımları gerçekleştirin:
   
   ![Kullanıcı Ekle](./media/active-directory-saas-cisco-webex-tutorial/IC777626.png "Kullanıcı Ekle")   
   1. Olarak **hesap türü**seçin **konak**.
   2. Varolan bir Azure AD kullanıcı bilgilerinin aşağıdaki metin kutularına yazın: **ad, Soyadı**, **kullanıcı adı**, **e-posta**, **parola**, **parolayı onayla**.
   3. **Ekle**'ye tıklayın.

>[!NOTE]
>API sağlama AAD kullanıcı hesaplarına Cisco Webex tarafından sağlanan veya herhangi diğer Cisco Webex kullanıcı hesabı oluşturma araçlarını kullanabilirsiniz. 
> 

## <a name="assign-users"></a>Kullanıcılar atama
Yapılandırmanızı test etmek için uygulama erişimi atayarak kullanarak izin vermek istediğiniz Azure AD kullanıcılarının vermeniz gerekir.

**Cisco Webex kullanıcılara atamak için aşağıdaki adımları gerçekleştirin:**

1. Klasik Azure portalında bir test hesabı oluşturun.
2. Üzerinde **Cisco Webex** uygulama tümleştirme sayfasını tıklatın **kullanıcı atama**.
   
   ![Kullanıcılar atama](./media/active-directory-saas-cisco-webex-tutorial/IC777627.png "kullanıcı atama")
3. Test kullanıcınız seçin, **atamak**ve ardından **Evet** , atama onaylamak için.
   
   ![Evet](./media/active-directory-saas-cisco-webex-tutorial/IC767830.png "Evet")

Çoklu oturum açma ayarlarınızı test etmek isterseniz, erişim paneli açın. Erişim paneli hakkında daha fazla ayrıntı için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md).

