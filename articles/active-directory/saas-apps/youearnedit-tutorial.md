---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle YouEarnedIt | Microsoft Docs'
description: Azure Active Directory ve YouEarnedIt arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 3011d44d-dfcf-4061-888f-cff90fbc8150
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2018
ms.author: jeedes
ms.openlocfilehash: f860036f1a69b2d1ab6ac8de763a49380f8fe4bf
ms.sourcegitcommit: 0fcd6e1d03e1df505cf6cb9e6069dc674e1de0be
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2018
ms.locfileid: "42060690"
---
# <a name="tutorial-azure-active-directory-integration-with-youearnedit"></a>Öğretici: Azure Active Directory YouEarnedIt ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile YouEarnedIt tümleştirme konusunda bilgi edinin.

Azure AD ile YouEarnedIt tümleştirme ile aşağıdaki avantajları sağlar:

- YouEarnedIt erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan için YouEarnedIt (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile YouEarnedIt yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik YouEarnedIt çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden YouEarnedIt ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-youearnedit-from-the-gallery"></a>Galeriden YouEarnedIt ekleme

Azure AD'de YouEarnedIt tümleştirmesini yapılandırmak için YouEarnedIt Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden YouEarnedIt eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **YouEarnedt**seçin **YouEarnedt** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde YouEarnedIt](./media/youearnedit-tutorial/tutorial_youearnedit_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı YouEarnedIt sınayın.

Tek iş için oturum açma için Azure AD ne YouEarnedIt karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının YouEarnedIt ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

YouEarnedIt içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma YouEarnedIt ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[YouEarnedIt test kullanıcısı oluşturma](#create-a-youearnedit-test-user)**  - kullanıcı Azure AD gösterimini bağlı YouEarnedIt Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve YouEarnedIt uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile YouEarnedIt yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **YouEarnedIt** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/youearnedit-tutorial/tutorial_youearnedit_samlbase.png)

3. Üzerinde **YouEarnedIt etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![YouEarnedIt etki alanı ve URL'ler tek oturum açma bilgileri](./media/youearnedit-tutorial/tutorial_youearnedit_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL kullanarak aşağıdaki düzenler: 
    | Ortam  | Desen  |
    |:--- |:--- |
    | Üretim | `https://<company name>.youearnedit.com/users/sign_in` |
    | Korumalı Alan  |`https://<company name>.sandbox.youearnedit.com/users/sign_in` |

    b. İçinde **tanımlayıcı** metin kutusuna bir URL kullanarak aşağıdaki düzenler:
    | Ortam  | Desen  |
    |:--- |:--- |
    | Üretim | `https://<company name>.youearnedit.com` |
    | Korumalı Alan  |`https://<company name>.sandbox.youearnedit.com` |

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Bu değerleri almak için atanan YouEarnedIt müşteri başarı yöneticinize başvurun.

4. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/youearnedit-tutorial/tutorial_youearnedit_certificate.png) 

5. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/youearnedit-tutorial/tutorial_general_400.png)

6. Üzerinde **YouEarnedIt yapılandırma** bölümünde **yapılandırma YouEarnedIt** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![YouEarnedIt yapılandırma](./media/youearnedit-tutorial/tutorial_youearnedit_configure.png) 

7. Çoklu oturum açmayı yapılandırma **YouEarnedIt** tarafı, indirilen göndermek için ihtiyacınız **Certificate(Base64)** ve **SAML çoklu oturum açma hizmeti URL'si** , atanan için YouEarnedIt müşteri başarısı Yöneticisi. Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/youearnedit-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/youearnedit-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/youearnedit-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/youearnedit-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.

### <a name="create-a-youearnedit-test-user"></a>YouEarnedIt test kullanıcısı oluşturma

Bu bölümde, Britta Simon YouEarnedIt içinde adlı bir kullanıcı oluşturun. Lütfen YouEarnedIt platform kullanıcıları eklemek için atanan YouEarnedIt müşteri başarısı Yöneticisi çalışın.

>[!NOTE]
>YouEarnedIt Nameıd öznitelik, bir EmailAddress veya kullanıcı adı sağlamak için kimlik sağlayıcısı bekler. Karşılık gelen bir kullanıcı adı veya EmailAddress veritabanı içinde bulunamadı veya tam olarak eşleşmiyor kimlik doğrulaması başarısız olur. Bu hesapları SSO tümleştirme (genellikle ya da API veya CSV içeri aktarma yoluyla) önce YouEarnedIt sistemine alınmasını gerektirir.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için YouEarnedIt erişim vererek Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200]

**Britta Simon YouEarnedIt için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201]

2. Uygulamalar listesinde **YouEarnedIt**.

    ![Uygulamalar listesinde YouEarnedIt bağlantı](./media/youearnedit-tutorial/tutorial_youearnedit_app.png)  

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde YouEarnedIt kutucuğa tıkladığınızda, otomatik olarak YouEarnedIt uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/youearnedit-tutorial/tutorial_general_01.png
[2]: ./media/youearnedit-tutorial/tutorial_general_02.png
[3]: ./media/youearnedit-tutorial/tutorial_general_03.png
[4]: ./media/youearnedit-tutorial/tutorial_general_04.png

[100]: ./media/youearnedit-tutorial/tutorial_general_100.png

[200]: ./media/youearnedit-tutorial/tutorial_general_200.png
[201]: ./media/youearnedit-tutorial/tutorial_general_201.png
[202]: ./media/youearnedit-tutorial/tutorial_general_202.png
[203]: ./media/youearnedit-tutorial/tutorial_general_203.png