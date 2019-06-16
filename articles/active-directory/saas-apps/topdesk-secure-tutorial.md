---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile TOPdesk - güvenli | Microsoft Docs'
description: Azure Active Directory ve TOPdesk - güvenli arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 8e06ee33-18f9-4c05-9168-e6b162079d88
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/27/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: eded8eb446d36a321acf46231eee3e764ba41504
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67088441"
---
# <a name="tutorial-azure-active-directory-integration-with-topdesk---secure"></a>Öğretici: Azure Active Directory Tümleştirmesi ile TOPdesk - güvenli

Bu öğreticide, şunların nasıl tümleştireceğinizi TOPdesk - Azure Active Directory (Azure AD) ile güvenli hale getirme.
TOPdesk - güvenli tümleştirme ile Azure AD ile aşağıdaki avantajları sağlar:

* TOPdesk - güvenli erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak TOPdesk için - güvenli (çoklu oturum açma) ile Azure AD hesaplarına oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile TOPdesk - yapılandırmak için güvenli, aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* TOPdesk - aboneliği etkin güvenli çoklu oturum açma

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* TOPdesk - güvenli destekler **SP** tarafından başlatılan

## <a name="adding-topdesk---secure-from-the-gallery"></a>TOPdesk - ekleme Galeriden güvenliğini sağlama

Güvenli TOPdesk - tümleştirmesini yapılandırmak için Azure AD ile Galeriden yönetilen SaaS listenize uygulamalarının güvenliğini sağlama - TOPdesk eklemek için ihtiyacınız.

