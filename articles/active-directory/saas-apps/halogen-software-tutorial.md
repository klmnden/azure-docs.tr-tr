---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle halojensiz yazılım | Microsoft Docs'
description: Azure Active Directory ve halojensiz yazılım arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 2ca2298d-9a0c-4f14-925c-fa23f2659d28
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/15/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: a8e1f4f0f2bd3521a312523fa9e36dcf14492862
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67101386"
---
# <a name="tutorial-azure-active-directory-integration-with-halogen-software"></a>Öğretici: Halojensiz yazılım ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile halojensiz yazılım tümleştirme konusunda bilgi edinin.
Halojensiz yazılım Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* Halojensiz yazılım erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak (çoklu oturum açma) halojensiz yazılım için oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi halojensiz yazılımıyla yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Abonelik halojensiz yazılım çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Destekleyen halojensiz yazılımı **SP** tarafından başlatılan

## <a name="adding-halogen-software-from-the-gallery"></a>Galeriden halojensiz yazılım ekleme

Azure AD'de halojensiz yazılım tümleştirmesini yapılandırmak için halojensiz yazılım Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden halojensiz yazılım eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **halojensiz yazılım**seçin **halojensiz yazılım** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde halojensiz yazılım](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma halojensiz yazılım adlı bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının halojensiz yazılımla ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma halojensiz yazılım ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Halojensiz yazılım çoklu oturum açmayı yapılandırma](#configure-halogen-software-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Halojensiz yazılım test kullanıcısı oluşturma](#create-halogen-software-test-user)**  - kullanıcı Azure AD gösterimini bağlı halojensiz yazılım Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma halojensiz yazılımıyla yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **halojensiz yazılım** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Halojensiz yazılım etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://global.hgncloud.com/<companyname>`

    b. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna şu biçimi kullanarak bir URL yazın:

    | |
    |--|
    | `https://global.halogensoftware.com/<companyname>`|
    | `https://global.hgncloud.com/<companyname>`|
    | |

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL ve tanımlayıcıdır ile güncelleştirin. İlgili kişi [halojensiz yazılım istemcisi Destek ekibine](https://support.halogensoftware.com/) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

4. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. Üzerinde **halojensiz yazılımını kurma** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-halogen-software-single-sign-on"></a>Halojensiz yazılım çoklu oturum açmayı yapılandırın

1. Bir farklı bir tarayıcı penceresinde için oturum açma, **halojensiz yazılım** uygulamasını yönetici olarak.

2. Tıklayın **seçenekleri** sekmesi.
  
    ![Azure AD Connect nedir?](./media/halogen-software-tutorial/tutorial_halogen_12.png)

3. Sol gezinti bölmesinden **SAML yapılandırma**.
  
    ![Azure AD Connect nedir?](./media/halogen-software-tutorial/tutorial_halogen_13.png)

4. Üzerinde **SAML yapılandırma** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Azure AD Connect nedir?](./media/halogen-software-tutorial/tutorial_halogen_14.png)

    a. Olarak **benzersiz tanımlayıcı**seçin **Nameıd**.

    b. Olarak **benzersiz tanımlayıcı eşlemeleri için**seçin **Username**.
  
    c. İndirilen meta veri dosyası karşıya yüklemek için tıklayın **Gözat** dosyasını seçin ve ardından **dosyasını karşıya yükle**.

    d. Test yapılandırması için Yardım düğmesini tıklatın **Test çalıştırması**.

    > [!NOTE]
    > Öğesinin ileti beklemesi gerekiyor "*SAML sınav tamamlanana. Lütfen bu pencereyi kapatın*". Ardından, açılan tarayıcı penceresini kapatın. **Etkinleştirme SAML** onay kutusu yalnızca etkin olmadığını test tamamlandı.

    e. Seçin **etkinleştirme SAML**.

    f. Tıklayın **değişiklikleri kaydetmek**.

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

Bu bölümde, Azure çoklu oturum açma halojensiz yazılıma erişim vererek kullanmak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **halojensiz yazılım**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **halojensiz yazılım**.

    ![Uygulamalar listesinde halojensiz yazılım bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-halogen-software-test-user"></a>Halojensiz yazılım test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon halojensiz yazılım adlı bir kullanıcı oluşturmaktır.

**Britta Simon halojensiz yazılım adlı bir kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açın, **halojensiz yazılım** uygulamasını yönetici olarak.

2. Tıklayın **kullanıcı Merkezi** sekmesine ve ardından **Create User**.

    ![Azure AD Connect nedir?](./media/halogen-software-tutorial/tutorial_halogen_300.png)  

3. Üzerinde **yeni kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Azure AD Connect nedir?](./media/halogen-software-tutorial/tutorial_halogen_301.png)

    a. İçinde **ad** metin adı gibi kullanıcı türü **Britta**.

    b. İçinde **Soyadı** metin türü Soyadı gibi kullanıcının **Simon**.

    c. İçinde **kullanıcıadı** metin kutusuna **Britta Simon**, Azure portalında olduğu gibi kullanıcı adı.

    d. İçinde **parola** metin Britta parolasını yazın.

    e. **Kaydet**’e tıklayın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde halojensiz yazılım kutucuğa tıkladığınızda, otomatik olarak SSO'yu ayarlama halojensiz yazılım için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)