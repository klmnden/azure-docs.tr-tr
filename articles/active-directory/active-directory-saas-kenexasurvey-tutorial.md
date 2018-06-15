---
title: 'Öğretici: Azure Active Directory Tümleştirme ile IBM Kenexa anket Kurumsal | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile IBM Kenexa anket kuruluş arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: c7aac6da-f4bf-419e-9e1a-16b460641a52
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: 88e0072adeeebaf6c9e964db28a45f6fe038fddf
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34342187"
---
# <a name="tutorial-azure-active-directory-integration-with-ibm-kenexa-survey-enterprise"></a>Öğretici: Azure Active Directory Tümleştirme ile IBM Kenexa anket Enterprise

Bu öğreticide, Azure Active Directory (Azure AD) ile IBM Kenexa anket Kurumsal tümleştirme öğrenin.

IBM Kenexa anket kuruluş Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- IBM Kenexa anket Kurumsal erişimi, Azure AD'de kontrol edebilirsiniz.
- Kullanıcılarınızın kendi Azure AD hesapları ile çoklu oturum açma (SSO) kullanarak IBM Kenexa anket kuruluş için otomatik olarak oturum açmak etkinleştirebilirsiniz.
- Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz: Azure portal.

Azure AD ile hizmet (SaaS) uygulaması tümleştirme olarak yazılım hakkında daha fazla bilgi edinmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme IBM Kenexa anket Enterprise ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- IBM Kenexa anket Kurumsal SSO etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test ettiğinizde, bir üretim ortamında kullanmamanızı öneririz.

Bu öğreticide adımları test etmek için aşağıdaki önerileri uygulayın:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD SSO bir test ortamında test. Öğreticide verilen senaryoda iki ana yapı taşlarını oluşur:

* IBM Kenexa anket Kurumsal Galeriden ekleme
* Yapılandırma ve Azure AD SSO test etme

## <a name="add-ibm-kenexa-survey-enterprise-from-the-gallery"></a>IBM Kenexa anket Kurumsal Galerisi'nden ekleme
Azure AD IBM Kenexa anket Kurumsal tümleştirilmesi yapılandırmak için IBM Kenexa anket Kurumsal Galeriden yönetilen SaaS uygulamaları listenize ekleyin.

IBM Kenexa anket Kurumsal Galeriden eklemek için aşağıdakileri yapın:

1. İçinde [Azure portal](https://portal.azure.com), sol bölmede **Azure Active Directory** düğmesi. 

    ![Azure Active Directory düğmesi][1]

2. Seçin **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Bir uygulama eklemek için tıklatın **yeni uygulama** düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **IBM Kenexa anket Kurumsal**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_search.png)

5. Sonuçlar listesinde **IBM Kenexa anket Kurumsal**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Sonuçlar listesinde IBM Kenexa anket Enterprise](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme
Bu bölümde, yapılandırma ve Azure AD SSO IBM Kenexa anket "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı kuruluş ile test etme

Çalışmak SSO için Azure AD içinde IBM Kenexa anket Kurumsal kullanıcı karşılık gelen tanımlamak Azure AD gerekiyor. Diğer bir deyişle, Azure AD IBM Kenexa anket kuruluşta bir Azure AD kullanıcısının ve ilgili kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Bağlantı ilişkisi kurmak için değeri atayın **kullanıcı adı** kuruluştaki IBM Kenexa anket değeri olarak **kullanıcıadı** Azure AD'de.

Yapılandırmak ve Azure AD IBM Kenexa anket Kurumsal SSO'su sınamak için sonraki iki bölümde yapı taşları tamamlayın.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO yapılandırın

Bu bölümde, Azure portalında Azure AD SSO'yu etkinleştirmek ve aşağıdakileri yaparak IBM Kenexa anket Kurumsal uygulamanızda SSO yapılandırın:

1. Azure portalında üzerinde **IBM Kenexa anket Kurumsal** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Oturum açma tek bağlantı IBM Kenexa anket Kurumsal yapılandırma][4]

2. İçinde **çoklu oturum açma** iletişim kutusunda **modu** kutusunda **SAML tabanlı oturum açma** SSO'yu etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_samlbase.png)

