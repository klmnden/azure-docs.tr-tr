---
title: 'Öğretici: Azure Active Directory Tümleştirmesi Fluxx laboratuvarlarla | Microsoft Docs'
description: Azure Active Directory ve Fluxx Labs arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: d8fac770-bb57-4e1f-b50b-9ffeae239d07
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3c48e41318ff5ba189e4cc8b8529bb3b81911052
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56204398"
---
# <a name="tutorial-azure-active-directory-integration-with-fluxx-labs"></a>Öğretici: Fluxx laboratuvarlar ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Fluxx Laboratuvarlarla tümleştirme konusunda bilgi edinin.

Fluxx Labs, Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Fluxx Labs erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan (çoklu oturum açma) Fluxx Labs kullanarak açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Fluxx Labs ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik Fluxx Labs çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Fluxx Labs ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-fluxx-labs-from-the-gallery"></a>Galeriden Fluxx Labs ekleme
Azure AD'de Fluxx Labs tümleştirmesini yapılandırmak için Fluxx Labs Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Fluxx Labs eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]

1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **Fluxx Labs**seçin **Fluxx Labs** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Fluxx Laboratuvarları](./media/fluxxlabs-tutorial/tutorial_fluxxlabs_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve "Britta Simon" adlı bir test kullanıcı tabanlı Fluxx laboratuvarlarla Azure AD çoklu oturum açma testi.

Tek çalışmak için oturum açma için Azure AD ne Fluxx Labs karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve ilgili kullanıcı Fluxx Labs arasında bir bağlantı ilişki kurulması gerekir.

Değerini Fluxx Labs'de atama **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Fluxx Labs ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Fluxx Labs test kullanıcısı oluşturma](#create-a-fluxx-labs-test-user)**  - kullanıcı Azure AD gösterimini bağlı Fluxx Labs Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Fluxx Labs uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma Fluxx Labs ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Fluxx Labs** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma iletişim kutusu](./media/fluxxlabs-tutorial/tutorial_fluxxlabs_samlbase.png)

1. Üzerinde **Fluxx Labs etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Fluxx Labs etki alanı ve URL'ler tek oturum açma bilgileri](./media/fluxxlabs-tutorial/tutorial_fluxxlabs_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak:

    | Ortam | URL deseni|
    |-------------|------------|
    | Üretim | `https://<subdomain>.fluxx.io` |
    | Üretim öncesi | `https://<subdomain>.preprod.fluxxlabs.com`|
        
    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak:

    | Ortam | URL deseni|
    |-------------|------------|
    | Üretim | `https://<subdomain>.fluxx.io/auth/saml/callback` |
    | Üretim öncesi | `https://<subdomain>.preprod.fluxxlabs.com/auth/saml/callback`|

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı ve yanıt URL'si ile güncelleştirin. İlgili kişi [Fluxx Labs Destek ekibine](mailto:travis@fluxxlabs.com) bu değerleri almak için.

1. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/fluxxlabs-tutorial/tutorial_fluxxlabs_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/fluxxlabs-tutorial/tutorial_general_400.png)

1. Üzerinde **Fluxx Labs yapılandırma** bölümünde **yapılandırma Fluxx Labs** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Fluxx Laboratuvar yapılandırma](./media/fluxxlabs-tutorial/tutorial_fluxxlabs_configure.png)

1. Farklı bir web tarayıcı penceresinde Fluxx Labs şirketinizin sitesi için yönetici olarak oturum açma.

1. Seçin **yönetici** aşağıda **ayarları** bölümü.

    ![Fluxx Laboratuvar yapılandırma](./media/fluxxlabs-tutorial/config1.png)

1. Yönetim panelinde seçin **eklentileri** > **tümleştirmeler** seçip **SAML SSO-(Disabled)**

    ![Fluxx Laboratuvar yapılandırma](./media/fluxxlabs-tutorial/config2.png)

1. Öznitelik bölümünde aşağıdaki adımları gerçekleştirin:

    ![Fluxx Laboratuvar yapılandırma](./media/fluxxlabs-tutorial/config3.png)

    a. Seçin **SAML SSO** onay kutusu.

    b. İçinde **istek yolu** metin kutusuna **/auth/saml**.

    c. İçinde **geri arama yolu** metin kutusuna **/auth/saml/callback**.

    d. İçinde **onaylama tüketici hizmeti Url(Single Sign-On URL)** metin girin **yanıt URL'si** Azure portalında girdiğiniz değer.

    e. İçinde **İzleyici (SP varlık kimliği)** metin girin **tanımlayıcı** Azure portalında girdiğiniz değer.

    f. İçinde **kimlik sağlayıcısı SSO hedef URL'si** metin kutusu, yapıştırma **SAML çoklu oturum açma hizmeti URL'si** Azure portaldan kopyaladığınız değeri.

    g. Base-64 kodlanmış sertifikanızı Not Defteri'nde açın, içeriğini, panoya kopyalayın ve ardından ona yapıştırın **kimlik sağlayıcısı sertifikası** metin.

    h. İçinde **ad tanımlayıcı biçimi** metin değeri girin `urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress`.

    i. **Kaydet**’e tıklayın.

    > [!NOTE]
    > Kaydedilmiş bir içerik alanı bir kez güvenlik için boş görünmez, ancak değeri yapılandırmada kaydedildi.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/fluxxlabs-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/fluxxlabs-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/fluxxlabs-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/fluxxlabs-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
  
### <a name="create-a-fluxx-labs-test-user"></a>Fluxx Labs test kullanıcısı oluşturma

Fluxx Labs kullanarak oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunlar Fluxx Labs sağlanması gerekir. Fluxx Labs söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Fluxx Labs şirketinizin sitesi için bir yönetici olarak oturum açın.

1. Tıklayarak aşağıda görüntülenen **simgesi**.

    ![Fluxx Laboratuvar yapılandırma](./media/fluxxlabs-tutorial/config6.png)

1. Panoda, tıklayarak açmak için görüntülenen simgesinin altında **yeni kişiler** kart.

    ![Fluxx Laboratuvar yapılandırma](./media/fluxxlabs-tutorial/config4.png)

1. Üzerinde **yeni kişiler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Fluxx Laboratuvar yapılandırma](./media/fluxxlabs-tutorial/config5.png)

    a. Fluxx Laboratuarları e-posta SSO oturumu için benzersiz tanımlayıcı olarak kullanın. Doldurma **SSO UID** alan SSO ile oturum açma olarak kullanarak e-posta adresiyle eşleşen, kullanıcının e-posta adresi.

    b. **Kaydet**’e tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Fluxx Labs kullanarak erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200]

**Britta Simon Fluxx Labs kullanarak atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Fluxx Labs**.

    ![Uygulamalar listesinde Fluxx Labs bağlantı](./media/fluxxlabs-tutorial/tutorial_fluxxlabs_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Fluxx Labs kutucuğa tıkladığınızda, otomatik olarak Fluxx Labs uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/fluxxlabs-tutorial/tutorial_general_01.png
[2]: ./media/fluxxlabs-tutorial/tutorial_general_02.png
[3]: ./media/fluxxlabs-tutorial/tutorial_general_03.png
[4]: ./media/fluxxlabs-tutorial/tutorial_general_04.png

[100]: ./media/fluxxlabs-tutorial/tutorial_general_100.png

[200]: ./media/fluxxlabs-tutorial/tutorial_general_200.png
[201]: ./media/fluxxlabs-tutorial/tutorial_general_201.png
[202]: ./media/fluxxlabs-tutorial/tutorial_general_202.png
[203]: ./media/fluxxlabs-tutorial/tutorial_general_203.png
