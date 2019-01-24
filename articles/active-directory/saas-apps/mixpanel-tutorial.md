---
title: "Öğretici: Azure Active Directory Tümleştirmesi ile Mixpanel'a | Microsoft Docs"
description: Azure Active Directory ve Mixpanel'a arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: a2df26ef-d441-44ac-a9f3-b37bf9709bcb
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 8475cccdac5c864171ac0bad0ad16ed6d6849ecc
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54823173"
---
# <a name="tutorial-azure-active-directory-integration-with-mixpanel"></a>Öğretici: Mixpanel'a ile Azure Active Directory Tümleştirme

Bu öğreticide, Mixpanel'a Azure Active Directory (Azure AD) ile tümleştirmeyi öğrenin.

Azure AD ile Mixpanel'a tümleştirme ile aşağıdaki avantajları sağlar:

- Mixpanel'a erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan Mixpanel'a gönderdiğiniz (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Mixpanel'a yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik Mixpanel'a çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Mixpanel'a ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-mixpanel-from-the-gallery"></a>Galeriden Mixpanel'a ekleme
Azure AD'de Mixpanel'a tümleştirmesini yapılandırmak için Mixpanel'a Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Mixpanel'a eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **Mixpanel'a**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/mixpanel-tutorial/tutorial_mixpanel_search.png)

1. Sonuçlar panelinde seçin **Mixpanel'a**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/mixpanel-tutorial/tutorial_mixpanel_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Mixpanel'a sınayın.

Tek iş için oturum açma için Azure AD ne Mixpanel'a karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Mixpanel'a ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Mixpanel'da, değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Mixpanel'a ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Mixpanel'a test kullanıcısı oluşturma](#creating-a-mixpanel-test-user)**  - kullanıcı Azure AD gösterimini bağlı Mixpanel'a Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Mixpanel'a uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Mixpanel'a yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Mixpanel'a** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/mixpanel-tutorial/tutorial_mixpanel_samlbase.png)

1. Üzerinde **Mixpanel'a etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/mixpanel-tutorial/tutorial_mixpanel_url.png)

     İçinde **oturum açma URL'si** metin kutusuna bir URL: `https://mixpanel.com/login/`

    > [!NOTE] 
    > Lütfen adresindeki kayıt [ https://mixpanel.com/register/ ](https://mixpanel.com/register/) , oturum açma kimlik bilgileri ve iletişim kurmak için [Mixpanel'a Destek ekibine](mailto:support@mixpanel.com) kiracınızın SSO ayarlarını etkinleştirmek için. Ayrıca, üzerinde oturum URL değeri gerekirse Mixpanel'a destek ekibinizden alabilirsiniz. 
 
1. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/mixpanel-tutorial/tutorial_mixpanel_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/mixpanel-tutorial/tutorial_general_400.png)

1. Üzerinde **Mixpanel'a yapılandırma** bölümünde **yapılandırma Mixpanel'a** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/mixpanel-tutorial/tutorial_mixpanel_configure.png) 

1. Farklı bir tarayıcı penceresinde, Mixpanel'a uygulamanıza yönetici olarak oturum.

1. Sayfanın altındaki üzerinde bırakmaya tıklayın **dişli** sol üst köşedeki bir simge. 
   
    ![Mixpanel'a çoklu oturum açma](./media/mixpanel-tutorial/tutorial_mixpanel_06.png) 

1. Tıklayın **erişim güvenlik** sekmesine ve ardından **ayarlarını değiştir**.
   
    ![Mixpanel'a ayarları](./media/mixpanel-tutorial/tutorial_mixpanel_08.png) 

1. Üzerinde **sertifikanızı değiştirmek** iletişim sayfasına tıklayın **dosya** indirilen sertifikanızı karşıya yükleyin ve ardından **sonraki**.
   
    ![Mixpanel'a ayarları](./media/mixpanel-tutorial/tutorial_mixpanel_09.png) 

1.  Kimlik doğrulama URL'si metin kutusuna **kimlik doğrulaması URL'nizi değiştirmek** iletişim sayfasında, değerini yapıştırın **SAML çoklu oturum açma hizmeti URL'si** , Azure Portalı'ndan kopyaladığınız ve ardından**Sonraki**.
   
   ![Mixpanel'a ayarları](./media/mixpanel-tutorial/tutorial_mixpanel_10.png) 

1. **Bitti**’ye tıklayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/mixpanel-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/mixpanel-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/mixpanel-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/mixpanel-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-mixpanel-test-user"></a>Mixpanel'a test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon Mixpanel'da adlı bir kullanıcı oluşturmaktır. 

1. Mixpanel'a şirketinizin sitesi için bir yönetici olarak oturum açın.

1. Sayfanın sonuna üzerinde çok az açmak için sol üst köşedeki dişli düğmesine tıklayın **ayarları** penceresi.

1. Tıklayın **takım** sekmesi.

1. İçinde **takım üyesi** metin Azure'da Britta'nın e-posta adresini yazın.
   
    ![Mixpanel'a ayarları](./media/mixpanel-tutorial/tutorial_mixpanel_11.png) 

1. Tıklayın **davet**. 

> [!Note]
> Kullanıcı profili ayarlamak için bir e-posta alırsınız.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Mixpanel'a gönderdiğiniz erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Mixpanel'a gönderdiğiniz atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Mixpanel'a**.

    ![Çoklu oturum açmayı yapılandırın](./media/mixpanel-tutorial/tutorial_mixpanel_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Mixpanel'a kutucuğa tıkladığınızda, otomatik olarak Mixpanel'a uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/mixpanel-tutorial/tutorial_general_01.png
[2]: ./media/mixpanel-tutorial/tutorial_general_02.png
[3]: ./media/mixpanel-tutorial/tutorial_general_03.png
[4]: ./media/mixpanel-tutorial/tutorial_general_04.png

[100]: ./media/mixpanel-tutorial/tutorial_general_100.png

[200]: ./media/mixpanel-tutorial/tutorial_general_200.png
[201]: ./media/mixpanel-tutorial/tutorial_general_201.png
[202]: ./media/mixpanel-tutorial/tutorial_general_202.png
[203]: ./media/mixpanel-tutorial/tutorial_general_203.png

