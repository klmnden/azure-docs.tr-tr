---
title: 'Öğretici: Kuruluş Şeması şimdi Azure Active Directory Tümleştirme | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory kuruluş şeması şimdi arasındaki yapılandırmayı öğrenin.
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
ms.openlocfilehash: f68c5d6a022cccecde3b3eb272e51f75ae6bc50e
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36222070"
---
# <a name="tutorial-azure-active-directory-integration-with-orgchart-now"></a>Öğretici: Kuruluş Şeması şimdi Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Kuruluş Şeması şimdi tümleştirmek öğrenin.

Kuruluş Şeması şimdi Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Kuruluş Şeması şimdi erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak (çoklu oturum açma) için Kuruluş Şeması şimdi açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Kuruluş Şeması şimdi yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir kuruluş şeması şimdi çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Kuruluş Şeması şimdi Galeriden ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-orgchart-now-from-the-gallery"></a>Kuruluş Şeması şimdi Galeriden ekleme
Azure AD Kuruluş Şeması şimdi tümleştirilmesi yapılandırmak için Kuruluş Şeması şimdi Galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Kuruluş Şeması şimdi Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Kuruluş Şeması şimdi**seçin **Kuruluş Şeması şimdi** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Kuruluş Şeması şimdi sonuçlar listesinde](./media/orgchartnow-tutorial/tutorial_orgchartnow_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırmanız ve Kuruluş Şeması şimdi ile Azure AD çoklu oturum açmayı test "Britta Simon" adlı bir test kullanıcı tabanlı.

Tekli çalışmaya oturum için Azure AD Kuruluş Şeması şimdi karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcı ve Kuruluş Şeması şimdi ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Kuruluş Şeması şimdi ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir kuruluş şeması şimdi test kullanıcısı oluşturma](#create-an-orgchart-now-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı kuruluş şeması şimdi sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Kuruluş Şeması şimdi uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Kuruluş Şeması şimdi yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Kuruluş Şeması şimdi** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/orgchartnow-tutorial/tutorial_orgchartnow_samlbase.png)

3. Üzerinde **Kuruluş Şeması artık etki alanı ve URL'leri** uygulamada yapılandırmak istiyorsanız, bölüm **IDP** modu tarafından başlatılan:

    ![Kuruluş Şeması artık etki alanı ve URL'leri tek oturum açma bilgileri](./media/orgchartnow-tutorial/tutorial_orgchartnow_url.png)

    İçinde **tanımlayıcısı** metin kutusuna, bir URL yazın: `https://sso2.orgchartnow.com`

4. Denetleme **Göster Gelişmiş URL ayarları** ve uygulamada yapılandırmak istiyorsanız aşağıdaki adımı gerçekleştirin **SP** modunda başlatılan:

    ![Kuruluş Şeması artık etki alanı ve URL'leri tek oturum açma bilgileri](./media/orgchartnow-tutorial/tutorial_orgchartnow_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://sso2.orgchartnow.com/Shibboleth.sso/Login?entityID=<YourEntityID>&target=https://sso2.orgchartnow.com`
     
    > [!NOTE]
    > `<YourEntityID>` SAML varlık kimliği öğreticinin ilerleyen bölümlerinde açıklanan hızlı başvuru bölümünden kopyalanır.

5. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/orgchartnow-tutorial/tutorial_orgchartnow_certificate.png) 

6. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/orgchartnow-tutorial/tutorial_general_400.png)
    
7. Üzerinde **Kuruluş Şeması artık yapılandırma** 'yi tıklatın **Kuruluş Şeması Şimdi Yapılandır** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML varlık kimliği** gelen **hızlı başvuru bölümünde** ve tamamlamak için kullanmak **oturum açma URL'si** içinde **Kuruluş Şeması artık etki alanı ve URL'leri bölümüne**.

    ![Kuruluş Şeması şimdi yapılandırma](./media/orgchartnow-tutorial/tutorial_orgchartnow_configure.png) 

8. Çoklu oturum açma yapılandırmak için **Kuruluş Şeması şimdi** yan, indirilen göndermek için ihtiyacınız **meta veri XML** için [Kuruluş Şeması şimdi destek ekibi](mailto:ocnsupport@officeworksoftware.com). Bunlar, her iki tarafta da ayarlamanızı SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/orgchartnow-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/orgchartnow-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/orgchartnow-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/orgchartnow-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-an-orgchart-now-test-user"></a>Bir kuruluş şeması şimdi test kullanıcısı oluşturma

Kuruluş Şeması şimdi oturum açmak Azure AD kullanıcıları etkinleştirmek için bunların Kuruluş Şeması şimdi sağlanmalıdır. 

1. Kuruluş Şeması şimdi yalnızca zaman sağlama, varsayılan olarak etkin olduğu destekler. Yeni bir kullanıcı henüz yoksa Kuruluş Şeması şimdi erişme denemesi sırasında oluşturulur. Özellik sağlama tam zamanı kullanıcı yalnızca oluşturacak bir **salt okunur** kullanıcıya bir SSO isteği tanınan bir IDP gelir ve SAML onayı e-postayla kullanıcı listesinde bulunamadı. Başlıklı bir erişim grubu oluşturmak gereken özellik sağlama bu auto **genel** Kuruluş Şeması şimdi içinde. Lütfen izleyin bir erişim grubu oluşturmak için aşağıdaki adımları:

    a. Git **grupları yönet** tıkladıktan sonra seçeneği **gear** kullanıcı arabirimini ekranın sağ üst köşesinde içinde.

    ![Kuruluş Şeması şimdi grupları](./media/orgchartnow-tutorial/tutorial_orgchartnow_manage.png)    

    b. Seçin **Ekle** simgesi ve grup adı **genel** ardından **Tamam**. 

    ![Kuruluş Şeması şimdi ekleyin](./media/orgchartnow-tutorial/tutorial_orgchartnow_add.png)

    c. Erişebilmeleri için genel veya salt okunur kullanıcıların istediğiniz klasörleri seçin:

    ![Kuruluş Şeması şimdi klasörleri](./media/orgchartnow-tutorial/tutorial_orgchartnow_chart.png)

    d. **Kilit** klasörleri böylece yalnızca yönetici kullanıcılar bunları değiştirebilirsiniz. Tuşuna basarak **Tamam**.

    ![Kuruluş Şeması şimdi Kilitle](./media/orgchartnow-tutorial/tutorial_orgchartnow_lock.png)

2. Oluşturmak için **yönetici** kullanıcılar ve **okuma/yazma** kullanıcıları, el ile oluşturmanız gerekir bir kullanıcı kendi ayrıcalık düzeyi SSO aracılığıyla erişmek için. Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:

    a. İçin Kuruluş Şeması şimdi bir güvenlik yöneticisi olarak oturum açın.

    b.  Tıklayın **ayarları** sağ üst köşe ve ardından gidin **kullanıcıları yönetme**.

    ![Kuruluş Şeması şimdi ayarları](./media/orgchartnow-tutorial/tutorial_orgchartnow_settings.png)

    c. Tıklayın **Ekle** ve aşağıdaki adımları gerçekleştirin:

    ![Kuruluş Şeması şimdi yönetme](./media/orgchartnow-tutorial/tutorial_orgchartnow_manageusers.png)

    * İçinde **kullanıcı kimliği** metin kutusu, kullanıcı kimliği gibi girin **brittasimon@contoso.com**.

    * İçinde **e-posta adresi** metin kutusunda, bir kullanıcı gibi e-posta girin **brittasimon@contoso.com**.

    * **Ekle**'ye tıklayın.
    
### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Kuruluş Şeması şimdi erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Kuruluş Şeması şimdi Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Kuruluş Şeması şimdi**.

    ![Kuruluş Şeması şimdi bağlantı uygulamalar listesinde](./media/orgchartnow-tutorial/tutorial_orgchartnow_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Kuruluş Şeması şimdi parçasında tıklattığınızda, otomatik olarak kuruluş şeması şimdi uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)



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

