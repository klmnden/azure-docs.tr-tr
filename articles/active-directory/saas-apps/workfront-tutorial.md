---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Workfront | Microsoft Docs'
description: Azure Active Directory ve Workfront arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: aab8bd2f-f9dd-42da-a18e-d707865687d7
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.openlocfilehash: bdb132deec61ff3d373b4c520a7c35d363818f6d
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55189502"
---
# <a name="tutorial-azure-active-directory-integration-with-workfront"></a>Öğretici: Workfront ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Workfront tümleştirme konusunda bilgi edinin.

Azure AD ile Workfront tümleştirme ile aşağıdaki avantajları sağlar:

- Workfront erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için Workfront (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Workfront yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir Workfront çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Workfront ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-workfront-from-the-gallery"></a>Galeriden Workfront ekleme
Azure AD'de Workfront tümleştirmesini yapılandırmak için Workfront Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Workfront eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **Workfront**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/workfront-tutorial/tutorial_workfront_search.png)

1. Sonuçlar panelinde seçin **Workfront**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/workfront-tutorial/tutorial_workfront_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." adlı bir test kullanıcı tabanlı Workfront ile test etme

Tek iş için oturum açma için Azure AD ne Workfront karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Workfront ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Workfront içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Workfront ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Workfront test kullanıcısı oluşturma](#creating-a-workfront-test-user)**  - kullanıcı Azure AD gösterimini bağlı Workfront Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Workfront uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Workfront yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Workfront** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/workfront-tutorial/tutorial_workfront_samlbase.png)

1. Üzerinde **Workfront etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/workfront-tutorial/tutorial_workfront_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<companyname>.attask-ondemand.com`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<companyname>.attasksandbox.com/SAML2`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [Workfront istemci Destek ekibine](https://www.workfront.com/services-and-support) bu değerleri almak için. 
 
1. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/workfront-tutorial/tutorial_workfront_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/workfront-tutorial/tutorial_general_400.png)

1. Üzerinde **Workfront yapılandırma** bölümünde **yapılandırma Workfront** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/workfront-tutorial/tutorial_workfront_configure.png) 

1. Workfront şirketinizin sitesi için yönetici olarak oturum.

1. Git **çoklu oturum açma yapılandırması**.

1. Üzerinde **çoklu oturum açma** iletişim kutusunda, aşağıdaki adımları gerçekleştirin
    
    ![Çoklu oturum açmayı yapılandırın][23]
   
    a. Olarak **türü**seçin **SAML 2.0**.
   
    b. Seçin **hizmet sağlayıcısı kimliği**.
   
    c. Yapıştırma **SAML çoklu oturum açma hizmeti URL'si** içine **oturum açma portalı URL'si** metin.
   
    d. Yapıştırma **çoklu oturum kapatma hizmeti URL'si** içine **oturum kapatma URL'si** metin.
   
    e. Yapıştırma **parola URL'yi Değiştir** içine **parola URL'yi Değiştir** metin.
   
    f. **Kaydet**’e tıklayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/workfront-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/workfront-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/workfront-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/workfront-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-workfront-test-user"></a>Workfront test kullanıcısı oluşturma

Bu bölümün amacı Workfront Britta Simon adlı bir kullanıcı oluşturmaktır.

**Britta Simon Workfront içinde adlı bir kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Workfront şirketinizin sitesi için yönetici olarak oturum açın.
1. Üstteki menüden **kişiler**.
1. Tıklayın **yeni kişi**. 
1. Yeni bir kişiye iletişim kutusunda aşağıdaki adımları gerçekleştirin:
   
    ![Bir Workfront test kullanıcısı oluşturma][21] 
   
    a. İçinde **ad** metin "Britta." yazın
   
    b. İçinde **Soyadı** metin "Simon." yazın
   
    c. İçinde **e-posta adresi** metin kutusuna, Azure Active Directory'de Britta Simon'ın e-posta adresini yazın.
   
    d. Tıklayın **Kişi Ekle**.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için Workfront erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Workfront için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Workfront**.

    ![Çoklu oturum açmayı yapılandırın](./media/workfront-tutorial/tutorial_workfront_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Workfront kutucuğa tıkladığınızda, oturum açma sayfası Workfront uygulamanın almanız gerekir.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/workfront-tutorial/tutorial_general_01.png
[2]: ./media/workfront-tutorial/tutorial_general_02.png
[3]: ./media/workfront-tutorial/tutorial_general_03.png
[4]: ./media/workfront-tutorial/tutorial_general_04.png
[21]:./media/workfront-tutorial/tutorial_attask_08.png
[23]:./media/workfront-tutorial/tutorial_attask_06.png
[100]: ./media/workfront-tutorial/tutorial_general_100.png

[200]: ./media/workfront-tutorial/tutorial_general_200.png
[201]: ./media/workfront-tutorial/tutorial_general_201.png
[202]: ./media/workfront-tutorial/tutorial_general_202.png
[203]: ./media/workfront-tutorial/tutorial_general_203.png

