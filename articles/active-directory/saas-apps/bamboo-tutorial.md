---
title: 'Öğretici: GmbH çözünürlüğün Bamboo için SAML SSO ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Çoklu oturum açma SAML SSO Bamboo için Azure Active Directory arasındaki GmbH çözünürlüğün yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: f00160c7-f4cc-43bf-af18-f04168d3767c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/16/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: e25744b82b05c29b2b5615fd62a3b146ee60f45e
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59699390"
---
# <a name="tutorial-azure-active-directory-integration-with-saml-sso-for-bamboo-by-resolution-gmbh"></a>Öğretici: GmbH çözünürlüğün Bamboo için SAML SSO ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile GmbH çözünürlüğün Bamboo için SAML SSO tümleştirme konusunda bilgi edinin.
SAML SSO için Bamboo çözünürlüğün GmbH Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* SAML SSO için Bamboo GmbH çözünürlüğün erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak Bamboo için SAML SSO (çoklu oturum açma) GmbH çözünürlüğüyle kendi Azure AD hesapları ile oturum, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi GmbH çözünürlüğüyle Bamboo için SAML SSO ile yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa alabileceğiniz bir [ücretsiz hesap](https://azure.microsoft.com/free/)
* Bamboo çözünürlüğün GmbH çoklu oturum açma için SAML SSO abonelik etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Bamboo GmbH destekler çözünürlüğün için SAML SSO **SP ve IDP** tarafından başlatılan
* Bamboo GmbH destekler çözünürlüğün için SAML SSO **zamanında** kullanıcı sağlama

## <a name="adding-saml-sso-for-bamboo-by-resolution-gmbh-from-the-gallery"></a>Bamboo için SAML SSO GmbH çözünürlüğüyle galeri ekleme

Bamboo için SAML SSO, Azure AD'de tümleştirmesini GmbH çözünürlüğün yapılandırmak için SAML SSO için Bamboo GmbH çözünürlüğüyle Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**SAML SSO için Bamboo GmbH çözünürlüğüyle Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **GmbH çözünürlüğün Bamboo için SAML SSO**seçin **GmbH çözünürlüğün Bamboo için SAML SSO** sonucu panelinden ardından **Ekle** eklemek için Ekle düğmesine uygulama.

    ![SAML SSO için sonuç listesinde GmbH çözünürlüğün Bamboo](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma SAML SSO ile Bamboo GmbH çözünürlüğün adlı bir test kullanıcı tabanlı için test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ve SAML SSO Bamboo için ilgili kullanıcı çözünürlüğün arasında bir bağlantı ilişki GmbH kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma SAML SSO için Bamboo ile GmbH çözünürlüğün sınamak için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[SAML SSO için Bamboo çözünürlüğüyle GmbH çoklu oturum açmayı yapılandırma](#configure-saml-sso-for-bamboo-by-resolution-gmbh-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[SAML SSO için çözüm GmbH test kullanıcısı tarafından Bamboo oluşturma](#create-saml-sso-for-bamboo-by-resolution-gmbh-test-user)**  - kullanıcı Azure AD gösterimini bağlı GmbH çözünürlüğün Bamboo için SAML SSO içinde bir karşılığı Britta simon'un sağlamak için.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma SAML SSO için Bamboo ile GmbH çözünürlüğüyle yapılandırmak için aşağıdaki adımları uygulayın:

1. İçinde [Azure portalında](https://portal.azure.com/), **GmbH çözünürlüğün Bamboo için SAML SSO** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** uygulamada yapılandırmak isterseniz, bölümü **IDP** başlatılan modu, aşağıdaki adımları gerçekleştirin:

    ![Oturum açma bilgileri tek bir SAML SSO için Bamboo çözünürlüğün GmbH etki alanı ve URL'ler](common/idp-intiated.png)

    a. İçinde **tanımlayıcı** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<server-base-url>/plugins/servlet/samlsso`

    b. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<server-base-url>/plugins/servlet/samlsso`

5. Tıklayın **ek URL'lerini ayarlayın** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![Oturum açma bilgileri tek bir SAML SSO için Bamboo çözünürlüğün GmbH etki alanı ve URL'ler](common/metadata-upload-additional-signon.png)

    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:  `https://<server-base-url>/plugins/servlet/samlsso`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. İlgili kişi [SAML SSO Bamboo çözünürlüğün GmbH istemci için Destek ekibine](https://marketplace.atlassian.com/plugins/com.resolution.atlasplugins.samlsso-bamboo/server/support) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

6. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

7. Üzerinde **GmbH çözünürlüğüyle Bamboo için SAML SSO ayarlama** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-saml-sso-for-bamboo-by-resolution-gmbh-single-sign-on"></a>SAML SSO için Bamboo çözünürlüğüyle GmbH çoklu oturum açmayı yapılandırma

1. SAML SSO için çözüm GmbH şirket site tarafından Bamboo için yönetici olarak oturum.

1. Ana araç çubuğunun sağ tarafında tıklayın **ayarları** > **eklentileri**.

    ![Ayarları](./media/bamboo-tutorial/tutorial_bamboo_setings.png)

1. Güvenlik bölümüne gidin, tıklayarak **SAML SingleSignOn** menü çubuğu üzerinde.

    ![Samlsingle](./media/bamboo-tutorial/tutorial_bamboo_samlsingle.png)

1. Üzerinde **SAML SIngleSignOn eklentisi yapılandırma sayfası**, tıklayın **IDP ekleme**.

    ![IDP Ekle](./media/bamboo-tutorial/tutorial_bamboo_addidp.png)

1. Üzerinde **SAML kimlik sağlayıcınızı seçin** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Kimlik sağlayıcısı](./media/bamboo-tutorial/tutorial_bamboo_identityprovider.png)

    a. Seçin **IDP türü** olarak **AZURE AD'ye**.

    b. İçinde **adı** metin kutusuna bir ad yazın.

    c. İçinde **açıklama** metin kutusuna bir açıklama yazın.

    d. **İleri**’ye tıklayın.

1. Üzerinde **kimlik sağlayıcı Yapılandırması** sayfasında **sonraki**.

    ![Kimlik yapılandırma](./media/bamboo-tutorial/tutorial_bamboo_identityconfig.png)

1. Üzerinde **SAML IDP meta verileri içeri aktarma** sayfasında, **yük dosyası** yüklenecek **meta veri XML** Azure portalından indirdiğiniz dosyası.

    ![İdpmetadata](./media/bamboo-tutorial/tutorial_bamboo_idpmetadata.png)

1. **İleri**’ye tıklayın.

1. **Ayarları kaydet**’e tıklayın.

    ![Kaydetme](./media/bamboo-tutorial/tutorial_bamboo_save.png)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü `brittasimon@yourcompanydomain.extension`. Örneğin, BrittaSimon@contoso.com

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma GmbH çözünürlüğüyle Bamboo için SAML SSO için erişim vererek kullanmak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **GmbH çözünürlüğün Bamboo için SAML SSO**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **GmbH çözünürlüğün Bamboo için SAML SSO**.

    ![SAML SSO uygulamaları listesinde çözümleme GmbH bağlantısıyla Bamboo için](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-saml-sso-for-bamboo-by-resolution-gmbh-test-user"></a>SAML SSO için çözüm GmbH test kullanıcısı tarafından Bamboo oluşturma

Bu bölümün amacı, Britta Simon SAML SSO için Bamboo GmbH çözünürlüğüyle adlı bir kullanıcı oluşturmaktır. SAML SSO GmbH çözünürlüğün Bamboo, just-ın-time sağlamayı destekler ve ayrıca kullanıcılar el ile oluşturulabilir başvurun [SAML SSO Bamboo çözünürlüğün GmbH istemci için Destek ekibine](https://marketplace.atlassian.com/plugins/com.resolution.atlasplugins.samlsso-bamboo/server/support) ihtiyacınıza göre.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Bamboo için SAML SSO tarafından erişim panelinde çözümleme GmbH kutucuk'ye tıkladığınızda, otomatik olarak Bamboo için SAML SSO için SSO'yu ayarlama çözümleme GmbH tarafından oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [ SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
