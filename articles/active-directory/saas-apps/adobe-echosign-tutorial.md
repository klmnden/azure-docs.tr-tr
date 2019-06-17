---
title: "Öğretici: Adobe Sign ile'Azure Active Directory Tümleştirme | Microsoft Docs"
description: Azure Active Directory ve Adobe Sign arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: f9385723-8fe7-4340-8afb-1508dac3e92b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/19/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: bcb27e24e9b53b734a24304a63c8fd91d5e94f5d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67107341"
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-sign"></a>Öğretici: Adobe Sign ile'Azure Active Directory Tümleştirme

Bu öğreticide, Adobe Sign'ı Azure Active Directory (Azure AD) ile tümleştirme konusunda bilgi edinin.
Adobe Sign'ı Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* Adobe Sign erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak (çoklu oturum açma) için Adobe oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Adobe Sign yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Adobe Sign tek oturum açma etkin abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Adobe Sign'ı destekleyen **SP** tarafından başlatılan

## <a name="adding-adobe-sign-from-the-gallery"></a>Adobe Sign galeri ekleme

Azure AD'de Adobe Sign tümleştirmesini yapılandırmak için Adobe Sign Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Adobe Sign'ı eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Adobe Sign**seçin **Adobe Sign** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde Adobe oturum](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Adobe Sign'adlı bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ve Adobe Sign ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Adobe Sign ile'test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Adobe oturum çoklu oturum açmayı yapılandırma](#configure-adobe-sign-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Adobe Sign test kullanıcısı oluşturma](#create-adobe-sign-test-user)**  - kullanıcı Azure AD gösterimini bağlı Adobe Sign Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile Adobe Sign yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Adobe Sign** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Adobe Sign etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<companyname>.echosign.com/`

    b. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<companyname>.echosign.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL ve tanımlayıcıdır ile güncelleştirin. İlgili kişi [Adobe oturum istemci Destek ekibine](https://helpx.adobe.com/in/contact/support.html) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

4. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. Üzerinde **Adobe Sign ' ayarlamak** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-adobe-sign-single-sign-on"></a>Adobe oturum çoklu oturum açmayı yapılandırın

1. Yapılandırma önce başvurun [Adobe oturum istemci Destek ekibine](https://helpx.adobe.com/in/contact/support.html) eklemek için Adobe Sign etki alanınızda izin listesinde. Etki alanı ekleme şöyledir:

    a. [Adobe oturum istemci Destek ekibine](https://helpx.adobe.com/in/contact/support.html) rastgele oluşturulmuş bir belirteç gönderir. Etki alanınız için belirteci aşağıdaki gibi olacaktır: **adobe oturum doğrulama xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx =**

    b. Doğrulama belirteci DNS metin kaydı yayımlama ve bildirim [Adobe oturum istemci Destek ekibine](https://helpx.adobe.com/in/contact/support.html).
    
    > [!NOTE]
    > Bu birkaç gün veya daha uzun sürebilir. Bir saat veya daha fazla bilgi için bir değer DNS'de yayımlanan DNS yayma gecikmeleri anlamına Not görünmeyebilir. BT yöneticiniz bu belirteci DNS metin kaydı yayımlama hakkında bilgi sahibi olması gerekir.
    
    c. Ne zaman size bildirim [Adobe oturum istemci Destek ekibine](https://helpx.adobe.com/in/contact/support.html) üzerinden destek bileti belirteç yayımlandıktan sonra etki alanını doğrulama ve hesabınıza ekleyin.
    
    d. Genellikle, bir DNS kaydı belirteçte yayımlama şöyledir:

    * Etki alanı hesabınızda oturum açın
    * DNS kaydı güncelleştirmek için sayfayı bulun. Bu sayfada, DNS Yönetimi, ad sunucusu yönetimi veya Gelişmiş ayarları çağrılabilir.
    * TXT kayıtlarının, etki alanınız için bulun.
    * Adobe tarafından sağlanan tam belirteç değeri içeren bir TXT kaydı ekleyin.
    * Yaptığınız değişiklikleri kaydedin.

1. Farklı bir web tarayıcı penceresinde bir Adobe Sign şirketinizin sitesi için bir yönetici olarak oturum açın.

1. SAML menüde **hesap ayarları** > **SAML ayarlarını**.
   
    ![Ekran Adobe oturum SAML Ayarları sayfası](./media/adobe-echosign-tutorial/ic789520.png "hesabı")

1. İçinde **SAML ayarlarını** bölümünde, aşağıdaki adımları gerçekleştirin:
  
   ![SAML ayarlarını görüntüsü](./media/adobe-echosign-tutorial/ic789521.png "SAML ayarları")
   
   ![SAML ayarlarını görüntüsü](./media/adobe-echosign-tutorial/ic789522.png "SAML ayarları")

   a. Altında **SAML modu**seçin **SAML zorunlu**.
   
   b. Seçin **izin Echosign hesap yöneticileri Echosign kimlik bilgilerini kullanarak oturum açması**.
   
   c. Altında **kullanıcı oluşturma**seçin **SAML ile kimliği doğrulanmış kullanıcılar otomatik olarak Ekle**.

   d. Yapıştırma **Azure Ad tanımlayıcısı**, hangi Azure portaldan kopyaladığınız **IDP varlık kimliği** metin kutusu.
    
   e. Yapıştırma **oturum açma URL'si**, Azure portalından kopyalanan **Idp'nin oturum açma URL'si** metin kutusu.
   
   f. Yapıştırma **oturum kapatma URL'si**, hangi Azure portaldan kopyaladığınız **IDP oturum kapatma URL'si** metin kutusu.

   g. İndirilen açın **Certificate(Base64)** dosyasını Not Defteri'nde. İçeriğini sizin panoya kopyalayın ve yapıştırın kendisine **IDP sertifika** metin kutusu.

   h. Seçin **değişiklikleri kaydetmek**.

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

Bu bölümde, Azure çoklu oturum açma kullanmak için Adobe Sign erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Adobe Sign**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde yazın ve **Adobe Sign**.

    ![Uygulamalar listesini Adobe Sign bağlantıdaki](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-adobe-sign-test-user"></a>Adobe Sign test kullanıcısı oluşturma

Adobe Sign'için oturum açmak Azure AD kullanıcılarının etkinleştirmek için Adobe oturum sağlanması gerekir. Bu el ile gerçekleştirilen bir görevdir.

>[!NOTE]
>Azure AD kullanıcı hesapları sağlamak için herhangi bir Adobe Sign kullanıcı hesabı oluşturma araçları veya Adobe Sign tarafından sağlanan API'leri kullanabilirsiniz. 

1. Oturum açın, **Adobe Sign** yönetici olarak şirketin site.

2. Üst menüde **hesabı**. Sol bölmede, ardından **kullanıcıları ve grupları** > **yeni kullanıcı oluşturma**.
   
    ![Adobe Sign ekran şirket hesabı, kullanıcıları ve grupları, site ve vurgulanmış yeni kullanıcı oluşturma](./media/adobe-echosign-tutorial/ic789524.png "hesabı")
   
3. İçinde **yeni kullanıcı oluşturma** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Ekran görüntüsü, oluşturma yeni kullanıcı bölümünün](./media/adobe-echosign-tutorial/ic789525.png "kullanıcı oluştur")
   
    a. Tür **e-posta adresi**, **ad**, ve **Soyadı** geçerli bir Azure AD hesabı ilgili metin kutularına sağlamak istediğiniz.
   
    b. Seçin **oluşturacağı**.

>[!NOTE]
>Azure Active Directory hesap sahibinin önce etkin hale gelir ve hesabı onaylamak için bir bağlantı içeren bir e-posta alır. 

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Adobe Sign kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama Adobe Sign için'oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

