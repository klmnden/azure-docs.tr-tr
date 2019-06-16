---
title: 'Öğretici: Azure Active Directory Tümleştirmesi Mimecast Yönetici Konsolu ile | Microsoft Docs'
description: Azure Active Directory ve Mimecast Yönetici Konsolu arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 81c50614-f49b-4bbc-97d5-3cf77154305f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/27/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0bbbd73d1856ba5d3dc19873c56fce622b272939
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67097334"
---
# <a name="tutorial-azure-active-directory-integration-with-mimecast-admin-console"></a>Öğretici: Mimecast Yönetim Konsolu ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Mimecast Yönetici Konsolu tümleştirme konusunda bilgi edinin.
Mimecast Yönetim Konsolu Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* Mimecast yönetici Konsolu'na erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak (çoklu oturum açma) Mimecast yönetim konsoluna oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Mimecast Yönetici Konsolu ile yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Abonelik Mimecast Yönetici Konsolu çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Yönetici Konsolu Mimecast destekler **SP** tarafından başlatılan

## <a name="adding-mimecast-admin-console-from-the-gallery"></a>Galeriden Mimecast Yönetici Konsolu'nu ekleme

Azure AD Yönetici Konsolu Mimecast tümleştirilmesi yapılandırmak için Mimecast Yönetici Konsolu Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Mimecast Yönetici Konsolu eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Mimecast Yönetici Konsolu**seçin **Mimecast Yönetici Konsolu** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde Mimecast Yönetici Konsolu](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Mimecast Yönetim Konsolu olarak adlandırılan bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının Mimecast Yönetici konsolunda ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Mimecast Yönetici Konsolu ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Mimecast Yönetici Konsolu çoklu oturum açmayı yapılandırma](#configure-mimecast-admin-console-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Yönetici Konsolu Mimecast test kullanıcısı oluşturma](#create-mimecast-admin-console-test-user)**  - Mimecast Yönetici konsolunda, kullanıcının Azure AD gösterimini bağlı Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma Mimecast Yönetici Konsolu ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Mimecast Yönetici Konsolu** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Mimecast Yönetici Konsolu etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-signonurl.png)

    İçinde **oturum açma URL'si** metin kutusuna URL'yi yazın:
    
    | |
    | -- |
    | `https://webmail-uk.mimecast.com`|
    | `https://webmail-us.mimecast.com`|

    > [!NOTE] 
    > Oturum açma URL'si belirli bölgedir.

4. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. Üzerinde **Mimecast Yönetici Konsolu ayarlama** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-mimecast-admin-console-single-sign-on"></a>Mimecast Yönetici Konsolu çoklu oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde Mimecast yönetici konsolunuza yönetici olarak oturum.

2. Git **Hizmetleri \> uygulama**.

    ![Hizmetleri](./media/mimecast-admin-console-tutorial/ic794998.png "Hizmetleri")

3. Tıklayın **kimlik doğrulaması profilleri**.

    ![Kimlik doğrulaması profilleri](./media/mimecast-admin-console-tutorial/ic794999.png "kimlik doğrulaması profilleri")
    
4. Tıklayın **yeni kimlik doğrulama profili**.

    ![Yeni kimlik doğrulama profilleri](./media/mimecast-admin-console-tutorial/ic795000.png "yeni kimlik doğrulama profilleri")

5. İçinde **kimlik doğrulama profili** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Kimlik doğrulama profili](./media/mimecast-admin-console-tutorial/ic795015.png "kimlik doğrulama profili")
    
    a. İçinde **açıklama** metin yapılandırmanız için bir ad yazın.
    
    b. Seçin **SAML kimlik doğrulaması Mimecast Yönetici Konsolu için zorunlu**.
    
    c. Olarak **sağlayıcısı**seçin **Azure Active Directory**.
    
    d. Yapıştırma **Azure Ad tanımlayıcısı**, hangi Azure portaldan kopyaladığınız **veren URL'si** metin.
    
    e. Yapıştırma **oturum açma URL'si**, hangi Azure portaldan kopyaladığınız **oturum açma URL'si** metin.

    f. Yapıştırma **oturum açma URL'si**, hangi Azure portaldan kopyaladığınız **oturum kapatma URL'si** metin.
    
    >[!NOTE]
    >Oturum açma URL'si ve oturum kapatma URL'si değerleri Mimecast Yönetici Konsolu için aynıdır.
    
    g. Açık base-64 sertifikanızı indirilen Not Defteri'nde, Azure portalından ilk satırı Kaldır (" *--* ") ve son satırı (" *--* "), kalan içeriği içine kopyalayın, Pano, kendisine yapıştırın **kimlik sağlayıcısı sertifikası (meta veriler)** metin.
    
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

Bu bölümde, Azure çoklu oturum açma Mimecast yönetim konsoluna erişim vererek kullanmak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Mimecast Yönetici Konsolu**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde yazın ve **Mimecast Yönetici Konsolu**.

    ![Uygulamalar listesinde Mimecast Yönetici konsolunda bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-mimecast-admin-console-test-user"></a>Yönetici Konsolu Mimecast test kullanıcısı oluşturma

Azure AD kullanıcılarının Mimecast Yönetici Konsolu'nda sizin oturum etkinleştirmek için bunlar Mimecast yönetici konsoluna sağlanması gerekir. Mimecast yönetim söz konusu olduğunda, sağlama elle bir görevin konsoludur.

* Kullanıcılar oluşturabilmeniz için önce bir etki alanı kayıt olmanız gerekir.

**Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açın, **Mimecast Yönetici Konsolu** yönetici olarak.

2. Git **dizinleri \> iç**.
   
    ![Dizinleri](./media/mimecast-admin-console-tutorial/ic795003.png "dizinleri")

3. Tıklayın **yeni etki alanı kayıt**.
   
    ![Yeni etki alanı kayıt](./media/mimecast-admin-console-tutorial/ic795004.png "kaydetme yeni etki alanı")

4. Yeni etki alanınız oluşturulduktan sonra tıklayın **yeni adresi**.
   
    ![Yeni adresi](./media/mimecast-admin-console-tutorial/ic795005.png "yeni adresi")

5. Yeni adresi iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
    ![Kaydet](./media/mimecast-admin-console-tutorial/ic795006.png "Kaydet")
   
    a. Tür **e-posta adresi**, **genel adı**, **parola**, ve **parolayı onayla** öznitelikleri geçerli bir Azure ad hesabı istiyorsunuz ilgili metin kutularına sağlayın.

    b. **Kaydet**’e tıklayın.

>[!NOTE]
>Azure AD kullanıcı hesapları sağlamak için herhangi bir Mimecast Yönetim Konsolu kullanıcı hesabı oluşturma araçları veya Mimecast Yönetim Konsolu tarafından sağlanan API'leri kullanabilirsiniz. 

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Mimecast Yönetici Konsolu kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama Mimecast yönetici Konsolu'na oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

