---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Replicon | Microsoft Docs'
description: Azure Active Directory ve Replicon arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 02a62f15-917c-417c-8d80-fe685e3fd601
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/10/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: b27615b0c76b5c23bbc79788431b0e909b8bf22a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67092761"
---
# <a name="tutorial-integrate-replicon-with-azure-active-directory"></a>Öğretici: Replicon Azure Active Directory ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Replicon tümleştirme öğreneceksiniz. Replicon Azure AD ile tümleştirdiğinizde, şunları yapabilirsiniz:

* Replicon erişimi, Azure AD'de denetler.
* Otomatik olarak Replicon için kendi Azure AD hesapları ile oturum açmış olmasını sağlayın.
* Bir merkezi konumda - Azure portalı hesaplarınızı yönetin.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir aboneliğiniz yoksa, bir aylık ücretsiz deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).
* Replicon çoklu oturum açma (SSO) abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD SSO bir test ortamında test edin. Replicon destekler **SP** SSO başlattı.

## <a name="adding-replicon-from-the-gallery"></a>Galeriden Replicon ekleme

Azure AD'de Replicon tümleştirmesini yapılandırmak için Replicon Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde seçin **Azure Active Directory** hizmeti.
1. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları**.
1. Yeni bir uygulama eklemek için seçin **yeni uygulama**.
1. İçinde **Galeriden Ekle** bölümüne şunu yazın **Replicon** arama kutusuna.
1. Seçin **Replicon** gelen sonuçlar panelinde ve uygulama ekleyin. Uygulama, kiracınıza eklendiği sırada birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Yapılandırma ve Azure AD SSO kullanarak adlı bir test kullanıcı Replicon ile test etme **B.Simon**. Çalışmak SSO için Replicon içinde bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişki oluşturmanız gerekir.

Yapılandırma ve Azure AD SSO ile Replicon sınamak için aşağıdaki yapı taşlarını tamamlayın:

