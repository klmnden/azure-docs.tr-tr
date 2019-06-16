---
title: 'Öğretici: Syncplicity ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Syncplicity arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 896a3211-f368-46d7-95b8-e4768c23be08
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/10/2019
ms.author: jeedes
ms.openlocfilehash: e6a8a25e88d4193562c818f30efd5eb017c372fd
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67089302"
---
# <a name="tutorial-integrate-syncplicity-with-azure-active-directory"></a>Öğretici: Syncplicity Azure Active Directory ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Syncplicity tümleştirme öğreneceksiniz. Syncplicity Azure AD ile tümleştirdiğinizde, şunları yapabilirsiniz:

* Syncplicity erişimi, Azure AD'de denetler.
* Otomatik olarak Syncplicity için kendi Azure AD hesapları ile oturum açmış olmasını sağlayın.
* Bir merkezi konumda - Azure portalı hesaplarınızı yönetin.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir aboneliğiniz yoksa, bir aylık ücretsiz deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).
* Syncplicity çoklu oturum açma (SSO) abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD SSO bir test ortamında test edin. Syncplicity destekler **SP** SSO başlattı.

## <a name="adding-syncplicity-from-the-gallery"></a>Syncplicity galeri ekleme

Azure AD'de Syncplicity tümleştirmesini yapılandırmak için Syncplicity Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde seçin **Azure Active Directory** hizmeti.
1. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları**.
1. Yeni bir uygulama eklemek için seçin **yeni uygulama**.
1. İçinde **Galeriden Ekle** bölümüne şunu yazın **Syncplicity** arama kutusuna.
1. Seçin **Syncplicity** gelen sonuçlar panelinde ve uygulama ekleyin. Uygulama, kiracınıza eklendiği sırada birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-sso"></a>Yapılandırma ve Azure AD SSO test etme

Yapılandırma ve Azure AD SSO kullanarak adlı bir test kullanıcı Syncplicity ile test etme **B.Simon**. Çalışmak SSO için Syncplicity içinde bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişki oluşturmanız gerekir.

Yapılandırma ve Azure AD SSO ile Syncplicity sınamak için aşağıdaki yapı taşlarını tamamlayın:

