---
title: 'Öğretici: Rightscale ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Rightscale arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 3a8d376d-95fb-4dd7-832a-4fdd4dd7c87c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 416a98c5f9c5a2ec813206ea9ea7f311b23e86cb
ms.sourcegitcommit: 1c2cf60ff7da5e1e01952ed18ea9a85ba333774c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/12/2019
ms.locfileid: "59525589"
---
# <a name="tutorial-azure-active-directory-integration-with-rightscale"></a>Öğretici: Rightscale ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Rightscale tümleştirme konusunda bilgi edinin.

Rightscale Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Rightscale erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için Rightscale (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Rightscale yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Rightscale çoklu oturum açma abonelik etkin.

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Rightscale galeri ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-rightscale-from-the-gallery"></a>Rightscale galeri ekleme
Azure AD'de Rightscale tümleştirmesini yapılandırmak için Rightscale Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Rightscale eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **Rightscale**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/rightscale-tutorial/tutorial_rightscale_search.png)

1. Sonuçlar panelinde seçin **Rightscale**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/rightscale-tutorial/tutorial_rightscale_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Rightscale sınayın.

Tek iş için oturum açma için Azure AD ne Rightscale karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Rightscale ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Rightscale içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Rightscale ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Rightscale test kullanıcısı oluşturma](#creating-a-rightscale-test-user)**  - kullanıcı Azure AD gösterimini bağlı Rightscale Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Rightscale uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Rightscale yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Rightscale** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/rightscale-tutorial/tutorial_rightscale_samlbase.png)

1. Üzerinde **Rightscale etki alanı ve URL'ler** uygulamada yapılandırmak isterseniz, bölümü **IDP tarafından başlatılan modu** uygulama zaten Azure ile önceden tümleştirilmiş olduğu gibi tüm adımları gerekmez.

    ![Çoklu oturum açmayı yapılandırın](./media/rightscale-tutorial/tutorial_rightscale_url.png)

1. Üzerinde **Rightscale etki alanı ve URL'ler** uygulamada yapılandırmak isterseniz, bölümü **SP tarafından başlatılan modu**, aşağıdaki adımları gerçekleştirin:
    
    ![Çoklu oturum açmayı yapılandırın](./media/rightscale-tutorial/tutorial_rightscale_url1.png)

    a. Tıklayarak **Gelişmiş URL ayarlarını göster**.

    b. İçinde **işareti bulunan URL'si** metin kutusuna URL'yi yazın: `https://login.rightscale.com/`

1. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/rightscale-tutorial/tutorial_rightscale_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/rightscale-tutorial/tutorial_general_400.png)

1. Üzerinde **Rightscale yapılandırma** bölümünde **yapılandırma Rightscale** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/rightscale-tutorial/tutorial_rightscale_configure.png) 

1. Uygulamanız için yapılandırılmış SSO elde etmek için RightScale Kiracı yönetici olarak oturum açma gerekir.

    a. Üstteki menüden **ayarları** sekmenize **çoklu oturum açma**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/rightscale-tutorial/tutorial_rightscale_001.png) 

    b. Tıklayın "**yeni**" eklemek için Ekle düğmesine **bilgisayarınızı SAML kimlik sağlayıcısı**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/rightscale-tutorial/tutorial_rightscale_002.png) 
 
    c. Metin kutusuna **görünen ad**, şirketinizin adını girin.
   
    ![Çoklu oturum açmayı yapılandırın](./media/rightscale-tutorial/tutorial_rightscale_003.png)
 
    d. Seçin **izin RightScale tarafından başlatılan SSO'yu bir bulma ipucu kullanarak** ve giriş, **etki alanı adı** içinde metin kutusu altında.
   
    ![Çoklu oturum açmayı yapılandırın](./media/rightscale-tutorial/tutorial_rightscale_004.png)

    e. Değerini yapıştırın **SAML çoklu oturum açma hizmeti URL'si** Azure portalından kopyalanan **SAML SSO uç noktası** RightScale içinde.
   
    ![Çoklu oturum açmayı yapılandırın](./media/rightscale-tutorial/tutorial_rightscale_006.png)

    f. Değerini yapıştırın **SAML varlık kimliği** Azure portalından kopyalanan **SAML Entityıd** RightScale içinde.
   
    ![Çoklu oturum açmayı yapılandırın](./media/rightscale-tutorial/tutorial_rightscale_008.png)

    g. Tıklayın **tarayıcı** Azure portalından indirilen sertifikayı karşıya yüklemek için düğme.
   
    ![Çoklu oturum açmayı yapılandırın](./media/rightscale-tutorial/tutorial_rightscale_009.png)

    h. **Kaydet**’e tıklayın.

   > [!TIP]
   > İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
   > 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/rightscale-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/rightscale-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/rightscale-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/rightscale-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-rightscale-test-user"></a>Rightscale test kullanıcısı oluşturma

Bu bölümde, Britta Simon RightScale içinde adlı bir kullanıcı oluşturun. Çalışmak [Rightscale istemci Destek ekibine](mailto:support@rightscale.com) RightScale platform kullanıcıları eklemek için.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için Rightscale erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Rightscale için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Rightscale**.

    ![Çoklu oturum açmayı yapılandırın](./media/rightscale-tutorial/tutorial_rightscale_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümün amacı, erişim panelini kullanarak Azure AD SSO yapılandırmanızı sınamanızı sağlamaktır.  

Erişim panelinde RightScale kutucuğa tıkladığınızda, otomatik olarak RightScale uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/rightscale-tutorial/tutorial_general_01.png
[2]: ./media/rightscale-tutorial/tutorial_general_02.png
[3]: ./media/rightscale-tutorial/tutorial_general_03.png
[4]: ./media/rightscale-tutorial/tutorial_general_04.png

[100]: ./media/rightscale-tutorial/tutorial_general_100.png

[200]: ./media/rightscale-tutorial/tutorial_general_200.png
[201]: ./media/rightscale-tutorial/tutorial_general_201.png
[202]: ./media/rightscale-tutorial/tutorial_general_202.png
[203]: ./media/rightscale-tutorial/tutorial_general_203.png

