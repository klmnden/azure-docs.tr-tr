---
title: 'Öğretici: Workday ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Workday ile Azure Active Directory arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: cmmdesai
manager: daveba
ms.reviewer: jeedes
ms.assetid: e9da692e-4a65-4231-8ab3-bc9a87b10bca
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/11/2018
ms.author: chmutali
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2e3f60c3b0578647e68109a21ba7d57b083bea11
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56184542"
---
# <a name="tutorial-azure-active-directory-integration-with-workday"></a>Öğretici: Workday ile Azure Active Directory Tümleştirme

Bu öğreticide, Workday, Azure Active Directory (Azure AD) ile tümleştirme konusunda bilgi edinin.

Workday Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Workday erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan için Workday (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Workday ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir iş günü çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Workday galeri ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-workday-from-the-gallery"></a>Workday galeri ekleme

Workday tümleştirmesi Azure AD'de yapılandırmak için Workday galerideki yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Workday eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Workday**seçin **Workday** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde workday](./media/workday-tutorial/tutorial_workday_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Workday ile test edin.

Tek çalışmak için oturum açma için Azure AD ne karşılık gelen kullanıcının workday'deki bir kullanıcının Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile ilgili kullanıcının workday'deki arasında bir bağlantı ilişki kurulması gerekir.

Değerini, Workday'de atama **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Workday ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Workday test kullanıcısı oluşturma](#create-a-workday-test-user)**  - kullanıcı Azure AD gösterimini bağlı workday'deki Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Workday uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma Workday ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Workday** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma iletişim kutusu](./media/workday-tutorial/tutorial_workday_samlbase.png)

3. Üzerinde **Workday etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açma bilgileri workday etki alanı ve URL'ler](./media/workday-tutorial/tutorial_workday_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://impl.workday.com/<tenant>/login-saml2.htmld`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL: `https://www.workday.com`

4. Denetleme **Gelişmiş URL ayarlarını göster** ve aşağıdaki adımı uygulayın:

    ![Çoklu oturum açma bilgileri workday etki alanı ve URL'ler](./media/workday-tutorial/tutorial_workday_url1.png)

    İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://impl.workday.com/<tenant>/login-saml.htmld`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve yanıt URL'si ile güncelleştirin. Yanıt URL'si, örneğin bir alt etki alanı olmalıdır: www, wd2, wd3, wd3 Impl, wd5 wd5 Impl).
    > Aşağıdaki gibi kullanarak "*http://www.myworkday.com*" çalışır, ancak "*http://myworkday.com*" yok. İlgili kişi [Workday istemci Destek ekibine](https://www.workday.com/en-us/partners-services/services/support.html) bu değerleri almak için.

5. Workday uygulama belirli bir biçimde SAML onaylamalarını bekler. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz **kullanıcı öznitelikleri** uygulama tümleştirme sayfasında bölümü. Aşağıdaki ekran görüntüsünde, bu yapılandırma için bir örnek gösterilmektedir.

    ![Çoklu oturum açmayı yapılandırın](./media/Workday-tutorial/tutorial_workday_attributes.png)

    > [!NOTE]
    > Burada şu varsayılan UPN (user.userprincipalname) ile ad kimliği eşlediğiniz. SSO, başarılı çalışma için gerçek kullanıcı kimliği, Workday hesabınızdaki (e-postanıza, UPN vb.) ad kimliği eşlemeniz gerekir.

6. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/workday-tutorial/tutorial_workday_certificate.png)

7. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/workday-tutorial/tutorial_general_400.png)

8. Üzerinde **Workday yapılandırma** bölümünde **yapılandırma Workday** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Workday yapılandırma](./media/workday-tutorial/tutorial_workday_configure.png)

9. Farklı bir web tarayıcı penceresinde Workday'e şirketinizin sitesi için bir yönetici olarak oturum açın.

10. İçinde **arama kutusuna** adlı arama **Kiracı kurulumunu Düzenle – güvenlik** tarafı giriş sayfasının sol üstte.

    ![Kiracı Güvenliği Düzenle](./media/workday-tutorial/IC782925.png "Kiracı güvenlik Düzenle")

