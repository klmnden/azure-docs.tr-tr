---
title: 'Öğretici: Microsoft tarafından JIRA SAML SSO ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Microsoft tarafından JIRA SAML SSO arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 4b663047-7f88-443b-97bd-54224b232815
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/10/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9a0911588141552e616e8555380b14c910225840
ms.sourcegitcommit: 41015688dc94593fd9662a7f0ba0e72f044915d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/11/2019
ms.locfileid: "59501401"
---
# <a name="tutorial-azure-active-directory-integration-with-jira-saml-sso-by-microsoft"></a>Öğretici: Microsoft tarafından JIRA SAML SSO ile Azure Active Directory Tümleştirme

Bu öğreticide, JIRA SAML SSO Microsoft tarafından Azure Active Directory (Azure AD) ile tümleştirmeyi öğrenin.
Microsoft tarafından JIRA SAML SSO, Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* Microsoft tarafından JIRA SAML SSO erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak JIRA SAML SSO (çoklu oturum açma) Microsoft tarafından oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="description"></a>Açıklama

Microsoft Azure Active Directory hesabınız Atlassian JIRA sunucusu ile çoklu oturum açmayı etkinleştirmek için kullanın. Bu şekilde tüm kuruluş kullanıcıları JIRA uygulamasına oturum açma için Azure AD kimlik bilgilerini kullanabilirsiniz. Bu eklenti, Federasyon için SAML 2.0 kullanır.

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Microsoft tarafından JIRA SAML SSO ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- JIRA çekirdek 7.12 yazılım 6.0 veya JIRA hizmet Masası 3.0, 3.5 için yüklü ve yapılandırılmış Windows 64-bit sürümü
- HTTPS etkin JIRA sunucusudur
- JIRA eklentisi için desteklenen sürümler bölümü belirtilmiştir unutmayın.
- JIRA sunucu kimlik doğrulaması için Azure AD oturum açma sayfasına özellikle Internet üzerinden erişilebilir olduğundan ve Azure AD'den belirteç alamaz gerekir
- Yönetici kimlik bilgileri JIRA'da ayarlanmıştır
- JIRA'da WebSudo devre dışı bırakıldı
- JIRA sunucu uygulamasında oluşturulan bir test kullanıcısı

