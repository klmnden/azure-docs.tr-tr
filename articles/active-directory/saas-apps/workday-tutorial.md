---
title: 'Öğretici: Workday ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Workday ile Azure Active Directory arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: e9da692e-4a65-4231-8ab3-bc9a87b10bca
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/20/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: bb9c8d1fb234efd5df297082cfc1001f28ca1656
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65990323"
---
# <a name="tutorial-integrate-workday-with-azure-active-directory"></a>Öğretici: Workday Azure Active Directory ile tümleştirme

Bu öğreticide, Workday, Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. Workday Azure AD ile tümleştirdiğinizde, şunları yapabilirsiniz:

* Workday erişimi, Azure AD'de denetler.
* Otomatik olarak Workday'e için kendi Azure AD hesapları ile oturum açmış olmasını sağlayın.
* Bir merkezi konumda - Azure portalı hesaplarınızı yönetin.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir aboneliğiniz yoksa, alabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free/).
* Workday çoklu oturum açma (SSO) abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD SSO bir test ortamında test edin. Workday destekler **SP** SSO başlattı.

## <a name="adding-workday-from-the-gallery"></a>Workday galeri ekleme

Workday tümleştirmesi Azure AD'de yapılandırmak için Workday galerideki yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde seçin **Azure Active Directory** hizmeti.
1. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları**.
1. Yeni bir uygulama eklemek için seçin **yeni uygulama**.
1. İçinde **Galeriden Ekle** bölümüne şunu yazın **Workday** arama kutusuna.
1. Seçin **Workday** gelen sonuçlar panelinde ve uygulama ekleyin. Uygulama, kiracınıza eklendiği sırada birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Yapılandırma ve Azure AD SSO kullanarak adlı bir test kullanıcı Workday ile test etme **Britta Simon**. Çalışmak SSO için Workday'de bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişki oluşturmanız gerekir.

