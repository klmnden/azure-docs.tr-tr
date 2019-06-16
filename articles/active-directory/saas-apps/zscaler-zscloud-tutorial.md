---
title: 'Öğretici: Zscaler ZSCloud ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Zscaler ZSCloud arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 411d5684-a780-410a-9383-59f92cf569b5
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/24/2019
ms.author: jeedes
ms.openlocfilehash: f83fdf1b30e3b9d2a8dea9a4fe3c820e15a3ca39
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67085845"
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-zscloud"></a>Öğretici: Zscaler ZSCloud ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Zscaler ZSCloud tümleştirme konusunda bilgi edinin.
Zscaler ZSCloud Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* Zscaler ZSCloud erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak (çoklu oturum açma) Zscaler ZSCloud için oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Zscaler ZSCloud yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa alabileceğiniz bir [ücretsiz hesap](https://azure.microsoft.com/free/)
* Zscaler ZSCloud çoklu oturum açma abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Zscaler ZSCloud destekler **SP** tarafından başlatılan

* Zscaler ZSCloud destekler **zamanında** kullanıcı sağlama

## <a name="adding-zscaler-zscloud-from-the-gallery"></a>Zscaler ZSCloud galeri ekleme

Azure AD'de Zscaler ZSCloud tümleştirmesini yapılandırmak için Zscaler ZSCloud Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Zscaler ZSCloud eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Zscaler ZSCloud**seçin **Zscaler ZSCloud** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde Zscaler ZSCloud](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırmanız ve Zscaler ZSCloud ile Azure AD çoklu oturum açmayı test adlı bir test kullanıcı tabanlı **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının Zscaler ZSCloud ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Zscaler ZSCloud ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Zscaler ZSCloud çoklu oturum açmayı yapılandırma](#configure-zscaler-zscloud-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Zscaler ZSCloud test kullanıcısı oluşturma](#create-zscaler-zscloud-test-user)**  - kullanıcı Azure AD gösterimini bağlı Zscaler ZSCloud Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile Zscaler ZSCloud yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Zscaler ZSCloud** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Zscaler ZSCloud etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-signonurl.png)

    İçinde **oturum açma URL'si** metin kutusu, türü URL kullanıcılarınız oturum açmaya ZScaler ZSCloud uygulamanıza tarafından kullanılıyor.

    > [!NOTE]
    > Değerini gerçek oturum açma URL'si ile güncelleştirmeniz gerekir. İlgili kişi [Zscaler ZSCloud istemci Destek ekibine](https://help.zscaler.com/) değeri alınamıyor. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. Zscaler ZSCloud uygulamanız SAML onaylamalarını özel öznitelik eşlemelerini SAML belirteci öznitelikleri yapılandırmanıza ekleyin gerektiren belirli bir biçimde bekliyor. Aşağıdaki ekran görüntüsünde, varsayılan öznitelikler listesinde gösterilmiştir. Tıklayın **Düzenle** açmak için simgeyi **kullanıcı öznitelikleri** iletişim.

    ![image](common/edit-attribute.png)

6. Yukarıdaki için ayrıca Zscaler ZSCloud uygulama SAML yanıtta geçirilecek birkaç daha fazla öznitelik bekliyor. İçinde **kullanıcı taleplerini** bölümünde **kullanıcı öznitelikleri** iletişim kutusunda gösterildiği gibi SAML belirteci özniteliği eklemek için aşağıdaki adımları gerçekleştirin tablonun altındaki:
    
    | Ad | Kaynak özniteliği |
    | ---------| ------------ |
    | İz     | User.assignedroles |

    a. Tıklayın **Ekle yeni talep** açmak için **yönetmek, kullanıcı talepleri** iletişim.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.

    c. Bırakın **Namespace** boş.

    d. Kaynağı olarak **özniteliği**.

    e. Gelen **kaynak özniteliği** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.
    
    f. **Kaydet**’e tıklayın.

    > [!NOTE]
    > Lütfen tıklayın [burada](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-app-role-management) Azure AD'de rol yapılandırma bilmek

7. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

8. Üzerinde **Zscaler ZSCloud kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-zscaler-zscloud-single-sign-on"></a>Zscaler ZSCloud çoklu oturum açmayı yapılandırın

1. Zscaler ZSCloud içinde yapılandırmasını otomatik hale getirmenizi yüklemeniz gerekir **My Apps güvenli oturum açma tarayıcı uzantısı** tıklayarak **uzantıyı yükleme**.

    ![Uygulamaları uzantım](common/install-myappssecure-extension.png)

2. Uzantı tarayıcıya ekledikten sonra tıklayarak **Kurulum Zscaler ZSCloud** Zscaler ZSCloud uygulamaya yönlendirir. Burada, Zscaler ZSCloud oturum açmak için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı otomatik olarak sizin için uygulamayı yapılandırma ve 3-6 adımları otomatik hale getirin.

    ![Kurulum sso](common/setup-sso.png)

3. Zscaler ZSCloud el ile ayarlamak istiyorsanız, yeni bir web tarayıcı penceresi ve oturum Zscaler ZSCloud şirketinizin sitesi yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

4. Git **Yönetim > kimlik doğrulama > kimlik doğrulama ayarları** ve aşağıdaki adımları gerçekleştirin:
   
    ![Yönetim](./media/zscaler-zscloud-tutorial/ic800206.png "Yönetim")

    a. Kimlik doğrulaması türü'nün altında seçin **SAML**.

    b. Tıklayın **SAML'yi yapılandırmak**.

5. Üzerinde **Düzenle SAML** penceresinde aşağıdaki adımları gerçekleştirin: ve Kaydet'e tıklayın.  
            
    ![Kullanıcı ve kimlik doğrulaması yönetmek](./media/zscaler-zscloud-tutorial/ic800208.png "kullanıcı ve kimlik doğrulaması'nı yönetme")
    
    a. İçinde **SAML portalı URL'si** metin kutusu, yapıştırma **oturum açma URL'si** , Azure Portalı'ndan kopyaladığınız.

    b. İçinde **oturum açma adı özniteliği** metin girin **Nameıd**.

    c. Tıklayın **karşıya**, içinde Azure portalından indirilen Azure SAML imzalama sertifikasını karşıya yüklemek için **ortak SSL sertifikası**.

    d. İki durumlu **SAML otomatik sağlamayı etkinleştirme**.

    e. İçinde **kullanıcı görünen adı özniteliği** metin girin **displayName** SAML otomatik sağlama displayName öznitelikler için etkinleştirmek istiyorsanız.

    f. İçinde **grubu adı özniteliği** metin girin **memberOf** SAML otomatik sağlama memberOf öznitelikler için etkinleştirmek istiyorsanız.

    g. İçinde **departmanı Name özniteliği** Enter **departmanı** SAML otomatik sağlama için bölüm özniteliklerini etkinleştirmek istiyorsanız.

    h. **Kaydet**’e tıklayın.

6. Üzerinde **kullanıcı kimlik doğrulamasını yapılandırma** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Yönetim](./media/zscaler-zscloud-tutorial/ic800207.png)

    a. Üzerine **etkinleştirme** ekranın sol menü.

    b. Tıklayın **etkinleştirme**.

