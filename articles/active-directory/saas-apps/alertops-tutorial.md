---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile AlertOps | Microsoft Docs'
description: Azure Active Directory ve AlertOps arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: 3db13ed4-35c2-4b1e-bed8-9b5977061f93
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/20/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 124f69b3fd6944d0a6e3814f1bbfa5594bcd95cb
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65988381"
---
# <a name="tutorial-integrate-alertops-with-azure-active-directory"></a>Öğretici: AlertOps Azure Active Directory ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile AlertOps tümleştirme öğreneceksiniz. AlertOps Azure AD ile tümleştirdiğinizde, şunları yapabilirsiniz:

* AlertOps erişimi, Azure AD'de denetler.
* Otomatik olarak AlertOps için kendi Azure AD hesapları ile oturum açmış olmasını sağlayın.
* Bir merkezi konumda - Azure portalı hesaplarınızı yönetin.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir aboneliğiniz yoksa, alabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free/).
* AlertOps çoklu oturum açma (SSO) abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD SSO bir test ortamında test edin. AlertOps destekler **SP ve IDP** SSO başlattı.

## <a name="adding-alertops-from-the-gallery"></a>Galeriden AlertOps ekleme

Azure AD'de AlertOps tümleştirmesini yapılandırmak için AlertOps Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde seçin **Azure Active Directory** hizmeti.
1. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları**.
1. Yeni bir uygulama eklemek için seçin **yeni uygulama**.
1. İçinde **Galeriden Ekle** bölümüne şunu yazın **AlertOps** arama kutusuna.
1. Seçin **AlertOps** gelen sonuçlar panelinde ve uygulama ekleyin. Uygulama, kiracınıza eklendiği sırada birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Yapılandırma ve Azure AD SSO kullanarak adlı bir test kullanıcı AlertOps ile test etme **Britta Simon**. Çalışmak SSO için AlertOps içinde bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişki oluşturmanız gerekir.

Yapılandırma ve Azure AD SSO ile AlertOps sınamak için aşağıdaki yapı taşlarını tamamlayın:

