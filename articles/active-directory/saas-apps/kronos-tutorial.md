---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle Kronos | Microsoft Docs'
description: Azure Active Directory ve Kronos arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: e28d6191-c375-43c6-b2df-22daa88d9939
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 31e8c990e39b3dc99ccd4dcda0a8d00ecb83b440
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39435892"
---
# <a name="tutorial-azure-active-directory-integration-with-kronos"></a>Öğretici: Azure Active Directory Kronos ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Kronos tümleştirme konusunda bilgi edinin.

Azure AD ile Kronos tümleştirme ile aşağıdaki avantajları sağlar:

- Kronos erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için Kronos (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Kronos yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- A **Kronos iş gücü Merkezi** SSO aboneliği etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Kronos ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-kronos-from-the-gallery"></a>Galeriden Kronos ekleme
Azure AD'de Kronos tümleştirmesini yapılandırmak için Kronos Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Kronos eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **Kronos**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/kronos-tutorial/tutorial_kronos_search.png)

1. Sonuçlar panelinde seçin **Kronos**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/kronos-tutorial/tutorial_kronos_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." adlı bir test kullanıcı tabanlı Kronos ile test etme

Tek iş için oturum açma için Azure AD ne Kronos karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Kronos ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Kronos içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Kronos ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Kronos test kullanıcısı oluşturma](#creating-a-kronos-test-user)**  - kullanıcı Azure AD gösterimini bağlı Kronos Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Kronos uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Kronos yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Kronos** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/kronos-tutorial/tutorial_kronos_samlbase.png)

1. Üzerinde **Kronos etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/kronos-tutorial/tutorial_kronos_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<company name>.kronos.net/`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<company name>.kronos.net/wfc/navigator/logonWithUID`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı ve yanıt URL'si ile güncelleştirin. İlgili kişi [Kronos Destek ekibine](https://www.kronos.in/contact/en-in/form) bu değerleri almak için.
 
1. Kronos uygulamanızı belirli bir biçimde SAML onaylamalarını bekliyor. Çalışmak [Kronos Destek ekibine](https://www.kronos.in/contact/en-in/form) ilk uygulamasına eşlenen doğru kullanıcı tanımlayıcısını tanımlamak için. Ayrıca bu eşleme için kullanmak istedikleri özniteliği, ilgili yönergeleri Lütfen ayırın.
 
     Microsoft, kullanılmasını önerir **"NameIdentifier"** kullanıcı tanımlayıcısı olarak özniteliği. Bu öznitelikleri değerlerini yönetebilirsiniz **"Kullanıcı öznitelikleri"** uygulama tümleştirme sayfasında bölümü.
     
     Aşağıdaki ekran görüntüsü bunun bir örneği gösterilmektedir. Biz burada eşlediğiniz **kullanıcı tanımlayıcısı (nameıd)** ile **ExtractMailPrefix()** işlevi **user.userprincipalname**. Bu ön eki değeri benzersiz kullanıcı kimliği olan kullanıcının e-postanın sağlar Bu, her başarılı yanıt Kronos uygulamaya gönderilir. 
     
    ![Çoklu oturum açmayı yapılandırın](./media/kronos-tutorial/tutorial_kronos_attribute.png)

1. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim:

    a. Kullanıcı tanımlayıcısı açılan listesinde seçin **ExtractMailPrefix**.

    b. İçinde **posta** açılan listesinden **user.userprincipalname**.

1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/kronos-tutorial/tutorial_kronos_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/kronos-tutorial/tutorial_general_400.png)

1. Çoklu oturum açmayı yapılandırma **Kronos** tarafı, indirilen göndermek için ihtiyacınız **meta veri XML** için [Kronos Destek ekibine](https://www.kronos.in/contact/en-in/form). 

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/kronos-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/kronos-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/kronos-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/kronos-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-kronos-test-user"></a>Kronos test kullanıcısı oluşturma

Bu bölümde, Britta Simon Kronos içinde adlı bir kullanıcı oluşturun. Kronos uygulamanın tüm kullanıcılar uygulamada SSO yapmadan önce sağlanması gerekir. 

Çalışmak [Kronos Destek ekibine](https://www.kronos.in/contact/en-in/form) bu kullanıcılar uygulamaya sağlamak için. 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için Kronos erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Kronos için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Kronos**.

    ![Çoklu oturum açmayı yapılandırın](./media/kronos-tutorial/tutorial_kronos_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Azure AD SSO yapılandırmanızı erişim panelini kullanarak test edin.

Erişim panelinde Kronos kutucuğa tıkladığınızda, otomatik olarak Kronos uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/kronos-tutorial/tutorial_general_01.png
[2]: ./media/kronos-tutorial/tutorial_general_02.png
[3]: ./media/kronos-tutorial/tutorial_general_03.png
[4]: ./media/kronos-tutorial/tutorial_general_04.png

[100]: ./media/kronos-tutorial/tutorial_general_100.png

[200]: ./media/kronos-tutorial/tutorial_general_200.png
[201]: ./media/kronos-tutorial/tutorial_general_201.png
[202]: ./media/kronos-tutorial/tutorial_general_202.png
[203]: ./media/kronos-tutorial/tutorial_general_203.png

