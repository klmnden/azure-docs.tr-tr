---
title: 'Öğretici: Azure Active Directory Tümleştirme ile uygunluk NetBenefits | Microsoft Docs'
description: Azure Active Directory ve uygunluk NetBenefits arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 77dc8a98-c0e7-4129-ab88-28e7643e432a
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/12/2018
ms.author: jeedes
ms.openlocfilehash: d11164fafa3c05c8c61c352f4d6be6607fa52ebb
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39425264"
---
# <a name="tutorial-azure-active-directory-integration-with-fidelity-netbenefits"></a>Öğretici: Azure Active Directory uygunluk NetBenefits ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile uygunluk NetBenefits tümleştirme konusunda bilgi edinin.

Azure AD ile bir doğruluk NetBenefits tümleştirme ile aşağıdaki avantajları sağlar:

- Uygunluk NetBenefits erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan (çoklu oturum açma) için uygunluk NetBenefits açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile uygunluk NetBenefits yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik bir doğruluk NetBenefits çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden uygunluk NetBenefits ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-fidelity-netbenefits-from-the-gallery"></a>Galeriden uygunluk NetBenefits ekleme
Azure AD'de uygunluk NetBenefits tümleştirmesini yapılandırmak için uygunluk NetBenefits Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden uygunluk NetBenefits eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **uygunluk NetBenefits**seçin **uygunluk NetBenefits** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde uygunluk NetBenefits](./media/fidelitynetbenefits-tutorial/tutorial_fidelitynetbenefits_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve uygunluk NetBenefits "Britta Simon" adlı bir test kullanıcı tabanlı Azure AD çoklu oturum açmayı sınayın.

Tek çalışmak için oturum açma için Azure AD ne uygunluk NetBenefits karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve uygunluk NetBenefits ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Uygunluk NetBenefits içinde **kullanıcı** eşleme ile yapılmalıdır **Azure AD kullanıcı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma uygunluk NetBenefits ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Uygunluk NetBenefits test kullanıcısı oluşturma](#create-a-fidelity-netbenefits-test-user)**  - kullanıcı Azure AD gösterimini bağlıdır kalitesini NetBenefits Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve uygunluk NetBenefits uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile uygunluk NetBenefits yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **uygunluk NetBenefits** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/fidelitynetbenefits-tutorial/tutorial_fidelitynetbenefits_samlbase.png)

1. Üzerinde **uygunluk NetBenefits etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Uygunluk NetBenefits etki alanı ve URL'ler tek oturum açma bilgileri](./media/fidelitynetbenefits-tutorial/tutorial_fidelitynetbenefits_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL:

    Test ortamı için:  `urn:sp:fidelity:geninbndnbparts20:uat:xq1`

    Üretim ortamı için:  `urn:sp:fidelity:geninbndnbparts20`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL:

    Test ortamı için:  `https://loginxq1.fidelity.com/ftgw/Fas/NBExternal/NBPartSSO/InboundSSO/consumer/sp/ACS.saml2`

    Üretim ortamı için:  `https://login.fidelity.com/ftgw/Fas/NBExternal/NBPartSSO/InboundSSO/consumer/sp/ACS.saml2`
 
1. Uygunluk NetBenefits uygulama belirli bir biçimde SAML onaylamalarını bekler. Şu eşlendi **kullanıcı tanımlayıcısı** ile **user.userprincipalname**. Bu harita **EmployeeID** veya, kuruluşunuz için uygun olan diğer talep **kullanıcı tanımlayıcısı**. Aşağıdaki ekran görüntüsünde bu yalnızca bir örneği gösterilmektedir.

    ![Uygunluk NetBenefits özniteliği](./media/fidelitynetbenefits-tutorial/tutorial_fidelitynetbenefits_attribute.png)

    >[!Note]
    >Statik ve dinamik Federasyon uygunluk NetBenefits destekler. Statik süre kullanıcı yalnızca sağlama destekler yalnızca zaman kullanıcı sağlama ve dinamik anlamına gelir. temel SAML kullanmaz anlamına gelir. JIT kullanmak için temel sağlama müşteriler Azure AD'de kullanıcının doğum tarihi vb. gibi bazı çok talep eklemeniz gerekir. Bu ayrıntıları tarafından sağlanan [uygunluk NetBenefits Destek ekibine](mailto:SSOMaintenance@fmr.com) ve bunlar bu dinamik Federasyon Örneğiniz için etkinleştirmeniz gerekir.
    
1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/fidelitynetbenefits-tutorial/tutorial_fidelitynetbenefits_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/fidelitynetbenefits-tutorial/tutorial_general_400.png)

1. Üzerinde **uygunluk NetBenefits yapılandırma** bölümünde **yapılandırma uygunluk NetBenefits** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Uygunluk NetBenefits yapılandırma](./media/fidelitynetbenefits-tutorial/tutorial_fidelitynetbenefits_configure.png) 

1. Çoklu oturum açmayı yapılandırma **uygunluk NetBenefits** tarafı, indirilen göndermek için ihtiyacınız **meta veri XML**, **SAML çoklu oturum açma hizmeti URL'si** ve  **SAML varlık kimliği** için [uygunluk NetBenefits Destek ekibine](mailto:SSOMaintenance@fmr.com). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/fidelitynetbenefits-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/fidelitynetbenefits-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/fidelitynetbenefits-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/fidelitynetbenefits-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
  
### <a name="create-a-fidelity-netbenefits-test-user"></a>Uygunluk NetBenefits test kullanıcısı oluşturma

Bu bölümde, Britta Simon uygunluk NetBenefits adlı bir kullanıcı oluşturun. Statik Federasyon oluşturuyorsanız, lütfen birlikte çalışarak [uygunluk NetBenefits Destek ekibine](mailto:SSOMaintenance@fmr.com) kullanıcıları uygunluk NetBenefits platformu oluşturmak için. Bu kullanıcılar oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi. 

Dinamik Federasyon için kullanıcılar, kullanıcı tam zamanında sağlama kullanılarak oluşturulur. JIT kullanmak için temel sağlama müşteriler Azure AD'de kullanıcının doğum tarihi vb. gibi bazı çok talep eklemeniz gerekir. Bu ayrıntıları tarafından sağlanan [uygunluk NetBenefits Destek ekibine](mailto:SSOMaintenance@fmr.com) ve bunlar bu dinamik Federasyon Örneğiniz için etkinleştirmeniz gerekir.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için uygunluk NetBenefits erişim vererek Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon için uygunluk NetBenefits atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **uygunluk NetBenefits**.

    ![Uygulamalar listesinde uygunluk NetBenefits bağlantı](./media/fidelitynetbenefits-tutorial/tutorial_fidelitynetbenefits_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde uygunluk NetBenefits kutucuğa tıkladığınızda, otomatik olarak uygunluk NetBenefits uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/fidelitynetbenefits-tutorial/tutorial_general_01.png
[2]: ./media/fidelitynetbenefits-tutorial/tutorial_general_02.png
[3]: ./media/fidelitynetbenefits-tutorial/tutorial_general_03.png
[4]: ./media/fidelitynetbenefits-tutorial/tutorial_general_04.png

[100]: ./media/fidelitynetbenefits-tutorial/tutorial_general_100.png

[200]: ./media/fidelitynetbenefits-tutorial/tutorial_general_200.png
[201]: ./media/fidelitynetbenefits-tutorial/tutorial_general_201.png
[202]: ./media/fidelitynetbenefits-tutorial/tutorial_general_202.png
[203]: ./media/fidelitynetbenefits-tutorial/tutorial_general_203.png

