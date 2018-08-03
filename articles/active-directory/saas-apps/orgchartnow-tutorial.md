---
title: 'Öğretici: Azure Active Directory kuruluş şeması şimdi ile tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Kuruluş Şeması şimdi arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 50a1522f-81de-4d14-9b6b-dd27bb1338a4
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2018
ms.author: jeedes
ms.openlocfilehash: e23d76074f4b428b672e0cd5aeeaba99d080a4cf
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39435943"
---
# <a name="tutorial-azure-active-directory-integration-with-orgchart-now"></a>Öğretici: Azure Active Directory kuruluş şeması şimdi ile tümleştirme

Bu öğreticide, Kuruluş Şeması artık Azure Active Directory (Azure AD) ile tümleştirmeyi öğrenin.

Kuruluş Şeması şimdi Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Kuruluş Şeması şimdi erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan (çoklu oturum açma) için Kuruluş Şeması şimdi açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Kuruluş Şeması artık Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Bir kuruluş şeması şimdi çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Kuruluş Şeması şimdi galeri ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-orgchart-now-from-the-gallery"></a>Kuruluş Şeması şimdi galeri ekleme
Kuruluş Şeması artık Azure AD'de tümleştirmesini yapılandırmak için Kuruluş Şeması şimdi Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Kuruluş Şeması şimdi Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **Kuruluş Şeması şimdi**seçin **Kuruluş Şeması şimdi** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Kuruluş Şeması artık sonuçlar listesinde](./media/orgchartnow-tutorial/tutorial_orgchartnow_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırmanız ve Kuruluş Şeması şimdi ile Azure AD çoklu oturum açmayı test "Britta Simon" adlı bir test kullanıcı tabanlı.

