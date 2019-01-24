---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile MaxxPoint | Microsoft Docs'
description: Azure Active Directory ve MaxxPoint arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 15ba026e-96fc-4ae8-b135-0169da810e99
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/13/2017
ms.author: jeedes
ms.openlocfilehash: b4cf7c8388b0a5a278298fff274ba49d542bae36
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54806972"
---
# <a name="tutorial-azure-active-directory-integration-with-maxxpoint"></a>Öğretici: MaxxPoint ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile MaxxPoint tümleştirme konusunda bilgi edinin.

Azure AD ile MaxxPoint tümleştirme ile aşağıdaki avantajları sağlar:

- MaxxPoint erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için MaxxPoint (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile MaxxPoint yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir MaxxPoint çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Bu gerekli olmadığı sürece üretim ortamınızı kullanmamanız gerekir.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden MaxxPoint ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma


## <a name="adding-maxxpoint-from-the-gallery"></a>Galeriden MaxxPoint ekleme
Azure AD'de MaxxPoint tümleştirmesini yapılandırmak için MaxxPoint Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden MaxxPoint eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Tıklayın **yeni uygulama** yeni bir uygulama eklemek için iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **MaxxPoint**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/maxxpoint-tutorial/tutorial_maxxpoint_001.png)

1. Sonuçlar panelinde seçin **MaxxPoint**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/maxxpoint-tutorial/tutorial_maxxpoint_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı MaxxPoint sınayın.

Tek iş için oturum açma için Azure AD ne MaxxPoint karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının MaxxPoint ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Değerini atayarak bu bağlantı ilişki kurulduktan **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** MaxxPoint içinde.

Yapılandırma ve Azure AD çoklu oturum açma MaxxPoint ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[MaxxPoint test kullanıcısı oluşturma](#creating-a-maxxpoint-test-user)**  - Azure AD gösterimini her için bağlı MaxxPoint Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve MaxxPoint uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile MaxxPoint yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **MaxxPoint** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/maxxpoint-tutorial/tutorial_general_300.png)

1. Üzerinde **MaxxPoint etki alanı ve URL'ler** uygulamada yapılandırmak isterseniz, bölümü **IDP tarafından başlatılan modu**, herhangi bir adım gerçekleştirmeniz gerekmez.

    ![Çoklu oturum açmayı yapılandırın](./media/maxxpoint-tutorial/tutorial_maxxpoint_02.png)
    
1. Üzerinde **MaxxPoint etki alanı ve URL'ler** uygulamada yapılandırmak isterseniz, bölümü **SP tarafından başlatılan modu**, aşağıdaki adımları gerçekleştirin:
    
    ![Çoklu oturum açmayı yapılandırın](./media/maxxpoint-tutorial/tutorial_maxxpoint_03.png)

    a. Tıklayın **Gelişmiş URL ayarlarını göster** seçeneği

    b. İçinde **işareti bulunan URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://maxxpoint.westipc.com/default/sso/login/entity/<customer-id>-azure`

    > [!NOTE] 
    > Bu gerçek değer olmadığını unutmayın. Bu değer gerçek işareti bulunan URL'si ile güncelleştirmeniz gerekiyor. MaxxPoint takım çağırmak **888-728-0950** bu değeri alınamıyor.

1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/maxxpoint-tutorial/tutorial_maxxpoint_06.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/maxxpoint-tutorial/tutorial_general_400.png)

1. Uygulamanız için yapılandırılmış SSO almak için MaxxPoint Destek ekibine çağırmak **888-728-0950** ve size daha fazla indirilen sağlayacağını nasıl yardımcı **meta veri XML** dosya. 

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/maxxpoint-tutorial/create_aaduser_01.png) 

1. Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar** kullanıcılar listesini görüntüleyin.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/maxxpoint-tutorial/create_aaduser_02.png) 

1. İletişim kutusunun en üstünde tıklayın **Ekle** açmak için **kullanıcı** iletişim.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/maxxpoint-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/maxxpoint-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın. 

### <a name="creating-a-maxxpoint-test-user"></a>MaxxPoint test kullanıcısı oluşturma

Bu bölümde, Britta Simon MaxxPoint içinde adlı bir kullanıcı oluşturun. MaxxPoint Destek ekibine Lütfen çağırmak **888-728-0950** MaxxPoint uygulamada kullanıcıları eklemek için.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için MaxxPoint erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon MaxxPoint için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **MaxxPoint**.

    ![Çoklu oturum açmayı yapılandırın](./media/maxxpoint-tutorial/tutorial_maxxpoint_50.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde MaxxPoint kutucuğa tıkladığınızda, otomatik olarak MaxxPoint uygulamanıza açan.


## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/maxxpoint-tutorial/tutorial_general_01.png
[2]: ./media/maxxpoint-tutorial/tutorial_general_02.png
[3]: ./media/maxxpoint-tutorial/tutorial_general_03.png
[4]: ./media/maxxpoint-tutorial/tutorial_general_04.png

[100]: ./media/maxxpoint-tutorial/tutorial_general_100.png

[200]: ./media/maxxpoint-tutorial/tutorial_general_200.png
[201]: ./media/maxxpoint-tutorial/tutorial_general_201.png
[202]: ./media/maxxpoint-tutorial/tutorial_general_202.png
[203]: ./media/maxxpoint-tutorial/tutorial_general_203.png