1. **[Azure AD SSO'yu yapılandırma](#configure-azure-ad-sso)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Syncplicity SSO'yu yapılandırarak](#configure-syncplicity-sso)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma B.Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak B.Simon etkinleştirmek için.
5. **[Syncplicity test kullanıcısı oluşturma](#create-syncplicity-test-user)**  - kullanıcı Azure AD gösterimini bağlı Syncplicity B.Simon bir karşılığı vardır.
6. **[Test SSO](#test-sso)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO'yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek üzere aşağıdaki adımları izleyin.

1. İçinde [Azure portalında](https://portal.azure.com/), **Syncplicity** uygulama tümleştirme sayfası, bulma **Yönet** bölümünde ve seçin **çoklu oturum açma**.
1. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** sayfasında **SAML**.
1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında, düzenleme/kalem simgesine tıklayıp **temel SAML yapılandırma** ayarlarını düzenlemek için.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. Üzerinde **temel SAML yapılandırma** sayfasında, aşağıdaki alanlar için değerleri girin:

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<companyname>.syncplicity.com`

    b. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<companyname>.syncplicity.com/sp`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL ve tanımlayıcıdır ile güncelleştirin. İlgili kişi [Syncplicity istemci Destek ekibine](https://www.syncplicity.com/contact-us) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde, bulma **sertifika (Base64)** seçip **indirin** sertifikayı indirin ve bilgisayarınıza kaydedin.

   ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. Üzerinde **Syncplicity kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

   ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

### <a name="configure-syncplicity-sso"></a>Syncplicity SSO yapılandırma

1. Oturum açın, **Syncplicity** Kiracı.

1. Üstteki menüden **yönetici**seçin **ayarları**ve ardından **özel etki alanı ve çoklu oturum açma**.

    ![Syncplicity](./media/syncplicity-tutorial/ic769545.png "Syncplicity")

1. Üzerinde **çoklu oturum açma (SSO)** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açma \(SSO\)](./media/syncplicity-tutorial/ic769550.png "Single Sign-On \\\(SSO\\\)")

    a. İçinde **özel etki alanı** metin etki alanınızın adını yazın.
  
    b. Seçin **etkin** olarak **çoklu oturum açma durumu**.

    c. İçinde **varlık kimliği** metin kutusu, yapıştırma **tanımlayıcı (varlık kimliği)** kullandığınız değeri **temel SAML yapılandırma** Azure portalında.

    d. İçinde **oturum açma sayfası URL'si** metin kutusu, yapıştırma **oturum açma URL'si** , Azure Portalı'ndan kopyaladığınız.

    e. İçinde **oturum kapatma sayfasını URL'si** metin kutusu, yapıştırma **oturum kapatma URL'si** , Azure Portalı'ndan kopyaladığınız.

    f. İçinde **kimlik sağlayıcısı sertifikası**, tıklayın **dosya**ve sonra Azure portalından indirdiğiniz sertifika karşıya yükleyin.

    g. Tıklayın **değişiklikleri kaydetme**.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümde, bir test kullanıcısı B.Simon adlı Azure portalında oluşturacaksınız.

1. Azure Portalı'ndaki sol bölmeden seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.
1. Seçin **yeni kullanıcı** ekranın üstünde.
1. İçinde **kullanıcı** özellikleri, aşağıdaki adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. İçinde **kullanıcı adı** alanına username@companydomain.extension. Örneğin, `B.Simon@contoso.com`.
   1. Seçin **Show parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.
   1. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Syncplicity erişim vererek B.Simon tıklatmalarını sağlarsınız.

1. Azure portalında **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.
1. Uygulamalar listesinde **Syncplicity**.
1. Uygulamanın genel bakış sayfasında bulma **Yönet** seçin ve bölüm **kullanıcılar ve gruplar**.

   !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

1. Seçin **Kullanıcı Ekle**, ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Kullanıcı ekleme bağlantısı](common/add-assign-user.png)

1. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **B.Simon** kullanıcılar listesinden ardından **seçin** ekranın alt kısmındaki düğmesi.
1. SAML onaylama işlemi herhangi bir rolü değer de beklediğiniz varsa **rolü Seç** iletişim kutusunda, listeden bir kullanıcı için uygun rolü seçin ve ardından **seçin** ekranın alt kısmındaki düğmesi.
1. İçinde **atama Ekle** iletişim kutusunda, tıklayın **atama** düğmesi.

### <a name="create-syncplicity-test-user"></a>Syncplicity test kullanıcısı oluşturma

AAD kullanıcıları oturum açabilmesi bunlar Syncplicity uygulama sağlanmalıdır. Bu bölümde Syncplicity AAD kullanıcı hesapları oluşturma işlemini açıklar.

**Bir kullanıcı hesabına Syncplicity sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açın, **Syncplicity** Kiracı (örneğin: `https://company.Syncplicity.com`).

1. Tıklayın **yönetici** seçip **kullanıcı hesaplarını** ve ardından **bir kullanıcı Ekle**.

    ![Kullanıcıları Yönet](./media/syncplicity-tutorial/ic769764.png "kullanıcıları yönetme")

1. Tür **e-posta adreslerini** seçin, sağlamak istediğiniz bir Azure AD hesabı **kullanıcı** olarak **rol**ve ardından **sonraki**.

    ![Hesap bilgileri](./media/syncplicity-tutorial/ic769765.png "hesap bilgileri")

    > [!NOTE]
    > AAD hesap sahibinin onaylayın ve hesap etkinleştirme bağlantısı içeren bir e-posta alır.

1. Bir grubu seçin, yeni kullanıcı bir üyesi olun ve ardından, şirketinizde **sonraki**.

    ![Grup üyeliği](./media/syncplicity-tutorial/ic769772.png "grup üyeliği")

    > [!NOTE]
    > Listelenen hiçbir grupları varsa, tıklayın **sonraki**.

1. Kullanıcının bilgisayarında Syncplicity'nın denetimi altına yerleştirin ve ardından istediğiniz klasörleri seçin **sonraki**.

    ![Syncplicity klasörleri](./media/syncplicity-tutorial/ic769773.png "Syncplicity klasörleri")

> [!NOTE]
> Herhangi diğer Syncplicity kullanıcı hesabı oluşturma araçları kullanabilir veya API'leri için AAD kullanıcı hesapları sağlamak Syncplicity tarafından sağlanan.

### <a name="test-sso"></a>Test SSO

Erişim Paneli'nde Syncplicity kutucuğu seçtiğinizde, otomatik olarak SSO'yu ayarlama Syncplicity için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)