Yapılandırma ve Azure AD SSO Workday ile test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. **[Azure AD SSO'yu yapılandırma](#configure-azure-ad-sso)**  kullanıcılarınız bu özelliği kullanmak etkinleştirmek için.
2. **[Workday yapılandırma](#configure-workday)**  uygulama tarafında SSO ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  Britta Simon, Azure AD çoklu oturum açma kullanmak üzere etkinleştirmek için.
5. **[Workday test kullanıcısı oluşturma](#create-workday-test-user)**  kullanıcı Azure AD gösterimini bağlantılı iş günü içinde bir karşılığı Britta simon'un sağlamak için.
6. **[Test SSO](#test-sso)**  yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO'yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek üzere aşağıdaki adımları izleyin.

1. İçinde [Azure portalında](https://portal.azure.com/), **Workday** uygulama tümleştirme sayfası, bulma **Yönet** bölümünde ve seçin **çoklu oturum açma**.
1. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** sayfasında **SAML**.
1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında, düzenleme/kalem simgesine tıklayıp **temel SAML yapılandırma** ayarlarını düzenlemek için.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. Üzerinde **temel SAML yapılandırma** sayfasında, aşağıdaki alanlar için değerleri girin:

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://impl.workday.com/<tenant>/login-saml2.flex`

    b. İçinde **tanımlayıcı** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://www.workday.com`

    c. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://impl.workday.com/<tenant>/login-saml.htmld`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve yanıt URL'si ile güncelleştirin. Yanıt URL'si, örneğin bir alt etki alanı olmalıdır: www, wd2, wd3, wd3 Impl, wd5 wd5 Impl).
    > Aşağıdaki gibi kullanarak `http://www.myworkday.com` çalışır ancak `http://myworkday.com` desteklemez. İlgili kişi [Workday istemci Destek ekibine](https://www.workday.com/partners-services/services/support.html) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

6. Workday uygulamanız SAML onaylamalarını özel öznitelik eşlemelerini SAML belirteci öznitelikleri yapılandırmanıza ekleyin gerektiren belirli bir biçimde bekliyor. Varsayılan öznitelikler listesinde aşağıdaki ekran görüntüsünde gösterilmektedir oysa **NameIdentifier** ile eşlenmiş **user.userprincipalname**. Workday uygulama bekliyor **NameIdentifier** ile eşlenecek **user.mail**, **UPN**vb. tıklayarak özellik eşlemesi düzenlemek gereken şekilde  **Düzen** simgesi ve değişiklik öznitelik eşlemesi.

    ![image](common/edit-attribute.png)

    > [!NOTE]
    > Burada şu varsayılan UPN (user.userprincipalname) ile ad kimliği eşlediğiniz. Ad kimliği Workday hesabınızdaki gerçek kullanıcı kimliği ile eşlemeniz gerekir (e-posta, UPN, vb.) için sso'nun başarılı çalışma.

1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde, bulma **sertifika (Base64)** seçip **indirin** sertifikayı indirin ve bilgisayarınıza kaydedin.

   ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. Değiştirilecek **imzalama** seçenekleri, ihtiyacınıza göre **Düzenle** açmak için düğmeyi **SAML imzalama sertifikası** iletişim.

    ![image](common/edit-certificate.png) 

    ![image](./media/workday-tutorial/signing-option.png)

    a. Seçin **oturum SAML yanıtını ve onayını** için **imzalama seçeneği**.

    b. **Kaydet**'e tıklayın.

1. Üzerinde **Workday kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

   ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

### <a name="configure-workday"></a>Workday yapılandırın

1. Farklı bir web tarayıcı penceresinde Workday'e şirketinizin sitesi için bir yönetici olarak oturum açın.

2. İçinde **arama kutusuna** adlı arama **Kiracı kurulumunu Düzenle – güvenlik** tarafı giriş sayfasının sol üstte.

    ![Kiracı Güvenliği Düzenle](./media/workday-tutorial/IC782925.png "Kiracı güvenlik Düzenle")

3. İçinde **yeniden yönlendirme URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Yeniden yönlendirme URL'leri](./media/workday-tutorial/IC7829581.png "yeniden yönlendirme URL'leri")

    a. Tıklayın **satır**.

    b. İçinde **oturum açma yeniden yönlendirme URL'si**, **zaman aşımı tekrar yönlendirme URL'sini** ve **mobil yeniden yönlendirme URL'si** metin kutusu, yapıştırma **oturum açma URL'si** kopyaladığınız gelen **Workday kümesi** Azure Portalı'nın bölümü.

    c. İçinde **oturum kapatma yeniden yönlendirme URL'si** metin kutusu, yapıştırma **oturum kapatma URL'si** kopyalanan **Workday kümesi** Azure Portalı'nın bölümü.

    d. İçinde **ortamlar için kullanılan** metin ortam adı seçin.  

   > [!NOTE]
   > Ortam özniteliğinin değeri, Kiracı URL'si değerine bağlıdır:  
   > -Workday kiracısı URL'si etki alanı adı ile Impl örneğin başlayıp başlamadığını: *https:\//impl.workday.com/\<Kiracı\>/login-saml2.flex*), **ortam**özniteliği, uygulama için ayarlanmış olması gerekir.  
   > -Etki alanı adı başka bir şey ile başlar, iletişime geçmeniz [Workday istemci Destek ekibine](https://www.workday.com/partners-services/services/support.html) eşleşen almak için **ortam** değeri.

4. İçinde **SAML Kurulumu** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![SAML Kurulumu](./media/workday-tutorial/IC782926.png "SAML Kurulumu")

    a.  Seçin **SAML kimlik doğrulamasını etkinleştirme**.

    b.  Tıklayın **satır**.

5. İçinde **SAML kimlik sağlayıcısı** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![SAML kimlik sağlayıcısı](./media/workday-tutorial/IC7829271.png "SAML kimlik sağlayıcıları")

    a. İçinde **kimlik sağlayıcı adı** metin kutusuna sağlayıcı adını yazın (örneğin: *SPInitiatedSSO*).

    b. Azure portalında, üzerinde **Workday ayarlayın** bölümünü, Kopyala **Azure AD tanımlayıcısı** değeri ve ardından yapıştırın **veren** metin.

    ![SAML kimlik sağlayıcısı](./media/workday-tutorial/IC7829272.png "SAML kimlik sağlayıcıları")

    c. Azure portalında, üzerinde **Workday kümesi** bölümünü, Kopyala **oturum kapatma URL'si** değeri ve ardından yapıştırın **oturum kapatma yanıt URL'si** metin.

    d. Azure portalında, üzerinde **Workday kümesi** bölümünü, Kopyala **oturum açma URL'si** değeri ve ardından yapıştırın **IDP SSO hizmet URL'si** metin.

    e. İçinde **ortamlar için kullanılan** metin ortam adı seçin.

    f. Tıklayın **kimlik sağlayıcısı ortak anahtar sertifikası**ve ardından **Oluştur**.

    ![Oluşturma](./media/workday-tutorial/IC782928.png "oluşturma")

    g. Tıklayın **x509 oluşturma ortak anahtar**.

    ![Oluşturma](./media/workday-tutorial/IC782929.png "oluşturma")

6. İçinde **görünümü x509 ortak anahtar** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Görünüm x509 ortak anahtar](./media/workday-tutorial/IC782930.png "görünümü x509 ortak anahtarı")

    a. İçinde **adı** metin sertifikanız için bir ad yazın (örneğin: *PPE\_SP*).

    b. İçinde **geçerlilik başlangıcı** metin geçerlilik sertifikanızın öznitelik değeri yazın.

    c.  İçinde **için geçerli** metin sertifikanızın öznitelik değeri geçerli yazın.

    > [!NOTE]
    > Tarih ve geçerli çift tıklayarak indirilen sertifikası tarihi geçerli alabilirsiniz.  Tarihleri altında listelenen **ayrıntıları** sekmesi.
    >
    >

    d.  Base-64 kodlanmış sertifikanızı Not Defteri'nde açın ve içeriğini kopyalayın.

    e.  İçinde **sertifika** metin kutusu, panonuzun içeriğini yapıştırın.

    f.  **Tamam**'ı tıklatın.

7. Aşağıdaki adımları gerçekleştirin:

    ![SSO yapılandırma](./media/workday-tutorial/WorkdaySSOConfiguratio.png "SSO yapılandırma")

    a.  İçinde **hizmet sağlayıcı kimliği** metin kutusuna **https://www.workday.com**.

    b. Seçin **SP tarafından başlatılan kimlik doğrulama isteği Deflate değil**.

    c. Olarak **kimlik doğrulaması istek imzası yöntemi**seçin **SHA256**.

    ![Kimlik doğrulaması istek imzası yöntemi](./media/workday-tutorial/WorkdaySSOConfiguration.png "kimlik doğrulaması istek imzası yöntemi") 

    d. **Tamam**'ı tıklatın.

    ![TAMAM](./media/workday-tutorial/IC782933.png "TAMAM")

    > [!NOTE]
    > Lütfen, çoklu oturum açmayı doğru ayarlandığından emin olun. Yanlış kurulum ile çoklu oturum açmayı etkinleştirin durumunda, uygulama ile kimlik bilgilerinizi girin ve kilitli mümkün olmayabilir. Bu durumda, Workday yedek bir oturum açma URL'si sağlar. kullanıcılar kendi normal kullanıcı adı ve parola, şu biçimde kullanarak oturum durumlarda: [Your Workday URL]/login.flex?redirect=n

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümde, bir test kullanıcısı Britta Simon adlı Azure portalında oluşturacaksınız.

1. Azure Portalı'ndaki sol bölmeden seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.
1. Seçin **yeni kullanıcı** ekranın üstünde.
1. İçinde **kullanıcı** özellikleri, aşağıdaki adımları izleyin:
   1. **Ad** alanına `Britta Simon` girin.  
   1. İçinde **kullanıcı adı** alanına username@companydomain.extension. Örneğin, `BrittaSimon@contoso.com`.
   1. Seçin **Show parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.
   1. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Workday erişim vererek Britta Simon tıklatmalarını sağlarsınız.

1. Azure portalında **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.
1. Uygulamalar listesinde **Workday**.
1. Uygulamanın genel bakış sayfasında bulma **Yönet** seçin ve bölüm **kullanıcılar ve gruplar**.

   !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

1. Seçin **Kullanıcı Ekle**, ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Kullanıcı ekleme bağlantısı](common/add-assign-user.png)

1. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcılar listesinden ardından **seçin** ekranın alt kısmındaki düğmesi.
1. SAML onaylama işlemi herhangi bir rolü değer de beklediğiniz varsa **rolü Seç** iletişim kutusunda, listeden bir kullanıcı için uygun rolü seçin ve ardından **seçin** ekranın alt kısmındaki düğmesi.
1. İçinde **atama Ekle** iletişim kutusunda, tıklayın **atama** düğmesi.

### <a name="create-workday-test-user"></a>Workday test kullanıcısı oluşturma

Bu bölümde, Workday'de Britta Simon adlı bir kullanıcı oluşturun. Çalışmak [Workday istemci Destek ekibine](https://www.workday.com/en-us/partners-services/services/support.html) Workday platform kullanıcıları eklemek için. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi.

### <a name="test-sso"></a>Test SSO

Erişim Paneli'nde Workday kutucuğu seçtiğinizde, otomatik olarak SSO'yu ayarlama Workday için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
