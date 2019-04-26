---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile dmarcian | Microsoft Docs'
description: Azure Active Directory ve dmarcian arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: a04b9383-3a60-4d54-9412-123daaddff3b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/30/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: c24cbf8ad21c7dd5875a71532a5278e313774e66
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60280624"
---
# <a name="tutorial-azure-active-directory-integration-with-dmarcian"></a>Öğretici: Dmarcian ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile dmarcian tümleştirme konusunda bilgi edinin.
Azure AD ile dmarcian tümleştirme ile aşağıdaki avantajları sağlar:

* Dmarcian erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak (çoklu oturum açma) dmarcian için kendi Azure AD hesapları ile oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile dmarcian yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Abonelik dmarcian çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* dmarcian destekler **SP** ve **IDP** tarafından başlatılan

## <a name="adding-dmarcian-from-the-gallery"></a>Galeriden dmarcian ekleme

Azure AD'de dmarcian tümleştirmesini yapılandırmak için dmarcian Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden dmarcian eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **dmarcian**seçin **dmarcian** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![sonuç listesinde dmarcian](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma dmarcian adlı bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının dmarcian ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma dmarcian ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Çoklu oturum açma dmarcian yapılandırma](#configure-dmarcian-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Dmarcian test kullanıcısı oluşturma](#create-dmarcian-test-user)**  - Britta Simon kullanıcı Azure AD gösterimini bağlı dmarcian içinde bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile dmarcian yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **dmarcian** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** uygulamada yapılandırmak isterseniz, bölümü **IDP** başlatılan modu, aşağıdaki adımları gerçekleştirin:

    ![dmarcian etki alanı ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. İçinde **tanımlayıcı** metin kutusuna şu biçimi kullanarak bir URL yazın:

    | |
    | -- |
    | `https://us.dmarcian.com/sso/saml/<ACCOUNT_ID>/sp.xml` |
    | `https://dmarcian-eu.com/sso/saml/<ACCOUNT_ID>/sp.xml` |
    | `https://dmarcian-ap.com/sso/saml/<ACCOUNT_ID>/sp.xml` |

    b. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:

    | |
    |--|
    | `https://us.dmarcian.com/login/<ACCOUNT_ID>/handle/` |
    | `https://dmarcian-eu.com/login/<ACCOUNT_ID>/handle/` |
    | `https://dmarcian-ap.com/login/<ACCOUNT_ID>/handle/` |

5. Tıklayın **ek URL'lerini ayarlayın** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![dmarcian etki alanı ve URL'ler tek oturum açma bilgileri](common/metadata-upload-additional-signon.png)

    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:
    
    | |
    |--|
    | `https://us.dmarcian.com/login/<ACCOUNT_ID>` |
    | `https://dmarcian-eu.com/login/<ACCOUNT_ID>` |
    | `https://dmarciam-ap.com/login/<ACCOUNT_ID>` |
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma, öğreticinin ilerleyen bölümlerinde açıklanan URL'si ile güncelleştirir. 

6. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında **SAML imzalama sertifikası** bölümünde, kopyalamak için Kopyala düğmesine **uygulama Federasyon meta verileri URL'sini** ve üzerinde kaydedin, bilgisayar.

    ![Sertifika indirme bağlantısı](common/copy-metadataurl.png)

### <a name="configure-dmarcian-single-sign-on"></a>Çoklu oturum açma dmarcian yapılandırın

1. Farklı bir web tarayıcı penceresinde dmarcian için bir güvenlik yöneticisi olarak oturum açın.

2. Tıklayarak **profili** üzerinde sağ üst köşe ve gidin **tercihleri**.

    ![Tercihleri](./media/dmarcian-tutorial/tutorial_dmarcian_pref.png)

3. Ekranı aşağı kaydırın ve tıklayarak **çoklu oturum açma** bölümüne ve ardından tıklayarak **yapılandırma**.

    ![Tek](./media/dmarcian-tutorial/tutorial_dmarcian_sso.png)

4. Üzerinde **SAML çoklu oturum açma** sayfasında kümesi **durumu** olarak **etkin** ve aşağıdaki adımları gerçekleştirin:

    ![Kimlik doğrulaması](./media/dmarcian-tutorial/tutorial_dmarcian_auth.png)

    * Altında **kimlik sağlayıcınız dmarcian ekleme** bölümünde **kopyalama** kopyalamak için **onay belgesi tüketici hizmeti URL'si** örneğinizin yapıştırın  **Yanıt URL'si** metin kutusunda **temel SAML yapılandırma bölümü** Azure portalında.

    * Altında **kimlik sağlayıcınız dmarcian ekleme** bölümünde **kopyalama** kopyalamak için **varlık kimliği** örneğinizin yapıştırın **tanımlayıcı**metin kutusunda **temel SAML yapılandırma bölümü** Azure portalında.

    * Altında **kimlik doğrulamasını ayarlama** bölümünde **kimlik sağlayıcısı meta verileri** textbox Yapıştır **uygulama Federasyon meta verileri URL'sini**, hangi Azure Portalı'ndan kopyaladığınız.

    * Altında **kimlik doğrulamasını ayarlama** bölümünde **özniteliği deyimleri** metin URL'yi yapıştırın `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`

    * Altında **oturum açma URL'sini ayarlayın** bölümünde, kopya **oturum açma URL'si** örneğinizin yapıştırın **oturum açma URL'si** metin kutusunda **temelSAMLyapılandırmabölümü** Azure portalında.

        > [!Note]
        > Değiştirebileceğiniz **oturum açma URL'si** kuruluşunuz göre.

    * **Kaydet**’e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alanına **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alanına **brittasimon\@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma dmarcian erişim vererek kullanmak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **dmarcian**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **dmarcian**.

    ![Uygulamalar listesinde dmarcian bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim kutusunda, tıklayın **atama** düğmesi.

### <a name="create-dmarcian-test-user"></a>Dmarcian test kullanıcısı oluşturma

Dmarcian için oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunların dmarcian sağlanması gerekir. Dmarcian içinde sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Dmarcian için bir güvenlik yöneticisi olarak oturum açın.

2. Tıklayarak **profili** üst sağ köşedeki gidin **Kullanıcıları Yönet**.

    ![Kullanıcı](./media/dmarcian-tutorial/tutorial_dmarcian_user.png)

3. Sağ alt tarafında **SSO kullanıcıların** bölümünde, tıklayarak **yeni kullanıcı Ekle**.

    ![Kullanıcı Ekle](./media/dmarcian-tutorial/tutorial_dmarcian_addnewuser.png)

4. Üzerinde **yeni kullanıcı Ekle** açılan, aşağıdaki adımları gerçekleştirin:

    ![Yeni kullanıcı](./media/dmarcian-tutorial/tutorial_dmarcian_save.png)

    a. İçinde **yeni kullanıcı e-posta** metin gibi kullanıcının e-posta girin **brittasimon\@contoso.com**.

    b. Kullanıcısına yönetim hakları vermek isteyip istemediğinizi seçin **olun kullanıcı yönetici**.

    c. Tıklayın **kullanıcı ekleme**.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli dmarcian kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama dmarcian için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

