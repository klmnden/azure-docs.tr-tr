---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile LiquidFiles | Microsoft Docs'
description: Azure Active Directory ve LiquidFiles arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: cb517134-0b34-4a74-b40c-5a3223ca81b6
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 20a3144f2a8727420803034426106a29a7924727
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60258570"
---
# <a name="tutorial-azure-active-directory-integration-with-liquidfiles"></a>Öğretici: LiquidFiles ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile LiquidFiles tümleştirme konusunda bilgi edinin.

Azure AD ile LiquidFiles tümleştirme ile aşağıdaki avantajları sağlar:

- LiquidFiles erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için LiquidFiles (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile LiquidFiles yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik LiquidFiles çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden LiquidFiles ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-liquidfiles-from-the-gallery"></a>Galeriden LiquidFiles ekleme
Azure AD'de LiquidFiles tümleştirmesini yapılandırmak için LiquidFiles Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden LiquidFiles eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **LiquidFiles**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/liquidfiles-tutorial/tutorial_liquidfiles_search.png)

1. Sonuçlar panelinde seçin **LiquidFiles**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/liquidfiles-tutorial/tutorial_liquidfiles_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı LiquidFiles sınayın.

Tek iş için oturum açma için Azure AD ne LiquidFiles karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının LiquidFiles ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

LiquidFiles içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma LiquidFiles ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[LiquidFiles test kullanıcısı oluşturma](#creating-a-liquidfiles-test-user)**  - kullanıcı Azure AD gösterimini bağlı LiquidFiles Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve LiquidFiles uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile LiquidFiles yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **LiquidFiles** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/liquidfiles-tutorial/tutorial_liquidfiles_samlbase.png)

1. Üzerinde **LiquidFiles etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/liquidfiles-tutorial/tutorial_liquidfiles_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<YOUR_SERVER_URL>/saml/init`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<YOUR_SERVER_URL>`

    c. b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<YOUR_SERVER_URL>/saml/consume`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Gerçek oturum açma URL'si ile bu değerleri güncelleştirmek tanımlayıcısı ve yanıt URL'si. İlgili kişi [LiquidFiles istemci Destek ekibine](https://www.liquidfiles.com/support.html) bu değerleri almak için. 
 
1. Üzerinde **SAML imzalama sertifikası** bölümünde, kopya **parmak İZİ** sertifika değeri.

    ![Çoklu oturum açmayı yapılandırın](./media/liquidfiles-tutorial/tutorial_liquidfiles_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/liquidfiles-tutorial/tutorial_general_400.png)

1. Üzerinde **LiquidFiles yapılandırma** bölümünde **yapılandırma LiquidFiles** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/liquidfiles-tutorial/tutorial_liquidfiles_configure.png)
 
1. LiquidFiles şirketinizin sitesi için yönetici olarak oturum.

1. Tıklayın **çoklu oturum açma** içinde **yönetici > yapılandırma** menüsünde.

1. Üzerinde **çoklu oturum açma yapılandırması** sayfasında, aşağıdaki adımları gerçekleştirin

    ![Çoklu oturum açmayı yapılandırın](./media/liquidfiles-tutorial/tutorial_single_01.png)

    a. Olarak **tek oturum açma yöntemini**seçin **SAML 2**.

    b. İçinde **IDP'nin oturum açma URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmeti URL'si**, hangi Azure Portalı'ndan kopyaladığınız.

    c. İçinde **IDP oturum kapatma URL'si** metin değerini yapıştırın **oturum kapatma URL'si**, hangi Azure Portalı'ndan kopyaladığınız.

    d. İçinde **IDP sertifika parmak izi** metin kutusu, yapıştırma **parmak İZİ** Azure Portalı'ndan kopyaladığınız değeri...

    e. Ad tanımlayıcı biçimi metin kutusunda, değeri yazın **urn: OASIS: adları: tc: SAML:1.1:nameid-biçimi: emailAddress**.

    f. Authn bağlam metin kutusunda, değeri yazın **urn: OASIS: adları: tc: SAML:2.0:ac:classes:PasswordProtectedTransport**.

    g. **Kaydet**’e tıklayın.  

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/liquidfiles-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/liquidfiles-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/liquidfiles-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/liquidfiles-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-liquidfiles-test-user"></a>LiquidFiles test kullanıcısı oluşturma

Bu bölümün amacı LiquidFiles Britta Simon adlı bir kullanıcı oluşturmaktır. Kendiniz LiquidFiles uygulamanıza oturum açma önce bir kullanıcı olarak eklenmiş almak için LiquidFiles server yöneticinizle birlikte çalışın.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için LiquidFiles erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon LiquidFiles için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **LiquidFiles**.

    ![Çoklu oturum açmayı yapılandırın](./media/liquidfiles-tutorial/tutorial_liquidfiles_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümün amacı, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test sağlamaktır.

Erişim panelinde LiquidFiles kutucuğa tıkladığınızda, otomatik olarak LiquidFiles uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/liquidfiles-tutorial/tutorial_general_01.png
[2]: ./media/liquidfiles-tutorial/tutorial_general_02.png
[3]: ./media/liquidfiles-tutorial/tutorial_general_03.png
[4]: ./media/liquidfiles-tutorial/tutorial_general_04.png

[100]: ./media/liquidfiles-tutorial/tutorial_general_100.png

[200]: ./media/liquidfiles-tutorial/tutorial_general_200.png
[201]: ./media/liquidfiles-tutorial/tutorial_general_201.png
[202]: ./media/liquidfiles-tutorial/tutorial_general_202.png
[203]: ./media/liquidfiles-tutorial/tutorial_general_203.png

