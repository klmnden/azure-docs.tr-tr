---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile CylancePROTECT | Microsoft Docs'
description: Azure Active Directory ve CylancePROTECT arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: ea392d8c-c8aa-4475-99d0-b08524ef0f3a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/02/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 82da2aec617841643d083662639ebde1b4513ed0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67104356"
---
# <a name="tutorial-azure-active-directory-integration-with-cylanceprotect"></a>Öğretici: CylancePROTECT ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile CylancePROTECT tümleştirme konusunda bilgi edinin.
Azure AD ile CylancePROTECT tümleştirme ile aşağıdaki avantajları sağlar:

* CylancePROTECT erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak (çoklu oturum açma) CylancePROTECT için kendi Azure AD hesapları ile oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile CylancePROTECT yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Abonelik CylancePROTECT çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* CylancePROTECT destekler **IDP** tarafından başlatılan

## <a name="adding-cylanceprotect-from-the-gallery"></a>Galeriden CylancePROTECT ekleme

Azure AD'de CylancePROTECT tümleştirmesini yapılandırmak için CylancePROTECT Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden CylancePROTECT eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **CylancePROTECT**seçin **CylancePROTECT** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde CylancePROTECT](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma CylancePROTECT adlı bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının CylancePROTECT ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma CylancePROTECT ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[CylancePROTECT çoklu oturum açmayı yapılandırma](#configure-cylanceprotect-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[CylancePROTECT test kullanıcısı oluşturma](#create-cylanceprotect-test-user)**  - kullanıcı Azure AD gösterimini bağlı CylancePROTECT Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile CylancePROTECT yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **CylancePROTECT** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için düğmeyi **temel SAML yapılandırma** iletişim.

    ![CylancePROTECT etki alanı ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. İçinde **tanımlayıcı** metin kutusuna URL'yi yazın:
    
    | Bölge | URL değeri |
    |----------|---------|
    | Asya Pasifik Kuzeydoğu (APNE1)| `https://login-apne1.cylance.com/EnterpriseLogin/ConsumeSaml`|
    | Asya Pasifik Güneydoğu (AU) | `https://login-au.cylance.com/EnterpriseLogin/ConsumeSaml` |
    | Avrupa, Orta (EUC1)|`https://login-euc1.cylance.com/EnterpriseLogin/ConsumeSaml`|
    | Kuzey Amerika|`https://login.cylance.com/EnterpriseLogin/ConsumeSaml`|
    | Güney Amerika (SAE1)|`https://login-sae1.cylance.com/EnterpriseLogin/ConsumeSaml`|

    b. İçinde **yanıt URL'si** metin kutusuna URL'yi yazın:
    
    | Bölge | URL değeri |
    |----------|---------|
    | Asya Pasifik Kuzeydoğu (APNE1)|`https://login-apne1.cylance.com/EnterpriseLogin/ConsumeSaml`|
    | Asya Pasifik Güneydoğu (AU)|`https://login-au.cylance.com/EnterpriseLogin/ConsumeSaml`|
    | Avrupa, Orta (EUC1)|`https://login-euc1.cylance.com/EnterpriseLogin/ConsumeSaml`|
    | Kuzey Amerika|`https://login.cylance.com/EnterpriseLogin/ConsumeSaml`|
    | Güney Amerika (SAE1)|`https://login-sae1.cylance.com/EnterpriseLogin/ConsumeSaml`|

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. Üzerinde **CylancePROTECT kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-cylanceprotect-single-sign-on"></a>CylancePROTECT tek oturum açmayı yapılandırın

Çoklu oturum açmayı yapılandırma **CylancePROTECT** tarafı, indirilen göndermek için ihtiyacınız **sertifika (Base64)** ve uygun URL'ler Azure portalından Konsolu yönetici. Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü **brittasimon\@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için CylancePROTECT erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **CylancePROTECT**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **CylancePROTECT**.

    ![Uygulamalar listesinde CylancePROTECT bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-cylanceprotect-test-user"></a>CylancePROTECT test kullanıcısı oluşturma

Bu bölümde, Britta Simon CylancePROTECT içinde adlı bir kullanıcı oluşturun. CylancePROTECT platform kullanıcıları eklemek için Yönetici Konsolu ile çalışma. Azure Active Directory hesap sahibinin e-posta alır ve bunu etkinleştirilmeden önce hesabını onaylamak için bağlantıyı izleyin.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli CylancePROTECT kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama CylancePROTECT için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

