---
title: 'Öğretici: Arcgıs Online ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Arcgıs Online arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: a9e132a4-29e7-48bf-beb9-4148e617c8a9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/19/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: db56cd7551ef8179aeff575fdd1f2578cbee74ee
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67106700"
---
# <a name="tutorial-azure-active-directory-integration-with-arcgis-online"></a>Öğretici: Arcgıs Online ile Azure Active Directory Tümleştirmesi

Bu öğreticide, Azure Active Directory (Azure AD) ile Arcgıs Online tümleştirme konusunda bilgi edinin.
Arcgıs Online Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* Arcgıs Online erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak Arcgıs Online için (çoklu oturum açma) oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Arcgıs Online ile yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Arcgıs Online çoklu oturum açma abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Arcgıs Online destekler **SP** tarafından başlatılan

## <a name="adding-arcgis-online-from-the-gallery"></a>Arcgıs Online galeri ekleme

Azure AD'de Arcgıs Online'nın tümleştirmesini yapılandırmak için Arcgıs Online Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Arcgıs Online Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Arcgıs Online**seçin **Arcgıs Online** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Arcgıs Online sonuç listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve adlı bir test kullanıcı tabanlı Arcgıs Online ile Azure AD çoklu oturum açmayı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ile ilgili kullanıcı Arcgıs Online arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Arcgıs Online ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Arcgıs Online çoklu oturum açmayı yapılandırma](#configure-arcgis-online-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Arcgıs Online test kullanıcısı oluşturma](#create-arcgis-online-test-user)**  - Arcgıs kullanıcı Azure AD gösterimini bağlı olan çevrimiçi bir karşılığı Britta simon'un sağlamak için.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma Arcgıs Online ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Arcgıs Online** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Arcgıs Online etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<companyname>.maps.arcgis.com`

    b. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna şu biçimi kullanarak bir URL yazın: `<companyname>.maps.arcgis.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL ve tanımlayıcıdır ile güncelleştirin. İlgili kişi [Arcgıs Online istemci Destek ekibine](https://support.esri.com/en/) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. İçinde yapılandırmasını otomatik hale getirmenizi **Arcgıs Online**, yüklemeniz gerekir **My Apps güvenli oturum açma tarayıcı uzantısı** tıklayarak **uzantıyı yükleme**.

    ![image](./media/arcgis-tutorial/install_extension.png)

7. Uzantı tarayıcıya ekledikten sonra tıklayarak **Arcgıs Online Kurulum** Arcgıs Online uygulamaya yönlendirir. Burada, Arcgıs Online imzalamak için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı otomatik olarak sizin için uygulamayı yapılandırma ve bölümdeki adımları otomatikleştirmek **yapılandırma Arcgıs Online çoklu oturum açma**.

### <a name="configure-arcgis-online-single-sign-on"></a>Arcgıs Online çoklu oturum açmayı yapılandırın

1. Arcgıs Online el ile ayarlamak istiyorsanız, yeni bir web tarayıcı penceresi açın ve Arcgıs şirketinizin sitesi yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

2. Tıklayın **ayarları düzenleme**.

    ![Ayarları Düzenle](./media/arcgis-tutorial/ic784742.png "ayarlarını Düzenle")

3. Tıklayın **güvenlik**.

    ![Güvenlik](./media/arcgis-tutorial/ic784743.png "güvenlik")

4. Altında **Kurumsal oturum açma bilgileri**, tıklayın **AYARLANMIŞ bir kimlik sağlayıcı**.

    ![Kurumsal oturum açma bilgileri](./media/arcgis-tutorial/ic784744.png "Kurumsal oturum açma bilgileri")

5. Üzerinde **ayarlanmış bir kimlik sağlayıcı** yapılandırma sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Kimlik sağlayıcısını Ayarla](./media/arcgis-tutorial/ic784745.png "kimlik sağlayıcısını Ayarla")

    a. İçinde **adı** metin kutusuna kuruluşunuzun adını yazın.

    b. İçin **Kurumsal kimlik sağlayıcısı meta verileri kullanarak belirtilmelidir**seçin **dosya**.

    c. İndirilen meta verileri dosyanızı karşıya yüklemek için tıklayın **dosya**.

    d. Tıklayın **KÜMESİ kimlik SAĞLAYICISI**.

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

Bu bölümde, Arcgıs Online erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Arcgıs Online**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde yazın ve **Arcgıs Online**.

    ![Uygulamalar listesinde Arcgıs Online bağlantısı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-arcgis-online-test-user"></a>Arcgıs Online test kullanıcısı oluşturma

Arcgıs Online'da oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunlar Arcgıs Online sağlanması gerekir.  
Arcgıs Online söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açın, **Arcgıs** Kiracı.

2. Tıklayın **davet ÜYELERİ**.
   
    ![Üyeler davet](./media/arcgis-tutorial/ic784747.png "üyeler davet")

3. Seçin **üyeleri otomatik olarak bir e-posta göndermeden ekleme**ve ardından **sonraki**.
   
    ![Üyeleri otomatik olarak Ekle](./media/arcgis-tutorial/ic784748.png "üyeleri otomatik olarak Ekle")

4. Üzerinde **üyeleri** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
   
     ![Ekleme ve gözden geçirme](./media/arcgis-tutorial/ic784749.png "ekleme ve gözden geçirme")
    
     a. Girin **e-posta**, **ad**, ve **Soyadı** sağlamak istediğiniz geçerli bir AAD hesabı.
  
     b. Tıklayın **ekleme ve gözden geçirme**.
5. Girdiğiniz ve ardından verileri gözden **Üye Ekle**.
   
    ![Üye Ekle](./media/arcgis-tutorial/ic784750.png "Üye Ekle")
        
    > [!NOTE]
    > Azure Active Directory hesap sahibinin e-posta alır ve bunu etkinleştirilmeden önce hesabını onaylamak için bağlantıyı izleyin.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Arcgıs Online kutucuğa tıkladığınızda, size otomatik olarak Arcgıs çevrimiçi SSO'yu ayarlamak için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

