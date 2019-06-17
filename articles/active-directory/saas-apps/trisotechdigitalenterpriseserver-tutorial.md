---
title: 'Öğretici: Trisotech dijital kuruluş sunucusu ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory Trisotech dijital kuruluş sunucusu arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 6d54d20c-eca1-4fa6-b56a-4c3ed0593db0
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/28/2019
ms.author: jeedes
ms.openlocfilehash: 56399f99ede611c4a120603cce3a3eede2728c6d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67088257"
---
# <a name="tutorial-azure-active-directory-integration-with-trisotech-digital-enterprise-server"></a>Öğretici: Azure Active Directory tümleştirmesiyle Trisotech dijital kuruluş sunucusu

Bu öğreticide, Azure Active Directory (Azure AD) ile Trisotech dijital Enterprise Server Tümleştirme konusunda bilgi edinin.
Azure AD ile Trisotech dijital Enterprise Server Tümleştirme ile aşağıdaki avantajları sağlar:

* Trisotech dijital kuruluş sunucusuna erişimi olan Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak Trisotech dijital kuruluş sunucusuna (çoklu oturum açma) oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Trisotech dijital Kurumsal sunucusuyla yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Abonelik Trisotech dijital kuruluş sunucusu çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Trisotech dijital Enterprise Server destekler **SP** tarafından başlatılan

* Trisotech dijital Enterprise Server destekler **zamanında** kullanıcı sağlama

## <a name="adding-trisotech-digital-enterprise-server-from-the-gallery"></a>Galeriden Trisotech dijital kuruluş sunucusu ekleme

Azure AD'de Trisotech dijital Enterprise Server tümleştirmesini yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Trisotech dijital kuruluş sunucusu eklemeniz gerekir.

**Galeriden Trisotech dijital Kurumsal sunucusu eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Trisotech dijital Enterprise Server**seçin **Trisotech dijital Enterprise Server** sonucu panelinden ardından **Ekle** düğme eklemek için uygulama.

     ![Sonuç listesinde Trisotech dijital kuruluş sunucusu](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Trisotech dijital Kurumsal adlı bir test kullanıcı tabanlı sunucuyu test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ve ilgili kullanıcı Trisotech dijital kuruluş sunucusu arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Trisotech dijital kuruluş sunucusu ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Trisotech dijital Kurumsal sunucusu çoklu oturum açmayı yapılandırma](#configure-trisotech-digital-enterprise-server-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Trisotech dijital Enterprise Server test kullanıcısı oluşturma](#create-trisotech-digital-enterprise-server-test-user)**  - Trisotech dijital Kurumsal Server'da kullanıcı Azure AD gösterimini bağlı Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma Trisotech dijital Kurumsal sunucusuyla yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Trisotech dijital Enterprise Server** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Trisotech dijital Kurumsal sunucusu etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<companyname>.trisotech.com`

    b. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<companyname>.trisotech.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL ve tanımlayıcıdır ile güncelleştirin. İlgili kişi [Trisotech dijital Kurumsal sunucusu istemci Destek ekibine](mailto:support@trisotech.com) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

4. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında **SAML imzalama sertifikası** bölümünde, kopyalamak için Kopyala düğmesine **uygulama Federasyon meta verileri URL'sini** ve üzerinde kaydedin, bilgisayar.

    ![Sertifika indirme bağlantısı](common/copy-metadataurl.png)

### <a name="configure-trisotech-digital-enterprise-server-single-sign-on"></a>Trisotech dijital Kurumsal sunucusu çoklu oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde Trisotech dijital Enterprise sunucu yapılandırması şirketinizin sitesi için bir yönetici olarak oturum açın.

2. Tıklayarak **menüsü simgesi** seçip **Yönetim**.

    ![Çoklu oturum açmayı yapılandırın](./media/trisotechdigitalenterpriseserver-tutorial/user1.png)

3. Seçin **kullanıcı sağlayıcısı**.

    ![Çoklu oturum açmayı yapılandırın](./media/trisotechdigitalenterpriseserver-tutorial/user2.png)

4. İçinde **kullanıcı sağlayıcısı yapılandırmalarını** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/trisotechdigitalenterpriseserver-tutorial/user3.png)

    a. Seçin **güvenli onaylama işlemi biçimlendirme dili 2 (SAML 2)** açılır listeden **kimlik doğrulama yöntemi**.

    b. İçinde **meta veri URL'si** metin kutusu, yapıştırma **uygulama Federasyon meta verileri URL'sini** kopyaladığınız değeri form Azure portalı.

    c. İçinde **uygulama kimliği** metin kutusuna şu biçimi kullanarak URL'yi girin: `https://<companyname>.trisotech.com`.

    d. **Kaydet**'e tıklayın.

    e. Etki alanı adı girin **(boş gösterir herkes) etki alanlarına izin verilir** metin otomatik olarak atar izin etki alanları ile eşleşen kullanıcılar için lisans

    f. **Kaydet**'e tıklayın.

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

Bu bölümde, Trisotech dijital kuruluş sunucusuna erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Trisotech dijital Enterprise Server**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Trisotech dijital Enterprise Server**.

    ![Uygulamalar listesinde Trisotech dijital Enterprise Server bağlantısı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-trisotech-digital-enterprise-server-test-user"></a>Trisotech dijital Enterprise Server test kullanıcısı oluşturma

Bu bölümde, Britta Simon adlı bir kullanıcı Trisotech dijital Kurumsal Server'da oluşturulur. Trisotech dijital Enterprise Server just-ın-time kullanıcı hazırlama, varsayılan olarak etkin olduğu destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Bir kullanıcı Trisotech dijital Kurumsal Server'da zaten mevcut değilse yeni bir kimlik doğrulamasından sonra oluşturulur.

>[!Note]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [Trisotech dijital Enterprise Server Destek ekibine](mailto:support@trisotech.com).

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Trisotech dijital Enterprise Server kutucuğa tıkladığınızda, size otomatik olarak Trisotech dijital kuruluş SSO'yu ayarlama sunucusuna oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

