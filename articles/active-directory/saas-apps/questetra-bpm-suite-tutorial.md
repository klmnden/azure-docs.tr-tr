---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Questetra BPM paketi | Microsoft Docs'
description: Azure Active Directory ve Questetra BPM Suite arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: fb6d5b73-e491-4dd2-92d6-94e5aba21465
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/29/2017
ms.author: jeedes
ms.openlocfilehash: cc2d88bfc6b8ce57cebc2e35e3a9f2e3b826e9cd
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54816781"
---
# <a name="tutorial-azure-active-directory-integration-with-questetra-bpm-suite"></a>Öğretici: Questetra BPM Suite ile Azure Active Directory Tümleştirmesi

Bu öğreticide, Azure Active Directory (Azure AD) ile Questetra BPM Suite tümleştirme konusunda bilgi edinin.

Azure AD ile Questetra BPM Suite tümleştirme ile aşağıdaki avantajları sağlar:

- Questetra BPM Suite erişimi, Azure AD'de denetleyebilirsiniz
- Azure AD hesaplarına otomatik olarak imzalanan (çoklu oturum açma) Questetra BPM paketine açma, kullanıcılarınızın etkinleştirebilirsiniz
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Questetra BPM Suite ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik Questetra BPM Suite çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Questetra BPM paketine ekleyin
1. Yapılandırma ve Azure AD çoklu oturum açmayı test etme

## <a name="add-questetra-bpm-suite-from-the-gallery"></a>Galeriden Questetra BPM paketine ekleyin
Azure AD'de Questetra BPM Suite tümleştirmesini yapılandırmak için Questetra BPM Suite Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Questetra BPM paketi eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **Questetra BPM Suite**seçin **Questetra BPM Suite** sonuç paneli ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Galeriden Ekle](./media/questetra-bpm-suite-tutorial/tutorial_questetra-bpm-suite_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme
Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Questetra BPM Suite ile test edin.

Tek iş için oturum açma için Azure AD ne karşılık gelen kullanıcı Questetra BPM grubundaki bir kullanıcı için Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Questetra BPM paketindeki ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Değerini Questetra BPM paketindeki atama **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Questetra BPM Suite ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Questetra BPM Suite test kullanıcısı oluşturma](#create-a-questetra-bpm-suite-test-user)**  - Questetra BPM paketindeki, kullanıcının Azure AD gösterimini bağlı Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Questetra BPM Suite uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma Questetra BPM Suite ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Questetra BPM Suite** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![SAML Tabanlı Oturum Açma](./media/questetra-bpm-suite-tutorial/tutorial_questetra-bpm-suite_samlbase.png)

1. Üzerinde **Questetra BPM Suite etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Questetra BPM Suite etki alanı ve URL'ler bölümü](./media/questetra-bpm-suite-tutorial/tutorial_questetra-bpm-suite_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<subdomain>.questetra.net/saml/SSO/alias/bpm`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<subdomain>.questetra.net/`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Bu değerleri alabilirsiniz **SP bilgi** bölümünde, **Questetra BPM Suite** daha sonra öğreticide veya kişi açıklanan şirket sitesi [Questetra BPM Suite istemci desteği Takım](https://www.questetra.com/contact/). 
 
1. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base 64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![SAML imzalama sertifikası bölümü](./media/questetra-bpm-suite-tutorial/tutorial_questetra-bpm-suite_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Kaydet Düğmesi](./media/questetra-bpm-suite-tutorial/tutorial_general_400.png)

1. Üzerinde **Questetra BPM paketi yapılandırması** bölümünde **Questetra BPM paketini Yapılandır** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Questetra BPM Suite yapılandırma bölümü](./media/questetra-bpm-suite-tutorial/tutorial_questetra-bpm-suite_configure.png) 

1. Oturum başka bir web tarayıcı penceresinde açın, **Questetra BPM Suite** yönetici olarak şirketin site.

1. Üstteki menüden **sistem ayarlarını**. 
   
    ![Azure AD çoklu oturum açma][10]

1. Açmak için **SingleSignOnSAML** sayfasında **SSO (SAML)**. 
   
    ![Azure AD çoklu oturum açma][11]

