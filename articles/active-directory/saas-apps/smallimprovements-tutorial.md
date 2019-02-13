---
title: 'Öğretici: Küçük geliştirme ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve küçük geliştirmeler arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 59c8a112-41e1-4337-9ef3-3d7029780d61
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 39e0ec3a69997541c926c5da58ed9d24df5498c9
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56165544"
---
# <a name="tutorial-azure-active-directory-integration-with-small-improvements"></a>Öğretici: Küçük geliştirme ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile küçük iyileştirmeler tümleştirme konusunda bilgi edinin.

Küçük geliştirme Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Küçük iyileştirmeler erişimi, Azure AD'de denetleyebilirsiniz
- Azure AD hesaplarına otomatik olarak küçük iyileştirmeleri (çoklu oturum açma) açan, kullanıcılarınızın etkinleştirebilirsiniz
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile küçük iyileştirmeler yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Küçük iyileştirmeler çoklu oturum açma abonelik etkin.

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme burada alabilirsiniz [deneme teklifi](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden küçük geliştirmeler ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-small-improvements-from-the-gallery"></a>Galeriden küçük geliştirmeler ekleme
Azure AD'ye küçük iyileştirmeler tümleştirmesini yapılandırmak için küçük geliştirme Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden küçük iyileştirmeler eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **küçük iyileştirmeler**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/smallimprovements-tutorial/tutorial_smallimprovements_search.png)

1. Sonuçlar panelinde seçin **küçük iyileştirmeler**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/smallimprovements-tutorial/tutorial_smallimprovements_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcıya bağlı küçük geliştirme ile test.

Tek çalışmak için oturum açma için Azure AD ne küçük iyileştirmeler karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile ilgili kullanıcı küçük geliştirmeleri arasında bir bağlantı ilişki kurulması gerekir.

Küçük yenilikleri değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma küçük geliştirme ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Bir küçük test kullanıcısı oluşturma](#creating-a-small-improvements-test-user)**  - kullanıcı Azure AD gösterimini bağlı küçük geliştirmeleri Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve küçük iyileştirmeler uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile küçük iyileştirmeler yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **küçük iyileştirmeler** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/smallimprovements-tutorial/tutorial_smallimprovements_samlbase.png)

1. Üzerinde **küçük geliştirmeler etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/smallimprovements-tutorial/tutorial_smallimprovements_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<subdomain>.small-improvements.com`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<subdomain>.small-improvements.com`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [küçük iyileştirmeler istemci Destek ekibine](mailto:support@small-improvements.com) bu değerleri almak için. 
 
1. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/smallimprovements-tutorial/tutorial_smallimprovements_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/smallimprovements-tutorial/tutorial_general_400.png)

1. Üzerinde **küçük iyileştirmeler yapılandırma** bölümünde **yapılandırma küçük iyileştirmeler** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/smallimprovements-tutorial/tutorial_smallimprovements_configure.png) 

1. Başka bir tarayıcı penceresinde küçük iyileştirmeler şirketinizin sitesi için yönetici olarak oturum açın.

1. Pano ana sayfasında **Yönetim** soldaki düğme.
   
    ![Çoklu oturum açmayı yapılandırın](./media/smallimprovements-tutorial/tutorial_smallimprovements_06.png) 

1. Tıklayın **SAML SSO** düğmesini **tümleştirmeler** bölümü.
   
    ![Çoklu oturum açmayı yapılandırın](./media/smallimprovements-tutorial/tutorial_smallimprovements_07.png) 

1. SSO Kurulumu sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açmayı yapılandırın](./media/smallimprovements-tutorial/tutorial_smallimprovements_08.png)  

    a. İçinde **HTTP uç noktası** metin değerini yapıştırın **SAML çoklu oturum açma hizmeti URL'si**, hangi Azure Portalı'ndan kopyaladığınız.

    b. İndirilen sertifikanızı Not Defteri'nde açın, içeriği kopyalayın ve ardından yapıştırın **x509 sertifika** metin. 

    c. SSO ve oturum açma form kimlik doğrulaması seçeneği kullanıcılar için kullanılabilir olmasını istiyorsanız, daha sonra kontrol **oturum açma/parola ile çok erişmesini** seçeneği.  

    d. SSO oturum açma düğmesi olarak adlandırmak için uygun değeri girin **SAML istemi** metin.  

    e. **Kaydet**’e tıklayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/smallimprovements-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/smallimprovements-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/smallimprovements-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/smallimprovements-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-small-improvements-test-user"></a>Bir küçük test kullanıcısı oluşturma

Küçük iyileştirmeler oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunlar küçük artışlarını sağlanması gerekir. Küçük iyileştirmeler söz konusu olduğunda sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Küçük iyileştirmeler şirketinizin sitesi için bir yönetici olarak oturum.

1. Giriş sayfasından sol tıklayın menüsüne gidin **Yönetim**.

1. Tıklayın **kullanıcı dizini** kullanıcı yönetim bölümünden düğmesi. 
   
    ![Bir Azure AD test kullanıcısı oluşturma](./media/smallimprovements-tutorial/tutorial_smallimprovements_10.png) 

1. Tıklayın **kullanıcı ekleme**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/smallimprovements-tutorial/tutorial_smallimprovements_11.png) 

1. Üzerinde **Add Users** iletişim kutusunda, aşağıdaki adımları gerçekleştirin: 

    ![Bir Azure AD test kullanıcısı oluşturma](./media/smallimprovements-tutorial/tutorial_smallimprovements_12.png)
    
    a. Girin **ad** gibi kullanıcının **Britta**.

    b. Girin **Soyadı** gibi kullanıcının **Simon**.

    c. Girin **e-posta** gibi kullanıcının <strong>brittasimon@contoso.com</strong>. 

    d. Ayrıca, kişisel bir ileti girin seçebilirsiniz **bildirim e-posta Gönder** kutusu. Sonra bildirim göndermesini istemiyorsanız, bu onay kutusunun işaretini kaldırın.

    e. Tıklayın **kullanıcılar oluşturma**.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, küçük iyileştirmeler erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon küçük iyileştirmeler atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **küçük iyileştirmeler**.

    ![Çoklu oturum açmayı yapılandırın](./media/smallimprovements-tutorial/tutorial_smallimprovements_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümün amacı, erişim panelini kullanarak Azure AD SSO yapılandırmanızı sınamanızı sağlamaktır.  

Erişim panelinde küçük iyileştirmeler kutucuğa tıkladığınızda, otomatik olarak küçük iyileştirmeler uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/smallimprovements-tutorial/tutorial_general_01.png
[2]: ./media/smallimprovements-tutorial/tutorial_general_02.png
[3]: ./media/smallimprovements-tutorial/tutorial_general_03.png
[4]: ./media/smallimprovements-tutorial/tutorial_general_04.png

[100]: ./media/smallimprovements-tutorial/tutorial_general_100.png

[200]: ./media/smallimprovements-tutorial/tutorial_general_200.png
[201]: ./media/smallimprovements-tutorial/tutorial_general_201.png
[202]: ./media/smallimprovements-tutorial/tutorial_general_202.png
[203]: ./media/smallimprovements-tutorial/tutorial_general_203.png