**TOPdesk - eklemek için galerideki güvenli, aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **TOPdesk - güvenli**seçin **TOPdesk - güvenli** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![TOPdesk - sonuçlar listesinde güvenliğini sağlama](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma TOPdesk ile test etme - güvenli adlı bir test kullanıcı tabanlı **Britta Simon**.
Tek çalışmak için oturum açma, bir bağlantı arasındaki ilişki bir Azure AD kullanıcısının TOPdesk - ilgili kullanıcı güvenli kurulması gerekiyor.

Yapılandırma ve Azure AD çoklu oturum açma TOPdesk ile-test etmek için güvenli, aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[-Güvenli çoklu oturum açma TOPdesk yapılandırma](#configure-topdesk---secure-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[TOPdesk - güvenli bir test kullanıcısı oluşturma](#create-topdesk---secure-test-user)**  - kullanıcı Azure AD gösterimini bağlı Britta simon'un TOPdesk - güvenli içinde bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma - TOPdesk ile yapılandırmak için güvenli, aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **TOPdesk - güvenli** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açma bilgileri TOPdesk - güvenli etki alanı ve URL'ler](common/sp-identifier-reply.png)

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<companyname>.topdesk.net`

    b. İçinde **tanımlayıcı** kutusuna şu biçimi kullanarak bir URL yazın: `https://<companyname>.topdesk.net/tas/secure/login/verify`

    c. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<companyname>.topdesk.net/tas/public/login/saml`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si, tanımlayıcı ve yanıt URL'si ile güncelleştirin. İlgili kişi [TOPdesk - güvenli istemci Destek ekibine](https://www.topdesk.com/us/support/) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. Üzerinde **TOPdesk - güvenli ayarlamak** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-topdesk---secure-single-sign-on"></a>TOPdesk yapılandırma - güvenli çoklu oturum açma

1. Oturum açın, **TOPdesk - güvenli** yönetici olarak şirketin site.

2. İçinde **TOPdesk** menüsünü tıklatın **ayarları**.

    ![Ayarları](./media/topdesk-secure-tutorial/ic790598.png "ayarları")

3. Tıklayın **oturum açma ayarları**.

    ![Oturum açma ayarları](./media/topdesk-secure-tutorial/ic790599.png "oturum açma ayarları")

4. Genişletin **oturum açma ayarları** menüsüne ve ardından **genel**.

    ![Genel](./media/topdesk-secure-tutorial/ic790600.png "genel")

5. İçinde **güvenli** bölümünü **SAML oturum açma** yapılandırma bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Teknik ayarlarla](./media/topdesk-secure-tutorial/ic790855.png "teknik ayarları")

    a. Tıklayın **indirme** ortak meta veri dosyası indirin ve bilgisayarınıza yerel olarak kaydedin.

    b. Meta veri dosyası açın ve ardından bulun **AssertionConsumerService** düğümü.

    ![Onaylama tüketici hizmeti](./media/topdesk-secure-tutorial/ic790856.png "onaylama tüketici hizmeti")

    c. Kopyalama **AssertionConsumerService** değeri, bu değer yanıt URL'si metin kutusuna yapıştırın **TOPdesk - güvenli etki alanı ve URL'ler** bölümü.

6. Bir sertifika dosyası oluşturmak için aşağıdaki adımları gerçekleştirin:

    ![Sertifika](./media/topdesk-secure-tutorial/ic790606.png "sertifika")

    a. Azure Portalı'ndan indirilen meta veri dosyası açın.

    b. Genişletin **verilerde Securitytokenservicetype** sahip düğüm bir **xsi: type** , **beslenir: ApplicationServiceType**.

    c. Değerini kopyalayın **X509Certificate** düğümü.

    d. Kopyalanan Kaydet **X509Certificate** yerel olarak bilgisayarınızda bir dosyadaki değeri.

7. İçinde **genel** bölümünde **Ekle**.

    ![Ekleme](./media/topdesk-secure-tutorial/ic790607.png "Ekle")

8. Üzerinde **SAML yapılandırma Yardımcısı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![SAML yapılandırma Yardımcısı](./media/topdesk-secure-tutorial/ic790608.png "SAML yapılandırma Yardımcısı")

    a. Azure portalından indirilen meta verileri dosyanızı altında karşıya yüklemek için **Federasyon meta verileri**, tıklayın **Gözat**.

    b. Altında sertifika dosyası karşıya **sertifika (RSA)** , tıklayın **Gözat**.

    c. İçin **özel anahtarı (RSA, PKCS8, DER)** , özel anahtarınızı karşıya yükleyebilirsiniz ya da iletişime geçebilirsiniz [TOPdesk - güvenli istemci Destek ekibine](https://www.topdesk.com/us/support) özel anahtarı alınamıyor.

    d. Aldığınız TOPdesk destek ekibinden altında logosu dosyayı karşıya yüklemeyi **logosu simgesi**, tıklayın **Gözat**.

    e. İçinde **kullanıcı adı özniteliği** metin kutusuna `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.

    f. İçinde **görünen adı** metin yapılandırmanız için bir ad yazın.

    g. **Kaydet**’e tıklayın.

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

Bu bölümde, Azure çoklu oturum açmayı kullanmak için TOPdesk - güvenli erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **TOPdesk - güvenli**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde yazın ve **TOPdesk - güvenli**.

    ![TOPdesk - uygulamalar listesinde güvenli bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-topdesk---secure-test-user"></a>TOPdesk - güvenli bir test kullanıcısı oluşturma

Azure AD kullanıcılarının TOPdesk - oturum etkinleştirmek için güvenli, bunların TOPdesk - güvenli sağlanması gerekir.  
Söz konusu olduğunda TOPdesk - sağlama elle bir görevin güvenlidir.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Oturum açın, **TOPdesk - güvenli** şirketinizin sitesi yöneticisi olarak.

2. Üstteki menüden **TOPdesk \> yeni \> destek dosyalarını \> işleci**.

    ![İşleç](./media/topdesk-secure-tutorial/ic790610.png "işleci")

3. Üzerinde **New işleci** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![New işleci](./media/topdesk-secure-tutorial/ic790611.png "New işleci")

    a. Tıklayın **genel** sekmesi.

    b. İçinde **Soyadı** kullanıcının Soyadı türü metin ister **Simon**.

    c. Seçin bir **Site** hesap **konumu** bölümü.

    d. İçinde **oturum açma adı** textbox'ın **TOPdesk oturum açma** bölümünde, bir kullanıcı için oturum açma adı yazın.

    e. **Kaydet**’e tıklayın.

> [!NOTE]
> Herhangi diğer TOPdesk - güvenli kullanıcı hesabı oluşturma araçları veya tarafından TOPdesk - AAD kullanıcı hesapları sağlamak için güvenli sağlanan API'leri kullanabilirsiniz.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

-Güvenli kutucuk erişim Paneli'nde TOPdesk tıklattığınızda, otomatik olarak SSO'yu ayarlama TOPdesk - güvenli şekilde oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

