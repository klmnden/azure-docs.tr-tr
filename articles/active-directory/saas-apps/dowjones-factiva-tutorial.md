---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle Ceresinde Jones Factiva | Microsoft Docs'
description: Azure Active Directory ve Ceresinde Jones Factiva arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: b36e97e8-37a6-4096-a894-530427ee1331
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/11/2017
ms.author: jeedes
ms.openlocfilehash: 3f7d0f4fdde3e3ce46e4398500505085639ac33a
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54807448"
---
# <a name="tutorial-azure-active-directory-integration-with-dow-jones-factiva"></a>Öğretici: Pencere Jones Factiva ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Ceresinde Jones Factiva tümleştirme konusunda bilgi edinin.

Pencere Jones Factiva Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Pencere Jones Factiva erişimi, Azure AD'de denetleyebilirsiniz
- Azure AD hesaplarına otomatik olarak imzalanan (çoklu oturum açma) için Ceresinde Jones Factiva açma, kullanıcılarınızın etkinleştirebilirsiniz
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Ceresinde Jones Factiva yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir pencere Jones Factiva çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Ceresinde Jones Factiva ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-dow-jones-factiva-from-the-gallery"></a>Galeriden Ceresinde Jones Factiva ekleme
Azure AD'de Ceresinde Jones Factiva tümleştirmesini yapılandırmak için pencere Jones Factiva Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Ceresinde Jones Factiva eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **Ceresinde Jones Factiva**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/dowjones-factiva-tutorial/tutorial_dowjonesfactiva_search.png)

1. Sonuçlar panelinde seçin **Ceresinde Jones Factiva**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/dowjones-factiva-tutorial/tutorial_dowjonesfactiva_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Ceresinde Jones "Britta Simon." adlı bir test kullanıcı tabanlı Factiva ile test etme

Tek iş için oturum açma için Azure AD ne Ceresinde Jones Factiva karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Ceresinde Jones Factiva ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Pencere Jones Factiva içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Ceresinde Jones Factiva ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Bir pencere Jones Factiva test kullanıcısı oluşturma](#creating-a-dow-jones-factiva-test-user)**  - kullanıcı Azure AD gösterimini bağlantılı bir pencere Jones Factiva Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Ceresinde Jones Factiva uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Ceresinde Jones Factiva yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Ceresinde Jones Factiva** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/dowjones-factiva-tutorial/tutorial_dowjonesfactiva_samlbase.png)

1. Üzerinde **Ceresinde Jones Factiva etki alanı ve URL'ler** bölümü, kullanıcı gerekmez uygulama zaten Azure ile önceden tümleştirilmiştir gibi tüm adımları gerçekleştirin.

    ![Çoklu oturum açmayı yapılandırın](./media/dowjones-factiva-tutorial/tutorial_dowjonesfactiva_url.png)

1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/dowjones-factiva-tutorial/tutorial_dowjonesfactiva_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/dowjones-factiva-tutorial/tutorial_general_400.png)

1. Çoklu oturum açmayı yapılandırma **Ceresinde Jones Factiva** tarafı, indirilen göndermek için ihtiyacınız **meta veri XML** için [Ceresinde Jones Factiva Destek ekibine](https://www.dowjones.com/contact/). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/dowjones-factiva-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/dowjones-factiva-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/dowjones-factiva-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/dowjones-factiva-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-dow-jones-factiva-test-user"></a>Bir pencere Jones Factiva test kullanıcısı oluşturma

Bu bölümde, Britta Simon Ceresinde Jones Factiva içinde adlı bir kullanıcı oluşturun. Lütfen pencere ile çalışarak [Jones Factiva Destek ekibine](https://www.dowjones.com/contact/) Ceresinde Jones Factiva platform kullanıcıları eklemek için.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma için Ceresinde Jones Factiva erişim vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Ceresinde Jones Factiva için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Ceresinde Jones Factiva**.

    ![Çoklu oturum açmayı yapılandırın](./media/dowjones-factiva-tutorial/tutorial_dowjonesfactiva_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Ceresinde Jones Factiva kutucuğa tıkladığınızda, otomatik olarak Ceresinde Jones Factiva uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/dowjones-factiva-tutorial/tutorial_general_01.png
[2]: ./media/dowjones-factiva-tutorial/tutorial_general_02.png
[3]: ./media/dowjones-factiva-tutorial/tutorial_general_03.png
[4]: ./media/dowjones-factiva-tutorial/tutorial_general_04.png

[100]: ./media/dowjones-factiva-tutorial/tutorial_general_100.png

[200]: ./media/dowjones-factiva-tutorial/tutorial_general_200.png
[201]: ./media/dowjones-factiva-tutorial/tutorial_general_201.png
[202]: ./media/dowjones-factiva-tutorial/tutorial_general_202.png
[203]: ./media/dowjones-factiva-tutorial/tutorial_general_203.png