> [!NOTE]
> Bu öğreticideki adımları test etmek için bir üretim ortamına JIRA kullanarak önermiyoruz. Tümleşik geliştirme ya da hazırlık ortamı uygulamanın önce test ve üretim ortamına kullanın.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme burada alabilirsiniz: [Deneme teklifi](https://azure.microsoft.com/pricing/free-trial/).

## <a name="supported-versions-of-jira"></a>JIRA desteklenen sürümleri

* JIRA çekirdek ve yazılım: 6.0 için 7.12
* JIRA hizmet Masası 3.0.0 için 3.5.0
* JIRA 5.2 da destekler. Daha fazla bilgi için tıklayın [Microsoft Azure Active Directory çoklu oturum açma için JIRA 5.2](jira52microsoft-tutorial.md)

> [!NOTE]
> Lütfen JIRA Linux Ubuntu 16.04 sürümünü desteklediğini unutmayın.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Microsoft tarafından JIRA SAML SSO destekler **SP** tarafından başlatılan

## <a name="adding-jira-saml-sso-by-microsoft-from-the-gallery"></a>Galeriden Microsoft tarafından JIRA SAML SSO ekleme

Azure AD'de Microsoft tarafından JIRA SAML SSO tümleştirmesini yapılandırmak için Microsoft tarafından JIRA SAML SSO Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Microsoft tarafından JIRA SAML SSO eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **JIRA SAML SSO Microsoft tarafından**seçin **JIRA SAML SSO Microsoft tarafından** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde Microsoft tarafından JIRA SAML SSO](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma ile JIRA SAML SSO adlı bir test kullanıcı tabanlı Microsoft tarafından test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ve Microsoft tarafından JIRA SAML SSO ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Microsoft tarafından JIRA SAML SSO ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[JIRA SAML SSO tarafından Microsoft Çoklu oturum açmayı yapılandırma](#configure-jira-saml-sso-by-microsoft-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Microsoft test kullanıcısı tarafından JIRA SAML SSO oluşturma](#create-jira-saml-sso-by-microsoft-test-user)**  - JIRA SAML SSO kullanıcı Azure AD gösterimini bağlantılı Microsoft tarafından bir karşılığı Britta simon'un sağlamak için.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Microsoft tarafından JIRA SAML SSO ile Azure AD çoklu oturum açmayı yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **JIRA SAML SSO Microsoft tarafından** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![JIRA SAML SSO bilgileriyle Microsoft Domain ve URL'leri tek oturum açma](common/sp-identifier-reply.png)

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<domain:port>/plugins/servlet/saml/auth`

    b. İçinde **tanımlayıcı** kutusuna şu biçimi kullanarak bir URL yazın: `https://<domain:port>/`

    c. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<domain:port>/plugins/servlet/saml/auth`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Adlandırılmış bir URL olması durumunda bağlantı noktası isteğe bağlıdır. Bu değerler, öğreticinin ilerleyen bölümlerinde açıklanan Jıra eklentisi, yapılandırma sırasında alınır.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında **SAML imzalama sertifikası** bölümünde, kopyalamak için Kopyala düğmesine **uygulama Federasyon meta verileri URL'sini** ve üzerinde kaydedin, bilgisayar.

    ![Sertifika indirme bağlantısı](common/copy-metadataurl.png)

### <a name="configure-jira-saml-sso-by-microsoft-single-sign-on"></a>JIRA SAML SSO tarafından Microsoft Çoklu oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde bir JIRA Örneğiniz için bir yönetici olarak oturum açın.

2. Dişlisine gelin ve tıklayın **eklentileri**.

    ![Çoklu oturum açmayı yapılandırın](./media/jiramicrosoft-tutorial/addon1.png)

3. Eklentisini indirin [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=56506). Microsoft tarafından sağlanan eklentisini el ile karşıya **karşıya yükleme, eklenti** menüsü. Eklenti indirilmesini altında ele [Microsoft hizmet sözleşmesi](https://www.microsoft.com/servicesagreement/).

    ![Çoklu oturum açmayı yapılandırın](./media/jiramicrosoft-tutorial/addon12.png)

4. JIRA ters proxy senaryonuz ya da yük dengeleyici senaryosu çalıştırmak için aşağıdaki adımları gerçekleştirin:

    > [!NOTE]
    > Sunucu ilk ile yapılandırdığınız aşağıdaki yönergeleri ve eklentiyi yükleyin.

    a. Aşağıdaki özniteliği ekleyin **bağlayıcı** içinde bağlantı noktası **server.xml** JIRA sunucu uygulaması, dosya.

    `scheme="https" proxyName="<subdomain.domain.com>" proxyPort="<proxy_port>" secure="true"`

    ![Çoklu oturum açmayı yapılandırın](./media/jiramicrosoft-tutorial/reverseproxy1.png)

    b. Değişiklik **temel URL** içinde **sistem ayarlarını** proxy/yük dengeleyici göre.

    ![Çoklu oturum açmayı yapılandırın](./media/jiramicrosoft-tutorial/reverseproxy2.png)

5. Eklentiyi yükledikten sonra görünür **kullanıcı yüklü** eklentileri bölümünü **yönetme eklenti** bölümü. Tıklayın **yapılandırma** yeni eklentiyi yapılandırmak için.

    ![Çoklu oturum açmayı yapılandırın](./media/jiramicrosoft-tutorial/addon13.png)

6. Yapılandırma sayfasında aşağıdaki adımları uygulayın:

    ![Çoklu oturum açmayı yapılandırın](./media/jiramicrosoft-tutorial/addon52.png)

    > [!TIP]
    > Böylece hata meta veriler çözümlenmesinde uygulamayı karşı eşlenen yalnızca bir sertifika olduğundan emin olun. Yönetici, meta veri çözümleme sırasında birden çok sertifika varsa bir hata alır.

    a. İçinde **meta veri URL'si** metin kutusu, yapıştırma **uygulama Federasyon meta verileri URL'sini** Azure portal'ı seçin ve kopyaladığınız değeri **çözmek** düğmesi. IDP meta veri URL'sini okur ve tüm alanları bilgileri doldurur.

    b. Kopyalama **tanımlayıcısı, yanıt URL'si ve oturum açma URL'si** değerleri ve bunları yapıştırın **tanımlayıcısı, yanıt URL'si ve oturum açma URL'si** sırasıyla içinde metin kutuları **Microsoft Domain ve URL'leriJIRASAMLSSO** bölümü Azure portalı.

    c. İçinde **oturum açma düğmesi adı** kuruluşunuzun oturum açma ekranında kullanıcıların istediği düğme adı yazın.

    d. İçinde **SAML kullanıcı kimliği konumları** seçin **kullanıcı kimliğidir konu deyiminin NameIdentifier öğesinde** veya **kullanıcı kimliği olup öznitelik öğe**.  Bu kimliği JIRA kullanıcı kimliği olması gerekir Kullanıcı Kimliği eşleşmiyorsa, sonra sistem oturum açmasına izin vermez.

    > [!Note]
    > Varsayılan kullanıcı kimliği SAML ad tanımlayıcısı konumdur. Bu öznitelik seçeneği değiştirin ve uygun öznitelik adını girin.

    e. Seçerseniz **kullanıcı kimliği olup öznitelik öğe** seçeneği, ardından **öznitelik adı** metin kullanıcı kimliği beklenirken özniteliğin adını yazın.

    f. Azure AD ile Federasyon etki alanını (örneğin, ADFS vb.) kullanıyorsanız, ardından **giriş bölgesi bulmayı etkinleştirmek** seçeneği ve yapılandırma **etki alanı adı**.

    g. İçinde **etki alanı adı** ADFS tabanlı oturum açma durumunda burada etki alanı adını yazın.

    h. Denetleme **etkinleştirme çoklu oturum kapatma** ne zaman bir kullanıcının oturumunu kapatmak JIRA Azure AD oturumunu kapatmak istediğinizde.

    i. Tıklayın **Kaydet** düğmesini kullanarak ayarları kaydedin.

    > [!NOTE]
    > Yükleme ve sorun giderme hakkında daha fazla bilgi için ziyaret [MS JIRA SSO Bağlayıcısı yönetici kılavuzundaki](../ms-confluence-jira-plugin-adminguide.md) ayrıca [SSS](../ms-confluence-jira-plugin-faq.md) , Yardım almak için

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü `brittasimon\@yourcompanydomain.extension`. Örneğin, BrittaSimon@contoso.com.

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Microsoft tarafından JIRA SAML SSO için erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **JIRA SAML SSO Microsoft tarafından**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **JIRA SAML SSO Microsoft tarafından**.

    ![Uygulamalar listesinde Microsoft bağlantısıyla JIRA SAML SSO](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-jira-saml-sso-by-microsoft-test-user"></a>JIRA SAML SSO tarafından Microsoft test kullanıcısı oluşturma

JIRA şirket içi sunucuya oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunlar JIRA SAML SSO Microsoft tarafından sağlanması gerekir. Microsoft tarafından JIRA SAML SSO için sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. JIRA şirket içi sunucunuza yönetici olarak oturum açın.

2. Dişlisine gelin ve tıklayın **kullanıcı yönetimi**.

    ![Çalışan Ekle](./media/jiramicrosoft-tutorial/user1.png)

3. Girmek için yönetici erişimi sayfasına yönlendirilirsiniz **parola** tıklatıp **Onayla** düğmesi.

    ![Çalışan Ekle](./media/jiramicrosoft-tutorial/user2.png)

4. Altında **kullanıcı yönetimi** sekmesinde bölüm **oluşturacağı**.

    ![Çalışan Ekle](./media/jiramicrosoft-tutorial/user3.png) 

5. Üzerinde **"Yeni kullanıcı oluşturma"** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Çalışan Ekle](./media/jiramicrosoft-tutorial/user4.png) 

    a. İçinde **e-posta adresi** metin kutusuna kullanıcı e-posta adresi türünü ister Brittasimon@contoso.com.

    b. İçinde **tam adı** metin Britta Simon gibi kullanıcının tam adını yazın.

    c. İçinde **kullanıcıadı** metin kutusuna kullanıcı e-posta türünü ister Brittasimon@contoso.com.

    d. İçinde **parola** metin kutusu, kullanıcı parolasını yazın.

    e. Tıklayın **kullanıcı oluşturma**.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

JIRA SAML SSO erişim Paneli'nde Microsoft kutucuk tarafından'ye tıkladığınızda, otomatik olarak JIRA SAML SSO için SSO'yu ayarlama Microsoft tarafından oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
