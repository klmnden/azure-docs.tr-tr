---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Signagelive | Microsoft Docs'
description: Azure Active Directory ve Signagelive arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: d923f0e7-ad31-4d59-a6fd-f0e895e1a32d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 1/11/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 36d4bb38d7a12edddac9d64ecc1ed3ee5a34456c
ms.sourcegitcommit: 48a41b4b0bb89a8579fc35aa805cea22e2b9922c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2019
ms.locfileid: "59577821"
---
# <a name="tutorial-azure-active-directory-integration-with-signagelive"></a>Öğretici: Signagelive ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Signagelive tümleştirme konusunda bilgi edinin.
Azure AD ile Signagelive tümleştirme ile aşağıdaki avantajları sağlar:

* Signagelive erişimi, Azure AD'de kontrol edebilirsiniz.
* Kullanıcılarınız için Signagelive (çoklu oturum açma) ile Azure AD hesaplarına otomatik olarak oturum açmanız etkinleştirebilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilir: Azure portalı.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis). Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Signagelive yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa alabileceğiniz bir [bir aylık deneme](https://azure.microsoft.com/pricing/free-trial/).
* Tek oturum üzerinde-etkin olmayan bir Signagelive abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Signagelive SP tarafından başlatılan SSO'yu destekler.

## <a name="add-signagelive-from-the-gallery"></a>Galeriden Signagelive Ekle

Azure AD'de Signagelive tümleştirmesini yapılandırmak için önce Signagelive Galeriden yönetilen SaaS uygulamaları listenize ekleyin.

Galeriden Signagelive eklemek için aşağıdaki adımları uygulayın:

1. İçinde [Azure portalında](https://portal.azure.com), sol bölmede seçin **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Git **kurumsal uygulamalar**ve ardından **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için seçin **yeni uygulama** iletişim kutusunun üstündeki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Signagelive**. 

     ![Sonuç listesinde Signagelive](common/search-new-app.png)

5. Seçin **Signagelive** seçin ve sonuçlar bölmesinde **Ekle** uygulama eklemek için Ekle düğmesine.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Signagelive adlı bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için Signagelive içinde bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı oluşturmanız gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Signagelive ile test etmek için önce aşağıdaki yapı taşlarını tamamlayın:

1. [Azure AD çoklu oturum açmayı yapılandırma](#configure-azure-ad-single-sign-on) kullanıcılarınız bu özelliği kullanmak etkinleştirmek için.
2. [Signagelive çoklu oturum açmayı yapılandırma](#configure-signagelive-single-sign-on) üzerinde uygulama tarafından çoklu oturum açma ayarları yapılandırmak için.
3. [Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user) Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. [Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user) Britta Simon, Azure AD çoklu oturum açma kullanmak üzere etkinleştirmek için.
5. [Signagelive test kullanıcısı oluşturma](#create-a-signagelive-test-user) bir karşılığı Britta simon'un kullanıcı Azure AD gösterimini bağlı Signagelive sağlamak için.
6. [Çoklu oturum açmayı test](#test-single-sign-on) yapılandırma çalıştığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile Signagelive yapılandırmak için aşağıdaki adımları uygulayın:

1. İçinde [Azure portalında](https://portal.azure.com/), **Signagelive** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. İçinde **tek bir oturum açma yönteminizi seçmeniz** Seç iletişim kutusunda **SAML** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **SAML ile çoklu oturum açmayı ayarlama** sayfasında **Düzenle** açmak için **temel SAML yapılandırma** iletişim kutusu.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. İçinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları uygulayın:

    ![Signagelive etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-signonurl.png)

    İçinde **oturum açma URL'si** kutusunda, aşağıdaki desen kullanan bir URL girin:  `https://login.signagelive.com/sso/<ORGANIZATIONALUNITNAME>`

    > [!NOTE]
    > Değer, gerçek değil. Değerini gerçek oturum açma URL'si ile güncelleştirin. Değer almak için iletişime geçin [Signagelive istemci Destek ekibine](mailto:support@signagelive.com) . Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünden **indirme** indirmek için **sertifika (ham)** , gereksinim başına verilen seçenekleri. Bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](common/certificateraw.png)

6. İçinde **Signagelive kümesi** bölümünde, gereksinim duyduğunuz URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-signagelive-single-sign-on"></a>Signagelive tek oturum açmayı yapılandırın

Çoklu oturum açma Signagelive tarafında yapılandırmak için indirilen Gönder **sertifika (ham)** ve URL'leri için Azure Portalı'ndan kopyaladığınız [Signagelive Destek ekibine](mailto:support@signagelive.com). Bunlar, SAML SSO bağlantının her iki kenarı da düzgün ayarlandığından emin olun.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları uygulayın.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alanına **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alanına, "brittasimon@yourcompanydomain.extension". Örneğin, bu durumda, girdiğiniz "BrittaSimon@contoso.com".

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından (parola) kutusunda görüntülenen değeri not edin.

    d. **Oluştur**’u seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Signagelive erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**ve ardından **Signagelive**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Signagelive**.

    ![Uygulamalar listesinde Signagelive bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Seçin **Kullanıcı Ekle** düğmesi. Ardından **atama Ekle** iletişim kutusunda **kullanıcılar ve gruplar**.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusundaki **kullanıcılar** listesinden **Britta Simon**. Ardından **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylama işlemi ardından, içinde bir rol değer bekleniyor durumunda **rolü Seç** iletişim kutusunda, listeden bir kullanıcı için uygun rolü seçin. Ardından, **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim kutusunda **atama** düğmesi.

### <a name="create-a-signagelive-test-user"></a>Signagelive test kullanıcısı oluşturma

Bu bölümde, Britta Simon Signagelive içinde adlı bir kullanıcı oluşturun. Çalışmak [Signagelive Destek ekibine](mailto:support@signagelive.com) Signagelive platform kullanıcıları eklemek için. Oluşturma ve kullanıcılara çoklu oturum açma kullanmadan önce etkinleştirmeniz gerekir.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Azure AD çoklu oturum açma yapılandırmanızı MyApps portalında kullanarak test.

Seçtiğinizde, **Signagelive** döşeme MyApps portalında, otomatik olarak açmış olmanız. MyApps portalında hakkında daha fazla bilgi için bkz: [MyApps portalı nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [ SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