3. İçinde **IBM Kenexa anket kuruluş etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![IBM Kenexa anket kuruluş etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_url.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, aşağıdaki desende bir URL yazın: `https://surveys.kenexa.com/<companycode>`

    b. İçinde **yanıt URL'si** metin kutusuna, aşağıdaki desende bir URL yazın: `https://surveys.kenexa.com/<companycode>/tools/sso.asp`

    > [!NOTE] 
    > Yukarıdaki değerleri gerçek değildir. Gerçek tanımlayıcısı ile güncelleştirin ve URL yanıt. Gerçek değerler elde etmek için ilgili kişi [IBM Kenexa anket kurumsal destek ekibi](https://www.ibm.com/support/home/?lnk=fcw).

4. Altında **SAML imzalama sertifikası**, tıklatın **sertifika (Base64)** ve ardından sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika (Base64) indirme bağlantısı](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_certificate.png) 

    IBM Kenexa anket Kurumsal uygulama, SAML belirteci öznitelikleri yapılandırma için özel öznitelik eşlemelerini eklemenizi gerektirir belirli bir biçimde güvenlik onaylar biçimlendirme dili (SAML) onaylar almak bekliyor. Yanıt kullanıcı tanımlayıcısı talebin değerini Kenexa sistemde yapılandırılmış SSO kimliği eşleşmelidir. Kuruluşunuzdaki SSO Internet Veri Birimi Protokolü (IDP) olarak uygun kullanıcı tanımlayıcısı eşlemek için çalışmak [IBM Kenexa anket kurumsal destek ekibi](https://www.ibm.com/support/home/?lnk=fcw). 

    Varsayılan olarak, Azure AD Kullanıcı tanımlayıcısı kullanıcı asıl adı (UPN) değerini ayarlar. Bu değeri değiştirebilirsiniz **özniteliği** sekmesinde, aşağıdaki ekran görüntüsünde gösterildiği gibi. Tümleştirme çalışır eşleme yalnızca tamamladıktan sonra doğru.
    
    ![Kullanıcı öznitelikleri iletişim kutusu](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_attribute.png)   

5. **Kaydet**’e tıklayın.

    ![Yapılandırma çoklu oturum açma düğmesi Kaydet](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_400.png)

6. Açmak için **yapılandırma oturum açma** penceresi altında **IBM Kenexa anket Kurumsal yapılandırma**, tıklatın **IBM Kenexa anket Kurumsal yapılandırma**. 
 
    ![IBM Kenexa anket Kurumsal yapılandırma bağlantısı](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_configure.png)

7. Kopya **Sign-Out URL**, **SAML varlık kimliği**, ve **SAML çoklu oturum açma hizmet URL'si** gelen değerleri **hızlı başvuru** bölümü.

8. İçinde **yapılandırma oturum açma** penceresi altında **hızlı başvuru**, kopya **Sign-Out URL**, **SAML varlık kimliği**, ve **SAML Çoklu oturum açma hizmet URL'si** değerleri.

9. SSO yapılandırmak için **IBM Kenexa anket Kurumsal** tarafı, indirilen Gönder **sertifika (Base64)**, **Sign-Out URL**, **SAML varlık kimliği**, ve **SAML çoklu oturum açma hizmet URL'si** değerler [IBM Kenexa anket kurumsal destek ekibi](https://www.ibm.com/support/home/?lnk=fcw).

> [!TIP]
> Bu yönergelerde kısa bir sürümünü başvurabilirsiniz [Azure portal](https://portal.azure.com) uygulaması kuruluyor sırada. Uygulamadan ekledikten sonra **Active Directory** > **kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesini tıklatın ve ardından erişim Belge aracılığıyla katıştırılmış **yapılandırma** sonunda bölüm. Embedded belgeler özelliği hakkında daha fazla bilgi için bkz: [Azure AD embedded belgeler](https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümde, aşağıdakileri yaparak Azure portalında test kullanıcısı Britta Simon oluşturun:

![Bir Azure AD test kullanıcısı oluşturma][100]

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.
    
    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.
 
    ![Ekle düğmesi](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_03.png) 

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
 
    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-an-ibm-kenexa-survey-enterprise-test-user"></a>IBM Kenexa anket Kurumsal test kullanıcısı oluşturma

Bu bölümde, IBM Kenexa anket kuruluşta Britta Simon adlı bir kullanıcı oluşturun. 

Kullanıcılar IBM Kenexa anket Kurumsal sistemde oluşturmak ve bunları için SSO kimliği eşlemek için çalışabileceğiniz [IBM Kenexa anket kurumsal destek ekibi](https://www.ibm.com/support/home/?lnk=fcw). Bu SSO kimlik değeri de Azure AD'den kullanıcı tanımlayıcısı değerine eşlenmelidir. Bu varsayılan ayarı değiştirebilirsiniz **özniteliği** sekmesi.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, kullanıcı Britta IBM Kenexa anket kuruluş erişim vererek Azure SSO kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

IBM Kenexa anket kuruluş kullanıcı Britta Simon atamak için aşağıdakileri yapın:

1. Azure portalında açmak **uygulamaları** görünümüne gidin **dizin** görünümü, select **kurumsal uygulamalar**ve ardından **tüm uygulamalar** .

    !["Kurumsal uygulamalar" ve "Tüm uygulamaları" bağlantılar][201] 

2. İçinde **uygulamaları** listesinde **IBM Kenexa anket Kurumsal**.

    ![Uygulamalar listesinde IBM Kenexa anket Kurumsal bağlantı](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_app.png) 

3. Sol bölmede **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202] 

4. Tıklatın **Ekle** düğmesine ve ardından **eklemek atama** bölmesinde, **kullanıcılar ve gruplar**.

    ![Ekleme atama bölmesi][203]

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **kullanıcılar** listesinde **Britta Simon**.

6. İçinde **kullanıcılar ve gruplar** iletişim kutusu, tıklatın **seçin** düğmesi.

7. İçinde **eklemek atama** iletişim kutusu, tıklatın **atamak** düğmesi.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD SSO yapılandırmanızı sınayın.

Tıkladığınızda **IBM Kenexa anket Kurumsal** döşeme erişim panelinde, otomatik olarak IBM Kenexa anket Kurumsal uygulamanıza oturum.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_203.png

 
