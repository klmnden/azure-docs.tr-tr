---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile QuickHelp | Microsoft Docs'
description: Azure Active Directory ve QuickHelp arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 655c9ad3-2076-4e2c-8e47-9ed3bf04be56
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 013504fefd927d2e970c5b07a0e9c61afc715d7c
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56186922"
---
# <a name="tutorial-azure-active-directory-integration-with-quickhelp"></a>Öğretici: QuickHelp ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile QuickHelp tümleştirme konusunda bilgi edinin.

Azure AD ile QuickHelp tümleştirme ile aşağıdaki avantajları sağlar:

- QuickHelp erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için QuickHelp (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile QuickHelp yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik QuickHelp çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden QuickHelp ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-quickhelp-from-the-gallery"></a>Galeriden QuickHelp ekleme
Azure AD'de QuickHelp tümleştirmesini yapılandırmak için QuickHelp Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden QuickHelp eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **QuickHelp**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/quickhelp-tutorial/tutorial_quickhelp_search.png)

1. Sonuçlar panelinde seçin **QuickHelp**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/quickhelp-tutorial/tutorial_quickhelp_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı QuickHelp sınayın.

Tek iş için oturum açma için Azure AD ne QuickHelp karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının QuickHelp ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

QuickHelp içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma QuickHelp ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[QuickHelp test kullanıcısı oluşturma](#creating-a-quickhelp-test-user)**  - kullanıcı Azure AD gösterimini bağlı QuickHelp Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve QuickHelp uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile QuickHelp yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **QuickHelp** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/quickhelp-tutorial/tutorial_quickhelp_samlbase.png)

1. Üzerinde **QuickHelp etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/quickhelp-tutorial/tutorial_quickhelp_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://quickhelp.com/<ROUTEURL>`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL: `https://auth.quickhelp.com`

    > [!NOTE] 
    > Oturum açma URL değeri, gerçek değil. Değerini gerçek oturum açma URL'si ile güncelleştirin. Değerini almak için kuruluşunuzun QuickHelp yöneticinize veya beyin fırtınasını istemci başarı yöneticinize başvurun.
 
1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/quickhelp-tutorial/tutorial_quickhelp_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/quickhelp-tutorial/tutorial_general_400.png) 

1. QuickHelp şirketinizin sitesi için yönetici olarak oturum.

1. Üstteki menüden **yönetici**.
   
    ![Çoklu oturum açmayı yapılandırın][21]

1. İçinde **QuickHelp yönetici** menüsünde tıklatın **ayarları**.
   
    ![Çoklu oturum açmayı yapılandırın][22]

1. Tıklayın **kimlik doğrulama ayarları**.

1. Üzerinde **kimlik doğrulama ayarları** sayfasında, aşağıdaki adımları gerçekleştirin
   
    ![Çoklu oturum açmayı yapılandırın][23]
   
    a. Olarak **SSO türü**seçin **WSFederation**.
   
    b. İndirilen Azure meta verileri dosyanızı karşıya yüklemek için tıklayın **Gözat**, dosyasına gidin, ardından son **meta verilerini karşıya yükleme**.
   
    c. İçinde **e-posta** metin kutusuna `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.
   
    d. İçinde **ad** metin `type http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.
   
    e. İçinde **Soyadı** metin `type http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.
   
    f. İçinde **Eylem çubuğu**, tıklayın **Kaydet**.

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/quickhelp-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/quickhelp-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/quickhelp-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/quickhelp-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-quickhelp-test-user"></a>QuickHelp test kullanıcısı oluşturma

Bu bölümün amacı QuickHelp Britta Simon adlı bir kullanıcı oluşturmaktır.
Tek iş için oturum açma için Azure AD QuickHelp karşılığı kullanıcının Azure AD'de bir kullanıcıya ne olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının QuickHelp ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Tam zamanında sağlama QuickHelp destekler. Bunun anlamı, gerekirse, bir kullanıcı hesabı QuickHelp içinde otomatik olarak oluşturulan ve hesap Azure AD hesabına bağlanır.

Bu bölümde, hiçbir eylem öğesini yoktur.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için QuickHelp erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon QuickHelp için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **QuickHelp**.

    ![Çoklu oturum açmayı yapılandırın](./media/quickhelp-tutorial/tutorial_quickhelp_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümün amacı, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test sağlamaktır.  

Erişim panelinde QuickHelp kutucuğa tıkladığınızda, otomatik olarak QuickHelp uygulamanıza açan.


## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/quickhelp-tutorial/tutorial_general_01.png
[2]: ./media/quickhelp-tutorial/tutorial_general_02.png
[3]: ./media/quickhelp-tutorial/tutorial_general_03.png
[4]: ./media/quickhelp-tutorial/tutorial_general_04.png

[100]: ./media/quickhelp-tutorial/tutorial_general_100.png

[200]: ./media/quickhelp-tutorial/tutorial_general_200.png
[201]: ./media/quickhelp-tutorial/tutorial_general_201.png
[202]: ./media/quickhelp-tutorial/tutorial_general_202.png
[203]: ./media/quickhelp-tutorial/tutorial_general_203.png
[21]: ./media/quickhelp-tutorial/tutorial_quickhelp_05.png
[22]: ./media/quickhelp-tutorial/tutorial_quickhelp_06.png
[23]: ./media/quickhelp-tutorial/tutorial_quickhelp_07.png
