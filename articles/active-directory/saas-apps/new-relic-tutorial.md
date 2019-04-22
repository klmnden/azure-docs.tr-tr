---
title: 'Öğretici: New Relic ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: New Relic ile Azure Active Directory arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 3186b9a8-f4d8-45e2-ad82-6275f95e7aa6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/04/2019
ms.author: jeedes
ms.openlocfilehash: 4d50c4366128edb5b4bc76408eb1085817af1ce3
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59271909"
---
# <a name="tutorial-azure-active-directory-integration-with-new-relic"></a>Öğretici: New Relic ile Azure Active Directory Tümleştirme

Bu öğreticide, New Relic, Azure Active Directory (Azure AD) ile tümleştirme konusunda bilgi edinin.
New Relic, Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* New Relic erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak (çoklu oturum açma) için New Relic oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

New Relic ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Yeni Relic çoklu oturum açma abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Yeni Relic destekler **SP** tarafından başlatılan

## <a name="adding-new-relic-from-the-gallery"></a>New Relic galeri ekleme

New Relic tümleştirmesi Azure AD'de yapılandırmak için New Relic Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**New Relic Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **New Relic**seçin **New Relic** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde yeni Relic](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma adlı bir test kullanıcı tabanlı New Relic ile test etme **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ve New Relic ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma New Relic ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Yeni Relic çoklu oturum açmayı yapılandırma](#configure-new-relic-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[New Relic test kullanıcısı oluşturma](#create-new-relic-test-user)**  - kullanıcı Azure AD gösterimini bağlı New Relic Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma New Relic ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **New Relic** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açma bilgileri yeni Relic etki alanı ve URL'ler](common/sp-identifier.png)

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://rpm.newrelic.com/accounts/{acc_id}/sso/saml/login` -kendi yeni Relic hesabı kimliği değiştirdiğinizden emin olun

    b. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna bir URL yazın: `rpm.newrelic.com`

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. Üzerinde **New Relic ayarlamak** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-new-relic-single-sign-on"></a>Yeni Relic çoklu oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde oturum açın, **New Relic** şirketinizin sitesi yöneticisi olarak.

2. Üstteki menüden **hesap ayarları**.
   
    ![Hesap ayarları](./media/new-relic-tutorial/ic797036.png "hesap ayarları")

3. Tıklayın **güvenlik ve kimlik doğrulama** sekmesine ve ardından **tekli oturum** sekmesi.
   
    ![Çoklu oturum açma](./media/new-relic-tutorial/ic797037.png "çoklu oturum açma")

4. SAML iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![SAML](./media/new-relic-tutorial/ic797038.png "SAML")
   
    a. Tıklayın **Dosya Seç** indirilen Azure Active Directory sertifikanızı karşıya yüklemek için.

    b. İçinde **uzaktan oturum açma URL'si** metin değerini yapıştırın **oturum açma URL'si**, hangi Azure Portalı'ndan kopyaladığınız.
   
    c. İçinde **oturum kapatma URL'si** metin değerini yapıştırın **oturum kapatma URL'si**, hangi Azure Portalı'ndan kopyaladığınız.

    d. Tıklayın **Değişikliklerimi Kaydet**.

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

Bu bölümde, Azure çoklu oturum açmayı kullanmak için New Relic erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **New Relic**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **New Relic**.

    ![New Relic bağlantıya uygulamalar listesi](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-new-relic-test-user"></a>New Relic test kullanıcısı oluşturma

Azure Active Directory Kullanıcıları için New Relic oturum açmayı etkinleştirmek için bunlar New Relic ile sağlanması gerekir. New Relic söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı için New Relic sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açın, **New Relic** şirketinizin sitesi yöneticisi olarak.

2. Üstteki menüden **hesap ayarları**.
   
    ![Hesap ayarları](./media/new-relic-tutorial/ic797040.png "hesap ayarları")

3. İçinde **hesabı** bölmesi sol tarafta **özeti**ve ardından **Kullanıcı Ekle**.
   
    ![Hesap ayarları](./media/new-relic-tutorial/ic797041.png "hesap ayarları")

4. Üzerinde **etkin kullanıcılar** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
    ![Etkin kullanıcılar](./media/new-relic-tutorial/ic797042.png "etkin kullanıcılar")
   
    a. İçinde **e-posta** metin sağlamak istediğiniz geçerli bir Azure Active Directory kullanıcı e-posta adresini yazın.

    b. Olarak **rol** seçin **kullanıcı**.

    c. Tıklayın **bu kullanıcıyı eklemek**.

>[!NOTE]
>API'leri, AAD kullanıcı hesapları sağlamak için New Relic tarafından sağlanan ya da tüm diğer New Relic kullanıcı hesabı oluşturma araçları kullanabilirsiniz.
> 

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli New Relic kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama New Relic için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

