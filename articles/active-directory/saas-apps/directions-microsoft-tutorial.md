---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Microsoft yönergeleri | Microsoft Docs'
description: Microsoft Azure Active Directory ve yönergeleri arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: e0c8986f-2acd-418d-a306-437abc44b640
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: b83f452300295078e4edf797ec2ef2f0c5223031
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39047482"
---
# <a name="tutorial-azure-active-directory-integration-with-directions-on-microsoft"></a>Öğretici: Azure Active Directory Tümleştirme ile Microsoft yönergeleri

Bu öğreticide, yönergeleri Microsoft Azure Active Directory (Azure AD) ile tümleştirme konusunda bilgi edinin.

Yönergeleri Microsoft Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Microsoft yönergeleri erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için yönergeleri (çoklu oturum açma) Microsoft Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Microsoft yönergeleri ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik bir yönünden Microsoft Çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Microsoft ekleme yönergeleri
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-directions-on-microsoft-from-the-gallery"></a>Galeriden Microsoft ekleme yönergeleri
Tümleştirme yönergeleri, Microsoft Azure AD ile yapılandırmak için yönergeleri Microsoft Galeriden yönetilen SaaS uygulamaları listenize eklemek gerekir.

**Yönergeleri Galeriden Microsoft eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Microsoft yönergeleri**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/directions-microsoft-tutorial/tutorial_directionsonmicrosoft_search.png)

5. Sonuçlar panelinde seçin **Microsoft yönergeleri**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/directions-microsoft-tutorial/tutorial_directionsonmicrosoft_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma yönergeleri "Britta Simon." adlı bir test kullanıcıya dayanarak Microsoft ile test etme

Tek iş için oturum açma için Azure AD ne yönde Microsoft gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve ilgili kullanıcı yönde Microsoft arasında bir bağlantı ilişki kurulması gerekir.

Microsoft yönde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma yönergeleri Microsoft ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Microsoft test kullanıcısı üzerinde bir yönde oluşturma](#creating-a-directions-on-microsoft-test-user)**  - kullanıcı Azure AD gösterimini bağlantılı Microsoft yönde Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Microsoft uygulamasında, bir yönde çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma yönergeleri Microsoft ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Microsoft yönergeleri** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/directions-microsoft-tutorial/tutorial_directionsonmicrosoft_samlbase.png)

3. Üzerinde **Microsoft Domain ve URL'ler hakkında yönergeleri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/directions-microsoft-tutorial/tutorial_directionsonmicrosoft_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak:
    |  |
    | --- |
    | `https://www.directionsonmicrosoft.com/user/login` |
    | `https://<subdomain>.devcloud.acquia-sites.com/<companyname>` |

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak:
    |  |
    | --- |
    | `https://rhelmdirectionsonmicrosoftcomtest.devcloud.acquia-sites.com/simplesaml/<companyname>` |
    | `https://www.directionsonmicrosoft.com/simplesaml/<companyname>` |

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [yönergeleri Microsoft Client üzerinde Destek ekibine](mailto:service@DirectionsOnMicrosoft.com) bu değerleri almak için. 
 
4. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/directions-microsoft-tutorial/tutorial_directionsonmicrosoft_certificate.png) 

5. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/directions-microsoft-tutorial/tutorial_general_400.png)

6. Çoklu oturum açmayı yapılandırma **Microsoft yönergeleri** tarafı, indirilen göndermek için ihtiyacınız **meta veri XML** için [yönergeleri Microsoft Destek ekibine](mailto:service@DirectionsOnMicrosoft.com). Federasyon site üyeliğinize bulmak Microsoft destek ekibi yönünden etkinleştirmek için e-postanızda şirketinizin bilgilerini içerir.
    
    >[!NOTE]
    >Microsoft yönergeleri için çoklu oturum açmayı tarafından etkinleştirilmesi gereken [yönergeleri Microsoft Client üzerinde Destek ekibine](mailto:service@DirectionsOnMicrosoft.com). Çoklu oturum açma etkin olduğunda bir bildirim alırsınız.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/directions-microsoft-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/directions-microsoft-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/directions-microsoft-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/directions-microsoft-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-directions-on-microsoft-test-user"></a>Bir yönde üzerinde Microsoft test kullanıcısı oluşturma

Kullanıcı için Microsoft yönergeleri sağlama yapılandırmanız için hiçbir eylem öğesini yoktur.  

Atanan kullanıcı oturumu açmak için yönergeleri kullanarak erişim panelinde Microsoft çalıştığında yönergeleri Microsoft denetler kullanıcının var olup olmadığını. Varsa kullanıcı hesabı kullanılabilir henüz yönergeleri Microsoft tarafından otomatik olarak oluşturulur.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Microsoft yönergeleri için erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Microsoft yönergeleri atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **Microsoft yönergeleri**.

    ![Çoklu oturum açmayı yapılandırın](./media/directions-microsoft-tutorial/tutorial_directionsonmicrosoft_app.png) 

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.
 
Yönergeleri erişim Paneli'nde Microsoft kutucuğuna tıkladığınızda, otomatik olarak Microsoft uygulamasında, yönergeleri için açan.

Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/directions-microsoft-tutorial/tutorial_general_01.png
[2]: ./media/directions-microsoft-tutorial/tutorial_general_02.png
[3]: ./media/directions-microsoft-tutorial/tutorial_general_03.png
[4]: ./media/directions-microsoft-tutorial/tutorial_general_04.png

[100]: ./media/directions-microsoft-tutorial/tutorial_general_100.png

[200]: ./media/directions-microsoft-tutorial/tutorial_general_200.png
[201]: ./media/directions-microsoft-tutorial/tutorial_general_201.png
[202]: ./media/directions-microsoft-tutorial/tutorial_general_202.png
[203]: ./media/directions-microsoft-tutorial/tutorial_general_203.png