Tek iş için oturum açma için Azure AD Kuruluş Şeması şimdi karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmesi gerekir. Diğer bir deyişle, bir Azure AD kullanıcı ve Kuruluş Şeması şimdi ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Kuruluş Şeması şimdi ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Bir kuruluş şeması şimdi test kullanıcısı oluşturma](#create-an-orgchart-now-test-user)**  - kullanıcı Azure AD gösterimini bağlı kuruluş şeması şimdi Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Kuruluş Şeması artık uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Kuruluş Şeması şimdi yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Kuruluş Şeması şimdi** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/orgchartnow-tutorial/tutorial_orgchartnow_samlbase.png)

1. Üzerinde **Kuruluş Şeması artık etki alanı ve URL'ler** uygulamada yapılandırmak isterseniz, bölümü **IDP** başlatılan modu:

    ![Kuruluş Şeması artık etki alanı ve URL'ler tek oturum açma bilgileri](./media/orgchartnow-tutorial/tutorial_orgchartnow_url.png)

    İçinde **tanımlayıcı** metin kutusuna bir URL: `https://sso2.orgchartnow.com`

1. Denetleme **Gelişmiş URL ayarlarını göster** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![Kuruluş Şeması artık etki alanı ve URL'ler tek oturum açma bilgileri](./media/orgchartnow-tutorial/tutorial_orgchartnow_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://sso2.orgchartnow.com/Shibboleth.sso/Login?entityID=<YourEntityID>&target=https://sso2.orgchartnow.com`
     
    > [!NOTE]
    > `<YourEntityID>` SAML varlık kimliği hızlı başvuru bölümünde, bu öğreticinin sonraki bölümlerinde açıklanan kopyalanır.

1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/orgchartnow-tutorial/tutorial_orgchartnow_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/orgchartnow-tutorial/tutorial_general_400.png)
    
1. Üzerinde **Kuruluş Şeması artık yapılandırma** bölümünde **Kuruluş Şeması Şimdi Yapılandır** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML varlık kimliği** gelen **hızlı başvuru bölümüne** tamamlamak için **oturum açma URL'si** içinde **Kuruluş Şeması artık etki alanı ve URL'ler bölüm**.

    ![Kuruluş Şeması şimdi yapılandırma](./media/orgchartnow-tutorial/tutorial_orgchartnow_configure.png) 

1. Çoklu oturum açmayı yapılandırma **Kuruluş Şeması şimdi** tarafı, indirilen göndermek için ihtiyacınız **meta veri XML** için [Kuruluş Şeması şimdi Destek ekibine](mailto:ocnsupport@officeworksoftware.com). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/orgchartnow-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/orgchartnow-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/orgchartnow-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/orgchartnow-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-an-orgchart-now-test-user"></a>Bir kuruluş şeması şimdi test kullanıcısı oluşturma

Azure AD kullanıcıları için Kuruluş Şeması şimdi oturum açmayı etkinleştirmek için bunların Kuruluş Şeması şimdi sağlanması gerekir. 

1. Kuruluş Şeması şimdi tam zamanında sağlama, varsayılan olarak etkin olan destekler. Yeni bir kullanıcı, henüz yoksa, Kuruluş Şeması şimdi erişme denemesi sırasında oluşturulur. Sağlama özelliğini tam zamanında kullanıcı yalnızca oluşturacak bir **salt okunur** kullanıcıya bir SSO isteği tanınan IDP'den gelir ve SAML onayı e-postada kullanıcı listesinde bulunamadı. Başlıklı bir erişim grubu oluşturmak gereken bu otomatik sağlama için **genel** Kuruluş Şeması şimdi. Lütfen bir erişim grubu oluşturmak için aşağıdaki adımları:

    a. Git **grupları yönet** tıkladıktan sonra seçeneği **dişli** sağ üst köşesindeki kullanıcı Arabirimi içinde.

    ![Kuruluş Şeması şimdi grupları](./media/orgchartnow-tutorial/tutorial_orgchartnow_manage.png)    

    b. Seçin **Ekle** simgesi ve grubun adı **genel** ardından **Tamam**. 

    ![Kuruluş Şeması şimdi ekleyin](./media/orgchartnow-tutorial/tutorial_orgchartnow_add.png)

    c. Genel veya salt okunur kullanıcı erişebilmesi için istediğiniz klasörleri seçin:

    ![Kuruluş Şeması şimdi klasörleri](./media/orgchartnow-tutorial/tutorial_orgchartnow_chart.png)

    d. **Kilit** klasörleri yalnızca yönetici kullanıcılar bunları değiştirebilir. Tuşuna basarak **Tamam**.

    ![Kuruluş Şeması şimdi Kilitle](./media/orgchartnow-tutorial/tutorial_orgchartnow_lock.png)

1. Oluşturulacak **yönetici** kullanıcılar ve **okuma/yazma** kullanıcıları, el ile oluşturmanız gerekir bir kullanıcı kendi ayrıcalık düzeyi SSO aracılığıyla erişmek için. Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:

    a. İçin Kuruluş Şeması artık bir güvenlik yöneticisi olarak oturum açın.

    b.  Tıklayarak **ayarları** sağ üst köşe ve ardından gidin **Kullanıcıları Yönet**.

    ![Kuruluş Şeması şimdi ayarları](./media/orgchartnow-tutorial/tutorial_orgchartnow_settings.png)

    c. Tıklayarak **Ekle** ve aşağıdaki adımları gerçekleştirin:

    ![Kuruluş Şeması şimdi yönetme](./media/orgchartnow-tutorial/tutorial_orgchartnow_manageusers.png)

    * İçinde **kullanıcı kimliği** metin kullanıcı kimliği gibi girin **brittasimon@contoso.com**.

    * İçinde **e-posta adresi** metin kutusuna, kullanıcının gibi e-posta girin **brittasimon@contoso.com**.

    * **Ekle**'ye tıklayın.
    
### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Kuruluş Şeması şimdi erişim vererek Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon için Kuruluş Şeması şimdi atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Kuruluş Şeması şimdi**.

    ![Kuruluş Şeması şimdi bağlantısına uygulamalar listesinde](./media/orgchartnow-tutorial/tutorial_orgchartnow_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Kuruluş Şeması şimdi kutucuğa tıkladığınızda, otomatik olarak kuruluş şeması şimdi uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/orgchartnow-tutorial/tutorial_general_01.png
[2]: ./media/orgchartnow-tutorial/tutorial_general_02.png
[3]: ./media/orgchartnow-tutorial/tutorial_general_03.png
[4]: ./media/orgchartnow-tutorial/tutorial_general_04.png

[100]: ./media/orgchartnow-tutorial/tutorial_general_100.png

[200]: ./media/orgchartnow-tutorial/tutorial_general_200.png
[201]: ./media/orgchartnow-tutorial/tutorial_general_201.png
[202]: ./media/orgchartnow-tutorial/tutorial_general_202.png
[203]: ./media/orgchartnow-tutorial/tutorial_general_203.png

