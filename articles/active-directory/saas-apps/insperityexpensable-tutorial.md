---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle Insperity ExpensAble | Microsoft Docs'
description: Azure Active Directory ve Insperity ExpensAble arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: c579c453-580e-417d-8a5e-9b6b352795c0
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: jeedes
ms.openlocfilehash: 2baeffe5d400d568ba3c2dd8e1cba06501ad23d7
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55198019"
---
# <a name="tutorial-azure-active-directory-integration-with-insperity-expensable"></a>Öğretici: Azure Active Directory tümleştirmesiyle Insperity ExpensAble

Bu öğreticide, Azure Active Directory (Azure AD) ile Insperity ExpensAble tümleştirme konusunda bilgi edinin.

Insperity ExpensAble Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Insperity ExpensAble erişimi, Azure AD'de denetleyebilirsiniz
- Azure AD hesaplarına otomatik olarak imzalanan Insperity ExpensAble için (çoklu oturum açma) açma, kullanıcılarınızın etkinleştirebilirsiniz
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Insperity ExpensAble ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir Insperity ExpensAble çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Insperity ExpensAble ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-insperity-expensable-from-the-gallery"></a>Galeriden Insperity ExpensAble ekleme
Azure AD'de Insperity ExpensAble'nın tümleştirmesini yapılandırmak için Insperity ExpensAble Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Insperity ExpensAble eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **Insperity ExpensAble**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/insperityexpensable-tutorial/tutorial_insperityexpensable_search.png)

1. Sonuçlar panelinde seçin **Insperity ExpensAble**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/insperityexpensable-tutorial/tutorial_insperityexpensable_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Insperity ExpensAble ile test edin.

Tek iş için oturum açma için Azure AD ne Insperity ExpensAble karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile ilgili kullanıcı Insperity ExpensAble arasında bir bağlantı ilişki kurulması gerekir.

ExpensAble Insperity içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Insperity ExpensAble ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Insperity ExpensAble bir test kullanıcısı oluşturma](#creating-an-insperity-expensable-test-user)**  - Insperity kullanıcı Azure AD gösterimini bağlı ExpensAble Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Insperity ExpensAble uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma Insperity ExpensAble ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Insperity ExpensAble** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/insperityexpensable-tutorial/tutorial_insperityexpensable_samlbase.png)

1. Üzerinde **Insperity ExpensAble etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/insperityexpensable-tutorial/tutorial_insperityexpensable_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://server.expensable.com/esapp/Authenticate?companyId=<company ID>`

    > [!NOTE] 
    > Bu değer, gerçek değil. Bu değer, gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [Insperity ExpensAble istemci Destek ekibine](http://expensable.com/support/support-overview) bu değeri alınamıyor. 
 
1. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/insperityexpensable-tutorial/tutorial_insperityexpensable_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/insperityexpensable-tutorial/tutorial_general_400.png)

1. Üzerinde **Insperity ExpensAble yapılandırma** bölümünde **Insperity ExpensAble yapılandırma** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/insperityexpensable-tutorial/tutorial_insperityexpensable_configure.png) 

1. Çoklu oturum açmayı yapılandırma **Insperity ExpensAble** tarafı, indirilen göndermek için ihtiyacınız **meta veri XML**, **SAML çoklu oturum açma hizmeti URL'si** ve  **SAML varlık kimliği** için [Insperity ExpensAble Destek ekibine](http://expensable.com/support/support-overview). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/insperityexpensable-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/insperityexpensable-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/insperityexpensable-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/insperityexpensable-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-an-insperity-expensable-test-user"></a>Insperity ExpensAble bir test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon Insperity ExpensAble adlı bir kullanıcı oluşturmaktır. Lütfen birlikte çalışarak [Insperity ExpensAble Destek ekibine](http://expensable.com/support/support-overview) Insperity ExpensAble hesabında kullanıcıları eklemek için.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma Insperity ExpensAble için erişim vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Insperity ExpensAble için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Insperity ExpensAble**.

    ![Çoklu oturum açmayı yapılandırın](./media/insperityexpensable-tutorial/tutorial_insperityexpensable_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Insperity ExpensAble kutucuğa tıkladığınızda, otomatik olarak Insperity ExpensAble uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/insperityexpensable-tutorial/tutorial_general_01.png
[2]: ./media/insperityexpensable-tutorial/tutorial_general_02.png
[3]: ./media/insperityexpensable-tutorial/tutorial_general_03.png
[4]: ./media/insperityexpensable-tutorial/tutorial_general_04.png
[100]: ./media/insperityexpensable-tutorial/tutorial_general_100.png
[200]: ./media/insperityexpensable-tutorial/tutorial_general_200.png
[201]: ./media/insperityexpensable-tutorial/tutorial_general_201.png
[202]: ./media/insperityexpensable-tutorial/tutorial_general_202.png
[203]: ./media/insperityexpensable-tutorial/tutorial_general_203.png

