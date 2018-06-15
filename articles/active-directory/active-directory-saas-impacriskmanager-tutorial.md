---
title: 'Öğretici: Azure Active Directory Tümleştirme ile IMPAC Risk Manager | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory IMPAC Risk Yöneticisi arasındaki yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 4d77390e-898c-4258-a562-a1181dfe2880
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: jeedes
ms.openlocfilehash: d7d45356e7b84d5a4ecbfc964fc1d4b5d8cc8db4
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34345379"
---
# <a name="tutorial-azure-active-directory-integration-with-impac-risk-manager"></a>Öğretici: Azure Active Directory Tümleştirme ile IMPAC Risk Yöneticisi

Bu öğreticide, Azure Active Directory (Azure AD) ile IMPAC Risk Manager tümleştirme öğrenin.

IMPAC Risk Yöneticisi Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- IMPAC Risk Manager'a erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak (çoklu oturum açma) IMPAC Risk Manager'a açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme IMPAC Risk Yöneticisi ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir IMPAC riski yönetici çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden IMPAC Risk Yöneticisi ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-impac-risk-manager-from-the-gallery"></a>Galeriden IMPAC Risk Yöneticisi ekleme
Azure AD tümleştirmeye IMPAC Risk Yöneticisi'ni yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden IMPAC riski yönetici eklemeniz gerekir.

**Galeriden IMPAC Risk Yöneticisi eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna yazın **IMPAC riski yönetici**seçin **IMPAC riski yönetici** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde IMPAC Risk Yöneticisi](./media/active-directory-saas-impacriskmanager-tutorial/tutorial_impacriskmanager_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma IMPAC Risk "Britta Simon" adlı bir test kullanıcı tabanlı Yöneticisi ile test etme.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen IMPAC Risk Yöneticisi'nde bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve ilgili kullanıcı IMPAC Risk Yöneticisi'nde arasında bir bağlantı ilişkisi kurulması gerekir.

Değeri IMPAC Risk Yöneticisi'nde, Ata **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma IMPAC Risk Yöneticisi ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[IMPAC riski yönetici test kullanıcısı oluşturma](#create-a-impac-risk-manager-test-user)**  - Britta Simon, karşılık gelen IMPAC Risk kullanıcı Azure AD gösterimini bağlantılı Yöneticisi'nde sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma IMPAC riski yönetici uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma IMPAC Risk Yöneticisi ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **IMPAC riski yönetici** uygulama tümleştirmesi sayfasında, tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-impacriskmanager-tutorial/tutorial_impacriskmanager_samlbase.png)

3. Üzerinde **IMPAC riski yönetici etki alanı ve URL'leri** bölümünde, uygulama tarafından başlatılan IDP modunda yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin:

    ![IMPAC riski yönetici etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-impacriskmanager-tutorial/tutorial_impacriskmanager_url_new.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, bir IMPAC tarafından sağlanan değer türü

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:
    | Ortam | URL deseni |
    | ---------------|--------------- |    
    | Üretim İçin |`https://www.riskmanager.co.nz/DotNet/SSOv2/AssertionConsumerService.aspx?client=<ClientSuffix>`|
    | Hazırlama ve eğitim  |`https://staging.riskmanager.co.nz/DotNet/SSOv2/AssertionConsumerService.aspx?client=<ClientSuffix>`|
    | Geliştirme İçin  |`https://dev.riskmanager.co.nz/DotNet/SSOv2/AssertionConsumerService.aspx?client=<ClientSuffix>`|
    | QA için |`https://QA.riskmanager.co.nz/DotNet/SSOv2/AssertionConsumerService.aspx?client=<ClientSuffix>`|
    | Test için |`https://test.riskmanager.co.nz/DotNet/SSOv2/AssertionConsumerService.aspx?client=<ClientSuffix>`|

4. Denetleme **Göster Gelişmiş URL ayarları** ve uygulamada yapılandırmak istiyorsanız aşağıdaki adımı gerçekleştirin **SP** modunda başlatılan:

    ![IMPAC riski yönetici etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-impacriskmanager-tutorial/tutorial_impacriskmanager_url1_new.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:
    | Ortam | URL deseni |
    | ---------------|--------------- |    
    | Üretim İçin |`https://www.riskmanager.co.nz/SSOv2/<ClientSuffix>`|
    | Hazırlama ve eğitim  |`https://staging.riskmanager.co.nz/SSOv2/<ClientSuffix>`|
    | Geliştirme İçin  |`https://dev.riskmanager.co.nz/SSOv2/<ClientSuffix>`|
    | QA için |`https://QA.riskmanager.co.nz/SSOv2/<ClientSuffix>`|
    | Test için |`https://test.riskmanager.co.nz/SSOv2/<ClientSuffix>`|

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler, gerçek tanımlayıcı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Kişi [IMPAC Risk Yöneticisi istemci destek ekibi](mailto:rmsupport@Impac.co.nz) bu değerleri almak için.

5. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-impacriskmanager-tutorial/tutorial_impacriskmanager_certificate.png) 

6. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-impacriskmanager-tutorial/tutorial_general_400.png)
    
7. Üzerinde **IMPAC Risk Yöneticisi yapılandırma** 'yi tıklatın **IMPAC Risk Yöneticisi yapılandırma** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML çoklu oturum açma hizmet URL'si, SAML varlık kimliği** ve **Sign-Out URL** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-impacriskmanager-tutorial/tutorial_impacriskmanager_configure.png)

8. Çoklu oturum açma yapılandırmak için **IMPAC riski yönetici** yan, indirilen göndermek için ihtiyacınız **Certificate(Base64)**, **Sign-Out URL, SAML varlık kimliği** ve  **SAML çoklu oturum açma hizmet URL'si** için [IMPAC Risk Manager destek ekip](mailto:rmsupport@Impac.co.nz). Bunlar, her iki tarafta da ayarlamanızı SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-impacriskmanager-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-impacriskmanager-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-impacriskmanager-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-impacriskmanager-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-impac-risk-manager-test-user"></a>IMPAC riski yönetici test kullanıcısı oluşturma

Bu bölümde, Britta Simon IMPAC Risk Yöneticisi'nde adlı bir kullanıcı oluşturun. Çalışmak [IMPAC Risk Manager destek ekip](mailto:rmsupport@Impac.co.nz) IMPAC riski yönetici platform kullanıcıları eklemek için. Kullanıcıların oluşturulan ve çoklu oturum açma kullanmadan önce etkinleştirilmelidir. 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta IMPAC Risk Yöneticisi için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon IMPAC riski yönetici atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **IMPAC riski yönetici**.

    ![Uygulamalar listesinde IMPAC riski yönetici bağlantı](./media/active-directory-saas-impacriskmanager-tutorial/tutorial_impacriskmanager_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli IMPAC riski yönetici parçasında tıklattığınızda, otomatik olarak IMPAC riski yönetici uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/active-directory-saas-impacriskmanager-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-impacriskmanager-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-impacriskmanager-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-impacriskmanager-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-impacriskmanager-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-impacriskmanager-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-impacriskmanager-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-impacriskmanager-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-impacriskmanager-tutorial/tutorial_general_203.png

