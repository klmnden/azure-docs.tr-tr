---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile devralarak LMS | Microsoft Docs'
description: Azure Active Directory ve LMS devralarak arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: ba9f1b3d-a4a0-4ff7-b0e7-428e0ed92142
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/02/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: b46135366c76abf8da5387ff0698b4dc7634d79c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60285757"
---
# <a name="tutorial-azure-active-directory-integration-with-absorb-lms"></a>Öğretici: Devralarak LMS ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile devralarak LMS tümleştirme konusunda bilgi edinin.
Azure AD ile devralarak LMS tümleştirme ile aşağıdaki avantajları sağlar:

* Devralarak LMS erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak (çoklu oturum açma) devralarak LMS için oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile devralarak LMS yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa alabileceğiniz bir [ücretsiz hesap](https://azure.microsoft.com/free/)
* LMS tek oturum açma etkin abonelik Al

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* LMS destekler devralarak **IDP** tarafından başlatılan

## <a name="adding-absorb-lms-from-the-gallery"></a>Galeriden devralarak LMS ekleme

Azure AD'de devralarak LMS tümleştirmesini yapılandırmak için devralarak LMS Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden devralarak LMS eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **devralarak LMS**seçin **devralarak LMS** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuçlar listesinde LMS Al](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma adlı bir test kullanıcı tabanlı LMS devralarak test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ve LMS devralarak ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma devralarak LMS ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Devralarak LMS çoklu oturum açmayı yapılandırma](#configure-absorb-lms-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Devralarak LMS test kullanıcısı oluşturma](#create-absorb-lms-test-user)**  - kullanıcı Azure AD gösterimini bağlı devralarak LMS Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma devralarak LMS ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **devralarak LMS** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için düğmeyi **temel SAML yapılandırma** iletişim.

    ![LMS etki alanı ve URL'ler tek oturum açma bilgilerini al](common/idp-intiated.png)

    Kullanıyorsanız **devralarak 5 - kullanıcı Arabirimi** şu yapılandırmayı kullanın:

    a. İçinde **tanımlayıcı** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://company.myabsorb.com/account/saml`

    b. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://company.myabsorb.com/account/saml`

    Kullanıyorsanız **devralarak 5 - yeni Learner deneyimi** şu yapılandırmayı kullanın:

    a. İçinde **tanımlayıcı** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://company.myabsorb.com/api/rest/v2/authentication/saml`

    b. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://company.myabsorb.com/api/rest/v2/authentication/saml`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı ve yanıt URL'si ile güncelleştirin. İlgili kişi [devralarak LMS istemci Destek ekibine](https://support.absorblms.com/hc/) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. Varsayılan öznitelikler listesinde aşağıdaki ekran görüntüsünde gösterilmektedir oysa **NameIdentifier** ile eşlenmiş **user.userprincipalname**.

    ![image](common/edit-attribute.png)

6. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

7. Üzerinde **devralarak LMS kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-absorb-lms-single-sign-on"></a>Yapılandırma LMS çoklu oturum açma Al

1. Yeni bir web tarayıcısı penceresinde devralarak LMS şirketinizin sitesi için bir yönetici olarak oturum açın.

2. Seçin **hesabı** sağ üst köşedeki düğmesi.

    ![Hesap Ekle düğmesine](./media/absorblms-tutorial/1.png)

3. Hesap bölmesinde seçin **Portal ayarları**.

    ![Portal ayarları bağlantısı](./media/absorblms-tutorial/2.png)

4. Seçin **SSO ayarlarını Yönet** sekmesi.

    ![Kullanıcılar sekmesine](./media/absorblms-tutorial/managesso.png)

5. Üzerinde **yönetme çoklu oturum açma ayarları** sayfasında, aşağıdakileri yapın:

    ![Çoklu oturum açma Yapılandırması sayfası](./media/absorblms-tutorial/settings.png)

    a. İçinde **adı** metin kutusuna, Azure AD Market SSO gibi bir ad girin.

    b. Seçin **SAML** olarak bir **yöntemi**.

    c. Azure portalından indirdiğiniz sertifika Not Defteri'nde açın. Kaldırma **---BEGIN CERTIFICATE---** ve **---END CERTIFICATE---** etiketler. Ardından **anahtar** kutusunda, kalan içeriği yapıştırın.

    d. İçinde **modu** kutusunda **kimlik sağlayıcısı tarafından başlatılan**.

    e. İçinde **ID özelliği** kutusunda, Azure AD'de kullanıcı tanımlayıcısı yapılandırdığınız öznitelik seçin. Örneğin, varsa *NameIdentifier* Azure AD'de seçin seçili **Username**.

    f. Seçin **Sha256** olarak bir **imza türü**.

    g. İçinde **oturum açma URL'si** kutusu, yapıştırma **kullanıcı erişim URL'SİNDEN** uygulamanın gelen **özellikleri** Azure Portalı'nın sayfasında.

    h. İçinde **oturum kapatma URL'si**, yapıştırın **oturum kapatma URL'si** kopyalama değeri **yapılandırma oturum açma** Azure portal'ın penceresi.

    i. İki durumlu **otomatik olarak yeniden yönlendirme** için **üzerinde**.

6. Seçin **kaydedin.**

    ![Yalnızca izin SSO Oturum Aç/Kapat](./media/absorblms-tutorial/save.png)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü `brittasimon\@yourcompanydomain.extension`  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, LMS etkisini azaltmak için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **devralarak LMS**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde yazın ve **devralarak LMS**.

    ![Uygulamalar listesinde devralarak LMS bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-absorb-lms-test-user"></a>Devralarak LMS test kullanıcısı oluşturma

Azure AD kullanıcılarının LMS etkisini azaltmak için oturum açmak bunlar devralarak LMS ayarlanması gerekir. Devralarak LMS söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Devralarak LMS şirketinizin sitesi için bir yönetici olarak oturum açın.

2. İçinde **kullanıcılar** bölmesinde **kullanıcılar**.

    ![Kullanıcıları bağlantısı](./media/absorblms-tutorial/absorblms_userssub.png)

3. Seçin **kullanıcı** sekmesi.

    ![Yeni Ekle aşağı açılan listesi](./media/absorblms-tutorial/absorblms_createuser.png)

4. Üzerinde **Kullanıcı Ekle** sayfasında, aşağıdakileri yapın:

    ![Kullanıcı Ekle sayfası](./media/absorblms-tutorial/user.png)

    a. İçinde **ad** ad gibi yazın **Britta**.

    b. İçinde **Soyadı** Soyadı gibi yazın **Simon**.

    c. İçinde **kullanıcıadı** gibi tam bir ad yazın **Britta Simon**.

    d. İçinde **parola** kutusu, kullanıcı parolayı girin.

    e. İçinde **parolayı onayla** kutusuna, parolayı yeniden yazın.

    f. Ayarlama **etkindir** geç **etkin**.

5. Seçin **kaydedin.**

    ![Yalnızca izin SSO Oturum Aç/Kapat](./media/absorblms-tutorial/save.png)

    > [!NOTE]
    > Varsayılan olarak, kullanıcı sağlama SSO etkin değil. Bu özelliği etkinleştirmek müşteri isterse, en fazla belirtildiği gibi ayarlamak sahip oldukları [bu](https://support.absorblms.com/hc/en-us/articles/360014083294-Incoming-SAML-2-0-SSO-Account-Provisioning) belgeleri. Ayrıca kullanıcı Provisioing yalnızca kullanılabilir olmadığını unutmayın **devralarak 5 - yeni Learner deneyimi** ile ACS URL -`https://company.myabsorb.com/api/rest/v2/authentication/saml`

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli devralarak LMS kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama LMS etkisini azaltmak için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
