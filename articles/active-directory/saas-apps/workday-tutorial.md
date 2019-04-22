---
title: 'Öğretici: Workday ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Workday ile Azure Active Directory arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: e9da692e-4a65-4231-8ab3-bc9a87b10bca
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/04/2019
ms.author: jeedes
ms.openlocfilehash: 8451fd692409933803f5f8023f1e1161c3a97daf
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59278540"
---
# <a name="tutorial-azure-active-directory-integration-with-workday"></a>Öğretici: Workday ile Azure Active Directory Tümleştirme

Bu öğreticide, Workday, Azure Active Directory (Azure AD) ile tümleştirme konusunda bilgi edinin.
Workday Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* Workday erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak (çoklu oturum açma) için Workday ile Azure AD hesaplarına oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Workday ile yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Workday çoklu oturum açma abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Workday destekler **SP** ve **IDP** tarafından başlatılan

## <a name="adding-workday-from-the-gallery"></a>Workday galeri ekleme

Workday tümleştirmesi Azure AD'de yapılandırmak için Workday galerideki yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Workday eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Workday**seçin **Workday** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde workday](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma adlı bir test kullanıcı tabanlı Workday ile test etme **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ile ilgili kullanıcının workday'deki arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Workday ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Workday çoklu oturum açmayı yapılandırma](#configure-workday-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Workday test kullanıcısı oluşturma](#create-workday-test-user)**  - kullanıcı Azure AD gösterimini bağlı workday'deki Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma Workday ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Workday** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açma bilgileri workday etki alanı ve URL'ler](common/sp-identifier.png)

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https:\//impl.workday.com/<tenant>/login-saml2.flex`

    b. İçinde **tanımlayıcı** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://www.workday.com`

5. Tıklayın **ek URL'lerini ayarlayın** ve aşağıdaki adımı uygulayın:

    ![Çoklu oturum açma bilgileri workday etki alanı ve URL'ler](./media/workday-tutorial/reply.png)

    İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:  `https:\//impl.workday.com/<tenant>/login-saml.htmld`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve yanıt URL'si ile güncelleştirin. Yanıt URL'si, örneğin bir alt etki alanı olmalıdır: www, wd2, wd3, wd3 Impl, wd5 wd5 Impl).
    > Aşağıdaki gibi kullanarak `http://www.myworkday.com` çalışır ancak `http://myworkday.com` desteklemez. İlgili kişi [Workday istemci Destek ekibine](https://www.workday.com/en-us/partners-services/services/support.html) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

6. Workday uygulamanız SAML onaylamalarını özel öznitelik eşlemelerini SAML belirteci öznitelikleri yapılandırmanıza ekleyin gerektiren belirli bir biçimde bekliyor. Varsayılan öznitelikler listesinde aşağıdaki ekran görüntüsünde gösterilmektedir oysa **NameIdentifier** ile eşlenmiş **user.userprincipalname**. Workday uygulama bekliyor **NameIdentifier** ile eşlenecek **user.mail**, **UPN** vb. tıklayarak özellik eşlemesi düzenlemek gereken şekilde **Düzenle**  simgesi ve değişiklik öznitelik eşlemesi.

    ![image](common/edit-attribute.png)

    > [!NOTE]
    > Burada şu varsayılan UPN (user.userprincipalname) ile ad kimliği eşlediğiniz. SSO, başarılı çalışma için gerçek kullanıcı kimliği, Workday hesabınızdaki (e-postanıza, UPN vb.) ad kimliği eşlemeniz gerekir.

7. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

8. Üzerinde **Workday kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-workday-single-sign-on"></a>Workday çoklu oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde Workday'e şirketinizin sitesi için bir yönetici olarak oturum açın.

2. İçinde **arama kutusuna** adlı arama **Kiracı kurulumunu Düzenle – güvenlik** tarafı giriş sayfasının sol üstte.

    ![Kiracı Güvenliği Düzenle](./media/workday-tutorial/IC782925.png "Kiracı güvenlik Düzenle")

3. İçinde **yeniden yönlendirme URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Yeniden yönlendirme URL'leri](./media/workday-tutorial/IC7829581.png "yeniden yönlendirme URL'leri")

    a. Tıklayın **satır**.

    b. İçinde **oturum açma yeniden yönlendirme URL'si** metin kutusu ve **mobil yeniden yönlendirme URL'si** metin kutusuna **oturum açma URL'si** girmiş olduğunuz **temel SAML yapılandırma**  Azure portal'ın bölümü.

    c. Azure portalında, üzerinde **Workday kümesi** bölümünü, Kopyala **oturum kapatma URL'si**, ardından yapıştırın **oturum kapatma yeniden yönlendirme URL'si** metin.

    d. İçinde **ortamlar için kullanılan** metin ortam adı seçin.  

   > [!NOTE]
   > Ortam özniteliğinin değeri, Kiracı URL'si değerine bağlıdır:  
   > -Workday kiracısı URL'si etki alanı adı ile Impl örneğin başlayıp başlamadığını: *https:\//impl.workday.com/\<Kiracı\>/login-saml2.flex*), **ortam**özniteliği, uygulama için ayarlanmış olması gerekir.  
   > -Etki alanı adı başka bir şey ile başlar, iletişime geçmeniz [Workday istemci Destek ekibine](https://www.workday.com/en-us/partners-services/services/support.html) eşleşen almak için **ortam** değeri.

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

    f.  **Tamam** düğmesine tıklayın.

7. Aşağıdaki adımları gerçekleştirin:

    ![SSO yapılandırma](./media/workday-tutorial/WorkdaySSOConfiguratio.png "SSO yapılandırma")

    a.  İçinde **hizmet sağlayıcı kimliği** metin kutusuna **https://www.workday.com**.

    b. Seçin **SP tarafından başlatılan kimlik doğrulama isteği Deflate değil**.

    c. Olarak **kimlik doğrulaması istek imzası yöntemi**seçin **SHA256**.

    ![Kimlik doğrulaması istek imzası yöntemi](./media/workday-tutorial/WorkdaySSOConfiguration.png "kimlik doğrulaması istek imzası yöntemi") 

    d. **Tamam** düğmesine tıklayın.

    ![TAMAM](./media/workday-tutorial/IC782933.png "TAMAM")

    > [!NOTE]
    > Lütfen, çoklu oturum açmayı doğru ayarlandığından emin olun. Yanlış kurulum ile çoklu oturum açmayı etkinleştirin durumunda, uygulama ile kimlik bilgilerinizi girin ve kilitli mümkün olmayabilir. Bu durumda, Workday yedek bir oturum açma URL'si sağlar. kullanıcılar kendi normal kullanıcı adı ve parola, şu biçimde kullanarak oturum durumlarda: [Your Workday URL]/login.flex?redirect=n

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

Bu bölümde, Azure çoklu oturum açma kullanmak için Workday erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Workday**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Workday**.

    ![Uygulamalar listesini Workday bağlantıdaki](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-workday-test-user"></a>Workday test kullanıcısı oluşturma

Bu bölümde, Workday'de Britta Simon adlı bir kullanıcı oluşturun. Çalışmak [Workday istemci Destek ekibine](https://www.workday.com/en-us/partners-services/services/support.html) Workday platform kullanıcıları eklemek için. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Workday kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama Workday için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

