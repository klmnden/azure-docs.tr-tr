---
title: 'Öğretici: Azure Active Directory Tümleştirme Microsoft tarafından Confluence SAML SSO ile | Microsoft Docs'
description: Azure Active Directory ve Microsoft tarafından Confluence SAML SSO arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 1ad1cf90-52bc-4b71-ab2b-9a5a1280fb2d
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/16/2018
ms.author: jeedes
ms.openlocfilehash: 68d8ba6b08811b96df8b8b2daa074166301ffcd0
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39422000"
---
# <a name="tutorial-azure-active-directory-integration-with-confluence-saml-sso-by-microsoft"></a>Öğretici: Azure Active Directory Tümleştirme ile Microsoft tarafından Confluence SAML SSO

Bu öğreticide, Microsoft tarafından Confluence SAML SSO, Azure Active Directory (Azure AD) ile tümleştirme konusunda bilgi edinin.

Microsoft tarafından Confluence SAML SSO, Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Microsoft tarafından Confluence SAML SSO erişimi, Azure AD'de denetleyebilirsiniz
- Azure AD hesaplarına otomatik olarak imzalanan Confluence SAML SSO (çoklu oturum açma) Microsoft tarafından açık, kullanıcılarınızın etkinleştirebilirsiniz
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="description"></a>Açıklama:

Microsoft Azure Active Directory hesabınız Atlassian Confluence sunucusu ile çoklu oturum açmayı etkinleştirmek için kullanın. Bu şekilde tüm kuruluş kullanıcıları Confluence uygulamasına oturum açma için Azure AD kimlik bilgilerini kullanın. Bu eklenti, Federasyon için SAML 2.0 kullanır.

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Microsoft tarafından Confluence SAML SSO ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Bir Windows 64 bit sunucuda yüklü confluence sunucu uygulaması (şirket içinde veya bulutta Iaas altyapınıza)
- HTTPS etkin confluence sunucusudur
- Not Confluence eklentisi için desteklenen sürümler bölümü belirtilmiştir.
- Confluence sunucu kimlik doğrulaması için Azure AD oturum açma sayfasına özellikle Internet üzerinden erişilebilir olduğundan ve Azure AD'den belirteç alamaz gerekir
- Yönetici kimlik bilgileri Confluence ayarlanır
- WebSudo Confluence devre dışı bırakıldı
- Confluence sunucu uygulamasında oluşturulan bir test kullanıcısı

> [!NOTE]
> Bu öğreticideki adımları test etmek için bir üretim ortamına Confluence kullanarak önermiyoruz. Tümleşik geliştirme ya da hazırlık ortamı uygulamanın önce test ve üretim ortamına kullanın.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme burada alabilirsiniz: [deneme teklifi](https://azure.microsoft.com/pricing/free-trial/).

## <a name="supported-versions-of-confluence"></a>Confluence desteklenen sürümleri 

Şu anda, Confluence'nın şu sürümleri desteklenir:

- Confluence: 5.0 5.10

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Microsoft tarafından Confluence SAML SSO ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-confluence-saml-sso-by-microsoft-from-the-gallery"></a>Galeriden Microsoft tarafından Confluence SAML SSO ekleme
Azure AD'de Microsoft tarafından Confluence SAML SSO tümleştirmesini yapılandırmak için Microsoft tarafından Confluence SAML SSO Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Microsoft tarafından Confluence SAML SSO eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **Confluence SAML SSO Microsoft tarafından**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_search.png)

1. Sonuçlar panelinde seçin **Confluence SAML SSO Microsoft tarafından**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcıya dayanarak Microsoft tarafından Confluence SAML SSO ile test edin.

Tek iş için oturum açma için Azure AD ne Confluence SAML SSO Microsoft gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve Microsoft tarafından Confluence SAML SSO ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Microsoft tarafından Confluence SAML SSO ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Microsoft test kullanıcı tarafından bir Confluence SAML SSO oluşturma](#creating-a-confluence-saml-sso-by-microsoft-test-user)**  - Confluence SAML SSO kullanıcı Azure AD gösterimini bağlantılı Microsoft tarafından bir karşılığı Britta simon'un sağlamak için.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma, Confluence SAML SSO, Microsoft uygulama tarafından yapılandırın.

**Microsoft tarafından Confluence SAML SSO ile Azure AD çoklu oturum açmayı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Confluence SAML SSO Microsoft tarafından** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açmayı yapılandırın](./media/confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_samlbase.png)

