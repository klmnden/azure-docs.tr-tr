---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle StatusPage | Microsoft Docs'
description: Azure Active Directory ve StatusPage arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: f6ee8bb3-df43-4c0d-bf84-89f18deac4b9
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jeedes
ms.openlocfilehash: e79eb2473760fd1eb7ccc3816ac73cce7c801f3e
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39445373"
---
# <a name="tutorial-azure-active-directory-integration-with-statuspage"></a>Öğretici: Azure Active Directory StatusPage ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile StatusPage tümleştirme konusunda bilgi edinin.

Azure AD ile StatusPage tümleştirme ile aşağıdaki avantajları sağlar:

- StatusPage erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için StatusPage (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile StatusPage yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik StatusPage çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden StatusPage ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-statuspage-from-the-gallery"></a>Galeriden StatusPage ekleme
Azure AD'de StatusPage tümleştirmesini yapılandırmak için StatusPage Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden StatusPage eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **StatusPage**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/statuspage-tutorial/tutorial_statuspage_search.png)

1. Sonuçlar panelinde seçin **StatusPage**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/statuspage-tutorial/tutorial_statuspage_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı StatusPage sınayın.

Tek iş için oturum açma için Azure AD ne StatusPage karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının StatusPage ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

StatusPage içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma StatusPage ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[StatusPage test kullanıcısı oluşturma](#creating-a-statuspage-test-user)**  - kullanıcı Azure AD gösterimini bağlı StatusPage Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve StatusPage uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile StatusPage yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **StatusPage** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/statuspage-tutorial/tutorial_statuspage_samlbase.png)

1. Üzerinde **StatusPage etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/statuspage-tutorial/tutorial_statuspage_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak:
    | |
    |--|
    | `https://<subdomain>.statuspagestaging.com/` |
    | `https://<subdomain>.statuspage.io/` |

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: 
    | |
    |--|
    | `https://<subdomain>.statuspagestaging.com/sso/saml/consume` |
    | `https://<subdomain>.statuspage.io/sso/saml/consume` |

    > [!NOTE]
    > Adresinden StatusPage Destek ekibine başvurun [ SupportTeam@statuspage.io ](mailto:SupportTeam@statuspage.io)çoklu oturum açma için yapılandırmak için gerekli meta verileri istemek için. 
    >
    >a. Meta verileri, veren değerini kopyalayın ve ardından yapıştırın **tanımlayıcı** metin.
    >
    >b. Meta verilerden yanıt URL'sini kopyalayıp içine yapıştırın **yanıt URL'si** metin.

1. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/statuspage-tutorial/tutorial_statuspage_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/statuspage-tutorial/tutorial_general_400.png)

1. Üzerinde **StatusPage yapılandırma** bölümünde **yapılandırma StatusPage** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/statuspage-tutorial/tutorial_statuspage_configure.png) 

1. Başka bir tarayıcı penceresinde StatusPage şirketinizin sitesi için bir yönetici olarak oturum açın.

1. Ana araç çubuğunda tıklatın **hesabı Yönet**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/statuspage-tutorial/tutorial_statuspage_06.png) 

1. Tıklayın **çoklu oturum açma** sekmesi. 
   
    ![Çoklu oturum açmayı yapılandırın](./media/statuspage-tutorial/tutorial_statuspage_07.png) 

1. SSO Kurulumu sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açmayı yapılandırın](./media/statuspage-tutorial/tutorial_statuspage_08.png) 

    ![Çoklu oturum açmayı yapılandırın](./media/statuspage-tutorial/tutorial_statuspage_09.png) 
 
    a. İçinde **SSO hedef URL** metin değerini yapıştırın **SAML çoklu oturum açma hizmeti URL'si**, hangi Azure Portalı'ndan kopyaladığınız.

    b. İndirilen sertifikanızı Not Defteri'nde açın, içeriği kopyalayın ve ardından yapıştırın **sertifika** metin. 

    c. Tıklayın **YAPILANDIRMASINI Kaydet**.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/statuspage-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/statuspage-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/statuspage-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/statuspage-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-statuspage-test-user"></a>StatusPage test kullanıcısı oluşturma

Bu bölümün amacı StatusPage Britta Simon adlı bir kullanıcı oluşturmaktır.

Tam zamanında sağlama StatusPage destekler. İçinde zaten etkinleştirdiyseniz [yapılandırma Azure AD çoklu oturum açma](#configuring-azure-ad-single-sign-on).

**Britta Simon StatusPage içinde adlı bir kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. StatusPage şirketinizin sitesi için bir yönetici olarak oturum.

1. Üstteki menüden **hesabı Yönet**.

    ![Çoklu oturum açmayı yapılandırın](./media/statuspage-tutorial/tutorial_statuspage_06.png)

1. Tıklayın **takım üyeleri** sekmesi. 
   
    ![Bir Azure AD test kullanıcısı oluşturma](./media/statuspage-tutorial/tutorial_statuspage_10.png) 

1. Tıklayın **Ekle TAKIM üyesi**. 
   
    ![Bir Azure AD test kullanıcısı oluşturma](./media/statuspage-tutorial/tutorial_statuspage_11.png) 

1. Tür **e-posta adresi**, **ad**, ve **soyad** istediğiniz ilgili metin kutularına zbilgisayarlar geçerli bir kullanıcı. 
   
    ![Bir Azure AD test kullanıcısı oluşturma](./media/statuspage-tutorial/tutorial_statuspage_12.png) 

1. Olarak **rol**, seçin **istemci yönetici**.

1. Tıklayın **hesap oluştur**.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için StatusPage erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon StatusPage için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **StatusPage**.

    ![Çoklu oturum açmayı yapılandırın](./media/statuspage-tutorial/tutorial_statuspage_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümün amacı, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test sağlamaktır.

Erişim panelinde StatusPage kutucuğa tıkladığınızda, otomatik olarak StatusPage uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/statuspage-tutorial/tutorial_general_01.png
[2]: ./media/statuspage-tutorial/tutorial_general_02.png
[3]: ./media/statuspage-tutorial/tutorial_general_03.png
[4]: ./media/statuspage-tutorial/tutorial_general_04.png

[100]: ./media/statuspage-tutorial/tutorial_general_100.png

[200]: ./media/statuspage-tutorial/tutorial_general_200.png
[201]: ./media/statuspage-tutorial/tutorial_general_201.png
[202]: ./media/statuspage-tutorial/tutorial_general_202.png
[203]: ./media/statuspage-tutorial/tutorial_general_203.png

