---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle edici çevrimiçi | Microsoft Docs'
description: Azure Active Directory ve edici Online arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 4fd6b29b-1b46-4fd1-9f5e-16b1c9d892cd
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 043226f333649132cb33a64609a68a705851068e
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55173437"
---
# <a name="tutorial-azure-active-directory-integration-with-bgs-online"></a>Öğretici: Azure Active Directory tümleştirmesiyle edici çevrimiçi

Bu öğreticide, Azure Active Directory (Azure AD) ile edici çevrimiçi tümleştirme konusunda bilgi edinin.

EDİCİ çevrimiçi Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- EDİCİ Online'a erişimi, Azure AD'de denetleyebilirsiniz
- Azure AD hesaplarına otomatik olarak imzalanan edici Online'a (çoklu oturum açma) açma, kullanıcılarınızın etkinleştirebilirsiniz
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi edici Online ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir edici çevrimiçi çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. EDİCİ çevrimiçi galeriden ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-bgs-online-from-the-gallery"></a>EDİCİ çevrimiçi galeriden ekleme
Azure AD'de edici Online'nın tümleştirmesini yapılandırmak için edici çevrimiçi galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**EDİCİ çevrimiçi Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **edici çevrimiçi**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/bgsonline-tutorial/tutorial_bgsonline_search.png)

1. Sonuçlar panelinde seçin **edici çevrimiçi**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/bgsonline-tutorial/tutorial_bgsonline_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." adlı bir test kullanıcı tabanlı edici Online ile test etme

Tek çalışmak için oturum açma için Azure AD ne karşılık gelen kullanıcı edici çevrimiçi bir kullanıcının Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve ilgili kullanıcı edici Online arasında bir bağlantı ilişki kurulması gerekir.

EDİCİ Online'da değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma edici Online ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[EDİCİ çevrimiçi bir test kullanıcısı oluşturma](#creating-a-bgs-online-test-user)**  - edici kullanıcı Azure AD gösterimini bağlı olan çevrimiçi bir karşılığı Britta simon'un sağlamak için.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve edici çevrimiçi uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma edici Online ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **edici çevrimiçi** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/bgsonline-tutorial/tutorial_bgsonline_samlbase.png)

1. Üzerinde **edici çevrimiçi etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/bgsonline-tutorial/tutorial_bgsonline_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak:

    Üretim ortamı için bu düzeni kullanın. `https://<company name>.millwardbrown.report` 

    Test ortamı için bu düzeni kullanın. `https://millwardbrown.marketingtracker.nl/mt5/`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak:
    
    Üretim ortamı için bu düzeni kullanın. `https://<company name>.millwardbrown.report/sso/saml/AssertionConsumerService.aspx` 
      
    Test ortamı için bu düzeni kullanın. `https://millwardbrown.marketingtracker.nl/mt5/sso/saml/AssertionConsumerService.aspx`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı ve yanıt URL'si ile güncelleştirin. İlgili kişi [edici Çevrimiçi Destek ekibine](mailTo:bgsdashboardteam@millwardbrown.com) bu değerleri almak için.
 

1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/bgsonline-tutorial/tutorial_bgsonline_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/bgsonline-tutorial/tutorial_general_400.png)

1. Üzerinde **edici çevrimiçi yapılandırma** bölümünde **edici çevrimiçi yapılandırma** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/bgsonline-tutorial/tutorial_bgsonline_configure.png) 

1. Çoklu oturum açmayı yapılandırma **edici çevrimiçi** tarafı, indirilen göndermek için ihtiyacınız **meta veri XML** ve **SAML çoklu oturum açma hizmeti URL'si** için [edici çevrimiçi Destek](mailto:bgsdashboardteam@millwardbrown.com). 


> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/bgsonline-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/bgsonline-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/bgsonline-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/bgsonline-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-bgs-online-test-user"></a>EDİCİ çevrimiçi bir test kullanıcısı oluşturma

Bu bölümde, Britta Simon edici Online'da adlı bir kullanıcı oluşturun. Çalışmak [edici Çevrimiçi Destek ekibine](mailto:bgsdashboardteam@millwardbrown.com) edici çevrimiçi platform kullanıcıları eklemek için.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma edici çevrimiçi erişim vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon edici çevrimiçi olarak atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **edici çevrimiçi**.

    ![Çoklu oturum açmayı yapılandırın](./media/bgsonline-tutorial/tutorial_bgsonline_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Azure AD SSO yapılandırmanızı erişim panelini kullanarak test edin.

Erişim paneli edici çevrimiçi kutucuğa tıkladığınızda, size otomatik olarak edici çevrimiçi uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/bgsonline-tutorial/tutorial_general_01.png
[2]: ./media/bgsonline-tutorial/tutorial_general_02.png
[3]: ./media/bgsonline-tutorial/tutorial_general_03.png
[4]: ./media/bgsonline-tutorial/tutorial_general_04.png

[100]: ./media/bgsonline-tutorial/tutorial_general_100.png

[200]: ./media/bgsonline-tutorial/tutorial_general_200.png
[201]: ./media/bgsonline-tutorial/tutorial_general_201.png
[202]: ./media/bgsonline-tutorial/tutorial_general_202.png
[203]: ./media/bgsonline-tutorial/tutorial_general_203.png

