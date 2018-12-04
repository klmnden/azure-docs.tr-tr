---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Perception Amerika Birleşik Devletleri (UltiPro olmayan) | Microsoft Docs'
description: Azure Active Directory ve Perception Amerika Birleşik Devletleri (UltiPro olmayan) arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: b4a8f026-cb5f-41eb-9680-68eddc33565e
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: 8c29d054f2e4e9ff4b57785a57e5c6ea512623a6
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52840677"
---
# <a name="tutorial-azure-active-directory-integration-with-perception-united-states-non-ultipro"></a>Öğretici: Azure Active Directory Tümleştirme ile Perception Amerika Birleşik Devletleri (UltiPro olmayan)

Bu öğreticide, Perception Amerika Birleşik Devletleri (UltiPro olmayan) Azure Active Directory (Azure AD) ile tümleştirmeyi öğrenin.

Perception Amerika Birleşik Devletleri (UltiPro olmayan) Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Kimlerin erişebildiğini Perception Amerika Birleşik Devletleri (UltiPro olmayan) için Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan Perception Amerika Birleşik Devletleri (Non-UltiPro) (çoklu oturum açma) için Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Perception Amerika Birleşik Devletleri (UltiPro olmayan) yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik Perception Amerika Birleşik Devletleri (UltiPro olmayan) çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Perception Amerika Birleşik Devletleri (UltiPro olmayan) ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-perception-united-states-non-ultipro-from-the-gallery"></a>Galeriden Perception Amerika Birleşik Devletleri (UltiPro olmayan) ekleme
Azure AD ile tümleştirme, Perception Amerika Birleşik Devletleri (UltiPro olmayan) yapılandırmak için Perception Amerika Birleşik Devletleri (UltiPro olmayan) eklemek Galeriden yönetilen SaaS uygulamaları listenize gerekir.

**Galeriden Perception Amerika Birleşik Devletleri (UltiPro olmayan) eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **Perception Amerika Birleşik Devletleri (UltiPro olmayan)** seçin **Perception Amerika Birleşik Devletleri (UltiPro olmayan)** sonucu panelinden ardından **Ekle** düğme eklemek için uygulama.

    ![Sonuç listesinde perception Amerika Birleşik Devletleri (UltiPro olmayan)](./media/perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma ile Perception "Britta Simon" adlı bir test kullanıcı tabanlı ABD (UltiPro olmayan) test edin.

Tek iş için oturum açma için Azure AD ne karşılık gelen kullanıcı tarafından Amerika Birleşik Devletleri (UltiPro olmayan), bir kullanıcının Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı tarafından Amerika Birleşik Devletleri (UltiPro olmayan), ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Değerini Perception Amerika Birleşik Devletleri (UltiPro olmayan), Ata **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Perception Amerika Birleşik Devletleri (UltiPro olmayan) ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Perception Amerika Birleşik Devletleri (UltiPro olmayan) bir test kullanıcısı oluşturma](#create-a-perception-united-states-non-ultipro-test-user)**  - içinde Perception kullanıcı Azure AD gösterimini bağlı ABD (UltiPro olmayan) Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Perception Amerika Birleşik Devletleri (UltiPro olmayan) uygulamanızı yapılandırın.

**Azure AD çoklu oturum açma Perception Amerika Birleşik Devletleri (UltiPro olmayan) yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Perception Amerika Birleşik Devletleri (UltiPro olmayan)** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_samlbase.png)

1. Üzerinde **Perception Amerika Birleşik Devletleri (UltiPro olmayan) etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Perception Amerika Birleşik Devletleri (UltiPro olmayan) etki alanı ve URL'ler tek oturum açma bilgileri](./media/perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna URL'yi yazın: `https://perception.kanjoya.com/sp`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://perception.kanjoya.com/sso?idp=<entity_id>`

    > [!NOTE] 
    > Değer, gerçek değil. Bu öğreticinin ilerleyen bölümlerinde açıklanan gerçek yanıt URL'si ile değeri güncelleştirir.
 
1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/perceptionunitedstates-tutorial/tutorial_general_400.png)

