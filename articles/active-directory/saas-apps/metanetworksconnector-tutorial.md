---
title: 'Öğretici: Meta ağları Bağlayıcısı ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Meta ağları bağlayıcı arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 4ae5f30d-113b-4261-b474-47ffbac08bf7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/21/2019
ms.author: jeedes
ms.openlocfilehash: ae0b8bb6dec4b129a4965426789819e119a25c53
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65991499"
---
# <a name="tutorial-azure-active-directory-integration-with-meta-networks-connector"></a>Öğretici: Meta ağları Bağlayıcısı ile Azure Active Directory Tümleştirme

Bu öğreticide, Meta ağları bağlayıcı Azure Active Directory (Azure AD) ile tümleştirmeyi öğrenin.
Meta ağları Bağlayıcısı Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* Meta ağları bağlayıcı erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak Meta ağları bağlayıcıya (çoklu oturum açma) oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Meta ağları Bağlayıcısı ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Abonelik meta ağları bağlayıcı çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Meta ağları bağlayıcı destekler **SP** ve **IDP** tarafından başlatılan
 
* Meta ağları bağlayıcı destekler **zamanında** kullanıcı sağlama

## <a name="adding-meta-networks-connector-from-the-gallery"></a>Galeriden meta ağları bağlayıcı ekleme

Azure AD'ye Meta ağları bağlayıcı tümleştirmesini yapılandırmak için Meta ağları bağlayıcı Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden meta ağları bağlayıcı eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Meta ağları bağlayıcı**seçin **Meta ağları bağlayıcı** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde meta ağları Bağlayıcısı](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Meta ağları adlı bir test kullanıcı tabanlı Bağlayıcısı ile Azure AD çoklu oturum açmayı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ve ilgili kullanıcı Meta ağları bağlayıcısında arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Meta ağları Bağlayıcısı ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Meta ağları bağlayıcı çoklu oturum açmayı yapılandırma](#configure-meta-networks-connector-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Meta ağları bağlayıcı test kullanıcısı oluşturma](#create-meta-networks-connector-test-user)**  - kullanıcı Azure AD gösterimini bağlı Meta ağları bağlayıcısında Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma Meta ağları Bağlayıcısı ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Meta ağları bağlayıcı** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** uygulamada yapılandırmak isterseniz, bölümü **IDP** başlatılan modu, aşağıdaki adımları gerçekleştirin:

    ![Meta ağları bağlayıcı etki alanı ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. İçinde **tanımlayıcı** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://login.nsof.io/v1/<ORGANIZATION-SHORT-NAME>/saml/metadata`

    b. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://login.nsof.io/v1/<ORGANIZATION-SHORT-NAME>/sso/saml`

5. Tıklayın **ek URL'lerini ayarlayın** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![Meta ağları bağlayıcı etki alanı ve URL'ler tek oturum açma bilgileri](common/both-advanced-urls.png)

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<ORGANIZATION-SHORT-NAME>.metanetworks.com/login`

    b. İçinde **geçiş durumu** metin kutusuna bir URL şu biçimi kullanarak: `https://<ORGANIZATION-SHORT-NAME>.metanetworks.com/#/`

    > [!NOTE]
    > Bu değerler gerçek değildir. Yanıt URL'si gerçek tanımlayıcısı bu değerleri güncelleştirin ve oturum açma URL'si, öğreticinin ilerleyen bölümlerinde açıklanmıştır.

6. Meta ağları bağlayıcı uygulaması, özel öznitelik eşlemelerini SAML belirteci öznitelikleri yapılandırmanıza ekleyin gerektiren belirli bir biçimde SAML onaylamalarını bekliyor. Aşağıdaki ekran görüntüsünde, varsayılan öznitelikler listesinde gösterilmiştir. Tıklayın **Düzenle** açmak için simgeyi **kullanıcı öznitelikleri** iletişim.

    ![image](common/edit-attribute.png)
    
7. Yukarıdaki için Ayrıca, SAML yanıtta geçirilecek birkaç daha fazla öznitelik Meta ağları bağlayıcı uygulaması bekliyor. İçinde **kullanıcı taleplerini** bölümünde **kullanıcı öznitelikleri** iletişim kutusunda gösterildiği gibi SAML belirteci özniteliği eklemek için aşağıdaki adımları gerçekleştirin tablonun altındaki:
    
    | Ad | Kaynak özniteliği | Ad alanı|
    | ---------------| --------------- | -------- |
    | firstName | User.givenName | |
    | Soyadı | User.surname | |
    | emailaddress| User.Mail| `http://schemas.xmlsoap.org/ws/2005/05/identity/claims` |
    | name | User.userPrincipalName| `http://schemas.xmlsoap.org/ws/2005/05/identity/claims` |
    | telefon | User.telephoneNumber | |

    a. Tıklayın **Ekle yeni talep** açmak için **yönetmek, kullanıcı talepleri** iletişim.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.

    c. Bırakın **Namespace** boş.

    d. Kaynağı olarak **özniteliği**.

    e. Gelen **kaynak özniteliği** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    f. Tıklayın **Tamam**

    g. **Kaydet**’e tıklayın.

8. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

9. Üzerinde **Meta ağları Bağlayıcısı'nı Ayarla** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-meta-networks-connector-single-sign-on"></a>Meta ağları bağlayıcı çoklu oturum açmayı yapılandırın

1. Tarayıcınızda yeni bir sekme açın ve Meta ağları Bağlayıcısı yönetici hesabınızda oturum açın.
    
    > [!NOTE]
    > Güvenli sistem meta ağları Bağlayıcıdır. Bu nedenle, portal erişmeden önce kendi tarafında bir izin verilenler listesi eklenen, genel IP adresini almak gerekir. Genel IP adresi almak için izlemeniz aşağıdaki bağlantıda belirtilen [burada](https://whatismyipaddress.com/). IP adresiniz Gönder [Meta ağları bağlayıcı istemci Destek ekibine](mailto:support@metanetworks.com) bir izin verilenler listesine eklendiğinden, IP adresini almak için.
    
2. Git **yönetici** seçip **ayarları**.
    
    ![Çoklu oturum açmayı yapılandırın](./media/metanetworksconnector-tutorial/configure3.png)
    
3. Emin **Internet trafiğini günlüğe kaydetme** ve **zorla VPN MFA** kapalı olarak ayarlayın.
    
    ![Çoklu oturum açmayı yapılandırın](./media/metanetworksconnector-tutorial/configure1.png)
    
4. Git **yönetici** seçip **SAML**.
    
    ![Çoklu oturum açmayı yapılandırın](./media/metanetworksconnector-tutorial/configure4.png)
    
5. Aşağıdaki adımları gerçekleştirin **ayrıntıları** sayfası:
    
    ![Çoklu oturum açmayı yapılandırın](./media/metanetworksconnector-tutorial/configure2.png)
    
    a. Kopyalama **SSO URL** yapıştırın ve değer **oturum açma URL'si** metin kutusunda **Meta ağları bağlayıcı etki alanı ve URL'ler** bölümü.
    
    b. Kopyalama **alıcı URL** yapıştırın ve değer **yanıt URL'si** metin kutusunda **Meta ağları bağlayıcı etki alanı ve URL'ler** bölümü.
    
    c. Kopyalama **hedef kitle URİ'si (SP varlık kimliği)** yapıştırın ve değer **tanımlayıcı (varlık kimliği)** metin kutusunda **Meta ağları bağlayıcı etki alanı ve URL'ler** bölümü.
    
    d. SAML'yi etkinleştir
    
6. Üzerinde **genel** sekmesinde aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/metanetworksconnector-tutorial/configure5.png)

    a. İçinde **kimlik sağlayıcısının çoklu oturum açma URL'si**, Yapıştır **oturum açma URL'si** Azure portaldan kopyaladığınız değeri.

    b. İçinde **kimlik sağlayıcısını veren**, Yapıştır **Azure AD tanımlayıcısı** Azure portaldan kopyaladığınız değeri.

    c. Açık Not Defteri'nde, Azure portalından indirilen sertifikanın yapıştırın içine **X.509 sertifikası** metin.

    d. Etkinleştirme **tam zamanında sağlama**.

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

Bu bölümde, Azure çoklu oturum açma Meta ağları bağlayıcısına erişim izni verdiğinizde kullanmak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Meta ağları bağlayıcı**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Meta ağları bağlayıcı**.

    ![Uygulamalar listesini Meta ağları Bağlayıcısı bağlantıyı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim kutusunda, tıklayın **atama** düğmesi.

### <a name="create-meta-networks-connector-test-user"></a>Meta ağları bağlayıcı test kullanıcısı oluşturma

Bu bölümde, Britta Simon adlı bir kullanıcı Meta ağları bağlayıcısında oluşturulur. Meta ağları bağlayıcı tam zamanında sağlama, varsayılan olarak etkin olduğu destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Meta ağları bağlayıcısında bir kullanıcı zaten mevcut değilse yeni bir tane Meta ağları bağlayıcı erişmeye oluşturulur.

>[!Note]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [Meta ağları bağlayıcı istemci Destek ekibine](mailto:support@metanetworks.com).

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Meta ağları bağlayıcı kutucuğa tıkladığınızda, otomatik olarak Meta ağları SSO'yu ayarlama bağlayıcısına oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

