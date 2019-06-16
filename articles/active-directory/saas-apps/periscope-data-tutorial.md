---
title: 'Öğretici: Azure Active Directory Tümleştirmesi Periscope verilerle | Microsoft Docs'
description: Azure Active Directory ve Periscope veriler arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 3f378edb-9ac9-494d-a84a-03357b923ee1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/21/2019
ms.author: jeedes
ms.openlocfilehash: efde1f1dafc62576398c5225ad1c652438fc0c31
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67094494"
---
# <a name="tutorial-azure-active-directory-integration-with-periscope-data"></a>Öğretici: Periscope veri ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Periscope veri tümleştirme konusunda bilgi edinin.
Azure AD ile Periscope veri tümleştirme ile aşağıdaki avantajları sağlar:

* Periscope veri erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak Periscope verileri (çoklu oturum açma) oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Periscope verilerle yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Tek bir oturum açma etkin abonelik periscope veri

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Periscope verilerini destekleyen **SP** tarafından başlatılan

## <a name="adding-periscope-data-from-the-gallery"></a>Galeriden Periscope veri ekleme

Azure AD'de Periscope veri tümleştirmesini yapılandırmak için Periscope veri Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Periscope veri eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Periscope veri**seçin **Periscope veri** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde periscope veri](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Periscope adlı bir test kullanıcı göre verileri test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ve ilgili kullanıcı Periscope veri arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Periscope verilerle test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Periscope veri çoklu oturum açmayı yapılandırma](#configure-periscope-data-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Periscope veri test kullanıcısı oluşturma](#create-periscope-data-test-user)**  - kullanıcı Azure AD gösterimini bağlı Periscope veri Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma Periscope verilerle yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Periscope veri** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Periscope veri etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL'lerden herhangi birini yazın:
    
    | |
    |--|
    | `https://app.periscopedata.com/` |
    | `https://app.periscopedata.com/app/<SITENAME>` |

    b. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://app.periscopedata.com/<SITENAME>/sso`

    > [!NOTE]
    > Oturum açma URL değeri, gerçek değil. Gerçek oturum açma URL'si ile güncelleştirin. Kişi [Periscope veri istemci Destek ekibine](mailto:support@periscopedata.com) bu değeri ve gelen alırsınız tanımlayıcı değerini almak için **yapılandırma Periscope veri çoklu oturum açma** bölümünde öğreticinin ilerleyen bölümlerinde açıklanmıştır. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında **SAML imzalama sertifikası** bölümünde, kopyalamak için Kopyala düğmesine **uygulama Federasyon meta verileri URL'sini** ve üzerinde kaydedin, bilgisayar.

    ![Sertifika indirme bağlantısı](common/copy-metadataurl.png)

### <a name="configure-periscope-data-single-sign-on"></a>Periscope veri çoklu oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde Periscope verileri için bir yönetici olarak oturum açın.

2. Alt sol dişli menüsünü açın ve açık **faturalama** > **güvenlik** menü ve aşağıdaki adımları gerçekleştirin. Yalnızca Yöneticiler bu ayarlara erişimi.

    ![Periscope verileri yapılandırma bilgileri](./media/periscope-data-tutorial/configure01.png)

    a. Kopyalama **uygulama Federasyon meta verileri URL'sini** gelen #5. adım **SAML imzalama sertifikası** ve bir tarayıcıda açın. Bu, bir XML belgesi açar.

    b. İçinde **çoklu oturum açma** metin seçme **Azure Active Directory**.

    c. Etiket Bul **SingleSignOnService** Yapıştır **konumu** değerini **SSO URL** metin.

    d. Etiket Bul **SingleLogoutService** Yapıştır **konumu** değerini **SLO URL** metin.

    e. Kopyalama **tanımlayıcı** yapıştırın ve değer Örneğiniz için **tanımlayıcı (varlık kimliği)** textbox'ın **temel SAML yapılandırma** bölümü Azure portalı.

    f. XML dosyasının ilk Etiket Bul, değerini kopyalayın **Entityıd** yapıştırın **veren** metin.

    g. Etiket Bul **IDPSSODescriptor** SAML protokolü. Bu bölümde Etiket Bul **KeyDescriptor** ile **kullanın imzalama =** . değerini kopyalayın **X509Certificate** yapıştırın **sertifika** metin.

    h. Siteleri ile birden fazla alanı varsayılan alan seçebilirsiniz **varsayılan alanı** açılır. Bu yeni kullanıcı eklendi Periscope verileri ilk kez oturum açın ve Active Directory çoklu oturum açma sağlanan alanı olacaktır.

    i. Son olarak, tıklayın **Kaydet** ve **onaylayın** yazarak SSO'su ayarları **oturum kapatma**.

    ![Periscope verileri yapılandırma bilgileri](./media/periscope-data-tutorial/configure02.png)

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

Bu bölümde, Periscope verilere erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Periscope veri**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Periscope veri**.

    ![Uygulamalar listesinde Periscope veri bağlantısı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-periscope-data-test-user"></a>Periscope veri test kullanıcısı oluşturma

Periscope veri oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunlar Periscope verileri sağlanması gerekir. Periscope verilerde sağlama bir el ile bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Periscope verilere yönetici olarak oturum açın.

2. Tıklayarak **ayarları** sol alt kısmındaki menü simgesine gidin **izinleri**.

    ![Periscope verileri yapılandırma bilgileri](./media/periscope-data-tutorial/configure03.png)

3. Tıklayarak **Kullanıcı Ekle** ve aşağıdaki adımları gerçekleştirin:

      ![Periscope verileri yapılandırma bilgileri](./media/periscope-data-tutorial/configure04.png)

    a. İçinde **ad** metin kutusunda, gibi kullanıcı adını girin **Britta**.

    b. İçinde **Soyadı** metin kutusunda, son kullanıcı gibi adını **Simon**.

    c. İçinde **e-posta** metin kutusuna, kullanıcının gibi e-posta girin **brittasimon\@contoso.com**.

    d. Tıklayın **ekleme**.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Periscope veri kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama Periscope verilere oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

