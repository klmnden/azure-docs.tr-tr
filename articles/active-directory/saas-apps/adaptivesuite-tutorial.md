---
title: 'Öğretici: Uyarlamalı Insights ile Azure Active Directory Tümleştirmesi | Microsoft Docs'
description: Azure Active Directory ve Uyarlamalı Insights arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 13af9d00-116a-41b8-8ca0-4870b31e224c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/17/2019
ms.author: jeedes
ms.openlocfilehash: c217663c5752907e0b3d6372d4522f6aba982b3d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67107400"
---
# <a name="tutorial-azure-active-directory-integration-with-adaptive-insights"></a>Öğretici: Uyarlamalı Insights ile Azure Active Directory Tümleştirmesi

Bu öğreticide, Uyarlamalı Insights Azure Active Directory (Azure AD) ile tümleştirmeyi öğrenin.
Uyarlamalı Insights'ı Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* Uyarlamalı Insights erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak Uyarlamalı Insights'a (çoklu oturum açma) oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Uyarlamalı Insights ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Uyarlamalı Insights tek oturum açma etkin abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Uyarlamalı Insights destekler **IDP** tarafından başlatılan

## <a name="adding-adaptive-insights-from-the-gallery"></a>Galeriden Uyarlamalı Insights ekleme

Azure AD Uyarlamalı Öngörüler tümleştirmesini yapılandırmak için Uyarlamalı Insights Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Uyarlamalı Insights eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Uyarlamalı Insights**seçin **Uyarlamalı Insights** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde Uyarlamalı öngörüleri](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açmayı test etme Uyarlamalı adlı bir test kullanıcı dayalı öngörülerle **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ve ilgili kullanıcı Uyarlamalı ınsights arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Uyarlamalı öngörülerle sınamak için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Uyarlamalı Insights çoklu oturum açmayı yapılandırma](#configure-adaptive-insights-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Uyarlamalı Insights test kullanıcısı oluşturma](#create-adaptive-insights-test-user)**  - kullanıcı Azure AD gösterimini bağlı Uyarlamalı ınsights'ta Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma Uyarlamalı Insights ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Uyarlamalı Insights** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açma bilgileri Uyarlamalı Insights etki alanı ve URL'ler](common/idp-intiated.png)

    a. İçinde **tanımlayıcı** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://login.adaptiveinsights.com:443/samlsso/<unique-id>`

    b. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://login.adaptiveinsights.com:443/samlsso/<unique-id>`

    > [!NOTE]
    > Uyarlamalı Insights'tan 's tanımlayıcı (varlık kimliği) ve yanıt URL'si değerleri alabilirsiniz **SAML SSO ayarlarını** sayfası.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. Üzerinde **Uyarlamalı ınsights'ı ayarlarken** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-adaptive-insights-single-sign-on"></a>Uyarlamalı Insights çoklu oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde Uyarlamalı Insights şirketinizin sitesi için bir yönetici olarak oturum açın.

2. Git **yönetici**.

    ![Yönetici](./media/adaptivesuite-tutorial/ic805644.png "yönetici")

3. İçinde **kullanıcıları ve rolleri** bölümünde **SAML SSO ayarlarını Yönet**.

    ![SAML SSO ayarlarını yönetme](./media/adaptivesuite-tutorial/ic805645.png "SAML SSO ayarlarını yönetme")

4. Üzerinde **SAML SSO ayarlarını** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![SAML SSO ayarlarını](./media/adaptivesuite-tutorial/ic805646.png "SAML SSO ayarları")

    a. İçinde **kimlik sağlayıcı adı** metin yapılandırmanız için bir ad yazın.

    b. Yapıştırma **Azure Ad tanımlayıcısı** değeri kopyalanan Azure portalından **kimlik sağlayıcısı varlık kimliği** metin.

    c. Yapıştırma **oturum açma URL'si** değeri kopyalanan Azure portalından **kimlik sağlayıcısı SSO URL** metin.

    d. Yapıştırma **oturum kapatma URL'si** değeri kopyalanan Azure portalından **özel oturum kapatma URL'si** metin.

    e. İndirilen sertifikanızı karşıya yüklemek için tıklayın **dosya**.

    f. İçin şunu seçin:

     * **SAML kullanıcı kimliği**seçin **Uyarlamalı Insights kullanıcının adını**.

     * **SAML kullanıcı kimliği konumu**seçin **kullanıcı kimliği, Nameıd konusunda**.

     * **SAML Nameıd biçimi**seçin **e-posta adresi**.

     * **SAML'yi etkinleştir**seçin **izin SAML SSO ve doğrudan Uyarlamalı Insights oturum açma**.

    g. Kopyalama **Uyarlamalı Insights SSO URL** ve yapıştırmak **tanımlayıcı (varlık kimliği)** ve **yanıt URL'si** metin kutuları içinde **Uyarlamalı Insights etki alanı ve URL'ler** bölümünde Azure portalında.

    h. **Kaydet**’e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü 'brittasimon@yourcompanydomain.extension. Örneğin, BrittaSimon@contoso.com.

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Uyarlamalı Öngörülere erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Uyarlamalı Insights**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Uyarlamalı Insights**.

    ![Uygulamalar listesinde Uyarlamalı Insights bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-adaptive-insights-test-user"></a>Uyarlamalı Insights test kullanıcısı oluşturma

Uyarlamalı Insights'a oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunlar Uyarlamalı Öngörüler sağlanması gerekir. Uyarlamalı Insights söz konusu olduğunda, sağlama, elle bir görevin.

**Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açın, **Uyarlamalı Insights** yönetici olarak şirketin site.

2. Git **yönetici**.

   ![Yönetici](./media/adaptivesuite-tutorial/IC805644.png "yönetici")

3. İçinde **kullanıcıları ve rolleri** bölümünde **Kullanıcı Ekle**.

   ![Kullanıcı ekleme](./media/adaptivesuite-tutorial/IC805648.png "kullanıcı ekleme")

4. İçinde **yeni kullanıcı** bölümünde, aşağıdaki adımları gerçekleştirin:

   ![Gönderme](./media/adaptivesuite-tutorial/IC805649.png "gönderin")

   a. Tür **adı**, **oturum açma**, **e-posta**, **parola** ilgili zbilgisayarlar istediğiniz geçerli bir Azure Active Directory kullanıcısı metin kutuları.

   b. Seçin bir **rol**.

   c. **Gönder**'e tıklayın.

> [!NOTE]
> Herhangi diğer Uyarlamalı Insights kullanıcı hesabı oluşturma araçları kullanabilir veya API'leri için AAD kullanıcı hesapları sağlamak Uyarlamalı Insights tarafından sağlanan.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Uyarlamalı Insights kutucuğa tıkladığınızda, size otomatik olarak Uyarlamalı SSO'yu ayarlama öngörüleri için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)