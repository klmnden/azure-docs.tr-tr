---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile RunMyProcess | Microsoft Docs'
description: Azure Active Directory ve RunMyProcess arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: d31f7395-048b-4a61-9505-5acf9fc68d9b
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: dfef1371b7ac61712c0f70efd48c0e791c4c729d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60518266"
---
# <a name="tutorial-azure-active-directory-integration-with-runmyprocess"></a>Öğretici: RunMyProcess ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile RunMyProcess tümleştirme konusunda bilgi edinin.

Azure AD ile RunMyProcess tümleştirme ile aşağıdaki avantajları sağlar:

- RunMyProcess erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için RunMyProcess (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile RunMyProcess yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik RunMyProcess çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme burada alabilirsiniz:[deneme teklifi](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden RunMyProcess ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-runmyprocess-from-the-gallery"></a>Galeriden RunMyProcess ekleme
Azure AD'de RunMyProcess tümleştirmesini yapılandırmak için RunMyProcess Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden RunMyProcess eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **RunMyProcess**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/runmyprocess-tutorial/tutorial_runmyprocess_search.png)

1. Sonuçlar panelinde seçin **RunMyProcess**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/runmyprocess-tutorial/tutorial_runmyprocess_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı RunMyProcess sınayın.

Tek iş için oturum açma için Azure AD ne RunMyProcess karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının RunMyProcess ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

RunMyProcess içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma RunMyProcess ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[RunMyProcess test kullanıcısı oluşturma](#creating-a-runmyprocess-test-user)**  - kullanıcı Azure AD gösterimini bağlı RunMyProcess Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve RunMyProcess uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile RunMyProcess yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **RunMyProcess** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/runmyprocess-tutorial/tutorial_runmyprocess_samlbase.png)

1. Üzerinde **RunMyProcess etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/runmyprocess-tutorial/tutorial_runmyprocess_url.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://live.runmyprocess.com/live/<tenant id>`

    > [!NOTE] 
    > Değer, gerçek değil. Değerini gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [RunMyProcess istemci Destek ekibine](mailto:support@runmyprocess.com) değeri alınamıyor. 

1. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/runmyprocess-tutorial/tutorial_runmyprocess_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/runmyprocess-tutorial/tutorial_general_400.png)

1. Üzerinde **RunMyProcess yapılandırma** bölümünde **yapılandırma RunMyProcess** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/runmyprocess-tutorial/tutorial_runmyprocess_configure.png) 

1. Farklı bir web tarayıcı penceresinde RunMyProcess kiracınıza yönetici olarak oturum.

1. Sol gezinti panelinde **hesabı** seçip **yapılandırma**.
   
    ![Çoklu oturum açma uygulama tarafında yapılandırma](./media/runmyprocess-tutorial/tutorial_runmyprocess_001.png)

1. Git **kimlik doğrulama yöntemi** bölümünde ve aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açma uygulama tarafında yapılandırma](./media/runmyprocess-tutorial/tutorial_runmyprocess_002.png)

    a. Olarak **yöntemi**seçin **Samlv2 ile SSO**. 

    b. İçinde **SSO yeniden yönlendirme** metin değerini yapıştırın **SAML çoklu oturum açma hizmeti URL'si**, hangi Azure Portalı'ndan kopyaladığınız.

    c. İçinde **oturumu kapatıp yeniden yönlendirme** metin değerini yapıştırın **oturum kapatma URL'si**, hangi Azure Portalı'ndan kopyaladığınız.

    d. İçinde **ad kimliği biçimi** metin değerini yazın. **ad tanımlayıcı biçimi** olarak **urn: OASIS: adları: tc: SAML:1.1:nameid-biçimi: emailAddress**.

    e. İndirilen sertifika dosyasının içeriğini kopyalayın ve ardından yapıştırın **sertifika** metin. 
 
    f. Tıklayın **Kaydet** simgesi.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/runmyprocess-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/runmyprocess-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/runmyprocess-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/runmyprocess-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-runmyprocess-test-user"></a>RunMyProcess test kullanıcısı oluşturma

RunMyProcess için oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunların RunMyProcess sağlanması gerekir. RunMyProcess söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. RunMyProcess şirketinizin sitesi için bir yönetici olarak oturum açın.

1. Tıklayın **hesabı** seçip **kullanıcılar** sol gezinti panelinde, ardından **yeni kullanıcı**.
   
    ![Yeni kullanıcı](./media/runmyprocess-tutorial/tutorial_runmyprocess_003.png "yeni kullanıcı")

1. İçinde **kullanıcı ayarları** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Profili](./media/runmyprocess-tutorial/tutorial_runmyprocess_004.png "profili") 
  
    a. Tür **adı** ve **e-posta** geçerli bir Azure AD hesabı ilgili metin kutularına zbilgisayarlar istediğiniz. 

    b. Seçin bir **IDE dil**, **dil**, ve **profili**. 

    c. Seçin **hesap oluşturma e-posta Gönder bana**. 

    d. **Kaydet**’e tıklayın.
   
    >[!NOTE]
    >Herhangi diğer RunMyProcess kullanıcı hesabı oluşturma araçları kullanabilir veya API'leri tarafından RunMyProcess sağlamak için Azure Active Directory kullanıcı hesaplarını sağlanan. 
    > 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için RunMyProcess erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon RunMyProcess için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **RunMyProcess**.

    ![Çoklu oturum açmayı yapılandırın](./media/runmyprocess-tutorial/tutorial_runmyprocess_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümün amacı, erişim panelini kullanarak Azure AD SSO yapılandırmanızı sınamanızı sağlamaktır.

Erişim panelinde RunMyProcess kutucuğa tıkladığınızda, otomatik olarak RunMyProcess uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/runmyprocess-tutorial/tutorial_general_01.png
[2]: ./media/runmyprocess-tutorial/tutorial_general_02.png
[3]: ./media/runmyprocess-tutorial/tutorial_general_03.png
[4]: ./media/runmyprocess-tutorial/tutorial_general_04.png

[100]: ./media/runmyprocess-tutorial/tutorial_general_100.png

[200]: ./media/runmyprocess-tutorial/tutorial_general_200.png
[201]: ./media/runmyprocess-tutorial/tutorial_general_201.png
[202]: ./media/runmyprocess-tutorial/tutorial_general_202.png
[203]: ./media/runmyprocess-tutorial/tutorial_general_203.png

