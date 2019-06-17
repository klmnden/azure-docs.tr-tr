---
title: 'Öğretici: UserVoice ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve UserVoice arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 684a405b-8932-46f6-b43a-4d97a42b6b87
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/29/2019
ms.author: jeedes
ms.openlocfilehash: c0c259d3d05232aa70016771e2a2bce7622730a0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67087631"
---
# <a name="tutorial-azure-active-directory-integration-with-uservoice"></a>Öğretici: UserVoice ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile UserVoice tümleştirme konusunda bilgi edinin.
UserVoice Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* UserVoice erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak (çoklu oturum açma) Uservoice'a kendi Azure AD hesapları ile oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

UserVoice ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa alabileceğiniz bir [ücretsiz hesap](https://azure.microsoft.com/free/)
* UserVoice çoklu oturum açma abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* UserVoice destekler **SP** tarafından başlatılan

## <a name="adding-uservoice-from-the-gallery"></a>UserVoice galeri ekleme

Azure AD'de UserVoice tümleştirmesini yapılandırmak için UserVoice Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden UserVoice eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **UserVoice**seçin **UserVoice** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde UserVoice](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma UserVoice adlı bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ve UserVoice ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma UserVoice ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[UserVoice çoklu oturum açmayı yapılandırma](#configure-uservoice-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[UserVoice test kullanıcısı oluşturma](#create-uservoice-test-user)**  - kullanıcı Azure AD gösterimini bağlı UserVoice Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma UserVoice ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **UserVoice** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![UserVoice etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<tenantname>.UserVoice.com`

    b. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<tenantname>.UserVoice.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL ve tanımlayıcıdır ile güncelleştirin. İlgili kişi [UserVoice istemci Destek ekibine](https://www.uservoice.com/) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. İçinde **SAML imzalama sertifikası** bölümünde **Düzenle** açmak için düğmeyi **SAML imzalama sertifikası** iletişim.

    ![SAML imzalama sertifikası Düzenle](common/edit-certificate.png)

6. İçinde **SAML imzalama sertifikası** bölümünde, kopya **parmak izi** ve bilgisayarınıza kaydedin.

    ![Parmak izi değerini kopyalayın](common/copy-thumbprint.png)

7. Üzerinde **UserVoice kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-uservoice-single-sign-on"></a>UserVoice tek oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde UserVoice şirketinizin sitesi için bir yönetici olarak oturum açın.

2. Üst araç çubuğunda tıklatın **ayarları**ve ardından **Web portalı** menüsünde.
   
    ![Uygulama tarafında ayarları bölümündeki](./media/uservoice-tutorial/ic777519.png "ayarları")

3. Üzerinde **Web portalı** sekmesinde **kullanıcı kimlik doğrulaması** bölümünde **Düzenle** açmak için **kullanıcı kimlik doğrulamasını Düzenle** iletişim sayfası.
   
    ![Web portalı sekmesini](./media/uservoice-tutorial/ic777520.png "Web portalı")

4. Üzerinde **kullanıcı kimlik doğrulamasını Düzenle** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Kullanıcı kimlik doğrulamasını Düzenle](./media/uservoice-tutorial/ic777521.png "düzenleme kullanıcı kimlik doğrulaması")
   
    a. Tıklayın **çoklu oturum açma (SSO)** .
 
    b. Yapıştırma **oturum açma URL'si** Azure portaldan kopyaladığınız değeri **SSO uzaktan oturum açma** metin.

    c. Yapıştırma **oturum kapatma URL'si** Azure portaldan kopyaladığınız değeri **SSO uzak oturum kapatma textbox**.
 
    d. Yapıştırma **parmak izi** Azure portaldan kopyaladığınız değeri **geçerli sertifikası SHA1 parmak izi** metin.
    
    e. Tıklayın **kimlik doğrulama ayarlarını kaydetme**.

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

Bu bölümde, Azure çoklu oturum açma kullanmak için UserVoice erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **UserVoice**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **UserVoice**.

    ![Uygulamalar listesini UserVoice bağlantıdaki](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-uservoice-test-user"></a>UserVoice test kullanıcısı oluşturma

Oturum açmak için UserVoice Azure AD kullanıcılarının etkinleştirmek için bunların UserVoice sağlanması gerekir. UserVoice söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

### <a name="to-provision-a-user-account-perform-the-following-steps"></a>Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:

1. Oturum açın, **UserVoice** Kiracı.

2. Git **ayarları**.
   
    ![Ayarları](./media/uservoice-tutorial/ic777811.png "ayarları")

3. Tıklayın **genel**.

4. Tıklayın **aracıları ve izinleri**.
   
    ![Aracıları ve izinleri](./media/uservoice-tutorial/ic777812.png "aracıları ve izinleri")

5. Tıklayın **yöneticileri ekleyin**.
   
    ![Yönetici eklemek](./media/uservoice-tutorial/ic777813.png "yöneticileri ekleyin")

6. Üzerinde **davet yöneticileri** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
    ![Yöneticiler davet](./media/uservoice-tutorial/ic777814.png "davet yöneticileri")
   
    a. E-postaları metin kutusunda sağlamak istediğiniz ve ardından hesabın e-posta adresini yazın **Ekle**.
   
    b. Tıklayın **davet**.

> [!NOTE]
> API'leri, AAD kullanıcı hesapları sağlamak için UserVoice tarafından sağlanan ya da tüm diğer UserVoice kullanıcı hesabı oluşturma araçları kullanabilirsiniz.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli UserVoice kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama UserVoice için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