11. İçinde **yeniden yönlendirme URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Yeniden yönlendirme URL'leri](./media/workday-tutorial/IC7829581.png "yeniden yönlendirme URL'leri")

    a. Tıklayın **satır**.

    b. İçinde **oturum açma yeniden yönlendirme URL'si** metin kutusu ve **mobil yeniden yönlendirme URL'si** metin kutusuna **oturum açma URL'si** girmiş olduğunuz **Workday etki alanı ve URL'ler** Azure portal'ın bölümü.

    c. Azure portalında, üzerinde **yapılandırma oturum açma** penceresinde, kopyalama **oturum kapatma URL'si**, ardından yapıştırın **oturum kapatma yönlendirme URL'sini** metin.

    d. İçinde **ortamlar için kullanılan** metin ortam adı seçin.  

    >[!NOTE]
    > Ortam özniteliğinin değeri, Kiracı URL'si değerine bağlıdır:  
    >-Workday kiracısı URL'si etki alanı adı ile Impl örneğin başlayıp başlamadığını: *https://impl.workday.com/\<tenant\>/login-saml2.htmld*), **ortam** özniteliği, uygulama için ayarlanmış olması gerekir.  
    >-Etki alanı adı başka bir şey ile başlar, iletişime geçmeniz [Workday istemci Destek ekibine](https://www.workday.com/en-us/partners-services/services/support.html) eşleşen almak için **ortam** değeri.

12. İçinde **SAML Kurulumu** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![SAML Kurulumu](./media/workday-tutorial/IC782926.png "SAML Kurulumu")

    a.  Seçin **SAML kimlik doğrulamasını etkinleştirme**.

    b.  Tıklayın **satır**.

13. İçinde **SAML kimlik sağlayıcısı** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![SAML kimlik sağlayıcısı](./media/workday-tutorial/IC7829271.png "SAML kimlik sağlayıcıları")

    a. İçinde **kimlik sağlayıcı adı** metin kutusuna sağlayıcı adını yazın (örneğin: *SPInitiatedSSO*).

    b. Azure portalında, üzerinde **yapılandırma oturum açma** penceresinde, kopyalama **SAML varlık kimliği** değeri ve ardından yapıştırın **veren** metin.

    ![SAML kimlik sağlayıcısı](./media/workday-tutorial/IC7829272.png "SAML kimlik sağlayıcıları")

    c. Azure portalında, üzerinde **yapılandırma oturum açma** penceresinde, kopyalama **oturum kapatma URL'si** değeri ve ardından yapıştırın **oturum kapatma yanıt URL'si** metin.

    d. Azure portalında, üzerinde **yapılandırma oturum açma** penceresinde, kopyalama **SAML çoklu oturum açma hizmeti URL'si** değeri ve ardından yapıştırın **IDP SSO hizmet URL'si** metin.

    e. İçinde **ortamlar için kullanılan** metin ortam adı seçin.

    f. Tıklayın **kimlik sağlayıcısı ortak anahtar sertifikası**ve ardından **Oluştur**.

    ![Oluşturma](./media/workday-tutorial/IC782928.png "oluşturma")

    g. Tıklayın **x509 oluşturma ortak anahtar**.

    ![Oluşturma](./media/workday-tutorial/IC782929.png "oluşturma")

14. İçinde **görünümü x509 ortak anahtar** bölümünde, aşağıdaki adımları gerçekleştirin:

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

15. Aşağıdaki adımları gerçekleştirin:

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

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/workday-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/workday-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/workday-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/workday-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-workday-test-user"></a>Workday test kullanıcısı oluşturma

Bu bölümde, Workday'de Britta Simon adlı bir kullanıcı oluşturun. Çalışmak [Workday istemci Destek ekibine](https://www.workday.com/en-us/partners-services/services/support.html) Workday platform kullanıcıları eklemek için. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi. 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Workday erişim vererek Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon için Workday atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **Workday**.

    ![Uygulamalar listesini Workday bağlantıdaki](./media/workday-tutorial/tutorial_workday_app.png)  

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Workday kutucuğa tıkladığınızda, otomatik olarak Workday uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)


<!--Image references-->

[1]: ./media/workday-tutorial/tutorial_general_01.png
[2]: ./media/workday-tutorial/tutorial_general_02.png
[3]: ./media/workday-tutorial/tutorial_general_03.png
[4]: ./media/workday-tutorial/tutorial_general_04.png

[100]: ./media/workday-tutorial/tutorial_general_100.png

[200]: ./media/workday-tutorial/tutorial_general_200.png
[201]: ./media/workday-tutorial/tutorial_general_201.png
[202]: ./media/workday-tutorial/tutorial_general_202.png
[203]: ./media/workday-tutorial/tutorial_general_203.png
