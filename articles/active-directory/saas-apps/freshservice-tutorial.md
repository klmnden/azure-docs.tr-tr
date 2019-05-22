---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Freshservice | Microsoft Docs'
description: Azure Active Directory ve Freshservice arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 3dd22b1f-445d-45c6-8eda-30207eb9a1a8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/20/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: c87b23c338788804be22639f73acfb61ce8d6973
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65989406"
---
# <a name="tutorial-integrate-freshservice-with-azure-active-directory"></a>Öğretici: Freshservice Azure Active Directory ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Freshservice tümleştirme öğreneceksiniz. Freshservice Azure AD ile tümleştirdiğinizde, şunları yapabilirsiniz:

* Freshservice erişimi, Azure AD'de denetler.
* Otomatik olarak Freshservice için kendi Azure AD hesapları ile oturum açmış olmasını sağlayın.
* Bir merkezi konumda - Azure portalı hesaplarınızı yönetin.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir aboneliğiniz yoksa, bir aylık ücretsiz deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).
* Freshservice çoklu oturum açma (SSO) abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD SSO bir test ortamında test edin. Freshservice destekler **SP** tarafından başlatılan

## <a name="adding-freshservice-from-the-gallery"></a>Galeriden Freshservice ekleme

Azure AD'de Freshservice tümleştirmesini yapılandırmak için Freshservice Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde seçin **Azure Active Directory** hizmeti.
1. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları**.
1. Yeni bir uygulama eklemek için seçin **yeni uygulama**.
1. İçinde **Galeriden Ekle** bölümüne şunu yazın **Freshservice** arama kutusuna.
1. Seçin **Freshservice** gelen sonuçlar panelinde ve uygulama ekleyin. Uygulama, kiracınıza eklendiği sırada birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Yapılandırma ve Azure AD SSO kullanarak bir test kullanıcı olarak adlandırılır ve Freshservice ile test etme **Britta Simon**. Çalışmak SSO için Freshservice içinde bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişki oluşturmanız gerekir.

Yapılandırma ve Azure AD SSO ile Freshservice sınamak için aşağıdaki yapı taşlarını tamamlayın:

1. **[Azure AD SSO'yu yapılandırma](#configure-azure-ad-sso)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Freshservice SSO'yu yapılandırarak](#configure-freshservice-sso)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Freshservice test kullanıcısı oluşturma](#create-freshservice-test-user)**  - kullanıcı Azure AD gösterimini bağlı Freshservice Britta simon'un bir karşılığı vardır.
6. **[Test SSO](#test-sso)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO'yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek üzere aşağıdaki adımları izleyin.

1. İçinde [Azure portalında](https://portal.azure.com/), **Freshservice** uygulama tümleştirme sayfası, bulma **Yönet** bölümünde ve seçin **çoklu oturum açma**.
1. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** sayfasında **SAML**.
1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında, düzenleme/kalem simgesine tıklayıp **temel SAML yapılandırma** ayarlarını düzenlemek için.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. Üzerinde **temel SAML yapılandırma** sayfasında, aşağıdaki alanlar için değerleri girin:

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<democompany>.freshservice.com`

    b. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<democompany>.freshservice.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL ve tanımlayıcıdır ile güncelleştirin. İlgili kişi [Freshservice istemci Destek ekibine](https://support.freshservice.com/) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

1. İçinde **SAML imzalama sertifikası** bölümünde **Düzenle** açmak için düğmeyi **SAML imzalama sertifikası** iletişim.

    ![SAML imzalama sertifikası Düzenle](common/edit-certificate.png)

1. İçinde **SAML imzalama sertifikası** bölümünde, kopya **parmak izi** ve bilgisayarınıza kaydedin.

    ![Parmak izi değerini kopyalayın](common/copy-thumbprint.png)

1. Üzerinde **Freshservice kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-freshservice-sso"></a>Freshservice SSO yapılandırma

1. Yüklemeniz gerekiyor Freshservice içinde yapılandırmasını otomatik hale getirmenizi **My Apps güvenli oturum açma tarayıcı uzantısı** tıklayarak **uzantıyı yükleme**.

    ![Uygulamaları uzantım](common/install-myappssecure-extension.png)

2. Uzantı tarayıcıya ekledikten sonra tıklayarak **Kurulum Freshservice** Freshservice uygulamaya yönlendirir. Burada, Freshservice oturum açmak için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı otomatik olarak sizin için uygulamayı yapılandırma ve 3-6 adımları otomatik hale getirin.

    ![Kurulum yapılandırması](common/setup-sso.png)

3. Freshservice el ile ayarlamak istiyorsanız, yeni bir web tarayıcı penceresi ve oturum Freshservice şirketinizin sitesi yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

4. Üstteki menüden **yönetici**.

    ![Yönetici](./media/freshservice-tutorial/ic790814.png "yönetici")

5. İçinde **müşteri portalı**, tıklayın **güvenlik**.

    ![Güvenlik](./media/freshservice-tutorial/ic790815.png "güvenlik")

6. İçinde **güvenlik** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum](./media/freshservice-tutorial/ic790816.png "tekli oturum")

    a. Anahtar **tekli oturum**.

    b. Seçin **SAML SSO**.

    c. İçinde **SAML oturum açma URL'si** metin değerini yapıştırın **oturum açma URL'si**, hangi Azure Portalı'ndan kopyaladığınız.

    d. İçinde **oturum kapatma URL'si** metin değerini yapıştırın **oturum kapatma URL'si**, hangi Azure Portalı'ndan kopyaladığınız.

    e. İçinde **güvenlik sertifikası parmak izi** metin kutusu, yapıştırma **parmak İZİ** Azure Portalı'ndan kopyaladığınız sertifika değeri.

    f. **Kaydet**'e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümde, bir test kullanıcısı Britta Simon adlı Azure portalında oluşturacaksınız.

1. Azure Portalı'ndaki sol bölmeden seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.
1. Seçin **yeni kullanıcı** ekranın üstünde.
1. İçinde **kullanıcı** özellikleri, aşağıdaki adımları izleyin:
   1. **Ad** alanına `Britta Simon` girin.  
   1. İçinde **kullanıcı adı** alanına username@companydomain.extension. Örneğin, `BrittaSimon@contoso.com`.
   1. Seçin **Show parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.
   1. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Freshservice erişim vererek Britta Simon tıklatmalarını sağlarsınız.

1. Azure portalında **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.
1. Uygulamalar listesinde **Freshservice**.
1. Uygulamanın genel bakış sayfasında bulma **Yönet** seçin ve bölüm **kullanıcılar ve gruplar**.

   !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

1. Seçin **Kullanıcı Ekle**, ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Kullanıcı ekleme bağlantısı](common/add-assign-user.png)

1. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcılar listesinden ardından **seçin** ekranın alt kısmındaki düğmesi.
1. SAML onaylama işlemi herhangi bir rolü değer de beklediğiniz varsa **rolü Seç** iletişim kutusunda, listeden bir kullanıcı için uygun rolü seçin ve ardından **seçin** ekranın alt kısmındaki düğmesi.
1. İçinde **atama Ekle** iletişim kutusunda, tıklayın **atama** düğmesi.

### <a name="create-freshservice-test-user"></a>Freshservice test kullanıcısı oluşturma

FreshService için oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunların FreshService sağlanması gerekir. FreshService söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açın, **FreshService** yönetici olarak şirketin site.

2. Üstteki menüden **yönetici**.

    ![Yönetici](./media/freshservice-tutorial/ic790814.png "yönetici")

3. İçinde **kullanıcı yönetimi** bölümünde **isteyenlere**.

    ![Kararınız istekte](./media/freshservice-tutorial/ic790818.png "isteyenlere")

4. Tıklayın **yeni bir istek sahibi**.

    ![Yeni isteyenlere](./media/freshservice-tutorial/ic790819.png "yeni isteyenlere")

5. İçinde **yeni bir istek sahibi** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Yeni Talep sahibinin](./media/freshservice-tutorial/ic790820.png "yeni bir istek sahibi")  

    a. Girin **ad** ve **e-posta** istediğiniz ilgili metin kutularına zbilgisayarlar geçerli bir Azure Active Directory hesabının öznitelikleri.

    b. **Kaydet**’e tıklayın.

    > [!NOTE]
    > Azure Active Directory hesap sahibinin etkin hale gelir önce hesabı onaylamak için bir bağlantı içeren bir e-posta alır.
    >  

> [!NOTE]
> Herhangi diğer FreshService kullanıcı hesabı oluşturma araçları kullanabilir veya API'leri için AAD kullanıcı hesapları sağlamak FreshService tarafından sağlanan.

### <a name="test-sso"></a>Test SSO

Erişim Paneli'nde Freshservice kutucuğu seçtiğinizde, otomatik olarak SSO'yu ayarlama Freshservice için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
