---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Displayr | Microsoft Docs'
description: Azure Active Directory ve Displayr arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: b739b4e3-1a37-4e3c-be89-c3945487f4c1
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/20/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 93a1ad1f9fbc01cd06b3aaffc8a718634e8454d6
ms.sourcegitcommit: 8e76be591034b618f5c11f4e66668f48c090ddfd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66357038"
---
# <a name="tutorial-integrate-displayr-with-azure-active-directory"></a>Öğretici: Displayr Azure Active Directory ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Displayr tümleştirme öğreneceksiniz. Displayr Azure AD ile tümleştirdiğinizde, şunları yapabilirsiniz:

* Displayr erişimi, Azure AD'de denetler.
* Otomatik olarak Displayr için kendi Azure AD hesapları ile oturum açmış olmasını sağlayın.
* Bir merkezi konumda - Azure portalı hesaplarınızı yönetin.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir aboneliğiniz yoksa, alabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free/).
* Displayr çoklu oturum açma (SSO) abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD SSO bir test ortamında test edin. Displayr destekler **SP** SSO başlattı.

## <a name="adding-displayr-from-the-gallery"></a>Galeriden Displayr ekleme

Azure AD'de Displayr tümleştirmesini yapılandırmak için Displayr Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde seçin **Azure Active Directory** hizmeti.
1. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları**.
1. Yeni bir uygulama eklemek için seçin **yeni uygulama**.
1. İçinde **Galeriden Ekle** bölümüne şunu yazın **Displayr** arama kutusuna.
1. Seçin **Displayr** gelen sonuçlar panelinde ve uygulama ekleyin. Uygulama, kiracınıza eklendiği sırada birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Yapılandırma ve Azure AD SSO kullanarak adlı bir test kullanıcı Displayr ile test etme **Britta Simon**. Çalışmak SSO için Displayr içinde bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişki oluşturmanız gerekir.

Yapılandırma ve Azure AD SSO ile Displayr sınamak için aşağıdaki yapı taşlarını tamamlayın:

1. **[Azure AD SSO'yu yapılandırma](#configure-azure-ad-sso)**  kullanıcılarınız bu özelliği kullanmak etkinleştirmek için.
2. **[Displayr yapılandırma](#configure-displayr)**  uygulama tarafında SSO ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  Britta Simon, Azure AD çoklu oturum açma kullanmak üzere etkinleştirmek için.
5. **[Displayr test kullanıcısı oluşturma](#create-displayr-test-user)**  bir karşılığı Britta simon'un kullanıcı Azure AD gösterimini bağlı Displayr sağlamak için.
6. **[Test SSO](#test-sso)**  yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO'yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek üzere aşağıdaki adımları izleyin.

1. İçinde [Azure portalında](https://portal.azure.com/), **Displayr** uygulama tümleştirme sayfası, bulma **Yönet** bölümünde ve seçin **çoklu oturum açma**.
1. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** sayfasında **SAML**.
1. Üzerinde **Kurulum çoklu oturum açma SAML ile** sayfasında, düzenleme/kalem simgesine tıklayıp **temel SAML yapılandırma** ayarlarını düzenlemek için.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımı uygulayın:

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<YOURDOMAIN>.displayr.com`

    b. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna şu biçimi kullanarak bir URL yazın:`<YOURDOMAIN>.displayr.com`

    >[!NOTE]
    >Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL ve tanımlayıcıdır ile güncelleştirin. İlgili kişi [Displayr istemci Destek ekibine](mailto:support@displayr.com) bu değerleri almak için. Azure portalında temel bir SAML yapılandırma bölümünde gösterilen desenleri de başvurabilirsiniz.

1. Üzerinde **Kurulum çoklu oturum açma SAML ile** sayfasında **SAML imzalama sertifikası** bölümünde, bulma **sertifika (Base64)** seçip **indirin** sertifikayı indirin ve bilgisayarınıza kaydedin.

   ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. Displayr uygulama, özel öznitelik eşlemelerini SAML belirteci öznitelikleri yapılandırmanıza ekleyin gerektiren belirli bir biçimde SAML onaylamalarını bekler. Aşağıdaki ekran görüntüsünde, varsayılan öznitelikler listesinde gösterilmiştir. Tıklayın **Düzenle** kullanıcı öznitelikleri iletişim kutusunu açmak için simge.

    ![image](common/edit-attribute.png)

1. Yukarıdaki için ayrıca Displayr uygulama SAML yanıtta geçirilecek birkaç daha fazla öznitelik bekliyor. İçinde **kullanıcı öznitelikleri ve talepler** bölümünde **grup talepleri (Önizleme)** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    a. Tıklayın **kalem** yanındaki **grupları döndürülen talebi**.

    ![image](./media/displayr-tutorial/config04.png)

    ![image](./media/displayr-tutorial/config05.png)

    b. Seçin **tüm grupları** radyo listeden.

    c. Seçin **kaynak özniteliği** , **Grup Kimliği**.

    d. Denetleme **grup talebi adını özelleştirme**.

    e. Denetleme **yayma gruplar olarak rol talepleri**.

    f. **Kaydet**’e tıklayın.

1. Üzerinde **Kurulum Displayr** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

   ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

### <a name="configure-displayr"></a>Displayr yapılandırın

1. Yüklemeniz gerekiyor Displayr içinde yapılandırmasını otomatik hale getirmenizi **My Apps güvenli oturum açma tarayıcı uzantısı** tıklayarak **uzantıyı yükleme**.

    ![Uygulamaları uzantım](common/install-myappssecure-extension.png)

2. Uzantı tarayıcıya ekledikten sonra tıklayarak **Kurulum Displayr** Displayr uygulamaya yönlendirir. Burada, Displayr oturum açmak için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı otomatik olarak sizin için uygulamayı yapılandırma ve 3-6 adımları otomatik hale getirin.

    ![Kurulum yapılandırması](common/setup-sso.png)

3. Displayr el ile ayarlamak istiyorsanız, yeni bir web tarayıcı penceresi ve oturum Displayr şirketinizin sitesi yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

4. Tıklayarak **ayarları** gidin **hesabı**.

    ![Yapılandırma](./media/displayr-tutorial/config01.png)

5. Geçiş **ayarları** tıklayarak için sayfayı aşağı kaydırın ve üstteki menüden **yapılandırma tek oturum üzerinde (SAML)** .

    ![Yapılandırma](./media/displayr-tutorial/config02.png)

6. Üzerinde **tek oturum üzerinde (SAML)** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Yapılandırma](./media/displayr-tutorial/config03.png)

    a. Denetleme **etkinleştirme tek oturum üzerinde (SAML)** kutusu.

    b. Gerçek kopyalama **tanımlayıcısı** değerini **temel SAML yapılandırma** Azure AD'ye bölümüne yapıştırın **veren** metin kutusu.

    c. İçinde **oturum açma URL'si** metin kutusunda, değerini yapıştırın **oturum açma URL'si**, hangi Azure Portalı'ndan kopyaladığınız.

    d. İçinde **oturum kapatma URL'si** metin kutusunda, değerini yapıştırın **oturum kapatma URL'si**, hangi Azure Portalı'ndan kopyaladığınız.

    e. Sertifika (Base64) Not Defteri'nde açın, içeriğini kopyalayın ve yapıştırın **sertifika** metin kutusu.

    f. **Grup eşlemelerini** isteğe bağlıdır.

    g. **Kaydet**’e tıklayın.  

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

Bu bölümde, Azure çoklu oturum açma kullanmak için Displayr erişim vererek Britta Simon tıklatmalarını sağlarsınız.

1. Azure portalında **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.
1. Uygulamalar listesinde **Displayr**.
1. Uygulamanın genel bakış sayfasında bulma **Yönet** seçin ve bölüm **kullanıcılar ve gruplar**.

   !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

1. Seçin **Kullanıcı Ekle**, ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Kullanıcı ekleme bağlantısı](common/add-assign-user.png)

1. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcılar listesinden ardından **seçin** ekranın alt kısmındaki düğmesi.
1. SAML onaylama işlemi herhangi bir rolü değer de beklediğiniz varsa **rolü Seç** iletişim kutusunda, listeden bir kullanıcı için uygun rolü seçin ve ardından **seçin** ekranın alt kısmındaki düğmesi.
1. İçinde **atama Ekle** iletişim kutusunda, tıklayın **atama** düğmesi.

### <a name="create-displayr-test-user"></a>Displayr test kullanıcısı oluşturma

Azure AD kullanıcılarının etkinleştirmek için oturum içinde Displayr için bunların Displayr sağlanması gerekir. Displayr içinde sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Yönetici olarak Displayr için oturum açın.

2. Tıklayarak **ayarları** gidin **hesabı**.

    ![Displayr yapılandırma](./media/displayr-tutorial/config01.png)

3. Geçiş **ayarları** sayfayı aşağı kaydırın ve üstteki menüden kadar **kullanıcılar** bölümünde sonra tıklayın **yeni kullanıcı**.

    ![Displayr yapılandırma](./media/displayr-tutorial/config07.png)

4. Üzerinde **yeni kullanıcı** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Displayr yapılandırma](./media/displayr-tutorial/config06.png)

    a. İçinde **adı** metin kutusunda, gibi kullanıcı adını girin **Brittasimon**.

    b. İçinde **e-posta** metin kutusuna, kullanıcının gibi e-posta girin `Brittasimon@contoso.com`.

    c. Uygun seçin **grup üyeliği**.

    d. **Kaydet**’e tıklayın.

### <a name="test-sso"></a>Test SSO

Erişim Paneli'nde Displayr kutucuğu seçtiğinizde, otomatik olarak SSO'yu ayarlama Displayr için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
