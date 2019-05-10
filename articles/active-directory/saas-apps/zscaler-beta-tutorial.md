---
title: 'Öğretici: Zscaler Beta ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Zscaler Beta arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 56b846ae-a1e7-45ae-a79d-992a87f075ba
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/24/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: f31361dc3d7e24092677f1a78b2c405ae84578ed
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65230064"
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-beta"></a>Öğretici: Zscaler Beta ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Zscaler Beta tümleştirme konusunda bilgi edinin.
Zscaler Beta Azure AD ile tümleştirdiğinizde, şunları yapabilirsiniz:

* Zscaler Beta erişimi, Azure AD'de denetler.
* Zscaler Beta için Azure AD hesaplarına otomatik olarak oturum açmasına imkan tanıyın. Bu erişim denetimi, çoklu oturum açma (SSO) denir.
* Azure portalını kullanarak tek bir merkezi konumda hesaplarınızı yönetin.

Azure AD ile bir hizmet (SaaS) uygulamasını tümleştirme olarak yazılım hakkında daha fazla bilgi için bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Zscaler Beta yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa alabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free/).
* Çoklu oturum açma kullanan Zscaler Beta abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Zscaler Beta SP tarafından başlatılan SSO'yu destekler.
* Zscaler Beta, just-ın-time kullanıcı sağlamayı destekler.

## <a name="add-zscaler-beta-from-the-azure-marketplace"></a>Azure Market'ten Zscaler Beta Ekle

Azure AD'ye Zscaler Beta tümleştirmesini yapılandırmak için Zscaler Beta Azure Market'te yönetilen SaaS uygulamaları listenize ekleyin.

Azure Market'ten Zscaler Beta eklemek için aşağıdaki adımları izleyin.

