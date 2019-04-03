---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile ThousandEyes | Microsoft Docs'
description: Azure Active Directory ve ThousandEyes arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 790e3f1e-1591-4dd6-87df-590b7bf8b4ba
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/27/2019
ms.author: jeedes
ms.openlocfilehash: 605ab73a11874c85f20728f5524eb770a7a1b259
ms.sourcegitcommit: 04716e13cc2ab69da57d61819da6cd5508f8c422
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2019
ms.locfileid: "58848013"
---
# <a name="tutorial-azure-active-directory-integration-with-thousandeyes"></a>Öğretici: ThousandEyes ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile ThousandEyes tümleştirme konusunda bilgi edinin.
Azure AD ile ThousandEyes tümleştirme ile aşağıdaki avantajları sağlar:

* ThousandEyes erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak (çoklu oturum açma) ThousandEyes için kendi Azure AD hesapları ile oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile ThousandEyes yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* ThousandEyes tek oturum açma etkin abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* ThousandEyes destekler **SP** tarafından başlatılan

* ThousandEyes destekler [ **otomatik** kullanıcı sağlama](https://docs.microsoft.com/azure/active-directory/saas-apps/thousandeyes-provisioning-tutorial)

## <a name="adding-thousandeyes-from-the-gallery"></a>Galeriden ThousandEyes ekleme

Azure AD'de ThousandEyes tümleştirmesini yapılandırmak için ThousandEyes Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden ThousandEyes eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **ThousandEyes**seçin **ThousandEyes** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde ThousandEyes](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma ThousandEyes adlı bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının ThousandEyes ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma ThousandEyes ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[ThousandEyes çoklu oturum açmayı yapılandırma](#configure-thousandeyes-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[ThousandEyes test kullanıcısı oluşturma](#create-thousandeyes-test-user)**  - kullanıcı Azure AD gösterimini bağlı ThousandEyes Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile ThousandEyes yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **ThousandEyes** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![ThousandEyes etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-signonurl.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL yazın:  `https://app.thousandeyes.com/login/sso`

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. Üzerinde **ThousandEyes kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-thousandeyes-single-sign-on"></a>ThousandEyes tek oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde oturum açın, **ThousandEyes** yönetici olarak şirketin site.

2. Üstteki menüden **ayarları**.

    ![Ayarları](./media/thousandeyes-tutorial/ic790066.png "ayarları")

3. Tıklayın **hesabı**

    ![Hesap](./media/thousandeyes-tutorial/ic790067.png "hesabı")

4. Tıklayın **güvenlik ve kimlik doğrulaması** sekmesi.

    ![Güvenlik ve kimlik doğrulaması](./media/thousandeyes-tutorial/ic790068.png "güvenlik ve kimlik doğrulaması")

5. İçinde **Kurulum çoklu oturum açma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açma Kurulumu](./media/thousandeyes-tutorial/ic790069.png "çoklu oturum açma Kurulumu")

    a. Seçin **çoklu oturum açmayı etkinleştirme**.

    b. İçinde **oturum açma sayfası URL'si** metin kutusu, yapıştırma **oturum açma URL'si**, hangi Azure Portalı'ndan kopyaladığınız.

    c. İçinde **oturum kapatma sayfasını URL'si** metin kutusu, yapıştırma **oturum kapatma URL'si**, hangi Azure Portalı'ndan kopyaladığınız.

    d. **Kimlik sağlayıcısını veren** metin kutusu, yapıştırma **Azure AD tanımlayıcısı**, hangi Azure Portalı'ndan kopyaladığınız.

    e. İçinde **doğrulama sertifikası**, tıklayın **dosya**ve sonra Azure portalından indirilen sertifikayı karşıya yükleyin.

    f. **Kaydet**’e tıklayın.

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

Bu bölümde, Azure çoklu oturum açma kullanmak için ThousandEyes erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **ThousandEyes**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **ThousandEyes**.

    ![Uygulamalar listesinde ThousandEyes bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-thousandeyes-test-user"></a>ThousandEyes test kullanıcısı oluşturma

Bu bölümün amacı ThousandEyes Britta Simon adlı bir kullanıcı oluşturmaktır. ThousandEyes otomatik kullanıcı hazırlama, varsayılan olarak etkin olan destekler. Daha fazla ayrıntı bulabilirsiniz [burada](thousandeyes-provisioning-tutorial.md) otomatik kullanıcı sağlamayı yapılandırma.

**Kullanıcı el ile oluşturmanız gerekiyorsa, aşağıdaki adımları uygulayın:**

1. ThousandEyes şirketinizin sitesi için bir yönetici olarak oturum açın.

2. Tıklayın **ayarları**.

    ![Ayarları](./media/thousandeyes-tutorial/IC790066.png "ayarları")

3. Tıklayın **hesabı**.

    ![Hesap](./media/thousandeyes-tutorial/IC790067.png "hesabı")

4. Tıklayın **hesapları k & ullanıcıların** sekmesi.

    ![K & ullanıcıların hesapları](./media/thousandeyes-tutorial/IC790073.png "k & ullanıcıların hesapları")

5. İçinde **Add Users & hesapları** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı hesaplarını eklemek](./media/thousandeyes-tutorial/IC790074.png "kullanıcı hesaplarını ekleyin")

    a. İçinde **adı** metin kutusuna kullanıcı adını yazın ister **Britta Simon**.

    b. İçinde **e-posta** metin kutusuna kullanıcı e-posta türünü ister brittasimon@contoso.com.

    b. Tıklayın **yeni kullanıcı hesabına eklemek**.

    > [!NOTE]
    > Azure Active Directory hesap sahibinin onaylayın ve hesap etkinleştirme bağlantısı içeren bir e-posta alırsınız.

> [!NOTE]
> Herhangi diğer ThousandEyes kullanıcı hesabı oluşturma araçları kullanabilir veya API'leri tarafından ThousandEyes sağlamak için Azure Active Directory kullanıcı hesaplarını sağlanan.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli ThousandEyes kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama ThousandEyes için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [ SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Kullanıcı sağlamayı yapılandırma](https://docs.microsoft.com/azure/active-directory/saas-apps/thousandeyes-provisioning-tutorial)
