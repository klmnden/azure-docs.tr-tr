---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Bonusly | Microsoft Docs'
description: Azure Active Directory ve Bonusly arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 29fea32a-fa20-47b2-9e24-26feb47b0ae6
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 37feab857f5a8c95b2e47491256c290020615d2d
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55177993"
---
# <a name="tutorial-azure-active-directory-integration-with-bonusly"></a>Öğretici: Bonusly ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Bonusly tümleştirme konusunda bilgi edinin.

Azure AD ile Bonusly tümleştirme ile aşağıdaki avantajları sağlar:

- Bonusly erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için Bonusly (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Bonusly yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik Bonusly çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Bonusly ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-bonusly-from-the-gallery"></a>Galeriden Bonusly ekleme
Azure AD'de Bonusly tümleştirmesini yapılandırmak için Bonusly Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Bonusly eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **Bonusly**seçin **Bonusly** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde bonusly](./media/bonus-tutorial/tutorial_bonusly_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme
Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Bonusly sınayın.

Tek iş için oturum açma için Azure AD ne Bonusly karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Bonusly ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Bonusly içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Bonusly ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Bonusly test kullanıcısı oluşturma](#create-a-bonusly-test-user)**  - kullanıcı Azure AD gösterimini bağlı Bonusly Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Bonusly uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Bonusly yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Bonusly** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/bonus-tutorial/tutorial_bonusly_samlbase.png)

1. Üzerinde **Bonusly etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açma bilgileri bonusly etki alanı ve URL'ler](./media/bonus-tutorial/tutorial_bonusly_url.png)

    İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://Bonus.ly/saml/<tenant-name>`

    > [!NOTE] 
    > Değer, gerçek değil. Değerini gerçek yanıt URL'si ile güncelleştirin. İlgili kişi [Bonusly Destek ekibine](https://bonus.ly/contact) değeri alınamıyor.
 
1. Üzerinde **SAML imzalama sertifikası** bölümünde, kopya **parmak İZİ** sertifikadan değeri.

    ![Sertifika indirme bağlantısı](./media/bonus-tutorial/tutorial_bonusly_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/bonus-tutorial/tutorial_general_400.png)

1. Üzerinde **Bonusly yapılandırma** bölümünde **yapılandırma Bonusly** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Bonusly yapılandırma](./media/bonus-tutorial/tutorial_bonusly_configure.png) 

1. Farklı bir tarayıcı penceresinde, oturum açın, **Bonusly** Kiracı.

1. Üst araç çubuğunda tıklatın **ayarları**ve ardından **tümleştirmeler ve uygulamaları**.
   
    ![Bonusly sosyal bölüm](./media/bonus-tutorial/ic773686.png "Bonusly")
1. Altında **çoklu oturum açma**seçin **SAML**.

1. Üzerinde **SAML** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Bonusly Saml iletişim sayfa](./media/bonus-tutorial/ic773687.png "Bonusly")
   
    a. İçinde **IDP SSO hedef URL** metin değerini yapıştırın **SAML çoklu oturum açma hizmeti URL'si**, hangi Azure Portalı'ndan kopyaladığınız.
   
    b. İçinde **IDP veren** metin değerini yapıştırın **SAML varlık kimliği**, hangi Azure Portalı'ndan kopyaladığınız. 

    c. İçinde **Idp'nin oturum açma URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmeti URL'si**, hangi Azure Portalı'ndan kopyaladığınız.

    d. Yapıştırma **parmak izi** değeri kopyalanan Azure portalından **sertifika parmak izi** metin.
   
1. **Kaydet**’e tıklayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](./media/bonus-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/bonus-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Ekle düğmesi](./media/bonus-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Kullanıcı iletişim kutusu](./media/bonus-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-bonusly-test-user"></a>Bonusly test kullanıcısı oluşturma

Bonusly için oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunların Bonusly sağlanması gerekir. Bonusly söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

>[!NOTE]
>API için AAD kullanıcı hesapları sağlamak Bonusly tarafından sağlanan ya da diğer Bonusly kullanıcı hesabı oluşturma araçlardan kullanabilirsiniz.
>  

**Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Bir web tarayıcısı penceresinde Bonusly kiracınızda oturum açın.

1. Tıklayın **ayarları**.
 
    ![Ayarları](./media/bonus-tutorial/ic781041.png "ayarları")

1. Tıklayın **kullanıcılar ve İkramiyeler** sekmesi.
   
    ![Kullanıcılar ve İkramiyeler](./media/bonus-tutorial/ic781042.png "kullanıcılar ve İkramiyeler")

1. Tıklayın **kullanıcıları yönetme**.
   
    ![Kullanıcıları Yönet](./media/bonus-tutorial/ic781043.png "kullanıcıları yönetme")

1. Tıklayın **kullanıcı ekleme**.
   
    ![Kullanıcı ekleme](./media/bonus-tutorial/ic781044.png "kullanıcı ekleme")

1. Üzerinde **Kullanıcı Ekle** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
    ![Kullanıcı ekleme](./media/bonus-tutorial/ic781045.png "kullanıcı ekleme")  

    a. İçinde **ad** metin gibi kullanıcı adını girin **Britta**.

    b. İçinde **Soyadı** metin gibi kullanıcının soyadını girin **Simon**.
 
    c. İçinde **e-posta** metin gibi kullanıcının e-posta girin **brittasimon@contoso.com**.

    d. **Kaydet**’e tıklayın.
   
     >[!NOTE]
     >Azure AD hesap sahibi bu etkinleştirilmeden önce hesabı onaylamak için bir bağlantı içeren bir e-posta alır.
     >  

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Bonusly erişim vererek Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Bonusly için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Bonusly**.

    ![Uygulamalar listesinde Bonusly bağlantı](./media/bonus-tutorial/tutorial_bonusly_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümün amacı, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test sağlamaktır.

Bonusly kutucuğa tıkladığınızda, erişim Paneli'nde, otomatik olarak Bonusly uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/bonus-tutorial/tutorial_general_01.png
[2]: ./media/bonus-tutorial/tutorial_general_02.png
[3]: ./media/bonus-tutorial/tutorial_general_03.png
[4]: ./media/bonus-tutorial/tutorial_general_04.png

[100]: ./media/bonus-tutorial/tutorial_general_100.png

[200]: ./media/bonus-tutorial/tutorial_general_200.png
[201]: ./media/bonus-tutorial/tutorial_general_201.png
[202]: ./media/bonus-tutorial/tutorial_general_202.png
[203]: ./media/bonus-tutorial/tutorial_general_203.png

