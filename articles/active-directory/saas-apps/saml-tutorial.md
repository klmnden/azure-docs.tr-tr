---
title: 'Öğretici: LOB uygulaması SAML 1.1 belirteci ile Azure Active Directory Tümleştirme etkin | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory arasında yapılandırmayı öğrenin ve LOB uygulaması SAML 1.1 belirteç etkin.
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
ms.openlocfilehash: fca447b24a299fed116356ca0f985020e079344b
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36317611"
---
# <a name="tutorial-azure-active-directory-integration-with-saml-11-token-enabled-lob-app"></a>Öğretici: LOB uygulaması SAML 1.1 belirteci ile Azure Active Directory Tümleştirme etkin

Bu öğreticide, nasıl tümleştireceğinizi öğrenin SAML 1.1 belirteç etkin LOB uygulama Azure Active Directory'ye (Azure AD).

SAML 1.1 belirteç tümleştirme etkin LOB uygulaması Azure AD ile aşağıdaki faydaları sağlar:

- Azure'da denetleyebilirsiniz SAML 1.1 belirteç erişimi AD etkin LOB uygulaması.
- Oturum açma için otomatik olarak almak, kullanıcılarınızın etkinleştirebilirsiniz SAML 1.1 belirteç etkin LOB uygulaması (çoklu oturum açma) Azure AD hesaplarına.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure yapılandırmak için SAML 1.1 belirteci ile AD tümleştirme etkin LOB uygulaması, aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir SAML 1.1 belirteci LOB uygulaması tek oturum açma etkin abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. LOB uygulaması galerisinden etkinleştirilen SAML 1.1 belirteç ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-saml-11-token-enabled-lob-app-from-the-gallery"></a>LOB uygulaması galerisinden etkinleştirilen SAML 1.1 belirteç ekleme
LOB uygulaması SAML 1.1 belirteç tümleştirmesini yapılandırmak için Azure AD ile etkinleştirilen, SAML 1.1 belirteç LOB uygulaması galerisinden yönetilen SaaS uygulama listenizden etkinleştirilen eklemeniz gerekir.

**SAML 1.1 belirteç LOB uygulaması galerisinden etkinleştirilen eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna yazın **LOB uygulaması SAML 1.1 belirteç etkin**seçin **LOB uygulaması SAML 1.1 belirteç etkin** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![SAML 1.1 belirteç LOB uygulaması sonuçlar listesinde etkin](./media/saml-tutorial/tutorial_saml_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma SAML 1.1 etkin LOB uygulaması "Britta Simon" adlı bir test kullanıcı tabanlı belirteci ile test etme.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen kullanıcı SAML 1.1 etkin LOB uygulama bir kullanıcıya Azure AD içinde olduğu belirteci bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve ilgili kullanıcı SAML 1.1 belirteç arasında bir bağlantı ilişkisi kurulmasını LOB uygulama gereksinimlerini etkin.

Yapılandırmanıza ve sınamanıza Azure AD çoklu oturum açma SAML 1.1 belirteci ile etkin LOB uygulaması, aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir SAML 1.1 etkin belirteci oluşturma LOB uygulaması test kullanıcısı](#create-a-saml-11-token-enabled-lob-app-test-user)**  - sağlamak için kullanıcı Azure AD gösterimini bağlı LOB uygulaması SAML 1.1 belirteç içinde Britta Simon, karşılık gelen etkin.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma yapılandırmak, SAML 1.1 belirteç içinde App LOB uygulaması etkinleştirilmiş.

**Azure yapılandırmak için AD çoklu oturum açma SAML 1.1 belirteci ile LOB uygulaması etkinleştirilen, aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **LOB uygulaması SAML 1.1 belirteç etkin** uygulama tümleştirmesi sayfasında, tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/saml-tutorial/tutorial_saml_samlbase.png)

3. Üzerinde **SAML 1.1 belirteç etkinleştirilmiş LOB uygulama etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![SAML 1.1 belirteç LOB uygulama etki alanı ve URL'leri tek oturum açma bilgilerini etkin](./media/saml-tutorial/tutorial_saml_url.png)

    a. İçinde **URL üzerinde oturum** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://your-app-url`

    b. İçinde **tanımlayıcısı (varlık kimliği)** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://your-app-url`
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Lütfen bu değerleri uygulama belirli URL'ler ile değiştirin.  

5. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/saml-tutorial/tutorial_saml_certificate.png) 

6. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/saml-tutorial/tutorial_general_400.png)
    
7. Üzerinde **SAML 1.1 belirteç etkin LOB uygulama yapılandırması** 'yi tıklatın **LOB uygulaması SAML 1.1 belirteci yapılandırma etkin** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![SAML 1.1 belirteç LOB uygulama yapılandırması etkin](./media/saml-tutorial/tutorial_saml_configure.png) 

8. Çoklu oturum açma yapılandırmak için **LOB uygulaması SAML 1.1 belirteç etkin** yan, indirilen göndermek için ihtiyacınız **sertifika (Base64), Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** için uygulama destek ekibi. Bunlar, her iki tarafta da ayarlamanızı SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/saml-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/saml-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/saml-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/saml-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-saml-11-token-enabled-lob-app-test-user"></a>Bir SAML 1.1 etkin belirteci oluşturma LOB uygulaması test kullanıcısı

Bu bölümde, Britta Simon adlı bir kullanıcı oluşturmak LOB uygulaması SAML 1.1 belirteç etkin. Uygulama ile çalışma uygulama tarafında kullanıcı oluşturmak için takım destekler. Kullanıcıların oluşturulan ve çoklu oturum açma kullanmadan önce etkinleştirilmelidir.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure kullanılacak Britta Simon etkinleştirmek tekli erişim SAML 1.1 belirteci verme tarafından oturum etkin LOB uygulaması.

![Kullanıcı rolü atayın][200] 

**Etkin LOB uygulaması Britta Simon SAML 1.1 belirteci olarak atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **LOB uygulaması SAML 1.1 belirteç etkin**.

    ![SAML 1.1 belirteç uygulamalar listesinde LOB uygulaması bağlantı etkin](./media/saml-tutorial/tutorial_saml_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

' I tıklattığınızda SAML 1.1 belirteç LOB uygulama erişim Paneli'ne parçasında etkin, otomatik olarak oturum açmayı almanız gerekir, SAML 1.1 belirteç App LOB uygulaması etkinleştirilmiş.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)



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
