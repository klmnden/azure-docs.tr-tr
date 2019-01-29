---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Picturepark | Microsoft Docs'
description: Azure Active Directory ve Picturepark arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 31c21cd4-9c00-4cad-9538-a13996dc872f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2017
ms.author: jeedes
ms.openlocfilehash: 10ddfa2f7b2b17c23da1e67474a4b464780bc2e6
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55163654"
---
# <a name="tutorial-azure-active-directory-integration-with-picturepark"></a>Öğretici: Picturepark ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Picturepark tümleştirme konusunda bilgi edinin.

Azure AD ile Picturepark tümleştirme ile aşağıdaki avantajları sağlar:

- Picturepark erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için Picturepark (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Picturepark yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik Picturepark çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Picturepark ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-picturepark-from-the-gallery"></a>Galeriden Picturepark ekleme
Azure AD'de Picturepark tümleştirmesini yapılandırmak için Picturepark Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Picturepark eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **Picturepark**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/picturepark-tutorial/tutorial_picturepark_search.png)

1. Sonuçlar panelinde seçin **Picturepark**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/picturepark-tutorial/tutorial_picturepark_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Picturepark sınayın.

Tek iş için oturum açma için Azure AD ne Picturepark karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Picturepark ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Picturepark içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Picturepark ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Picturepark test kullanıcısı oluşturma](#creating-a-picturepark-test-user)**  - kullanıcı Azure AD gösterimini bağlı Picturepark Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Picturepark uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Picturepark yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Picturepark** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/picturepark-tutorial/tutorial_picturepark_samlbase.png)

1. Üzerinde **Picturepark etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/picturepark-tutorial/tutorial_picturepark_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<companyname>.picturepark.com`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: 
    
    |  |
    |--|
    | `https://<companyname>.current-picturepark.com`|
    | `https://<companyname>.picturepark.com`|
    | `https://<companyname>.next-picturepark.com`|
    | |

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [Picturepark istemci Destek ekibine](https://picturepark.com/about/contact/) bu değerleri almak için. 
 
1. Üzerinde **SAML imzalama sertifikası** bölümünde, kopya **parmak İZİ** sertifika değeri.

    ![Çoklu oturum açmayı yapılandırın](./media/picturepark-tutorial/tutorial_picturepark_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/picturepark-tutorial/tutorial_general_400.png)

1. Üzerinde **Picturepark yapılandırma** bölümünde **yapılandırma Picturepark** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/picturepark-tutorial/tutorial_picturepark_configure.png) 

1. Farklı bir web tarayıcı penceresinde Picturepark şirket sitenize yönetici olarak oturum.

1. Üst araç çubuğunda tıklatın **Yönetimsel Araçlar**ve ardından **Yönetim Konsolu**.
   
    ![Yönetim Konsolu](./media/picturepark-tutorial/ic795062.png "Yönetim Konsolu")

1. Tıklayın **kimlik doğrulaması**ve ardından **kimlik sağlayıcıları**.
   
    ![Kimlik doğrulaması](./media/picturepark-tutorial/ic795063.png "kimlik doğrulaması")

1. İçinde **kimlik sağlayıcı Yapılandırması** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Kimlik sağlayıcısı Yapılandırması](./media/picturepark-tutorial/ic795064.png "kimlik sağlayıcı yapılandırması")
   
    a. **Ekle**'ye tıklayın.
  
    b. Yapılandırma için bir ad yazın.
   
    c. Seçin **varsayılan olarak ayarla**.
   
    d. İçinde **veren URI** metin değerini yapıştırın **SAML çoklu oturum açma hizmeti URL'si** , Azure Portalı'ndan kopyaladığınız.
   
    e. İçinde **güvenilir verenin parmak izi** metin değerini yapıştırın **parmak izi** kopyalanan **SAML imzalama sertifikası** bölümü. 

1. Tıklayın **JoinDefaultUsersGroup**.

1. Ayarlanacak **Emailaddress** özniteliğini **talep** metin kutusuna `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` tıklatıp **Kaydet**.

      ![Yapılandırma](./media/picturepark-tutorial/ic795065.png "yapılandırma")

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/picturepark-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/picturepark-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/picturepark-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/picturepark-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-picturepark-test-user"></a>Picturepark test kullanıcısı oluşturma

Picturepark açarken Azure AD kullanıcılarının etkinleştirmek için bunların Picturepark sağlanması gerekir. Picturepark söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açın, **Picturepark** Kiracı.

1. Üst araç çubuğunda tıklatın **Yönetimsel Araçlar**ve ardından **kullanıcılar**.
   
    ![Kullanıcılar](./media/picturepark-tutorial/ic795067.png "kullanıcılar")

1. İçinde **kullanıcılar genel bakış** sekmesinde **yeni**.
   
    ![Kullanıcı Yönetimi](./media/picturepark-tutorial/ic795068.png "kullanıcı yönetimi")

1. Üzerinde **Create User** iletişim kutusunda, geçerli bir Azure Active Directory istediğiniz kullanıcı sağlamak için aşağıdaki adımları gerçekleştirin:
   
    ![Kullanıcı oluşturma](./media/picturepark-tutorial/ic795069.png "kullanıcı oluşturma")
   
    a. İçinde **e-posta adresi** metin kutusuna **e-posta adresi** kullanıcının **BrittaSimon@contoso.com**.  
   
    b. İçinde **parola** ve **parolayı onayla** metin kutuları, türü **parola** BrittaSimon biri. 
   
    c. İçinde **ad** metin kutusuna **ad** kullanıcının **Britta**. 
   
    d. İçinde **Soyadı** metin kutusuna **Soyadı** kullanıcının **Simon**.
   
    e. İçinde **şirket** metin kutusuna **şirket adı** kullanıcının. 
   
    f. İçinde **Ülke** metin seçme **Ülke** kullanıcının.
  
    g. İçinde **ZIP** metin kutusuna **posta kodu** şehir.
   
    h. İçinde **Şehir** metin kutusuna **şehir adı** kullanıcının.

    i. Seçin bir **dil**.
   
    j. **Oluştur**’a tıklayın.

>[!NOTE]
>Herhangi diğer Picturepark kullanıcı hesabı oluşturma araçları kullanabilir veya API Azure AD'ye kullanıcı hesapları sağlamak için Picturepark tarafından sağlanan.
> 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için Picturepark erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Picturepark için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Picturepark**.

    ![Çoklu oturum açmayı yapılandırın](./media/picturepark-tutorial/tutorial_picturepark_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Picturepark kutucuğa tıkladığınızda, otomatik olarak Picturepark uygulamanıza açan. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/picturepark-tutorial/tutorial_general_01.png
[2]: ./media/picturepark-tutorial/tutorial_general_02.png
[3]: ./media/picturepark-tutorial/tutorial_general_03.png
[4]: ./media/picturepark-tutorial/tutorial_general_04.png

[100]: ./media/picturepark-tutorial/tutorial_general_100.png

[200]: ./media/picturepark-tutorial/tutorial_general_200.png
[201]: ./media/picturepark-tutorial/tutorial_general_201.png
[202]: ./media/picturepark-tutorial/tutorial_general_202.png
[203]: ./media/picturepark-tutorial/tutorial_general_203.png

