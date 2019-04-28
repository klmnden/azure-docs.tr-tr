---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile 8x8lik sanal Office | Microsoft Docs'
description: Azure Active Directory ve 8x8lik sanal Office arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: b34a6edf-e745-4aec-b0b2-7337473d64c5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/27/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9385ec6a86c24e619ffafdae67bc66f66e099f3b
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62117239"
---
# <a name="tutorial-azure-active-directory-integration-with-8x8-virtual-office"></a>Öğretici: 8x8lik sanal Office ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile 8x8lik sanal Office tümleştirme konusunda bilgi edinin.
8x8lik tümleştirmek Azure AD ile sanal Office ile aşağıdaki avantajları sağlar:

* Kimlerin erişebildiğini 8x8lik sanal Office için Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak (çoklu oturum açma) 8x8lik sanal Office oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi 8x8lik sanal Office ile yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Abonelik 8x8lik sanal Office çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.


* Sanal Office 8x8lik destekler **IDP** tarafından başlatılan

* Sanal Office 8x8lik destekler **zamanında** kullanıcı sağlama

## <a name="adding-8x8-virtual-office-from-the-gallery"></a>Galeriden 8x8lik sanal Office ekleme

Azure AD'de 8x8lik sanal Office tümleştirmesini yapılandırmak için 8x8lik sanal Office Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden 8x8lik sanal Office eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **8x8lik sanal Office**seçin **8x8lik sanal Office** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![sonuç listesinde 8x8lik sanal Office](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma 8 x 8 sanal Office tabanlı adlı bir test kullanıcı ile test etme **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının 8x8lik ilgili kullanıcı arasında bir bağlantı ilişki sanal Office kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma 8x8lik sanal Office ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[8x8lik yapılandırma sanal Office çoklu oturum açma](#configure-8x8-virtual-office-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[8x8lik sanal Office test kullanıcısı oluşturma](#create-8x8-virtual-office-test-user)**  - kullanıcı Azure AD gösterimini bağlı 8x8lik sanal Office Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma 8x8lik sanal Office ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **8x8lik sanal Office** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![8x8lik sanal Office etki alanı ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. İçinde **tanımlayıcı** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://sso.8x8.com/saml2`

    b. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://sso.8x8.com/saml2`

4. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (ham)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificateraw.png)

6. Üzerinde **8x8lik sanal ofis Kur** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-8x8-virtual-office-single-sign-on"></a>8x8lik sanal Office çoklu oturum açmayı yapılandırın

1. 8x8lik sanal Office kiracınıza yönetici olarak oturum.

1. Seçin **sanal Office hesabı Mgr** uygulama panosunda.

    ![Uygulama tarafında yapılandırma](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_001.png)

1. Seçin **iş** yönetmek ve hesap **oturum** düğmesi.

    ![Uygulama tarafında yapılandırma](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_002.png)

1. Tıklayın **HESAPLARI** menü listesi sekmesindedir.

   ![Uygulama tarafında yapılandırma](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_003.png)

1. Tıklayın **çoklu oturum açma** hesapları listesinde.
  
   ![Uygulama tarafında yapılandırma](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_004.png)

1. Seçin **çoklu oturum açma** kimlik doğrulama yöntemleri ve tıklatın altında **SAML**.

   ![Uygulama tarafında yapılandırma](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_005.png)

1. İçinde **SAML çoklu oturum açma** bölümünde, aşağıdaki adımları gerçekleştirin:

   ![Uygulama tarafında yapılandırma](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_006.png)

   a. İçinde **oturum açma URL'si** metin kutusu, yapıştırma **oturum açma URL'si** Azure portaldan kopyaladığınız değeri.

   b. İçinde **oturum kapatma URL'si** metin kutusu, yapıştırma **oturum kapatma URL'si** Azure portaldan kopyaladığınız değeri.

   c. İçinde **veren URL'si** metin kutusu, yapıştırma **Azure AD tanımlayıcısı** Azure portaldan kopyaladığınız değeri.

   d. Tıklayın **Gözat** Azure portalından indirilen sertifikayı karşıya yüklemek için düğme.

   e. **Kaydet** düğmesine tıklayın.

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

Bu bölümde, 8x8lik sanal Office için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **8x8lik sanal Office**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde yazın ve **8x8lik sanal Office**.

    ![Uygulamalar listesinde 8x8lik sanal Office bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-8x8-virtual-office-test-user"></a>8x8lik sanal Office test kullanıcısı oluşturma

Bu bölümde, Britta Simon adlı bir kullanıcı 8x8lik sanal Office içinde oluşturulur. Sanal Office 8x8lik destekler **just-ın-time kullanıcı sağlamayı**, varsayılan olarak etkindir. Bu bölümde, hiçbir eylem öğesini yoktur. Bir kullanıcı 8x8lik sanal Office'te zaten mevcut değilse yeni bir kimlik doğrulamasından sonra oluşturulur.

> [!NOTE]
> Bir kullanıcı el ile oluşturmanız gerekiyorsa, iletişime geçmeniz [8x8lik sanal Office Destek ekibine](https://www.8x8.com/about-us/contact-us).

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli 8x8lik sanal Office kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama 8x8lik sanal Office için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

