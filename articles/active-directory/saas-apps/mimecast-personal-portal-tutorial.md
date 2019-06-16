---
title: 'Öğretici: Mimecast kişisel portalı ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Mimecast kişisel Portal arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 345b22be-d87e-45a4-b4c0-70a67eaf9bfd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/24/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: bfafa1157619e151f97fcf9c8a410a0644354b80
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67097391"
---
# <a name="tutorial-azure-active-directory-integration-with-mimecast-personal-portal"></a>Öğretici: Mimecast kişisel portalı ile Azure Active Directory Tümleştirme

Bu öğreticide, Mimecast kişisel portalı Azure Active Directory (Azure AD) ile tümleştirmeyi öğrenin.
Mimecast kişisel portalı, Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* Mimecast kişisel Portal erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak (çoklu oturum açma) Mimecast kişisel portalı için oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Mimecast kişisel portalıyla yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Abonelik Mimecast kişisel portalı çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Mimecast kişisel Portal destekler **SP** tarafından başlatılan

## <a name="adding-mimecast-personal-portal-from-the-gallery"></a>Galeriden Mimecast kişisel portalı ekleme

Azure AD'de Mimecast kişisel portalı tümleştirmesini yapılandırmak için Mimecast kişisel portalı Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Mimecast kişisel portalı eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Mimecast kişisel portalı**seçin **Mimecast kişisel portalı** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde Mimecast kişisel portalı](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Mimecast kişisel adlı bir test kullanıcı tabanlı Portal ile Azure AD çoklu oturum açmayı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ve ilgili kullanıcı Mimecast kişisel portalında arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Mimecast kişisel portalı ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Mimecast kişisel portalı çoklu oturum açmayı yapılandırma](#configure-mimecast-personal-portal-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Mimecast kişisel portalı test kullanıcısı oluşturma](#create-mimecast-personal-portal-test-user)**  - kullanıcı Azure AD gösterimini bağlı Mimecast kişisel portalında Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma Mimecast kişisel portalıyla yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Mimecast kişisel portalı** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Mimecast kişisel portalı etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier-reply.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL: 

    | Bölge  |  Değer | 
    | --------------- | --------------- | 
    | Avrupa          | `https://eu-api.mimecast.com/login/saml`|
    | Amerika Birleşik Devletleri   | `https://us-api.mimecast.com/login/saml`|
    | Güney Afrika    | `https://za-api.mimecast.com/login/saml`|
    | Avustralya       | `https://au-api.mimecast.com/login/saml`|
    | Yurtdışında        | `https://jer-api.mimecast.com/login/saml`|

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak:

    | Bölge  |  Değer | 
    | --------------- | --------------- |
    | Avrupa          | `https://eu-api.mimecast.com/sso/<accountcode>`|
    | Amerika Birleşik Devletleri   | `https://us-api.mimecast.com/sso/<accountcode>`|    
    | Güney Afrika    | `https://za-api.mimecast.com/sso/<accountcode>`|
    | Avustralya       | `https://au-api.mimecast.com/sso/<accountcode>`|
    | Yurtdışında        | `https://jer-api.mimecast.com/sso/<accountcode>`|

    c. İçinde **yanıt URL'si** metin kutusuna bir URL: 

    | Bölge  |  Değer | 
    | --------------- | --------------- | 
    | Avrupa          | `https://eu-api.mimecast.com/login/saml`|
    | Amerika Birleşik Devletleri   | `https://us-api.mimecast.com/login/saml`|
    | Güney Afrika    | `https://za-api.mimecast.com/login/saml`|
    | Avustralya       | `https://au-api.mimecast.com/login/saml`|
    | Yurtdışında        | `https://jer-api.mimecast.com/login/saml`|

    > [!NOTE]
    > Tanımlayıcı değerini gerçek değil. Değerini gerçek tanımlayıcısıyla güncelleştirin. İlgili kişi [Mimecast kişisel portalı istemcisi Destek ekibine](https://www.mimecast.com/customer-success/technical-support/) değeri alınamıyor. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

4. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. Üzerinde **Mimecast kişisel Portal'ı ayarlama** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-mimecast-personal-portal-single-sign-on"></a>Mimecast kişisel Portal çoklu oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde, bir yönetici olarak, Mimecast kişisel portalında oturum açın.

2. Git **Hizmetleri \> uygulamaları**.
   
    ![Uygulamaları](./media/mimecast-personal-portal-tutorial/ic794998.png "uygulamaları")

3. Tıklayın **kimlik doğrulaması profilleri**.
   
    ![Kimlik doğrulaması profilleri](./media/mimecast-personal-portal-tutorial/ic794999.png "kimlik doğrulaması profilleri")

4. Tıklayın **yeni kimlik doğrulama profili**.
   
    ![Yeni kimlik doğrulama profili](./media/mimecast-personal-portal-tutorial/ic795000.png "yeni kimlik doğrulama profili")

5. İçinde **kimlik doğrulama profili** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Kimlik doğrulama profili](./media/mimecast-personal-portal-tutorial/ic795001.png "kimlik doğrulama profili")
   
    a. İçinde **açıklama** metin yapılandırmanız için bir ad yazın.
   
    b. Seçin **Mimecast kişisel portalı SAML kimlik doğrulaması zorunlu**.
   
    c. Olarak **sağlayıcısı**seçin **Azure Active Directory**.
   
    d. İçinde **veren URL'si** metin değerini yapıştırın **Azure Ad tanımlayıcısı**, hangi Azure Portalı'ndan kopyaladığınız.
   
    e. İçinde **oturum açma URL'si** metin değerini yapıştırın **oturum açma URL'si**, hangi Azure Portalı'ndan kopyaladığınız.
   
    f. İçinde **oturum kapatma URL'si** metin değerini yapıştırın **oturum kapatma URL'si**, hangi Azure Portalı'ndan kopyaladığınız.

    g. Açık, **base-64** Defteri'nde kodlanmış sertifika Azure portalından indirilen, içeriğini, panoya kopyalayın ve ardından ona yapıştırın **kimlik sağlayıcısı sertifikası (meta veriler)** metin.

    h. Seçin **çoklu oturum açmaya izin ver**.
   
    i. **Kaydet**’e tıklayın.

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

