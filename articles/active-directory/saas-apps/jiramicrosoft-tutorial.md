---
title: 'Öğretici: Azure Active Directory Tümleştirme Microsoft tarafından JIRA SAML SSO ile | Microsoft Docs'
description: Azure Active Directory ve Microsoft tarafından JIRA SAML SSO arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 4b663047-7f88-443b-97bd-54224b232815
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/22/2018
ms.author: jeedes
ms.openlocfilehash: 608269a05ae1ed699954cd301aa03056e089fa8a
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2018
ms.locfileid: "52426110"
---
# <a name="tutorial-azure-active-directory-integration-with-jira-saml-sso-by-microsoft"></a>Öğretici: Azure Active Directory Tümleştirme ile Microsoft tarafından JIRA SAML SSO

Bu öğreticide, JIRA SAML SSO Microsoft tarafından Azure Active Directory (Azure AD) ile tümleştirmeyi öğrenin.

Microsoft tarafından JIRA SAML SSO, Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Microsoft tarafından JIRA SAML SSO erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan JIRA SAML SSO (çoklu oturum açma) Microsoft tarafından açık, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md)

## <a name="description"></a>Açıklama

Microsoft Azure Active Directory hesabınız Atlassian JIRA sunucusu ile çoklu oturum açmayı etkinleştirmek için kullanın. Bu şekilde tüm kuruluş kullanıcıları Azure AD kimlik JIRA uygulamasına oturum açmak için kullanabilirsiniz. Bu eklenti, Federasyon için SAML 2.0 kullanır.

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
- Azure AD deneme ortamı yoksa, bir aylık deneme burada alabilirsiniz: [deneme teklifi](https://azure.microsoft.com/pricing/free-trial/).

## <a name="supported-versions-of-jira"></a>JIRA desteklenen sürümleri

* JIRA çekirdek ve yazılım: 6.0 için 7.12
* JIRA hizmet Masası 3.0.0 için 3.5.0
* JIRA 5.2 da destekler. Daha fazla bilgi için tıklayın [Microsoft Azure Active Directory çoklu oturum açma için JIRA 5.2](jira52microsoft-tutorial.md)

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin.
Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Microsoft tarafından JIRA SAML SSO ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-jira-saml-sso-by-microsoft-from-the-gallery"></a>Galeriden Microsoft tarafından JIRA SAML SSO ekleme

Azure AD'de Microsoft tarafından JIRA SAML SSO tümleştirmesini yapılandırmak için Microsoft tarafından JIRA SAML SSO Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Microsoft tarafından JIRA SAML SSO eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **JIRA SAML SSO Microsoft tarafından**seçin **JIRA SAML SSO Microsoft tarafından** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Microsoft tarafından JIRA SAML SSO](./media/jiramicrosoft-tutorial/tutorial_singlesign-onforjira_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcıya dayanarak Microsoft tarafından JIRA SAML SSO ile test edin.

Tek iş için oturum açma için Azure AD ne JIRA SAML SSO Microsoft gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve Microsoft tarafından JIRA SAML SSO ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Microsoft tarafından JIRA SAML SSO ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Microsoft test kullanıcısı tarafından JIRA SAML SSO oluşturma](#creating-jira-saml-sso-by-microsoft-test-user)**  - JIRA SAML SSO kullanıcı Azure AD gösterimini bağlantılı Microsoft tarafından bir karşılığı Britta simon'un sağlamak için.
4. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma, JIRA SAML SSO, Microsoft uygulama tarafından yapılandırın.

**Microsoft tarafından JIRA SAML SSO ile Azure AD çoklu oturum açmayı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **JIRA SAML SSO Microsoft tarafından** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunu tıklatın **seçin** için **SAML** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açmayı yapılandırın](common/tutorial_general_301.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Çoklu oturum açmayı yapılandırın](common/editconfigure.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![JIRA SAML SSO bilgileriyle Microsoft Domain ve URL'leri tek oturum açma](./media/jiramicrosoft-tutorial/tutorial_singlesign-onforjira_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<domain:port>/plugins/servlet/saml/auth`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<domain:port>/`

    c. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<domain:port>/plugins/servlet/saml/auth`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Adlandırılmış bir URL olması durumunda bağlantı noktası isteğe bağlıdır. Bu değerler, öğreticinin ilerleyen bölümlerinde açıklanan Jıra eklentisi, yapılandırma sırasında alınır.

5. Üzerinde **SAML imzalama sertifikası** sayfasında **SAML imzalama sertifikası** bölümünde, kopyalamak için Kopyala düğmesine **uygulama Federasyon meta verileri URL'sini** kopyalayıp Not Defteri'ne yapıştırın.

    ![Sertifika indirme bağlantısı](./media/jiramicrosoft-tutorial/tutorial_metadataurl.png) 

6. Farklı bir web tarayıcı penceresinde bir JIRA Örneğiniz için bir yönetici olarak oturum açın.

7. Dişlisine gelin ve tıklayın **eklentileri**.

    ![Çoklu oturum açmayı yapılandırın](./media/jiramicrosoft-tutorial/addon1.png)

8. Eklentisini indirin [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=56506). Microsoft tarafından sağlanan eklentisini el ile karşıya **karşıya yükleme, eklenti** menüsü. Eklenti indirilmesini altında ele [Microsoft hizmet sözleşmesi](https://www.microsoft.com/servicesagreement/).

    ![Çoklu oturum açmayı yapılandırın](./media/jiramicrosoft-tutorial/addon12.png)

9. JIRA ters proxy senaryonuz ya da yük dengeleyici senaryosu çalıştırmak için aşağıdaki adımları gerçekleştirin:

    > [!NOTE]
    > Sunucu ilk ile yapılandırdığınız aşağıdaki yönergeleri ve eklentiyi yükleyin.

    a. Aşağıdaki özniteliği ekleyin **bağlayıcı** içinde bağlantı noktası **server.xml** JIRA sunucu uygulaması, dosya.

    `scheme="https" proxyName="<subdomain.domain.com>" proxyPort="<proxy_port>" secure="true"`

    ![Çoklu oturum açmayı yapılandırın](./media/jiramicrosoft-tutorial/reverseproxy1.png)

    b. Değişiklik **temel URL** içinde **sistem ayarlarını** proxy/yük dengeleyici göre.

    ![Çoklu oturum açmayı yapılandırın](./media/jiramicrosoft-tutorial/reverseproxy2.png)

10. Eklentiyi yükledikten sonra görünür **kullanıcı yüklü** eklentileri bölümünü **yönetme eklenti** bölümü. Tıklayın **yapılandırma** yeni eklentiyi yapılandırmak için.

    ![Çoklu oturum açmayı yapılandırın](./media/jiramicrosoft-tutorial/addon13.png)

11. Yapılandırma sayfasında aşağıdaki adımları uygulayın:

    ![Çoklu oturum açmayı yapılandırın](./media/jiramicrosoft-tutorial/addon52.png)

    > [!TIP]
    > Böylece hata meta veriler çözümlenmesinde uygulamayı karşı eşlenen yalnızca bir sertifika olduğundan emin olun. Yönetici, meta veri çözümleme sırasında birden çok sertifika varsa bir hata alır.

    a. İçinde **meta veri URL'si** metin kutusu, yapıştırma **uygulama Federasyon meta verileri URL'sini** Azure portal'ı seçin ve kopyaladığınız değeri **çözmek** düğmesi. IDP meta veri URL'sini okur ve tüm alanları bilgileri doldurur.

    b. Kopyalama **tanımlayıcısı, yanıt URL'si ve oturum açma URL'si** değerleri ve bunları yapıştırın **tanımlayıcısı, yanıt URL'si ve oturum açma URL'si** sırasıyla içinde metin kutuları **Microsoft Domain ve URL'leriJIRASAMLSSO** bölümü Azure portalı.

    c. İçinde **oturum açma düğmesi adı** kuruluşunuzun oturum açma ekranında kullanıcıların istediği düğme adı yazın.

    d. İçinde **SAML kullanıcı kimliği konumları** seçin **kullanıcı kimliğidir konu deyiminin NameIdentifier öğesinde** veya **kullanıcı kimliği olup öznitelik öğe**.  Bu kimliği JIRA kullanıcı kimliği olması gerekir. Kullanıcı Kimliği eşleşmiyorsa, sonra sistem oturum açmasına izin vermez.

    > [!Note]
    > Varsayılan kullanıcı kimliği SAML ad tanımlayıcısı konumdur. Bu öznitelik seçeneği değiştirin ve uygun öznitelik adını girin.

    e. Seçerseniz **kullanıcı kimliği olup öznitelik öğe** seçeneği, ardından **öznitelik adı** metin kullanıcı kimliği beklenirken özniteliğin adını yazın.

    f. Azure AD ile Federasyon etki alanını (örneğin, ADFS vb.) kullanıyorsanız, ardından **giriş bölgesi bulmayı etkinleştirmek** seçeneği ve yapılandırma **etki alanı adı**.

    g. İçinde **etki alanı adı** ADFS tabanlı oturum açma durumunda burada etki alanı adını yazın.

    h. Denetleme **etkinleştirme çoklu oturum kapatma** Azure AD'den bir kullanıcı oturum açtığında JIRA oturumunuzu kapatmak istiyor.

    i. Tıklayın **Kaydet** düğmesini kullanarak ayarları kaydedin.

    > [!NOTE]
    > Yükleme ve sorun giderme hakkında daha fazla bilgi için ziyaret [MS JIRA SSO Bağlayıcısı yönetici kılavuzundaki](../ms-confluence-jira-plugin-adminguide.md) ayrıca [SSS](../ms-confluence-jira-plugin-faq.md) , Yardım almak için

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    ![Azure AD kullanıcısı oluşturun][100]

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Bir Azure AD test kullanıcısı oluşturma](common/create_aaduser_01.png) 

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Bir Azure AD test kullanıcısı oluşturma](common/create_aaduser_02.png)

    a. İçinde **adı** alanına **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alanına **brittasimon@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **özellikleri**seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’u seçin.

### <a name="creating-jira-saml-sso-by-microsoft-test-user"></a>JIRA SAML SSO tarafından Microsoft test kullanıcısı oluşturma

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

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Microsoft tarafından JIRA SAML SSO için erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**.

    ![Kullanıcı Ata][201]

2. Uygulamalar listesinde **JIRA SAML SSO Microsoft tarafından**.

    ![Çoklu oturum açmayı yapılandırın](./media/jiramicrosoft-tutorial/tutorial_singlesign-onforjira_app.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. İçinde **atama Ekle** iletişim kutusunda **atama** düğmesi.

### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

JIRA SAML SSO erişim Paneli'nde Microsoft kutucuk tarafından'ye tıkladığınızda, otomatik olarak, JIRA SAML SSO için Microsoft uygulama tarafından açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: common/tutorial_general_01.png
[2]: common/tutorial_general_02.png
[3]: common/tutorial_general_03.png
[4]: common/tutorial_general_04.png

[100]: common/tutorial_general_100.png

[201]: common/tutorial_general_201.png
[202]: common/tutorial_general_202.png
[203]: common/tutorial_general_203.png
