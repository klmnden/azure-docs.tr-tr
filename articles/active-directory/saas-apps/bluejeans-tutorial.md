---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile BlueJeans | Microsoft Docs'
description: Azure Active Directory ve BlueJeans arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: dfc634fd-1b55-4ba8-94a8-b8288429b6a9
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/16/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 39e9f52948d035c72a6a019558915d8c92ceebeb
ms.sourcegitcommit: e6d53649bfb37d01335b6bcfb9de88ac50af23bd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65463506"
---
# <a name="tutorial-azure-active-directory-integration-with-bluejeans"></a>Öğretici: BlueJeans ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile BlueJeans tümleştirme konusunda bilgi edinin.
Azure AD ile BlueJeans tümleştirme ile aşağıdaki avantajları sağlar:

* BlueJeans erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak (çoklu oturum açma) BlueJeans için kendi Azure AD hesapları ile oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile BlueJeans yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa alabileceğiniz bir [ücretsiz hesap](https://azure.microsoft.com/free/)
* BlueJeans tek oturum açma etkin abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* BlueJeans destekler **SP** tarafından başlatılan

* BlueJeans destekler [ **otomatik** kullanıcı sağlama](bluejeans-provisioning-tutorial.md)

## <a name="adding-bluejeans-from-the-gallery"></a>Galeriden BlueJeans ekleme

Azure AD'de BlueJeans tümleştirmesini yapılandırmak için BlueJeans Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden BlueJeans eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **BlueJeans**seçin **BlueJeans** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde blueJeans](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma BlueJeans adlı bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının BlueJeans ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma BlueJeans ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[BlueJeans çoklu oturum açmayı yapılandırma](#configure-bluejeans-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[BlueJeans test kullanıcısı oluşturma](#create-bluejeans-test-user)**  - kullanıcı Azure AD gösterimini bağlı Britta simon'un BlueJeans içinde bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile BlueJeans yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **BlueJeans** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](media/bluejeans-tutorial/edit-urls-bluejeans.png)

4. İçinde **temel SAML yapılandırma** iletişim kutusunda aşağıdaki değerleri girin:

    ![BlueJeans etki alanı ve URL'ler tek oturum açma bilgileri](media/bluejeans-tutorial/tutorial_bluejeans-basic-configuration.png)

   - İçinde **tanımlayıcı** metin kutusunda, aşağıdaki komutu yazın: `https://samlsp.bluejeans.com`
    
   - İçinde **oturum açma URL'si** metin kutusunda, size BlueJeans tarafından sağlanan giriş sayfası URL'sini yazın (Bu değer almak için sizinle iletişim [BlueJeans istemci Destek ekibine](https://support.bluejeans.com/contact)): `https://<companyname>.bluejeans.com`
    
   - **Kaydet**’e tıklayın.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. Üzerinde **BlueJeans kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-bluejeans-single-sign-on"></a>BlueJeans çoklu oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde oturum açın, **BlueJeans** yönetici olarak şirketin site.

2. Git **yönetici \> grup ayarları \> güvenlik**.

    ![Yönetici](./media/bluejeans-tutorial/ic785868.png "yönetici")

3. İçinde **güvenlik** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![SAML çoklu oturum açma](./media/bluejeans-tutorial/ic785869.png "SAML çoklu oturum açma")

    a. Seçin **SAML çoklu oturum açma**.

    b. Seçin **otomatik sağlamayı etkinleştirme**.

4. Aşağıdaki adımlara geçin:

    ![Sertifika yolu](./media/bluejeans-tutorial/ic785870.png "sertifika yolu")

    a. Tıklayın **Dosya Seç**, Azure portalından indirdiğiniz base-64 kodlanmış sertifikasını karşıya yükleyin.

    b. İçinde **oturum açma URL'si** metin değerini yapıştırın **oturum açma URL'si** , Azure Portalı'ndan kopyaladığınız.

    c. İçinde **parola değişikliği URL'si** metin değerini yapıştırın **parola URL'yi Değiştir** , Azure Portalı'ndan kopyaladığınız.

    d. İçinde **oturum kapatma URL'si** metin değerini yapıştırın **oturum kapatma URL'si** , Azure Portalı'ndan kopyaladığınız.

5. Aşağıdaki adımlara geçin:

    ![Değişiklikleri kaydetmek](./media/bluejeans-tutorial/ic785874.png "Değişiklikleri Kaydet")

    a. İçinde **kullanıcı kimliği** metin kutusuna `https://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.

    b. İçinde **e-posta** metin kutusuna `https://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.

    c. Tıklayın **değişiklikleri kaydetme**.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü `brittasimon\@yourcompanydomain.extension`. Örneğin, BrittaSimon@contoso.com.

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için BlueJeans erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **BlueJeans**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **BlueJeans**.

    ![Uygulamalar listesinde BlueJeans bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-bluejeans-test-user"></a>BlueJeans test kullanıcısı oluşturma

Bu bölümün amacı BlueJeans Britta Simon adlı bir kullanıcı oluşturmaktır. BlueJeans destekler otomatik kullanıcı hazırlama, varsayılan olarak etkin olduğu. Daha fazla ayrıntı bulabilirsiniz [burada](bluejeans-provisioning-tutorial.md) otomatik kullanıcı sağlamayı yapılandırma.

**Kullanıcı el ile oluşturmanız gerekiyorsa, aşağıdaki adımları uygulayın:**

1. Oturum açın, **BlueJeans** yönetici olarak şirketin site.

2. Git **yönetici \> Kullanıcıları Yönet \> Kullanıcı Ekle**.

    ![Yönetici](./media/bluejeans-tutorial/ic785877.png "yönetici")

    > [!IMPORTANT]
    > **Kullanıcı Ekle** sekmesi kullanılabilir yalnızca ise, **SORARAK sekmesini**, **otomatik sağlamayı etkinleştirme** olarak işaretli değildir.

3. İçinde **Kullanıcı Ekle** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı ekleme](./media/bluejeans-tutorial/ic785886.png "kullanıcı ekleme")

    a. İçinde **ad** metin kutusunda, gibi kullanıcı adını girin **Britta**.

    b. İçinde **Soyadı** metin kutusunda, son kullanıcı gibi adını **Simon**.

    c. İçinde **BlueJeans kullanıcıadı çekme** metin kutusunda, gibi kullanıcının kullanıcı adı girin **Brittasimon**

    d. İçinde **parola oluşturma** metin kutusunda, parolanızı girin.

    e. İçinde **şirket** metin kutusunda, şirketinizin girin.

    f. İçinde **e-posta adresi** metin kutusuna, kullanıcının gibi e-posta girin `brittasimon\@contoso.com`.

    g. İçinde **BlueJeans toplantı I.D oluşturma** metin kutusunda, toplantı kimliğinizi girin

    h. İçinde **Moderator geçiş kodu çekme** metin kutusunda, geçiş kodunuzu girin.

    i. Tıklayın **devam**.

    ![Kullanıcı ekleme](./media/bluejeans-tutorial/ic785887.png "kullanıcı ekleme")

    J. Tıklayın **Kullanıcı Ekle**.

> [!NOTE]
> Herhangi diğer BlueJeans kullanıcı hesabı oluşturma araçları kullanabilir veya API Azure AD'ye kullanıcı hesapları sağlamak için BlueJeans tarafından sağlanan.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli BlueJeans kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama BlueJeans için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
