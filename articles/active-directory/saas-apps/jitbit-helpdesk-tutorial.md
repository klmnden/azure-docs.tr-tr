---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Jitbit Yardım Masası | Microsoft Docs'
description: Azure Active Directory ve Jitbit Yardım Masası arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 15ce27d4-0621-4103-8a34-e72c98d72ec3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/14/2019
ms.author: jeedes
ms.openlocfilehash: 29addcd62afd193af83196b2d942e9778ff3f031
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67099419"
---
# <a name="tutorial-azure-active-directory-integration-with-jitbit-helpdesk"></a>Öğretici: Jitbit Yardım Masası ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Jitbit Yardım Masası tümleştirme konusunda bilgi edinin.
Azure AD ile Jitbit Yardım Masası tümleştirme ile aşağıdaki avantajları sağlar:

* Jitbit Yardım Masası erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak (çoklu oturum açma) için Yardım Masası Jitbit oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Jitbit Yardım Masası ile yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Abonelik Jitbit Yardım Masası çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Jitbit Yardım Masası destekler **SP** tarafından başlatılan

## <a name="adding-jitbit-helpdesk-from-the-gallery"></a>Galeriden Jitbit Yardım Masası ekleme

Azure AD'de Jitbit Yardım Masası tümleştirmesini yapılandırmak için Jitbit Yardım Masası Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Jitbit Yardım Masası eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Jitbit Yardım Masası**seçin **Jitbit Yardım Masası** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde Jitbit Yardım Masası](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırmanız ve Jitbit Yardım Masası ile Azure AD çoklu oturum açmayı test adlı bir test kullanıcı tabanlı **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının Jitbit Yardım Masası ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Jitbit Yardım Masası ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Jitbit Yardım Masası çoklu oturum açmayı yapılandırma](#configure-jitbit-helpdesk-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Jitbit Yardım Masası test kullanıcısı oluşturma](#create-jitbit-helpdesk-test-user)**  - Jitbit kullanıcı Azure AD gösterimini bağlı olan Yardım Masası Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma Jitbit Yardım Masası ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Jitbit Yardım Masası** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Jitbit Yardım Masası etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:
    | |
    | ----------------------------------------|
    | `https://<hostname>/helpdesk/User/Login`|
    | `https://<tenant-name>.Jitbit.com`|
    | |
    
    > [!NOTE] 
    > Bu değer, gerçek değil. Bu değer, gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [Jitbit Yardım Masası istemci Destek ekibine](https://www.jitbit.com/support/) bu değeri alınamıyor.

    b. İçinde **tanımlayıcı (varlık kimliği)** metin kutusunda, aşağıdaki gibi bir URL yazın: `https://www.jitbit.com/web-helpdesk/`

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. İçinde **Jitbit Yardım Masası kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-jitbit-helpdesk-single-sign-on"></a>Jitbit Yardım Masası çoklu oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde Jitbit Yardım Masası şirketinizin sitesi için bir yönetici olarak oturum açın.

1. Üst araç çubuğunda tıklatın **Yönetim**.

    ![Yönetim](./media/jitbit-helpdesk-tutorial/ic777681.png "Yönetim")

1. Tıklayın **genel ayarlar**.

    ![Kullanıcıları, şirketler ve izinleri](./media/jitbit-helpdesk-tutorial/ic777680.png "kullanıcıları, şirketler ve izinleri")

1. İçinde **kimlik doğrulama ayarları** yapılandırma bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Kimlik doğrulama ayarları](./media/jitbit-helpdesk-tutorial/ic777683.png "kimlik doğrulama ayarları")

    a. Seçin **SAML 2.0 tek etkinleştirme oturum**, ile çoklu oturum açma (SSO), kullanarak oturum açmanız **OneLogin**.

    b. İçinde **uç nokta URL'si** metin değerini yapıştırın **oturum açma URL'si** , Azure Portalı'ndan kopyaladığınız.

    c. Açık, **base-64** kodlanmış sertifika Not Defteri'nde, içeriğini, panoya kopyalayın ve ardından ona yapıştırın **X.509 sertifikası** metin kutusu

    d. Tıklayın **değişiklikleri kaydetmek**.

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

Bu bölümde, Jitbit Yardım Masası için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Jitbit Yardım Masası**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Jitbit Yardım Masası**.

    ![Uygulamalar listesinde Jitbit Yardım Masası bağlantısı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-jitbit-helpdesk-test-user"></a>Jitbit Yardım Masası test kullanıcısı oluşturma

Yardım masasına Jitbit oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunlar Jitbit Yardım Masası sağlanması gerekir. Jitbit Yardım Masası söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açın, **Jitbit Yardım Masası** Kiracı.

1. Üstteki menüden **Yönetim**.

    ![Yönetim](./media/jitbit-helpdesk-tutorial/ic777681.png "Yönetim")

1. Tıklayın **kullanıcıları, şirketler ve izinleri**.

    ![Kullanıcıları, şirketler ve izinleri](./media/jitbit-helpdesk-tutorial/ic777682.png "kullanıcıları, şirketler ve izinleri")

1. **Kullanıcı ekle**'ye tıklayın.

    ![Kullanıcı Ekle](./media/jitbit-helpdesk-tutorial/ic777685.png "Kullanıcı Ekle")

1. Oluşturma bölümünde şu şekilde sağlamak için Azure AD hesap verilerini yazın:

    ![Oluşturma](./media/jitbit-helpdesk-tutorial/ic777686.png "oluşturma")

   a. İçinde **kullanıcıadı** metin kutusu, kullanıcının kullanıcı adı türü ister **BrittaSimon**.

   b. İçinde **e-posta** metin kutusu, kullanıcının e-posta ister **BrittaSimon@contoso.com** .

   c. İçinde **ad** metin adı gibi kullanıcı türü **Britta**.

   d. İçinde **Soyadı** metin türü Soyadı gibi kullanıcının **Simon**.

   e. **Oluştur**’a tıklayın.

> [!NOTE]
> Azure AD kullanıcı hesapları sağlamak için herhangi bir Jitbit Yardım Masası kullanıcı hesabı oluşturma araçları veya Jitbit Yardım Masası tarafından sağlanan API'leri kullanabilirsiniz.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Jitbit Yardım Masası kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama Jitbit Yardım Masası için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
