---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle yani | Microsoft Docs'
description: Azure Active Directory arasında çoklu oturum açmayı yapılandırma hakkında bilgi edinin ve özelliği.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 9541d5c4-4c82-4b5b-b01a-6a3f75a2b7a1
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: b76dfa7d16da487798281f5f31b9915ca9716ebc
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54808417"
---
# <a name="tutorial-azure-active-directory-integration-with-namely"></a>Öğretici: Azure Active Directory tümleştirmesiyle özelliği

Bu öğreticide, yani Azure Active Directory (Azure AD) ile tümleştirme konusunda bilgi edinin.

Yani Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Yani erişimi olan Azure AD'de denetleyebilirsiniz.
- Kullanıcılarınızın, yani otomatik olarak açan almak etkinleştirebilirsiniz (çoklu oturum açma) ile Azure AD hesaplarına
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Yani, tümleştirmesiyle Azure AD yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Yani çoklu oturum açma abonelik etkin.

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Yani galeri ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-namely-from-the-gallery"></a>Yani galeri ekleme
Yani Azure AD uygulamasına tümleştirmesini yapılandırmak için galerideki yani yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Yani galerideki eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **yani**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/namely-tutorial/tutorial_namely_search.png)

1. Sonuçlar panelinde seçin **yani**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/namely-tutorial/tutorial_namely_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma ile yani "Britta Simon" adlı bir test kullanıcı tabanlı test.

Tek iş için oturum açma için Azure AD ne karşılık gelen kullanıcı özelliği bir kullanıcının Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişki özelliği kurulması gerekir.

Yani değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırmanıza ve sınamanıza Azure AD çoklu oturum açma özelliği, sizinle şu yapı taşları tamamlamanız gereken:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Oluşturma bir kullanıcı özelliği test](#creating-a-namely-test-user)**  - Britta simon'un bir karşılığı vardır, yani, bağlı kullanıcı Azure AD temsili.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma, yani uygulama yapılandırın.

**Yani Azure AD çoklu oturum açma ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **yani** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/namely-tutorial/tutorial_namely_samlbase.png)

1. Üzerinde **adlı etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/namely-tutorial/tutorial_namely_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<subdomain>.namely.com`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<subdomain>.namely.com/saml/metadata`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [yani istemci Destek ekibine](https://www.namely.com/contact/) bu değerleri almak için. 
 
1. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/namely-tutorial/tutorial_namely_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/namely-tutorial/tutorial_general_400.png)

1. Üzerinde **yani yapılandırma** bölümünde **yani yapılandırma** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/namely-tutorial/tutorial_namely_configure.png) 

1. Başka bir tarayıcı penceresinde oturum açın, yani şirket site yönetici olarak.

1. Üst araç çubuğunda tıklatın **şirket**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/namely-tutorial/tutorial_namely_06.png) 

1. **Ayarlar** sekmesine tıklayın.
   
    ![Çoklu oturum açmayı yapılandırın](./media/namely-tutorial/tutorial_namely_07.png) 

1. Tıklayın **SAML**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/namely-tutorial/tutorial_namely_08.png) 

1. Üzerinde **SAML ayarlarını** sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açmayı yapılandırın](./media/namely-tutorial/tutorial_namely_09.png)
 
    a. Tıklayın **etkinleştirme SAML**. 

    b. İçinde **kimlik sağlayıcısı SSO URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmeti URL'si**, hangi Azure Portalı'ndan kopyaladığınız.
    
    c. İndirilen sertifikanızı Not Defteri'nde açın, içeriği kopyalayın ve ardından yapıştırın **kimlik sağlayıcısı sertifikası** metin.
     
    d. **Kaydet**’e tıklayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/namely-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/namely-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/namely-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/namely-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-namely-test-user"></a>Oluşturma bir özelliği test kullanıcısı

Bu bölümün amacı, yani Britta Simon adlı bir kullanıcı oluşturmaktır.

**Britta Simon yani adlı bir kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açma, yani şirket site yönetici olarak.

1. Üst araç çubuğunda tıklatın **kişiler**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/namely-tutorial/tutorial_namely_10.png) 

1. Tıklayın **dizin** sekmesi.
   
    ![Çoklu oturum açmayı yapılandırın](./media/namely-tutorial/tutorial_namely_11.png) 

1. Tıklayın **Yeni Kişi Ekle**.

    ![Çoklu oturum açmayı yapılandırın](./media/namely-tutorial/tutorial_namely_12.png)

1. Üzerinde **Yeni Kişi Ekle** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    a. İçinde **ad** metin kutusuna **Britta**.

    b. İçinde **Soyadı** metin kutusuna **Simon**.

    c. İçinde **e-posta** metin kutusuna **e-posta adresi** BrittaSimon biri.

    d. **Kaydet**’e tıklayın.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, yani erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Yani, aşağıdaki adımları gerçekleştirmek için Britta Simon atamak için:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **yani**.

    ![Çoklu oturum açmayı yapılandırın](./media/namely-tutorial/tutorial_namely_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümün amacı, erişim panelini kullanarak Azure AD SSO yapılandırmanızı sınamanızı sağlamaktır.

Tıkladığınızda, yani erişim Paneli'nde kutucuğunda, otomatik olarak imzalanmış, yani uygulamasını açma

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/namely-tutorial/tutorial_general_01.png
[2]: ./media/namely-tutorial/tutorial_general_02.png
[3]: ./media/namely-tutorial/tutorial_general_03.png
[4]: ./media/namely-tutorial/tutorial_general_04.png

[100]: ./media/namely-tutorial/tutorial_general_100.png

[200]: ./media/namely-tutorial/tutorial_general_200.png
[201]: ./media/namely-tutorial/tutorial_general_201.png
[202]: ./media/namely-tutorial/tutorial_general_202.png
[203]: ./media/namely-tutorial/tutorial_general_203.png

