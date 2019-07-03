---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile AcquireIO | Microsoft Docs'
description: Azure Active Directory ve AcquireIO arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: 97815e13-32ec-4470-b075-1253f5814f4f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/26/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 75a92e78e7293316cad6e567ae49c4998299415c
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67544273"
---
# <a name="tutorial-integrate-acquireio-with-azure-active-directory"></a>Öğretici: AcquireIO Azure Active Directory ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile AcquireIO tümleştirme öğreneceksiniz. AcquireIO Azure AD ile tümleştirdiğinizde, şunları yapabilirsiniz:

* AcquireIO erişimi, Azure AD'de denetler.
* Otomatik olarak AcquireIO için kendi Azure AD hesapları ile oturum açmış olmasını sağlayın.
* Bir merkezi konumda - Azure portalı hesaplarınızı yönetin.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir aboneliğiniz yoksa, alabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free/).
* AcquireIO çoklu oturum açma (SSO) abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD SSO bir test ortamında test edin. AcquireIO destekler **IDP** SSO başlattı.

## <a name="adding-acquireio-from-the-gallery"></a>Galeriden AcquireIO ekleme

Azure AD'de AcquireIO tümleştirmesini yapılandırmak için AcquireIO Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde seçin **Azure Active Directory** hizmeti.
1. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları**.
1. Yeni bir uygulama eklemek için seçin **yeni uygulama**.
1. İçinde **Galeriden Ekle** bölümüne şunu yazın **AcquireIO** arama kutusuna.
1. Seçin **AcquireIO** gelen sonuçlar panelinde ve uygulama ekleyin. Uygulama, kiracınıza eklendiği sırada birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Yapılandırma ve Azure AD SSO kullanarak adlı bir test kullanıcı AcquireIO ile test etme **B.Simon**. Çalışmak SSO için AcquireIO içinde bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişki oluşturmanız gerekir.

Yapılandırma ve Azure AD SSO ile AcquireIO sınamak için aşağıdaki yapı taşlarını tamamlayın:

1. **[Azure AD SSO'yu yapılandırma](#configure-azure-ad-sso)**  kullanıcılarınız bu özelliği kullanmak etkinleştirmek için.
2. **[AcquireIO yapılandırma](#configure-acquireio)**  uygulama tarafında SSO ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  Azure AD çoklu oturum açma B.Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  B.Simon Azure AD çoklu oturum açma kullanmak üzere etkinleştirmek için.
5. **[AcquireIO test kullanıcısı oluşturma](#create-acquireio-test-user)**  kullanıcı Azure AD gösterimini bağlı AcquireIO B.Simon bir karşılığı sağlamak için.
6. **[Test SSO](#test-sso)**  yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO'yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek üzere aşağıdaki adımları izleyin.

1. İçinde [Azure portalında](https://portal.azure.com/), **AcquireIO** uygulama tümleştirme sayfası, bulma **Yönet** bölümünde ve seçin **çoklu oturum açma**.
1. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** sayfasında **SAML**.
1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında, düzenleme/kalem simgesine tıklayıp **temel SAML yapılandırma** ayarlarını düzenlemek için.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımı uygulayın:

    İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:  `https://app.acquire.io/ad/<acquire_account_uid>`

    > [!NOTE]
    > Değer, gerçek değil. Daha sonra açıklanan gerçek yanıt URL'si erişmenizi sağlayacak **yapılandırma AcquireIO** öğreticinin bölümünde. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde, bulma **sertifika (Base64)** seçip **indirin** sertifikayı indirin ve bilgisayarınıza kaydedin.

   ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. Üzerinde **AcquireIO kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

   ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

### <a name="configure-acquireio"></a>AcquireIO yapılandırın

1. Farklı bir web tarayıcı penceresinde AcquireIO için yönetici olarak oturum açın.

2. Menü Sol taraftan tıklayarak **App Store**.

     ![AcquireIO yapılandırma](./media/acquireio-tutorial/config01.png)

3. En fazla aşağı **Active Directory** tıklayın **yükleme**.

    ![AcquireIO yapılandırma](./media/acquireio-tutorial/config02.png)

4. Active Directory açılır pencere üzerinde aşağıdaki adımları gerçekleştirin:

    ![AcquireIO yapılandırma](./media/acquireio-tutorial/config03.png)

    a. Tıklayın **kopyalama** Örneğiniz için yanıt URL'sini kopyalayıp yapıştırın **yanıt URL'si** metin kutusunda **temel SAML yapılandırma** bölümü Azure portalı.

    b. İçinde **oturum açma URL'si** metin değerini yapıştırın **oturum açma URL'si**, hangi Azure Portalı'ndan kopyaladığınız.

    c. Base64 olarak kodlanmış sertifika Not Defteri'nde açın, içeriğini kopyalayın ve yapıştırın **X.509 sertifikası** metin kutusu.

    d. Tıklayın **artık bağlantı**.

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

Bu bölümde, Azure çoklu oturum açma kullanmak için AcquireIO erişim vererek B.Simon tıklatmalarını sağlarsınız.

1. Azure portalında **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.
1. Uygulamalar listesinde **AcquireIO**.
1. Uygulamanın genel bakış sayfasında bulma **Yönet** seçin ve bölüm **kullanıcılar ve gruplar**.

   !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

1. Seçin **Kullanıcı Ekle**, ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Kullanıcı ekleme bağlantısı](common/add-assign-user.png)

1. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **B.Simon** kullanıcılar listesinden ardından **seçin** ekranın alt kısmındaki düğmesi.
1. SAML onaylama işlemi herhangi bir rolü değer de beklediğiniz varsa **rolü Seç** iletişim kutusunda, listeden bir kullanıcı için uygun rolü seçin ve ardından **seçin** ekranın alt kısmındaki düğmesi.
1. İçinde **atama Ekle** iletişim kutusunda, tıklayın **atama** düğmesi.

### <a name="create-acquireio-test-user"></a>AcquireIO test kullanıcısı oluşturma

AcquireIO için oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunların AcquireIO sağlanması gerekir. AcquireIO içinde sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Farklı bir web tarayıcı penceresinde AcquireIO için yönetici olarak oturum açın.

2. Menü Sol taraftan tıklayın **profilleri** gidin **profili Ekle**.

     ![AcquireIO yapılandırma](./media/acquireio-tutorial/config04.png)

3. Üzerinde **Ekle müşteri** açılır penceresinde, aşağıdaki adımları gerçekleştirin:

    ![AcquireIO yapılandırma](./media/acquireio-tutorial/config05.png)

    a. İçinde **adı** metin kutusunda, gibi kullanıcı adını girin **B.simon**.

    b. İçinde **e-posta** metin kutusuna, kullanıcının gibi e-posta girin **B.simon@contoso.com** .

    c. **Gönder**'e tıklayın.

### <a name="test-sso"></a>Test SSO

Erişim Paneli'nde AcquireIO kutucuğu seçtiğinizde, otomatik olarak SSO'yu ayarlama AcquireIO için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)