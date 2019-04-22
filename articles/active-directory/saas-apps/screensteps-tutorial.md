---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile ScreenSteps | Microsoft Docs'
description: Azure Active Directory ve ScreenSteps arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 4563fe94-a88f-4895-a07f-79df44889cf9
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/07/2019
ms.author: jeedes
ms.openlocfilehash: 570f789d0f399c5ffa7535101136ab65ba58ccd5
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59277094"
---
# <a name="tutorial-azure-active-directory-integration-with-screensteps"></a>Öğretici: ScreenSteps ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile ScreenSteps tümleştirme konusunda bilgi edinin.
Azure AD ile ScreenSteps tümleştirme ile aşağıdaki avantajları sağlar:

* ScreenSteps erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak (çoklu oturum açma) ScreenSteps için kendi Azure AD hesapları ile oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile ScreenSteps yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* ScreenSteps tek oturum açma etkin abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* ScreenSteps destekler **SP** tarafından başlatılan

## <a name="adding-screensteps-from-the-gallery"></a>Galeriden ScreenSteps ekleme

Azure AD'de ScreenSteps tümleştirmesini yapılandırmak için ScreenSteps Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden ScreenSteps eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **ScreenSteps**seçin **ScreenSteps** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde ScreenSteps](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma ScreenSteps adlı bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının ScreenSteps ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma ScreenSteps ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[ScreenSteps çoklu oturum açmayı yapılandırma](#configure-screensteps-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[ScreenSteps test kullanıcısı oluşturma](#create-screensteps-test-user)**  - kullanıcı Azure AD gösterimini bağlı ScreenSteps Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile ScreenSteps yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **ScreenSteps** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![ScreenSteps etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-signonurl.png)

    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:  `https://<tenantname>.ScreenSteps.com`

    > [!NOTE]
    > Bu değer, gerçek değil. Bu değer, gerçek oturum açma, bu öğreticinin ilerleyen bölümlerinde açıklanan URL ile güncelleştirin.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. Üzerinde **ScreenSteps kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-screensteps-single-sign-on"></a>ScreenSteps tek oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde ScreenSteps şirket sitenize yönetici olarak oturum.

1. Tıklayın **hesap ayarları**.

    ![Hesap Yönetimi](./media/screensteps-tutorial/ic778523.png "hesap yönetimi")

1. Tıklayın **çoklu oturum açma**.

    ![Uzak kimlik doğrulaması](./media/screensteps-tutorial/ic778524.png "uzak kimlik doğrulaması")

1. Tıklayın **tek oturum açma uç noktası oluşturma**.

    ![Uzak kimlik doğrulaması](./media/screensteps-tutorial/ic778525.png "uzak kimlik doğrulaması")

1. İçinde **oluşturma tek oturum açma uç noktası** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Bir kimlik doğrulama uç noktası oluşturma](./media/screensteps-tutorial/ic778526.png "bir kimlik doğrulama uç noktası oluşturma")

    a. İçinde **başlık** metin kutusu, bir başlık yazın.

    b. Gelen **modu** listesinden **SAML**.

    c. **Oluştur**’a tıklayın.

1. **Düzen** yeni uç nokta.

    ![Uç noktayı Düzenle](./media/screensteps-tutorial/ic778528.png "uç noktasını Düzenle")

1. İçinde **Düzenle tek oturum açma uç noktası** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Uzaktan kimlik doğrulama uç noktası](./media/screensteps-tutorial/ic778527.png "uzaktan kimlik doğrulama uç noktası")

    a. Tıklayın **yeni SAML sertifika dosyası karşıya yükleme**ve sonra Azure portalından indirdiğiniz sertifika karşıya yükleyin.

    b. Yapıştırma **oturum açma URL'si** Azure portaldan kopyaladığınız değeri **uzaktan oturum açma URL'si** metin.

    c. Yapıştırma **oturum kapatma URL'si** Azure portaldan kopyaladığınız değeri **oturum kapatma URL'si** metin.

    d. Seçin bir **grubu** olduğunda bunlar sağlanan için kullanıcılara atamak için.

    e. Tıklayın **güncelleştirme**.

    f. Kopyalama **SAML tüketici URL** Pano ve Yapıştır ' **oturum açma URL'si** metin kutusunda **temel SAML yapılandırma** bölümünde Azure portalında.

    g. Geri dönüp **Düzenle tek oturum açma uç noktası**.

    h. Tıklayın **hesabı için varsayılan yap** düğmesine ScreenSteps açan tüm kullanıcılar için bu uç noktayı kullanın. Alternatif olarak tıklayabilirsiniz **ekleme siteye** belirli siteler için bu endpoint kullanmak için düğme **ScreenSteps**.

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

Bu bölümde, Azure çoklu oturum açma kullanmak için ScreenSteps erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **ScreenSteps**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **ScreenSteps**.

    ![Uygulamalar listesinde ScreenSteps bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-screensteps-test-user"></a>ScreenSteps test kullanıcısı oluşturma

Bu bölümde, Britta Simon ScreenSteps içinde adlı bir kullanıcı oluşturun. Çalışmak [ScreenSteps istemci Destek ekibine](https://www.screensteps.com/contact) ScreenSteps platform kullanıcıları eklemek için. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli ScreenSteps kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama ScreenSteps için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)