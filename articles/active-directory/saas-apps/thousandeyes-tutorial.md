---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile ThousandEyes | Microsoft Docs'
description: Azure Active Directory ve ThousandEyes arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 790e3f1e-1591-4dd6-87df-590b7bf8b4ba
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2018
ms.author: jeedes
ms.openlocfilehash: 06de55fd2bd1064e93e38fe0ff0cfb13fdc1463a
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55175443"
---
# <a name="tutorial-azure-active-directory-integration-with-thousandeyes"></a>Öğretici: ThousandEyes ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile ThousandEyes tümleştirme konusunda bilgi edinin.

Azure AD ile ThousandEyes tümleştirme ile aşağıdaki avantajları sağlar:

- ThousandEyes erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için ThousandEyes (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile ThousandEyes yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik ThousandEyes çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme burada alabilirsiniz: [Deneme teklifi](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden ThousandEyes ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-thousandeyes-from-the-gallery"></a>Galeriden ThousandEyes ekleme
Azure AD'de ThousandEyes tümleştirmesini yapılandırmak için ThousandEyes Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden ThousandEyes eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **ThousandEyes**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/thousandeyes-tutorial/tutorial_thousandeyes_search.png)

1. Sonuçlar panelinde seçin **ThousandEyes**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/thousandeyes-tutorial/tutorial_thousandeyes_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı ThousandEyes sınayın.

Tek iş için oturum açma için Azure AD ne ThousandEyes karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının ThousandEyes ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

ThousandEyes içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma ThousandEyes ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[ThousandEyes test kullanıcısı oluşturma](#creating-a-thousandeyes-test-user)**  - kullanıcı Azure AD gösterimini bağlı ThousandEyes Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve ThousandEyes uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile ThousandEyes yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **ThousandEyes** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açmayı yapılandırın](./media/thousandeyes-tutorial/tutorial_thousandeyes_samlbase.png)

1. Üzerinde **ThousandEyes etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/thousandeyes-tutorial/tutorial_thousandeyes_url.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL: `https://app.thousandeyes.com/login/sso`

1. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/thousandeyes-tutorial/tutorial_thousandeyes_certificate.png)

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/thousandeyes-tutorial/tutorial_general_400.png)

1. Üzerinde **ThousandEyes yapılandırma** bölümünde **yapılandırma ThousandEyes** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/thousandeyes-tutorial/tutorial_thousandeyes_configure.png) 

1. Farklı bir web tarayıcı penceresinde oturum açın, **ThousandEyes** yönetici olarak şirketin site.

1. Üstteki menüden **ayarları**.

    ![Ayarları](./media/thousandeyes-tutorial/ic790066.png "ayarları")

1. Tıklayın **hesabı**

    ![Hesap](./media/thousandeyes-tutorial/ic790067.png "hesabı")

1. Tıklayın **güvenlik ve kimlik doğrulaması** sekmesi.

    ![Güvenlik ve kimlik doğrulaması](./media/thousandeyes-tutorial/ic790068.png "güvenlik ve kimlik doğrulaması")

1. İçinde **Kurulum çoklu oturum açma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açma Kurulumu](./media/thousandeyes-tutorial/ic790069.png "çoklu oturum açma Kurulumu")

    a. Seçin **çoklu oturum açmayı etkinleştirme**.

    b. İçinde **oturum açma sayfası URL'si** metin kutusu, yapıştırma **SAML çoklu oturum açma hizmeti URL'si**, hangi Azure Portalı'ndan kopyaladığınız.

    c. İçinde **oturum kapatma sayfasını URL'si** metin kutusu, yapıştırma **oturum kapatma URL'si**, hangi Azure Portalı'ndan kopyaladığınız.

    d. **Kimlik sağlayıcısını veren** metin kutusu, yapıştırma **SAML varlık kimliği**, hangi Azure Portalı'ndan kopyaladığınız.

    e. İçinde **doğrulama sertifikası**, tıklayın **dosya**ve sonra Azure portalından indirilen sertifikayı karşıya yükleyin.

    f. **Kaydet**’e tıklayın.

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/thousandeyes-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/thousandeyes-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/thousandeyes-tutorial/create_aaduser_03.png)

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Bir Azure AD test kullanıcısı oluşturma](./media/thousandeyes-tutorial/create_aaduser_04.png)

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.

### <a name="creating-a-thousandeyes-test-user"></a>ThousandEyes test kullanıcısı oluşturma

Bu bölümün amacı ThousandEyes Britta Simon adlı bir kullanıcı oluşturmaktır. ThousandEyes otomatik kullanıcı hazırlama, varsayılan olarak etkin olan destekler. Daha fazla ayrıntı bulabilirsiniz [burada](thousandeyes-provisioning-tutorial.md) otomatik kullanıcı sağlamayı yapılandırma.

**Kullanıcı el ile oluşturmanız gerekiyorsa, aşağıdaki adımları uygulayın:**

1. ThousandEyes şirket sitenize yönetici olarak oturum açın.

1. Tıklayın **ayarları**.

    ![Ayarları](./media/thousandeyes-tutorial/IC790066.png "ayarları")

1. Tıklayın **hesabı**.

    ![Hesap](./media/thousandeyes-tutorial/IC790067.png "hesabı")

1. Tıklayın **hesapları k & ullanıcıların** sekmesi.

    ![K & ullanıcıların hesapları](./media/thousandeyes-tutorial/IC790073.png "k & ullanıcıların hesapları")

1. İçinde **Add Users & hesapları** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı hesaplarını eklemek](./media/thousandeyes-tutorial/IC790074.png "kullanıcı hesaplarını ekleyin")

    a. İçinde **adı** metin kutusuna kullanıcı adını yazın ister **Britta Simon**.

    b. İçinde **e-posta** metin kutusuna kullanıcı e-posta türünü ister **brittasimon@contoso.com**.

    b. Tıklayın **yeni kullanıcı hesabına eklemek**.

    > [!NOTE]
    > Azure Active Directory hesap sahibinin onaylayın ve hesap etkinleştirme bağlantısı içeren bir e-posta alırsınız.

> [!NOTE]
> Herhangi diğer ThousandEyes kullanıcı hesabı oluşturma araçları kullanabilir veya API'leri tarafından ThousandEyes sağlamak için Azure Active Directory kullanıcı hesaplarını sağlanan.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için ThousandEyes erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon ThousandEyes için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **ThousandEyes**.

    ![Çoklu oturum açmayı yapılandırın](./media/thousandeyes-tutorial/tutorial_thousandeyes_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde ThousandEyes kutucuğa tıkladığınızda, otomatik olarak ThousandEyes uygulamanıza açan.

Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
* [Kullanıcı sağlamayı yapılandırma](thousandeyes-provisioning-tutorial.md)


<!--Image references-->

[1]: ./media/thousandeyes-tutorial/tutorial_general_01.png
[2]: ./media/thousandeyes-tutorial/tutorial_general_02.png
[3]: ./media/thousandeyes-tutorial/tutorial_general_03.png
[4]: ./media/thousandeyes-tutorial/tutorial_general_04.png

[100]: ./media/thousandeyes-tutorial/tutorial_general_100.png

[200]: ./media/thousandeyes-tutorial/tutorial_general_200.png
[201]: ./media/thousandeyes-tutorial/tutorial_general_201.png
[202]: ./media/thousandeyes-tutorial/tutorial_general_202.png
[203]: ./media/thousandeyes-tutorial/tutorial_general_203.png