1. Üzerinde **Questetra BPM Suite** site, şirket içinde **SP bilgi** bölümünde, aşağıdaki adımları gerçekleştirin:

    a. Kopyalama **ACS URL**, ardından yapıştırın **işareti bulunan URL'si** metin kutusunda **Questetra BPM Suite etki alanı ve URL'ler** Azure portalından bölümü.
    
    b. Kopyalama **varlık kimliği**, ardından yapıştırın **tanımlayıcı** metin kutusunda **Questetra BPM Suite etki alanı ve URL'ler** Azure portalından bölümü.

1. Üzerinde **Questetra BPM Suite** şirket site, aşağıdaki adımları gerçekleştirin: 
   
    ![Çoklu oturum açmayı yapılandırın][15]
   
    a. Seçin **çoklu oturum açmayı etkinleştirme**.
   
    b. İçinde **varlık kimliği** metin değerini yapıştırın **SAML varlık kimliği** , Azure Portalı'ndan kopyaladığınız.
    
    c. İçinde **oturum açma sayfası URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmeti URL'si** , Azure Portalı'ndan kopyaladığınız.
    
    d. İçinde **sayfa oturum kapatma URL'si** metin değerini yapıştırın **oturum kapatma URL'si** , Azure Portalı'ndan kopyaladığınız.
    
    e. İçinde **Nameıd biçimi** metin kutusuna `urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress`.

    f. Açık, **Base-64** Defteri'nde kodlanmış sertifika Azure portalından indirilen, içeriğini sizin panoya kopyalayın ve ardından yapıştırın **doğrulama sertifikası** metin. 

    g. **Kaydet**’e tıklayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/questetra-bpm-suite-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/questetra-bpm-suite-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/questetra-bpm-suite-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/questetra-bpm-suite-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-questetra-bpm-suite-test-user"></a>Questetra BPM Suite test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon Questetra BPM paketindeki adlı bir kullanıcı oluşturmaktır.

**Britta Simon Questetra BPM paketindeki adlı bir kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Questetra BPM Suite şirketinizin sitesi için bir yönetici olarak oturum açın.
1. Git **sistem ayarları > kullanıcı listesi > Yeni kullanıcı**. 
1. Yeni kullanıcı iletişim kutusunda aşağıdaki adımları gerçekleştirin: 
   
    ![Test kullanıcısı oluşturma][300] 
   
    a. İçinde **adı** metin kutusuna **adı** kullanıcının **britta.simon@contoso.com**.
   
    b. İçinde **e-posta** metin kutusuna **e-posta** kullanıcının **britta.simon@contoso.com**
   
    c. İçinde **parola** metin bir **parola** kullanıcının.
    
    d. Tıklayın **yeni kullanıcı Ekle**.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma Questetra BPM paketine erişim vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Questetra BPM paketine atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Questetra BPM Suite**.

    ![Uygulamalar listesinde Questetra BPM paketi](./media/questetra-bpm-suite-tutorial/tutorial_questetra-bpm-suite_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Questetra BPM Suite kutucuğa tıkladığınızda, otomatik olarak Questetra BPM paketini uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/questetra-bpm-suite-tutorial/tutorial_general_01.png
[2]: ./media/questetra-bpm-suite-tutorial/tutorial_general_02.png
[3]: ./media/questetra-bpm-suite-tutorial/tutorial_general_03.png
[4]: ./media/questetra-bpm-suite-tutorial/tutorial_general_04.png
[10]: ./media/questetra-bpm-suite-tutorial/questera_bpm_suite_03.png
[11]: ./media/questetra-bpm-suite-tutorial/questera_bpm_suite_04.png
[15]: ./media/questetra-bpm-suite-tutorial/questera_bpm_suite_08.png

[100]: ./media/questetra-bpm-suite-tutorial/tutorial_general_100.png

[200]: ./media/questetra-bpm-suite-tutorial/tutorial_general_200.png
[201]: ./media/questetra-bpm-suite-tutorial/tutorial_general_201.png
[202]: ./media/questetra-bpm-suite-tutorial/tutorial_general_202.png
[203]: ./media/questetra-bpm-suite-tutorial/tutorial_general_203.png
[300]: ./media/questetra-bpm-suite-tutorial/questera_bpm_suite_11.png 

