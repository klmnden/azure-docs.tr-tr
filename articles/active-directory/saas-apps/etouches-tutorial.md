---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle etouches | Microsoft Docs'
description: Azure Active Directory ve etouches arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 76cccaa8-859c-4c16-9d1d-8a6496fc7520
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 6850763aa13e30265ca055482917edd28e4759d6
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39425046"
---
# <a name="tutorial-azure-active-directory-integration-with-etouches"></a>Öğretici: Azure Active Directory etouches ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile etouches tümleştirme konusunda bilgi edinin.

Azure AD ile etouches tümleştirme ile aşağıdaki avantajları sağlar:

- Etouches erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için etouches (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile etouches yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik bir etouches çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden etouches ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-etouches-from-the-gallery"></a>Galeriden etouches ekleme
Azure AD'de etouches tümleştirmesini yapılandırmak için etouches Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden etouches eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **etouches**seçin **etouches** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![sonuç listesinde etouches](./media/etouches-tutorial/tutorial_etouches_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme
Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı etouches sınayın.

Tek iş için oturum açma için Azure AD ne etouches karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının etouches ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Etouches içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma etouches ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Bir etouches test kullanıcısı oluşturma](#create-an-etouches-test-user)**  - Britta Simon kullanıcı Azure AD gösterimini bağlı etouches içinde bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve etouches uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile etouches yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **etouches** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/etouches-tutorial/tutorial_etouches_samlbase.png)

1. Üzerinde **etouches etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![etki alanı ve URL'ler etouches çoklu oturum açma bilgileri](./media/etouches-tutorial/tutorial_etouches_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://www.eiseverywhere.com/saml/accounts/?sso&accountid=<ACCOUNTID>`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://www.eiseverywhere.com/<instance name>`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Değeri, gerçek oturum açma URL'si ve öğreticinin sonraki bölümlerinde açıklanan tanımlayıcısı ile güncelleştirin.
    > 

1. etouches uygulama belirli bir biçimde SAML onaylamalarını bekler. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz **kullanıcı özniteliği** uygulama. Aşağıdaki ekran görüntüsü bunun bir örneği gösterilmektedir. 

    ![Kullanıcı Özniteliği](./media/etouches-tutorial/tutorial_etouches_attribute.png) 

1. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, SAML belirteci özniteliği resimde gösterildiği gibi yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Öznitelik Adı | Öznitelik Değeri |
    | ------------------- | -------------------- |
    | Email | User.Mail |    
    
    a. Tıklayın **eklemek agentconfigutil** açmak için **öznitelik Ekle** iletişim.

    ![Öznitelik Ekle](./media/etouches-tutorial/tutorial_attribute_04.png)

    ![Öznitelik iletişim kutusu Ekle](./media/etouches-tutorial/tutorial_attribute_05.png)

    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.

    c. Gelen **değer** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.
    
    d. **Tamam**’a tıklayın. 

1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/etouches-tutorial/tutorial_etouches_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/etouches-tutorial/tutorial_general_400.png)

1. Uygulamanız için yapılandırılmış SSO almak için etouches uygulamada aşağıdaki adımları gerçekleştirin: 

    ![etouches yapılandırma](./media/etouches-tutorial/tutorial_etouches_06.png) 

    a. Oturum açma **etouches** yönetici hakları kullanan uygulama.
   
    b. Git **SAML** yapılandırma.

    c. İçinde **genel ayarlar** bölümünde Not Defteri'nde Azure portalından indirilen sertifikanızı açın, içeriği kopyalayın ve IDP meta verileri metin kutusuna yapıştırın. 

    d. Tıklayarak **Kaydet ve kalın** düğmesi.
  
    e. Tıklayarak **güncelleştirme meta verilerini** SAML meta veri bölümündeki düğmesini. 

    f. Bu, sayfası açılır ve SSO gerçekleştirin. SSO çalışmaya başladığında, kullanıcı adına ayarlayabilirsiniz.

    g. Kullanıcı adı alanı seçin **emailaddress** aşağıdaki resimde gösterildiği gibi. 

    h. Kopyalama **SP varlık kimliği** yapıştırın ve değer **tanımlayıcı** içinde metin **etouches etki alanı ve URL'ler** bölümü Azure portalı.

    i. Kopyalama **SSO URL / ACS** yapıştırın ve değer **oturum açma URL'si** içinde metin **etouches etki alanı ve URL'ler** bölümü Azure portalı.
   
> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](./media/etouches-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/etouches-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Ekle düğmesi](./media/etouches-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Kullanıcı iletişim kutusu](./media/etouches-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-an-etouches-test-user"></a>Bir etouches test kullanıcısı oluşturma

Bu bölümde, Britta Simon etouches içinde adlı bir kullanıcı oluşturun. Çalışmak [etouches istemci Destek ekibine](https://www.etouches.com/event-software/support/customer-support/) etouches platform kullanıcıları eklemek için.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma etouches erişim vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon etouches için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **etouches**.

    ![Uygulamalar listesinde etouches bağlantı](./media/etouches-tutorial/tutorial_etouches_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi


Bu bölümün amacı, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test sağlamaktır.

Erişim panelinde etouches kutucuğa tıkladığınızda, otomatik olarak etouches uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/etouches-tutorial/tutorial_general_01.png
[2]: ./media/etouches-tutorial/tutorial_general_02.png
[3]: ./media/etouches-tutorial/tutorial_general_03.png
[4]: ./media/etouches-tutorial/tutorial_general_04.png

[100]: ./media/etouches-tutorial/tutorial_general_100.png

[200]: ./media/etouches-tutorial/tutorial_general_200.png
[201]: ./media/etouches-tutorial/tutorial_general_201.png
[202]: ./media/etouches-tutorial/tutorial_general_202.png
[203]: ./media/etouches-tutorial/tutorial_general_203.png

