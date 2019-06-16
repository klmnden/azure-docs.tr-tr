---
title: 'Öğretici: LaunchDarkly ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve LaunchDarkly arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 3f0671bc-f93f-496e-b465-b9ce8c6633fa
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/02/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: de59ce587facb94728e0652f47c7cdfce0ab4388
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67098420"
---
# <a name="tutorial-azure-active-directory-integration-with-launchdarkly"></a>Öğretici: LaunchDarkly ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile LaunchDarkly tümleştirme konusunda bilgi edinin.
LaunchDarkly Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* LaunchDarkly erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak (çoklu oturum açma) LaunchDarkly için kendi Azure AD hesapları ile oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi LaunchDarkly ile yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* LaunchDarkly çoklu oturum açma abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* LaunchDarkly destekler **SP ve IDP** tarafından başlatılan
* LaunchDarkly destekler **zamanında** kullanıcı sağlama

## <a name="adding-launchdarkly-from-the-gallery"></a>LaunchDarkly galeri ekleme

LaunchDarkly tümleştirmesi Azure AD'de yapılandırmak için LaunchDarkly Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden LaunchDarkly eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **LaunchDarkly**seçin **LaunchDarkly** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde LaunchDarkly](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD'ye tek temelinde oturum açma adlı bir test kullanıcısı [uygulama adı] ile test etme **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ve [uygulama adı] ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma [uygulama adı] ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[LaunchDarkly çoklu oturum açmayı yapılandırma](#configure-launchdarkly-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[LaunchDarkly test kullanıcısı oluşturma](#create-launchdarkly-test-user)**  - kullanıcı Azure AD gösterimini bağlı LaunchDarkly Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma [uygulama adı] ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **LaunchDarkly** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** uygulamada yapılandırmak isterseniz, bölümü **IDP** başlatılan modu, aşağıdaki adımları gerçekleştirin:

    ![LaunchDarkly etki alanı ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL yazın: `app.launchdarkly.com`

    b. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://app.launchdarkly.com/trust/saml2/acs/<customers-unique-id>`

    > [!NOTE]
    > Yanıt URL'si değeri gerçek değil. Bu öğreticinin ilerleyen bölümlerinde açıklanan gerçek yanıt URL'si ile değeri güncelleştirir. Uygulamada kullanmak amaçlanıyorsa **IDP** bırakmak için gereksinim duyduğunuz modu **oturum açma URL'si** boş alan, aksi halde oturum açma işlemi başlatmak mümkün olmayacak **IDP**. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. Tıklayın **ek URL'lerini ayarlayın** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:  `https://app.launchdarkly.com`

    ![LaunchDarkly etki alanı ve URL'ler tek oturum açma bilgileri](common/metadata-upload-additional-signon.png)

6. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

7. Üzerinde **LaunchDarkly kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-launchdarkly-single-sign-on"></a>LaunchDarkly tek oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde LaunchDarkly şirket sitenize yönetici olarak oturum.

2. Seçin **hesap ayarları** sol gezinti panelinde.

    ![LaunchDarkly yapılandırma](./media/launchdarkly-tutorial/configure1.png)

3. Tıklayın **güvenlik** sekmesi.

    ![LaunchDarkly yapılandırma](./media/launchdarkly-tutorial/configure2.png)

4. Tıklayın **etkinleştir SSO** ardından **düzenleme SAML yapılandırma**.

    ![LaunchDarkly yapılandırma](./media/launchdarkly-tutorial/configure3.png)

5. Üzerinde **SAML yapılandırmanızı Düzenle** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![LaunchDarkly yapılandırma](./media/launchdarkly-tutorial/configure4.png)

    a. Kopyalama **SAML tüketici hizmeti URL'si** örneğinizin ve yanıt URL'si metin kutusuna yapıştırın **LaunchDarkly etki alanı ve URL'ler** bölümü Azure portalı.

    b. İçinde **oturum açma URL'si** metin kutusu, yapıştırma **oturum açma URL'si** Azure portaldan kopyaladığınız değeri.

    c. İndirilen sertifikanın Not Defteri'ne Azure portalından açın, içeriği kopyalayın ve ardından yapıştırın **X.509 sertifikası** kutusu veya doğrudan yükleyebileceği sertifika tıklayarak **karşıya**.

    d. **Kaydet**'e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü **brittasimon\@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için LaunchDarkly erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **LaunchDarkly**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **LaunchDarkly**.

    ![Uygulamalar listesinde LaunchDarkly bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-launchdarkly-test-user"></a>LaunchDarkly test kullanıcısı oluşturma

Bu bölümün amacı LaunchDarkly Britta Simon adlı bir kullanıcı oluşturmaktır. LaunchDarkly tam zamanında sağlama, varsayılan olarak etkin olan destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Yeni bir kullanıcı, henüz yoksa LaunchDarkly erişme denemesi sırasında oluşturulur.

> [!Note]
> Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [LaunchDarkly istemci Destek ekibine](mailto:support@launchdarkly.com).

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli LaunchDarkly kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama LaunchDarkly için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
