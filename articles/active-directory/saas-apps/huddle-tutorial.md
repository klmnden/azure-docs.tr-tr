---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle Huddle | Microsoft Docs'
description: Azure Active Directory ve Huddle arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 8389ba4c-f5f8-4ede-b2f4-32eae844ceb0
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/13/2018
ms.author: jeedes
ms.openlocfilehash: fc4ea2538ebe5876e8f3572ab8ad76c4b3b44b8c
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2018
ms.locfileid: "45634345"
---
# <a name="tutorial-azure-active-directory-integration-with-huddle"></a>Öğretici: Azure Active Directory Huddle ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Huddle tümleştirme konusunda bilgi edinin.

Azure AD ile Huddle tümleştirme ile aşağıdaki avantajları sağlar:

- Huddle erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için Huddle (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Huddle yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik Huddle çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Huddle ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-huddle-from-the-gallery"></a>Galeriden Huddle ekleme

Azure AD'de Huddle tümleştirmesini yapılandırmak için Huddle Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Huddle eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Huddle**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/huddle-tutorial/tutorial_huddle_search.png)

5. Sonuçlar panelinde seçin **Huddle**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/huddle-tutorial/tutorial_huddle_addfromgallery.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." adlı bir test kullanıcı tabanlı Huddle ile test etme

Tek iş için oturum açma için Azure AD ne Huddle karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Huddle ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Huddle ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Huddle test kullanıcısı oluşturma](#creating-a-huddle-test-user)**  - kullanıcı Azure AD gösterimini bağlı Huddle Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Huddle uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Huddle yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Huddle** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açmayı yapılandırın](./media/huddle-tutorial/tutorial_huddle_samlbase.png)

3. Üzerinde **Huddle etki alanı ve URL'ler** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** başlatılan modu:

    > [!NOTE]
    > Huddle örneğinizin aşağıdaki girdiğiniz etki alanından otomatik olarak algılanır.

    ![Etki alanı ve URL'ler tek oturum açma bilgileri huddle](./media/huddle-tutorial/tutorial_huddle_url.png)

    a. İçinde **tanımlayıcı** metin herhangi biri şu biçimi kullanarak URL'yi yazın:

    | | |
    |--|--|
    | `https://<customsubdomain>.huddle.net`|
    | `https://my.huddle.net` |
    | |

    b. İçinde **yanıt URL'si** metin herhangi biri şu biçimi kullanarak URL'yi yazın:

    | | |
    |--|--|
    | `https://<customsubdomain>.huddle.net/saml/idp-initiated-sso`|
    | `https://my.huddle.net/saml/idp-initiated-sso`|
    | |

4. Denetleme **Gelişmiş URL ayarlarını göster** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![Etki alanı ve URL'ler tek oturum açma bilgileri huddle](./media/huddle-tutorial/tutorial_huddle_url1.png)

    İçinde **oturum açma URL'si** metin herhangi biri şu biçimi kullanarak URL'yi yazın:

    | | |
    |--|--|
    | `https://<customsubdomain>.huddle.net`|
    | |

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. İlgili kişi [Huddle istemci Destek ekibine](https://huddle.zendesk.com) bu değerleri almak için.

5. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/huddle-tutorial/tutorial_huddle_certificate.png) 

6. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/huddle-tutorial/tutorial_general_400.png)

7. Üzerinde **Huddle yapılandırma** bölümünde **yapılandırma Huddle** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/huddle-tutorial/tutorial_huddle_configure.png) 

8. Çoklu oturum açma Huddle tarafında yapılandırmak için indirilen göndermek gereken **sertifika**, **SAML çoklu oturum açma hizmeti URL'si**, ve **SAML varlık kimliği** için [ İstemci Destek ekibine huddle](https://huddle.zendesk.com). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.  

    >[!NOTE]
    > Çoklu oturum açma Huddle destek ekibi tarafından etkinleştirmesi gerekir. Yapılandırma tamamlandıktan sonra bir bildirim alırsınız.
    >

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/huddle-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/huddle-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/huddle-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/huddle-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-huddle-test-user"></a>Huddle test kullanıcısı oluşturma

Huddle için oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunların Huddle sağlanması gerekir. Huddle söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açın, **Huddle** şirketinizin sitesi yöneticisi olarak.

2. Tıklayın **çalışma**.

3. Tıklayın **kişiler \> kişileri davet edin**.
   
    ![Kişiler](./media/huddle-tutorial/IC787838.png "kişiler")

4. İçinde **yeni bir davet oluşturma** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Yeni davet](./media/huddle-tutorial/IC787839.png "yeni davet")
   
    a. İçinde **insanları katılmaya davet etmek için bir takım seçin** listesinden **takım**.

    b. Tür **e-posta adresi** geçerli bir Azure AD hesabı içinde sağlamak istediğiniz **davet etmek istediğiniz kişilerin e-posta adresi girin** metin.

    c. Tıklayın **davet**.   
   
    >[!NOTE]
    > Azure AD hesap sahibinin etkin hale gelir önce hesabı onaylamak için bir bağlantı içeren bir e-posta alırsınız. 
    > 

>[!NOTE]
>Azure AD kullanıcı hesapları sağlamak için herhangi bir Huddle kullanıcı hesabı oluşturma araçları veya Huddle tarafından sağlanan API'leri kullanabilirsiniz. 
> 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için Huddle erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Huddle için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **Huddle**.

    ![Çoklu oturum açmayı yapılandırın](./media/huddle-tutorial/tutorial_huddle_app.png) 

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Huddle kutucuğa tıkladığınızda, otomatik olarak Huddle uygulamanın oturum açma sayfası almanız gerekir.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/huddle-tutorial/tutorial_general_01.png
[2]: ./media/huddle-tutorial/tutorial_general_02.png
[3]: ./media/huddle-tutorial/tutorial_general_03.png
[4]: ./media/huddle-tutorial/tutorial_general_04.png
[100]: ./media/huddle-tutorial/tutorial_general_100.png
[200]: ./media/huddle-tutorial/tutorial_general_200.png
[201]: ./media/huddle-tutorial/tutorial_general_201.png
[202]: ./media/huddle-tutorial/tutorial_general_202.png
[203]: ./media/huddle-tutorial/tutorial_general_203.png
