---
title: 'Öğretici: SAP Business nesne bulut ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve SAP iş nesnesi bulut arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 6c5e44f0-4e52-463f-b879-834d80a55cdf
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/31/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8ad2ffddf96aa6ecc886ac5653d2d0b8dcfb0856
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67091707"
---
# <a name="tutorial-azure-active-directory-integration-with-sap-business-object-cloud"></a>Öğretici: SAP Business nesne bulut ile Azure Active Directory Tümleştirme

Bu öğreticide, SAP iş nesnesi bulut Azure Active Directory (Azure AD) ile tümleştirmeyi öğrenin.
Azure AD ile SAP iş nesnesi bulut tümleştirme ile aşağıdaki avantajları sağlar:

* SAP Business nesnenin Bulutuna erişimi olan Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak (çoklu oturum açma) için SAP iş nesnesi bulut oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

SAP Business nesne bulut ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* SAP Business nesne bulut çoklu oturum açmayı abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* SAP Business nesne bulutun desteklediği **SP** tarafından başlatılan

## <a name="adding-sap-business-object-cloud-from-the-gallery"></a>SAP Business nesne bulut galeri ekleme

Azure AD'de SAP iş nesnesi bulut tümleştirmesini yapılandırmak için SAP iş nesnesi bulut Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**SAP Business nesne bulut Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **SAP iş nesnesi bulut**seçin **SAP iş nesnesi bulut** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde SAP iş nesnesi bulut](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve SAP iş nesnesi bulut adlı bir test kullanıcı tabanlı Azure AD çoklu oturum açmayı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ve SAP iş nesnesi bulutta ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma SAP iş nesnesi Cloud ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[SAP Business nesne bulut çoklu oturum açmayı yapılandırma](#configure-sap-business-object-cloud-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[SAP Business nesne bulut test kullanıcısı oluşturma](#create-sap-business-object-cloud-test-user)**  - kullanıcı Azure AD gösterimini bağlı SAP iş nesnesi bulutta Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma SAP iş nesnesi Bulutla yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **SAP iş nesnesi bulut** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![SAP Business nesne bulut etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:

    | |
    |-|-|
    | `https://<sub-domain>.sapanalytics.cloud/` |
    | `https://<sub-domain>.sapbusinessobjects.cloud/` |

    b. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna şu biçimi kullanarak bir URL yazın:
    
    | |
    |-|-|
    | `<sub-domain>.sapbusinessobjects.cloud` |
    | `<sub-domain>.sapanalytics.cloud` |

    > [!NOTE] 
    > Bu URL'ler gösterimi için değerler. Tanımlayıcı URL'sini ve gerçek oturum açma URL'si ile güncelleştirin. Oturum açma URL'si almak için iletişime geçin [SAP iş nesnesi bulut istemci Destek ekibine](https://help.sap.com/viewer/product/SAP_BusinessObjects_Cloud/release/). SAP Business nesne bulut meta verilerini yönetici konsolundan indirerek tanımlayıcı URL'sini alabilirsiniz. Bu öğreticinin ilerleyen bölümlerinde açıklanmıştır.

4. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

### <a name="configure-sap-business-object-cloud-single-sign-on"></a>SAP Business nesne bulut çoklu oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde bir SAP iş nesnesi bulut şirketinizin sitesi için bir yönetici olarak oturum açın.

2. Seçin **menü** > **sistem** > **Yönetim**.
    
    ![Menü, sonra sistem ve yönetim seçin](./media/sapboc-tutorial/config1.png)

3. Üzerinde **güvenlik** sekmesinde **Düzenle** (Kalem) simgesi.
    
    ![Güvenlik sekmesinde düzenleme simgesini seçin.](./media/sapboc-tutorial/config2.png)  

4. İçin **kimlik doğrulama yöntemi**seçin **SAML çoklu oturum açma (SSO)** .

    ![SAML çoklu oturum açma için kimlik doğrulama yöntemini seçin.](./media/sapboc-tutorial/config3.png)  

5. Hizmet sağlayıcısı meta verileri (adım 1) indirmek için seçin **indirme**. Meta veri dosyasında bulup kopyalayabilirsiniz **Entityıd** değeri. Azure portalında, üzerinde **temel SAML yapılandırma** iletişim kutusunda, değeri olarak yapıştırın **tanımlayıcı** kutusu.

    ![Kopyalama ve yapıştırma Entityıd değeri](./media/sapboc-tutorial/config4.png)  

6. Hizmet sağlayıcısı meta verileri (Adım 2) altında Azure portalından indirilen dosyayı karşıya yüklemek için **karşıya kimlik sağlayıcısı meta verileri**seçin **karşıya**.  

    ![Karşıya yükleme kimlik sağlayıcısı meta verileri altında seçin](./media/sapboc-tutorial/config5.png)

7. İçinde **kullanıcı özniteliği** listesinde, uygulamanız için kullanmak istediğiniz kullanıcı özniteliği (adım 3) seçin. Bu kullanıcı özniteliğini kimlik sağlayıcısına eşler. Özel bir öznitelik kullanıcının sayfasında girmek için kullanın. **özel SAML eşleme** seçeneği. Ya da her ikisini seçebilirsiniz **e-posta** veya **kullanıcı kimliği** kullanıcı özniteliği. Bizim örneğimizde, biz seçili **e-posta** biz kullanıcı tanımlayıcısı talebi ile eşlenmiş çünkü **userprincipalname** özniteliğini **kullanıcı öznitelikleri ve talepler** konusundaki Azure portalı. Bu, her başarılı SAML yanıtını SAP iş nesnesi bulut uygulamasına gönderdiği bir benzersiz kullanıcı e-posta, sağlar.

    ![Kullanıcı özniteliğini seçin](./media/sapboc-tutorial/config6.png)

8. Kimlik sağlayıcısı (adım 4) hesabı doğrulamak için **oturum açma kimlik bilgileri (e-posta)** kutusuna, kullanıcının e-posta adresi girin. Ardından, **hesabı doğrula**. Sistem kullanıcı hesabı ile oturum açma kimlik bilgilerini ekler.

    ![E-posta girin ve doğrulayın hesabı seçin](./media/sapboc-tutorial/config7.png)

9. Seçin **Kaydet** simgesi.

    ![Kaydet simgesine](./media/sapboc-tutorial/save.png)

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

Bu bölümde, SAP iş nesnenin Bulutuna erişimi vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **SAP iş nesnesi bulut**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **SAP iş nesnesi bulut**.

    ![Uygulamalar listesinde SAP iş nesnesi bulut bağlantısı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-sap-business-object-cloud-test-user"></a>SAP Business nesne bulut test kullanıcısı oluşturma

SAP Business nesne buluta oturum önce azure AD kullanıcıları SAP iş nesnesi bulutta sağlanması gerekir. SAP iş nesnesi bulutta sağlama bir el ile gerçekleştirilen bir görevdir.

Bir kullanıcı hesabı sağlamak için:

1. SAP Business nesne bulut şirketinizin sitesi için bir yönetici olarak oturum açın.

2. Seçin **menü** > **güvenlik** > **kullanıcılar**.

    ![Çalışan Ekle](./media/sapboc-tutorial/user1.png)

3. Üzerinde **kullanıcılar** seçin sayfasında, yeni kullanıcı ayrıntıları eklemek için **+** . 

    ![Kullanıcılar sayfasına ekleme](./media/sapboc-tutorial/user4.png)

    Ardından, aşağıdaki adımları tamamlayın:

    a. İçinde **kullanıcı kimliği** kutusuna, kullanıcının kullanıcı kimliği gibi girin **Britta**.

    b. İçinde **ad** kutusunda, kullanıcı adı gibi girin **Britta**.

    c. İçinde **SOYADI** kutusunda, son kullanıcı adı gibi girin **Simon**.

    d. İçinde **GÖRÜNEN ad** kutusuna, kullanıcının tam adı gibi girin **Britta Simon**.

    e. İçinde **e-posta** kutusuna, kullanıcının e-posta adresi gibi girin **brittasimon\@contoso.com**.

    f. Üzerinde **Rollerini Seç** sayfasında kullanıcı için uygun rolü seçin ve ardından **Tamam**.

      ![Rol seç](./media/sapboc-tutorial/user3.png)

    g. Seçin **Kaydet** simgesi.    

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli SAP iş nesnesi bulut kutucuğa tıkladığınızda, size otomatik olarak buluta SAP iş nesnesi SSO'yu ayarlama oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

