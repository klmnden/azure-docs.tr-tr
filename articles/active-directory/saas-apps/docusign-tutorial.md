---
title: 'Öğretici: DocuSign ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve DocuSign arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: a691288b-84c1-40fb-84bd-5b06878865f0
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/01/2019
ms.author: jeedes
ms.openlocfilehash: 607e5aded52375190878de6b48ffa4aa2ab49767
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67104081"
---
# <a name="tutorial-azure-active-directory-integration-with-docusign"></a>Öğretici: DocuSign ile Azure Active Directory Tümleştirme

Bu öğreticide, DocuSign, Azure Active Directory (Azure AD) ile tümleştirme konusunda bilgi edinin.
DocuSign'ı Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* DocuSign erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak DocuSign'ın (çoklu oturum açma) ile Azure AD hesaplarına oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

DocuSign ve Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa alabileceğiniz bir [ücretsiz hesap](https://azure.microsoft.com/free/)
* DocuSign çoklu oturum açma abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* DocuSign'ı destekleyen **SP** tarafından başlatılan

* DocuSign'ı destekleyen **zamanında** kullanıcı sağlama

## <a name="adding-docusign-from-the-gallery"></a>DocuSign galeri ekleme

Azure AD'ye DocuSign tümleştirmesini yapılandırmak için DocuSign galerideki yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden DocuSign eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **DocuSign**seçin **DocuSign** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde DocuSign](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma DocuSign adlı bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının DocuSign ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma DocuSign ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[DocuSign çoklu oturum açmayı yapılandırma](#configure-docusign-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[DocuSign test kullanıcısı oluşturma](#create-docusign-test-user)**  - kullanıcı Azure AD gösterimini bağlı DocuSign Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

DocuSign ve Azure AD çoklu oturum açmayı yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **DocuSign** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![DocuSign etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<subdomain>.docusign.com/organizations/<OrganizationID>/saml2/login/sp/<IDPID>`

    b. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<subdomain>.docusign.com/organizations/<OrganizationID>/saml2`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve daha sonra açıklanan tanımlayıcısı güncelleştirme **uç noktalarını görüntüle SAML 2.0** öğretici bölümünde.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. Üzerinde **DocuSign ' ayarlamak** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-docusign-single-sign-on"></a>DocuSign tek oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde oturum, **DocuSign Yönetici portalı** yönetici olarak.

2. Üst sayfanın sağ tıklayın, profili **logosu** ve ardından **Admin Git**.
  
    ![Çoklu oturum açmayı yapılandırma][51]

3. Etki alanı çözümleri sayfanızda tıklayarak **etki alanları**

    ![Çoklu oturum açmayı yapılandırma][50]

4. Altında **etki alanları** bölümünde **talep etki alanı**.

    ![Çoklu oturum açmayı yapılandırma][52]

5. Üzerinde **bir etki alanı talep** iletişim, **etki alanı adı** metin şirket etki alanınızı girin ve ardından **talep**. Etki alanını doğrulayın ve durumun etkin olduğundan emin olun.

    ![Çoklu oturum açmayı yapılandırma][53]

6. Etki alanı çözümleri sayfanızda tıklayın **kimlik sağlayıcıları**.
  
    ![Çoklu oturum açmayı yapılandırma][54]

7. Altında **kimlik sağlayıcıları** bölümünde **kimlik SAĞLAYICISI Ekle**. 

    ![Çoklu oturum açmayı yapılandırma][55]

8. Üzerinde **kimlik sağlayıcı ayarları** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırma][56]

    a. İçinde **adı** metin yapılandırmanız için benzersiz bir ad yazın. Boşluk kullanmayın.

    b. İçinde **kimlik sağlayıcısını veren textbox**, değerini yapıştırın **Azure AD tanımlayıcısı**, hangi Azure Portalı'ndan kopyaladığınız.

    c. İçinde **kimlik sağlayıcısı oturum açma URL'si** metin değerini yapıştırın **oturum açma URL'si**, hangi Azure Portalı'ndan kopyaladığınız.

    d. İçinde **kimlik sağlayıcısı oturum kapatma URL'si** metin değerini yapıştırın **oturum kapatma URL'si**, hangi Azure Portalı'ndan kopyaladığınız.

    e. Seçin **oturum AuthN isteği**.

    f. Olarak **AuthN gönderme isteği tarafından**seçin **POST**.

    g. Olarak **gönderme oturum kapatma isteği tarafından**seçin **alma**.

    h. İçinde **özel öznitelik eşlemesi** bölümünde, tıklayarak **yeni eşleme Ekle**.

    ![Çoklu oturum açmayı yapılandırma][62]

    i. Azure AD talep ile eşlemek istediğiniz alanı seçin. Bu örnekte, **emailaddress** talep değeriyle eşleşen **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress** . Azure ad için e-posta talebi ve ardından varsayılan talep adı olan **Kaydet**.

    ![Çoklu oturum açmayı yapılandırma][57]

    > [!NOTE]
    > Uygun kullanın **kullanıcı tanımlayıcısı** kullanıcının Azure AD'den DocuSign kullanıcı eşleme eşlemek için. Uygun alanı seçmek ve kuruluş ayarlarınıza göre uygun değeri girin.

    j. İçinde **kimlik sağlayıcısı sertifikaları** bölümünde **sertifika Ekle**ve ardından Azure AD Portalı'ndan yüklemiş ve tıklayın sertifikasını yükleme **Kaydet**.

    ![Çoklu oturum açmayı yapılandırma][58]

    k. İçinde **kimlik sağlayıcıları** bölümünde **eylemleri**ve ardından **uç noktaları**.

    ![Çoklu oturum açmayı yapılandırma][59]

    m. İçinde **uç noktalarını görüntüle SAML 2.0** bölümünde **DocuSign Yönetici portalı**, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırma][60]

    * Kopyalama **hizmet sağlayıcısı veren URL'si**, ardından yapıştırın **tanımlayıcı** metin kutusunda **temel SAML yapılandırma** bölümünde Azure portalında.

    * Kopyalama **hizmet sağlayıcısı oturum açma URL'si**, ardından yapıştırın **işareti bulunan URL'si** metin kutusunda **temel SAML yapılandırma** bölümünde Azure portalında.

    * Tıklayın **Kapat**

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

