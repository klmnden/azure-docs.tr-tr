---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle GaggleAMP | Microsoft Docs'
description: Azure Active Directory ve GaggleAMP arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 9cc1a4b7-964b-406b-9e0c-05cb1a7c9856
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/06/2018
ms.author: jeedes
ms.openlocfilehash: 828dd1e1dcef900a7105143088f6782032b4f22e
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39436521"
---
# <a name="tutorial-azure-active-directory-integration-with-gaggleamp"></a>Öğretici: Azure Active Directory GaggleAMP ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile GaggleAMP tümleştirme konusunda bilgi edinin.

Azure AD ile GaggleAMP tümleştirme ile aşağıdaki avantajları sağlar:

- GaggleAMP erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için GaggleAMP (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile GaggleAMP yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik GaggleAMP çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden GaggleAMP ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-gaggleamp-from-the-gallery"></a>Galeriden GaggleAMP ekleme
Azure AD'de GaggleAMP tümleştirmesini yapılandırmak için GaggleAMP Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden GaggleAMP eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **GaggleAMP**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/gaggleamp-tutorial/tutorial_gaggleamp_search.png)

1. Sonuçlar panelinde seçin **GaggleAMP**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/gaggleamp-tutorial/tutorial_gaggleamp_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı GaggleAMP sınayın.

Tek iş için oturum açma için Azure AD ne GaggleAMP karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının GaggleAMP ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

GaggleAMP içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma GaggleAMP ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[GaggleAMP test kullanıcısı oluşturma](#creating-a-gaggleamp-test-user)**  - kullanıcı Azure AD gösterimini bağlı GaggleAMP Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve GaggleAMP uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile GaggleAMP yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **GaggleAMP** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/gaggleamp-tutorial/tutorial_gaggleamp_samlbase.png)

1. Üzerinde **GaggleAMP etki alanı ve URL'ler** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** başlatılan modu:

    ![Çoklu oturum açmayı yapılandırın](./media/gaggleamp-tutorial/tutorial_gaggleamp_url.png)

     İçinde **tanımlayıcı** metin kutusuna URL'yi yazın: `https://accounts.gaggleamp.com/auth/saml/callback`

1. Denetleme **Gelişmiş URL ayarlarını göster** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![Çoklu oturum açmayı yapılandırın](./media/gaggleamp-tutorial/tutorial_gaggleamp_url1.png)

     İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://gaggleamp.com/i/<customerid>`

    > [!NOTE]
    > Oturum açma URL değeri, gerçek değil. Bu değer, gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [GaggleAMP istemci Destek ekibine](mailto:sales@gaggleamp.com) bu değeri alınamıyor.
 
1. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/gaggleamp-tutorial/tutorial_gaggleamp_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/gaggleamp-tutorial/tutorial_general_400.png)

1. Üzerinde **GaggleAMP yapılandırma** bölümünde **yapılandırma GaggleAMP** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/gaggleamp-tutorial/tutorial_gaggleamp_configure.png) 

1. Başka bir tarayıcı örneğinde sizin tarafınızdan Gaggle takım desteklemek için oluşturulan SAML SSO sayfasına gidin (örneğin: *https://accounts.gaggleamp.com/saml_configurations/oXH8sQcP79dOzgFPqrMTyw/edit*).

1. Üzerinde **SAML SSO** sayfasında, aşağıdaki adımları gerçekleştirin:  
   
    ![GaggleAMP çoklu oturum açma](./media/gaggleamp-tutorial/tutorial_gaggleamp_06.png)

    a. Seçin **diğer** form **kimlik sağlayıcısı** açılan menüsü.
    
    b. İçinde **kimlik sağlayıcısını veren** metin değerini yapıştırın **veren URL'si** , Azure Portalı'ndan kopyaladığınız.
    
    c. İçinde **kimlik sağlayıcısının çoklu oturum açma URL'si** metin değerini yapıştırın **çoklu oturum açma hizmeti URL'si** , Azure Portalı'ndan kopyaladığınız.
    
    d. İndirilen açın **Certificate(Base64)** dosyasını Not Defteri'nde, içeriğini, panoya kopyalayın ve ardından ona yapıştırın **X.509 sertifikası** metin.
    
    e. **Kaydet**’e tıklayın.

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/gaggleamp-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/gaggleamp-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/gaggleamp-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/gaggleamp-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-gaggleamp-test-user"></a>GaggleAMP test kullanıcısı oluşturma

Bu bölümün amacı GaggleAMP Britta Simon adlı bir kullanıcı oluşturmaktır. GaggleAMP tam zamanında sağlama, varsayılan olarak etkin olan destekler.

Bu bölümde, hiçbir eylem öğesini yoktur. Yeni bir kullanıcı, henüz yoksa GaggleAMP erişme denemesi sırasında oluşturulur. 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için GaggleAMP erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon GaggleAMP için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **GaggleAMP**.

    ![Çoklu oturum açmayı yapılandırın](./media/gaggleamp-tutorial/tutorial_gaggleamp_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümün amacı, erişim panelini kullanarak Azure AD SSO yapılandırmanızı sınamanızı sağlamaktır.

Erişim panelinde GaggleAMP kutucuğa tıkladığınızda, otomatik olarak GaggleAMP uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/gaggleamp-tutorial/tutorial_general_01.png
[2]: ./media/gaggleamp-tutorial/tutorial_general_02.png
[3]: ./media/gaggleamp-tutorial/tutorial_general_03.png
[4]: ./media/gaggleamp-tutorial/tutorial_general_04.png

[100]: ./media/gaggleamp-tutorial/tutorial_general_100.png

[200]: ./media/gaggleamp-tutorial/tutorial_general_200.png
[201]: ./media/gaggleamp-tutorial/tutorial_general_201.png
[202]: ./media/gaggleamp-tutorial/tutorial_general_202.png
[203]: ./media/gaggleamp-tutorial/tutorial_general_203.png
