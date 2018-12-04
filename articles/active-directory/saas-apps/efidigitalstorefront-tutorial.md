---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle EFI dijital mağaza | Microsoft Docs'
description: Azure Active Directory ve EFI dijital mağaza arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 33c148fc-d490-4bb9-90c1-d5933679ce4e
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2017
ms.author: jeedes
ms.openlocfilehash: 7df615caf3ca1b8ca7dd7d4da876c840e20defd8
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52833134"
---
# <a name="tutorial-azure-active-directory-integration-with-efi-digital-storefront"></a>Öğretici: Azure Active Directory EFI dijital mağaza ile tümleştirme

Bu öğreticide, EFI dijital mağaza Azure Active Directory (Azure AD) ile tümleştirmeyi öğrenin.

EFI dijital mağaza Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- EFI dijital mağaza erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan EFI (çoklu oturum açma) için dijital mağaza açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile dijital mağaza EFI yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik bir EFI dijital mağaza çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. EFI dijital mağaza galeri ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-efi-digital-storefront-from-the-gallery"></a>EFI dijital mağaza galeri ekleme
Azure AD'de EFI dijital mağaza tümleştirmesini yapılandırmak için EFI dijital mağaza Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**EFI dijital mağaza Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **EFI dijital mağaza**seçin **EFI dijital mağaza** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde EFI dijital mağaza](./media/efidigitalstorefront-tutorial/tutorial_efidigital_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma EFI dijital "Britta Simon" adlı bir test kullanıcı tabanlı vitrin sınayın.

Tek iş için oturum açma için Azure AD ne EFI dijital mağaza karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının EFI dijital mağaza ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

EFI dijital mağazada, değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma EFI dijital mağaza ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[EFI dijital mağaza test kullanıcısı oluşturma](#create-a-efi-digital-storefront-test-user)**  - kullanıcı Azure AD gösterimini bağlı EFI dijital mağaza Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma EFI dijital mağaza uygulamanızı yapılandırın.

**Azure AD çoklu oturum açma ile dijital mağaza EFI yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **EFI dijital mağaza** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/efidigitalstorefront-tutorial/tutorial_efidigital_samlbase.png)

1. Üzerinde **EFI dijital mağaza etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![EFI dijital mağaza etki alanı ve URL'ler tek oturum açma bilgileri](./media/efidigitalstorefront-tutorial/tutorial_efidigital_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<companyname>.myprintdesk.net/DSF`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<companyname>.myprintdesk.net/DSF/asp4/`

1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/efidigitalstorefront-tutorial/tutorial_efidigital_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/efidigitalstorefront-tutorial/tutorial_general_400.png)

1. Çoklu oturum açmayı yapılandırma **EFI dijital mağaza** tarafı, indirilen göndermek için ihtiyacınız **meta veri XML** için [EFI dijital mağaza Destek ekibine](https://www.efi.com/products/productivity-software/ecommerce-web-to-print/efi-digital-storefront/support/). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/efidigitalstorefront-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/efidigitalstorefront-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/efidigitalstorefront-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/efidigitalstorefront-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-efi-digital-storefront-test-user"></a>EFI dijital mağaza test kullanıcısı oluşturma

Bu bölümde, EFI dijital mağazada Britta Simon adlı bir kullanıcı oluşturun. Çalışmak [EFI dijital mağaza Destek ekibine](https://www.efi.com/products/productivity-software/ecommerce-web-to-print/efi-digital-storefront/support/) EFI dijital mağaza platform kullanıcıları eklemek için. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi. 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, EFI dijital mağaza için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon EFI dijital mağaza için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **EFI dijital mağaza**.

    ![Uygulamalar listesinde EFI dijital mağaza bağlantısı](./media/efidigitalstorefront-tutorial/tutorial_efidigital_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli EFI dijital mağaza kutucuğa tıkladığınızda, size otomatik olarak EFI dijital mağaza uygulamanızı açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/efidigitalstorefront-tutorial/tutorial_general_01.png
[2]: ./media/efidigitalstorefront-tutorial/tutorial_general_02.png
[3]: ./media/efidigitalstorefront-tutorial/tutorial_general_03.png
[4]: ./media/efidigitalstorefront-tutorial/tutorial_general_04.png

[100]: ./media/efidigitalstorefront-tutorial/tutorial_general_100.png

[200]: ./media/efidigitalstorefront-tutorial/tutorial_general_200.png
[201]: ./media/efidigitalstorefront-tutorial/tutorial_general_201.png
[202]: ./media/efidigitalstorefront-tutorial/tutorial_general_202.png
[203]: ./media/efidigitalstorefront-tutorial/tutorial_general_203.png

