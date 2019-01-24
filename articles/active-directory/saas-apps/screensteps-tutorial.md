---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile ScreenSteps | Microsoft Docs'
description: Azure Active Directory ve ScreenSteps arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 4563fe94-a88f-4895-a07f-79df44889cf9
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: jeedes
ms.openlocfilehash: 50e59c9ab04c1f17d55461b0562491143c21e51d
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54815914"
---
# <a name="tutorial-azure-active-directory-integration-with-screensteps"></a>Öğretici: ScreenSteps ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile ScreenSteps tümleştirme konusunda bilgi edinin.

Azure AD ile ScreenSteps tümleştirme ile aşağıdaki avantajları sağlar:

- ScreenSteps erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan için ScreenSteps (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile ScreenSteps yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik ScreenSteps çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden ScreenSteps ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-screensteps-from-the-gallery"></a>Galeriden ScreenSteps ekleme
Azure AD'de ScreenSteps tümleştirmesini yapılandırmak için ScreenSteps Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden ScreenSteps eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **ScreenSteps**seçin **ScreenSteps** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde ScreenSteps](./media/screensteps-tutorial/tutorial_screensteps_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı ScreenSteps sınayın.

Tek iş için oturum açma için Azure AD ne ScreenSteps karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının ScreenSteps ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

ScreenSteps içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma ScreenSteps ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[ScreenSteps test kullanıcısı oluşturma](#create-a-screensteps-test-user)**  - kullanıcı Azure AD gösterimini bağlı ScreenSteps Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve ScreenSteps uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile ScreenSteps yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **ScreenSteps** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/screensteps-tutorial/tutorial_screensteps_samlbase.png)

1. Üzerinde **ScreenSteps etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![ScreenSteps etki alanı ve URL'ler tek oturum açma bilgileri](./media/screensteps-tutorial/tutorial_screensteps_url.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<tenantname>.ScreenSteps.com`

    > [!NOTE] 
    > Bu değer, gerçek değil. Bu değer, gerçek oturum açma, bu öğreticinin ilerleyen bölümlerinde açıklanan URL ile güncelleştirin. 

1. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/screensteps-tutorial/tutorial_screensteps_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/screensteps-tutorial/tutorial_general_400.png)

1. Üzerinde **ScreenSteps yapılandırma** bölümünde **yapılandırma ScreenSteps** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![ScreenSteps yapılandırma](./media/screensteps-tutorial/tutorial_screensteps_configure.png) 

1. Farklı bir web tarayıcı penceresinde ScreenSteps şirket sitenize yönetici olarak oturum.

1. Tıklayın **hesap ayarları**.

    ![Hesap Yönetimi](./media/screensteps-tutorial/ic778523.png "hesap yönetimi")

1. Tıklayın **çoklu oturum açma**.

    ![Uzak kimlik doğrulaması](./media/screensteps-tutorial/ic778524.png "uzak kimlik doğrulaması")

1. Tıklayın **tek oturum açma uç noktası oluşturma**.

    ![Uzak kimlik doğrulaması](./media/screensteps-tutorial/ic778525.png "uzak kimlik doğrulaması")

1. İçinde **oluşturma tek oturum açma uç noktası** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Bir kimlik doğrulama uç noktası oluşturma](./media/screensteps-tutorial/ic778526.png "bir kimlik doğrulama uç noktası oluşturma")
    
    a. İçinde **başlık** metin kutusu, bir başlık yazın.
    
    b. Gelen **modu** listesinden **SAML**.
    
    c. **Oluştur**’a tıklayın.

1. **Düzen** yeni uç nokta.

    ![Uç noktayı Düzenle](./media/screensteps-tutorial/ic778528.png "uç noktasını Düzenle")

1. İçinde **Düzenle tek oturum açma uç noktası** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Uzaktan kimlik doğrulama uç noktası](./media/screensteps-tutorial/ic778527.png "uzaktan kimlik doğrulama uç noktası")

    a. Tıklayın **yeni SAML sertifika dosyası karşıya yükleme**ve sonra Azure portalından indirdiğiniz sertifika karşıya yükleyin.
    
    b. Yapıştırma **SAML çoklu oturum açma hizmeti URL'si** Azure portaldan kopyaladığınız değeri **uzaktan oturum açma URL'si** metin.
    
    c. Yapıştırma **oturum kapatma URL'si** Azure portaldan kopyaladığınız değeri **oturum kapatma URL'si** metin.
    
    d. Seçin bir **grubu** olduğunda bunlar sağlanan için kullanıcılara atamak için.
    
    e. Tıklayın **güncelleştirme**.

    f. Kopyalama **SAML tüketici URL** Pano ve Yapıştır ' **oturum açma URL'si** metin kutusunda **ScreenSteps etki alanı ve URL'ler** bölümü.
    
    g. Geri dönüp **Düzenle tek oturum açma uç noktası**.
    
    h. Tıklayın **hesabı için varsayılan yap** düğmesine ScreenSteps açan tüm kullanıcılar için bu uç noktayı kullanın. Alternatif olarak tıklayabilirsiniz **ekleme siteye** belirli siteler için bu endpoint kullanmak için düğme **ScreenSteps**.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/screensteps-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/screensteps-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/screensteps-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/screensteps-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-screensteps-test-user"></a>ScreenSteps test kullanıcısı oluşturma

Bu bölümde, Britta Simon ScreenSteps içinde adlı bir kullanıcı oluşturun. Çalışmak [ScreenSteps istemci Destek ekibine](https://www.screensteps.com/contact) ScreenSteps platform kullanıcıları eklemek için. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi. 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için ScreenSteps erişim vererek Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon ScreenSteps için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **ScreenSteps**.

    ![Uygulamalar listesinde ScreenSteps bağlantı](./media/screensteps-tutorial/tutorial_screensteps_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde ScreenSteps kutucuğa tıkladığınızda, otomatik olarak ScreenSteps uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/screensteps-tutorial/tutorial_general_01.png
[2]: ./media/screensteps-tutorial/tutorial_general_02.png
[3]: ./media/screensteps-tutorial/tutorial_general_03.png
[4]: ./media/screensteps-tutorial/tutorial_general_04.png

[100]: ./media/screensteps-tutorial/tutorial_general_100.png

[200]: ./media/screensteps-tutorial/tutorial_general_200.png
[201]: ./media/screensteps-tutorial/tutorial_general_201.png
[202]: ./media/screensteps-tutorial/tutorial_general_202.png
[203]: ./media/screensteps-tutorial/tutorial_general_203.png