1. **[Azure AD SSO'yu yapılandırma](#configure-azure-ad-sso)**  kullanıcılarınız bu özelliği kullanmak etkinleştirmek için.
2. **[AlertOps yapılandırma](#configure-alertops)**  uygulama tarafında SSO ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  Britta Simon, Azure AD çoklu oturum açma kullanmak üzere etkinleştirmek için.
5. **[AlertOps test kullanıcısı oluşturma](#create-alertops-test-user)**  bir karşılığı Britta simon'un kullanıcı Azure AD gösterimini bağlı AlertOps sağlamak için.
6. **[Test SSO](#test-sso)**  yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO'yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek üzere aşağıdaki adımları izleyin.

1. İçinde [Azure portalında](https://portal.azure.com/), **AlertOps** uygulama tümleştirme sayfası, bulma **Yönet** bölümünde ve seçin **çoklu oturum açma**.
1. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** sayfasında **SAML**.
1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında, düzenleme/kalem simgesine tıklayıp **temel SAML yapılandırma** ayarlarını düzenlemek için.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. Üzerinde **temel SAML yapılandırma** uygulamada yapılandırmak isterseniz, bölümü **IDP** başlatılan modu, aşağıdaki adımları gerçekleştirin:

    1. İçinde **tanımlayıcı** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<SUBDOMAIN>.alertops.com`

    1. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<SUBDOMAIN>.alertops.com/login.aspx`

1. Tıklayın **ek URL'lerini ayarlayın** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:  `https://<SUBDOMAIN>.alertops.com/login.aspx`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. İlgili kişi [AlertOps istemci Destek ekibine](mailto:support@alertops.com) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde, bulma **sertifika (Base64)** seçip **indirin** sertifikayı indirin ve bilgisayarınıza kaydedin.

   ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. Üzerinde **AlertOps kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

   ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

### <a name="configure-alertops"></a>AlertOps yapılandırın

1. Yüklemeniz gerekiyor AlertOps içinde yapılandırmasını otomatik hale getirmenizi **My Apps güvenli oturum açma tarayıcı uzantısı** tıklayarak **uzantıyı yükleme**.

    ![Uygulamaları uzantım](common/install-myappssecure-extension.png)

2. Uzantı tarayıcıya ekledikten sonra tıklayarak **Kurulum AlertOps** AlertOps uygulamaya yönlendirir. Burada, AlertOps oturum açmak için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı otomatik olarak sizin için uygulamayı yapılandırma ve 3-5 adımlarını otomatik hale getirin.

    ![Kurulum yapılandırması](common/setup-sso.png)

3. AlertOps el ile ayarlamak istiyorsanız, yeni bir web tarayıcı penceresi açın ve AlertOps şirketinizin sitesi yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

4. Tıklayarak **hesap ayarları** sol gezinti panelinde.

    ![AlertOps yapılandırma](./media/alertops-tutorial/configure1.png)

5. Üzerinde **Abonelik ayarları** seçin sayfasında **SSO** ve aşağıdaki adımları gerçekleştirin:

    ![AlertOps yapılandırma](./media/alertops-tutorial/configure2.png)

    a. Seçin **kullanım tek Sign-On(SSO)** onay kutusu.

    b. Seçin **Azure Active Directory** olarak bir **SSO sağlayıcısı** açılır listeden.

    c. İçinde **veren URL'si** metin kullandığınız tanımlayıcı değerini kullanmak **temel SAML yapılandırma** bölümünde Azure portalında.

    d. İçinde **SAML uç nokta URL'si** metin kutusu, yapıştırma **oturum açma URL'si** Azure portaldan kopyaladığınız değeri.

    e. İçinde **SLO uç nokta URL'si** metin kutusu, yapıştırma **oturum açma URL'si** Azure portaldan kopyaladığınız değeri.

    f. Seçin **SHA256** olarak bir **SAML imza algoritması** açılır listeden.

    g. İndirilen Certificate(Base64) dosyanızı Not Defteri'nde açın. Bu içeriği, panoya kopyalayın ve X.509 sertifikası metin kutusuna yapıştırın.

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

Bu bölümde, Azure çoklu oturum açma kullanmak için AlertOps erişim vererek Britta Simon tıklatmalarını sağlarsınız.

1. Azure portalında **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.
1. Uygulamalar listesinde **AlertOps**.
1. Uygulamanın genel bakış sayfasında bulma **Yönet** seçin ve bölüm **kullanıcılar ve gruplar**.

   !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

1. Seçin **Kullanıcı Ekle**, ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Kullanıcı ekleme bağlantısı](common/add-assign-user.png)

1. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcılar listesinden ardından **seçin** ekranın alt kısmındaki düğmesi.
1. SAML onaylama işlemi herhangi bir rolü değer de beklediğiniz varsa **rolü Seç** iletişim kutusunda, listeden bir kullanıcı için uygun rolü seçin ve ardından **seçin** ekranın alt kısmındaki düğmesi.
1. İçinde **atama Ekle** iletişim kutusunda, tıklayın **atama** düğmesi.

### <a name="create-alertops-test-user"></a>AlertOps test kullanıcısı oluşturma

1. Bir farklı bir tarayıcı penceresinde AlertOps şirketinizin sitesi için yönetici olarak oturum açın.

2. Tıklayarak **kullanıcılar** sol gezinti panelinde.

    ![AlertOps yapılandırma](./media/alertops-tutorial/user1.png)

3. Seçin **kullanıcı ekleme**.

    ![AlertOps yapılandırma](./media/alertops-tutorial/user2.png)

4. Üzerinde **Kullanıcı Ekle** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![AlertOps yapılandırma](./media/alertops-tutorial/user3.png)

    a. İçinde **oturum açma kullanıcı adı** metin gibi kullanıcının kullanıcı adını girin **Brittasimon**.

    b. İçinde **resmi bir e-posta** metin gibi kullanıcı e-posta adresini girin **Brittasimon\@contoso.com**.

    c. İçinde **ad** metin gibi kullanıcı adını girin **Britta**.

    d. İçinde **Soyadı** metin gibi kullanıcı adını girin **Simon**.

    e. Seçin **türü** kuruluşunuz göre açılır listeden bir değer.

    f. Seçin **rol** kullanıcının kuruluşunuzun göre açılır listeden.

    g. **Add (Ekle)** seçeneğini belirleyin.

### <a name="test-sso"></a>Test SSO

Erişim Paneli'nde AlertOps kutucuğu seçtiğinizde, otomatik olarak SSO'yu ayarlama AlertOps için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)