---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile IMPAC Risk Manager | Microsoft Docs'
description: Azure Active Directory ve IMPAC Risk Manager arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 4d77390e-898c-4258-a562-a1181dfe2880
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: jeedes
ms.openlocfilehash: ca0ea482b1cfb2f7af962ae1b7537f79bb60a62b
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54823156"
---
# <a name="tutorial-azure-active-directory-integration-with-impac-risk-manager"></a>Öğretici: Azure Active Directory Tümleştirmesi ile IMPAC Risk Yöneticisi

Bu öğreticide, Azure Active Directory (Azure AD) ile IMPAC Risk Manager tümleştirme konusunda bilgi edinin.

Azure AD ile IMPAC Risk Manager tümleştirme ile aşağıdaki avantajları sağlar:

- Kimlerin erişebildiğini IMPAC Risk Yöneticisi Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan (çoklu oturum açma) IMPAC Risk Manager'a açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi IMPAC Risk Yöneticisi ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir Risk Manager IMPAC çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden IMPAC Risk Yöneticisi ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-impac-risk-manager-from-the-gallery"></a>Galeriden IMPAC Risk Yöneticisi ekleme
Azure AD'de IMPAC Risk Manager'ın tümleştirmesini yapılandırmak için IMPAC Risk Manager Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden IMPAC Risk Yöneticisi eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **IMPAC Risk Manager**seçin **IMPAC Risk Manager** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde IMPAC Risk Yöneticisi](./media/impacriskmanager-tutorial/tutorial_impacriskmanager_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma IMPAC Risk "Britta Simon" adlı bir test kullanıcı tabanlı Yöneticisi ile test edin.

Tek iş için oturum açma için Azure AD ne karşılık gelen kullanıcı IMPAC Risk Yöneticisi'nde bir kullanıcının Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile ilgili kullanıcı IMPAC Risk Yöneticisi'nde arasında bir bağlantı ilişki kurulması gerekir.

Değerini IMPAC Risk Yöneticisi'nde atamak **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma IMPAC Risk Yöneticisi ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[IMPAC Risk Manager test kullanıcısı oluşturma](#create-a-impac-risk-manager-test-user)**  - IMPAC Risk kullanıcı Azure AD gösterimini bağlı Yöneticisi'nde Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve IMPAC Risk Manager uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma IMPAC Risk Yöneticisi ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **IMPAC Risk Manager** uygulama tümleştirme sayfası, tıklayın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/impacriskmanager-tutorial/tutorial_impacriskmanager_samlbase.png)

1. Üzerinde **IMPAC Risk yöneticisi etki alanı ve URL'ler** bölümünde, IDP tarafından başlatılan modunda uygulama yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin:

    ![IMPAC Risk yöneticisi etki alanı ve URL'ler tek oturum açma bilgileri](./media/impacriskmanager-tutorial/tutorial_impacriskmanager_url_new.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir değer IMPAC tarafından sağlanan

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak:
    | Ortam | URL deseni |
    | ---------------|--------------- |    
    | Üretim İçin |`https://www.riskmanager.co.nz/DotNet/SSOv2/AssertionConsumerService.aspx?client=<ClientSuffix>`|
    | Hazırlama ve eğitim  |`https://staging.riskmanager.co.nz/DotNet/SSOv2/AssertionConsumerService.aspx?client=<ClientSuffix>`|
    | Geliştirme İçin  |`https://dev.riskmanager.co.nz/DotNet/SSOv2/AssertionConsumerService.aspx?client=<ClientSuffix>`|
    | QA için |`https://QA.riskmanager.co.nz/DotNet/SSOv2/AssertionConsumerService.aspx?client=<ClientSuffix>`|
    | Test için |`https://test.riskmanager.co.nz/DotNet/SSOv2/AssertionConsumerService.aspx?client=<ClientSuffix>`|

1. Denetleme **Gelişmiş URL ayarlarını göster** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![IMPAC Risk yöneticisi etki alanı ve URL'ler tek oturum açma bilgileri](./media/impacriskmanager-tutorial/tutorial_impacriskmanager_url1_new.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak:
    | Ortam | URL deseni |
    | ---------------|--------------- |    
    | Üretim İçin |`https://www.riskmanager.co.nz/SSOv2/<ClientSuffix>`|
    | Hazırlama ve eğitim  |`https://staging.riskmanager.co.nz/SSOv2/<ClientSuffix>`|
    | Geliştirme İçin  |`https://dev.riskmanager.co.nz/SSOv2/<ClientSuffix>`|
    | QA için |`https://QA.riskmanager.co.nz/SSOv2/<ClientSuffix>`|
    | Test için |`https://test.riskmanager.co.nz/SSOv2/<ClientSuffix>`|

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. İlgili kişi [IMPAC Risk Manager istemci Destek ekibine](mailto:rmsupport@Impac.co.nz) bu değerleri almak için.

1. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/impacriskmanager-tutorial/tutorial_impacriskmanager_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/impacriskmanager-tutorial/tutorial_general_400.png)
    
1. Üzerinde **IMPAC Risk Manager yapılandırması** bölümünde **IMPAC Risk Yapılandırma Yöneticisi'ni** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML çoklu oturum açma hizmeti URL'si, SAML varlık kimliği** ve **oturum kapatma URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/impacriskmanager-tutorial/tutorial_impacriskmanager_configure.png)

1. Çoklu oturum açmayı yapılandırma **IMPAC Risk Manager** tarafı, indirilen göndermek için ihtiyacınız **Certificate(Base64)**, **oturum kapatma URL'si, SAML varlık kimliği** ve  **SAML çoklu oturum açma hizmeti URL'si** için [IMPAC Risk Manager destek ekip](mailto:rmsupport@Impac.co.nz). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/impacriskmanager-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/impacriskmanager-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/impacriskmanager-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/impacriskmanager-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-impac-risk-manager-test-user"></a>IMPAC Risk Manager test kullanıcısı oluşturma

Bu bölümde, Britta Simon IMPAC Risk Yöneticisi'nde adlı bir kullanıcı oluşturun. Çalışmak [IMPAC Risk Manager destek ekip](mailto:rmsupport@Impac.co.nz) IMPAC Risk Manager platform kullanıcıları eklemek için. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi. 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, IMPAC Risk Yöneticisi erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon IMPAC Risk Manager'a atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **IMPAC Risk Manager**.

    ![Uygulamalar listesinde IMPAC Risk Manager bağlantısı](./media/impacriskmanager-tutorial/tutorial_impacriskmanager_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli IMPAC Risk Manager kutucuğa tıkladığınızda, size otomatik olarak IMPAC Risk Manager uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/impacriskmanager-tutorial/tutorial_general_01.png
[2]: ./media/impacriskmanager-tutorial/tutorial_general_02.png
[3]: ./media/impacriskmanager-tutorial/tutorial_general_03.png
[4]: ./media/impacriskmanager-tutorial/tutorial_general_04.png

[100]: ./media/impacriskmanager-tutorial/tutorial_general_100.png

[200]: ./media/impacriskmanager-tutorial/tutorial_general_200.png
[201]: ./media/impacriskmanager-tutorial/tutorial_general_201.png
[202]: ./media/impacriskmanager-tutorial/tutorial_general_202.png
[203]: ./media/impacriskmanager-tutorial/tutorial_general_203.png

