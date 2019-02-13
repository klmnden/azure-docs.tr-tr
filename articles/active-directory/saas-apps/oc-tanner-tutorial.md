---
title: 'Öğretici: O.C. ile Azure Active Directory Tümleştirme Etikan - AppreciateHub | Microsoft Docs'
description: Azure Active Directory ve O.C. arasında çoklu oturum açmayı yapılandırma hakkında bilgi edinin Tanner - AppreciateHub.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: dee8fbca-0b60-4a21-8917-1fb6919de5a0
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 57838d5f5a49045138ce9adbdcf7855aeab783e8
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56172124"
---
# <a name="tutorial-azure-active-directory-integration-with-oc-tanner---appreciatehub"></a>Öğretici: O.C. ile Azure Active Directory Tümleştirme Tanner - AppreciateHub

Bu öğreticide, O.C. tümleştirmeyi öğrenin Etikan - AppreciateHub ile Azure Active Directory (Azure AD).

O.C. tümleştirme Azure AD ile AppreciateHub Etikan - ile aşağıdaki avantajları sağlar:

- O.C. erişimi, Azure AD'de denetleyebilirsiniz Tanner - AppreciateHub
- Otomatik olarak imzalanan O.C. için açma, kullanıcılarınızın etkinleştirebilirsiniz. Etikan - Azure AD hesaplarıyla AppreciateHub (çoklu oturum açma)
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile O.C. yapılandırmak için Etikan - AppreciateHub, aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- BİR O.C. Etikan - aboneliği etkin AppreciateHub çoklu oturum açma

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. O.C. ekleme Etikan - AppreciateHub Galerisi
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-oc-tanner---appreciatehub-from-the-gallery"></a>O.C. ekleme Etikan - AppreciateHub Galerisi
O.C. tümleştirmesini yapılandırmak için Etikan - AppreciateHub Azure AD'ye ihtiyacınız O.C. eklemek Etikan - galerisinden AppreciateHub listenizi yönetilen SaaS uygulamaları için.

**O.C. eklemek için Etikan - AppreciateHub galerisinden, aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **O.C. Etikan - AppreciateHub**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/oc-tanner-tutorial/tutorial_octannerappreciatehub_search.png)

1. Sonuçlar panelinde seçin **O.C. Etikan - AppreciateHub**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/oc-tanner-tutorial/tutorial_octannerappreciatehub_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma O.C. ile test etme Etikan - AppreciateHub "Britta Simon" adlı bir test kullanıcısı üzerinde temel.

Tek iş için oturum açma için Azure AD içinde O.C. karşılığı kullanıcının bilmesi gerekir Etikan - AppreciateHub Azure AD'de bir kullanıcı için olan. Diğer bir deyişle, bir Azure AD kullanıcısının O.C. ilgili kullanıcı arasında bir bağlantı ilişkisi Etikan - AppreciateHub kurulması gerekir.

İçinde O.C. Etikan - AppreciateHub, değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma O.C. ile test etmek için Etikan - AppreciateHub, aşağıdaki yapı taşlarını tamamlamanız gereken:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Bir O.C. oluşturma Etikan - AppreciateHub test kullanıcısı](#creating-a-oc-tanner---appreciatehub-test-user)**  - O.C. içinde bir karşılığı Britta simon'un sağlamak için Etikan - kullanıcı Azure AD gösterimini bağlı AppreciateHub.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma, O.C. içinde yapılandırın. Etikan - AppreciateHub uygulama.

**Azure AD çoklu oturum açma ile O.C. yapılandırmak için Etikan - AppreciateHub, aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **O.C. Etikan - AppreciateHub** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/oc-tanner-tutorial/tutorial_octannerappreciatehub_samlbase.png)

1. Üzerinde **O.C. Etikan - AppreciateHub etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/oc-tanner-tutorial/tutorial_octannerappreciatehub_url.png)

    a. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<companyname>.octanner.net/sp/ACS.saml2`

    > [!NOTE] 
    > Bu değer, gerçek değil. Bu değer, gerçek yanıt URL'si ile güncelleştirin. İlgili kişi [O.C. Etikan - AppreciateHub Destek ekibine](mailto:sso@octanner.com) bu değeri alınamıyor.

    b. Aşağıdaki bağlantıyı kullanarak meta veri dosyası açın: [ https://fed.appreciatehub.com/fed/sp/metadata ](https://fed.appreciatehub.com/fed/sp/metadata).
   
    c. Bulun **md:AssertionConsumerService** düğümü. 
   
    d. Değerini kopyalayın **konumu** özniteliği. 
   
    ![Uygulama ayarlarını yapılandırma][12]
   
    e. İçinde **işareti bulunan URL'si** değeri önceki adımda elde ettiği geçen metin.

1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/oc-tanner-tutorial/tutorial_octannerappreciatehub_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/oc-tanner-tutorial/tutorial_general_400.png)

1. Çoklu oturum açmayı yapılandırma **O.C. Etikan - AppreciateHub** tarafı, indirilen göndermek için ihtiyacınız **meta veri XML** için [O.C. Etikan - AppreciateHub Destek ekibine](mailto:sso@octanner.com).

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/oc-tanner-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/oc-tanner-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/oc-tanner-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/oc-tanner-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-oc-tanner---appreciatehub-test-user"></a>Bir O.C. oluşturma Etikan - AppreciateHub test kullanıcısı

Bu bölümün amacı O.C. Britta Simon adlı bir kullanıcı oluşturmaktır. Tanner - AppreciateHub.

**Britta Simon O.C. içinde adlı bir kullanıcı oluşturmak için Etikan - AppreciateHub, aşağıdaki adımları gerçekleştirin:**

Sorun, [O.C. Etikan - AppreciateHub Destek ekibine](mailto:sso@octanner.com) Nameıd özniteliği olarak, Azure AD'de Britta simon'un kullanıcı adı ile aynı değere sahip bir kullanıcı oluşturun.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için O.C. erişim vererek Britta Simon etkinleştir Tanner - AppreciateHub.

![Kullanıcı Ata][200] 

**Britta Simon O.C. için atamak için Etikan - AppreciateHub, aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **O.C. Etikan - AppreciateHub**.

    ![Çoklu oturum açmayı yapılandırın](./media/oc-tanner-tutorial/tutorial_octannerappreciatehub_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümün amacı, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test sağlamaktır.  
O.C. tıkladığınızda Etikan - AppreciateHub kutucuk erişim Paneli'nde, otomatik olarak imzalanan, O.C. için açma Etikan - AppreciateHub uygulama.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/oc-tanner-tutorial/tutorial_general_01.png
[2]: ./media/oc-tanner-tutorial/tutorial_general_02.png
[3]: ./media/oc-tanner-tutorial/tutorial_general_03.png
[4]: ./media/oc-tanner-tutorial/tutorial_general_04.png

[12]: ./media/oc-tanner-tutorial/tutorial_octanner_08.png

[100]: ./media/oc-tanner-tutorial/tutorial_general_100.png

[200]: ./media/oc-tanner-tutorial/tutorial_general_200.png
[201]: ./media/oc-tanner-tutorial/tutorial_general_201.png
[202]: ./media/oc-tanner-tutorial/tutorial_general_202.png
[203]: ./media/oc-tanner-tutorial/tutorial_general_203.png

