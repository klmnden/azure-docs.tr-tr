---
title: 'Öğretici: Microsoft Azure için bulut Yönetim Portalı ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Microsoft Azure için bulut Yönetim Portalı arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 4ea9f47c-25ca-42b0-a878-9e7aa6f34973
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 0cd14a308758701e207e0b1ee6d3591c4b0347bd
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39427644"
---
# <a name="tutorial-azure-active-directory-integration-with-cloud-management-portal-for-microsoft-azure"></a>Öğretici: Microsoft Azure için bulut Yönetim Portalı ile Azure Active Directory Tümleştirme

Bu öğreticide, Microsoft Azure için bulut Yönetim Portalı, Azure Active Directory (Azure AD) ile tümleştirme konusunda bilgi edinin.

Microsoft Azure için bulut Yönetim Portalı, Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Microsoft Azure Yönetim Portalı'nı bulut erişimi, Azure AD'de denetleyebilirsiniz
- Azure AD hesaplarına otomatik olarak imzalanan (çoklu oturum açma) Microsoft Azure için bulut Yönetimi portalı açma, kullanıcılarınızın etkinleştirebilirsiniz
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Microsoft Azure için bulut Yönetim Portalı ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Microsoft Azure çoklu oturum açma etkin abonelik için bir bulut Yönetim Portalı

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Microsoft Azure için bulut Yönetim Portalı ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-cloud-management-portal-for-microsoft-azure-from-the-gallery"></a>Galeriden Microsoft Azure için bulut Yönetim Portalı ekleme
Microsoft Azure için bulut Yönetim Portalı'nın Azure AD'ye tümleştirmesini yapılandırmak için Microsoft Azure için bulut Yönetimi portalını Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Microsoft Azure için bulut Yönetimi portalını eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **Microsoft Azure için bulut Yönetimi portalını**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/newsignature-tutorial/tutorial_newsignature_search.png)

1. Sonuçlar panelinde seçin **Microsoft Azure için bulut Yönetimi portalını**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/newsignature-tutorial/tutorial_newsignature_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Microsoft Azure için bulut Yönetim Portalı ile test edin.

Tek çalışmak için oturum açma için Azure AD ne karşılık gelen kullanıcının Microsoft Azure için bulut Yönetim Portalı'nda bir kullanıcının Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile ilgili kullanıcı için Microsoft Azure yönetim portalında bulut arasında bir bağlantı ilişki kurulması gerekir.

Microsoft Azure için bulut Yönetim Portalı'nda değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma için Microsoft Azure bulut Yönetim Portalı ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Microsoft Azure test kullanıcısı için bir bulut Yönetim Portalı oluşturma](#creating-a-cloud-management-portal-for-microsoft-azure-test-user)**  - bulut için kullanıcı Azure AD gösterimini bağlantılı bir Microsoft Azure Yönetim Portalı'nda Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve bulut yönetim portalınızda Microsoft Azure uygulaması için çoklu oturum açmayı yapılandırın.

**Microsoft Azure için bulut Yönetim Portalı ile Azure AD çoklu oturum açmayı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Microsoft Azure için bulut Yönetimi portalını** uygulama tümleştirme sayfası, tıklayın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/newsignature-tutorial/tutorial_newsignature_samlbase.png)

1. Üzerinde **bulut Microsoft Azure etki alanı ve URL'ler için Yönetim Portalı** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/newsignature-tutorial/tutorial_newsignature_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL kullanarak aşağıdaki düzenler: 
    
    | |
    |--|
    | `https://portal.newsignature.com/<instancename>` |   
    | `https://portal.igcm.com/<instancename>` |
    
    b. İçinde **tanımlayıcı** metin kutusuna bir URL kullanarak aşağıdaki düzenler: 
    
    | |
    |--|
    | `https://<subdomain>.igcm.com` |
    | `https://<subdomain>.newsignature.com` |

    c. İçinde **yanıt URL'si** metin kutusuna bir URL kullanarak aşağıdaki düzenler: 
    
    | |
    |--|
    | `https://<subdomain>.igcm.com/<instancename>` |
    | `https://<subdomain>.newsignature.com` |
    | `https://<subdomain>.newsignature.com/<instancename>` |

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si, tanımlayıcı ve yanıt URL'si ile güncelleştirin. İlgili kişi [bulut Yönetim Portalı için Microsoft Azure müşteri destek ekibi](mailto:jczernuszka@newsignature.com) bu değerleri almak için. 
 
1. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/newsignature-tutorial/tutorial_newsignature_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/newsignature-tutorial/tutorial_general_400.png)

1. Üzerinde **bulut Yönetim Portalı için Microsoft Azure yapılandırma** bölümünde **Microsoft Azure için bulut Yönetim Portalı yapılandırma** açmak için **yapılandırma oturum açma** pencere. Kopyalama **oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/newsignature-tutorial/tutorial_newsignature_configure.png) 

1. Çoklu oturum açmayı yapılandırma **Microsoft Azure için bulut Yönetimi portalını** tarafı, indirilen göndermek için ihtiyacınız **sertifika**, **oturum kapatma URL'si**,  **SAML çoklu oturum açma hizmeti URL'si** ve **SAML varlık kimliği** için [bulut Yönetim Portalı için Microsoft Azure destek ekibi](mailto:jczernuszka@newsignature.com). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/newsignature-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/newsignature-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/newsignature-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/newsignature-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-cloud-management-portal-for-microsoft-azure-test-user"></a>Bir bulut Yönetim Portalı için Microsoft Azure test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon, Microsoft Azure için bulut Yönetim Portalı'nda adlı bir kullanıcı oluşturmaktır. Lütfen birlikte çalışarak [bulut Yönetim Portalı için Microsoft Azure destek ekibi](mailto:jczernuszka@newsignature.com) Microsoft Azure hesabı için bulut Yönetim Portalı'nda kullanıcıları eklemek için.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Microsoft Azure için bulut yönetim portalına erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Microsoft Azure için bulut Yönetimi portalını Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Microsoft Azure için bulut Yönetimi portalını**.

    ![Çoklu oturum açmayı yapılandırın](./media/newsignature-tutorial/tutorial_newsignature_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümün amacı, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test sağlamaktır.
Bulut Yönetim Portalı erişim Paneli'nde Microsoft Azure kutucuğa tıkladığınızda, otomatik olarak bulut Yönetim Portalı'na için Microsoft Azure uygulama açan.

Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/newsignature-tutorial/tutorial_general_01.png
[2]: ./media/newsignature-tutorial/tutorial_general_02.png
[3]: ./media/newsignature-tutorial/tutorial_general_03.png
[4]: ./media/newsignature-tutorial/tutorial_general_04.png

[100]: ./media/newsignature-tutorial/tutorial_general_100.png

[200]: ./media/newsignature-tutorial/tutorial_general_200.png
[201]: ./media/newsignature-tutorial/tutorial_general_201.png
[202]: ./media/newsignature-tutorial/tutorial_general_202.png
[203]: ./media/newsignature-tutorial/tutorial_general_203.png