1. İçinde [Azure portalında](https://portal.azure.com), sol gezinti bölmesinde seçin **Azure Active Directory**.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Git **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için seçin **yeni uygulama** iletişim kutusunun üst.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Zscaler Beta**. Seçin **Zscaler Beta** sonuç paneli ve ardından **Ekle**.

     ![Sonuç listesinde Zscaler Beta](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırmanız ve Azure AD çoklu oturum açmayı test Zscaler Beta ile test kullanıcıya Britta Simon bağlı.
Tek iş, bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişki Zscaler Beta kurmak için oturum açma için.

Yapılandırma ve Azure AD çoklu oturum açma ile Zscaler Beta test etmek için aşağıdaki yapı taşlarını tamamlayın:

- [Azure AD çoklu oturum açmayı yapılandırma](#configure-azure-ad-single-sign-on) kullanıcılarınız bu özelliği kullanmak etkinleştirmek için.
- [Zscaler Beta çoklu oturum açmayı yapılandırma](#configure-zscaler-beta-single-sign-on) üzerinde uygulama tarafından çoklu oturum açma ayarları yapılandırmak için.
- [Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user) Azure AD çoklu oturum açma Britta Simon ile test etmek için.
- [Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user) Britta Simon, Azure AD çoklu oturum açma kullanmak üzere etkinleştirmek için.
- [Zscaler Beta test kullanıcısı oluşturma](#create-a-zscaler-beta-test-user) Zscaler Beta, kullanıcının Azure AD gösterimini bağlı bir karşılığı Britta simon'un sağlamak için.
- [Çoklu oturum açmayı test](#test-single-sign-on) yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile Zscaler Beta yapılandırmak için aşağıdaki adımları izleyin.

1. İçinde [Azure portalında](https://portal.azure.com/), **Zscaler Beta** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. İçinde **tek bir oturum açma yönteminizi seçmeniz** Seç iletişim kutusunda **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için **temel SAML yapılandırma** iletişim kutusu.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. İçinde **temel SAML yapılandırma** bölümünde, bu adımı izleyin:

    ![Oturum açma bilgileri çoklu Zscaler Beta etki alanı ve URL'ler](common/sp-intiated.png)

    - İçinde **oturum açma URL'si** kutusunda, Zscaler Beta uygulamanıza oturum açmak için kullanıcılarınız tarafından kullanılan URL'yi girin.

    > [!NOTE]
    > Gerçek bir değer değil. Değeri, gerçek oturum açma URL değeri ile güncelleştirin. Değer almak için iletişime geçin [Zscaler Beta istemci Destek ekibine](https://www.zscaler.com/company/contact).

5. Zscaler Beta uygulama belirli bir biçimde SAML onaylamalarını bekliyor. Özel öznitelik eşlemelerini SAML belirteci öznitelikleri yapılandırmanıza eklemelisiniz. Aşağıdaki ekran görüntüsünde, varsayılan öznitelikler listesinde gösterilmiştir. Seçin **Düzenle** açmak için **kullanıcı öznitelikleri** iletişim kutusu.

    ![Kullanıcı öznitelikleri iletişim kutusu](common/edit-attribute.png)

6. Zscaler Beta uygulama SAML yanıtta geçirilecek birkaç daha fazla öznitelik bekliyor. İçinde **kullanıcı taleplerini** konusundaki **kullanıcı öznitelikleri** iletişim kutusu, şu adımları SAML belirteci öznitelik eklemek için aşağıdaki tabloda gösterildiği gibi izleyin.
    
    | Ad | Kaynak özniteliği | 
    | ---------------| --------------- |
    | İz  | User.assignedroles |

    a. Seçin **Ekle yeni talep** açmak için **yönetmek, kullanıcı talepleri** iletişim kutusu.

    ![Kullanıcı talepleri iletişim kutusu](common/new-save-attribute.png)

    ![Kullanıcı talepleri iletişim kutusu yönetme](common/new-attribute-details.png)

    b. İçinde **adı** kutusunda, ilgili satır için gösterilen öznitelik adını girin.

    c. Bırakın **Namespace** kutusunu boş.

    d. İçin **kaynak**seçin **özniteliği**.

    e. Gelen **kaynak özniteliği** listesinde, ilgili satır için gösterilen öznitelik değeri girin.

    f. **Tamam**’ı seçin.

    g. **Kaydet**’i seçin.

    > [!NOTE]
    > Azure AD'de rolleri yapılandırma konusunda bilgi için bkz: [rol talep yapılandırma](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-app-role-management).

7. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünden **indirme** indirmek için **sertifika (Base64)** . Dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

8. İçinde **Zscaler Beta ' ayarlamak** bölümünde, gereksinimleriniz için gereksinim duyduğunuz URL'leri kopyalayın:

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    - Oturum Açma URL'si:
    - Azure AD Tanımlayıcısı
    - Oturum Kapatma URL'si

### <a name="configure-zscaler-beta-single-sign-on"></a>Zscaler Beta çoklu oturum açmayı yapılandırın

1. Zscaler Beta içinde yapılandırmasını otomatik hale getirmenizi yükleme **My Apps güvenli oturum açma tarayıcı uzantısı** seçerek **uzantıyı yükleme**.

    ![Uygulamaları uzantım](common/install-myappssecure-extension.png)

2. Uzantı tarayıcıya ekledikten sonra seçerek **Zscaler Beta ' ayarlamak** Zscaler Beta uygulamaya yönlendirir. Burada, Zscaler Beta oturum açmak için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı otomatik olarak sizin için bir uygulamayı yapılandırır ve 3-6 adımlarını otomatik hale getirir.

    ![Kurulum yapılandırması](common/setup-sso.png)

3. Zscaler Beta ' el ile ayarlamak için yeni bir web tarayıcı penceresi açın. Zscaler Beta şirketinizin sitesi yönetici olarak oturum açın ve aşağıdaki adımları izleyin.

4. Git **Yönetim** > **kimlik doğrulaması** > **kimlik doğrulama ayarları**ve aşağıdaki adımları izleyin.
   
    ![Yönetim](./media/zscaler-beta-tutorial/ic800206.png "Yönetim")

    a. Altında **kimlik doğrulama türü**seçin **SAML**.

    b. Seçin **SAML'yi yapılandırmak**.

5. İçinde **Düzenle SAML** penceresinde aşağıdaki adımları izleyin: 
            
    ![Kullanıcı ve kimlik doğrulaması yönetmek](./media/zscaler-beta-tutorial/ic800208.png "kullanıcı ve kimlik doğrulaması'nı yönetme")
    
    a. İçinde **SAML portalı URL'si** kutusunda, yapıştırın **oturum açma URL'si** Azure portaldan kopyaladığınız.

    b. İçinde **oturum açma adı özniteliği** kutusuna **Nameıd**.

    c. İçinde **ortak SSL sertifikası** kutusunda **karşıya** Azure portalından indirdiğiniz Azure SAML imzalama sertifikasını karşıya yüklemek için.

    d. İki durumlu **SAML otomatik sağlamayı etkinleştirme**.

    e. İçinde **kullanıcı görünen adı özniteliği** kutusuna **displayName** SAML autoprovisioning displayName öznitelikler için etkinleştirmek istiyorsanız.

    f. İçinde **grubu adı özniteliği** kutusuna **memberOf** SAML autoprovisioning memberOf öznitelikler için etkinleştirmek istiyorsanız.

    g. İçinde **departmanı Name özniteliği** kutusuna **departmanı** SAML autoprovisioning bölüm öznitelikleri için etkinleştirmek istiyorsanız.

    h. **Kaydet**’i seçin.

6. Üzerinde **kullanıcı kimlik doğrulamasını yapılandırma** iletişim sayfasında, aşağıdaki adımları izleyin:

    ![Etkinleştirme menü ve etkinleştir düğmesine](./media/zscaler-beta-tutorial/ic800207.png)

    a. Üzerine **etkinleştirme** sol altta menüsü.

    b. Seçin **etkinleştirme**.

## <a name="configure-proxy-settings"></a>Proxy ayarlarını yapılandırma
Internet Explorer'ın proxy ayarlarını yapılandırmak için aşağıdaki adımları izleyin.

1. Başlangıç **Internet Explorer**.

2. Seçin **Internet Seçenekleri** gelen **Araçları** açmak için menü **Internet Seçenekleri** iletişim kutusu. 
    
     ![Internet Seçenekleri iletişim kutusu](./media/zscaler-beta-tutorial/ic769492.png "Internet Seçenekleri")

3. Seçin **bağlantıları** sekmesi. 
  
     ![Bağlantılar sekmesini](./media/zscaler-beta-tutorial/ic769493.png "bağlantıları")

4. Seçin **LAN Ayarları** açmak için **yerel alan ağı (LAN) ayarları** iletişim kutusu.

5. İçinde **Proxy sunucusu** bölümünde, aşağıdaki adımları izleyin: 
   
    ![Proxy sunucu bölümüne](./media/zscaler-beta-tutorial/ic769494.png "Proxy sunucusu")

    a. Seçin **AĞINIZ için bir proxy sunucusu kullan** onay kutusu.

    b. İçinde **adresi** kutusuna **ağ geçidi. Zscaler Beta.net**.

    c. İçinde **bağlantı noktası** kutusuna **80**.

    d. Seçin **yerel adresler için proxy sunucuyu atla** onay kutusu.

    e. Seçin **Tamam** kapatmak için **yerel alan ağı (LAN) ayarları** iletişim kutusu.

6. Seçin **Tamam** kapatmak için **Internet Seçenekleri** iletişim kutusu.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma 

Britta Simon adlı Azure portalında bir test kullanıcısı oluşturun.

1. Azure portalında, sol bölmede seçin **Azure Active Directory** > **kullanıcılar** > **tüm kullanıcılar**.

    ![Kullanıcılar ve tüm kullanıcıların bağlantılar](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları izleyin:

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** kutusuna `brittasimon@yourcompanydomain.extension`. BrittaSimon@contoso.com bunun bir örneğidir.

    c. Seçin **Show parola** onay kutusu. Görüntülenen değer azaltma **parola** kutusu.

    d. **Oluştur**’u seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Zscaler Beta için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar** > **tüm uygulamaları** > **Zscaler Beta**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesini girin ve seçin **Zscaler Beta**.

    ![Uygulamalar listesinde Zscaler Beta bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcılar ve gruplar bağlantı](common/users-groups-blade.png)

4. Seçin **Kullanıcı Ekle**. İçinde **atama Ekle** iletişim kutusunda **kullanıcılar ve gruplar**.

    ![Kullanıcı Ekle düğmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusu, kullanıcının seçin **Britta Simon** listeden. Ardından **seçin** ekranın alt kısmındaki.

    ![Kullanıcılar ve gruplar iletişim kutusu](./media/zscaler-beta-tutorial/tutorial_zscalerbeta_users.png)

6. İçinde **rolü Seç** iletişim kutusunda, listeden uygun kullanıcı rolünü seçin. Ardından **seçin** ekranın alt kısmındaki.

    ![Rol Seç](./media/zscaler-beta-tutorial/tutorial_zscalerbeta_roles.png)

7. İçinde **atama Ekle** iletişim kutusunda **atama**.

    ![Atama iletişim kutusu Ekle](./media/zscaler-beta-tutorial/tutorial_zscalerbeta_assign.png)

### <a name="create-a-zscaler-beta-test-user"></a>Zscaler Beta test kullanıcısı oluşturma

Bu bölümde, kullanıcının Britta Simon Zscaler Beta sürümünde oluşturuldu. Zscaler Beta destekler **just-ın-time kullanıcı sağlamayı**, varsayılan olarak etkindir. Bu bölümde yapmanız için hiçbir şey yoktur. Zscaler Beta sürümünde bir kullanıcı zaten mevcut değilse yeni bir kimlik doğrulamasından sonra oluşturulur.

>[!Note]
>Bir kullanıcı el ile oluşturmak için kişi [Zscaler Beta Destek ekibine](https://www.zscaler.com/company/contact).

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Azure AD çoklu oturum açma yapılandırmanızı erişim panelini kullanarak test edin.

Erişim Paneli'nde Zscaler Beta kutucuğu seçtiğinizde, otomatik olarak SSO'yu ayarlama Zscaler Beta için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)
- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)
- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

