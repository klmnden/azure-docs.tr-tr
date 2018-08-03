---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle tuval Lms | Microsoft Docs'
description: Azure Active Directory ve tuval LMS arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: bfed291c-a33e-410d-b919-5b965a631d45
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/08/2017
ms.author: jeedes
ms.openlocfilehash: af2c997f0842da751eb93f0788a7402fc7d144ae
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39433298"
---
# <a name="tutorial-azure-active-directory-integration-with-canvas-lms"></a>Öğretici: Azure Active Directory tuval LMS ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile tuval tümleştirme konusunda bilgi edinin.

Tuval Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Tuval erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak tuvale (çoklu oturum açma) ile Azure AD hesaplarına açan, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Tuval ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Bir tuval çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden tuval ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-canvas-from-the-gallery"></a>Galeriden tuval ekleme
Azure AD'de tuvalinin tümleştirmesini yapılandırmak için tuval Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden tuval eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **tuval**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/canvas-lms-tutorial/tutorial_canvaslms_search.png)

1. Sonuçlar panelinde seçin **tuval**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/canvas-lms-tutorial/tutorial_canvaslms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma tuval tabanlı "Britta Simon." adlı bir test kullanıcı ile test etme

Tek iş için oturum açma için Azure AD karşılık gelen kullanıcı tuvalinde bir kullanıcının Azure AD'de nedir bilmesi gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile ilgili kullanıcının tuval arasında bir bağlantı ilişki kurulması gerekir.

Tuvalde, değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma tuval ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Bir tuval test kullanıcısı oluşturma](#creating-a-canvas-test-user)**  - kullanıcı Azure AD gösterimini bağlı tuvalinde Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve tuval uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma tuval ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **tuval** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/canvas-lms-tutorial/tutorial_canvaslms_samlbase.png)

1. Üzerinde **tuval etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/canvas-lms-tutorial/tutorial_canvaslms_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<tenant-name>.instructure.com`

    b. İçinde **tanımlayıcı** metin kutusuna şu biçimi kullanarak değeri yazın: `https://<tenant-name>.instructure.com/saml2`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [tuval istemci Destek ekibine](https://community.canvaslms.com/community/help) bu değerleri almak için. 
 
1. Üzerinde **SAML imzalama sertifikası** bölümünde, kopya **parmak İZİ** sertifika değeri.

    ![Çoklu oturum açmayı yapılandırın](./media/canvas-lms-tutorial/tutorial_canvaslms_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/canvas-lms-tutorial/tutorial_general_400.png)

1. Üzerinde **tuval yapılandırma** bölümünde **yapılandırma tuval** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **parola URL'yi Değiştir, oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/canvas-lms-tutorial/tutorial_canvaslms_configure.png) 
 
1. Farklı bir web tarayıcı penceresinde bir tuval şirketinizin sitesi için bir yönetici olarak oturum açın.

1. Git **kursları \> yönetilen hesapları \> Microsoft**.
   
    ![Tuval](./media/canvas-lms-tutorial/IC775990.png "tuvali")

1. Sol taraftaki gezinti bölmesinde seçin **kimlik doğrulaması**ve ardından **yeni SAML yapılandırma Ekle**.
   
    ![Kimlik doğrulaması](./media/canvas-lms-tutorial/IC775991.png "kimlik doğrulaması")

1. Geçerli tümleştirme sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Geçerli tümleştirme](./media/canvas-lms-tutorial/IC775992.png "geçerli tümleştirme")

    a. İçinde **IDP varlık kimliği** metin değerini yapıştırın **SAML varlık kimliği** , Azure Portalı'ndan kopyaladığınız.

    b. İçinde **oturum üzerinde URL'sini** metin değerini yapıştırın **SAML çoklu oturum açma hizmeti URL'si** , Azure Portalı'ndan kopyaladığınız.

    c. İçinde **oturum kapatma URL'sini** metin değerini yapıştırın **oturum kapatma URL'si** , Azure Portalı'ndan kopyaladığınız.

    d. İçinde **Değiştir parola bağlantısını** metin değerini yapıştırın **parola URL'yi Değiştir** , Azure Portalı'ndan kopyaladığınız. 

    e. İçinde **sertifika parmak izi** metin kutusu, yapıştırma **parmak izi** Azure Portalı'ndan kopyaladığınız sertifika değeri.      
        
    f. Gelen **oturum açma özniteliği** listesinden **Nameıd**.

    g. Gelen **tanımlayıcı biçimi** listesinden **emailAddress**.

    h. Tıklayın **kimlik doğrulama ayarlarını kaydetme**.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/canvas-lms-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/canvas-lms-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/canvas-lms-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/canvas-lms-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-canvas-test-user"></a>Bir tuval test kullanıcısı oluşturma

Tuvale oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunlar tuvale sağlanması gerekir.

Durumunda, kullanıcı sağlamayı elle bir görevin tuvaldir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açın, **tuval** Kiracı.

1. Git **kursları \> yönetilen hesapları \> Microsoft**.
   
   ![Tuval](./media/canvas-lms-tutorial/IC775990.png "tuvali")

1. **Kullanıcılar**’a tıklayın.
   
   ![Kullanıcılar](./media/canvas-lms-tutorial/IC775995.png "kullanıcılar")

1. Tıklayın **yeni kullanıcı ekleme**.
   
   ![Kullanıcılar](./media/canvas-lms-tutorial/IC775996.png "kullanıcılar")

1. Yeni kullanıcı iletişim Sayfa Ekle üzerinde aşağıdaki adımları gerçekleştirin:
   
   ![Kullanıcı ekleme](./media/canvas-lms-tutorial/IC775997.png "kullanıcı ekleme")
   
   a. İçinde **tam adı** metin gibi kullanıcı adını girin **BrittaSimon**.

   b. İçinde **e-posta** metin gibi kullanıcının e-posta girin **brittasimon@contoso.com**.

   c. İçinde **oturum açma** metin kutusu, kullanıcının Azure AD e-posta adresi gibi girin **brittasimon@contoso.com**.

   d. Seçin **bu hesap oluşturma hakkında kullanıcıya e-posta**.

   e. Tıklayın **kullanıcı ekleme**.

>[!NOTE]
>Herhangi diğer tuval kullanıcı hesabı oluşturma araçları kullanabilir veya API'leri için AAD kullanıcı hesapları sağlamak tuval tarafından sağlanan.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma tuvale erişim vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon tuvale atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **tuval**.

    ![Çoklu oturum açmayı yapılandırın](./media/canvas-lms-tutorial/tutorial_canvaslms_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde tuval kutucuğa tıkladığınızda, otomatik olarak tuvale uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/canvas-lms-tutorial/tutorial_general_01.png
[2]: ./media/canvas-lms-tutorial/tutorial_general_02.png
[3]: ./media/canvas-lms-tutorial/tutorial_general_03.png
[4]: ./media/canvas-lms-tutorial/tutorial_general_04.png

[100]: ./media/canvas-lms-tutorial/tutorial_general_100.png

[200]: ./media/canvas-lms-tutorial/tutorial_general_200.png
[201]: ./media/canvas-lms-tutorial/tutorial_general_201.png
[202]: ./media/canvas-lms-tutorial/tutorial_general_202.png
[203]: ./media/canvas-lms-tutorial/tutorial_general_203.png

