---
title: 'Öğretici: Yaptığımız yolu ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory arasında yaptığımız gibi çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 84fc4f36-ecd1-42c6-8a70-cb0f3dc15655
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/03/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3fd0a4e77c36f8f9be220b4e56d76d17487b7017
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67087304"
---
# <a name="tutorial-azure-active-directory-integration-with-way-we-do"></a>Öğretici: Yaptığımız yolu ile Azure Active Directory Tümleştirme

Bu öğreticide, yaptığımız gibi Azure Active Directory (Azure AD) ile tümleştirmeyi öğrenin.
Azure AD ile tümleştirme gibi yaptığımız ile aşağıdaki avantajları sağlar:

* Erişimi yaptığımız gibi Azure AD'de kontrol edebilirsiniz.
* Kullanıcılarınızın şekilde yapmamız için (çoklu oturum açma) ile Azure AD hesaplarına otomatik olarak imzalanmasını etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi gibi biz yaparız ile yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa alabileceğiniz bir [ücretsiz hesap](https://azure.microsoft.com/free/)
* Size çoklu oturum açma yolu abonelik etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Yaptığımız biçimini destekler **SP** tarafından başlatılan

* Yaptığımız biçimini destekler **zamanında** kullanıcı sağlama

## <a name="adding-way-we-do-from-the-gallery"></a>Yaptığımız gibi galeri ekleme

Yaptığımız gibi Azure AD'de tümleştirmesini yapılandırmak için yaptığımız gibi galerideki yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden yaptığımız yolu eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **yaptığımız gibi**seçin **yaptığımız gibi** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuçlar listesinde şekilde desteklemiyoruz](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma yolu adlı bir test kullanıcı tabanlı olan yaptığımız ile test etme **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ile ilgili yaptığımız gibi kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma yolu yaptığımız ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Size çoklu oturum açma biçimini yapılandırmak](#configure-way-we-do-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Biz kullanıcı test yolu oluşturma](#create-way-we-do-test-user)**  - bir karşılığı Britta simon'un şekilde kullanıcı Azure AD gösterimini bağlı bunu sağlamak için.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma yolu yaptığımız ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **yaptığımız gibi** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Yol biz yapmak etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<SUBDOMAIN>.waywedo.com/Authentication/ExternalSignIn`

    b. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<SUBDOMAIN>.waywedo.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL ve tanımlayıcıdır ile güncelleştirin. İlgili kişi [şekilde biz yapmak istemci Destek ekibine](mailto:support@waywedo.com) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (ham)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificateraw.png)

6. Üzerinde **şekilde yaptığımız kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-way-we-do-single-sign-on"></a>Size çoklu oturum açmayı şekilde yapılandırın

1. Farklı bir web tarayıcı penceresinde bir şekilde yapmamız için bir güvenlik yöneticisi olarak oturum açın.

2. Tıklayın **kişi simgesi** yaptığımız gibi herhangi bir sayfasında sağ üst köşesinde, ardından **hesabı** açılır menüdeki.

    ![Yol yaptığımız hesabı](./media/waywedo-tutorial/tutorial_waywedo_account.png) 

3. Tıklayın **menüsü simgesi** anında iletme Gezinti menüsüne ve ardından açmak için **çoklu oturum açma**.

    ![Yol yaptığımız tek](./media/waywedo-tutorial/tutorial_waywedo_single.png)

4. Üzerinde **tek oturum açma Kurulumu** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Yol yaptığımız Kaydet](./media/waywedo-tutorial/tutorial_waywedo_save.png)

    a. Tıklayın **üzerinde çoklu oturum açmayı etkinleştirmek** geç **Evet** çoklu oturum açmayı etkinleştirmek için.

    b. İçinde **tek oturum açma adı** metin adınızı girin.

    c. İçinde **varlık kimliği** metin değerini yapıştırın **Azure AD tanımlayıcısı**, hangi Azure portaldan kopyaladığınız.

    d. İçinde **SAML SSO URL** metin değerini yapıştırın **oturum açma URL'si**, hangi Azure portaldan kopyaladığınız.

    e. Tıklayarak sertifikayı karşıya yüklemek **seçin** düğmesinin yanındaki **sertifika**.

    f. **İsteğe bağlı ayarlar** -
    
    * Parolaları - etkinleştirme bu seçeneği devre dışı bırakıldığında, böylece kullanıcıların yalnızca çoklu oturum açma kullanabilirsiniz normal parola şekilde yapmamız için çalışır.

    * Otomatik sağlamayı - etkinleştirme etkinleştirildiğinde, oturum açma için kullanılan e-posta adresine otomatik olarak yaptığımız gibi kullanıcıların listesine karşılaştırılır. Yaptığımız gibi etkin bir kullanıcı e-posta adresi ile eşleşmiyorsa, oturum açma, eksik bilgileri isteyen kişi için otomatik olarak yeni bir kullanıcı hesabı ekler.

      > [!NOTE]
      > Çoklu oturum açma ile eklenen kullanıcılar genel kullanıcılar olarak eklenir ve sistemde bir rolü atanmaz. Bir yönetici, Git ve bir düzenleyici veya yönetici olarak güvenlik rolü değiştirebilirsiniz ve ayrıca bir veya birden çok kuruluş şeması roller atayabilirsiniz. 

    g. Tıklayın **Kaydet** ayarlarınızı kalıcı hale getirmek için.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü `brittasimon@yourcompanydomain.extension`. Örneğin, BrittaSimon@contoso.com

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma yolu yapmamız için erişim izni verme kullanmak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **yaptığımız gibi**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **yaptığımız gibi**.

    ![Uygulamalar listesinde yaptığımız gibi bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-way-we-do-test-user"></a>Yaptığımız gibi test kullanıcısı oluşturma

Bu bölümde, Britta Simon adlı bir kullanıcı yolu yaptığımız içinde oluşturulur. Yol yaptığımız just-ın-time kullanıcı hazırlama, varsayılan olarak etkin olduğu destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Bir kullanıcı yolu yaptığımız içinde zaten mevcut değilse yeni bir kimlik doğrulamasından sonra oluşturulur.

> [!Note]
> Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [şekilde biz yapmak istemci Destek ekibine](mailto:support@waywedo.com).

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli yaptığımız gibi kutucuğa tıkladığınızda, size otomatik olarak yol biz SSO'yu ayarlama yapmak için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