Bu bölümde, Azure çoklu oturum açma kullanmak için DocuSign erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **DocuSign**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **DocuSign**.

    ![Uygulamalar listesini DocuSign bağlantıdaki](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-docusign-test-user"></a>DocuSign test kullanıcısı oluşturma

Bu bölümde, Britta Simon adlı bir kullanıcı Docusign'da oluşturulur. DocuSign just-ın-time kullanıcı hazırlama, varsayılan olarak etkin olduğu destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Docusign'da bir kullanıcı zaten mevcut değilse yeni bir kimlik doğrulamasından sonra oluşturulur.

>[!Note]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [DocuSign Destek ekibine](https://support.docusign.com/).

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli DocuSign kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama DocuSign için'oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

<!--Image references-->

[50]: ./media/docusign-tutorial/tutorial_docusign_18.png
[51]: ./media/docusign-tutorial/tutorial_docusign_21.png
[52]: ./media/docusign-tutorial/tutorial_docusign_22.png
[53]: ./media/docusign-tutorial/tutorial_docusign_23.png
[54]: ./media/docusign-tutorial/tutorial_docusign_19.png
[55]: ./media/docusign-tutorial/tutorial_docusign_20.png
[56]: ./media/docusign-tutorial/tutorial_docusign_24.png
[57]: ./media/docusign-tutorial/tutorial_docusign_25.png
[58]: ./media/docusign-tutorial/tutorial_docusign_26.png
[59]: ./media/docusign-tutorial/tutorial_docusign_27.png
[60]: ./media/docusign-tutorial/tutorial_docusign_28.png
[61]: ./media/docusign-tutorial/tutorial_docusign_29.png
[62]: ./media/docusign-tutorial/tutorial_docusign_30.png
