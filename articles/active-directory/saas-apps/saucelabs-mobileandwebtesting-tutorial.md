---
title: 'Öğretici: Görüntünüzün Labs - mobil ve Web testi ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Tariflerinizden Labs - mobil ve Web testi arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 3142d947-70e5-4345-8a30-b92d8715fac9
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/22/2019
ms.author: jeedes
ms.openlocfilehash: 41b35324ccca8cf40edbc53ed25a2d8615a9294e
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65813627"
---
# <a name="tutorial-azure-active-directory-integration-with-sauce-labs---mobile-and-web-testing"></a>Öğretici: Görüntünüzün Labs - mobil ve Web testi ile Azure Active Directory Tümleştirme

Bu öğreticide, Tariflerinizden Laboratuvarlarla - mobil ve Web testi Azure Active Directory (Azure AD) ile tümleştirme konusunda bilgi edinin.
Görüntünüzün Labs - mobil ve Web testi Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* Görüntünüzün Labs - mobil ve Web testi erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak Tariflerinizden Labs - mobil ve Web testi (çoklu oturum açma) ile kendi Azure AD hesapları için oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Görüntünüzün Laboratuvarlarla - mobil ve Web testi, Azure AD tümleştirmesi yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa alabileceğiniz bir [ücretsiz hesap](https://azure.microsoft.com/free/)
* Tek oturum açma etkin abonelik tariflerinizden Labs - mobil ve Web testi

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Labs - Mobile tariflerinizden ve Web testi destekler **IDP** tarafından başlatılan
* Labs - Mobile tariflerinizden ve Web testi destekler **zamanında** kullanıcı sağlama

## <a name="adding-sauce-labs---mobile-and-web-testing-from-the-gallery"></a>Görüntünüzün Labs - mobil ve Web testi galerisinden ekleme

Görüntünüzün Labs - mobil ve Web testi Azure AD'ye tümleştirmesini yapılandırmak için Sauce Labs - mobil ve uygulamalarınızın listesini yönetilen SaaS uygulamaları için Web testi galerisinden eklemeniz gerekir.

**Görüntünüzün Labs - mobil ve Web testi galerisinden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Tariflerinizden Labs - mobil ve Web testi**seçin **Tariflerinizden Labs - mobil ve Web testi** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Görüntünüzün Labs - mobil ve Web testi sonuç listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Tariflerinizden Labs ile test etme - mobil ve Web testi adlı bir test kullanıcı tabanlı **Britta Simon**.
İş, bir Azure AD kullanıcısı ve ilgili kullanıcı Tariflerinizden Labs - arasında bir bağlantı ilişki için çoklu oturum açma için mobil ve Web testi gereken kurulmalıdır.

Yapılandırma ve Azure AD çoklu oturum açma Tariflerinizden laboratuvarlarla - test etme için mobil ve Web testi, aşağıdaki yapı taşlarını izlemeniz gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Görüntünüzün Labs - mobil ve Web testi çoklu oturum açmayı yapılandırma](#configure-sauce-labs---mobile-and-web-testing-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Görüntünüzün Laboratuvarları oluşturma - kullanıcı mobil ve Web testi test](#create-sauce-labs---mobile-and-web-testing-test-user)**  : Sauce Labs - mobil ve Web kullanıcı Azure AD gösterimini bağlantılı test Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Görüntünüzün Laboratuvarlarla - mobil ve Web testi, Azure AD çoklu oturum açmayı yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Tariflerinizden Labs - mobil ve Web testi** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde kullanıcısının clonedatabase'i uygulama zaten Azure ile önceden tümleşik olarak herhangi bir adımı gerçekleştirmek.

    ![Görüntünüzün Labs - mobil ve Web testi etki alanı ve oturum açma URL tek bilgi](common/preintegrated.png)

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. Üzerinde **Tariflerinizden Labs - mobil ve Web testi ayarlama** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-sauce-labs---mobile-and-web-testing-single-sign-on"></a>Mobil Tariflerinizden Labs - yapılandırma ve test çoklu oturum açma Web

1. Farklı bir web tarayıcı penceresinde Tariflerinizden Labs - mobil ve Web şirket site yönetici olarak sınama için oturum açın.

2. Tıklayarak **kullanıcı simgesi** seçip **Ekip Yönetimi** sekmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/saucelabs-mobileandwebtesting-tutorial/configure1.png)

3. Girin, **etki alanı adı** metin kutusuna.

    ![Çoklu oturum açmayı yapılandırın](./media/saucelabs-mobileandwebtesting-tutorial/configure2.png)

4. Tıklayın **yapılandırma** sekmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/saucelabs-mobileandwebtesting-tutorial/configure3.png)

5. İçinde **çoklu oturum yapılandırma açma aktar** bölümünde, aşağıdaki adımları gerçekleştirin.

    ![Çoklu oturum açmayı yapılandırın](./media/saucelabs-mobileandwebtesting-tutorial/configure4.png)

    a. Tıklayın **Gözat** ve Azure AD'den indirilen meta veri dosyasını karşıya yükleyin.

    b. Seçin **izin tam zamanında sağlama** onay kutusu.

    c. **Kaydet**’e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü `brittasimon@yourcompanydomain.extension`  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma Tariflerinizden Labs kullanarak - mobil ve Web testi erişim vererek kullanmak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Tariflerinizden Labs - mobil ve Web testi**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Tariflerinizden Labs - mobil ve Web testi**.

    ![Uygulamalar listesinde Tariflerinizden Labs - mobil ve Web testi bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-sauce-labs---mobile-and-web-testing-test-user"></a>Görüntünüzün Laboratuvarları oluşturma - kullanıcı, mobil ve Web testi test

Bu bölümde, Britta Simon adlı bir kullanıcı Tariflerinizden Labs - mobil ve Web testi oluşturulur. Labs - Mobile tariflerinizden ve Web testi varsayılan olarak etkin olduğu just-ın-time kullanıcı hazırlama, destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Görüntünüzün Labs - mobil ve Web testi, bir kullanıcı zaten mevcut değilse yeni bir kimlik doğrulamasından sonra oluşturulur.

> [!Note]
> Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [Tariflerinizden Labs - mobil ve Web testi Destek ekibine](mailto:support@saucelabs.com).

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Tariflerinizden Labs tıklayın - mobil ve Web testi, erişim Paneli'nde döşeme sonra otomatik olarak Tariflerinizden Labs - mobil ve SSO'yu ayarlama Web testi için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

