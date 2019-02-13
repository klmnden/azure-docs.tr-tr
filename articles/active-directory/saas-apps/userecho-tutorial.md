---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile UserEcho | Microsoft Docs'
description: Azure Active Directory ve UserEcho arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: bedd916b-8f69-4b50-9b8d-56f4ee3bd3ed
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9f7c02fb395464c1b682a3cfb71cc072a807ef88
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56203259"
---
# <a name="tutorial-azure-active-directory-integration-with-userecho"></a>Öğretici: UserEcho ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile UserEcho tümleştirme konusunda bilgi edinin.

Azure AD ile UserEcho tümleştirme ile aşağıdaki avantajları sağlar:

- UserEcho erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için UserEcho (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile UserEcho yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik UserEcho çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme burada alabilirsiniz: [Deneme teklifi](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden UserEcho ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-userecho-from-the-gallery"></a>Galeriden UserEcho ekleme
Azure AD'de UserEcho tümleştirmesini yapılandırmak için UserEcho Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden UserEcho eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **UserEcho**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/userecho-tutorial/tutorial_userecho_search.png)

1. Sonuçlar panelinde seçin **UserEcho**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/userecho-tutorial/tutorial_userecho_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı UserEcho sınayın.

Tek iş için oturum açma için Azure AD ne UserEcho karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının UserEcho ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

UserEcho içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma UserEcho ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[UserEcho test kullanıcısı oluşturma](#creating-a-userecho-test-user)**  - kullanıcı Azure AD gösterimini bağlı UserEcho Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve UserEcho uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile UserEcho yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **UserEcho** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/userecho-tutorial/tutorial_userecho_samlbase.png)

1. Üzerinde **UserEcho etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/userecho-tutorial/tutorial_userecho_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<companyname>.userecho.com/`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<companyname>.userecho.com/saml/metadata/`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [UserEcho istemci Destek ekibine](https://feedback.userecho.com/) bu değerleri almak için. 

1. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/userecho-tutorial/tutorial_userecho_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/userecho-tutorial/tutorial_general_400.png)

1. Üzerinde **UserEcho yapılandırma** bölümünde **yapılandırma UserEcho** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/userecho-tutorial/tutorial_userecho_configure.png) 

1. Başka bir tarayıcı penceresinde UserEcho şirketinizin sitesi için bir yönetici olarak oturum açın.

1. Üst araç çubuğunda, menüyü genişletmek için kullanıcı adınıza tıklayın ve ardından **Kurulum**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/userecho-tutorial/tutorial_userecho_06.png) 

1. Tıklayın **tümleştirmeler**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/userecho-tutorial/tutorial_userecho_07.png) 

1. Tıklayın **Web sitesi**ve ardından **çoklu oturum açma (SAML2)**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/userecho-tutorial/tutorial_userecho_08.png) 

1. Üzerinde **çoklu oturum açma (SAML)** sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açmayı yapılandırın](./media/userecho-tutorial/tutorial_userecho_09.png)
    
    a. Olarak **SAML etkin**seçin **Evet**.
    
    b. Yapıştırma **SAML çoklu oturum açma hizmeti URL'si**, hangi Azure portaldan kopyaladığınız **SAML SSO URL** metin.
    
    c. Yapıştırma **oturum kapatma URL'si**, hangi Azure portaldan kopyaladığınız **uzak logoout URL** metin.
    
    d. İndirilen sertifikanızı Not Defteri'nde açın, içeriği kopyalayın ve ardından yapıştırın **X.509 sertifikası** metin.
    
    e. **Kaydet**’e tıklayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/userecho-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/userecho-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/userecho-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/userecho-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-userecho-test-user"></a>UserEcho test kullanıcısı oluşturma

Bu bölümün amacı UserEcho Britta Simon adlı bir kullanıcı oluşturmaktır.

**Britta Simon UserEcho içinde adlı bir kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. UserEcho şirketinizin sitesi için bir yönetici olarak oturum.

1. Üst araç çubuğunda, menüyü genişletmek için kullanıcı adınıza tıklayın ve ardından **Kurulum**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/userecho-tutorial/tutorial_userecho_06.png)

1. Tıklayın **kullanıcılar**genişletmek için **kullanıcılar** bölümü.
   
    ![Çoklu oturum açmayı yapılandırın](./media/userecho-tutorial/tutorial_userecho_10.png)

1. **Kullanıcılar**’a tıklayın.
   
    ![Çoklu oturum açmayı yapılandırın](./media/userecho-tutorial/tutorial_userecho_11.png)

1. Tıklayın **yeni bir kullanıcı davet**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/userecho-tutorial/tutorial_userecho_12.png)

1. Üzerinde **yeni bir kullanıcı davet** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açmayı yapılandırın](./media/userecho-tutorial/tutorial_userecho_13.png)

    a. İçinde **adı** metin kutusu, kullanıcının Britta Simon gibi tür adı.
    
    b.  İçinde **e-posta** metin kutusuna kullanıcı e-posta adresi türünü ister Brittasimon@contoso.com.
    
    c. Tıklayın **davet**.

Her UserEcho kullanmaya başlamak sağlayan Britta bir davet gönderilir. 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için UserEcho erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon UserEcho için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **UserEcho**.

    ![Çoklu oturum açmayı yapılandırın](./media/userecho-tutorial/tutorial_userecho_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümün amacı, erişim panelini kullanarak Azure AD SSO yapılandırmanızı sınamanızı sağlamaktır.  

Erişim panelinde UserEcho kutucuğa tıkladığınızda, otomatik olarak UserEcho uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/userecho-tutorial/tutorial_general_01.png
[2]: ./media/userecho-tutorial/tutorial_general_02.png
[3]: ./media/userecho-tutorial/tutorial_general_03.png
[4]: ./media/userecho-tutorial/tutorial_general_04.png

[100]: ./media/userecho-tutorial/tutorial_general_100.png

[200]: ./media/userecho-tutorial/tutorial_general_200.png
[201]: ./media/userecho-tutorial/tutorial_general_201.png
[202]: ./media/userecho-tutorial/tutorial_general_202.png
[203]: ./media/userecho-tutorial/tutorial_general_203.png

