---
title: 'Öğretici: Azure Active Directory Tümleştirme SAML 1.1 belirteci ile LOB uygulaması etkin | Microsoft Docs'
description: Azure Active Directory arasında çoklu oturum açmayı yapılandırma hakkında bilgi edinin ve SAML 1.1 belirteci etkin bir LOB uygulaması.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: ced1d88d-0e48-40d5-9aea-ef991cd9d270
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2018
ms.author: jeedes
ms.openlocfilehash: dc07dce24152f9f58253ad96c80f6b004cd1198b
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39050792"
---
# <a name="tutorial-azure-active-directory-integration-with-saml-11-token-enabled-lob-app"></a>Öğretici: Azure Active Directory Tümleştirme SAML 1.1 belirteci ile LOB uygulaması etkin

Bu öğreticide, şunların nasıl tümleştireceğinizi SAML 1.1 belirteci LOB uygulaması Azure Active Directory (Azure AD) ile etkin.

SAML 1.1 belirteci tümleştirme etkin LOB uygulamanızı Azure AD ile aşağıdaki faydaları sağlar:

- Azure'da denetleyebilirsiniz SAML 1.1 belirteç erişimi olan AD etkin LOB uygulaması.
- Otomatik olarak açan almak, kullanıcılarınızın etkinleştirebilirsiniz SAML 1.1 belirteci özellikli LOB (çoklu oturum açma) ile kendi Azure AD hesapları.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure'u yapılandırmak için SAML 1.1 belirteci AD tümleştirmesiyle etkin LOB uygulaması, aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Bir SAML 1.1 belirteci LOB uygulaması tek oturum açma etkin abonelik etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Etkin LOB uygulaması Galerisi SAML 1.1 belirteci ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-saml-11-token-enabled-lob-app-from-the-gallery"></a>Etkin LOB uygulaması Galerisi SAML 1.1 belirteci ekleme
SAML 1.1 belirteci tümleştirmesini yapılandırmak için Azure AD ile LOB uygulaması etkin, SAML 1.1 belirteci etkin LOB uygulaması Galerisi yönetilen SaaS listenizden eklemeniz gerekir.

**SAML 1.1 belirteci etkin galerisinden LOB uygulaması eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **SAML 1.1 belirteci etkin LOB uygulaması**seçin **SAML 1.1 belirteci etkin LOB uygulaması** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuçlar listesinde etkin LOB uygulaması SAML 1.1 belirteci](./media/saml-tutorial/tutorial_saml_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma SAML 1.1 etkin LOB uygulaması "Britta Simon" adlı bir test kullanıcı belirteci tabanlı ile test edin.

Tek iş için oturum açma için Azure AD SAML 1.1 etkin LOB uygulaması, Azure AD'de bir kullanıcıya olan belirteç içinde karşılık gelen kullanıcının bilmesi gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile ilgili kullanıcı SAML 1.1 belirteç arasında bir bağlantı ilişki kurulması LOB uygulama ihtiyaçlarınızı etkin.

Yapılandırmanıza ve sınamanıza Azure AD çoklu oturum açma SAML 1.1 belirteci ile etkin LOB uygulaması, aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir SAML 1.1 etkin belirteci oluşturma LOB uygulamanızı test kullanıcısı](#create-a-saml-11-token-enabled-lob-app-test-user)**  - olmasını Britta simon'un SAML 1.1 belirteç içinde bir karşılığı etkin kullanıcı Azure AD gösterimini bağlı LOB uygulaması.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalındaki etkinleştirme ve çoklu oturum açmayı yapılandırma, SAML 1.1 belirtecini LOB uygulaması özellikli.

**Azure'u yapılandırmak için AD çoklu oturum açma SAML 1.1 belirteci ile LOB uygulaması etkin, aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **SAML 1.1 belirteci etkin LOB uygulaması** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/saml-tutorial/tutorial_saml_samlbase.png)

3. Üzerinde **SAML 1.1 belirteci etkin LOB uygulama etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![SAML 1.1 belirteci LOB uygulama etki alanı ve URL'ler tek oturum açma bilgisi etkin](./media/saml-tutorial/tutorial_saml_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://your-app-url`

    b. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna bir URL şu biçimi kullanarak: `https://your-app-url`
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Lütfen bu değerleri uygulama belirli URL'ler ile değiştirin.  

5. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/saml-tutorial/tutorial_saml_certificate.png) 

6. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/saml-tutorial/tutorial_general_400.png)
    
7. Üzerinde **SAML 1.1 belirteci etkin LOB uygulama yapılandırması** bölümünde **SAML 1.1 belirteci yapılandırma etkin LOB uygulaması** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![SAML 1.1 belirteci LOB uygulama yapılandırması etkin](./media/saml-tutorial/tutorial_saml_configure.png) 

8. Çoklu oturum açmayı yapılandırma **SAML 1.1 belirteci etkin LOB uygulaması** tarafı, indirilen göndermek için ihtiyacınız **sertifika (Base64), oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** için uygulamanın Destek ekibine. Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/saml-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/saml-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/saml-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/saml-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-saml-11-token-enabled-lob-app-test-user"></a>Bir SAML 1.1 etkin belirteci oluşturma LOB uygulamanızı test kullanıcısı

Bu bölümde, Britta Simon adlı bir kullanıcı oluşturma SAML 1.1 belirteci etkin LOB uygulaması. İş uygulaması ile uygulama tarafında kullanıcı oluşturmak için takım destekler. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure'ı kullanmak Britta Simon etkinleştirme çoklu erişim SAML 1.1 belirteç verme ile oturum etkin LOB uygulaması.

![Kullanıcı rolü atayın][200] 

**Britta Simon SAML 1.1 belirtecini atamak için etkin LOB uygulaması, aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **SAML 1.1 belirteci etkin LOB uygulaması**.

    ![SAML 1.1 belirteci uygulamalar listesinde LOB uygulaması bağlantı etkin](./media/saml-tutorial/tutorial_saml_app.png)  

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

' A tıkladığınızda SAML 1.1 belirteci etkin erişim panelinde LOB uygulama kutucuğunda, otomatik olarak açan almalısınız, SAML 1.1 belirteci LOB uygulaması özellikli.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/saml-tutorial/tutorial_general_01.png
[2]: ./media/saml-tutorial/tutorial_general_02.png
[3]: ./media/saml-tutorial/tutorial_general_03.png
[4]: ./media/saml-tutorial/tutorial_general_04.png

[100]: ./media/saml-tutorial/tutorial_general_100.png

[200]: ./media/saml-tutorial/tutorial_general_200.png
[201]: ./media/saml-tutorial/tutorial_general_201.png
[202]: ./media/saml-tutorial/tutorial_general_202.png
[203]: ./media/saml-tutorial/tutorial_general_203.png
