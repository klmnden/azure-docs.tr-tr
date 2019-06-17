---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Perception Amerika Birleşik Devletleri (UltiPro olmayan) | Microsoft Docs'
description: Azure Active Directory ve Perception Amerika Birleşik Devletleri (UltiPro olmayan) arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: b4a8f026-cb5f-41eb-9680-68eddc33565e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/25/2019
ms.author: jeedes
ms.openlocfilehash: e9ba42f780c93486409077383750d0635637e99b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67094847"
---
# <a name="tutorial-azure-active-directory-integration-with-perception-united-states-non-ultipro"></a>Öğretici: Azure Active Directory Tümleştirmesi ile Perception Amerika Birleşik Devletleri (UltiPro olmayan)

Bu öğreticide, Perception Amerika Birleşik Devletleri (UltiPro olmayan) Azure Active Directory (Azure AD) ile tümleştirmeyi öğrenin.
Perception Amerika Birleşik Devletleri (UltiPro olmayan) Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* Kimlerin erişebildiğini Perception Amerika Birleşik Devletleri (UltiPro olmayan) için Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak Perception Amerika Birleşik Devletleri (Non-UltiPro) (çoklu oturum açma) için kendi Azure AD hesapları ile oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Perception Amerika Birleşik Devletleri (UltiPro olmayan) yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Abonelik perception Amerika Birleşik Devletleri (UltiPro olmayan) çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Perception Amerika Birleşik Devletleri (UltiPro olmayan) destekleyen **IDP** tarafından başlatılan

## <a name="adding-perception-united-states-non-ultipro-from-the-gallery"></a>Galeriden Perception Amerika Birleşik Devletleri (UltiPro olmayan) ekleme

Azure AD ile tümleştirme, Perception Amerika Birleşik Devletleri (UltiPro olmayan) yapılandırmak için Perception Amerika Birleşik Devletleri (UltiPro olmayan) eklemek Galeriden yönetilen SaaS uygulamaları listenize gerekir.

**Galeriden Perception Amerika Birleşik Devletleri (UltiPro olmayan) eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Perception Amerika Birleşik Devletleri (UltiPro olmayan)** seçin **Perception Amerika Birleşik Devletleri (UltiPro olmayan)** sonucu panelinden ardından **Ekle** düğme eklemek için uygulama.

     ![Sonuç listesinde perception Amerika Birleşik Devletleri (UltiPro olmayan)](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD'ye tek temelinde oturum açma adlı bir test kullanıcı tarafından Amerika Birleşik Devletleri (UltiPro olmayan) ile test etme **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı tarafından Amerika Birleşik Devletleri (UltiPro olmayan), ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Perception Amerika Birleşik Devletleri (UltiPro olmayan) ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Perception Amerika Birleşik Devletleri (UltiPro olmayan) çoklu oturum açmayı yapılandırma](#configure-perception-united-states-non-ultipro-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Perception Amerika Birleşik Devletleri (UltiPro olmayan) test kullanıcısı oluşturma](#create-perception-united-states-non-ultipro-test-user)**  - içinde Perception kullanıcı Azure AD gösterimini bağlı ABD (UltiPro olmayan) Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma Perception Amerika Birleşik Devletleri (UltiPro olmayan) yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Perception Amerika Birleşik Devletleri (UltiPro olmayan)** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Perception Amerika Birleşik Devletleri (UltiPro olmayan) etki alanı ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL yazın: `https://perception.kanjoya.com/sp`

    b. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://perception.kanjoya.com/sso?idp=<entity_id>`

    c. **Perception Amerika Birleşik Devletleri (UltiPro olmayan)** uygulama gerektirir **Azure AD tanımlayıcısı** gelen erişmenizi sağlayacak değer < entity_id > olarak **Perception Amerika Birleşik Devletleri (ayarlayın Non-UltiPro)** bölümünde URI olarak kodlanamadı için. URI ile kodlanacak değer almak için aşağıdaki bağlantıyı kullanın: **http://www.url-encode-decode.com/** .

    d. URI aldıktan sonra kodlanmış değer birleştirme ile **yanıt URL'si** aşağıdaki - belirtildiği gibi

    `https://perception.kanjoya.com/sso?idp=<URI encooded entity_id>`
    
    e. Yukarıdaki değerinde yapıştırın **yanıt URL'si** metin.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. Üzerinde **Perception Amerika Birleşik Devletleri (UltiPro olmayan) yedekleme kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si   

### <a name="configure-perception-united-states-non-ultipro-single-sign-on"></a>Perception Amerika Birleşik Devletleri (UltiPro olmayan) çoklu oturum açmayı yapılandırın

1. Başka bir tarayıcı penceresinde Perception Amerika Birleşik Devletleri (UltiPro olmayan) şirketinizin sitesi için yönetici olarak oturum açın.

2. Ana araç çubuğunda tıklatın **hesap ayarları**.

    ![Amerika Birleşik Devletleri (UltiPro olmayan) kullanıcı algısı](./media/perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_user.png)

3. Üzerinde **hesap ayarları** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Amerika Birleşik Devletleri (UltiPro olmayan) kullanıcı algısı](./media/perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_account.png)

    a. İçinde **şirket adı** metin adı **şirket**.
    
    b. İçinde **hesap adı** metin adı **hesabı**.

    c. İçinde **varsayılan Yanıtla eposta** metin kutusunda, geçerli **e-posta**.

    d. Seçin **SSO kimlik sağlayıcısı** olarak **SAML 2.0**.

4. Üzerinde **SSO yapılandırma** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Amerika Birleşik Devletleri (UltiPro olmayan) SSOConfig algısı](./media/perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_ssoconfig.png)

    a. Seçin **SAML Nameıd türü** olarak **e-posta**.

    b. İçinde **SSO yapılandırma adı** metin adını yazın, **yapılandırma**.
    
    c. İçinde **kimlik sağlayıcı adı** metin değerini yapıştırın **Azure AD tanımlayıcısı**, hangi Azure Portalı'ndan kopyaladığınız. 

    d. İçinde **SAML etki alanı metin kutusu**, gibi etki alanını girin @contoso.com.

    e. Tıklayarak **yeniden karşıya** yüklenecek **meta veri XML** dosya.

    f. Tıklayın **güncelleştirme**.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü brittasimon@yourcompanydomain.extension. Örneğin, BrittaSimon@contoso.com

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Perception Amerika Birleşik Devletleri (UltiPro olmayan) için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Perception Amerika Birleşik Devletleri (UltiPro olmayan)** .

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Perception Amerika Birleşik Devletleri (UltiPro olmayan)** .

    ![Uygulamalar listesinde Perception Amerika Birleşik Devletleri (UltiPro olmayan) bağlantısı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-perception-united-states-non-ultipro-test-user"></a>Perception Amerika Birleşik Devletleri (UltiPro olmayan) test kullanıcısı oluşturma

Bu bölümde, Britta Simon Perception Amerika Birleşik Devletleri (UltiPro olmayan) adlı bir kullanıcı oluşturun. Çalışmak [Perception Amerika Birleşik Devletleri (UltiPro olmayan) destek ekibi](https://www.ultimatesoftware.com/Contact/ContactUs) Perception Amerika Birleşik Devletleri (UltiPro olmayan) platform kullanıcıları eklemek için.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Perception Amerika Birleşik Devletleri (UltiPro olmayan) kutucuğa tıkladığınızda, size otomatik olarak için Perception SSO'yu ayarlama ABD (UltiPro olmayan) oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

