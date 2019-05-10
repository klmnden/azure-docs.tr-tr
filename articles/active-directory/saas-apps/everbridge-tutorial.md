---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Everbridge | Microsoft Docs'
description: Azure Active Directory ve Everbridge arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 58d7cd22-98c0-4606-9ce5-8bdb22ee8b3e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/18/2019
ms.author: jeedes
ms.openlocfilehash: f8dd11e7fb0b9fda0e0f1c7d3f794f6bfd766cdf
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65231458"
---
# <a name="tutorial-azure-active-directory-integration-with-everbridge"></a>Öğretici: Everbridge ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Everbridge tümleştirme konusunda bilgi edinin.
Everbridge Azure AD ile tümleştirdiğinizde, şunları yapabilirsiniz:

* Everbridge erişimi, Azure AD'de denetler.
* Everbridge için Azure AD hesaplarına otomatik olarak oturum açmasına imkan tanıyın. Bu erişim denetimi, çoklu oturum açma (SSO) denir.
* Azure portalını kullanarak tek bir merkezi konumda hesaplarınızı yönetin.
Azure AD ile bir hizmet (SaaS) uygulamasını tümleştirme olarak yazılım hakkında daha fazla bilgi için bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Everbridge yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa alabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free/).
* Çoklu oturum açma kullanan Everbridge abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Everbridge IDP tarafından başlatılan SSO'yu destekler.

## <a name="add-everbridge-from-the-azure-marketplace"></a>Azure Market'ten Everbridge Ekle

Azure AD'ye Everbridge tümleştirmesini yapılandırmak için Azure Marketi'nden Everbridge yönetilen SaaS uygulamaları listesine ekleyin.

Azure Market'ten Everbridge eklemek için aşağıdaki adımları izleyin.

