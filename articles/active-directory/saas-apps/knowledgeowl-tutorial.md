---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile KnowledgeOwl | Microsoft Docs'
description: Azure Active Directory ve KnowledgeOwl arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 2ae30996-864d-4872-90bc-f770e1ea159a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: c8c86ad6e4b11f21c648083fac35e15eec7658c1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60262583"
---
# <a name="tutorial-azure-active-directory-integration-with-knowledgeowl"></a>Öğretici: KnowledgeOwl ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile KnowledgeOwl tümleştirme konusunda bilgi edinin.

Azure AD ile KnowledgeOwl tümleştirme ile aşağıdaki avantajları sağlar:

- KnowledgeOwl erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan için KnowledgeOwl (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile KnowledgeOwl yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik KnowledgeOwl çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden KnowledgeOwl ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-knowledgeowl-from-the-gallery"></a>Galeriden KnowledgeOwl ekleme
Azure AD'de KnowledgeOwl tümleştirmesini yapılandırmak için KnowledgeOwl Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden KnowledgeOwl eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **KnowledgeOwl**seçin **KnowledgeOwl** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde KnowledgeOwl](./media/knowledgeowl-tutorial/tutorial_knowledgeowl_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı KnowledgeOwl sınayın.

Tek iş için oturum açma için Azure AD ne KnowledgeOwl karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının KnowledgeOwl ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma KnowledgeOwl ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[KnowledgeOwl test kullanıcısı oluşturma](#create-a-knowledgeowl-test-user)**  - kullanıcı Azure AD gösterimini bağlı KnowledgeOwl Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve KnowledgeOwl uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile KnowledgeOwl yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **KnowledgeOwl** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/knowledgeowl-tutorial/tutorial_knowledgeowl_samlbase.png)

1. Üzerinde **KnowledgeOwl etki alanı ve URL'ler** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** başlatılan modu:

    ![KnowledgeOwl etki alanı ve URL'ler tek oturum açma bilgileri](./media/knowledgeowl-tutorial/tutorial_knowledgeowl_url.png)

    a. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna bir URL şu biçimi kullanarak:

    |||
    |-|-|
    | `https://app.knowledgeowl.com/sp`|
    | `https://app.knowledgeowl.com/sp/id/<unique ID>`|
    |||

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak:

    |||
    |-|-|
    | `https://subdomain.knowledgeowl.com/help/saml-login`|
    | `https://subdomain.knowledgeowl.com/docs/saml-login`|
    | `https://subdomain.knowledgeowl.com/home/saml-login`|
    | `https://privatedomain.com/help/saml-login`|
    | `https://privatedomain.com/docs/saml-login`|
    | `https://privatedomain.com/home/saml-login`|
    |||

1. Denetleme **Gelişmiş URL ayarlarını göster** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![KnowledgeOwl etki alanı ve URL'ler tek oturum açma bilgileri](./media/knowledgeowl-tutorial/tutorial_knowledgeowl_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak:
    
    |||
    |-|-|
    | `https://subdomain.knowledgeowl.com/help/saml-login`|
    | `https://subdomain.knowledgeowl.com/docs/saml-login`|
    | `https://subdomain.knowledgeowl.com/home/saml-login`|
    | `https://privatedomain.com/help/saml-login`|
    | `https://privatedomain.com/docs/saml-login`|
    | `https://privatedomain.com/home/saml-login`|
    |||
     
    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değer gerçek tanımlayıcısı, yanıt URL'si ve oturum açma öğreticinin ilerleyen bölümlerinde açıklanan URL'sini güncelleştirmeniz gerekir.

1. KnowledgeOwl uygulama, özel öznitelik eşlemelerini SAML belirteci öznitelikleri yapılandırmanıza ekleyin gerektiren belirli bir biçimde SAML onaylamalarını bekler. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz "**kullanıcı öznitelikleri**" uygulama tümleştirme sayfasında bölümü.

    ![Çoklu oturum açmayı yapılandırın](./media/knowledgeowl-tutorial/attribute.png)

1. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, önceki görüntüde gösterildiği gibi SAML belirteci özniteliği yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Öznitelik Adı | Öznitelik Değeri | Ad alanı|
    | ------------------- | -------------------- | -----|
    | ssoid | User.Mail | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims`|
    
    a. Tıklayın **eklemek agentconfigutil** açmak için **öznitelik Ekle** iletişim.
    
    ![Çoklu oturum açmayı yapılandırın](./media/knowledgeowl-tutorial/tutorial_attribute_04.png)

    ![Çoklu oturum açmayı yapılandırın](./media/knowledgeowl-tutorial/tutorial_attribute_05.png)

    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.
    
    c. Gelen **değer** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    d.From **Namespace** listesinde, ilgili satır için gösterilen ad alanı değeri girin.
    
    e. **Tamam**’a tıklayın.

1. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (ham)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/knowledgeowl-tutorial/tutorial_knowledgeowl_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/knowledgeowl-tutorial/tutorial_general_400.png)
    
1. Üzerinde **KnowledgeOwl yapılandırma** bölümünde **yapılandırma KnowledgeOwl** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![KnowledgeOwl yapılandırma](./media/knowledgeowl-tutorial/tutorial_knowledgeowl_configure.png)

1. Farklı bir web tarayıcı penceresinde KnowledgeOwl şirket sitenize yönetici olarak oturum.

1. Tıklayarak **ayarları** seçip **güvenlik**.

     ![KnowledgeOwl yapılandırma](./media/knowledgeowl-tutorial/configure1.png)

1. Kaydırma **SAML SSO tümleştirme** ve aşağıdaki adımları gerçekleştirin:
    
     ![KnowledgeOwl yapılandırma](./media/knowledgeowl-tutorial/configure2.png)

     a. Seçin **SAML SSO etkinleştirme**.

     b. Kopyalama **SP varlık kimliği** yapıştırın ve değer **tanımlayıcı (varlık kimliği)** içinde **KnowledgeOwl etki alanı ve URL'ler** bölümünde Azure portalında.

     c. Kopyalama **SP oturum açma URL'si** yapıştırın ve değer **oturum açma URL'si ve yanıt URL'si** metin kutuları içinde **KnowledgeOwl etki alanı ve URL'ler** bölümünde Azure portalında.

     d. İçinde **IDP Entityıd** metin kutusu, yapıştırma **SAML varlık kimliği** Azure portaldan kopyaladığınız değeri.

     e. İçinde **Idp'nin oturum açma URL'si** metin kutusu, yapıştırma **SAML çoklu oturum açma hizmeti URL'si** Azure portaldan kopyaladığınız değeri.

     f. İçinde **IDP oturum kapatma URL'si** metin kutusu, yapıştırma **oturum kapatma URL'si** Azure portaldan kopyaladığınız değeri

     g. Azure portalından indirilen sertifika form tıklayarak karşıya **IDP sertifikasını karşıya yükle**.

     h. Tıklayarak **eşleme SAML öznitelikleri** harita öznitelikleri ve aşağıdaki adımları gerçekleştirin:
    
     ![KnowledgeOwl yapılandırma](./media/knowledgeowl-tutorial/configure3.png)

    * Girin `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/ssoid` içine **SSO kimliği** metin kutusu
    * Girin `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` içine **kullanıcı adı/e-posta** metin.
    * Girin `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname` içine **ad** metin.
    * Girin `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname` içine **Soyadı** metin.
    * **Kaydet**’e tıklayın

      i. Sayfanın alt kısmındaki **Kaydet**’e tıklayın.

      ![KnowledgeOwl yapılandırma](./media/knowledgeowl-tutorial/configure4.png)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/knowledgeowl-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/knowledgeowl-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/knowledgeowl-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/knowledgeowl-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-knowledgeowl-test-user"></a>KnowledgeOwl test kullanıcısı oluşturma

Bu bölümün amacı KnowledgeOwl Britta Simon adlı bir kullanıcı oluşturmaktır. KnowledgeOwl tam zamanında sağlama, varsayılan olarak etkin olan destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Yeni bir kullanıcı, henüz yoksa KnowledgeOwl erişme denemesi sırasında oluşturulur.
>[!Note]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [KnowledgeOwl Destek ekibine](mailto:support@knowledgeowl.com).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için KnowledgeOwl erişim vererek Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon KnowledgeOwl için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **KnowledgeOwl**.

    ![Uygulamalar listesinde KnowledgeOwl bağlantı](./media/knowledgeowl-tutorial/tutorial_knowledgeowl_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde KnowledgeOwl kutucuğa tıkladığınızda, otomatik olarak KnowledgeOwl uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/knowledgeowl-tutorial/tutorial_general_01.png
[2]: ./media/knowledgeowl-tutorial/tutorial_general_02.png
[3]: ./media/knowledgeowl-tutorial/tutorial_general_03.png
[4]: ./media/knowledgeowl-tutorial/tutorial_general_04.png

[100]: ./media/knowledgeowl-tutorial/tutorial_general_100.png

[200]: ./media/knowledgeowl-tutorial/tutorial_general_200.png
[201]: ./media/knowledgeowl-tutorial/tutorial_general_201.png
[202]: ./media/knowledgeowl-tutorial/tutorial_general_202.png
[203]: ./media/knowledgeowl-tutorial/tutorial_general_203.png

