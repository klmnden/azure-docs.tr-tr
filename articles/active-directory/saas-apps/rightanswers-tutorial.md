---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile RightAnswers | Microsoft Docs'
description: Azure Active Directory ve RightAnswers arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 7f09e25a-a716-41e1-8ca3-fd00e3d1b8cc
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7a4f7fef8a1f73bb7c3e19ae32756d13273ddf2d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60255177"
---
# <a name="tutorial-azure-active-directory-integration-with-rightanswers"></a>Öğretici: RightAnswers ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile RightAnswers tümleştirme konusunda bilgi edinin.

Azure AD ile RightAnswers tümleştirme ile aşağıdaki avantajları sağlar:

- RightAnswers erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için RightAnswers (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile RightAnswers yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir RightAnswers çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden RightAnswers ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-rightanswers-from-the-gallery"></a>Galeriden RightAnswers ekleme
Azure AD'de RightAnswers tümleştirmesini yapılandırmak için RightAnswers Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden RightAnswers eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **RightAnswers**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/rightanswers-tutorial/tutorial_rightanswers_search.png)

1. Sonuçlar panelinde seçin **RightAnswers**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/rightanswers-tutorial/tutorial_rightanswers_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." adlı bir test kullanıcı tabanlı RightAnswers ile test etme

Tek iş için oturum açma için Azure AD ne RightAnswers karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının RightAnswers ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

RightAnswers içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma RightAnswers ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[RightAnswers test kullanıcısı oluşturma](#creating-a-rightanswers-test-user)**  - kullanıcı Azure AD gösterimini bağlı RightAnswers Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve RightAnswers uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile RightAnswers yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **RightAnswers** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/rightanswers-tutorial/tutorial_rightanswers_samlbase.png)

1. Üzerinde **RightAnswers etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/rightanswers-tutorial/tutorial_rightanswers_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<subdomain>.rightanswers.com/portal/ss/`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<subdomain>.rightanswers.com:<identifier>/portal`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [RightAnswers istemci Destek ekibine](https://www.rightanswers.com/contact-us/) bu değerleri almak için. 
 
1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/rightanswers-tutorial/tutorial_rightanswers_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/rightanswers-tutorial/tutorial_general_400.png)

1. Çoklu oturum açmayı yapılandırma **RightAnswers** tarafı, indirilen göndermek için ihtiyacınız **meta veri XML** için [RightAnswers Destek ekibine](https://www.rightanswers.com/contact-us/).

    >[!NOTE]
    >Gerçek SSO yapılandırmasını yapmak RightAnswers destek ekibinize sahiptir.
    >SSO, aboneliğiniz için etkinleştirildiğinde bir bildirim alırsınız.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/rightanswers-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/rightanswers-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/rightanswers-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/rightanswers-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-rightanswers-test-user"></a>RightAnswers test kullanıcısı oluşturma

RightAnswers için oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunların RightAnswers sağlanması gerekir. Ne zaman RightAnswers sağlama, hiçbir eylem öğesini, bu nedenle otomatik bir görevdir.

Kullanıcıları otomatik olarak ilk çoklu oturum açma girişimi sırasında gerekirse oluşturulur.

>[!NOTE]
>Herhangi diğer RightAnswers kullanıcı hesabı oluşturma araçları kullanabilir veya API'leri için AAD kullanıcı hesapları sağlamak RightAnswers tarafından sağlanan.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için RightAnswers erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon RightAnswers için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **RightAnswers**.

    ![Çoklu oturum açmayı yapılandırın](./media/rightanswers-tutorial/tutorial_rightanswers_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

SSO ayarlarınızı test etmek isterseniz, erişim Paneli'nde açın. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).
## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/rightanswers-tutorial/tutorial_general_01.png
[2]: ./media/rightanswers-tutorial/tutorial_general_02.png
[3]: ./media/rightanswers-tutorial/tutorial_general_03.png
[4]: ./media/rightanswers-tutorial/tutorial_general_04.png

[100]: ./media/rightanswers-tutorial/tutorial_general_100.png

[200]: ./media/rightanswers-tutorial/tutorial_general_200.png
[201]: ./media/rightanswers-tutorial/tutorial_general_201.png
[202]: ./media/rightanswers-tutorial/tutorial_general_202.png
[203]: ./media/rightanswers-tutorial/tutorial_general_203.png