Bu bölümde, kişisel Mimecast Portalı'na erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Mimecast kişisel portalı**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde yazın ve **Mimecast kişisel portalı**.

    ![Uygulamalar listesinde Mimecast kişisel portalı bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-mimecast-personal-portal-test-user"></a>Mimecast kişisel portalı test kullanıcısı oluşturma

Mimecast kişisel portalda oturum açarken Azure AD kullanıcılarının etkinleştirmek için bunlar Mimecast kişisel portalına sağlanması gerekir. Mimecast kişisel söz konusu olduğunda, sağlama elle bir görevin portalıdır.

Kullanıcılar oluşturabilmeniz için önce bir etki alanı kayıt olmanız gerekir.

**Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açın, **Mimecast kişisel portalı** yönetici olarak.

2. Git **dizinleri \> iç**.
   
    ![Dizinleri](./media/mimecast-personal-portal-tutorial/ic795003.png "dizinleri")

3. Tıklayın **yeni etki alanı kayıt**.
   
    ![Yeni etki alanı kayıt](./media/mimecast-personal-portal-tutorial/ic795004.png "kaydetme yeni etki alanı")

4. Yeni etki alanınız oluşturulduktan sonra tıklayın **yeni adresi**.
   
    ![Yeni adresi](./media/mimecast-personal-portal-tutorial/ic795005.png "yeni adresi")

5. Yeni adresi iletişim kutusunda geçerli bir Azure aşağıdaki adımları sağlamak istediğiniz AD hesabı:
   
    ![Kaydet](./media/mimecast-personal-portal-tutorial/ic795006.png "Kaydet")
   
    a. İçinde **e-posta adresi** metin kutusuna **e-posta adresi** olarak kullanıcı **BrittaSimon\@contoso.com**.
    
    b. İçinde **genel adı** metin kutusuna **kullanıcıadı** olarak **BrittaSimon**.

    c. İçinde **parola**, ve **parolayı onayla** metin kutuları, türü **parola** kullanıcının.
   
    b. **Kaydet**’e tıklayın.

>[!NOTE]
>Azure AD kullanıcı hesapları sağlamak için herhangi bir kişisel Mimecast portalı kullanıcı hesabı oluşturma araçları veya Mimecast kişisel portalı tarafından sağlanan API'leri kullanabilirsiniz.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Mimecast kişisel portalı kutucuğa tıkladığınızda, otomatik olarak Mimecast kişisel SSO'yu ayarlama portalında oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

