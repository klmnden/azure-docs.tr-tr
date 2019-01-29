---
title: 'Öğretici: IBM Kenexa anket Enterprise ile Azure Active Directory Tümleştirmesi | Microsoft Docs'
description: Azure Active Directory ve IBM Kenexa anket kuruluş arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: c7aac6da-f4bf-419e-9e1a-16b460641a52
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: a3c67fe6e6ed706d3e06a618ded9070f9f69ba19
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55180254"
---
# <a name="tutorial-azure-active-directory-integration-with-ibm-kenexa-survey-enterprise"></a>Öğretici: IBM Kenexa anket Enterprise ile Azure Active Directory Tümleştirmesi

Bu öğreticide, Azure Active Directory (Azure AD) ile IBM Kenexa anket Kurumsal tümleştirme konusunda bilgi edinin.

Azure AD ile IBM Kenexa anket Kurumsal tümleştirme ile aşağıdaki avantajları sağlar:

- IBM Kenexa anket Kurumsal erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına ile çoklu oturum açma (SSO) kullanarak IBM Kenexa anket kuruluş otomatik olarak oturum açın, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınızı tek bir merkezi konumda yönetebilir: Azure portalı.

Azure AD ile bir hizmet (SaaS) uygulamasını tümleştirme olarak yazılım hakkında daha fazla bilgi edinmek istiyorsanız bkz [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile IBM Kenexa anket Enterprise yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir IBM Kenexa anket Kurumsal SSO etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test ettiğinizde, bir üretim ortamında kullanmamanızı öneririz.

Bu öğreticideki adımları test etmek için aşağıdaki önerileri uygulayın:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD SSO bir test ortamında sınayın. Öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

* IBM Kenexa anket Kurumsal galeri ekleme
* Yapılandırma ve Azure AD SSO test etme

## <a name="add-ibm-kenexa-survey-enterprise-from-the-gallery"></a>IBM Kenexa anket Kurumsal Galeriden Ekle
Azure AD'de IBM Kenexa anket Kurumsal tümleştirmesini yapılandırmak için IBM Kenexa anket Kurumsal Galeriden yönetilen SaaS uygulamaları listenize ekleyin.

Galeriden IBM Kenexa anket kuruluş eklemek için aşağıdakileri yapın:

1. İçinde [Azure portalında](https://portal.azure.com), sol bölmede **Azure Active Directory** düğmesi. 

    ![Azure Active Directory düğmesi][1]

1. Seçin **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Bir uygulama eklemek için tıklatın **yeni uygulama** düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **IBM Kenexa anket Kurumsal**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/kenexasurvey-tutorial/tutorial_kenexasurvey_search.png)

1. Sonuç listesinden **IBM Kenexa anket Kurumsal**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde IBM Kenexa anket Enterprise](./media/kenexasurvey-tutorial/tutorial_kenexasurvey_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme
Bu bölümde, yapılandırma ve Azure AD SSO IBM Kenexa anket "Britta Simon." adlı bir test kullanıcı tabanlı Enterprise ile test etme

Çalışmak SSO için Azure AD'de IBM Kenexa anket Kurumsal kullanıcı karşılığı tanımlamak Azure AD'nin gerekiyor. Diğer bir deyişle, Azure AD IBM Kenexa anket kuruluşunuzda bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişki oluşturmanız gerekir.

Bağlantı kurmak için değerini atayın **kullanıcı adı** IBM Kenexa anket Kurumsal değeri olarak **kullanıcıadı** Azure AD'de.

Yapılandırma ve Azure AD SSO IBM Kenexa anket Enterprise ile test etmek için sonraki iki bölümde yapı taşları tamamlayın.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO'yu yapılandırma

Bu bölümde, Azure portalında Azure AD SSO'yu etkinleştirmek ve aşağıdakileri yaparak IBM Kenexa anket Kurumsal uygulamada SSO yapılandırın:

1. Azure portalında, üzerinde **IBM Kenexa anket Kurumsal** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![IBM Kenexa anket Kurumsal yapılandırma tek oturum açma bağlantısı][4]

1. İçinde **çoklu oturum açma** iletişim kutusundaki **modu** kutusunda **SAML tabanlı oturum açma** SSO'yu etkinleştirmek üzere.
 
    ![Çoklu oturum açma iletişim kutusu](./media/kenexasurvey-tutorial/tutorial_kenexasurvey_samlbase.png)

1. İçinde **IBM Kenexa anket Kurumsal etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![IBM Kenexa anket Kurumsal etki alanı ve URL'ler tek oturum açma bilgileri](./media/kenexasurvey-tutorial/tutorial_kenexasurvey_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL aşağıdaki desene sahip: `https://surveys.kenexa.com/<companycode>`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL aşağıdaki desene sahip: `https://surveys.kenexa.com/<companycode>/tools/sso.asp`

    > [!NOTE] 
    > Yukarıdaki değerleri gerçek değildir. Yanıt URL'si ve bunları gerçek tanımlayıcısıyla güncelleştirin. Gerçek değerleri almak için iletişime geçin [IBM Kenexa anket Kurumsal Destek ekibine](https://www.ibm.com/support/home/?lnk=fcw).

1. Altında **SAML imzalama sertifikası**, tıklayın **sertifika (Base64)** ve ardından sertifika dosyasını bilgisayarınıza kaydedin.

    ![Sertifika (Base64) indirme bağlantısı](./media/kenexasurvey-tutorial/tutorial_kenexasurvey_certificate.png) 

    IBM Kenexa anket Kurumsal uygulama, güvenlik onaylama işaretleme dili (SAML) onaylar SAML belirteci öznitelikleri yapılandırmanız için özel öznitelik eşlemelerini eklemeyi gerektiren belirli bir biçimde almak bekliyor. Yanıt kullanıcı tanımlayıcısı talep değerini Kenexa sisteminde yapılandırılmış SSO kimliği ile eşleşmelidir. Kuruluşunuzdaki SSO Internet Veri Birimi Protokolü (IDP) olarak uygun kullanıcı tanımlayıcısı eşlemek için çalışmak [IBM Kenexa anket Kurumsal Destek ekibine](https://www.ibm.com/support/home/?lnk=fcw). 

    Varsayılan olarak, Azure AD Kullanıcı tanımlayıcısı kullanıcı asıl adı (UPN) değerini ayarlar. Bu değeri değiştirebilirsiniz **özniteliği** sekmesinde, aşağıdaki ekran görüntüsünde gösterildiği gibi. Tümleştirme çalışır yalnızca eşlemeyi tamamladıktan sonra doğru.
    
    ![Kullanıcı öznitelikleri iletişim kutusu](./media/kenexasurvey-tutorial/tutorial_attribute.png) 

1. **Kaydet**’e tıklayın.

    ![Yapılandırma çoklu oturum açma düğmesi Kaydet](./media/kenexasurvey-tutorial/tutorial_general_400.png)

1. Açmak için **yapılandırma oturum açma** penceresinin altında **IBM Kenexa anket Kurumsal yapılandırma**, tıklayın **IBM Kenexa anket Kurumsal yapılandırma**. 
 
    ![Yapılandırma IBM Kenexa anket Kurumsal bağlantı](./media/kenexasurvey-tutorial/tutorial_kenexasurvey_configure.png)

1. Kopyalama **oturum kapatma URL'si**, **SAML varlık kimliği**, ve **SAML çoklu oturum açma hizmeti URL'si** değerlerini **hızlı başvuru** bölümü.

1. İçinde **yapılandırma oturum açma** penceresinin altında **hızlı başvuru**, kopyalama **oturum kapatma URL'si**, **SAML varlık kimliği**, ve **SAML Çoklu oturum açma hizmeti URL'si** değerleri.

1. SSO yapılandırma **IBM Kenexa anket Kurumsal** yan, indirilen Gönder **sertifika (Base64)**, **oturum kapatma URL'si**, **SAML varlık kimliği**, ve **SAML çoklu oturum açma hizmeti URL'si** değerler [IBM Kenexa anket Kurumsal Destek ekibine](https://www.ibm.com/support/home/?lnk=fcw).

> [!TIP]
> Bu yönergelerde kısa bir sürümüne başvurabilir [Azure portalında](https://portal.azure.com) uygulamasını ayarladığınız sırada. Uygulamadan ekledikten sonra **Active Directory** > **kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesine ve ardından erişim embedded belgeleri aracılığıyla **yapılandırma** sonunda bölüm. Ekli belge özelliği hakkında daha fazla bilgi edinmek için [Azure AD belgeleri katıştırılmış](https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümde, aşağıdakileri yaparak Azure portalında, Britta Simon test kullanıcısı oluşturun:

![Bir Azure AD test kullanıcısı oluşturma][100]

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/kenexasurvey-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.
    
    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/kenexasurvey-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.
 
    ![Ekle düğmesi](./media/kenexasurvey-tutorial/create_aaduser_03.png) 

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
 
    ![Kullanıcı iletişim kutusu](./media/kenexasurvey-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-an-ibm-kenexa-survey-enterprise-test-user"></a>Bir IBM Kenexa anket Kurumsal test kullanıcısı oluşturma

Bu bölümde, IBM Kenexa anket kuruluşta Britta Simon adlı bir kullanıcı oluşturun. 

IBM Kenexa anket Kurumsal sistemde kullanıcı oluşturun ve SSO kimliği eşleştirebilirsiniz için çalışabileceğiniz [IBM Kenexa anket Kurumsal Destek ekibine](https://www.ibm.com/support/home/?lnk=fcw). Bu SSO kimlik değeri de Azure AD'den kullanıcı tanımlayıcısı değeri eşlenmelidir. Bu varsayılan ayarı değiştirebilirsiniz **özniteliği** sekmesi.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, kullanıcı Britta Simon, IBM Kenexa anket kuruluş erişim vererek Azure SSO kullanacak şekilde etkinleştirin.

![Kullanıcı rolü atayın][200] 

IBM Kenexa anket kuruluş kullanıcı Britta Simon atamak için aşağıdakileri yapın:

1. Azure portalında açın **uygulamaları** görünümü, Git **dizin** görüntülenecek **kurumsal uygulamalar**ve ardından **tüm uygulamalar** .

    !["Kurumsal uygulamalar" ve "Tüm uygulamalar" bağlantıları][201] 

1. İçinde **uygulamaları** listesinden **IBM Kenexa anket Kurumsal**.

    ![Uygulamalar listesinde IBM Kenexa anket Kurumsal bağlantı](./media/kenexasurvey-tutorial/tutorial_kenexasurvey_app.png) 

1. Sol bölmede **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202] 

1. Tıklayın **Ekle** düğmesine ve ardından **atama Ekle** bölmesinde **kullanıcılar ve gruplar**.

    ![Atama Ekle bölmesi][203]

1. İçinde **kullanıcılar ve gruplar** iletişim kutusundaki **kullanıcılar** listesinden **Britta Simon**.

1. İçinde **kullanıcılar ve gruplar** iletişim kutusu, tıklayın **seçin** düğmesi.

1. İçinde **atama Ekle** iletişim kutusu, tıklayın **atama** düğmesi.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD SSO yapılandırmanızı test.

Tıkladığınızda **IBM Kenexa anket Kurumsal** kutucuğuna erişim panelinde, otomatik olarak IBM Kenexa anket Kurumsal uygulamanıza oturum.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/kenexasurvey-tutorial/tutorial_general_01.png
[2]: ./media/kenexasurvey-tutorial/tutorial_general_02.png
[3]: ./media/kenexasurvey-tutorial/tutorial_general_03.png
[4]: ./media/kenexasurvey-tutorial/tutorial_general_04.png

[100]: ./media/kenexasurvey-tutorial/tutorial_general_100.png

[200]: ./media/kenexasurvey-tutorial/tutorial_general_200.png
[201]: ./media/kenexasurvey-tutorial/tutorial_general_201.png
[202]: ./media/kenexasurvey-tutorial/tutorial_general_202.png
[203]: ./media/kenexasurvey-tutorial/tutorial_general_203.png

 