## <a name="configuring-proxy-settings"></a>Ara sunucu ayarlarını yapılandırma
### <a name="to-configure-the-proxy-settings-in-internet-explorer"></a>Internet Explorer'ın proxy ayarlarını yapılandırmak için

1. Başlangıç **Internet Explorer**.

2. Seçin **Internet Seçenekleri** gelen **Araçları** menüsünü aç **Internet Seçenekleri** iletişim.   
    
     ![Internet Seçenekleri](./media/zscaler-zscloud-tutorial/ic769492.png "Internet Seçenekleri")

3. Tıklayın **bağlantıları** sekmesi.   
  
     ![Bağlantıları](./media/zscaler-zscloud-tutorial/ic769493.png "bağlantıları")

4. Tıklayın **LAN Ayarları** açmak için **LAN Ayarları** iletişim.

5. Proxy sunucusu bölümünde aşağıdaki adımları gerçekleştirin:   
   
    ![Proxy sunucusu](./media/zscaler-zscloud-tutorial/ic769494.png "Proxy sunucusu")

    a. Seçin **AĞINIZ için bir proxy sunucusu kullan**.

    b. Adresi metin kutusuna yazın **ağ geçidi. Zscaler ZSCloud.net**.

    c. Bağlantı noktası metin kutusuna yazın **80**.

    d. Seçin **yerel adresler için proxy sunucuyu atla**.

    e. Tıklayın **Tamam** kapatmak için **yerel alan ağı (LAN) ayarları** iletişim.

6. Tıklayın **Tamam** kapatmak için **Internet Seçenekleri** iletişim.

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

Bu bölümde, Azure çoklu oturum açma kullanmak için Zscaler ZSCloud erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Zscaler ZSCloud**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Zscaler ZSCloud**.

    ![Uygulamalar listesinde Zscaler ZSCloud bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** kullanıcı gibi iletişim kutusunda **Britta Simon** listeden ardından **seçin** ekranın alt kısmındaki düğmesi.

    ![image](./media/zscaler-zscloud-tutorial/tutorial_zscalerzscloud_users.png)

6. Gelen **rolü Seç** iletişim listede uygun kullanıcı rolünü seçin ve ardından tıklayın **seçin** ekranın alt kısmındaki düğmesi.

    ![image](./media/zscaler-zscloud-tutorial/tutorial_zscalerzscloud_roles.png)

7. İçinde **atama Ekle** iletişim kutusunda **atama** düğmesi.

    ![image](./media/zscaler-zscloud-tutorial/tutorial_zscalerzscloud_assign.png)

    >[!NOTE]
    >Bu sağlama, keser gibi varsayılan rol kullanıcı atanırken seçilemez bu nedenle varsayılan erişim rolünü desteklenmiyor.

### <a name="create-zscaler-zscloud-test-user"></a>Zscaler ZSCloud test kullanıcısı oluşturma

Bu bölümde, Britta Simon adlı bir kullanıcı Zscaler ZSCloud oluşturulur. Zscaler ZSCloud just-ın-time kullanıcı hazırlama, varsayılan olarak etkin olduğu destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Zscaler ZSCloud içinde bir kullanıcı zaten mevcut değilse yeni bir kimlik doğrulamasından sonra oluşturulur.

>[!Note]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [Zscaler ZSCloud Destek ekibine](https://help.zscaler.com/).

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Zscaler ZSCloud kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama Zscaler ZSCloud için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

