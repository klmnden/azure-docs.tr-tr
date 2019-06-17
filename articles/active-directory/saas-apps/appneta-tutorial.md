---
title: 'Öğretici: AppNeta Performans İzleyicisi ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve AppNeta Performans İzleyicisi'ni arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 643a45fb-d6fc-4b32-b721-68899f8c7d44
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/17/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8b1e2baa2cdaee511ecff614406a0e4c25d553c8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67106844"
---
# <a name="tutorial-azure-active-directory-integration-with-appneta-performance-monitor"></a>Öğretici: Azure Active Directory tümleştirmesiyle AppNeta Performans İzleyicisi

Bu öğreticide, Azure Active Directory (Azure AD) ile AppNeta Performans İzleyicisi'ni tümleştirme konusunda bilgi edinin.
AppNeta Performans İzleyicisi'ni Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* Performans İzleyicisi'ni AppNeta erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak AppNeta Performans İzleyicisi (çoklu oturum açma) için oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile AppNeta Performans İzleyicisi'ni yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Abonelik AppNeta Performans İzleyicisi'ni çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* AppNeta Performans İzleyicisi'ni destekleyen **SP** tarafından başlatılan
* AppNeta Performans İzleyicisi'ni destekleyen **zamanında** kullanıcı sağlama

## <a name="adding-appneta-performance-monitor-from-the-gallery"></a>Galeriden AppNeta Performans İzleyicisi ekleme

Azure AD'de AppNeta Performans İzleyicisi'nin tümleştirmesini yapılandırmak için AppNeta Performans İzleyicisi'ni Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden AppNeta Performans İzleyicisi eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **AppNeta Performans İzleyicisi'ni**seçin **AppNeta Performans İzleyicisi'ni** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde AppNeta Performans İzleyicisi](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma AppNeta Performans İzleyicisi'ni adlı bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ile ilgili kullanıcı AppNeta Performans İzleyicisi'nde arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma AppNeta Performansı İzleyicisi ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[AppNeta Performans İzleyicisi çoklu oturum açmayı yapılandırma](#configure-appneta-performance-monitor-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[AppNeta Performans İzleyicisi'ni test kullanıcısı oluşturma](#create-appneta-performance-monitor-test-user)**  - kullanıcı Azure AD gösterimini bağlı AppNeta Performans İzleyicisi'nde Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile AppNeta Performans İzleyicisi'ni yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **AppNeta Performans İzleyicisi'ni** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![AppNeta Performans İzleyicisi'ni etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<subdomain>.pm.appneta.com`

    b. İçinde **tanımlayıcı (varlık kimliği)** metin kutusunda, değeri yazın: `PingConnect`

    > [!NOTE]
    > Oturum açma URL değeri, gerçek değil. Bu değer, gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [AppNeta Performans İzleyicisi'ni istemci Destek ekibine](mailto:support@appneta.com) bu değeri alınamıyor. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. AppNeta Performans İzleyicisi'ni uygulama belirli bir biçimde SAML onaylamalarını bekler. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz **kullanıcı öznitelikleri** uygulama tümleştirme sayfasında bölümü. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için düğmeyi **kullanıcı öznitelikleri** iletişim.

    ![image](common/edit-attribute.png)

6. İçinde **kullanıcı taleplerini** bölümünde **kullanıcı öznitelikleri** iletişim kutusunda kullanarak talep Düzenle **düzenleme simgesi** veya talep kullanarak **Ekle yeni talep**SAML belirteci özniteliği yukarıdaki görüntüde gösterildiği gibi yapılandırın ve aşağıdaki adımları gerçekleştirin: 

    | Ad | Kaynak özniteliği|
    | --------| ----------------|
    | FirstName| User.givenName|
    | Soyadı| User.surname|
    | email| User.userPrincipalName|
    | name| User.userPrincipalName|
    | groups  | User.assignedroles |
    | Telefon| User.telephoneNumber |
    | title| user.jobtitle|
    | | |

    > [!NOTE]
    > **grupları** seçtiğiniz güvenlik grubunda olan eşlenmiş Appneta başvurduğu bir **rol** Azure AD'de. Lütfen [bu](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-app-role-management) Azure AD'de özel roller oluşturmanızı da nasıl yapıldığını açıklayan bir belge.

    a. Tıklayın **Ekle yeni talep** açmak için **yönetmek, kullanıcı talepleri** iletişim.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.

    c. Bırakın **Namespace** boş.

    d. Kaynağı olarak **özniteliği**.

    e. Gelen **kaynak özniteliği** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    f. Tıklayın **Tamam**

    g. **Kaydet**’e tıklayın.

7. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

8. Üzerinde **AppNeta Performans İzleyicisi'ni ayarlayın** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-appneta-performance-monitor-single-sign-on"></a>AppNeta Performans İzleyicisi çoklu oturum açmayı yapılandırın

Çoklu oturum açmayı yapılandırma **AppNeta Performans İzleyicisi'ni** tarafı, indirilen göndermek için ihtiyacınız **Federasyon meta verileri XML** ve uygun Azure portalına kopyalanan URL'lerden [AppNeta Performans İzleyicisi Destek ekibine](mailto:support@appneta.com). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

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

Bu bölümde, AppNeta Performans İzleyicisi için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **AppNeta Performans İzleyicisi'ni**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **AppNeta Performans İzleyicisi'ni**.

    ![Uygulamalar listesinde AppNeta Performans İzleyicisi bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-appneta-performance-monitor-test-user"></a>AppNeta Performans İzleyicisi'ni test kullanıcısı oluşturma

Bu bölümde, Britta Simon adlı bir kullanıcı AppNeta Performans İzleyicisi'nde oluşturulur. AppNeta Performans İzleyicisi'ni just-ın-time kullanıcı hazırlama, varsayılan olarak etkin olduğu destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Bir kullanıcı AppNeta Performans İzleyicisi'nde zaten mevcut değilse yeni bir kimlik doğrulamasından sonra oluşturulur.

> [!Note]
> Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [AppNeta Performans İzleyicisi'ni Destek ekibine](mailto:support@appneta.com).

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli AppNeta Performans İzleyicisi'ni döşemede tıkladığınızda, otomatik olarak AppNeta Performans İzleyicisi'nin SSO'yu ayarlamak için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
