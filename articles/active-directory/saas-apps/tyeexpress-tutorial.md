---
title: 'Öğretici: T & E Express ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve T & E Express arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: B42374E5-2559-4309-8EF2-820BEE7EBB0C
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/07/2019
ms.author: jeedes
ms.openlocfilehash: d3459f7bcfc0e2e61cb55b38a05b7b6a21ea3e9e
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59283520"
---
# <a name="tutorial-azure-active-directory-integration-with-te-express"></a>Öğretici: T & E Express ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile T & E Express tümleştirme konusunda bilgi edinin.
T & E Express, Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* T & E Express erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak T & E Express (çoklu oturum açma) için kendi Azure AD hesapları ile oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

T & E Express ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Oturum açma etkin aboneliği tek bir T & E Express

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* T & E Express destekler **IDP** tarafından başlatılan

## <a name="adding-te-express-from-the-gallery"></a>T & E Express galeri ekleme

Azure AD'de T & E Express tümleştirmesini yapılandırmak için T & E Express Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**T & E Express Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **T & E Express**seçin **T & E Express** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![T & E Express sonuç listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma T ile test etme & E Express adlı bir test kullanıcı tabanlı **Britta Simon**.
Tek iş için oturum açma, bir Azure AD kullanıcısı ile kurulacak T & E Express gereksinimlerini ilgili kullanıcı arasında bir bağlantı ilişki için.

Yapılandırma ve Azure AD çoklu oturum açma T & E Express ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[T & E Express çoklu oturum açmayı Yapılandır](#configure-te-express-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[T & E Express test kullanıcısı oluşturma](#create-te-express-test-user)**  - T & E kullanıcı Azure AD gösterimini bağlı Express Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma T & E Express ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **T & E Express** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![T & E Express etki alanı ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. İçinde **tanımlayıcı** metin kutusunda, URL şu biçimi kullanarak olarak değeri yazın: `https://<domain>.tyeexpress.com`

    b. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<domain>.tyeexpress.com/authorize/samlConsume.aspx`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı ve yanıt URL'si ile güncelleştirin. Burada benzersiz dize değeri tanımlayıcıda kullanmanızı öneririz. İlgili kişi [T & E Express istemci Destek ekibine](http://www.tyeexpress.com/contacto.aspx) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. Üzerinde **T & E Express ayarlamak** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-te-express-single-sign-on"></a>T & E Express çoklu oturum açmayı Yapılandır

1. Çoklu oturum açmayı yapılandırma **T & E Express** tarafı, oturum açma T & E express SAML çoklu oturum açma olmadan uygulama yönetici kimlik bilgilerini kullanarak.

1. Altında **yönetici** sekmesinde, üzerinde **SAML etki alanı** SAML ayarlar sayfasını açın.

    ![Çoklu oturum açmayı yapılandırın](./media/tyeexpress-tutorial/tye-SAML.png)

1. Seçin **Activar(Activate)** seçeneğini **Hayır** için **SI(Yes)**. İçinde **kimlik sağlayıcısı meta verileri** metin meta veriler, Azure portalından indirdiğiniz XML yapıştırın.

    ![Çoklu oturum açmayı yapılandırın](./media/tyeexpress-tutorial/tyeAdmin.png)

1. Tıklayarak **Guardar(Save)** düğmesini kullanarak ayarları kaydedin.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü **brittasimon@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için T & E Express erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **T & E Express**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **T & E Express**.

    ![Uygulamalar listesinde T & E Express bağlantısı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-te-express-test-user"></a>T & E Express test kullanıcısı oluşturma

T & E Express açarken Azure AD kullanıcılarının etkinleştirmek için bunlar T & E Express sağlanması gerekir. T & E Express olması durumunda, sağlama el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesapları sağlamak için aşağıdaki adımları gerçekleştirin:**

1. T & E Express şirketinizin sitesi için bir yönetici olarak oturum açın.

1. Yönetici etiketi altında kullanıcıları kullanıcılar ana sayfasını açmak için tıklayın.

    ![Çalışan Ekle](./media/tyeexpress-tutorial/tye-adminusers.png)

1. Giriş sayfasında tıklayarak **+** kullanıcıları eklemek için.

    ![Çalışan Ekle](./media/tyeexpress-tutorial/tye-usershome.png)

1. Ayrıntılarını kaydetmek için Kaydet düğmesine tıklayın ve form içinde sorulan zorunlu olan tüm ayrıntıları girin.

    ![Çalışan Ekle](./media/tyeexpress-tutorial/tye-usersadd.png)

    ![Çalışan Ekle](./media/tyeexpress-tutorial/tye-userssave.png)

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli T & E Express kutucuğa tıkladığınızda, size otomatik olarak T & E SSO'yu ayarlama Express oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

