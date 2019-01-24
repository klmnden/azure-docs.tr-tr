---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile CloudPassage | Microsoft Docs'
description: Azure Active Directory ve CloudPassage arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: bfe1f14e-74e4-4680-ac9e-f7355e1c94cc
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 10ef9e0c07f6bad393fdb62de85456483284a998
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54821320"
---
# <a name="tutorial-azure-active-directory-integration-with-cloudpassage"></a>Öğretici: CloudPassage ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile CloudPassage tümleştirme konusunda bilgi edinin.

Azure AD ile CloudPassage tümleştirme ile aşağıdaki avantajları sağlar:

- CloudPassage erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için CloudPassage (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile CloudPassage yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik CloudPassage çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden CloudPassage ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-cloudpassage-from-the-gallery"></a>Galeriden CloudPassage ekleme
Azure AD'de CloudPassage tümleştirmesini yapılandırmak için CloudPassage Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden CloudPassage eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **CloudPassage**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/cloudpassage-tutorial/tutorial_cloudpassage_search.png)

1. Sonuçlar panelinde seçin **CloudPassage**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/cloudpassage-tutorial/tutorial_cloudpassage_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." adlı bir test kullanıcı tabanlı CloudPassage ile test etme

Tek iş için oturum açma için Azure AD ne CloudPassage karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının CloudPassage ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

CloudPassage içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma CloudPassage ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[CloudPassage test kullanıcısı oluşturma](#creating-a-cloudpassage-test-user)**  - kullanıcı Azure AD gösterimini bağlı CloudPassage Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve CloudPassage uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile CloudPassage yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **CloudPassage** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/cloudpassage-tutorial/tutorial_cloudpassage_samlbase.png)

1. Üzerinde **CloudPassage etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/cloudpassage-tutorial/tutorial_cloudpassage_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://portal.cloudpassage.com/saml/init/accountid`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://portal.cloudpassage.com/saml/consume/accountid`. Tıklayarak, bu öznitelik için bu değeri alabilirsiniz **SSO Kurulum belgeleri** içinde **çoklu oturum açma ayarları** CloudPassage portalınız bölümü.

    ![Çoklu oturum açmayı yapılandırın](./media/cloudpassage-tutorial/tutorial_cloudpassage_05.png)
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek yanıt URL'si ve oturum açma URL'si ile güncelleştirin. İlgili kişi [CloudPassage istemci Destek ekibine](https://www.cloudpassage.com/company/contact/) bu değerleri almak için. 

1. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/cloudpassage-tutorial/tutorial_cloudpassage_certificate.png) 

1. CloudPassage uygulamanız SAML onaylamalarını özel öznitelik eşlemelerini SAML belirteci öznitelikleri yapılandırmanıza ekleyin gerektiren belirli bir biçimde bekliyor. Aşağıdaki ekran görüntüsü bunun bir örneği gösterilmektedir.
   
   ![Çoklu oturum açmayı yapılandırın](./media/cloudpassage-tutorial/tutorial_cloudpassage_25.png) 

1. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, SAML belirteci özniteliği yukarıdaki görüntüde gösterilen şekilde yapılandırın ve aşağıdaki adımları gerçekleştirin:

    | Öznitelik Adı | Öznitelik Değeri |
    | --- | --- |
    | firstName |User.givenName |
    | Soyadı |User.surname |
    | e-posta |User.Mail |
    
    a. Tıklayın **eklemek agentconfigutil** açmak için **öznitelik Ekle** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/cloudpassage-tutorial/tutorial_attribute_04.png)
    
    ![Çoklu oturum açmayı yapılandırın](./media/cloudpassage-tutorial/tutorial_attribute_05.png)
    
    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.

    c. Gelen **değer** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.
    
    d. **Tamam**’a tıklayın.

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/cloudpassage-tutorial/tutorial_general_400.png)
    
1. Üzerinde **CloudPassage yapılandırma** bölümünde **yapılandırma CloudPassage** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/cloudpassage-tutorial/tutorial_cloudpassage_configure.png) 

1. Bir farklı bir tarayıcı penceresinde CloudPassage şirketinizin sitesi için yönetici olarak oturum.

1. Üstteki menüden **ayarları**ve ardından **Site Yönetimi**. 
   
    ![Çoklu oturum açmayı yapılandırın][12]

