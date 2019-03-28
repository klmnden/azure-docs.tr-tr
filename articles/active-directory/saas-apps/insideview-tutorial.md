---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile InsideView | Microsoft Docs'
description: Azure Active Directory ve InsideView arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: c489a7ab-6b1f-4efb-8a66-8bc13bca78c3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 845ab85ae10a9d57bf3e263d49532675f60cd84f
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58517853"
---
# <a name="tutorial-azure-active-directory-integration-with-insideview"></a>Öğretici: InsideView ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile InsideView tümleştirme konusunda bilgi edinin.

Azure AD ile InsideView tümleştirme ile aşağıdaki avantajları sağlar:

- InsideView erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için InsideView (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile InsideView yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik bir InsideView çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden InsideView ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-insideview-from-the-gallery"></a>Galeriden InsideView ekleme
Azure ad içinde InsideView tümleştirmesini yapılandırmak için InsideView Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden InsideView eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **InsideView**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/insideview-tutorial/tutorial_insideview_search.png)

1. Sonuçlar panelinde seçin **InsideView**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/insideview-tutorial/tutorial_insideview_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." adlı bir test kullanıcı tabanlı InsideView ile test etme

Tek iş için oturum açma için Azure AD ne InsideView karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının InsideView ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

InsideView içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma InsideView ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Bir InsideView test kullanıcısı oluşturma](#creating-an-insideview-test-user)**  - kullanıcı Azure AD gösterimini bağlı InsideView Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve InsideView uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile InsideView yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **InsideView** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/insideview-tutorial/tutorial_insideview_samlbase.png)

1. Üzerinde **InsideView etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/insideview-tutorial/tutorial_insideview_url.png)
    
    İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://my.insideview.com/iv/<STS Name>/login.iv`

    > [!NOTE] 
    > Bu değer, gerçek değil. Bu değer, gerçek yanıt URL'si ile güncelleştirin. İlgili kişi [InsideView Destek ekibine](mailto:support@insideview.com) bu değeri alınamıyor.
 
1. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (ham)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/insideview-tutorial/tutorial_insideview_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/insideview-tutorial/tutorial_general_400.png)

1. Üzerinde **InsideView yapılandırma** bölümünde **yapılandırma InsideView** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/insideview-tutorial/tutorial_insideview_configure.png) 

1. Farklı bir web tarayıcı penceresinde InsideView şirketinizin sitesi için bir yönetici olarak oturum açın.

1. Üst araç çubuğunda tıklatın **yönetici**, **SingleSignOn ayarları**ve ardından **ekleme SAML**.
   
   ![SAML çoklu oturum açma ayarları](./media/insideview-tutorial/ic794135.png "SAML çoklu oturum açma ayarları")

1. İçinde **yeni bir SAML ekleme** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Yeni bir SAML ekleme](./media/insideview-tutorial/ic794136.png "yeni bir SAML Ekle")
   
    a. İçinde **STS adını** metin yapılandırmanız için bir ad yazın.

    b. İçinde **SamlP/WS-Federasyon istenmeyen uç nokta** metin değerini yapıştırın **SAML çoklu oturum açma hizmeti URL'si**, hangi Azure Portalı'ndan kopyaladığınız.
    
    c. Azure Portalı'ndan yüklediğiniz, base-64 kodlanmış sertifika açın içeriğini sizin panoya kopyalayın ve yapıştırın kendisine **STS sertifikasını** metin.

    d. İçinde **Crm kullanıcı eşleme kimliği** metin kutusuna `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.
        
    e. İçinde **e-posta Crm eşleme** metin kutusuna `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.

    f. İçinde **Crm ad eşleme** metin kutusuna `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.
    
    g. İçinde **Crm lastName eşleme** metin kutusuna `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.  

    h. **Kaydet**’e tıklayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 
 
### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/insideview-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/insideview-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/insideview-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/insideview-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-an-insideview-test-user"></a>Bir InsideView test kullanıcısı oluşturma

Azure AD kullanıcıları için InsideView oturum açmak etkinleştirmek için bunlar içinde InsideView için sağlanması gerekir. InsideView söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

Kullanıcılar ve kişiler InsideView içinde oluşturulan almak için başvurun [InsideView Destek ekibine](mailto:support@insideview.com).

>[!NOTE]
>Herhangi diğer InsideView kullanıcı hesabı oluşturma araçları kullanabilir veya API Azure AD'ye kullanıcı hesapları sağlamak için InsideView tarafından sağlanan.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için InsideView erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon InsideView için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **InsideView**.

    ![Çoklu oturum açmayı yapılandırın](./media/insideview-tutorial/tutorial_insideview_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde InsideView kutucuğa tıkladığınızda, otomatik olarak InsideView uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/insideview-tutorial/tutorial_general_01.png
[2]: ./media/insideview-tutorial/tutorial_general_02.png
[3]: ./media/insideview-tutorial/tutorial_general_03.png
[4]: ./media/insideview-tutorial/tutorial_general_04.png

[100]: ./media/insideview-tutorial/tutorial_general_100.png

[200]: ./media/insideview-tutorial/tutorial_general_200.png
[201]: ./media/insideview-tutorial/tutorial_general_201.png
[202]: ./media/insideview-tutorial/tutorial_general_202.png
[203]: ./media/insideview-tutorial/tutorial_general_203.png
