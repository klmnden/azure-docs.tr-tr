---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Voyance | Microsoft Docs'
description: Azure Active Directory ve Voyance arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 539dc1f9-64c9-4dce-b259-2b0b49dcf857
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/07/2019
ms.author: jeedes
ms.openlocfilehash: 3c286c004885cdc2cc83899bd06af19176c5581b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67087584"
---
# <a name="tutorial-azure-active-directory-integration-with-voyance"></a>Öğretici: Voyance ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Voyance tümleştirme konusunda bilgi edinin.
Azure AD ile Voyance tümleştirme ile aşağıdaki avantajları sağlar:

* Voyance erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak (çoklu oturum açma) Voyance için kendi Azure AD hesapları ile oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Voyance yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa alabileceğiniz bir [ücretsiz hesap](https://azure.microsoft.com/free/)
* Abonelik Voyance çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Voyance destekler **SP** ve **IDP** tarafından başlatılan

* Voyance destekler **zamanında** kullanıcı sağlama

## <a name="adding-voyance-from-the-gallery"></a>Galeriden Voyance ekleme

Azure AD'de Voyance tümleştirmesini yapılandırmak için Voyance Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Voyance eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Voyance**seçin **Voyance** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde Voyance](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Voyance adlı bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının Voyance ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Voyance ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Voyance çoklu oturum açmayı yapılandırma](#configure-voyance-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Voyance test kullanıcısı oluşturma](#create-voyance-test-user)**  - kullanıcı Azure AD gösterimini bağlı Voyance Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile Voyance yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Voyance** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** uygulamada yapılandırmak isterseniz, bölümü **IDP** başlatılan modu, aşağıdaki adımları gerçekleştirin:

    ![Voyance etki alanı ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. İçinde **tanımlayıcı** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<companyname>.nyansa.com`

    b. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<companyname>.nyansa.com/saml/create/`

5. Tıklayın **ek URL'lerini ayarlayın** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![Voyance etki alanı ve URL'ler tek oturum açma bilgileri](common/metadata-upload-additional-signon.png)

    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:  `https://<companyname>.nyansa.com/`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. İlgili kişi [Voyance istemci Destek ekibine](mailto:support@nyansa.com) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

6. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

7. Üzerinde **Voyance kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-voyance-single-sign-on"></a>Voyance tek oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde Voyance kiracınıza yönetici olarak oturum.

2. Gezinti çubuğunun sağ üst köşesindeki gidin ve tıklayarak **profili**.
    
    ![Çoklu oturum açma uygulama yan Acme University üzerinde yapılandırın.](./media/voyance-tutorial/tutorial_voyance_001.png) 

3. Tıklayın **yönetici ayarları**.

    ![Çoklu oturum açma uygulama yan yönetici ayarlarını yapılandırın.](./media/voyance-tutorial/tutorial_voyance_002.png)

4. Tıklayın **kullanıcı erişimini** sekmesi.

    ![Çoklu oturum açma uygulama yan kullanıcı erişimini yapılandırma](./media/voyance-tutorial/tutorial_voyance_003.png)

5. Tıklayın **SSO devre dışı** Azure AD'yi SAML 2.0 kullanarak bir Idp'si yapılandırması için düğme.

    ![Yapılandırma çoklu oturum açma uygulama yan SSO devre dışı düğmesi](./media/voyance-tutorial/tutorial_voyance_004.png)

6. Git **SAML v2** bölümünde ve aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açma SAML uygulama tarafında yapılandırma v2](./media/voyance-tutorial/tutorial-voyance-005.png)
    
    a. **Etkin**’i seçin.
    
    b. Yapıştırma **oturum açma URL'si**, hangi Azure portaldan kopyaladığınız **Idp'nin oturum açma URL'si** metin.

    c. İndirilen, Base64 olarak kodlanmış sertifika Not Defteri'nde açın, içeriğini, panoya kopyalayın ve ardından ona yapıştırın **IDP Cert** metin.
    
    d. **Kaydet**’e tıklayın.

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

Bu bölümde, Azure çoklu oturum açma kullanmak için Voyance erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Voyance**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Voyance**.

    ![Uygulamalar listesinde Voyance bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-voyance-test-user"></a>Voyance test kullanıcısı oluşturma

Bu bölümde, Britta Simon adlı bir kullanıcı Voyance oluşturulur. Voyance just-ın-time kullanıcı hazırlama, varsayılan olarak etkin olduğu destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Bir kullanıcı Voyance içinde zaten mevcut değilse yeni bir kimlik doğrulamasından sonra oluşturulur.

>[!NOTE]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, iletişime geçmeniz [Voyance Destek ekibine](maiLto:support@nyansa.com).

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Voyance kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama Voyance için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

