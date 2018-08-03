---
title: 'Öğretici: Azure Active Directory Tümleştirme Predictix Sınıflama planlama | Microsoft Docs'
description: Azure Active Directory ve Sınıflama Predictix planlama arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 37e686ff-f8e5-40b1-9d7e-f64b076917b7
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: 1069b7f9bdc0301f840e796f49fdb4031d297cf2
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39436249"
---
# <a name="tutorial-azure-active-directory-integration-with-predictix-assortment-planning"></a>Öğretici: Azure Active Directory Tümleştirme Predictix Sınıflama planlama

Bu öğreticide, Azure Active Directory (Azure AD) ile tümleştirme Predictix Sınıflama planlama konusunda bilgi edinin.

Azure AD ile tümleştirme Predictix Sınıflama planlama ile aşağıdaki avantajları sağlar:

- Predictix Sınıflama planlama erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan Predictix Sınıflama planlama (çoklu oturum açma) açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Predictix Sınıflama planlama ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik Sınıflama Predictix planlama çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Predictix Sınıflama planlama ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-predictix-assortment-planning-from-the-gallery"></a>Galeriden Predictix Sınıflama planlama ekleme
Azure AD'de Sınıflama Predictix planlama tümleştirmesini yapılandırmak için Predictix Sınıflama planlama Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Predictix Sınıflama planlama eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **Sınıflama Predictix planlama**seçin **Sınıflama Predictix planlama** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Predictix Sınıflama planlama](./media/predictix-assortment-planning-tutorial/tutorial_predictixassortmentplanning_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." adlı bir test kullanıcı tabanlı Sınıflama Predictix planlama ile test etme

Tek çalışmak için oturum açma için Azure AD ne Sınıflama Predictix planlama karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve ilgili kullanıcı Sınıflama Predictix planlama arasında bir bağlantı ilişki kurulması gerekir.

Predictix Sınıflama planlama değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Sınıflama Predictix planlama ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Predictix Sınıflama planlama test kullanıcısı oluşturma](#create-a-predictix-assortment-planning-test-user)**  - Predictix Sınıflama kullanıcı Azure AD gösterimini bağlı planlama Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Sınıflama Predictix planlama uygulamanızı yapılandırın.

**Azure AD çoklu oturum açma Predictix Sınıflama planlama ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Sınıflama Predictix planlama** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/predictix-assortment-planning-tutorial/tutorial_predictixassortmentplanning_samlbase.png)

1. Üzerinde **Predictix Sınıflama planlama etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Predictix Sınıflama planlama etki alanı ve URL'ler tek oturum açma bilgileri](./media/predictix-assortment-planning-tutorial/tutorial_predictixassortmentplanning_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak:
    | |
    |--|--|
    | `https://<sub-domain>.ap.predictix.com/sso/request`|
    | `https://<sub-domain>.dev.ap.predictix.com/`|

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak:
    | |
    |--|--|
    | `https://<sub-domain>.ap.predictix.com`|
    | `https://<sub-domain>.dev.ap.predictix.com`|
    
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [Predictix Sınıflama planlama istemci Destek ekibine](http://www.infor.com/support) bu değerleri almak için. 
 


1. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/predictix-assortment-planning-tutorial/tutorial_predictixassortmentplanning_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/predictix-assortment-planning-tutorial/tutorial_general_400.png)

1. Üzerinde **Predictix Sınıflama yapılandırmasını planlama** bölümünde **yapılandırma Predictix Sınıflama planlama** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Predictix Sınıflama yapılandırmasını planlama](./media/predictix-assortment-planning-tutorial/tutorial_predictixassortmentplanning_configure.png) 

1. Çoklu oturum açmayı yapılandırma **Sınıflama Predictix planlama** tarafı, indirilen göndermek için ihtiyacınız **Certificate(Base64)**, **SAML varlık kimliği**, **SAML Çoklu oturum açma hizmeti URL'si**, ve **oturum kapatma URL'si** için [Sınıflama Predictix planlama Destek ekibine](http://www.infor.com/support). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/predictix-assortment-planning-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/predictix-assortment-planning-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/predictix-assortment-planning-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/predictix-assortment-planning-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-predictix-assortment-planning-test-user"></a>Predictix Sınıflama planlama test kullanıcısı oluşturma

Bu bölümde, Britta Simon Sınıflama Predictix Planlanıyor adlı bir kullanıcı oluşturun. Lütfen birlikte çalışarak [Sınıflama Predictix planlama Destek ekibine](http://www.infor.com/contact/) Sınıflama Predictix planlama platform kullanıcıları eklemek için.
 > [!NOTE]
 > Azure Active Directory hesap sahibinin e-posta alır ve etkin hale gelir önce hesabını onaylamak için bir bağlantı izler.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma Predictix Sınıflama planlama için erişim izni verme kullanmak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Sınıflama Predictix planlama atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Sınıflama Predictix planlama**.

    ![Uygulamalar listesinde Predictix Sınıflama planlama bağlantı](./media/predictix-assortment-planning-tutorial/tutorial_predictixassortmentplanning_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Predictix Sınıflama planlama kutucuğa tıkladığınızda, otomatik olarak Sınıflama Predictix planlama uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/predictix-assortment-planning-tutorial/tutorial_general_01.png
[2]: ./media/predictix-assortment-planning-tutorial/tutorial_general_02.png
[3]: ./media/predictix-assortment-planning-tutorial/tutorial_general_03.png
[4]: ./media/predictix-assortment-planning-tutorial/tutorial_general_04.png

[100]: ./media/predictix-assortment-planning-tutorial/tutorial_general_100.png

[200]: ./media/predictix-assortment-planning-tutorial/tutorial_general_200.png
[201]: ./media/predictix-assortment-planning-tutorial/tutorial_general_201.png
[202]: ./media/predictix-assortment-planning-tutorial/tutorial_general_202.png
[203]: ./media/predictix-assortment-planning-tutorial/tutorial_general_203.png