1. Üzerinde **Confluence SAML SSO Microsoft Domain ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<domain:port>/plugins/servlet/saml/auth`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<domain:port>/`

    c. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<domain:port>/plugins/servlet/saml/auth`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Adlandırılmış bir URL olması durumunda bağlantı noktası isteğe bağlıdır. Bu değerler, öğreticinin ilerleyen bölümlerinde açıklanan Confluence eklentisi, yapılandırma sırasında alınır.

1. Üzerinde **SAML imzalama sertifikası** bölümünde, kopyalamak için Kopyala düğmesine **uygulama Federasyon meta verileri URL'sini** kopyalayıp Not Defteri'ne yapıştırın.
    
    ![Çoklu oturum açmayı yapılandırın](./media/confluencemicrosoft-tutorial/tutorial_metadataurl.png)
     
1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/confluencemicrosoft-tutorial/tutorial_general_400.png)

1. Farklı bir web tarayıcı penceresinde Confluence Örneğiniz için bir yönetici olarak oturum açın.

1. Dişlisine gelin ve tıklayın **eklentileri**.
    
    ![Çoklu oturum açmayı yapılandırın](./media/confluencemicrosoft-tutorial/addon1.png)

1. Eklentisini indirin [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=56503). Microsoft tarafından sağlanan eklentisini el ile karşıya **karşıya yükleme, eklenti** menüsü. Eklenti indirilmesini altında ele [Microsoft hizmet sözleşmesi](https://www.microsoft.com/en-us/servicesagreement/). 
    
    ![Çoklu oturum açmayı yapılandırın](./media/confluencemicrosoft-tutorial/addon12.png)

1. Eklentiyi yükledikten sonra görünür **kullanıcı yüklü** eklentileri bölümünü **yönetme eklenti** bölümü. Tıklayın **yapılandırma** yeni eklentiyi yapılandırmak için.
    
    ![Çoklu oturum açmayı yapılandırın](./media/confluencemicrosoft-tutorial/addon13.png)

1. Yapılandırma sayfasında aşağıdaki adımları uygulayın:

    ![Çoklu oturum açmayı yapılandırın](./media/confluencemicrosoft-tutorial/addon52.png)

    > [!TIP]
    > Böylece hata meta veriler çözümlenmesinde uygulamayı karşı eşlenen yalnızca bir sertifika olduğundan emin olun. Birden çok sertifikası varsa, yönetim meta verileri çözme sırasında bir hata alır.

    a. İçinde **meta veri URL'si** metin kutusu, yapıştırma **uygulama Federasyon meta verileri URL'sini** Azure portal'ı seçin ve kopyaladığınız değeri **çözmek** düğmesi. IDP meta veri URL'sini okur ve tüm alanları bilgileri doldurur.

    b. Kopyalama **tanımlayıcısı, yanıt URL'si ve oturum açma URL'si** değerleri ve bunları yapıştırın **tanımlayıcısı, yanıt URL'si ve oturum açma URL'si** sırasıyla içinde metin kutuları **Confluence SAML SSO Microsoft Domain ve URL'leri**  bölümü Azure portalı.

    c. İçinde **oturum açma düğmesi adı** kuruluşunuzun oturum açma ekranında kullanıcıların istediği düğme adı yazın.

    d. İçinde **SAML kullanıcı kimliği konumları**, şunlardan birini seçin **kullanıcı kimliğidir konu deyiminin NameIdentifier öğesinde** veya **kullanıcı kimliği olup öznitelik öğe**.  Bu kimliği Confluence kullanıcı kimliği olması gerekir. Kullanıcı Kimliği eşleşmiyorsa, sonra sistem oturum açmasına izin vermez. 

    > [!Note]
    > Varsayılan kullanıcı kimliği SAML ad tanımlayıcısı konumdur. Bu öznitelik seçeneği değiştirin ve uygun öznitelik adını girin.
    
    e. Seçerseniz **kullanıcı kimliği olup öznitelik öğe** seçeneği, ardından **öznitelik adı** metin kullanıcı kimliği beklenirken özniteliğin adını yazın. 

    f. Azure AD ile Federasyon etki alanını (örneğin, ADFS vb.) kullanıyorsanız, ardından **giriş bölgesi bulmayı etkinleştirmek** seçeneği ve yapılandırma **etki alanı adı**.
    
    g. İçinde **etki alanı adı** ADFS tabanlı oturum açma durumunda burada etki alanı adını yazın.

    h. Denetleme **etkinleştirme çoklu oturum kapatma** Azure AD'den bir kullanıcı oturum açtığında Confluence oturumunuzu kapatmak istiyor. 

    i. Tıklayın **Kaydet** düğmesini kullanarak ayarları kaydedin.

    > [!NOTE]
    > Yükleme ve sorun giderme hakkında daha fazla bilgi için ziyaret [MS Confluence SSO Bağlayıcısı yönetici kılavuzundaki](../ms-confluence-jira-plugin-adminguide.md) ayrıca [SSS](../ms-confluence-jira-plugin-faq.md) , Yardım almak için

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/confluencemicrosoft-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/confluencemicrosoft-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/confluencemicrosoft-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/confluencemicrosoft-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-confluence-saml-sso-by-microsoft-test-user"></a>Microsoft test kullanıcı tarafından bir Confluence SAML SSO oluşturma

Confluence şirket içi sunucuya oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunlar Confluence SAML SSO Microsoft tarafından sağlanması gerekir. Microsoft tarafından Confluence SAML SSO için sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Confluence şirket içi sunucunuza yönetici olarak oturum açın.

1. Dişlisine gelin ve tıklayın **kullanıcı yönetimi**.

    ![Çalışan Ekle](./media/confluencemicrosoft-tutorial/user1.png) 

1. Kullanıcılar bölümü altında **kullanıcı ekleme** sekmesi. Üzerinde **kullanıcı ekleme** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Çalışan Ekle](./media/confluencemicrosoft-tutorial/user2.png) 

    a. İçinde **kullanıcıadı** metin Britta Simon gibi kullanıcının e-posta yazın.

    b. İçinde **tam adı** metin Britta Simon gibi kullanıcının tam adını yazın.

    c. İçinde **e-posta** metin kutusuna kullanıcı e-posta adresi türünü ister Brittasimon@contoso.com.

    d. İçinde **parola** metin Britta Simon için parolayı yazın.

    e. Tıklayın **parolayı onayla** parolayı yeniden girin.
    
    f. Tıklayın **Ekle** düğmesi.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Microsoft tarafından Confluence SAML SSO için erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Microsoft tarafından Confluence SAML SSO Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Confluence SAML SSO Microsoft tarafından**.

    ![Çoklu oturum açmayı yapılandırın](./media/confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim Paneli'nde Microsoft kutucuk tarafından Confluence SAML SSO'ye tıkladığınızda, otomatik olarak Confluence SAML SSO Microsoft uygulama tarafından açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/confluencemicrosoft-tutorial/tutorial_general_01.png
[2]: ./media/confluencemicrosoft-tutorial/tutorial_general_02.png
[3]: ./media/confluencemicrosoft-tutorial/tutorial_general_03.png
[4]: ./media/confluencemicrosoft-tutorial/tutorial_general_04.png

[100]: ./media/confluencemicrosoft-tutorial/tutorial_general_100.png

[200]: ./media/confluencemicrosoft-tutorial/tutorial_general_200.png
[201]: ./media/confluencemicrosoft-tutorial/tutorial_general_201.png
[202]: ./media/confluencemicrosoft-tutorial/tutorial_general_202.png
[203]: ./media/confluencemicrosoft-tutorial/tutorial_general_203.png
