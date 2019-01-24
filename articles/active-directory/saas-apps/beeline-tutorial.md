---
title: 'Öğretici: BeeLine ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: BeeLine ile Azure Active Directory arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 0726859d-1dac-44a0-810b-da56d89039ee
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: bc1859ae14dfe0328479e775ed058cdec12bfcc2
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54822833"
---
# <a name="tutorial-azure-active-directory-integration-with-beeline"></a>Öğretici: BeeLine ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile BeeLine tümleştirme konusunda bilgi edinin.

BeeLine'ı Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- BeeLine erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan BeeLine'nın (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

BeeLine ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- BeeLine çoklu oturum açma abonelik etkin.

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. BeeLine galeri ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-beeline-from-the-gallery"></a>BeeLine galeri ekleme
Azure AD'de BeeLine tümleştirmesini yapılandırmak için BeeLine Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**BeeLine Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **BeeLine**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/beeline-tutorial/tutorial_beeline_search.png)

1. Sonuçlar panelinde seçin **BeeLine**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/beeline-tutorial/tutorial_beeline_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." adlı bir test kullanıcı tabanlı BeeLine ile test etme

Tek iş için oturum açma için Azure AD ne BeeLine karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının BeeLine ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

BeeLine içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma BeeLine ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[BeeLine test kullanıcısı oluşturma](#creating-a-beeline-test-user)**  - kullanıcı Azure AD gösterimini bağlı BeeLine Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve BeeLine uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma BeeLine ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **BeeLine** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/beeline-tutorial/tutorial_beeline_samlbase.png)

1. Üzerinde **BeeLine etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/beeline-tutorial/tutorial_beeline_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://projects.beeline.net/<instancename>`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak:
    | |
    |--|
    | `https://projects.beeline.net/<instancename>/SSO_External.ashx`|
    | `https://projects.beeline.net/<companyname>/SSO_External.ashx` |

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı ve yanıt URL'si ile güncelleştirin. İlgili kişi [BeeLine Destek ekibine](https://www.beeline.com/contact-us/) bu değerleri almak için.
 
1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/beeline-tutorial/tutorial_beeline_certificate.png) 

1. Beeline uygulamanızı belirli bir biçimde SAML onaylamalarını bekliyor. Lütfen birlikte çalışarak [BeeLine Destek ekibine](https://www.beeline.com/contact-us/) ilk uygulamaya eşleşen doğru kullanıcı tanımlayıcısını tanımlamak için. Ayrıca Lütfen kılavuzdan yararlanın [BeeLine Destek ekibine](https://www.beeline.com/contact-us/) bunlar Bu eşleme için kullanmak istediğiniz özniteliği hakkında. Bu özniteliği değeri yönetebileceğiniz **kullanıcı öznitelikleri** uygulama sekmesinde. Aşağıdaki ekran görüntüsü bunun bir örneği gösterilmektedir. Biz burada eşlediğiniz **kullanıcı tanımlayıcısı** ile talep **userprincipalname** özniteliği, her bir başarılı SAML yanıtını Beeline uygulamaya gönderilen benzersiz kullanıcı Kimliğini sağlar.

    ![Çoklu oturum açmayı yapılandırın](./media/beeline-tutorial/tutorial_attribute.png)    

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/beeline-tutorial/tutorial_general_400.png)

1. Üzerinde **BeeLine yapılandırma** bölümünde **yapılandırma BeeLine** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si** ve **SAML varlık kimliği** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/beeline-tutorial/tutorial_beeline_configure.png) 

1. Çoklu oturum açmayı yapılandırma **BeeLine** tarafı, indirilen göndermek için ihtiyacınız **meta veri XML** ve **SAML varlık kimliği**, **oturum kapatma URL'si** için [BeeLine Destek ekibine](https://www.beeline.com/contact-us/).

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/beeline-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/beeline-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/beeline-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/beeline-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-beeline-test-user"></a>BeeLine test kullanıcısı oluşturma

Bu bölümde, Britta Simon Beeline adlı bir kullanıcı oluşturun. Beeline uygulamanın tüm kullanıcıları çoklu oturum açma yapmadan önce uygulamanın sağlanması gerekir. Bu nedenle iş [BeeLine Destek ekibine](https://www.beeline.com/contact-us/) bu kullanıcılar uygulamaya sağlamak için. 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için BeeLine erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon BeeLine için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **BeeLine**.

    ![Çoklu oturum açmayı yapılandırın](./media/beeline-tutorial/tutorial_beeline_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin. Erişim panelinde Beeline kutucuğa tıkladığınızda, otomatik olarak Beeline uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/beeline-tutorial/tutorial_general_01.png
[2]: ./media/beeline-tutorial/tutorial_general_02.png
[3]: ./media/beeline-tutorial/tutorial_general_03.png
[4]: ./media/beeline-tutorial/tutorial_general_04.png

[100]: ./media/beeline-tutorial/tutorial_general_100.png

[200]: ./media/beeline-tutorial/tutorial_general_200.png
[201]: ./media/beeline-tutorial/tutorial_general_201.png
[202]: ./media/beeline-tutorial/tutorial_general_202.png
[203]: ./media/beeline-tutorial/tutorial_general_203.png

