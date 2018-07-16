---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle FreshGrade | Microsoft Docs'
description: Azure Active Directory ve FreshGrade arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1055bba6-f4df-462e-bc9b-1ad5ada0f638
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: jeedes
ms.openlocfilehash: 7a24f69e19d492647a24284a13342795559e0542
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39047170"
---
# <a name="tutorial-azure-active-directory-integration-with-freshgrade"></a>Öğretici: Azure Active Directory FreshGrade ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile FreshGrade tümleştirme konusunda bilgi edinin.

Azure AD ile FreshGrade tümleştirme ile aşağıdaki avantajları sağlar:

- FreshGrade erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için FreshGrade (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile FreshGrade yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik FreshGrade çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden FreshGrade ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-freshgrade-from-the-gallery"></a>Galeriden FreshGrade ekleme
Azure AD'de FreshGrade tümleştirmesini yapılandırmak için FreshGrade Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden FreshGrade eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **FreshGrade**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/freshgrade-tutorial/tutorial_freshgrade_search.png)

5. Sonuçlar panelinde seçin **FreshGrade**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/freshgrade-tutorial/tutorial_freshgrade_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı FreshGrade sınayın.

Tek iş için oturum açma için Azure AD ne FreshGrade karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının FreshGrade ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

FreshGrade içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma FreshGrade ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[FreshGrade test kullanıcısı oluşturma](#creating-a-freshgrade-test-user)**  - kullanıcı Azure AD gösterimini bağlı FreshGrade Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve FreshGrade uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile FreshGrade yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **FreshGrade** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açmayı yapılandırın](./media/freshgrade-tutorial/tutorial_freshgrade_samlbase.png)

3. Üzerinde **FreshGrade etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/freshgrade-tutorial/tutorial_freshgrade_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL kullanarak aşağıdaki düzenler:
      | |
      |--|
      | `https://<subdomain>.freshgrade.com/login` |
      | `https://<subdomain>.onboarding.freshgrade.com/login` |

    b. İçinde **tanımlayıcı** metin kutusuna bir URL kullanarak aşağıdaki düzenler:
      | |
      |--|
      | `https://login.onboarding.freshgrade.com:443/saml/metadata/alias/<instancename>` |
      | `https://login.freshgrade.com:443/saml/metadata/alias/<instancename>` |

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [FreshGrade istemci Destek ekibine](mailTo:support@freshgrade.com) bu değerleri almak için.

4. Üzerinde **SAML imzalama sertifikası** bölümünde, kopyalamak için Kopyala düğmesine **uygulama Federasyon meta verileri URL'sini** kopyalayıp Not Defteri'ne yapıştırın.
    
    ![Çoklu oturum açmayı yapılandırın](./media/freshgrade-tutorial/tutorial_metadataurl.png)
     
5. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/freshgrade-tutorial/tutorial_general_400.png)

6. Üzerinde **FreshGrade yapılandırma** bölümünde **yapılandırma FreshGrade** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/freshgrade-tutorial/tutorial_freshgrade_configure.png)

7. Çoklu oturum açmayı yapılandırma **FreshGrade** tarafını göndermek için ihtiyacınız **uygulama Federasyon meta verileri URL'sini** ve **SAML çoklu oturum açma hizmeti URL'si** için [ FreshGrade Destek ekibine](mailTo:support@freshgrade.com). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/freshgrade-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/freshgrade-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/freshgrade-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/freshgrade-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-freshgrade-test-user"></a>FreshGrade test kullanıcısı oluşturma

Bu bölümde, Britta Simon FreshGrade içinde adlı bir kullanıcı oluşturun. Lütfen birlikte çalışarak [FreshGrade Destek ekibine](mailTo:support@freshgrade.com) FreshGrade platform kullanıcıları eklemek için.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için FreshGrade erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon FreshGrade için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **FreshGrade**.

    ![Çoklu oturum açmayı yapılandırın](./media/freshgrade-tutorial/tutorial_freshgrade_app.png) 

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde FreshGrade kutucuğa tıkladığınızda, otomatik olarak FreshGrade uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/freshgrade-tutorial/tutorial_general_01.png
[2]: ./media/freshgrade-tutorial/tutorial_general_02.png
[3]: ./media/freshgrade-tutorial/tutorial_general_03.png
[4]: ./media/freshgrade-tutorial/tutorial_general_04.png

[100]: ./media/freshgrade-tutorial/tutorial_general_100.png

[200]: ./media/freshgrade-tutorial/tutorial_general_200.png
[201]: ./media/freshgrade-tutorial/tutorial_general_201.png
[202]: ./media/freshgrade-tutorial/tutorial_general_202.png
[203]: ./media/freshgrade-tutorial/tutorial_general_203.png