1. Tıklayın **kimlik doğrulama ayarları** sekmesi. 
   
    ![Çoklu oturum açmayı yapılandırın][13]

1. İçinde **çoklu oturum açma ayarları** bölümünde, aşağıdaki adımları gerçekleştirin: 
   
    ![Çoklu oturum açmayı yapılandırın][14]

    a. Seçin **etkinleştirme tek sign-on(SSO) (SSO Kurulum belgeleri)** onay kutusu.
    
    b. Yapıştırma **SAML varlık kimliği** içine **SAML veren URL'si** metin.
  
    c. Yapıştırma **SAML çoklu oturum açma hizmeti URL'si** içine **SAML uç nokta URL'si** metin.
  
    d. Yapıştırma **oturum kapatma URL'si** içine **oturum kapatma giriş sayfası** metin.
  
    e. İndirilen sertifikanızı Not Defteri'nde açın, indirilen sertifikanın içeriğini sizin panoya kopyalayın ve ardından yapıştırın **x 509 sertifikası** metin.
  
    f. **Kaydet**’e tıklayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/cloudpassage-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/cloudpassage-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/cloudpassage-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/cloudpassage-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-cloudpassage-test-user"></a>CloudPassage test kullanıcısı oluşturma

Bu bölümün amacı CloudPassage Britta Simon adlı bir kullanıcı oluşturmaktır.

**Britta Simon CloudPassage içinde adlı bir kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açma, **CloudPassage** yönetici olarak şirketin site. 

1. Üst araç çubuğunda tıklatın **ayarları**ve ardından **Site Yönetimi**. 
   
   ![CloudPassage test kullanıcısı oluşturma][22] 

1. Tıklayın **kullanıcılar** sekmesine ve ardından **yeni kullanıcı Ekle**. 
   
   ![CloudPassage test kullanıcısı oluşturma][23]

1. İçinde **yeni kullanıcı Ekle** bölümünde, aşağıdaki adımları gerçekleştirin: 
   
   ![CloudPassage test kullanıcısı oluşturma][24]
    
    a. İçinde **ad** metin Britta yazın. 
  
    b. İçinde **Soyadı** metin Simon yazın.
  
    c. İçinde **kullanıcıadı** metin **e-posta** metin kutusu ve **yeniden e-posta** metin Azure AD'de Britta'nın kullanıcı adı yazın.
  
    d. Olarak **erişim türü**seçin **Halo Portal erişimini etkinleştir**.
  
    e. **Ekle**'ye tıklayın.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için CloudPassage erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon CloudPassage için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **CloudPassage**.

    ![Çoklu oturum açmayı yapılandırın](./media/cloudpassage-tutorial/tutorial_cloudpassage_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümün amacı, erişim panelini kullanarak Azure AD SSO yapılandırmanızı sınamanızı sağlamaktır.

Erişim panelinde CloudPassage kutucuğa tıkladığınızda, otomatik olarak CloudPassage uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/cloudpassage-tutorial/tutorial_general_01.png
[2]: ./media/cloudpassage-tutorial/tutorial_general_02.png
[3]: ./media/cloudpassage-tutorial/tutorial_general_03.png
[4]: ./media/cloudpassage-tutorial/tutorial_general_04.png
[12]: ./media/cloudpassage-tutorial/tutorial_cloudpassage_07.png
[13]: ./media/cloudpassage-tutorial/tutorial_cloudpassage_08.png
[14]: ./media/cloudpassage-tutorial/tutorial_cloudpassage_09.png
[15]: ./media/cloudpassage-tutorial/tutorial_cloudpassage_10.png
[22]: ./media/cloudpassage-tutorial/tutorial_cloudpassage_15.png
[23]: ./media/cloudpassage-tutorial/tutorial_cloudpassage_16.png
[24]: ./media/cloudpassage-tutorial/tutorial_cloudpassage_17.png

[100]: ./media/cloudpassage-tutorial/tutorial_general_100.png

[200]: ./media/cloudpassage-tutorial/tutorial_general_200.png
[201]: ./media/cloudpassage-tutorial/tutorial_general_201.png
[202]: ./media/cloudpassage-tutorial/tutorial_general_202.png
[203]: ./media/cloudpassage-tutorial/tutorial_general_203.png