1. İçinde [Azure portalında](https://portal.azure.com), sol gezinti bölmesinde seçin **Azure Active Directory**.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Git **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için seçin **yeni uygulama** iletişim kutusunun üst.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Everbridge**. Seçin **Everbridge** sonuç paneli ve select **Ekle**.

     ![Sonuç listesinde Everbridge](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açmayı test kullanıcıya Britta Simon bağlı Everbridge sınayın.
Tek iş, Everbridge içinde bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişki kurmak için oturum açma için.

Yapılandırma ve Azure AD çoklu oturum açma Everbridge ile test etmek için aşağıdaki yapı taşlarını tamamlayın:

- [Azure AD çoklu oturum açmayı yapılandırma](#configure-azure-ad-single-sign-on) kullanıcılarınız bu özelliği kullanmak etkinleştirmek için.
- [Everbridge manager portal çoklu oturum açma Everbridge yapılandırma](#configure-everbridge-as-everbridge-manager-portal-single-sign-on) üzerinde uygulama tarafından çoklu oturum açma ayarları yapılandırmak için.
- [Everbridge üye portal çoklu oturum açma Everbridge yapılandırma](#configure-everbridge-as-everbridge-member-portal-single-sign-on) üzerinde uygulama tarafından çoklu oturum açma ayarları yapılandırmak için.
- [Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user) Azure AD çoklu oturum açma Britta Simon ile test etmek için.
- [Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user) Britta Simon, Azure AD çoklu oturum açma kullanmak üzere etkinleştirmek için.
- [Bir Everbridge test kullanıcısı oluşturma](#create-an-everbridge-test-user) bir karşılığı Britta simon'un kullanıcı Azure AD gösterimini bağlı Everbridge sağlamak için.
- [Çoklu oturum açmayı test](#test-single-sign-on) yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile Everbridge yapılandırmak için aşağıdaki adımları izleyin.

1. İçinde [Azure portalında](https://portal.azure.com/), **Everbridge** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. İçinde **tek bir oturum açma yönteminizi seçmeniz** Seç iletişim kutusunda **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için **temel SAML yapılandırma** iletişim kutusu.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

    >[!NOTE]
    >Uygulamayı manager portalı olarak ya da yapılandırma *veya* hem Azure portalında hem de Everbridge portalı üye portalı.

4. Yapılandırmak için **Everbridge** olarak uygulama **Everbridge manager portalı**, **temel SAML yapılandırma** bölümünde, aşağıdaki adımları izleyin:

    ![Oturum açma bilgileri çoklu Everbridge etki alanı ve URL'ler](common/idp-intiated.png)

    a. İçinde **tanımlayıcı** kutusuna, modele bir URL girin `https://sso.everbridge.net/<API_Name>`

    b. İçinde **yanıt URL'si** kutusuna, modele bir URL girin `https://manager.everbridge.net/saml/SSO/<API_Name>/alias/defaultAlias`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı ve yanıt URL'si değerleri güncelleştirin. Bu değerleri almak için iletişime geçin [Everbridge Destek ekibine](mailto:support@everbridge.com). Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. Yapılandırmak için **Everbridge** olarak uygulama **Everbridge üye portalı**, **temel SAML yapılandırma** bölümünde, aşağıdaki adımları izleyin:

  * IDP tarafından başlatılan modunda uygulama yapılandırmak istiyorsanız, şu adımları izleyin:

     ![Mod IDP tarafından başlatılan oturum açma bilgileri çoklu Everbridge etki alanı ve URL'ler](common/idp-intiated.png)

    a. İçinde **tanımlayıcı** kutusuna, modele bir URL girin `https://sso.everbridge.net/<API_Name>/<Organization_ID>`

    b. İçinde **yanıt URL'si** kutusuna, modele bir URL girin `https://member.everbridge.net/saml/SSO/<API_Name>/<Organization_ID>/alias/defaultAlias`

   * SP tarafından başlatılan modunda uygulama yapılandırmak isteyip istemediğinizi seçin **ek URL'lerini ayarlayın** ve bu adımı izleyin:

     ![Mod SP tarafından başlatılan oturum açma bilgileri çoklu Everbridge etki alanı ve URL'ler](common/both-signonurl.png)

     a. İçinde **oturum açma URL'si** kutusuna, modele bir URL girin `https://member.everbridge.net/saml/login/<API_Name>/<Organization_ID>/alias/defaultAlias?disco=true`

     > [!NOTE]
     > Bu değerler gerçek değildir. Yanıt URL'si gerçek tanımlayıcısı bu değerleri güncelleştirin ve URL değerleri üzerinde oturum açın. Bu değerleri almak için iletişime geçin [Everbridge Destek ekibine](mailto:support@everbridge.com). Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

6. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünden **indirme** indirmek için **Federasyon meta veri XML** . Dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

7. İçinde **Everbridge kümesi** bölümünde, gereksinimleriniz için gereksinim duyduğunuz URL'leri kopyalayın:

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    - Oturum Açma URL'si:
    - Azure AD Tanımlayıcısı
    - Oturum Kapatma URL'si

### <a name="configure-everbridge-as-everbridge-manager-portal-single-sign-on"></a>Everbridge Everbridge manager portal çoklu oturum açma yapılandırma

SSO yapılandırma **Everbridge** olarak bir **Everbridge manager portalı** uygulama, şu adımları izleyin.
 
1. Farklı bir web tarayıcı penceresinde Everbridge için yönetici olarak oturum açın.

1. Üst menüde **ayarları** sekmesi. Altında **güvenlik**seçin **çoklu oturum açma**.
   
     ![Çoklu oturum açmayı yapılandırma](./media/everbridge-tutorial/tutorial_everbridge_002.png)
   
     a. İçinde **adı** kutusunda, kimlik sağlayıcısının adını girin. Şirketinizin adını buna bir örnektir.
   
     b. İçinde **API adı** kutusunu, API'ye adını girin.
   
     c. Seçin **Dosya Seç** Azure portalından indirdiğiniz meta veri dosyasını karşıya yükleyin.
   
     d. İçin **SAML kimlik konumu**seçin **kimliğidir konu deyiminin NameIdentifier öğesinde**.
   
     e. İçinde **kimlik sağlayıcısı oturum açma URL'si** kutusu, yapıştırma **oturum açma URL'si** Azure portaldan kopyaladığınız değeri.
   
     f. İçin **hizmet sağlayıcısı tarafından başlatılan istek bağlama**seçin **HTTP yeniden yönlendirme**.

     g. **Kaydet**’i seçin.

### <a name="configure-everbridge-as-everbridge-member-portal-single-sign-on"></a>Everbridge Everbridge üye portal çoklu oturum açma yapılandırma

Çoklu oturum açmayı yapılandırma **Everbridge** olarak bir **Everbridge üye portalı**, indirilen Gönder **Federasyon meta verileri XML** için [Everbridge Destek](mailto:support@everbridge.com). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma 

Azure portalında Britta Simon test kullanıcısı oluşturmak için aşağıdaki adımları izleyin.

1. Azure portalında, sol bölmede seçin **Azure Active Directory** > **kullanıcılar** > **tüm kullanıcılar**.

    ![Kullanıcılar ve tüm kullanıcıların bağlantılar](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları izleyin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** kutusuna `brittasimon@yourcompanydomain.extension`. BrittaSimon@contoso.com bunun bir örneğidir.

    c. Seçin **Göster parola** onay kutusu. Görüntülenen değer azaltma **parola** kutusu.

    d. **Oluştur**’u seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Britta Simon, Everbridge için erişim izni verdiğinizde Azure çoklu oturum açmayı kullanmak etkinleştirin.

1. Azure portalında **kurumsal uygulamalar** > **tüm uygulamaları** >**Everbridge**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Everbridge**.

    ![Uygulamalar listesinde Everbridge bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcılar ve gruplar bağlantı](common/users-groups-blade.png)

4. Seçin **Kullanıcı Ekle**. İçinde **atama Ekle** iletişim kutusunda **kullanıcılar ve gruplar**.

    ![Atama iletişim kutusu Ekle](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde. Seçin **seçin** ekranın alt kısmındaki.

6. SAML onaylama işlemi herhangi bir rolü değer de beklediğiniz **rolü Seç** iletişim kutusunda, listeden bir kullanıcı için uygun rolü seçin. Seçin **seçin** ekranın alt kısmındaki.

7. İçinde **atama Ekle** iletişim kutusunda **atama**.

### <a name="create-an-everbridge-test-user"></a>Bir Everbridge test kullanıcısı oluşturma

Bu bölümde, Britta Simon Everbridge içinde test kullanıcısı oluşturun. Everbridge platform kullanıcılar eklemek için çalışmak [Everbridge Destek ekibine](mailto:support@everbridge.com). Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce Everbridge etkinleştirildi. 

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Azure AD çoklu oturum açma yapılandırmanızı erişim panelini kullanarak test edin.

Erişim Paneli'nde Everbridge kutucuğu seçtiğinizde, otomatik olarak SSO'yu ayarlama Everbridge hesabıyla oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)
- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)
- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

