---
title: 'Öğretici: Icertis sözleşme yönetim platformu ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Icertis sözleşme yönetim platformu ile Azure Active Directory arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 6627e6dd-f559-4cd4-a509-f6d9a4961b49
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/16/2019
ms.author: jeedes
ms.openlocfilehash: 4d105fd9d0611ad56cd5ec2ecaa36bdcf9deed18
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67100739"
---
# <a name="tutorial-azure-active-directory-integration-with-icertis-contract-management-platform"></a>Öğretici: Icertis sözleşme yönetim platformu ile Azure Active Directory Tümleştirme

Bu öğreticide, Icertis sözleşme yönetim platformu Azure Active Directory (Azure AD) ile tümleştirmeyi öğrenin.
Icertis sözleşme yönetim platformu Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* Icertis sözleşme yönetim platformu erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak Icertis sözleşme yönetim platformuna (çoklu oturum açma) oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Icertis sözleşme yönetim platformu ile yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Icertis sözleşme yönetim platformu çoklu oturum açma abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Icertis sözleşme yönetim platformunu destekleyen **SP** tarafından başlatılan

## <a name="adding-icertis-contract-management-platform-from-the-gallery"></a>Icertis sözleşme yönetim platformu galeri ekleme

Azure AD'de Icertis sözleşme yönetim platformu tümleştirmesini yapılandırmak için Icertis sözleşme yönetim platformu Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Icertis sözleşme yönetim platformu eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Icertis sözleşme yönetim platformu**seçin **Icertis sözleşme yönetim platformu** sonucu panelinden ardından **Ekle** düğme eklemek için uygulama.

     ![Sonuç listesinde Icertis sözleşme yönetim platformu](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Icertis sözleşme yönetim adlı bir test kullanıcısı ve erişim yönetimi ile test etme **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının Icertis sözleşme yönetim platformu ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Icertis sözleşme yönetim platformu ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Icertis sözleşme yönetim platformu çoklu oturum açmayı yapılandırma](#configure-icertis-contract-management-platform-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Icertis sözleşme yönetim platformu test kullanıcısı oluşturma](#create-icertis-contract-management-platform-test-user)**  - kullanıcı Azure AD gösterimini bağlı Icertis sözleşme yönetim platformu Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma Icertis sözleşme yönetim platformu ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Icertis sözleşme yönetim platformu** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Icertis sözleşme yönetim platformu etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<company name>.icertis.com`

    b. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<company name>.icertis.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL ve tanımlayıcıdır ile güncelleştirin. İlgili kişi [Icertis sözleşme yönetim platformu istemci Destek ekibine](https://www.icertis.com/company/contact/) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. Üzerinde **Icertis sözleşme yönetim platformu ayarlamanıza** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın. İçin **oturum açma URL'si**, değeri ile şu biçimi kullanın: `https://login.microsoftonline.com/_my_directory_id_/wsfed`

    > [!Note]
    > _my_directory_id_ Azure AD aboneliğinin Kiracı kimliği.

    ![Yapılandırma URL'leri kopyalayın](media/icertisicm-tutorial/configurls.png)

    a. Azure AD Tanımlayıcısı

    b. Oturum Kapatma URL'si

### <a name="configure-icertis-contract-management-platform-single-sign-on"></a>Icertis sözleşme yönetim platformu çoklu oturum açmayı yapılandırın

Çoklu oturum açmayı yapılandırma **Icertis sözleşme yönetim platformu** tarafı, indirilen göndermek için ihtiyacınız **Federasyon meta verileri XML** ve uygun URL'ler için Azure portalından [ Icertis sözleşme yönetim platformu Destek ekibine](https://www.icertis.com/company/contact/). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü `brittasimon@yourcompanydomain.extension`. Örneğin, BrittaSimon@contoso.com.

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Icertis sözleşme yönetim platformu için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Icertis sözleşme yönetim platformu**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Icertis sözleşme yönetim platformu**.

    ![Uygulamalar listesinde Icertis sözleşme yönetim platformu bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-icertis-contract-management-platform-test-user"></a>Icertis sözleşme yönetim platformu test kullanıcısı oluşturma

Bu bölümde, Britta Simon Icertis sözleşme yönetim platformu adlı bir kullanıcı oluşturun. Çalışmak [Icertis sözleşme yönetim platformu Destek ekibine](https://www.icertis.com/company/contact/) Icertis sözleşme yönetim platformu platform kullanıcıları eklemek için. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Icertis sözleşme yönetim platformu kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama Icertis sözleşme yönetim platformu için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)