---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile barındırılan Grafit | Microsoft Docs'
description: Azure Active Directory ve barındırılan Grafit arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: a1ac4d7f-d079-4f3c-b6da-0f520d427ceb
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 8aefa0580e6a9a1e446dd4861a5627ea2a4d36ce
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54811885"
---
# <a name="tutorial-azure-active-directory-integration-with-hosted-graphite"></a>Öğretici: Barındırılan Grafit ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile barındırılan Grafit tümleştirme konusunda bilgi edinin.

Azure AD ile barındırılan Grafit tümleştirme ile aşağıdaki avantajları sağlar:

- Barındırılan Grafit erişimi, Azure AD'de denetleyebilirsiniz
- Azure AD hesaplarına otomatik olarak barındırılan (çoklu oturum açma) için Grafit açan, kullanıcılarınızın etkinleştirebilirsiniz
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile barındırılan Grafit yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik bir barındırılan Grafit çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden barındırılan Grafit ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-hosted-graphite-from-the-gallery"></a>Galeriden barındırılan Grafit ekleme
Azure AD'de barındırılan Grafit tümleştirmesini yapılandırmak için barındırılan Grafit Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden barındırılan Grafit eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **barındırılan Grafit**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_search.png)

1. Sonuçlar panelinde seçin **barındırılan Grafit**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Grafit barındırılan sınayın.

Tek iş için oturum açma için Azure AD ne barındırılan Grafit karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile barındırılan Grafit ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Barındırılan Grafit içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma barındırılan Grafit ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Barındırılan Grafit test kullanıcısı oluşturma](#creating-a-hosted-graphite-test-user)**  - kullanıcı Azure AD gösterimini bağlı barındırılan Grafit Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve barındırılan Grafit uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile barındırılan Grafit yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **barındırılan Grafit** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_samlbase.png)

1. Üzerinde **barındırılan Grafit etki alanı ve URL'ler** uygulamada yapılandırmak isterseniz, bölümü **IDP tarafından başlatılan modu**, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://www.hostedgraphite.com/metadata/<user id>`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://www.hostedgraphite.com/complete/saml/<user id>`

1. Üzerinde **barındırılan Grafit etki alanı ve URL'ler** uygulamada yapılandırmak isterseniz, bölümü **SP tarafından başlatılan modu**, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açmayı yapılandırın](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_10.png)
  
    a. Tıklayarak **Gelişmiş URL ayarlarını göster** seçeneği

    b. İçinde **işareti bulunan URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://www.hostedgraphite.com/login/saml/<user id>/`   

    > [!NOTE] 
    > Bunlar gerçek değerleri olmadığına dikkat edin. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum temel URL ile güncelleştirmeniz gerekiyor. Bu değerleri almak için erişim gidebilirsiniz SAML Kurulumu uygulama tarafından veya kişi -> [barındırılan Grafit Destek ekibine](mailto:help@hostedgraphite.com).
    >
 
1. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/hostedgraphite-tutorial/tutorial_general_400.png)

1. Üzerinde **barındırılan Grafit yapılandırma** bölümünde **barındırılan Grafit yapılandırma** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_configure.png) 

1. Barındırılan Grafit kiracınıza yönetici olarak oturum.

1. Git **SAML Kurulumu sayfasına** Kenar çubuğunda (**erişim -> SAML Kurulumu**).
   
    ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_000.png)

1. Onayı bu URL'leri aynı işlemi yapılandırmanızı **barındırılan Grafit etki alanı ve URL'ler** Azure portal'ın bölümü.
   
    ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_001.png)

1. İçinde **varlık veya sertifikayı verenin kimliği** ve **SSO oturum açma URL'si** metin kutuları, değerini yapıştırın **SAML varlık kimliği** ve **SAML çoklu oturum açma hizmeti URL'si** hangi Azure Portalı'ndan kopyaladığınız. 
   
    ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_002.png)
   

1. Seçin "**salt okunur**" olarak **varsayılan kullanıcı rolü**.
    
    ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_004.png)

1. Base-64 kodlanmış sertifikanızı Azure portalından indirdiğiniz Not Defteri'nde açın, içeriğini, panoya kopyalayın ve ardından ona yapıştırın **X.509 sertifikası** metin.
    
    ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_005.png)

1. Tıklayın **Kaydet** düğmesi.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/hostedgraphite-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/hostedgraphite-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/hostedgraphite-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/hostedgraphite-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-hosted-graphite-test-user"></a>Barındırılan Grafit test kullanıcısı oluşturma

Bu bölümün amacı barındırılan Grafit Britta Simon adlı bir kullanıcı oluşturmaktır. Barındırılan Grafit tam zamanında sağlama, varsayılan olarak etkin olan destekler.

Bu bölümde, hiçbir eylem öğesini yoktur. Yeni bir kullanıcı henüz mevcut değilse barındırılan Grafit erişme denemesi sırasında oluşturulur.

>[!NOTE]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, barındırılan Grafit destek ekibi ile iletişime geçmeniz <mailto:help@hostedgraphite.com>. 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için barındırılan Grafit erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon için barındırılan Grafit atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **barındırılan Grafit**.

    ![Çoklu oturum açmayı yapılandırın](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümün amacı, erişim panelini kullanarak Azure AD SSO yapılandırmanızı sınamanızı sağlamaktır.

Erişim panelinde barındırılan Grafit kutucuğa tıkladığınızda, otomatik olarak barındırılan Grafit uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/hostedgraphite-tutorial/tutorial_general_01.png
[2]: ./media/hostedgraphite-tutorial/tutorial_general_02.png
[3]: ./media/hostedgraphite-tutorial/tutorial_general_03.png
[4]: ./media/hostedgraphite-tutorial/tutorial_general_04.png

[100]: ./media/hostedgraphite-tutorial/tutorial_general_100.png

[200]: ./media/hostedgraphite-tutorial/tutorial_general_200.png
[201]: ./media/hostedgraphite-tutorial/tutorial_general_201.png
[202]: ./media/hostedgraphite-tutorial/tutorial_general_202.png
[203]: ./media/hostedgraphite-tutorial/tutorial_general_203.png

