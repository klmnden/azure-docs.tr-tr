---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Jostle | Microsoft Docs'
description: Azure Active Directory ve Jostle arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 9ca4ca1f-8f68-4225-81a6-1666b486d6a8
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: 0722cb2e583ae94b7c5dc8591e0c14ea1d359fe9
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55183178"
---
# <a name="tutorial-azure-active-directory-integration-with-jostle"></a>Öğretici: Jostle ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Jostle tümleştirme konusunda bilgi edinin.

Azure AD ile Jostle tümleştirme ile aşağıdaki avantajları sağlar:

- Jostle erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan Jostle (çoklu oturum açma) için Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Jostle yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir Jostle çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin.
Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Jostle ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-jostle-from-the-gallery"></a>Galeriden Jostle ekleme
Azure AD'de Jostle tümleştirmesini yapılandırmak için Jostle Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Jostle eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]

1. Tıklayın **Ekle** pencerenin üst kısmındaki.

    ![add_01](./media/jostle-tutorial/add_01.png)

1. Arama kutusuna altında **uygulama ekleme** türü **Jostle**.

    ![add_02](./media/jostle-tutorial/add_02.png)

1. Sonuçlar panelinde seçin **Jostle**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/jostle-tutorial/tutorial_jostle_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." adlı bir test kullanıcı tabanlı Jostle ile test etme

Tek iş için oturum açma için Azure AD ne Jostle karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Jostle ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Jostle içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Jostle ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Jostle test kullanıcısı oluşturma](#creating-a-jostle-test-user)**  - kullanıcı Azure AD gösterimini bağlı Jostle Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Jostle uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Jostle yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Jostle** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açmayı yapılandırın](./media/jostle-tutorial/tutorial_jostle_samlbase.png)

1. Üzerinde **Jostle etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![url_01](./media/jostle-tutorial/url_01.png)

    a. İçinde **oturum açma URL'si** metin girin: `https://login-prod.jostle.us`

    b. İçinde **tanımlayıcı** metin girin: `https://jostle.us`

    c. Yanındaki kutuyu işaretleyin **Gelişmiş URL ayarlarını göster**

    d. İçinde **yanıt URL'si** metin girin: `https://login-prod.jostle.us/saml/SSO/alias/newjostle.us`

1. Üzerinde **kullanıcı öznitelikleri** bölüm için **kullanıcı tanımlayıcısı** girin: `user.userprincipalname`

    ![url_02](./media/jostle-tutorial/url_02.png)

1. Tıklayın **Kaydet** pencerenin üst kısmındaki.

1. Git **SAML imzalama sertifikası** ve ayarlanmış olduğunu doğrulayın **etkin**. Ardından **meta veri XML** meta veri dosyası indirilemedi.

    ![url_03](./media/jostle-tutorial/url_03.png)

1. Çoklu oturum açma Jostle'nın tarafında yapılandırmak için indirilen meta verileri XML göndermek gereken şekilde [Jostle Destek ekibine](mailto:support@jostle.me). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
>

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/jostle-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/jostle-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/jostle-tutorial/create_aaduser_03.png)

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Bir Azure AD test kullanıcısı oluşturma](./media/jostle-tutorial/create_aaduser_04.png)

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.

### <a name="creating-a-jostle-test-user"></a>Jostle test kullanıcısı oluşturma

Bu bölümde, Britta Simon Jostle içinde adlı bir kullanıcı oluşturun. Lütfen kişiyle içinde Jostle Britta Simon ekleme bilmiyorsanız, [Jostle Destek ekibine](mailto:support@jostle.me) test kullanıcı eklemek ve SSO'yu etkinleştirmek için.

> [!NOTE]
> Azure Active Directory hesap sahibinin e-posta alır ve etkin hale gelir önce hesabını onaylamak için bir bağlantı izler.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için Jostle erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200]

**Britta Simon Jostle için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201]

1. Uygulamalar listesinde **Jostle**.

    ![Çoklu oturum açmayı yapılandırın](./media/jostle-tutorial/tutorial_jostle_app.png)

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.

### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Jostle kutucuğa tıkladığınızda, otomatik olarak Jostle uygulamanın oturum açma sayfası almanız gerekir.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/jostle-tutorial/tutorial_general_01.png
[2]: ./media/jostle-tutorial/tutorial_general_02.png
[3]: ./media/jostle-tutorial/tutorial_general_03.png
[4]: ./media/jostle-tutorial/tutorial_general_04.png

[100]: ./media/jostle-tutorial/tutorial_general_100.png

[200]: ./media/jostle-tutorial/tutorial_general_200.png
[201]: ./media/jostle-tutorial/tutorial_general_201.png
[202]: ./media/jostle-tutorial/tutorial_general_202.png
[203]: ./media/jostle-tutorial/tutorial_general_203.png
