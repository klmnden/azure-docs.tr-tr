---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle Optimizely | Microsoft Docs'
description: Azure Active Directory ve Optimizely arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 28ef03e1-9aad-4301-af97-d94e853edc74
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 872a441df795e742ac884f97843f179a782aec0d
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39421612"
---
# <a name="tutorial-azure-active-directory-integration-with-optimizely"></a>Öğretici: Azure Active Directory Optimizely ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Optimizely tümleştirme konusunda bilgi edinin.

Azure AD ile Optimizely tümleştirme ile aşağıdaki avantajları sağlar:

- Optimizely erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için Optimizely (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Optimizely yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik bir Optimizely çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Optimizely ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-optimizely-from-the-gallery"></a>Galeriden Optimizely ekleme
Azure AD'de Optimizely tümleştirmesini yapılandırmak için Optimizely Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Optimizely eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **Optimizely**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/optimizely-tutorial/tutorial_optimizely_search.png)

1. Sonuçlar panelinde seçin **Optimizely**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/optimizely-tutorial/tutorial_optimizely_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." adlı bir test kullanıcı tabanlı Optimizely ile test etme

Tek iş için oturum açma için Azure AD ne Optimizely karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Optimizely ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Değerini atayarak bu bağlantı ilişki kurulduktan **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** Optimizely içinde.

Yapılandırma ve Azure AD çoklu oturum açma Optimizely ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Bir Optimizely test kullanıcısı oluşturma](#creating-an-optimizely-test-user)**  - kullanıcı Azure AD gösterimini bağlı Optimizely Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Optimizely uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Optimizely yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Optimizely** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/optimizely-tutorial/tutorial_optimizely_samlbase.png)

1. Üzerinde **Optimizely etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/optimizely-tutorial/tutorial_optimizely_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://app.optimizely.net/<instance name>`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak:  `urn:auth0:optimizely:contoso`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Değer, öğreticinin ilerleyen bölümlerinde açıklanan tanımlayıcısı ve gerçek oturum açma URL'si ile güncelleştirir. 

1. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/optimizely-tutorial/tutorial_optimizely_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/optimizely-tutorial/tutorial_general_400.png)

1. Üzerinde **Optimizely yapılandırma** bölümünde **yapılandırma Optimizely** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/optimizely-tutorial/tutorial_optimizely_configure.png) 

1. Çoklu oturum açmayı yapılandırma **Optimizely** yan Optimizely hesap yöneticinize başvurun ve indirilen sağlamak **sertifika (Base64)**, ve **SAML çoklu oturum açma hizmeti URL'si**. 

1. E-postanıza yanıtta Optimizely üzerinde oturum URL'si (SP tarafından başlatılan SSO'yu) ve tanımlayıcı (hizmet sağlayıcı varlık kimliği) değerlerini sağlar.

    a. Kopyalama **SP tarafından başlatılan SSO'yu URL** Optimizely ve içine yapıştırma tarafından sağlanan **işareti bulunan URL'si** metin kutusunda **Optimizely etki alanı ve URL'ler** bölümü Azure portalı 

    b. Kopyalama **hizmet sağlayıcısı varlık kimliği** Optimizely ve içine yapıştırma tarafından sağlanan **tanımlayıcı** metin kutusunda **Optimizely etki alanı ve URL'ler** bölümü Azure portalı 

1. Bir farklı bir tarayıcı penceresinde Optimizely uygulamanıza oturum.

1. Hesap adı üst sağ alt köşesinde tıklayın ve ardından **hesap ayarları**.
   
    ![Azure AD çoklu oturum açma](./media/optimizely-tutorial/tutorial_optimizely_09.png)

1. Hesap sekmesinde, onay kutusunu **SSO etkinleştirme** çoklu oturum açma altında **genel bakış** bölümü.
   
    ![Azure AD çoklu oturum açma](./media/optimizely-tutorial/tutorial_optimizely_10.png)
    
1. **Kaydet**’e tıklayın

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/optimizely-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/optimizely-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/optimizely-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/optimizely-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** Britta simon'un.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-an-optimizely-test-user"></a>Bir Optimizely test kullanıcısı oluşturma

Bu bölümde, Britta Simon Optimizely içinde adlı bir kullanıcı oluşturun.

1. Giriş sayfasında, seçin **ortak çalışanlar** sekmesi.

1. Yeni bir ortak çalışanı projeye eklemek için tıklatın **yeni bir ortak çalışanı**.
   
    ![Bir Azure AD test kullanıcısı oluşturma](./media/optimizely-tutorial/create_aaduser_10.png)

1. E-posta adresi girin ve bunları bir role atayın. Tıklayın **davet**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/optimizely-tutorial/create_aaduser_11.png)

1. Bir e-posta daveti alırlar. E-posta adresini kullanarak oturum açmak için Optimizely sahiptirler.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için Optimizely erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Optimizely için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Optimizely**.

    ![Çoklu oturum açmayı yapılandırın](./media/optimizely-tutorial/tutorial_optimizely_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Optimizely kutucuğa tıkladığınızda, otomatik olarak Optimizely uygulamanıza açan. 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/optimizely-tutorial/tutorial_general_01.png
[2]: ./media/optimizely-tutorial/tutorial_general_02.png
[3]: ./media/optimizely-tutorial/tutorial_general_03.png
[4]: ./media/optimizely-tutorial/tutorial_general_04.png

[100]: ./media/optimizely-tutorial/tutorial_general_100.png

[200]: ./media/optimizely-tutorial/tutorial_general_200.png
[201]: ./media/optimizely-tutorial/tutorial_general_201.png
[202]: ./media/optimizely-tutorial/tutorial_general_202.png
[203]: ./media/optimizely-tutorial/tutorial_general_203.png

