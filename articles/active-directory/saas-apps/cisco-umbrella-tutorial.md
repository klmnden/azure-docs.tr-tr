---
title: 'Öğretici: Cisco terimdir ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Cisco terimdir arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 675dca98-f119-4463-8350-d6a45d5601e3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/17/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9a1b0763e33607367939476ca155040295de864c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60281550"
---
# <a name="tutorial-azure-active-directory-integration-with-cisco-umbrella"></a>Öğretici: Cisco terimdir ile Azure Active Directory Tümleştirme

Bu öğreticide, Cisco terimdir Azure Active Directory (Azure AD) ile tümleştirmeyi öğrenin.
Cisco terimdir Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* Cisco genel erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak (çoklu oturum açma) Cisco terimdir için oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Cisco terimdir yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Cisco terimdir çoklu oturum açmayı abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Cisco terimdir destekler **SP ve IDP** tarafından başlatılan

## <a name="adding-cisco-umbrella-from-the-gallery"></a>Cisco genel galeri ekleme

Azure AD'de Cisco terimdir tümleştirmesini yapılandırmak için Cisco genel Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Cisco genel Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Cisco terimdir**seçin **Cisco terimdir** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde Cisco terimdir](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD'ye tek temelinde oturum açma adlı bir test kullanıcısı [uygulama adı] ile test etme **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ve [uygulama adı] ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma [uygulama adı] ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Cisco terimdir çoklu oturum açmayı yapılandırma](#configure-cisco-umbrella-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Cisco genel test kullanıcısı oluşturma](#create-cisco-umbrella-test-user)**  - kullanıcı Azure AD gösterimini bağlı Cisco terimdir Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma [uygulama adı] ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Cisco terimdir** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde kullanıcısının clonedatabase'i uygulama zaten Azure ile önceden tümleşik olarak herhangi bir adımı gerçekleştirmek.

    ![Cisco genel etki alanı ve URL'ler tek oturum açma bilgileri](common/both-preintegrated-signon.png)

    a. Uygulamada yapılandırmak istiyorsanız **SP** intiated modu, aşağıdaki adımları gerçekleştirin:

    b. Tıklayın **ek URL'lerini ayarlayın**.

    c. İçinde **oturum açma URL'si** metin kutusuna bir URL: `https://login.umbrella.com/sso`

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **meta veri XML**bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. Üzerinde **Cisco terimdir kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-cisco-umbrella-single-sign-on"></a>Cisco terimdir çoklu oturum açmayı yapılandırın

1. Farklı bir tarayıcı penceresinde, Cisco terimdir şirketinizin sitesi için yönetici olarak oturum.

2. Menü Sol taraftan tıklayın **yönetici** gidin **kimlik doğrulaması** ve ardından **SAML**.

    ![Yönetici](./media/cisco-umbrella-tutorial/tutorial_cisco-umbrella_admin.png)

3. Seçin **diğer** tıklayın **sonraki**.

    ![Diğer](./media/cisco-umbrella-tutorial/tutorial_cisco-umbrella_other.png)

4. Üzerinde **Cisco terimdir meta verileri**, sayfasında, **sonraki**.

    ![Meta veriler](./media/cisco-umbrella-tutorial/tutorial_cisco-umbrella_metadata.png)

5. Üzerinde **meta verilerini karşıya yükleme** sekmesinde, SAML, select önceden yapılandırmışsa **bunları değiştirmek için buraya tıklayın** izleyin ve seçenek aşağıdaki adımları.

    ![Sonraki](./media/cisco-umbrella-tutorial/tutorial_cisco-umbrella_next.png)

6. İçinde **y: seçeneği XML dosyasını karşıya yükleyin**, karşıya yükleme **Federasyon meta verileri XML** Azure portalından ve meta verilerini karşıya yükledikten sonra indirilen dosya aşağıdaki değerlerden otomatik olarak doldurulur otomatik alma sonra **sonraki**.

    ![Choosefile](./media/cisco-umbrella-tutorial/tutorial_cisco-umbrella_choosefile.png)

7. Altında **SAML yapılandırmayı doğrula** bölümünde **TEST YAPILANDIRMANIZI SAML**.

    ![Test](./media/cisco-umbrella-tutorial/tutorial_cisco-umbrella_test.png)

8. **KAYDET**'e tıklayın.

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

Bu bölümde, Azure çoklu oturum açma kullanmak için Cisco genel erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Cisco terimdir**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde yazın ve **Cisco terimdir**.

    ![Uygulamalar listesinde Cisco genel bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-cisco-umbrella-test-user"></a>Cisco genel test kullanıcısı oluşturma

Cisco terimdir için oturum açmak Azure AD kullanıcılarının etkinleştirmek için Cisco terimdir sağlanması gerekir.  
Cisco terimdir söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Farklı bir tarayıcı penceresinde, Cisco terimdir şirketinizin sitesi için yönetici olarak oturum.

2. Menü Sol taraftan tıklayın **yönetici** gidin **hesapları**.

    ![Hesap](./media/cisco-umbrella-tutorial/tutorial_cisco-umbrella_account.png)

3. Üzerinde **hesapları** sayfasında, tıklayarak **Ekle** sayfanın üst sağ taraftaki ve aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı](./media/cisco-umbrella-tutorial/tutorial_cisco-umbrella_createuser.png)

    a. İçinde **ad** gibi ad alanına, **Britta**.

    b. İçinde **Soyadı** gibi bir soyadı girin **simon**.

    c. Gelen **temsilci yönetici rolünü seçin**, rolünüzü seçin.
  
    d. İçinde **e-posta adresi** alanına, kullanıcının gibi emailaddress **brittasimon\@contoso.com**.

    e. İçinde **parola** parolanızı girin.

    f. İçinde **parolayı onayla** parolanızı yeniden girin.

    g. Tıklayın **Oluştur**.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Cisco terimdir kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama Cisco terimdir için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
