---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile xMatters OnDemand | Microsoft Docs'
description: Azure Active Directory ve xMatters OnDemand arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: ca0633db-4f95-432e-b3db-0168193b5ce9
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/29/2019
ms.author: jeedes
ms.openlocfilehash: e8ae31122d59238ac104d7d873cf56f32977c9af
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67086512"
---
# <a name="tutorial-azure-active-directory-integration-with-xmatters-ondemand"></a>Öğretici: OnDemand xMatters ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile xMatters OnDemand tümleştirme konusunda bilgi edinin.
Azure AD ile xMatters OnDemand tümleştirme ile aşağıdaki avantajları sağlar:

* OnDemand xMatters erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak xMatters OnDemand (çoklu oturum açma) için kendi Azure AD hesapları ile oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi OnDemand xMatters ile yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa alabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free/).
* OnDemand xMatters tek oturum açma etkin abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* OnDemand destekler xMatters **IDP** tarafından başlatılan

## <a name="adding-xmatters-ondemand-from-the-gallery"></a>Galeriden xMatters OnDemand ekleme

Azure AD'de xMatters OnDemand tümleştirmesini yapılandırmak için xMatters OnDemand Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden xMatters OnDemand eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **xMatters OnDemand**seçin **xMatters OnDemand** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![sonuç listesinde xMatters OnDemand](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma xMatters OnDemand tabanlı adlı bir test kullanıcı ile test etme **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının xMatters ilgili kullanıcı arasında bir bağlantı ilişki OnDemand kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma xMatters OnDemand ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[OnDemand çoklu oturum açma xMatters yapılandırma](#configure-xmatters-ondemand-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[XMatters OnDemand test kullanıcısı oluşturma](#create-xmatters-ondemand-test-user)**  - Britta Simon xMatters kullanıcı Azure AD gösterimini bağlı OnDemand içinde bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma OnDemand xMatters ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **xMatters OnDemand** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![OnDemand etki alanı ve URL'ler xMatters çoklu oturum açma bilgileri](common/idp-intiated.png)

    a. İçinde **tanımlayıcı** metin kutusuna şu biçimi kullanarak bir URL yazın:

    | |
    |--|
    | `https://<companyname>.au1.xmatters.com.au/`|
    | `https://<companyname>.cs1.xmatters.com/`|
    | `https://<companyname>.xmatters.com/`|
    | `https://www.xmatters.com`|
    | `https://<companyname>.xmatters.com.au/`|
    | |

    b. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:

    | |
    |--|
    | `https://<companyname>.au1.xmatters.com.au`|
    | `https://<companyname>.xmatters.com/sp/<instancename>`|
    | `https://<companyname>.cs1.xmatters.com/sp/<instancename>`|
    | `https://<companyname>.au1.xmatters.com.au/<instancename>`|
    | |

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı ve yanıt URL'si ile güncelleştirin. İlgili kişi [xMatters OnDemand istemci Destek ekibine](https://www.xmatters.com/company/contact-us/) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

    > [!IMPORTANT]
    > Sertifikayı iletmek için ihtiyacınız [xMatters OnDemand Destek ekibine](https://www.xmatters.com/company/contact-us/). Çoklu oturum açma yapılandırması son haline getir önce xMatters destek ekibi tarafından karşıya yüklenecek sertifika gerekir.

6. Üzerinde **OnDemand xMatters kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-xmatters-ondemand-single-sign-on"></a>XMatters OnDemand çoklu oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde XMatters OnDemand şirketinizin sitesi için bir yönetici olarak oturum açın.

2. Üst araç çubuğunda tıklatın **yönetici**ve ardından **şirket ayrıntıları** sol taraftaki gezinti çubuğunda.

    ![Yönetici](./media/xmatters-ondemand-tutorial/IC776795.png "yönetici")

3. Üzerinde **SAML yapılandırma** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![SAML yapılandırma](./media/xmatters-ondemand-tutorial/IC776796.png "SAML yapılandırma")

    a. Seçin **etkinleştirme SAML**.

    b. İçinde **kimlik sağlayıcı kimliği** metin kutusu, yapıştırma **Azure AD tanımlayıcısı** Azure portaldan kopyaladığınız değeri.

    c. İçinde **üzerinde çoklu oturum URL'si** metin kutusu, yapıştırma **oturum açma URL'si** Azure portaldan kopyaladığınız değeri.

    d. İçinde **çoklu oturum kapatma URL'si** metin kutusu, yapıştırma **oturum kapatma URL'si**, hangi Azure portaldan kopyaladığınız.

    e. Şirket Ayrıntıları sayfası, en üstte tıklayarak **Değişiklikleri Kaydet**.

    ![Şirket ayrıntıları](./media/xmatters-ondemand-tutorial/IC776797.png "şirket ayrıntıları")

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

Bu bölümde, Azure çoklu oturum açma xMatters OnDemand erişim vererek kullanmak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **xMatters OnDemand**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **xMatters OnDemand**.

    ![Uygulamalar listesinde OnDemand bağlantı xMatters](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-xmatters-ondemand-test-user"></a>XMatters OnDemand test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon OnDemand xMatters içinde adlı bir kullanıcı oluşturmaktır.

**Kullanıcı el ile oluşturmanız gerekiyorsa, aşağıdaki adımları uygulayın:**

1. Oturum açın, **XMatters OnDemand** Kiracı.

2. Tıklayın **kullanıcılar** sekmesini ve ardından **Kullanıcı Ekle**.

    ![Kullanıcılar](./media/xmatters-ondemand-tutorial/IC781048.png "kullanıcılar")

3. İçinde **kullanıcı ekleme** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı ekleme](./media/xmatters-ondemand-tutorial/IC781049.png "kullanıcı ekleme")

    a. Seçin **etkin**.

    b. İçinde **kullanıcı kimliği** metin kutusu, kullanıcının kullanıcı kimliği türü ister Brittasimon@contoso.com.

    c. İçinde **ad** metin adı Britta gibi kullanıcı türü.

    d. İçinde **Soyadı** metin son Simon gibi kullanıcının adını yazın.

    e. İçinde **Site** metin Enter geçerli sitenin geçerli bir Azure AD hesabı sağlamak istediğiniz.

    f. **Kaydet**’e tıklayın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

OnDemand kutucuk erişim Paneli'nde xMatters tıklattığınızda, otomatik olarak SSO'yu ayarlama xMatters OnDemand için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

