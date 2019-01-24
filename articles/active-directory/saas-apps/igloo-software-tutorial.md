---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Igloo yazılım | Microsoft Docs'
description: Azure Active Directory ve Igloo yazılım arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 2eb625c1-d3fc-4ae1-a304-6a6733a10e6e
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: daa00db39ac55cd14b7720b534313407a98dde2c
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54817750"
---
# <a name="tutorial-azure-active-directory-integration-with-igloo-software"></a>Öğretici: Igloo yazılım ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Igloo yazılım tümleştirme konusunda bilgi edinin.

Azure AD ile Igloo yazılım tümleştirme ile aşağıdaki avantajları sağlar:

- Igloo yazılım erişimi, Azure AD'de denetleyebilirsiniz
- Azure AD hesaplarına otomatik olarak imzalanan Igloo yazılımı (çoklu oturum açma) açma, kullanıcılarınızın etkinleştirebilirsiniz
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Igloo yazılımıyla yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik bir Igloo yazılım çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Igloo yazılım ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-igloo-software-from-the-gallery"></a>Galeriden Igloo yazılım ekleme
Azure AD'de Igloo yazılım tümleştirmesini yapılandırmak için Igloo yazılım Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Igloo yazılım eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **Igloo yazılım**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/igloo-software-tutorial/tutorial_igloosoftware_search.png)

1. Sonuçlar panelinde seçin **Igloo yazılım**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/igloo-software-tutorial/tutorial_igloosoftware_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırın ve Igloo yazılım'ın "Britta Simon" adlı bir test kullanıcı tabanlı Azure AD çoklu oturum açmayı sınayın.

Tek iş için oturum açma için Azure AD ne Igloo yazılım karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Igloo yazılımla ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Igloo yazılımda değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Igloo yazılım ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Bir Igloo yazılım test kullanıcısı oluşturma](#creating-an-igloo-software-test-user)**  - kullanıcı Azure AD gösterimini bağlı Igloo yazılım Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Igloo yazılım uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma Igloo yazılımıyla yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Igloo yazılım** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/igloo-software-tutorial/tutorial_igloosoftware_samlbase.png)

1. Üzerinde **Igloo yazılım etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/igloo-software-tutorial/tutorial_igloosoftware_url.png)
    
    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<company name>.igloocommmunities.com`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<company name>.igloocommmunities.com/saml.digest`

    c. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<company name>.igloocommmunities.com/saml.digest`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. İlgili kişi [Igloo yazılım istemcisi Destek ekibine](https://www.igloosoftware.com/services/support) bu değerleri almak için. 

1. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/igloo-software-tutorial/tutorial_igloosoftware_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/igloo-software-tutorial/tutorial_general_400.png)
    
1. Üzerinde **Igloo yazılım yapılandırma** bölümünde **Igloo yazılımı Yapılandır** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/igloo-software-tutorial/tutorial_igloosoftware_configure.png) 

1. Farklı bir web tarayıcı penceresinde Igloo yazılım şirketinizin sitesi için bir yönetici olarak oturum açın.

1. Git **Denetim Masası**.
   
     ![Denetim Masası](./media/igloo-software-tutorial/ic799949.png "Denetim Masası")

1. İçinde **üyelik** sekmesinde **oturum ayarları**.
   
    ![Oturum açma ayarları](./media/igloo-software-tutorial/ic783968.png "oturum açma ayarları")

1. SAML yapılandırma bölümünde tıklayın **SAML kimlik doğrulamasını yapılandırma**.
   
    ![SAML yapılandırma](./media/igloo-software-tutorial/ic783969.png "SAML yapılandırma")
   
1. İçinde **genel yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Genel yapılandırma](./media/igloo-software-tutorial/ic783970.png "genel yapılandırma")

    a. İçinde **bağlantı adı** metin yapılandırmanızı için özel bir ad yazın.
   
    b. İçinde **Idp'nin oturum açma URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmeti URL'si** , Azure Portalı'ndan kopyaladığınız.
   
    c. İçinde **IDP oturum kapatma URL'si** metin değerini yapıştırın **oturum kapatma URL'si** , Azure Portalı'ndan kopyaladığınız.
    
    d. Seçin **oturum kapatma yanıt ve İstek HTTP türü** olarak **POST**.
   
    e. Açık, **base-64** Defteri'nde kodlanmış sertifika Azure portalından indirilen, içeriğini, panoya kopyalayın ve ardından ona yapıştırın **ortak sertifika** metin.
    
1. İçinde **yanıt ve kimlik doğrulama Yapılandırması**, aşağıdaki adımları gerçekleştirin:
    
    ![Yanıt ve kimlik doğrulama Yapılandırması](./media/igloo-software-tutorial/IC783971.png "yanıt ve kimlik doğrulaması yapılandırması")
  
      a. Olarak **kimlik sağlayıcısı**seçin **Microsoft ADFS**.
      
      b. Olarak **tanımlayıcı türü**seçin **e-posta adresi**. 

      c. İçinde **e-posta özniteliği** metin kutusuna **emailaddress**.

      d. İçinde **ad özniteliği** metin kutusuna **givenname**.

      e. İçinde **son Name özniteliği** metin kutusuna **Soyadı**.

1. Yapılandırmayı tamamlamak için aşağıdaki adımları gerçekleştirin:
    
    ![Oturum açma kullanıcı oluşturma](./media/igloo-software-tutorial/IC783972.png "oturum açma kullanıcı oluşturma") 

     a. Olarak **oturum açma kullanıcı oluşturma**seçin **oturum açtığında, sitenizde yeni bir kullanıcı oluşturmak**.

     b. Olarak **oturum açma ayarları**seçin **ekranında "Oturum Aç" düğmesi kullanım SAML**.

     c. **Kaydet**’e tıklayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/igloo-software-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/igloo-software-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/igloo-software-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/igloo-software-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-an-igloo-software-test-user"></a>Bir Igloo yazılım test kullanıcısı oluşturma

Hiçbir eylem öğesini, Igloo yazılım için kullanıcı sağlamayı yapılandırma yoktur.  

Atanan kullanıcı Igloo yazılıma erişim panelini kullanarak oturum açmak çalıştığında, Igloo yazılım, kullanıcının mevcut olup olmadığını denetler.  Varsa kullanıcı hesabı kullanılabilir henüz Igloo yazılımı tarafından otomatik olarak oluşturulur.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma Igloo yazılıma erişim vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Igloo yazılımı atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Igloo yazılım**.

    ![Çoklu oturum açmayı yapılandırın](./media/igloo-software-tutorial/tutorial_igloosoftware_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Igloo yazılım kutucuğa tıkladığınızda, otomatik olarak Igloo yazılım uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/igloo-software-tutorial/tutorial_general_01.png
[2]: ./media/igloo-software-tutorial/tutorial_general_02.png
[3]: ./media/igloo-software-tutorial/tutorial_general_03.png
[4]: ./media/igloo-software-tutorial/tutorial_general_04.png

[100]: ./media/igloo-software-tutorial/tutorial_general_100.png

[200]: ./media/igloo-software-tutorial/tutorial_general_200.png
[201]: ./media/igloo-software-tutorial/tutorial_general_201.png
[202]: ./media/igloo-software-tutorial/tutorial_general_202.png
[203]: ./media/igloo-software-tutorial/tutorial_general_203.png

