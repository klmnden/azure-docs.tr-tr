---
title: 'Öğretici: Citrix ShareFile ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Citrix ShareFile ile Azure Active Directory arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: e14fc310-bac4-4f09-99ef-87e5c77288b6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/24/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: d5839329f78fc349210f9439c5b1580a21f5172a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "65867471"
---
# <a name="tutorial-azure-active-directory-integration-with-citrix-sharefile"></a>Öğretici: Citrix ShareFile ile Azure Active Directory Tümleştirme

Bu öğreticide, Citrix ShareFile, Azure Active Directory (Azure AD) ile tümleştirme konusunda bilgi edinin.
Citrix ShareFile, Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* Citrix ShareFile erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak (çoklu oturum açma) için Citrix ShareFile oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Citrix ShareFile ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Citrix ShareFile çoklu oturum açma abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Citrix ShareFile destekler **SP** tarafından başlatılan

## <a name="adding-citrix-sharefile-from-the-gallery"></a>Citrix ShareFile galeri ekleme

Azure AD'de Citrix ShareFile tümleştirmesini yapılandırmak için Citrix ShareFile galerideki yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Citrix ShareFile galerideki eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Citrix ShareFile**seçin **Citrix ShareFile** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Citrix ShareFile sonuç listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırmanız ve Citrix ShareFile ile Azure AD çoklu oturum açmayı test adlı bir test kullanıcı tabanlı **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ve Citrix ShareFile ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Citrix ShareFile ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Citrix ShareFile çoklu oturum açmayı yapılandırma](#configure-citrix-sharefile-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Citrix ShareFile test kullanıcısı oluşturma](#create-citrix-sharefile-test-user)**  - bir karşılığı Britta simon'un kullanıcı Azure AD gösterimini bağlı Citrix ShareFile sağlamak için.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Citrix ShareFile ile Azure AD çoklu oturum açmayı yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Citrix ShareFile** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Citrix ShareFile etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier-reply.png)

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<tenant-name>.sharefile.com/saml/login`

    b. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna bir URL şu biçimi kullanarak:

    | |
    |---|
    | `https://<tenant-name>.sharefile.com`|
    | `https://<tenant-name>.sharefile.com/saml/info`|
    | `https://<tenant-name>.sharefile1.com/saml/info`|
    | `https://<tenant-name>.sharefile1.eu/saml/info`|
    | `https://<tenant-name>.sharefile.eu/saml/info`|
    | |
    
    c. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak:
    
    | |
    |---|
    | `https://<tenant-name>.sharefile.com/saml/acs`|
    | `https://<tenant-name>.sharefile.eu/saml/<URL path>`|
    | `https://<tenant-name>.sharefile.com/saml/<URL path>`|

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si, tanımlayıcı ve yanıt URL'si ile güncelleştirin. İlgili kişi [Citrix ShareFile istemci Destek ekibine](https://www.citrix.co.in/products/citrix-content-collaboration/support.html) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

4. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. Üzerinde **Citrix ShareFile kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-citrix-sharefile-single-sign-on"></a>Citrix ShareFile çoklu oturum açmayı yapılandırın

1. Oturum başka bir web tarayıcı penceresinde açın, **Citrix ShareFile** yönetici olarak şirketin site.

2. Üst araç çubuğunda tıklatın **yönetici**.

3. Sol gezinti bölmesinde seçin **yapılandırma çoklu oturum açma**.
   
    ![Hesap Yönetimi](./media/sharefile-tutorial/ic773627.png "hesap yönetimi")

4. Üzerinde **çoklu oturum açma / SAML 2.0 Yapılandırması** iletişim sayfanın altında **temel ayarları**, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açma](./media/sharefile-tutorial/ic773628.png "çoklu oturum açma")
   
    a. Tıklayın **etkinleştirme SAML**.
    
    b. İçinde **bilgisayarınızı IDP veren / varlık kimliği** metin değerini yapıştırın **Azure Ad tanımlayıcısı** , Azure Portalı'ndan kopyaladığınız.

    c. Tıklayın **değişiklik** yanındaki **X.509 sertifikası** alan ve sonra Azure portalından indirdiğiniz sertifika karşıya yükleyin.
    
    d. İçinde **oturum açma URL'si** metin değerini yapıştırın **oturum açma URL'si** , Azure Portalı'ndan kopyaladığınız.
    
    e. İçinde **oturum kapatma URL'si** metin değerini yapıştırın **oturum kapatma URL'si** , Azure Portalı'ndan kopyaladığınız.

5. Tıklayın **Kaydet** Citrix ShareFile Yönetim Portalı'nda.

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

Bu bölümde, Azure çoklu oturum açma kullanmak için Citrix ShareFile erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Citrix ShareFile**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde yazın ve **Citrix ShareFile**.

    ![Citrix ShareFile bağlantıya uygulamalar listesi](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-citrix-sharefile-test-user"></a>Citrix ShareFile test kullanıcısı oluşturma

Citrix ShareFile açarken Azure AD kullanıcılarının etkinleştirmek için bunlar uygulamasına Citrix ShareFile sağlanması gerekir. Citrix ShareFile söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açın, **Citrix ShareFile** Kiracı.

2. Tıklayın **kullanıcıları yönetme \> kullanıcıların giriş yönetme \> + çalışan Oluştur**.
   
    ![Çalışan oluşturma](./media/sharefile-tutorial/IC781050.png "çalışan oluşturun")

3. Üzerinde **temel bilgileri** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Temel bilgiler](./media/sharefile-tutorial/IC799951.png "temel bilgileri")
   
    a. İçinde **e-posta adresi** metin Britta Simon e-posta adresini yazın **brittasimon\@contoso.com**.
   
    b. İçinde **ad** metin kutusuna **ad** kullanıcının **Britta**.
   
    c. İçinde **Soyadı** metin kutusuna **Soyadı** kullanıcının **Simon**.

4. Tıklayın **kullanıcı ekleme**.
  
    >[!NOTE]
    >Azure AD hesap sahibinin e-posta alır ve bunu etkinleştirilmeden önce hesabını onaylamak için bağlantıyı izleyin. Azure AD kullanıcı hesapları sağlamak için herhangi bir Citrix ShareFile kullanıcı hesabı oluşturma araçları veya Citrix ShareFile tarafından sağlanan API'leri kullanabilirsiniz.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Citrix ShareFile kutucuğa tıkladığınızda, size otomatik olarak Citrix ShareFile SSO'yu ayarlamak için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