1. Üzerinde **Perception Amerika Birleşik Devletleri (UltiPro olmayan) yapılandırma** bölümünde **yapılandırma Perception Amerika Birleşik Devletleri (UltiPro olmayan)** açmak için **yapılandırma oturum açma** penceresi . Kopyalama **SAML varlık kimliği** gelen **hızlı başvuru bölümü.**

    a. **Perception Amerika Birleşik Devletleri (UltiPro olmayan)** uygulama gerektirir **SAML varlık kimliği** , URI olarak kodlanamadı için kopyaladığınız değeri. URI ile kodlanacak değer almak için aşağıdaki bağlantıyı kullanın:**http://www.url-encode-decode.com/**.

    b. URI aldıktan sonra kodlanmış değer birleştirme ile **yanıt URL'si** aşağıdaki - belirtildiği gibi

    `https://perception.kanjoya.com/sso?idp=<URI encooded entity_id>`
    
    c. Yukarıdaki değerinde yapıştırın **yanıt URL'si** metin kutusunda **Perception Amerika Birleşik Devletleri (UltiPro olmayan) etki alanı ve URL'ler** bölümü.

    ![Perception Amerika Birleşik Devletleri (UltiPro olmayan) yapılandırma](./media/perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_configure.png) 

1. Başka bir tarayıcı penceresinde Perception Amerika Birleşik Devletleri (UltiPro olmayan) şirketinizin sitesi için yönetici olarak oturum açın.

1. Ana araç çubuğunda tıklatın **hesap ayarları**.

    ![Amerika Birleşik Devletleri (UltiPro olmayan) kullanıcı algısı](./media/perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_user.png)

1. Üzerinde **hesap ayarları** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Amerika Birleşik Devletleri (UltiPro olmayan) kullanıcı algısı](./media/perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_account.png)

    a. İçinde **şirket adı** metin adı **şirket**.
    
    b. İçinde **hesap adı** metin adı **hesabı**.

    c. İçinde **varsayılan Yanıtla eposta** metin kutusunda, geçerli **e-posta**.

    d. Seçin **SSO kimlik sağlayıcısı** olarak **SAML 2.0**.

1. Üzerinde **SSO yapılandırma** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Amerika Birleşik Devletleri (UltiPro olmayan) SSOConfig algısı](./media/perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_ssoconfig.png)

    a. Seçin **SAML Nameıd türü** olarak **e-posta**.

    b. İçinde **SSO yapılandırma adı** metin adını yazın, **yapılandırma**.
    
    c. İçinde **kimlik sağlayıcı adı** metin değerini yapıştırın **SAML varlık kimliği**, hangi Azure Portalı'ndan kopyaladığınız. 

    d. İçinde **SAML etki alanı metin kutusu**, gibi etki alanını girin **@contoso.com**.

    e. Tıklayarak **yeniden karşıya** yüklenecek **meta veri XML** dosya.

    f. Tıklayın **güncelleştirme**.


> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/perceptionunitedstates-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/perceptionunitedstates-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/perceptionunitedstates-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/perceptionunitedstates-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
  
### <a name="create-a-perception-united-states-non-ultipro-test-user"></a>Perception Amerika Birleşik Devletleri (UltiPro olmayan) bir test kullanıcısı oluşturma

Bu bölümde, Britta Simon Perception Amerika Birleşik Devletleri (UltiPro olmayan) adlı bir kullanıcı oluşturun. Çalışmak [Perception Amerika Birleşik Devletleri (UltiPro olmayan) destek ekibi](https://www.ultimatesoftware.com/Contact/ContactUs) Perception Amerika Birleşik Devletleri (UltiPro olmayan) platform kullanıcıları eklemek için.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Perception Amerika Birleşik Devletleri (UltiPro olmayan) için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Perception Amerika Birleşik Devletleri (UltiPro olmayan) için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Perception Amerika Birleşik Devletleri (UltiPro olmayan)**.

    ![Uygulamalar listesinde Perception Amerika Birleşik Devletleri (UltiPro olmayan) bağlantısı](./media/perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Perception Amerika Birleşik Devletleri (UltiPro olmayan) kutucuğa tıkladığınızda, otomatik olarak Perception Amerika Birleşik Devletleri (UltiPro olmayan) uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/perceptionunitedstates-tutorial/tutorial_general_01.png
[2]: ./media/perceptionunitedstates-tutorial/tutorial_general_02.png
[3]: ./media/perceptionunitedstates-tutorial/tutorial_general_03.png
[4]: ./media/perceptionunitedstates-tutorial/tutorial_general_04.png

[100]: ./media/perceptionunitedstates-tutorial/tutorial_general_100.png

[200]: ./media/perceptionunitedstates-tutorial/tutorial_general_200.png
[201]: ./media/perceptionunitedstates-tutorial/tutorial_general_201.png
[202]: ./media/perceptionunitedstates-tutorial/tutorial_general_202.png
[203]: ./media/perceptionunitedstates-tutorial/tutorial_general_203.png

