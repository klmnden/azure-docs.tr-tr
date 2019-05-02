---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle Zoho bir | Microsoft Docs'
description: Azure Active Directory ve Zoho bir arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: bbc3038c-0d8b-45dd-9645-368bd3d01a0f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/16/2019
ms.author: jeedes
ms.openlocfilehash: c731919baf3acc8cedfb31c088f9a0a12791251c
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64717998"
---
# <a name="tutorial-azure-active-directory-integration-with-zoho-one"></a>Öğretici: Azure Active Directory tümleştirmesiyle Zoho bir

Bu öğreticide, Zoho bir Azure Active Directory (Azure AD) ile tümleştirme konusunda bilgi edinin.
Zoho bir Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* Kimlerin erişebildiğini Zoho bir Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak Zoho One (çoklu oturum açma) oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Zoho bir ile yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa alabileceğiniz bir [ücretsiz hesap](https://azure.microsoft.com/free/)
* Abonelik Zoho bir çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Zoho bir destekler **SP** ve **IDP** tarafından başlatılan

## <a name="adding-zoho-one-from-the-gallery"></a>Zoho bir galeri ekleme

Azure AD'de, Zoho bir tümleştirmesini yapılandırmak için Zoho bir Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden bir Zoho eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Zoho bir**seçin **Zoho bir** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde bir Zoho](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Zoho adlı bir test kullanıcı tabanlı bir test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ile ilgili kullanıcı Zoho bir arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma ile Zoho bir test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Zoho bir çoklu oturum açmayı yapılandırma](#configure-zoho-one-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Zoho bir test kullanıcısı oluşturma](#create-zoho-one-test-user)**  - Zoho kullanıcı Azure AD gösterimini bağlantılı bir Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma Zoho bir ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Zoho bir** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** uygulamada yapılandırmak isterseniz, bölümü **IDP** başlatılan modu, aşağıdaki adımları gerçekleştirin:

    ![Zoho bir etki alanı ve URL'ler tek oturum açma bilgileri](common/idp-relay.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL yazın: `one.zoho.com`

    b. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://accounts.zoho.com/samlresponse/<saml-identifier>`

    > [!NOTE]
    > Önceki **yanıt URL'si** değeri gerçek değil. Erişmenizi sağlayacak `<saml-identifier>` #step4 değerden **yapılandırma Zoho bir çoklu oturum açma** bölümünde, öğreticinin ilerleyen bölümlerinde açıklanmıştır.

    c. Tıklayın **ek URL'lerini ayarlayın**.

    d. İçinde **geçiş durumu** metin kutusuna bir URL yazın: `https://one.zoho.com`

5. Uygulamada yapılandırmak istiyorsanız **SP** modunda başlatılan aşağıdaki adımı uygulayın:


    ![Zoho bir etki alanı ve URL'ler tek oturum açma bilgileri](common/both-signonurl.png)

    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:  `https://accounts.zoho.com/samlauthrequest/<domain_name>?serviceurl=https://one.zoho.com` 

    > [!NOTE] 
    > Önceki **oturum açma URL'si** değeri gerçek değil. Değer, gerçek oturum açma URL'SİNDEN ile güncelleştirir **yapılandırma Zoho bir çoklu oturum açma** bölümünde, öğreticinin ilerleyen bölümlerinde açıklanmıştır. 

6. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

7. Üzerinde **Zoho bir kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-zoho-one-single-sign-on"></a>Zoho bir çoklu oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde Zoho bir şirketinizin sitesi için bir yönetici olarak oturum açın.

2. Üzerinde **kuruluş** sekmesine tıklatın **Kurulum** altında **SAML kimlik doğrulaması**.

    ![Zoho bir kuruluş](./media/zohoone-tutorial/tutorial_zohoone_setup.png)

3. Açılan sayfada, aşağıdaki adımları gerçekleştirin:

    ![Zoho bir imza](./media/zohoone-tutorial/tutorial_zohoone_save.png)

    a. İçinde **oturum açma URL'si** metin değerini yapıştırın **oturum açma URL'si**, hangi Azure Portalı'ndan kopyaladığınız.

    b. İçinde **oturum kapatma URL'si** metin değerini yapıştırın **oturum kapatma URL'si**, hangi Azure Portalı'ndan kopyaladığınız.

    c. Tıklayın **Gözat** yüklenecek **sertifika (Base64)** Azure portalından indirilen.

    d. **Kaydet**’e tıklayın.

4. SAML kimlik doğrulaması kurulumu kaydettikten sonra kopyalama **SAML kimlik** değeri ve onunla ekleme **yanıt URL'si** yerine `<saml-identifier>`gibi `https://accounts.zoho.com/samlresponse/one.zoho.com` oluşturulan değeri olarak yapıştırın. **Yanıt URL'si** metin kutusunun altında **temel SAML yapılandırma** bölümü.

    ![Zoho bir saml](./media/zohoone-tutorial/tutorial_zohoone_samlidenti.png)

5. Git **etki alanları** sekmesine ve ardından **etki alanı Ekle**.

    ![Zoho bir etki alanı](./media/zohoone-tutorial/tutorial_zohoone_domain.png)

6. Üzerinde **etki alanı Ekle** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Zoho bir etki alanı Ekle](./media/zohoone-tutorial/tutorial_zohoone_adddomain.png)

    a. İçinde **etki alanı adı** metin türü etki alanı contoso.com gibi.

    b. **Ekle**'ye tıklayın.

    >[!Note]
    >Etki alanı izleme ekledikten sonra [bunlar](https://www.zoho.com/one/help/admin-guide/domain-verification.html) adımları etki alanınızı doğrulayın. Etki alanı doğrulandıktan sonra etki alanı adınızı kullanmak **oturum açma URL'si** içinde **temel SAML yapılandırma** bölümü Azure Portalı'nda.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü `brittasimon@yourcompanydomain.extension`. Örneğin, BrittaSimon@contoso.com

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Zoho bir erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Zoho bir**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Zoho bir**.

    ![Uygulamalar listesini Zoho bir bağlantıya](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-zoho-one-test-user"></a>Zoho bir test kullanıcısı oluşturma

One Zoho oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunlar Zoho bir sağlanması gerekir. One Zoho, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. İçin Zoho tek bir güvenlik yöneticisi olarak oturum açın.

2. Üzerinde **kullanıcılar** sekmesine tıklatın üzerinde **kullanıcı logosu**.

    ![Zoho bir kullanıcı](./media/zohoone-tutorial/tutorial_zohoone_users.png)

3. Üzerinde **Kullanıcı Ekle** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Zoho bir kullanıcı ekleyin](./media/zohoone-tutorial/tutorial_zohoone_adduser.png)
    
    a. İçinde **adı** metin kutusunda, gibi kullanıcı adını girin **Britta simon**.
    
    b. İçinde **e-posta adresi** metin kutusuna, kullanıcının gibi e-posta girin brittasimon@contoso.com.

    >[!Note]
    >Doğrulanmış etki alanınızın etki alanı listeden seçin.

    c. **Ekle**'ye tıklayın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Zoho bir kutucuğa tıkladığınızda, size otomatik olarak Zoho SSO'yu ayarlama bir oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

