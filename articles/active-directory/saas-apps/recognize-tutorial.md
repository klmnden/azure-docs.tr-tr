---
title: 'Öğretici: Tanı ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve tanı arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: cfad939e-c8f4-45a0-bd25-c4eb9701acaa
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/27/2019
ms.author: jeedes
ms.openlocfilehash: bd772a10cd64b4198e994fdefa671444447c8a53
ms.sourcegitcommit: 04716e13cc2ab69da57d61819da6cd5508f8c422
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2019
ms.locfileid: "58849565"
---
# <a name="tutorial-azure-active-directory-integration-with-recognize"></a>Öğretici: Tanı ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile tanı tümleştirme konusunda bilgi edinin.
Tanı Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* Tanı erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak (çoklu oturum açma) için tanı kendi Azure AD hesapları ile oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile tanı yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Çoklu oturum açma etkin abonelik tanıması

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Destekler tanımak **SP** tarafından başlatılan

## <a name="adding-recognize-from-the-gallery"></a>Tanı galeri ekleme

Azure AD'de tanı tümleştirmesini yapılandırmak için tanı Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden tanı eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **tanı**seçin **tanı** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuçlar listesinde tanıması](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma tanı adlı bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ve tanı ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma tanı ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Tanı çoklu oturum açmayı yapılandırma](#configure-recognize-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Tanı test kullanıcısı oluşturma](#create-recognize-test-user)**  - kullanıcı Azure AD gösterimini bağlı tanı Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile tanı yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **tanı** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** varsa, bölüm **hizmet sağlayıcısı meta veri dosyası**, aşağıdaki adımları gerçekleştirin:

    >[!NOTE]
    >Erişmenizi sağlayacak **hizmet sağlayıcısı meta veri dosyası** gelen **Yapılandırma tanı çoklu oturum açma** öğreticinin bölümünde.

    a. Tıklayın **meta veri dosyasını karşıya yükleme**.

    ![Meta veri dosyasını yükleyin](common/upload-metadata.png)

    b. Tıklayarak **klasör logosu** meta veri dosyası seçin ve **karşıya**.

    ![meta veri dosyası seçin](common/browse-upload-metadata.png)

    c. Meta veri dosyası başarıyla karşıya yüklendikten sonra **tanımlayıcı** değer elde otomatik temel SAML yapılandırma bölümünde doldurulur.

    ![Etki alanı ve URL'ler tek oturum açma bilgileri tanıması](common/sp-identifier.png)

     İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:  `https://recognizeapp.com/<your-domain>/saml/sso`

    > [!Note]
    > Varsa **tanımlayıcı** değeri alınamadı otomatik doldurulur, daha sonra açıklanan SSO ayarları bölümünden hizmet sağlayıcısı meta verileri URL'sini açarak tanımlayıcı değeri alacaktır **tanımak tek yapılandırma Oturum açma** öğreticinin bölümünde. Oturum açma URL değeri, gerçek değil. Değerini gerçek oturum açma URL'si ile güncelleştirin. Kişi [tanımak istemci Destek ekibine](mailto:support@recognizeapp.com) değeri alınamıyor. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. Üzerinde **tanı kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-recognize-single-sign-on"></a>Yapılandırma çoklu oturum açma tanıması

1. Farklı bir web tarayıcı penceresinde tanı Kiracı yönetici olarak oturum açın.

2. Sağ üst köşede tıklayın **menü**. Git **şirket Yöneticisi**.
   
    ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/recognize-tutorial/tutorial_recognize_000.png)

3. Sol gezinti bölmesinde **ayarları**.
   
    ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/recognize-tutorial/tutorial_recognize_001.png)

4. Aşağıdaki adımları gerçekleştirin **SSO ayarlarını** bölümü.
   
    ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/recognize-tutorial/tutorial_recognize_002.png)
    
    a. Olarak **SSO etkinleştirme**seçin **ON**.

    b. İçinde **IDP varlık kimliği** metin değerini yapıştırın **Azure AD tanımlayıcısı** , Azure Portalı'ndan kopyaladığınız.
    
    c. İçinde **Sso hedef url** metin değerini yapıştırın **oturum açma URL'si** , Azure Portalı'ndan kopyaladığınız.
    
    d. İçinde **Slo hedef url** metin değerini yapıştırın **oturum kapatma URL'si** , Azure Portalı'ndan kopyaladığınız. 
    
    e. İndirilen açın **sertifika (Base64)** dosyasını Not Defteri'nde, içeriğini, panoya kopyalayın ve ardından ona yapıştırın **sertifika** metin.
    
    f. Tıklayın **ayarlarını kaydetmek** düğmesi. 

5. Yanında **SSO ayarlarını** bölümünde, altında URL'yi kopyalayın **hizmet sağlayıcısı meta verileri URL'sini**.
   
    ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/recognize-tutorial/tutorial_recognize_003.png)

6. Açık **meta veri URL'si bağlantı** meta veri belgesini indirmek için boş bir tarayıcı altında. Ardından EntityDescriptor value(entityID) dosyasından kopyalayın ve yapıştırın **tanımlayıcı** metin kutusunda **temel SAML yapılandırma** Azure portalında.
    
    ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/recognize-tutorial/tutorial_recognize_004.png)

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

Bu bölümde, Azure çoklu oturum açma kullanmak için tanı erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **tanı**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **tanı**.

    ![Uygulamalar listesinde tanı bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-recognize-test-user"></a>Tanı test kullanıcısı oluşturma

Tanı açarken Azure AD kullanıcılarının etkinleştirmek için bunların tanı sağlanmalıdır. Tanı söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

Bu uygulama SCIM sağlama desteklemez ancak kullanıcılar sağlayan bir alternatif kullanıcı eşitleme sahiptir. 

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Tanı şirket sitenize yönetici olarak oturum açın.

2. Sağ üst köşede tıklayın **menü**. Git **şirket Yöneticisi**.

3. Sol gezinti bölmesinde **ayarları**.

4. Aşağıdaki adımları gerçekleştirin **kullanıcı eşitleme** bölümü.
   
    ![Yeni kullanıcı](./media/recognize-tutorial/tutorial_recognize_005.png "yeni kullanıcı")
   
    a. Olarak **eşitleme özelliği etkin**seçin **ON**.
   
    b. Olarak **Seç eşitleme sağlayıcısı**seçin **Microsoft / Office 365**.
   
    c. Tıklayın **kullanıcı eşitleme Çalıştır**.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli tanı kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama tanı için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [ SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

