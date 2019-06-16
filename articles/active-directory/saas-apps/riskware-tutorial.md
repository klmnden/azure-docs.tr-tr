---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Riskware | Microsoft Docs'
description: Azure Active Directory ve Riskware arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 81866167-b163-4695-8978-fd29a25dac7a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/25/2019
ms.author: jeedes
ms.openlocfilehash: b2bfbed33433521fd086d474ea4b754f5435f5e6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67092918"
---
# <a name="tutorial-azure-active-directory-integration-with-riskware"></a>Öğretici: Riskware ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Riskware tümleştirme konusunda bilgi edinin.
Azure AD ile Riskware tümleştirme ile aşağıdaki avantajları sağlar:

* Riskware erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak (çoklu oturum açma) Riskware için kendi Azure AD hesapları ile oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Riskware yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa alabileceğiniz bir [ücretsiz hesap](https://azure.microsoft.com/free/)
* Abonelik Riskware çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Riskware destekler **SP** tarafından başlatılan

## <a name="adding-riskware-from-the-gallery"></a>Galeriden Riskware ekleme

Azure AD'de Riskware tümleştirmesini yapılandırmak için Riskware Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Riskware eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Riskware**seçin **Riskware** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Riskware](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Riskware adlı bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının Riskware ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Riskware ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Riskware çoklu oturum açmayı yapılandırma](#configure-riskware-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Riskware test kullanıcısı oluşturma](#create-riskware-test-user)**  - kullanıcı Azure AD gösterimini bağlı Riskware Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile Riskware yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Riskware** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Riskware etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:
    
    | Ortam| URL deseni|
    |--|--|
    | UAT|  `https://riskcloud.net/uat?ccode=<COMPANYCODE>` |
    | ÜRÜN| `https://riskcloud.net/prod?ccode=<COMPANYCODE>` |
    | TANITIMI| `https://riskcloud.net/demo?ccode=<COMPANYCODE>` |
    |||

    b. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna URL'yi yazın:
    
    | Ortam| URL deseni|
    |--|--|
    | UAT| `https://riskcloud.net/uat` |
    | ÜRÜN| `https://riskcloud.net/prod` |
    | TANITIMI| `https://riskcloud.net/demo` |
    |||

    > [!NOTE]
    > Oturum açma URL değeri, gerçek değil. Değerini gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [Riskware istemci Destek ekibine](mailto:support@pansoftware.com.au) değeri alınamıyor. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. Üzerinde **Riskware kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-riskware-single-sign-on"></a>Riskware tek oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde Riskware şirketinizin sitesi için bir yönetici olarak oturum açın.

1. Sağ üst kısımdaki tıklayın **Bakım** bakım sayfasını açın.

    ![Riskware yapılandırmaları tutmanız](./media/riskware-tutorial/tutorial_riskware_maintain.png)

1. Bakım sayfasında tıklatın **kimlik doğrulaması**.

    ![Riskware yapılandırma authen](./media/riskware-tutorial/tutorial_riskware_authen.png)

1. İçinde **kimlik doğrulama Yapılandırması** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Riskware yapılandırma authenconfig](./media/riskware-tutorial/tutorial_riskware_config.png)

    a. Seçin **türü** olarak **SAML** kimlik doğrulaması için.

    b. İçinde **kod** metin AZURE_UAT gibi kod yazın.

    c. İçinde **açıklama** metin SSO için AZURE yapılandırma gibi bir açıklama yazın.

    d. İçinde **tek oturum açma sayfasına** metin kutusu, yapıştırma **oturum açma URL'si** Azure Portalı'ndan kopyaladığınız değeri.

    e. İçinde **sayfa oturum** metin kutusu, yapıştırma **oturum kapatma URL'si** Azure Portalı'ndan kopyaladığınız değeri.

    f. İçinde **Post Form alanı** metin SAMLResponse gibi SAML içeren bir gönderi yanıtı mevcut alan adını yazın

    g. İçinde **XML kimlik etiket adı** metin Nameıd gibi SAML yanıtını benzersiz tanımlayıcı içeren tür özniteliği.

    h. İndirilen açın **meta veri Xml** meta veri dosyası sertifikadan Defteri'nde Azure portalından kopyalayın ve yapıştırın **sertifika** metin kutusu

    i. İçinde **tüketici URL** metin değerini yapıştırın **yanıt URL'si**, hangi destek ekibinden alın.

    j. İçinde **veren** metin değerini yapıştırın **tanımlayıcı**, hangi destek ekibinden alın.

    > [!Note]
    > İlgili kişi [Riskware istemci Destek ekibine](mailto:support@pansoftware.com.au) bu değerleri almak için

    k. Seçin **kullanım POST** onay kutusu.

    m. Seçin **kullanım SAML isteği** onay kutusu.

    m. **Kaydet**’e tıklayın.

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

Bu bölümde, Azure çoklu oturum açma kullanmak için Riskware erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Riskware**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Riskware**.

    ![Uygulamalar listesinde Riskware bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-riskware-test-user"></a>Riskware test kullanıcısı oluşturma

Riskware için oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunların Riskware sağlanması gerekir. Riskware içinde sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. İçin Riskware bir güvenlik yöneticisi olarak oturum açın.

1. Sağ üst kısımdaki tıklayın **Bakım** bakım sayfasını açın. 

    ![Riskware yapılandırma tutar](./media/riskware-tutorial/tutorial_riskware_maintain.png)

1. Bakım sayfasında tıklatın **kişiler**.

    ![Riskware yapılandırma kişiler](./media/riskware-tutorial/tutorial_riskware_people.png)

1. Seçin **ayrıntıları** sekmesinde ve aşağıdaki adımları gerçekleştirin:

    ![Riskware yapılandırma ayrıntıları](./media/riskware-tutorial/tutorial_riskware_details.png)

    a. Seçin **kişi türü** çalışana gibi.

    b. İçinde **ad** metin gibi kullanıcı adını girin **Britta**.

    c. İçinde **Soyadı** metin gibi kullanıcının soyadını girin **Simon**.

1. Üzerinde **güvenlik** sekmesinde, aşağıdaki adımları gerçekleştirin:

    ![Güvenlik Riskware yapılandırma](./media/riskware-tutorial/tutorial_riskware_security.png)

    a. Altında **kimlik doğrulaması** bölümünden **kimlik doğrulaması** sahip Kurulum modunu SSO için AZURE yapılandırma ister.

    b. Altında **oturum açma ayrıntıları** bölümünde **kullanıcı kimliği** metin gibi kullanıcının e-posta girin `brittasimon@contoso.com`.

    c. İçinde **parola** metin kutusu, kullanıcının parolasını girin.

1. Üzerinde **kuruluş** sekmesinde, aşağıdaki adımları gerçekleştirin:

    ![Kuruluş Riskware yapılandırma](./media/riskware-tutorial/tutorial_riskware_org.png)

    a. Olarak seçeneğini **Level1** kuruluş.

    b. Altında **kişinin birincil çalışma alanı** bölümünde **konumu** metin konumunuz yazın.

    c. Altında **çalışan** bölümünden **çalışan durumu** normal ister.

    d. **Kaydet**’e tıklayın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Riskware kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama Riskware için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
