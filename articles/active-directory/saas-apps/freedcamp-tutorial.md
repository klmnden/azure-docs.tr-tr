---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Freedcamp | Microsoft Docs'
description: Azure Active Directory ve Freedcamp arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: bfc73563-017d-458f-b634-162f93e03b74
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/20/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: b9aabdba16b334aa957e1e8109d1e16d22e01dc7
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65987868"
---
# <a name="tutorial-integrate-freedcamp-with-azure-active-directory"></a>Öğretici: Freedcamp Azure Active Directory ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Freedcamp tümleştirme öğreneceksiniz. Freedcamp Azure AD ile tümleştirdiğinizde, şunları yapabilirsiniz:

* Freedcamp erişimi, Azure AD'de denetler.
* Otomatik olarak Freedcamp için kendi Azure AD hesapları ile oturum açmış olmasını sağlayın.
* Bir merkezi konumda - Azure portalı hesaplarınızı yönetin.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir aboneliğiniz yoksa, alabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free/).
* Freedcamp çoklu oturum açma (SSO) abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD SSO bir test ortamında test edin. Freedcamp destekler **SP ve IDP** SSO başlattı.

## <a name="adding-freedcamp-from-the-gallery"></a>Galeriden Freedcamp ekleme

Azure AD'de Freedcamp tümleştirmesini yapılandırmak için Freedcamp Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde seçin **Azure Active Directory** hizmeti.
1. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları**.
1. Yeni bir uygulama eklemek için seçin **yeni uygulama**.
1. İçinde **Galeriden Ekle** bölümüne şunu yazın **Freedcamp** arama kutusuna.
1. Seçin **Freedcamp** gelen sonuçlar panelinde ve uygulama ekleyin. Uygulama, kiracınıza eklendiği sırada birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Yapılandırma ve Azure AD SSO kullanarak adlı bir test kullanıcı Freedcamp ile test etme **Britta Simon**. Çalışmak SSO için Freedcamp içinde bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişki oluşturmanız gerekir.

Yapılandırma ve Azure AD SSO ile Freedcamp sınamak için aşağıdaki yapı taşlarını tamamlayın:

1. **[Azure AD SSO'yu yapılandırma](#configure-azure-ad-sso)**  kullanıcılarınız bu özelliği kullanmak etkinleştirmek için.
2. **[Freedcamp yapılandırma](#configure-freedcamp)**  uygulama tarafında SSO ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  Britta Simon, Azure AD çoklu oturum açma kullanmak üzere etkinleştirmek için.
5. **[Freedcamp test kullanıcısı oluşturma](#create-freedcamp-test-user)**  bir karşılığı Britta simon'un kullanıcı Azure AD gösterimini bağlı Freedcamp sağlamak için.
6. **[Test SSO](#test-sso)**  yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO'yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek üzere aşağıdaki adımları izleyin.

1. İçinde [Azure portalında](https://portal.azure.com/), **Freedcamp** uygulama tümleştirme sayfası, bulma **Yönet** bölümünde ve seçin **çoklu oturum açma**.
1. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** sayfasında **SAML**.
1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında, düzenleme/kalem simgesine tıklayıp **temel SAML yapılandırma** ayarlarını düzenlemek için.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. Üzerinde **temel SAML yapılandırma** uygulamada yapılandırmak isterseniz, bölümü **IDP** başlatılan modu, aşağıdaki adımları gerçekleştirin:

    1. İçinde **tanımlayıcı** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<SUBDOMAIN>.freedcamp.com/sso/<UNIQUEID>`

    2. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<SUBDOMAIN>.freedcamp.com/sso/acs/<UNIQUEID>`

1. Tıklayın **ek URL'lerini ayarlayın** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:  `https://<SUBDOMAIN>.freedcamp.com/login`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Kullanıcılar ayrıca kendi müşteri etki alanına göre URL'si değerleri girin ve mutlaka desenini olmayabilir `freedcamp.com`, bunlar tüm müşteri etki alanı belirli bir değer, kendi uygulama örneği için belirli girebilirsiniz. De başvurabilirsiniz [Freedcamp istemci Destek ekibine](mailto:devops@freedcamp.com) url desenleri hakkında daha fazla bilgi için.

1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde, bulma **sertifika (Base64)** seçip **indirin** sertifikayı indirin ve bilgisayarınıza kaydedin.

   ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. Üzerinde **Freedcamp kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

   ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

### <a name="configure-freedcamp"></a>Freedcamp yapılandırın

1. Yüklemeniz gerekiyor Freedcamp içinde yapılandırmasını otomatik hale getirmenizi **My Apps güvenli oturum açma tarayıcı uzantısı** tıklayarak **uzantıyı yükleme**.

    ![Uygulamaları uzantım](common/install-myappssecure-extension.png)

2. Uzantı tarayıcıya ekledikten sonra tıklayarak **Kurulum Freedcamp** Freedcamp uygulamaya yönlendirir. Burada, Freedcamp oturum açmak için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı otomatik olarak sizin için uygulamayı yapılandırma ve 3-5 adımlarını otomatik hale getirin.

    ![Kurulum yapılandırması](common/setup-sso.png)

3. Freedcamp el ile ayarlamak istiyorsanız, yeni bir web tarayıcı penceresi ve oturum Freedcamp şirketinizin sitesi yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

4. Sayfanın sağ üst köşesindeki tıklayarak **profili** gidin **hesabım**.

    ![Freedcamp yapılandırma](./media/freedcamp-tutorial/config01.png)

5. Menü çubuğunun sol taraftan tıklayarak **SSO** ve **bilgisayarınızı SSO bağlantıları** sayfasında aşağıdaki adımları gerçekleştirin:

    ![Freedcamp yapılandırma](./media/freedcamp-tutorial/config02.png)

    a. İçinde **başlık** metin kutusunda, bir başlık yazın.

    b. İçinde **varlık kimliği** metin kutusu, yapıştırma **Azure AD tanımlayıcısı** Azure portaldan kopyaladığınız değeri.

    c. İçinde **oturum açma URL'si** metin kutusu, yapıştırma **oturum açma URL'si** Azure portaldan kopyaladığınız değeri.

    d. Base64 olarak kodlanmış sertifika Not Defteri'nde açın, içeriğini kopyalayın ve yapıştırın **sertifika** metin kutusu.

    e. **Gönder**'e tıklayın.

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

Bu bölümde, Azure çoklu oturum açma kullanmak için Freedcamp erişim vererek Britta Simon tıklatmalarını sağlarsınız.

1. Azure portalında **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.
1. Uygulamalar listesinde **Freedcamp**.
1. Uygulamanın genel bakış sayfasında bulma **Yönet** seçin ve bölüm **kullanıcılar ve gruplar**.

   !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

1. Seçin **Kullanıcı Ekle**, ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Kullanıcı ekleme bağlantısı](common/add-assign-user.png)

1. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcılar listesinden ardından **seçin** ekranın alt kısmındaki düğmesi.
1. SAML onaylama işlemi herhangi bir rolü değer de beklediğiniz varsa **rolü Seç** iletişim kutusunda, listeden bir kullanıcı için uygun rolü seçin ve ardından **seçin** ekranın alt kısmındaki düğmesi.
1. İçinde **atama Ekle** iletişim kutusunda, tıklayın **atama** düğmesi.

### <a name="create-freedcamp-test-user"></a>Freedcamp test kullanıcısı oluşturma

Azure AD kullanıcılarının etkinleştirmek için oturum içinde Freedcamp için bunların Freedcamp sağlanması gerekir. Freedcamp içinde sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Farklı bir web tarayıcı penceresinde Freedcamp için bir güvenlik yöneticisi olarak oturum açın.

2. Sayfanın üst toright köşesi üzerinde tıklayarak **profili** gidin **yönetme sistem**.

    ![Freedcamp yapılandırma](./media/freedcamp-tutorial/config03.png)

3. Sistemi yönetme sayfanın sağ tarafında aşağıdaki adımları gerçekleştirin:

    ![Freedcamp yapılandırma](./media/freedcamp-tutorial/config04.png)

    a. Tıklayarak **Ekle veya kullanıcıları davet**.

    b. İçinde **e-posta** metin kutusuna, kullanıcının gibi e-posta girin `Brittasimon@contoso.com`.

    c. Tıklayın **kullanıcı ekleme**.

### <a name="test-sso"></a>Test SSO

Erişim Paneli'nde Freedcamp kutucuğu seçtiğinizde, otomatik olarak SSO'yu ayarlama Freedcamp için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)