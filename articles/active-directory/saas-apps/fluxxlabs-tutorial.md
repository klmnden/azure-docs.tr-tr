---
title: 'Öğretici: Azure Active Directory Tümleştirmesi Fluxx laboratuvarlarla | Microsoft Docs'
description: Azure Active Directory ve Fluxx Labs arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: d8fac770-bb57-4e1f-b50b-9ffeae239d07
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/01/2019
ms.author: jeedes
ms.openlocfilehash: e1951a65c48c32f2ce4af722400d03c20dfa684b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60279117"
---
# <a name="tutorial-azure-active-directory-integration-with-fluxx-labs"></a>Öğretici: Fluxx laboratuvarlar ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Fluxx Laboratuvarlarla tümleştirme konusunda bilgi edinin.
Fluxx Labs, Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* Fluxx Labs erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak Fluxx Labs (çoklu oturum açma) kullanarak oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Fluxx Labs ile yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa alabileceğiniz bir [ücretsiz hesap](https://azure.microsoft.com/free/)
* Tek oturum açma etkin abonelik Fluxx Laboratuvarları

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Fluxx Labs destekler **IDP** tarafından başlatılan

## <a name="adding-fluxx-labs-from-the-gallery"></a>Galeriden Fluxx Labs ekleme

Azure AD'de Fluxx Labs tümleştirmesini yapılandırmak için Fluxx Labs Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Fluxx Labs eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Fluxx Labs**seçin **Fluxx Labs** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde Fluxx Laboratuvarları](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Fluxx adlı bir test kullanıcı tabanlı Labs ile test etme **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ve ilgili kullanıcı Fluxx Labs arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Fluxx Labs ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Fluxx Labs çoklu oturum açmayı yapılandırma](#configure-fluxx-labs-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Fluxx Labs test kullanıcısı oluşturma](#create-fluxx-labs-test-user)**  - kullanıcı Azure AD gösterimini bağlı Fluxx Labs Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma Fluxx Labs ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Fluxx Labs** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Fluxx Labs etki alanı ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. İçinde **tanımlayıcı** metin kutusuna şu biçimi kullanarak bir URL yazın:

    | Ortam | URL deseni|
    |-------------|------------|
    | Üretim | `https://<subdomain>.fluxx.io` |
    | Üretim öncesi | `https://<subdomain>.preprod.fluxxlabs.com`|

    b. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:

    | Ortam | URL deseni|
    |-------------|------------|
    | Üretim | `https://<subdomain>.fluxx.io/auth/saml/callback` |
    | Üretim öncesi | `https://<subdomain>.preprod.fluxxlabs.com/auth/saml/callback`|

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı ve yanıt URL'si ile güncelleştirin. İlgili kişi [Fluxx Labs istemcisini Destek ekibine](mailto:travis@fluxxlabs.com) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. Üzerinde **Fluxx Labs kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-fluxx-labs-single-sign-on"></a>Fluxx Labs çoklu oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde Fluxx Labs şirketinizin sitesi için yönetici olarak oturum açın.

2. Seçin **yönetici** aşağıda **ayarları** bölümü.

    ![Fluxx Laboratuvar yapılandırma](./media/fluxxlabs-tutorial/config1.png)

3. Yönetim panelinde seçin **eklentileri** > **tümleştirmeler** seçip **SAML SSO-(Disabled)**

    ![Fluxx Laboratuvar yapılandırma](./media/fluxxlabs-tutorial/config2.png)

4. Öznitelik bölümünde aşağıdaki adımları gerçekleştirin:

    ![Fluxx Laboratuvar yapılandırma](./media/fluxxlabs-tutorial/config3.png)

    a. Seçin **SAML SSO** onay kutusu.

    b. İçinde **istek yolu** metin kutusuna **/auth/saml**.

    c. İçinde **geri arama yolu** metin kutusuna **/auth/saml/callback**.

    d. İçinde **onaylama tüketici hizmeti Url(Single Sign-On URL)** metin girin **yanıt URL'si** Azure portalında girdiğiniz değer.

    e. İçinde **İzleyici (SP varlık kimliği)** metin girin **tanımlayıcı** Azure portalında girdiğiniz değer.

    f. İçinde **kimlik sağlayıcısı SSO hedef URL'si** metin kutusu, yapıştırma **oturum açma URL'si** Azure portaldan kopyaladığınız değeri.

    g. Base-64 kodlanmış sertifikanızı Not Defteri'nde açın, içeriğini, panoya kopyalayın ve ardından ona yapıştırın **kimlik sağlayıcısı sertifikası** metin.

    h. İçinde **ad tanımlayıcı biçimi** metin değeri girin `urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress`.

    i. **Kaydet**’e tıklayın.

    > [!NOTE]
    > Kaydedilmiş bir içerik alanı bir kez güvenlik için boş görünmez, ancak değeri yapılandırmada kaydedildi.

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

Bu bölümde, Fluxx Labs kullanarak erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Fluxx Labs**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Fluxx Labs**.

    ![Uygulamalar listesinde Fluxx Labs bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-fluxx-labs-test-user"></a>Fluxx Labs test kullanıcısı oluşturma

Fluxx Labs kullanarak oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunlar Fluxx Labs sağlanması gerekir. Fluxx Labs söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Fluxx Labs şirketinizin sitesi için bir yönetici olarak oturum açın.

2. Tıklayarak aşağıda görüntülenen **simgesi**.

    ![Fluxx Laboratuvar yapılandırma](./media/fluxxlabs-tutorial/config6.png)

3. Panoda, tıklayarak açmak için görüntülenen simgesinin altında **yeni kişiler** kart.

    ![Fluxx Laboratuvar yapılandırma](./media/fluxxlabs-tutorial/config4.png)

4. Üzerinde **yeni kişiler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Fluxx Laboratuvar yapılandırma](./media/fluxxlabs-tutorial/config5.png)

    a. Fluxx Laboratuarları e-posta SSO oturumu için benzersiz tanımlayıcı olarak kullanın. Doldurma **SSO UID** alan SSO ile oturum açma olarak kullanarak e-posta adresiyle eşleşen, kullanıcının e-posta adresi.

    b. **Kaydet**’e tıklayın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Fluxx Labs kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama Fluxx laboratuvarlara oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