1. **[Azure AD SSO'yu yapılandırma](#configure-azure-ad-sso)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Replicon SSO'yu yapılandırarak](#configure-replicon-sso)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma B.Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak B.Simon etkinleştirmek için.
5. **[Replicon test kullanıcısı oluşturma](#create-replicon-test-user)**  - kullanıcı Azure AD gösterimini bağlı Replicon B.Simon bir karşılığı vardır.
6. **[Test SSO](#test-sso)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO'yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek üzere aşağıdaki adımları izleyin.

1. İçinde [Azure portalında](https://portal.azure.com/), **Replicon** uygulama tümleştirme sayfası, bulma **Yönet** bölümünde ve seçin **çoklu oturum açma**.
1. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** sayfasında **SAML**.
1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında, düzenleme/kalem simgesine tıklayıp **temel SAML yapılandırma** ayarlarını düzenlemek için.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. Üzerinde **temel SAML yapılandırma** sayfasında, aşağıdaki alanlar için değerleri girin:

    1. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://global.replicon.com/!/saml2/<client name>/sp-sso/post`

    1. İçinde **tanımlayıcı** kutusuna şu biçimi kullanarak bir URL yazın: `https://global.replicon.com/!/saml2/<client name>`

    1. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://global.replicon.com/!/saml2/<client name>/sso/post`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si, tanımlayıcı ve yanıt URL'si ile güncelleştirin. İlgili kişi [Replicon istemci Destek ekibine](https://www.replicon.com/customerzone/contact-support) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

1. Düzenleme/kalem simgesine tıklayıp **SAML imzalama sertifikası** ayarlarını düzenlemek için.

    ![İmza algoritması](common/signing-algorithm.png)

    1. Seçin **oturum SAML onayı** olarak **imzalama seçeneği**.

    1. Seçin **SHA-256'yı** olarak **imza algoritması**.

1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde, bulma **Federasyon meta verileri XML** seçip **indirin** sertifikayı indirin ve bilgisayarınıza kaydedin.

   ![Sertifika indirme bağlantısı](common/metadataxml.png)

### <a name="configure-replicon-sso"></a>Replicon SSO yapılandırma

1. Farklı bir web tarayıcı penceresinde Replicon şirket sitenize yönetici olarak oturum açın.

2. SAML 2.0 yapılandırmak için aşağıdaki adımları gerçekleştirin:

    ![SAML kimlik doğrulamasını etkinleştirme](./media/replicon-tutorial/ic777805.png "etkinleştirme SAML kimlik doğrulaması")

    a. Görüntülenecek **EnableSAML Authentication2** iletişim kutusunda, aşağıdaki URL'nize, sonra şirket anahtarınızı ekleyin: `/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2`

    * Tam URL şeması aşağıda gösterilmiştir: `https://na2.replicon.com/\<YourCompanyKey\>/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2`

   b. Tıklayın **+** genişletmek için **v20Configuration** bölümü.

   c. Tıklayın **+** genişletmek için **metaDataConfiguration** bölümü.

   d. Seçin **SHA256** xmlSignatureAlgorithm için

   e. Tıklayın **Dosya Seç**, kimlik sağlayıcısı meta verileri XML dosyanızı seçin ve tıklayın **Gönder**.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümde, bir test kullanıcısı B.Simon adlı Azure portalında oluşturacaksınız.

1. Azure Portalı'ndaki sol bölmeden seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.
1. Seçin **yeni kullanıcı** ekranın üstünde.
1. İçinde **kullanıcı** özellikleri, aşağıdaki adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. İçinde **kullanıcı adı** alanına username@companydomain.extension. Örneğin, `BrittaSimon@contoso.com`.
   1. Seçin **Show parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.
   1. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Replicon erişim vererek B.Simon tıklatmalarını sağlarsınız.

1. Azure portalında **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.
1. Uygulamalar listesinde **Replicon**.
1. Uygulamanın genel bakış sayfasında bulma **Yönet** seçin ve bölüm **kullanıcılar ve gruplar**.

   !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

1. Seçin **Kullanıcı Ekle**, ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Kullanıcı ekleme bağlantısı](common/add-assign-user.png)

1. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **B.Simon** kullanıcılar listesinden ardından **seçin** ekranın alt kısmındaki düğmesi.
1. SAML onaylama işlemi herhangi bir rolü değer de beklediğiniz varsa **rolü Seç** iletişim kutusunda, listeden bir kullanıcı için uygun rolü seçin ve ardından **seçin** ekranın alt kısmındaki düğmesi.
1. İçinde **atama Ekle** iletişim kutusunda, tıklayın **atama** düğmesi.

### <a name="create-replicon-test-user"></a>Replicon test kullanıcısı oluşturma

Bu bölümün amacı Replicon B.Simon adlı bir kullanıcı oluşturmaktır.

**Kullanıcı el ile oluşturmanız gerekiyorsa, aşağıdaki adımları uygulayın:**

1. Bir web tarayıcısı penceresinde Replicon şirket sitenize yönetici olarak oturum açın.

2. Git **Yönetim \> kullanıcılar**.

    ![Kullanıcılar](./media/replicon-tutorial/ic777806.png "kullanıcılar")

3. Tıklayın **+ Kullanıcı Ekle**.

    ![Kullanıcı ekleme](./media/replicon-tutorial/ic777807.png "kullanıcı ekleme")

4. İçinde **kullanıcı profili** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı profili](./media/replicon-tutorial/ic777808.png "kullanıcı profili")

    a. İçinde **oturum açma adı** metin türü Azure AD e-posta adresi gibi sağlamak istediğiniz Azure AD kullanıcısının `B.Simon@contoso.com`.

    > [!NOTE]
    > Azure AD'de kullanıcının e-posta adresiyle eşleşecek şekilde oturum açma adı gerekiyor

    b. Olarak **kimlik doğrulama türü**seçin **SSO**.

    c. Kimlik doğrulama kimliği oturum açma adı (kullanıcı Azure AD e-posta adresi) ile aynı değere ayarlayın

    d. İçinde **departmanı** metin kutusu, kullanıcının bölüm adını yazın.

    e. Olarak **çalışan türü**seçin **yönetici**.

    f. Tıklayın **kullanıcı profili Kaydet**.

> [!NOTE]
> Herhangi diğer Replicon kullanıcı hesabı oluşturma araçları kullanabilir veya API Azure AD'ye kullanıcı hesapları sağlamak için Replicon tarafından sağlanan.

### <a name="test-sso"></a>Test SSO

Erişim Paneli'nde Replicon kutucuğu seçtiğinizde, otomatik olarak SSO'yu ayarlama Replicon için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)