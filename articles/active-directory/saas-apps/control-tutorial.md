---
title: 'Öğretici: Denetim ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve sürekliliği denetim arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 1cb7f505-0d06-44b0-95b1-65b470e97092
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/16/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: aa66ae77ccc271e475d61b286e0f236429e40feb
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66507499"
---
# <a name="tutorial-integrate-continuity-control-with-azure-active-directory"></a>Öğretici: Sürekliliği denetimi Azure Active Directory ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile sürekliliği denetim (Denetim) tümleştirme öğreneceksiniz. Denetim Azure AD ile tümleştirdiğinizde, şunları yapabilirsiniz:

* Denetim erişimi, Azure AD'de yönetin.
* Otomatik olarak denetlemek için kendi Azure AD hesapları ile oturum açmış olmasını sağlayın.
* Bir merkezi konumda - Azure portalı hesaplarınızı yönetin.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir aboneliğiniz yoksa, bir aylık ücretsiz deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).
* Bir Denetim Çoklu oturum açma (SSO) abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD SSO bir test ortamında test edin. Denetim destekler **SP** SSO başlattı.

## <a name="adding-control-from-the-gallery"></a>Galeri denetimi ekleme

Azure AD'de denetim tümleştirmesini yapılandırmak için Denetim Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde seçin **Azure Active Directory** hizmeti.
1. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları**.
1. Yeni bir uygulama eklemek için seçin **yeni uygulama**.
1. İçinde **Galeriden Ekle** bölümüne şunu yazın **denetimi** arama kutusuna.
1. Seçin **denetimi** gelen sonuçlar panelinde ve uygulama ekleyin. Uygulama, kiracınıza eklendiği sırada birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Yapılandırma ve Azure AD SSO kullanarak adlı bir test kullanıcı denetimi ile test etme **Britta Simon**. Çalışmak SSO için denetiminde bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişki oluşturmanız gerekir.

Yapılandırma ve Azure AD SSO denetimi ile test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. **[Azure AD SSO'yu yapılandırma](#configure-azure-ad-sso)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Denetim SSO'yu yapılandırarak](#configure-control-sso)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Denetim test kullanıcısı oluşturma](#create-control-test-user)**  - kullanıcı Azure AD gösterimini bağlı denetiminde Britta simon'un bir karşılığı vardır.
6. **[Test SSO](#test-sso)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO'yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek üzere aşağıdaki adımları izleyin.

1. İçinde [Azure portalında](https://portal.azure.com/), **denetimi** uygulama tümleştirme sayfası, bulma **Yönet** bölümünde ve seçin **çoklu oturum açma**.
1. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** sayfasında **SAML**.
1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında, düzenleme/kalem simgesine tıklayıp **temel SAML yapılandırma** ayarlarını düzenlemek için.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. Üzerinde **temel SAML yapılandırma** sayfasında, şu alan için değerleri girin:

    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:  `https://<SUBDOMAIN>.continuity.net/auth/saml`

    > [!Note]
    > Değer, gerçek değil. Değer doğru alt etki alanı ile güncelleştirin. Konumunda, SSO alt etki alanı yapılandırılabilir [denetim kimlik doğrulama stratejileri](https://control.continuity.net/settings/account_profile#tab/security). Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

1. İçinde **SAML imzalama sertifikası** bölümünde **Düzenle** açmak için düğmeyi **SAML imzalama sertifikası** iletişim.

    ![SAML imzalama sertifikası Düzenle](common/edit-certificate.png)

1. İçinde **SAML imzalama sertifikası** bölümünde, kopya **parmak izi** ve bilgisayarınıza kaydedin.

    ![Parmak izi değerini kopyalayın](common/copy-thumbprint.png)

1. Üzerinde **denetimini Ayarla** bölümünde oturum açma URL'sini kopyalayın ve bilgisayarınıza kaydedin.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

### <a name="configure-control-sso"></a>Denetim SSO yapılandırma

Çoklu oturum açmayı yapılandırma **denetimi** tarafı, çoklu oturum açma kimlik doğrulaması ayarlarını güncelleştirmek için ihtiyacınız [denetim kimlik doğrulama stratejileri](https://control.continuity.net/settings/account_profile#tab/security). Güncelleştirme **SAML SSO URL** ile **oturum açma URL'si** ve **sertifika parmak izi** ile **parmak izi değerini** Azure portalından.

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

Bu bölümde, denetim için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon tıklatmalarını sağlarsınız.

1. Azure portalında **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.
1. Uygulamalar listesinde **denetim**.
1. Uygulamanın genel bakış sayfasında bulma **Yönet** seçin ve bölüm **kullanıcılar ve gruplar**.

   !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

1. Seçin **Kullanıcı Ekle**, ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Kullanıcı ekleme bağlantısı](common/add-assign-user.png)

1. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcılar listesinden ardından **seçin** ekranın alt kısmındaki düğmesi.
1. SAML onaylama işlemi herhangi bir rolü değer de beklediğiniz varsa **rolü Seç** iletişim kutusunda, listeden bir kullanıcı için uygun rolü seçin ve ardından **seçin** ekranın alt kısmındaki düğmesi.
1. İçinde **atama Ekle** iletişim kutusunda, tıklayın **atama** düğmesi.

### <a name="create-control-test-user"></a>Denetim test kullanıcısı oluşturma

Bu bölümde, Britta Simon denetiminde adlı bir kullanıcı oluşturun. Çalışmak [denetim Destek ekibine](mailto:help@continuity.net) denetim platform kullanıcıları eklemek için. Britta Simon'ın kullanan Azure AD **kullanıcı adı** her doldurmak için **kimlik sağlayıcı kullanıcı kimliği** denetimi. Kullanıcılar oluşturulmalıdır ve bunların **kimlik sağlayıcı kullanıcı kimliği** , çoklu oturum açma kullanabilmeniz için önce denetiminde ayarlayın.

### <a name="test-sso"></a>Test SSO

Erişim panelinde denetimi kutucuğu seçtiğinizde, otomatik olarak SSO'yu ayarlama denetlemek için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [ SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
