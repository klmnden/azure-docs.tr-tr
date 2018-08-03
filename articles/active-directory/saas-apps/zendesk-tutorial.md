---
title: 'Öğretici: Azure Active Directory Zendesk ile tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Zendesk arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 9d7c91e5-78f5-4016-862f-0f3242b00680
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2018
ms.author: jeedes
ms.openlocfilehash: 9b467fa966c2a785677f47faaa4bb8bd3ed238e2
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39427610"
---
# <a name="tutorial-azure-active-directory-integration-with-zendesk"></a>Öğretici: Azure Active Directory Zendesk ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Zendesk tümleştirme konusunda bilgi edinin.

Zendesk Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Zendesk erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan için bir Zendesk (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Zendesk ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Bir Zendesk çoklu oturum açma abonelik etkin.

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Zendesk galeri ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-zendesk-from-the-gallery"></a>Zendesk galeri ekleme
Azure AD'de Zendesk tümleştirmesini yapılandırmak için Zendesk Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Zendesk eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **Zendesk**seçin **Zendesk** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Zendesk](./media/zendesk-tutorial/tutorial_zendesk_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Zendesk ile test edin.

Tek çalışmak için oturum açma için Azure AD ne Zendesk karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve Zendesk ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Zendesk'te, değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Zendesk ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Zendesk test kullanıcısı oluşturma](#create-a-zendesk-test-user)**  - kullanıcı Azure AD gösterimini bağlı Zendesk Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Zendesk uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma Zendesk ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Zendesk** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/zendesk-tutorial/tutorial_zendesk_samlbase.png)

1. Üzerinde **Zendesk etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Zendesk etki alanı ve URL'ler tek oturum açma bilgileri](./media/zendesk-tutorial/tutorial_zendesk_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<subdomain>.zendesk.com`

    b. İçinde **tanımlayıcı** metin kutusuna şu biçimi kullanarak değeri yazın: `<subdomain>.zendesk.com`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [Zendesk istemci Destek ekibine](https://support.zendesk.com/hc/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise) bu değerleri almak için.

1. Üzerinde **SAML imzalama sertifikası** bölümünde, kopya **parmak İZİ** sertifika değeri.

    ![Sertifika indirme bağlantısı](./media/zendesk-tutorial/tutorial_zendesk_certificate.png)

1. Zendesk, belirli bir biçimde SAML onaylamalarını bekliyor. Zorunlu SAML öznitelikleri vardır, ancak isteğe bağlı olarak bir öznitelik alma ekleyebilirsiniz **kullanıcı öznitelikleri** izleyerek bölümünde aşağıdaki adımları: 

     ![Çoklu oturum açmayı yapılandırın](./media/zendesk-tutorial/tutorial_zendesk_attributes1.png)

    a. Tıklayın **eklemek agentconfigutil** açmak için **öznitelik Ekle** iletişim.

    ![Çoklu oturum açmayı yapılandırma Ekle](./media/zendesk-tutorial/tutorial_attribute_04.png)

    ![Çoklu oturum açma addattb yapılandırın](./media/zendesk-tutorial/tutorial_attribute_05.png)

    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.

    c. Gelen **değer** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.
    
    d. **Tamam**’a tıklayın.

    > [!NOTE]
    > Uzantı öznitelikleri, varsayılan olarak Azure AD'de olmayan öznitelikler eklemek için kullanın. Tıklayın [SAML ayarlanabilir kullanıcı öznitelikleri](https://support.zendesk.com/hc/en-us/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise-) tam listesini almak için SAML öznitelikleri **Zendesk** kabul eder.

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/zendesk-tutorial/tutorial_general_400.png)

1. Üzerinde **Zendesk yapılandırma** bölümünde **yapılandırma Zendesk** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Zendesk yapılandırma](./media/zendesk-tutorial/tutorial_zendesk_configure.png) 

1. Farklı bir web tarayıcı penceresinde bir Zendesk şirket sitenize yönetici olarak oturum.

