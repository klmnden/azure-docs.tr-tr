---
title: 'Öğretici: Tableau Server ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Tableau Server ile Azure Active Directory arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: c1917375-08aa-445c-a444-e22e23fa19e0
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/22/2019
ms.author: jeedes
ms.openlocfilehash: 539a06398675dc7851017ec5d428e0942e54ce1f
ms.sourcegitcommit: b8a8d29fdf199158d96736fbbb0c3773502a092d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2019
ms.locfileid: "59564774"
---
# <a name="tutorial-azure-active-directory-integration-with-tableau-server"></a>Öğretici: Tableau Server ile Azure Active Directory Tümleştirme

Bu öğreticide, Tableau Server, Azure Active Directory (Azure AD) ile tümleştirme konusunda bilgi edinin.
Tableau Server, Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* Tableau Server erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak (çoklu oturum açma) Tableau Server için oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Tableau Server ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa alabileceğiniz bir [ücretsiz hesap](https://azure.microsoft.com/free/)
* Tableau Server Çoklu oturum açmayı abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Tableau Server destekler **SP** tarafından başlatılan

## <a name="adding-tableau-server-from-the-gallery"></a>Galeriden Tableau Server ekleme

Azure AD'de Tableau Server tümleştirmesini yapılandırmak için Tableau Server Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Tableau Server eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Tableau Server**seçin **Tableau Server** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde tableau Server](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Tableau Server'ın adlı bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ve Tableau Server ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Tableau Server ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Tableau Server Çoklu oturum açmayı yapılandırma](#configure-tableau-server-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Tableau Server test kullanıcısı oluşturma](#create-tableau-server-test-user)**  - kullanıcı Azure AD gösterimini bağlı Tableau Server Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma Tableau Server ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Tableau Server** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Tableau Server etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier-reply.png)

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://azure.<domain name>.link`

    b. İçinde **tanımlayıcı** kutusuna şu biçimi kullanarak bir URL yazın: `https://azure.<domain name>.link`

    c. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://azure.<domain name>.link/wg/saml/SSO/index.html`

    > [!NOTE]
    > Yukarıdaki değerlerden gerçek değerlerin değil. Gerçek URL ve tanımlayıcıdır öğreticinin ilerleyen bölümlerinde açıklanan Tableau Server yapılandırma sayfasından ile güncelleştirin.

5. Tableau Server uygulaması bir özel talep bekliyor **kullanıcıadı** aşağıda olarak tanımlanması gerekir. Benzersiz kullanıcı tanımlayıcısı talebi yerine kullanıcı tanımlayıcısı olarak kullanılıyor. Bu öznitelikleri değerlerini yönetebilirsiniz **kullanıcı öznitelikleri ve talepler** uygulama tümleştirme sayfasında bölümü. Tıklayın **Düzenle** açmak için düğmeyi **kullanıcı öznitelikleri ve talepler** iletişim.

    ![image](common/edit-attribute.png)

6. İçinde **kullanıcı taleplerini** bölümünde **kullanıcı öznitelikleri ve talepler** iletişim kutusunda, SAML belirteci özniteliği yukarıdaki görüntüde gösterilen şekilde yapılandırın ve aşağıdaki adımları gerçekleştirin:

    | Ad | Kaynak özniteliği | Ad alanı |
    | ---------------| --------------- | ----------- |
    | kullanıcı adı | User.userPrincipalName | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims` |
    | | |

    a. Tıklayın **Ekle yeni talep** açmak için **yönetmek, kullanıcı talepleri** iletişim.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.

    c. Girin **Namespace** değeri.

    d. Kaynağı olarak **özniteliği**.

    e. Gelen **kaynak özniteliği** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    f. Tıklayın **Tamam**

    g. **Kaydet**’e tıklayın.

7. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

8. Üzerinde **Tableau Server Ayarla** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-tableau-server-single-sign-on"></a>Tableau Server Çoklu oturum açmayı yapılandırın

1. Uygulamanız için yapılandırılmış SSO almak için Tableau Server kiracınıza yönetici olarak oturum açmanız gerekir.

2. Üzerinde **yapılandırma** sekmesinde **kullanıcı kimlik ve erişim**ve ardından **kimlik doğrulaması** yöntemi sekmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/tableauserver-tutorial/tutorial-tableauserver-auth.png)

3. Üzerinde **yapılandırma** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/tableauserver-tutorial/tutorial-tableauserver-config.png)

    a. İçin **kimlik doğrulama yöntemi**, SAML seçin.

    b. Onay kutusunu işaretleyin, **sunucusu için SAML kimlik doğrulamasını etkinleştir**.

    c. Tableau Server dönüş URL'si — Tableau Server kullanıcıları, gibi erişecek URL <http://tableau_server>. Kullanarak `http://localhost` önerilmez. Bir URL kullanarak bir eğik çizgiyle (örneğin, `http://tableau_server/`) desteklenmiyor. Kopyalama **Tableau Server dönüş URL'si** için yapıştırın **işareti bulunan URL'si** metin kutusunda **temel SAML yapılandırma** bölümünde Azure portalında

    d. SAML varlık kimliği — varlık kimliği için IDP Tableau Server yüklemenizin benzersiz olarak tanımlar. Tableau Server URL'nizi yeniden burada isterseniz girebilirsiniz, ancak Tableau Server URL'nizi olması gerekmez. Kopyalama **SAML varlık kimliği** için yapıştırın **tanımlayıcı** metin kutusunda **temel SAML yapılandırma** bölümünde Azure portalında

    e. Tıklayın **XML meta veri dosyası yükle** ve metin düzenleyicisi uygulamayı açın. Onay belgesi tüketici hizmeti URL'si Http Post ile bulup 0 dizini ve URL'yi kopyalayın. Artık kopyalayıp, yapıştırın **yanıt URL'si** metin kutusunda **temel SAML yapılandırma** bölümünde Azure portalında

    f. Azure portalından indirdiğiniz, Federasyon meta verileri dosyasını bulun ve bunu karşıya **IDP SAML meta veri dosyası**.

    g. Idp'nin kullanıcı adlarını basılı tutun, görünen adları ve e-posta adreslerini kullanan öznitelikleri adlarını girin.

    h. **Kaydet**’e tıklayın

    > [!NOTE]
    > Müşteriniz varsa Tableau Server SAML SSO yapılandırma herhangi bir sertifikayı karşıya yüklemek ve SSO akışta dikkate. Tableau Server üzerinde SAML yapılandırmasına yardımcı olması sonra lütfen bu makaleyi okuyun [SAML yapılandırma](https://onlinehelp.tableau.com/v2018.2/server/en-us/saml_config_steps_tsm_ui.htm).

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü `brittasimon@yourcompanydomain.extension`  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Tableau Server için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Tableau Server**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Tableau Server**.

    ![Uygulamalar listesinde Tableau Server bağlantısı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-tableau-server-test-user"></a>Tableau Server test kullanıcısı oluşturma

Bu bölümün amacı, Tableau Server Britta Simon adlı bir kullanıcı oluşturmaktır. Tableau server içindeki tüm kullanıcılar sağlamanız gerekir.

Bu kullanıcının kullanıcı adı Azure AD'ye özel öznitelik içinde yapılandırılmış değer ile eşleşmelidir **username**. Doğru eşleme ile tümleştirmeyi yapılandırma Azure AD çoklu oturum açma çalışması gerekir.

> [!NOTE]
> Bir kullanıcı el ile oluşturmanız gerekiyorsa, kuruluşunuzdaki Tableau Server yöneticinize başvurun gerekir.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Tableau Server kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama Tableau Server için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

