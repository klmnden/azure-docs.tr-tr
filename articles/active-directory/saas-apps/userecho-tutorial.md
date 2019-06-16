---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile UserEcho | Microsoft Docs'
description: Azure Active Directory ve UserEcho arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: bedd916b-8f69-4b50-9b8d-56f4ee3bd3ed
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/29/2019
ms.author: jeedes
ms.openlocfilehash: 59d61eda7002fe46cf99fac63822b2333b2d64b5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67087760"
---
# <a name="tutorial-azure-active-directory-integration-with-userecho"></a>Öğretici: UserEcho ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile UserEcho tümleştirme konusunda bilgi edinin.
Azure AD ile UserEcho tümleştirme ile aşağıdaki avantajları sağlar:

* UserEcho erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak (çoklu oturum açma) UserEcho için kendi Azure AD hesapları ile oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile UserEcho yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa alabileceğiniz bir [ücretsiz hesap](https://azure.microsoft.com/free/)
* Abonelik UserEcho çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* UserEcho destekler **SP** tarafından başlatılan

## <a name="adding-userecho-from-the-gallery"></a>Galeriden UserEcho ekleme

Azure AD'de UserEcho tümleştirmesini yapılandırmak için UserEcho Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden UserEcho eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **UserEcho**seçin **UserEcho** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde UserEcho](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma UserEcho adlı bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının UserEcho ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma UserEcho ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[UserEcho çoklu oturum açmayı yapılandırma](#configure-userecho-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[UserEcho test kullanıcısı oluşturma](#create-userecho-test-user)**  - kullanıcı Azure AD gösterimini bağlı UserEcho Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile UserEcho yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **UserEcho** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![UserEcho etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<companyname>.userecho.com/`

    b. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<companyname>.userecho.com/saml/metadata/`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL ve tanımlayıcıdır ile güncelleştirin. İlgili kişi [UserEcho istemci Destek ekibine](https://feedback.userecho.com/) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

4. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. Üzerinde **UserEcho kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-userecho-single-sign-on"></a>UserEcho tek oturum açmayı yapılandırın

1. Başka bir tarayıcı penceresinde UserEcho şirketinizin sitesi için bir yönetici olarak oturum açın.

2. Üst araç çubuğunda, menüyü genişletmek için kullanıcı adınıza tıklayın ve ardından **Kurulum**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/userecho-tutorial/tutorial_userecho_06.png) 

3. Tıklayın **tümleştirmeler**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/userecho-tutorial/tutorial_userecho_07.png) 

4. Tıklayın **Web sitesi**ve ardından **çoklu oturum açma (SAML2)** .
   
    ![Çoklu oturum açmayı yapılandırın](./media/userecho-tutorial/tutorial_userecho_08.png) 

5. Üzerinde **çoklu oturum açma (SAML)** sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açmayı yapılandırın](./media/userecho-tutorial/tutorial_userecho_09.png)
    
    a. Olarak **SAML etkin**seçin **Evet**.
    
    b. Yapıştırma **oturum açma URL'si**, hangi Azure portaldan kopyaladığınız **SAML SSO URL** metin.
    
    c. Yapıştırma **oturum kapatma URL'si**, hangi Azure portaldan kopyaladığınız **uzak oturum kapatma URL'si** metin.
    
    d. İndirilen sertifikanızı Not Defteri'nde açın, içeriği kopyalayın ve ardından yapıştırın **X.509 sertifikası** metin.
    
    e. **Kaydet**’e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü brittasimon@yourcompanydomain.extension. Örneğin, BrittaSimon@contoso.com

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için UserEcho erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **UserEcho**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **UserEcho**.

    ![Uygulamalar listesinde UserEcho bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-userecho-test-user"></a>UserEcho test kullanıcısı oluşturma

Bu bölümün amacı UserEcho Britta Simon adlı bir kullanıcı oluşturmaktır.

**Britta Simon UserEcho içinde adlı bir kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. UserEcho şirketinizin sitesi için bir yönetici olarak oturum.

2. Üst araç çubuğunda, menüyü genişletmek için kullanıcı adınıza tıklayın ve ardından **Kurulum**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/userecho-tutorial/tutorial_userecho_06.png)

3. Tıklayın **kullanıcılar**genişletmek için **kullanıcılar** bölümü.
   
    ![Çoklu oturum açmayı yapılandırın](./media/userecho-tutorial/tutorial_userecho_10.png)

4. **Kullanıcılar**’a tıklayın.
   
    ![Çoklu oturum açmayı yapılandırın](./media/userecho-tutorial/tutorial_userecho_11.png)

5. Tıklayın **yeni bir kullanıcı davet**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/userecho-tutorial/tutorial_userecho_12.png)

6. Üzerinde **yeni bir kullanıcı davet** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açmayı yapılandırın](./media/userecho-tutorial/tutorial_userecho_13.png)

    a. İçinde **adı** metin kutusu, kullanıcının Britta Simon gibi tür adı.
    
    b.  İçinde **e-posta** metin kutusuna kullanıcı e-posta adresi türünü ister Brittasimon@contoso.com.
    
    c. Tıklayın **davet**.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli UserEcho kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama UserEcho için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

