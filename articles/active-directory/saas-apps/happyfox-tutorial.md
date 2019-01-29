---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile HappyFox | Microsoft Docs'
description: Azure Active Directory ve HappyFox arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 8204ee77-f64b-4fac-b64a-25ea534feac0
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: jeedes
ms.openlocfilehash: 94bd0367b8626a83fd10fc51d8b606ab38f6182c
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55178078"
---
# <a name="tutorial-azure-active-directory-integration-with-happyfox"></a>Öğretici: HappyFox ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile HappyFox tümleştirme konusunda bilgi edinin.

Azure AD ile HappyFox tümleştirme ile aşağıdaki avantajları sağlar:

- HappyFox erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için HappyFox (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile HappyFox yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir HappyFox çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden HappyFox ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-happyfox-from-the-gallery"></a>Galeriden HappyFox ekleme
Azure AD'de HappyFox tümleştirmesini yapılandırmak için HappyFox Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden HappyFox eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **HappyFox**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/happyfox-tutorial/tutorial_happyfox_search.png)

1. Sonuçlar panelinde seçin **HappyFox**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/happyfox-tutorial/tutorial_happyfox_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." adlı bir test kullanıcı tabanlı HappyFox ile test etme

Tek iş için oturum açma için Azure AD ne HappyFox karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının HappyFox ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

HappyFox içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma HappyFox ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[HappyFox test kullanıcısı oluşturma](#creating-a-happyfox-test-user)**  - kullanıcı Azure AD gösterimini bağlı HappyFox Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve HappyFox uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile HappyFox yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **HappyFox** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/happyfox-tutorial/tutorial_happyfox_samlbase.png)

1. Üzerinde **HappyFox etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/happyfox-tutorial/tutorial_happyfox_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<subdomain>.happyfox.com/`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<subdomain>.happyfox.com/saml/metadata/`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [HappyFox istemci Destek ekibine](https://support.happyfox.com/home) bu değerleri almak için. 
 
1. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/happyfox-tutorial/tutorial_happyfox_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/happyfox-tutorial/tutorial_general_400.png)

1. Üzerinde **HappyFox yapılandırma** bölümünde **yapılandırma HappyFox** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümüne**.

    ![Çoklu oturum açmayı yapılandırın](./media/happyfox-tutorial/tutorial_happyfox_configure.png) 

1. HappyFox personeli portalınızda oturum açın ve gidin **Yönet**, tıklayarak **tümleştirmeler** sekmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/happyfox-tutorial/header.png) 

1. Tümleştirmeler sekmesini tıklatın **yapılandırma** altında **SAML tümleştirme** üzerinde tek oturum ayarlarını açın.

    ![Çoklu oturum açmayı yapılandırın](./media/happyfox-tutorial/configure.png) 

1. SAML yapılandırma bölümü içinde yapıştırın **SAML çoklu oturum açma hizmeti URL'si** Azure portaldan kopyaladığınız **SSO hedef URL** metin.

    ![Çoklu oturum açmayı yapılandırın](./media/happyfox-tutorial/targeturl.png)

1. Not Defteri'nde Azure portalından indirilen sertifikasını açın ve içeriğini yapıştırın **IDP imza** bölümü.
 
    ![Çoklu oturum açmayı yapılandırın](./media/happyfox-tutorial/cert.png)

1. Tıklayın **Ayarları Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/happyfox-tutorial/savesettings.png)

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/happyfox-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/happyfox-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/happyfox-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/happyfox-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-happyfox-test-user"></a>HappyFox test kullanıcısı oluşturma

Uygulama, zaman kullanıcı sağlamayı ve kimlik doğrulama kullanıcıları otomatik olarak uygulama oluşturulduktan sonra sadece destekler.
        
### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için HappyFox erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon HappyFox için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **HappyFox**.

    ![Çoklu oturum açmayı yapılandırın](./media/happyfox-tutorial/tutorial_happyfox_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

1. Erişim paneli HappyFox kutucuğa tıkladığınızda, oturum açma sayfası HappyFox uygulamanın almanız gerekir. Görmelisiniz **'SAML'** oturum açma sayfasında düğme.

    ![Eklentisi](./media/happyfox-tutorial/saml.png) 

1. Tıklayın **'SAML'** düğmesini için HappyFox Azure AD'ye hesabınızı kullanarak oturum açın.

Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/happyfox-tutorial/tutorial_general_01.png
[2]: ./media/happyfox-tutorial/tutorial_general_02.png
[3]: ./media/happyfox-tutorial/tutorial_general_03.png
[4]: ./media/happyfox-tutorial/tutorial_general_04.png

[100]: ./media/happyfox-tutorial/tutorial_general_100.png

[200]: ./media/happyfox-tutorial/tutorial_general_200.png
[201]: ./media/happyfox-tutorial/tutorial_general_201.png
[202]: ./media/happyfox-tutorial/tutorial_general_202.png
[203]: ./media/happyfox-tutorial/tutorial_general_203.png

