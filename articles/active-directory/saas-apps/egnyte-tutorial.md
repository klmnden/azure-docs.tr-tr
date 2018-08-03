---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle Egnyte | Microsoft Docs'
description: Azure Active Directory ve Egnyte arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 8c2101d4-1779-4b36-8464-5c1ff780da18
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: 4f6f6ef12f5a8dd8a9f210e9b1f1ca978ec5a1ac
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39440465"
---
# <a name="tutorial-azure-active-directory-integration-with-egnyte"></a>Öğretici: Azure Active Directory Egnyte ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Egnyte tümleştirme konusunda bilgi edinin.

Azure AD ile Egnyte tümleştirme ile aşağıdaki avantajları sağlar:

- Egnyte erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için Egnyte (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Egnyte yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik bir Egnyte çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Egnyte ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-egnyte-from-the-gallery"></a>Galeriden Egnyte ekleme
Azure AD'de Egnyte tümleştirmesini yapılandırmak için Egnyte Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Egnyte eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **Egnyte**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/egnyte-tutorial/tutorial_egnyte_search.png)

1. Sonuçlar panelinde seçin **Egnyte**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/egnyte-tutorial/tutorial_egnyte_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." adlı bir test kullanıcı tabanlı Egnyte ile test etme

Tek iş için oturum açma için Azure AD ne Egnyte karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Egnyte ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Egnyte içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Egnyte ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Bir Egnyte test kullanıcısı oluşturma](#creating-an-egnyte-test-user)**  - kullanıcı Azure AD gösterimini bağlı Egnyte Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Egnyte uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Egnyte yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Egnyte** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/egnyte-tutorial/tutorial_egnyte_samlbase.png)

1. Üzerinde **Egnyte etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/egnyte-tutorial/tutorial_egnyte_url.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<companyname>.egnyte.com`

    > [!NOTE] 
    > Bu değer, gerçek değil. Bu değer, gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [Egnyte istemci Destek ekibine](https://www.egnyte.com/corp/contact_egnyte.html) bu değeri alınamıyor. 
 
1. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/egnyte-tutorial/tutorial_egnyte_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/egnyte-tutorial/tutorial_general_400.png)

1. Üzerinde **Egnyte yapılandırma** bölümünde **yapılandırma Egnyte** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/egnyte-tutorial/tutorial_egnyte_configure.png) 

1. Farklı bir web tarayıcı penceresinde Egnyte şirketinizin sitesi için bir yönetici olarak oturum açın.

1. Tıklayın **ayarları**.
   
   ![Ayarları](./media/egnyte-tutorial/ic787819.png "ayarları")

1. Menüde **ayarları**.

   ![Ayarları](./media/egnyte-tutorial/ic787820.png "ayarları")

1. Tıklayın **yapılandırma** sekmesine ve ardından **güvenlik**.

    ![Güvenlik](./media/egnyte-tutorial/ic787821.png "güvenlik")

1. İçinde **çoklu oturum açma kimlik doğrulaması** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açma kimlik doğrulaması](./media/egnyte-tutorial/ic787822.png "çoklu oturum açma kimlik doğrulaması")   
    
    a. Olarak **çoklu oturum açma kimlik doğrulaması**seçin **SAML 2.0**.
   
    b. Olarak **kimlik sağlayıcısı**seçin **AzureAD**.
   
    c. Yapıştırma **SAML çoklu oturum açma hizmeti URL'si** Azure portalına kopyalama kaynağı **kimlik sağlayıcısı oturum açma URL'si** metin.
   
    d. Yapıştırma **SAML varlık kimliği** Azure portalından kopyalanan **kimlik sağlayıcısı varlık kimliği** metin.
      
    e. Base-64 kodlanmış sertifikanızı Azure portalından indirdiğiniz Not Defteri'nde açın, içeriğini, panoya kopyalayın ve ardından ona yapıştırın **kimlik sağlayıcısı sertifikası** metin.
   
    f. Olarak **kullanıcı eşleme varsayılan**seçin **e-posta adresi**.
   
    g. Olarak **etki alanına özgü veren değerini kullanmak**seçin **devre dışı**.
   
    h. **Kaydet**’e tıklayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/egnyte-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/egnyte-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/egnyte-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/egnyte-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-an-egnyte-test-user"></a>Bir Egnyte test kullanıcısı oluşturma

Egnyte için oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunların Egnyte sağlanması gerekir. Egnyte söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesapları sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açın, **Egnyte** şirketinizin sitesi yöneticisi olarak.

1. Git **ayarları \> kullanıcıları ve grupları**.

1. Tıklayın **yeni kullanıcı Ekle**ve ardından eklemek istediğiniz kullanıcı türünü seçin.
   
   ![Kullanıcılar](./media/egnyte-tutorial/ic787824.png "kullanıcılar")

1. İçinde **yeni standart kullanıcı** bölümünde, aşağıdaki adımları gerçekleştirin:
   
   ![Yeni standart kullanıcı](./media/egnyte-tutorial/ic787825.png "yeni standart kullanıcı")   

   a. Tür **e-posta**, **kullanıcıadı**ve diğer ayrıntıları sağlamak istediğiniz geçerli bir Azure Active Directory hesabı.
   
   b. **Kaydet**’e tıklayın.
    
    >[!NOTE]
    >Azure Active Directory hesap sahibi bir bildirim e-posta alacaksınız.
    >

>[!NOTE]
>Herhangi diğer Egnyte kullanıcı hesabı oluşturma araçları kullanabilir veya API'leri için AAD kullanıcı hesapları sağlamak Egnyte tarafından sağlanan.
> 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için Egnyte erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Egnyte için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Egnyte**.

    ![Çoklu oturum açmayı yapılandırın](./media/egnyte-tutorial/tutorial_egnyte_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Egnyte kutucuğa tıkladığınızda, otomatik olarak Egnyte uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/egnyte-tutorial/tutorial_general_01.png
[2]: ./media/egnyte-tutorial/tutorial_general_02.png
[3]: ./media/egnyte-tutorial/tutorial_general_03.png
[4]: ./media/egnyte-tutorial/tutorial_general_04.png

[100]: ./media/egnyte-tutorial/tutorial_general_100.png

[200]: ./media/egnyte-tutorial/tutorial_general_200.png
[201]: ./media/egnyte-tutorial/tutorial_general_201.png
[202]: ./media/egnyte-tutorial/tutorial_general_202.png
[203]: ./media/egnyte-tutorial/tutorial_general_203.png

