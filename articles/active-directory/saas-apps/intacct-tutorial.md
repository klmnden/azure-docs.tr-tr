---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Intacct | Microsoft Docs'
description: Azure Active Directory ve Intacct arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 92518e02-a62c-4b1b-a8e9-2803eb2b49ac
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: d7075894bde20f56c9323568ceffe8cad4118cbc
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54810100"
---
# <a name="tutorial-azure-active-directory-integration-with-intacct"></a>Öğretici: Intacct ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Intacct tümleştirme konusunda bilgi edinin.

Azure AD ile Intacct tümleştirme ile aşağıdaki avantajları sağlar:

- Intacct erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için Intacct (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Intacct yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik bir Intacct çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Intacct ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-intacct-from-the-gallery"></a>Galeriden Intacct ekleme
Azure AD'de Intacct tümleştirmesini yapılandırmak için Intacct Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Intacct eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **Intacct**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/intacct-tutorial/tutorial_intacct_search.png)

1. Sonuçlar panelinde seçin **Intacct**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/intacct-tutorial/tutorial_intacct_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Intacct sınayın.

Tek iş için oturum açma için Azure AD ne Intacct karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Intacct ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Intacct içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Intacct ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Bir Intacct test kullanıcısı oluşturma](#creating-an-intacct-test-user)**  - kullanıcı Azure AD gösterimini bağlı Intacct Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Intacct uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Intacct yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Intacct** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/intacct-tutorial/tutorial_intacct_samlbase.png)

1. Üzerinde **Intacct etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/intacct-tutorial/tutorial_intacct_url.png)

    İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak:
    | |
    |--|
    | `https://<companyname>.intacct.com/ia/acct/sso_response.phtml`|
    | `https://www.intacct.com/ia/acct/sso_response.phtml` |

    > [!NOTE] 
    > Bu değer, gerçek değil. Bu değer, gerçek yanıt URL'si ile güncelleştirin. İlgili kişi [Intacct Destek ekibine](https://us.intacct.com/support) bu değeri alınamıyor.

1. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/intacct-tutorial/tutorial_intacct_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/intacct-tutorial/tutorial_general_400.png)

1. Üzerinde **Intacct yapılandırma** bölümünde **yapılandırma Intacct** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/intacct-tutorial/tutorial_intacct_configure.png) 

1. Farklı bir web tarayıcı penceresinde Intacct şirketinizin sitesi için bir yönetici olarak oturum açın.

1. Tıklayın **şirket** sekmesine ve ardından **şirket bilgisi**.

    ![Şirket](./media/intacct-tutorial/ic790037.png "şirket")

1. Tıklayın **güvenlik** sekmesine ve ardından **Düzenle**.

    ![Güvenlik](./media/intacct-tutorial/ic790038.png "güvenlik")

1. İçinde **çoklu oturum açma (SSO)** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum](./media/intacct-tutorial/ic790039.png "tekli oturum")

    a. Seçin **etkinleştirme çoklu oturum açma**.

    b. Olarak **kimlik sağlayıcısı türü**seçin **SAML 2.0**.

    c. İçinde **veren URL'si** metin değerini yapıştırın **SAML varlık kimliği** , Azure Portalı'ndan kopyaladığınız.
   
    d. İçinde **oturum açma URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmeti URL'si** , Azure Portalı'ndan kopyaladığınız.

    e. Açık, **base-64** kodlanmış sertifika Not Defteri'nde, içeriğini, panoya kopyalayın ve ardından ona yapıştırın **sertifika** kutusu.
   
    f. **Kaydet**’e tıklayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/intacct-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/intacct-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/intacct-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/intacct-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-an-intacct-test-user"></a>Bir Intacct test kullanıcısı oluşturma

Bunlar için Intacct oturum açabilmeniz Azure AD kullanıcıları ayarlamak için bunların Intacct sağlanması gerekir. Intacct için sağlama bir el ile gerçekleştirilen bir görevdir.

**Kullanıcı hesapları sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açın, **Intacct** Kiracı.

1. Tıklayın **şirket** sekmesine ve ardından **kullanıcılar**.

    ![Kullanıcılar](./media/intacct-tutorial/ic790041.png "kullanıcılar")
1. Tıklayın **Ekle** sekmesi.

    ![Ekleme](./media/intacct-tutorial/ic790042.png "Ekle")
1. İçinde **kullanıcı bilgilerini** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı bilgilerini](./media/intacct-tutorial/ic790043.png "kullanıcı bilgileri")

    a. Girin **kullanıcı kimliği**, **Soyadı**, **ad**, **e-posta adresi**, **başlık**ve **Telefon** zbilgisayarlar istediğiniz bir Azure AD hesabının **kullanıcı bilgilerini** bölümü.

    b. Seçin **yönetici ayrıcalıkları** sağlamak istediğiniz bir Azure AD hesabı.
   
    c. **Kaydet**’e tıklayın. Azure AD hesap sahibinin e-posta alır ve etkin hale gelir önce hesabını onaylamak için bir bağlantı izler.

>[!NOTE]
>Azure AD kullanıcı hesapları sağlamak için diğer Intacct kullanıcı hesabı oluşturma araçları veya Intacct tarafından sağlanan API'leri kullanabilirsiniz.
        
### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için Intacct erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Intacct için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Intacct**.

    ![Çoklu oturum açmayı yapılandırın](./media/intacct-tutorial/tutorial_intacct_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test.

Erişim paneli Intacct kutucuğa tıkladığınızda, size otomatik olarak Intacct uygulamanıza oturum açmanız.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/intacct-tutorial/tutorial_general_01.png
[2]: ./media/intacct-tutorial/tutorial_general_02.png
[3]: ./media/intacct-tutorial/tutorial_general_03.png
[4]: ./media/intacct-tutorial/tutorial_general_04.png

[100]: ./media/intacct-tutorial/tutorial_general_100.png

[200]: ./media/intacct-tutorial/tutorial_general_200.png
[201]: ./media/intacct-tutorial/tutorial_general_201.png
[202]: ./media/intacct-tutorial/tutorial_general_202.png
[203]: ./media/intacct-tutorial/tutorial_general_203.png

