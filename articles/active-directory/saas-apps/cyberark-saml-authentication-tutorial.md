---
title: 'Öğretici: CyberArk SAML kimlik doğrulaması ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: CyberArk SAML kimlik doğrulaması ile Azure Active Directory arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: 58d8a0be-5f1b-4680-bbcb-2975e5c57014
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/10/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 612de66bfdc2778a87685e0157ba8ef013ac51b1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67108346"
---
# <a name="tutorial-integrate-cyberark-saml-authentication-with-azure-active-directory"></a>Öğretici: CyberArk SAML kimlik doğrulaması Azure Active Directory ile tümleştirme

Bu öğreticide, CyberArk SAML kimlik doğrulaması Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. CyberArk SAML kimlik doğrulaması, Azure AD ile tümleştirdiğinizde, şunları yapabilirsiniz:

* Kimlerin erişebildiğini CyberArk SAML kimlik doğrulaması için Azure AD'de denetler.
* Otomatik olarak CyberArk SAML kimlik doğrulaması için kendi Azure AD hesapları ile oturum açmış olmasını sağlayın.
* Bir merkezi konumda - Azure portalı hesaplarınızı yönetin.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir aboneliğiniz yoksa, alabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free/).
* CyberArk SAML kimlik doğrulaması çoklu oturum açma (SSO) abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD SSO bir test ortamında test edin. CyberArk SAML kimlik doğrulamasını destekleyen **SP ve IDP** SSO başlattı.

## <a name="adding-cyberark-saml-authentication-from-the-gallery"></a>Galeriden CyberArk SAML kimlik doğrulaması ekleme

Azure AD'de CyberArk SAML kimlik doğrulaması tümleştirmesini yapılandırmak için CyberArk SAML kimlik doğrulaması Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde seçin **Azure Active Directory** hizmeti.
1. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları**.
1. Yeni bir uygulama eklemek için seçin **yeni uygulama**.
1. İçinde **Galeriden Ekle** bölümüne şunu yazın **CyberArk SAML kimlik doğrulaması** arama kutusuna.
1. Seçin **CyberArk SAML kimlik doğrulaması** gelen sonuçlar panelinde ve uygulama ekleyin. Uygulama, kiracınıza eklendiği sırada birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Yapılandırma ve test etme CyberArk SAML adlı bir test kullanıcı kullanarak kimlik doğrulaması ile Azure AD SSO **b Simon**. Çalışmak SSO için bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişki CyberArk SAML kimlik doğrulamasını kurmanız gerekir.

Yapılandırma ve Azure AD SSO CyberArk SAML kimlik doğrulaması ile test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. **[Azure AD SSO'yu yapılandırma](#configure-azure-ad-sso)**  kullanıcılarınız bu özelliği kullanmak etkinleştirmek için.
2. **[CyberArk SAML kimlik doğrulaması yapılandırma](#configure-cyberark-saml-authentication)**  uygulama tarafında SSO ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  Azure AD çoklu oturum açma b Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  Azure AD çoklu oturum açmayı kullanmak b Simon etkinleştirmek için.
5. **[CyberArk SAML kimlik doğrulaması test kullanıcısı oluşturma](#create-cyberark-saml-authentication-test-user)**  b Simon bir karşılığı CyberArk SAML kullanıcı Azure AD gösterimini bağlı kimlik doğrulaması sağlamak için.
6. **[Test SSO](#test-sso)**  yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO'yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek üzere aşağıdaki adımları izleyin.

1. İçinde [Azure portalında](https://portal.azure.com/), **CyberArk SAML kimlik doğrulaması** uygulama tümleştirme sayfası, bulma **Yönet** bölümünde ve seçin **çoklu oturum açma** .
1. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** sayfasında **SAML**.
1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında, düzenleme/kalem simgesine tıklayıp **temel SAML yapılandırma** ayarlarını düzenlemek için.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. Üzerinde **temel SAML yapılandırma** uygulamada yapılandırmak isterseniz, bölümü **IDP** modunda başlatılan aşağıdaki adımı uygulayın:

    İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:  `https://<PVWA DNS or IP>/passwordvault/api/auth/saml/logon`

1. Tıklayın **ek URL'lerini ayarlayın** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:  `https://<PVWA DNS or IP>/PasswordVault/v10/logon/saml`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek yanıt URL'si ve oturum açma URL'si ile güncelleştirin. İlgili kişi [CyberArk SAML kimlik doğrulaması istemci Destek ekibine](mailto:bizdevtech@cyberark.com) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında **SAML imzalama sertifikası** bölümünde, bulma **sertifika (ham)** seçip **indirme**sertifikayı indirin ve bilgisayarınıza kaydedin.

   ![Sertifika indirme bağlantısı](common/certificateraw.png)

1. Üzerinde **CyberArk SAML kimlik doğrulamasını ayarlama** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

   ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

### <a name="configure-cyberark-saml-authentication"></a>CyberArk SAML kimlik doğrulamasını yapılandırma

Çoklu oturum açmayı yapılandırma **CyberArk SAML kimlik doğrulaması** tarafı, indirilen göndermek için ihtiyacınız **sertifika (ham)** ve uygun Azure portalına kopyalanan URL'lerden [CyberArk SAML Kimlik doğrulaması Destek ekibine](mailto:bizdevtech@cyberark.com). Bunlar her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümde, bir test kullanıcısı b Simon adlı Azure portalında oluşturacaksınız.

1. Azure Portalı'ndaki sol bölmeden seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.
1. Seçin **yeni kullanıcı** ekranın üstünde.
1. İçinde **kullanıcı** özellikleri, aşağıdaki adımları izleyin:
   1. **Ad** alanına `B. Simon` girin.  
   1. İçinde **kullanıcı adı** alanına username@companydomain.extension. Örneğin, `B. Simon@contoso.com`.
   1. Seçin **Show parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.
   1. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, B. CyberArk SAML kimlik doğrulaması için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Simon tıklatmalarını sağlarsınız.

1. Azure portalında **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.
1. Uygulamalar listesinde **CyberArk SAML kimlik doğrulaması**.
1. Uygulamanın genel bakış sayfasında bulma **Yönet** seçin ve bölüm **kullanıcılar ve gruplar**.

   !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

1. Seçin **Kullanıcı Ekle**, ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Kullanıcı ekleme bağlantısı](common/add-assign-user.png)

1. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **b Simon** kullanıcılar listesinden ardından **seçin** ekranın alt kısmındaki düğmesi.
1. SAML onaylama işlemi herhangi bir rolü değer de beklediğiniz varsa **rolü Seç** iletişim kutusunda, listeden bir kullanıcı için uygun rolü seçin ve ardından **seçin** ekranın alt kısmındaki düğmesi.
1. İçinde **atama Ekle** iletişim kutusunda, tıklayın **atama** düğmesi.

### <a name="create-cyberark-saml-authentication-test-user"></a>CyberArk SAML kimlik doğrulaması test kullanıcısı oluşturma

Bu bölümde, Britta Simon CyberArk SAML kimlik doğrulaması adlı bir kullanıcı oluşturun. Çalışmak [CyberArk SAML kimlik doğrulaması Destek ekibine](mailto:bizdevtech@cyberark.com) CyberArk SAML kimlik doğrulaması platform kullanıcıları eklemek için. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi.

### <a name="test-sso"></a>Test SSO

Erişim Paneli'nde CyberArk SAML kimlik doğrulaması kutucuğu seçtiğinizde, otomatik olarak SSO'yu ayarlama CyberArk SAML kimlik doğrulaması için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)