1. Tıklayın **yönetici**.

1. Sol gezinti bölmesinden **ayarları**ve ardından **güvenlik**.

1. Üzerinde **güvenlik** sayfasında, aşağıdaki adımları gerçekleştirin: 

     ![Güvenlik](./media/zendesk-tutorial/ic773089.png "güvenlik")

    ![Çoklu oturum açma](./media/zendesk-tutorial/ic773090.png "çoklu oturum açma")

     a. Tıklayın **yönetici ve aracılar** sekmesi.

     b. Seçin **çoklu oturum açma (SSO) ve SAML**ve ardından **SAML**.

     c. İçinde **SAML SSO URL** metin değerini yapıştırın **SAML çoklu oturum açma hizmeti URL'si** , Azure Portalı'ndan kopyaladığınız. 

     d. İçinde **uzak oturum kapatma URL'si** metin değerini yapıştırın **oturum kapatma URL'si** , Azure Portalı'ndan kopyaladığınız.

     e. İçinde **sertifika parmak izi** metin kutusu, yapıştırma **parmak izi** Azure Portalı'ndan kopyaladığınız sertifika değeri.

     f. **Kaydet**’e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/zendesk-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/zendesk-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/zendesk-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/zendesk-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.

### <a name="create-a-zendesk-test-user"></a>Zendesk test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon Zendesk'te adlı bir kullanıcı oluşturmaktır. Zendesk otomatik kullanıcı hazırlama, varsayılan olarak etkin olan destekler. Daha fazla ayrıntı bulabilirsiniz [burada](zendesk-provisioning-tutorial.md) otomatik kullanıcı sağlamayı yapılandırma.

**Kullanıcı el ile oluşturmanız gerekiyorsa, lütfen aşağıdaki adımları uygulayın:**

> [!NOTE]
> **Son kullanıcı** hesapları otomatik olarak oturum açarken sağlandı. **Aracı** ve **yönetici** hesapları içinde el ile sağlanması gerekir **Zendesk** oturum açmadan önce.

1. Oturum açın, **Zendesk** Kiracı.

1. Seçin **müşteri listesi** sekmesi.

1. Seçin **kullanıcı** sekmesine ve tıklayın **Ekle**.

    ![Kullanıcı Ekle](./media/zendesk-tutorial/ic773632.png "Kullanıcı Ekle")
1. Tür **adı** ve **e-posta** sağlamak istediğiniz ve ardından var olan bir Azure AD hesabının **Kaydet**.

    ![Yeni kullanıcı](./media/zendesk-tutorial/ic773633.png "yeni kullanıcı")

> [!NOTE]
> Herhangi diğer Zendesk kullanıcı hesabı oluşturma araçları kullanabilir veya API'leri için AAD kullanıcı hesapları sağlamak Zendesk tarafından sağlanan.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Zendesk erişim vererek Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200]

**Britta Simon Zendesk'e atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201]

1. Uygulamalar listesinde **Zendesk**.

    ![Uygulamalar listesini Zendesk bağlantıdaki](./media/zendesk-tutorial/tutorial_zendesk_app.png)

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Zendesk kutucuğa tıkladığınızda, otomatik olarak Zendesk uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
* [Kullanıcı sağlamayı yapılandırma](zendesk-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/zendesk-tutorial/tutorial_general_01.png
[2]: ./media/zendesk-tutorial/tutorial_general_02.png
[3]: ./media/zendesk-tutorial/tutorial_general_03.png
[4]: ./media/zendesk-tutorial/tutorial_general_04.png

[100]: ./media/zendesk-tutorial/tutorial_general_100.png

[200]: ./media/zendesk-tutorial/tutorial_general_200.png
[201]: ./media/zendesk-tutorial/tutorial_general_201.png
[202]: ./media/zendesk-tutorial/tutorial_general_202.png
[203]: ./media/zendesk-tutorial/tutorial_general_203.png
