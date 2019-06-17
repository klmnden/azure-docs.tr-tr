---
title: 'Öğretici: Parola Yöneticisi Keeper & dijital kasa ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Keeper parola Yöneticisi & dijital kasa arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: e1a98f6a-2dae-4734-bdbf-4fba742a61d2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/02/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 42d12c31e098765209d80c7b5f400333fc11e8bd
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67098871"
---
# <a name="tutorial-azure-active-directory-integration-with-keeper-password-manager--digital-vault"></a>Öğretici: Parola Yöneticisi Keeper & dijital kasa ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Keeper parola Yöneticisi & dijital kasası tümleştirmeyi öğrenin.
Parola Yöneticisi Keeper & dijital kasası, Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* Parola Yöneticisi Keeper & dijital kasa erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak Keeper parola Yöneticisi & dijital kasası (çoklu oturum açma) olarak oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Keeper parola Yöneticisi ve dijital kasa ile yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Parola Yöneticisi keeper & dijital kasa çoklu oturum açma abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Parola Yöneticisi keeper & dijital kasa destekler **SP** tarafından başlatılan

* Parola Yöneticisi keeper & dijital kasa destekler **zamanında** kullanıcı sağlama

## <a name="adding-keeper-password-manager--digital-vault-from-the-gallery"></a>Parola Yöneticisi Keeper & dijital kasa galeri ekleme

Azure AD'de parola Yöneticisi Keeper & dijital kasası tümleştirmesini yapılandırmak için parola Yöneticisi Keeper & dijital kasa Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Parola Yöneticisi Keeper & dijital kasa Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Keeper parola Yöneticisi & dijital kasası**seçin **Keeper parola Yöneticisi & dijital kasası** sonucu panelinden ardından **Ekle** düğmesi uygulama ekleyin.

     ![Parola Yöneticisi keeper & sonuçları listesinde dijital kasası](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Keeper parola Yöneticisi ile test etme ve dijital kasa adlı bir test kullanıcı tabanlı **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ile ilgili kullanıcı Keeper parola Yöneticisi ve dijital kasa arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Keeper parola Yöneticisi ve dijital kasa ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **Parola Yöneticisi Keeper & dijital kasa çoklu oturum açmayı Yapılandır** - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Parola Yöneticisi Keeper & dijital kasa test kullanıcısı oluşturma](#create-keeper-password-manager--digital-vault-test-user)**  - Britta simon'un Keeper parola Yöneticisi & kullanıcı Azure AD gösterimini bağlı dijital kasa içinde bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma Keeper parola Yöneticisi ve dijital kasa ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Keeper parola Yöneticisi & dijital kasası** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Keeper parola Yöneticisi & dijital kasa etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier-reply.png)

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://{SSO CONNECT SERVER}/sso-connect/saml/login`

    b. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://{SSO CONNECT SERVER}/sso-connect`

    c. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://{SSO CONNECT SERVER}/sso-connect/saml/sso`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si, tanımlayıcı ve yanıt URL'si ile güncelleştirin. İlgili kişi [Keeper parola Yöneticisi & dijital kasası istemcisinin Destek ekibine](https://keepersecurity.com/contact.html) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Fedderation meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. Üzerinde **Keeper parola Yöneticisi & dijital kasası kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-keeper-password-manager--digital-vault-single-sign-on"></a>Parola Yöneticisi Keeper & dijital kasa çoklu oturum açmayı Yapılandır

Çoklu oturum açmayı yapılandırma **Keeper parola Yöneticisi ve dijital kasa yapılandırma** , verilen yönergeleri izleyin, yan [Keeper destek Kılavuzu](https://keepersecurity.com/assets/pdf/KeeperSSOConnect_v11.pdf).

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

Bu bölümde, Azure çoklu oturum açma için parola Yöneticisi Keeper & dijital kasa erişim vererek kullanmak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Keeper parola Yöneticisi & dijital kasası**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Keeper parola Yöneticisi & dijital kasası**.

    ![Uygulamalar listesinde Keeper parola Yöneticisi & dijital kasa bağlantısı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-keeper-password-manager--digital-vault-test-user"></a>Parola Yöneticisi Keeper & dijital kasa test kullanıcısı oluşturma

Azure AD kullanıcılarının Keeper parola Yöneticisi için dijital kasa & oturum açmayı etkinleştirmek için bunlar Keeper parola Yöneticisi & dijital kasa sağlanması gerekir. Uygulama, zaman kullanıcı sağlamayı ve kimlik doğrulaması kullanıcılar uygulamaya otomatik olarak oluşturulacak sonra sadece destekler. Sizinle iletişim [Keeper Destek](https://keepersecurity.com/contact.html), kullanıcıların el ile ayarlamak istiyorsanız.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Keeper parola Yöneticisi & dijital kasası kutucuğa tıkladığınızda, otomatik olarak Keeper parola Yöneticisi & dijital SSO'yu ayarlama kasa oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

