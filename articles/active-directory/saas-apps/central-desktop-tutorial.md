---
title: 'Öğretici: Merkezi Masaüstü ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Merkezi Masaüstü arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: b805d485-93db-49b4-807a-18d446c7090e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/12/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2e5ddc8a1190161d9492cd083a50120ca9d5fc5f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60281805"
---
# <a name="tutorial-azure-active-directory-integration-with-central-desktop"></a>Öğretici: Merkezi Masaüstü ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Merkezi Masaüstü tümleştirme konusunda bilgi edinin.
Merkezi Masaüstü Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* Merkezi Masaüstü erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak (çoklu oturum açma) için Merkezi Masaüstü oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Merkezi Masaüstü ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Merkezi Masaüstü çoklu oturum açma abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Merkezi Masaüstü destekler **SP** tarafından başlatılan

## <a name="adding-central-desktop-from-the-gallery"></a>Merkezi Masaüstü galeri ekleme

Azure AD'de Merkezi Masaüstü tümleştirmesini yapılandırmak için Merkezi Masaüstü Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Merkezi Masaüstü Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Merkezi Masaüstü**seçin **Merkezi Masaüstü** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde Merkezi Masaüstü](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma merkezi adlı bir test kullanıcı tabanlı masaüstü test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ile ilgili kullanıcı Merkezi Masaüstü arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Merkezi Masaüstü ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Merkezi Masaüstü çoklu oturum açmayı yapılandırma](#configure-central-desktop-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Merkezi Masaüstü test kullanıcısı oluşturma](#create-central-desktop-test-user)**  - kullanıcı Azure AD gösterimini bağlı olduğu Merkezi Masaüstü Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma Merkezi Masaüstü ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Merkezi Masaüstü** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Oturum açma bilgileri tek bir merkezi Masaüstü etki alanı ve URL'ler](common/sp-identifier-reply.png)

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<companyname>.centraldesktop.com`

    b. İçinde **tanımlayıcı** kutusuna şu biçimi kullanarak bir URL yazın:
    
    | |
    |--|
    | `https://<companyname>.centraldesktop.com/saml2-metadata.php`|
    | `https://<companyname>.imeetcentral.com/saml2-metadata.php`|
    | |

    c. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<companyname>.centraldesktop.com/saml2-assertion.php`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si, tanımlayıcı ve yanıt URL'si ile güncelleştirin. İlgili kişi [Merkezi Masaüstü İstemcisi Destek ekibine](https://imeetcentral.com/contact-us) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (ham)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificateraw.png)

6. Üzerinde **Merkezi Masaüstü bağlantısı kurma** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-central-desktop-single-sign-on"></a>Merkezi Masaüstü çoklu oturum açmayı yapılandırın

1. Oturum açın, **Merkezi Masaüstü** Kiracı.

2. Git **ayarları**. Seçin **Gelişmiş**ve ardından **çoklu oturum açma**.

    ![Kurulum - Gelişmiş](./media/central-desktop-tutorial/ic769563.png "Kurulum - Gelişmiş")

3. Üzerinde **tek oturum açma ayarları** sayfasında, aşağıdaki adımları uygulayın:

    ![Çoklu oturum açma ayarları](./media/central-desktop-tutorial/ic769564.png "tek oturum açma ayarları")

    a. Seçin **etkinleştir SAML v2 çoklu oturum açma**.

    b. İçinde **SSO URL** kutusu, yapıştırma **Azure Ad tanımlayıcısı** Azure portaldan kopyaladığınız değeri.

    c. İçinde **SSO oturum açma URL'si** kutusu, yapıştırma **oturum açma URL'si** Azure portaldan kopyaladığınız değeri.

    d. İçinde **SSO oturum kapatma URL'si** kutusu, yapıştırma **oturum kapatma URL'si** Azure portaldan kopyaladığınız değeri.

4. İçinde **ileti imzası doğrulama yöntemi** bölümünde, aşağıdaki adımları uygulayın:

    ![İleti imzası doğrulama yöntemi](./media/central-desktop-tutorial/ic769565.png "ileti imzası doğrulama yöntemi")
    
    a. **Sertifika**’yı seçin.

    b. İçinde **SSO sertifikası** listesinden **RSH SHA256**.

    c. İndirilen sertifikanızı Not Defteri'nde açın. Ardından, sertifika içeriğini kopyalayın ve yapıştırın **SSO sertifikası** alan.

    d. Seçin **SAMLv2 oturum açma sayfanız bir bağlantı görüntüler**.

    e. Seçin **güncelleştirme**.

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

Bu bölümde, merkezi masaüstüne erişimi vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Merkezi Masaüstü**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Merkezi Masaüstü**.

    ![Uygulamalar listesinde Merkezi Masaüstü Bağlantısı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-central-desktop-test-user"></a>Merkezi Masaüstü test kullanıcısı oluşturma

Merkezi masaüstü uygulamasında Azure AD kullanıcılarının oturum açabilmesi bunlar sağlanmalıdır. Bu bölümde merkezi Desktop uygulamasında Azure AD kullanıcı hesapları oluşturma işlemini açıklar.

> [!NOTE]
> Azure AD kullanıcı hesapları sağlamak için herhangi bir merkezi Masaüstü kullanıcı hesabı oluşturma araçları veya Merkezi Masaüstü tarafından sağlanan API'leri kullanabilirsiniz.

**Merkezi Masaüstü kullanıcı hesapları sağlamak için:**

1. Merkezi Masaüstü kiracınızda oturum açın.

2. Seçin **kişiler** seçip **iç Üye Ekle**.

    ![Kişiler](./media/central-desktop-tutorial/ic781051.png "kişiler")

3. İçinde **yeni üyeler e-posta adresi** sağlayın ve ardından istediğiniz bir Azure AD hesabı yazın **sonraki**.

    ![E-posta adreslerini yeni üyelerin](./media/central-desktop-tutorial/ic781052.png "e-posta adreslerini yeni üye")

4. Seçin **Ekle iç üyeleri**.

    ![İç Üye Ekle](./media/central-desktop-tutorial/ic781053.png "iç Üye Ekle")
  
   > [!NOTE]
   > Eklediğiniz kullanıcı hesaplarını etkinleştirmek için onay bağlantısını içeren bir e-posta alırsınız.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Merkezi Masaüstü kutucuğa tıkladığınızda, otomatik olarak Merkezi Masaüstü SSO'yu ayarlama için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
