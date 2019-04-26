---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle atomik öğrenme | Microsoft Docs'
description: Azure Active Directory ve atomik öğrenme arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 495f54a6-e6c4-41b0-aafa-a6283d33efc8
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 74df7bd7073716dbbb372a43af809bf56c6cd32c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60440173"
---
# <a name="tutorial-azure-active-directory-integration-with-atomic-learning"></a>Öğretici: Atomik öğrenme ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile atomik öğrenme tümleştirme konusunda bilgi edinin.

Atomik öğrenme Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Atomik öğrenme erişimi, Azure AD'de denetleyebilirsiniz
- Azure AD hesaplarına otomatik olarak imzalanan (çoklu oturum açma) atomik öğrenme açma, kullanıcılarınızın etkinleştirebilirsiniz
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile atomik öğrenme yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir atomik öğrenme çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden atomik öğrenme ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-atomic-learning-from-the-gallery"></a>Galeriden atomik öğrenme ekleme
Azure AD'de atomik öğrenme tümleştirmesini yapılandırmak için atomik öğrenme Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden atomik öğrenme eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **atomik öğrenme**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/atomiclearning-tutorial/tutorial_atomiclearning_search.png)

1. Sonuçlar panelinde seçin **atomik öğrenme**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/atomiclearning-tutorial/tutorial_atomiclearning_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma atomik "Britta Simon." adlı bir test kullanıcı dayalı öğrenme ile test etme

Tek çalışmak için oturum açma için Azure AD ne atomik öğrenme karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve ilgili kullanıcı atomik learning'de arasında bir bağlantı ilişki kurulması gerekir.

Atomik Learning'de değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma atomik öğrenme ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Bir atomik öğrenme test kullanıcısı oluşturma](#creating-an-atomic-learning-test-user)**  - kullanıcı Azure AD gösterimini bağlı atomik learning'de Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve atomik öğrenme uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma atomik öğrenme ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **atomik öğrenme** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/atomiclearning-tutorial/tutorial_atomiclearning_samlbase.png)

1. Üzerinde **atomik öğrenme etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/atomiclearning-tutorial/tutorial_atomiclearning_url.png)

     İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://secure2.atomiclearning.com/sso/shibboleth/<companyname>`
    
    > [!NOTE] 
    > Bu değer, gerçek değil. Bu değer, gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [atomik öğrenme istemci Destek ekibine](mailto:cs@atomiclearning.com) bu değeri alınamıyor. 
 
1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/atomiclearning-tutorial/tutorial_atomiclearning_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/atomiclearning-tutorial/tutorial_general_400.png)

1. Çoklu oturum açmayı yapılandırma **atomik öğrenme** tarafı, indirilen göndermek için ihtiyacınız **meta veri XML** için [atomik öğrenme Destek ekibine](mailto:cs@atomiclearning.com). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/atomiclearning-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/atomiclearning-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/atomiclearning-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/atomiclearning-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-an-atomic-learning-test-user"></a>Bir atomik öğrenme test kullanıcısı oluşturma

Bu bölümde, atomik Learning'de Britta Simon adlı bir kullanıcı oluşturun. Atomik öğrenme tam zamanında sağlama, varsayılan olarak etkin olan destekler. 

Bu bölümde, hiçbir eylem öğesini yoktur. Yeni bir kullanıcı, e-posta adresi kullanarak kullanıcı için henüz mevcut değilse atomik öğrenme erişme denemesi sırasında oluşturulur.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, atomik öğrenme için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon atomik öğrenme atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **atomik öğrenme**.

    ![Çoklu oturum açmayı yapılandırın](./media/atomiclearning-tutorial/tutorial_atomiclearning_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde atomik öğrenme kutucuğa tıkladığınızda, otomatik olarak atomik öğrenme uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)


<!--Image references-->

[1]: ./media/atomiclearning-tutorial/tutorial_general_01.png
[2]: ./media/atomiclearning-tutorial/tutorial_general_02.png
[3]: ./media/atomiclearning-tutorial/tutorial_general_03.png
[4]: ./media/atomiclearning-tutorial/tutorial_general_04.png

[100]: ./media/atomiclearning-tutorial/tutorial_general_100.png

[200]: ./media/atomiclearning-tutorial/tutorial_general_200.png
[201]: ./media/atomiclearning-tutorial/tutorial_general_201.png
[202]: ./media/atomiclearning-tutorial/tutorial_general_202.png
[203]: ./media/atomiclearning-tutorial/tutorial_general_203.png

