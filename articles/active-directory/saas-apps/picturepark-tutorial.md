---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Picturepark | Microsoft Docs'
description: Azure Active Directory ve Picturepark arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 31c21cd4-9c00-4cad-9538-a13996dc872f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/18/2019
ms.author: jeedes
ms.openlocfilehash: 617c75024b45dab7ff2466b99bfb71c18cdd778a
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65904578"
---
# <a name="tutorial-azure-active-directory-integration-with-picturepark"></a>Öğretici: Picturepark ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Picturepark tümleştirme konusunda bilgi edinin.
Azure AD ile Picturepark tümleştirme ile aşağıdaki avantajları sağlar:

* Picturepark erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak (çoklu oturum açma) Picturepark için kendi Azure AD hesapları ile oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Picturepark yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa alabileceğiniz bir [ücretsiz hesap](https://azure.microsoft.com/free/)
* Abonelik Picturepark çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Picturepark destekler **SP** tarafından başlatılan

## <a name="adding-picturepark-from-the-gallery"></a>Galeriden Picturepark ekleme

Azure AD'de Picturepark tümleştirmesini yapılandırmak için Picturepark Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Picturepark eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Picturepark**seçin **Picturepark** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde Picturepark](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Picturepark adlı bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının Picturepark ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Picturepark ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Picturepark çoklu oturum açmayı yapılandırma](#configure-picturepark-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Picturepark test kullanıcısı oluşturma](#create-picturepark-test-user)**  - kullanıcı Azure AD gösterimini bağlı Picturepark Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile Picturepark yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Picturepark** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Picturepark etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<companyname>.picturepark.com`

    b. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna şu biçimi kullanarak bir URL yazın:

    |  |
    |--|
    | `https://<companyname>.current-picturepark.com`|
    | `https://<companyname>.picturepark.com`|
    | `https://<companyname>.next-picturepark.com`|
    | |

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL ve tanımlayıcıdır ile güncelleştirin. İlgili kişi [Picturepark istemci Destek ekibine](https://picturepark.com/about/contact/) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. İçinde **SAML imzalama sertifikası** bölümünde **Düzenle** açmak için düğmeyi **SAML imzalama sertifikası** iletişim.

    ![SAML imzalama sertifikası Düzenle](common/edit-certificate.png)

6. İçinde **SAML imzalama sertifikası** bölümünde, kopya **parmak izi** ve bilgisayarınıza kaydedin.

    ![Parmak izi değerini kopyalayın](common/copy-thumbprint.png)

7. Üzerinde **Picturepark kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın. İçin **oturum açma URL'si**, değeri ile şu biçimi kullanın: `https://login.microsoftonline.com/_my_directory_id_/wsfed`

    > [!Note]
    > _my_directory_id_ Azure AD aboneliğinin Kiracı kimliği.

    ![Yapılandırma URL'leri kopyalayın](./media/picturepark-tutorial/configurls.png)

    a. Azure AD Tanımlayıcısı

    b. Oturum Kapatma URL'si

### <a name="configure-picturepark-single-sign-on"></a>Picturepark tek oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde Picturepark şirket sitenize yönetici olarak oturum açın.

2. Üst araç çubuğunda tıklatın **Yönetimsel Araçlar**ve ardından **Yönetim Konsolu**.
   
    ![Yönetim Konsolu](./media/picturepark-tutorial/ic795062.png "Yönetim Konsolu")

3. Tıklayın **kimlik doğrulaması**ve ardından **kimlik sağlayıcıları**.
   
    ![Kimlik doğrulaması](./media/picturepark-tutorial/ic795063.png "kimlik doğrulaması")

4. İçinde **kimlik sağlayıcı Yapılandırması** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Kimlik sağlayıcısı Yapılandırması](./media/picturepark-tutorial/ic795064.png "kimlik sağlayıcı yapılandırması")
   
    a. **Ekle**'yi tıklatın.
  
    b. Yapılandırma için bir ad yazın.
   
    c. Seçin **varsayılan olarak ayarla**.
   
    d. İçinde **veren URI** metin değerini yapıştırın **oturum açma URL'si** , Azure Portalı'ndan kopyaladığınız.
   
    e. İçinde **güvenilir verenin parmak izi** metin değerini yapıştırın **parmak izi** kopyalanan **SAML imzalama sertifikası** bölümü. 

5. Tıklayın **JoinDefaultUsersGroup**.

6. Ayarlanacak **Emailaddress** özniteliğini **talep** metin kutusuna `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` tıklatıp **Kaydet**.

      ![Yapılandırma](./media/picturepark-tutorial/ic795065.png "yapılandırma")

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

Bu bölümde, Azure çoklu oturum açma kullanmak için Picturepark erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Picturepark**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Picturepark**.

    ![Uygulamalar listesinde Picturepark bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-picturepark-test-user"></a>Picturepark test kullanıcısı oluşturma

Azure AD Picturepark oturum açmasına etkinleştirmek için bunların Picturepark sağlanması gerekir. Picturepark söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açın, **Picturepark** Kiracı.

1. Üst araç çubuğunda tıklatın **Yönetimsel Araçlar**ve ardından **kullanıcılar**.
   
    ![Kullanıcılar](./media/picturepark-tutorial/ic795067.png "kullanıcılar")

1. İçinde **kullanıcılar genel bakış** sekmesinde **yeni**.
   
    ![Kullanıcı Yönetimi](./media/picturepark-tutorial/ic795068.png "kullanıcı yönetimi")

1. Üzerinde **Create User** iletişim kutusunda, geçerli bir Azure Active Directory istediğiniz kullanıcı sağlamak için aşağıdaki adımları gerçekleştirin:
   
    ![Kullanıcı oluşturma](./media/picturepark-tutorial/ic795069.png "kullanıcı oluşturma")
   
    a. İçinde **e-posta adresi** metin kutusuna **e-posta adresi** kullanıcının `BrittaSimon@contoso.com`.  
   
    b. İçinde **parola** ve **parolayı onayla** metin kutuları, türü **parola** BrittaSimon biri. 
   
    c. İçinde **ad** metin kutusuna **ad** kullanıcının **Britta**. 
   
    d. İçinde **Soyadı** metin kutusuna **Soyadı** kullanıcının **Simon**.
   
    e. İçinde **şirket** metin kutusuna **şirket adı** kullanıcının. 
   
    f. İçinde **Ülke** metin seçme **ülke/bölge** kullanıcının.
  
    g. İçinde **ZIP** metin kutusuna **posta kodu** şehir.
   
    h. İçinde **Şehir** metin kutusuna **şehir adı** kullanıcının.

    i. Seçin bir **dil**.
   
    j. **Oluştur**’a tıklayın.

>[!NOTE]
>Herhangi diğer Picturepark kullanıcı hesabı oluşturma araçları kullanabilir veya API Azure AD'ye kullanıcı hesapları sağlamak için Picturepark tarafından sağlanan.
>

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Picturepark kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama Picturepark için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

