---
title: 'Öğretici: SkyDesk e-posta ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve SkyDesk e-posta arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: a9d0bbcb-ddb5-473f-a4aa-028ae88ced1a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/07/2019
ms.author: jeedes
ms.openlocfilehash: e0c2dc6c370e697f896e24e7d56c6eb8900601a9
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59271059"
---
# <a name="tutorial-azure-active-directory-integration-with-skydesk-email"></a>Öğretici: SkyDesk e-posta ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile SkyDesk e-posta tümleştirme konusunda bilgi edinin.
Azure AD ile SkyDesk e-posta tümleştirme ile aşağıdaki avantajları sağlar:

* SkyDesk e-posta erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak SkyDesk e-posta (çoklu oturum açma) oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi SkyDesk e-posta ile yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Abonelik SkyDesk e-posta çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* SkyDesk e-posta destekler **SP** tarafından başlatılan

## <a name="adding-skydesk-email-from-the-gallery"></a>Galeriden SkyDesk e-posta ekleme

Azure AD'de SkyDesk e-posta tümleştirmesini yapılandırmak için SkyDesk e-posta Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden SkyDesk e-posta eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **SkyDesk e-posta**seçin **SkyDesk e-posta** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![SkyDesk e-posta sonuç listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırmanız ve SkyDesk e-posta ile Azure AD çoklu oturum açmayı test adlı bir test kullanıcı tabanlı **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının SkyDesk e-posta ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma SkyDesk e-posta ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[SkyDesk e-posta çoklu oturum açmayı yapılandırma](#configure-skydesk-email-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[SkyDesk e-posta test kullanıcısı oluşturma](#create-skydesk-email-test-user)**  - SkyDesk postada kullanıcının Azure AD gösterimini bağlı Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma SkyDesk e-posta ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **SkyDesk e-posta** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![SkyDesk e-posta etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-signonurl.png)

    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:  `https://mail.skydesk.jp/portal/<companyname>`

    > [!NOTE]
    > Değer, gerçek değil. Değerini gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [SkyDesk e-posta istemcisi Destek ekibine](https://www.skydesk.jp/apps/support/) değeri alınamıyor. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. Üzerinde **SkyDesk e-posta kurma** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-skydesk-email-single-sign-on"></a>SkyDesk e-posta çoklu oturum açmayı yapılandırın

1. Farklı bir web tarayıcısında, SkyDesk e-posta hesabınıza yönetici olarak oturum.

1. Üstteki menüden **Kurulum**seçip **kuruluş**.

    ![Çoklu oturum açmayı yapılandırın](./media/skydeskemail-tutorial/tutorial_skydeskemail_51.png)
  
1. Tıklayarak **etki alanları** sol panelden.

    ![Çoklu oturum açmayı yapılandırın](./media/skydeskemail-tutorial/tutorial_skydeskemail_53.png)

1. Tıklayarak **etki alanı ekleme**.

    ![Çoklu oturum açmayı yapılandırın](./media/skydeskemail-tutorial/tutorial_skydeskemail_54.png)

1. Etki alanı adınızı girin ve sonra etki alanını doğrulayın.

    ![Çoklu oturum açmayı yapılandırın](./media/skydeskemail-tutorial/tutorial_skydeskemail_55.png)

1. Tıklayarak **SAML kimlik doğrulaması** sol panelden.

    ![Çoklu oturum açmayı yapılandırın](./media/skydeskemail-tutorial/tutorial_skydeskemail_52.png)

1. Üzerinde **SAML kimlik doğrulaması** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/skydeskemail-tutorial/tutorial_skydeskemail_56.png)

    > [!NOTE]
    > SAML tabanlı kimlik doğrulaması kullanmak için ya da olması **etki alanını doğruladıysanız** veya **portal URL** kurulumu. Portal ayarlayabilirsiniz URL'si ile benzersiz adı.

    ![Çoklu oturum açmayı yapılandırın](./media/skydeskemail-tutorial/tutorial_skydeskemail_57.png)

    a. İçinde **oturum açma URL'si** metin değerini yapıştırın **oturum açma URL'si**, hangi Azure Portalı'ndan kopyaladığınız.

    b. İçinde **oturum kapatma** URL metin değerini yapıştırın **oturum kapatma URL'si**, hangi Azure Portalı'ndan kopyaladığınız.

    c. **Parola URL'sini değiştirmek** isteğe bağlı olduğundan bu nedenle boş bırakın.

    d. Tıklayarak **anahtarı dosyadan al** Azure portalından indirilen sertifikanızı seçin ve ardından **açık** sertifikayı karşıya yüklemek için.

    e. Olarak **algoritması**seçin **RSA**.

    f. Tıklayın **Tamam** değişiklikleri kaydedin.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü **brittasimon@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma SkyDesk e-postasına erişim verme tarafından kullanmak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **SkyDesk e-posta**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **SkyDesk e-posta**.

    ![Uygulamalar listesinde SkyDesk e-posta bağlantısı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-skydesk-email-test-user"></a>SkyDesk e-posta test kullanıcısı oluşturma

Bu bölümde, Britta Simon SkyDesk e-posta adlı bir kullanıcı oluşturun.

Tıklayarak **kullanıcı erişimini** Sol paneli SkyDesk e-postada ve kullanıcı adınızı girin.

![Çoklu oturum açmayı yapılandırın](./media/skydeskemail-tutorial/tutorial_skydeskemail_58.png)

> [!NOTE]
> Toplu kullanıcı oluşturmak ihtiyacınız varsa, iletişime geçmeniz [SkyDesk e-posta istemcisi Destek ekibine](https://www.skydesk.jp/apps/support/).

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli SkyDesk e-posta kutucuğa tıkladığınızda, size otomatik olarak SkyDesk SSO'yu ayarlama e-posta için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

