---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle Slack | Microsoft Docs'
description: Azure Active Directory ve Slack arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ffc5e73f-6c38-4bbb-876a-a7dd269d4e1c
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2018
ms.author: jeedes
ms.openlocfilehash: 8f79926d0d4729c6ad939bc604e9eb885dbe9f03
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39421272"
---
# <a name="tutorial-azure-active-directory-integration-with-slack"></a>Öğretici: Azure Active Directory Slack ile tümleştirme

Bu öğreticide, Slack, Azure Active Directory (Azure AD) ile tümleştirme konusunda bilgi edinin.

Slack Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Slack erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için Slack (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Slack yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Bir Slack çoklu oturum açma abonelik etkin.

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Slack galeri ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-slack-from-the-gallery"></a>Slack galeri ekleme
Azure AD'de Slack tümleştirmesini yapılandırmak için Slack galerideki yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Slack eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **Slack**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/slack-tutorial/tutorial_slack_search.png)

1. Sonuçlar panelinde seçin **Slack**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/slack-tutorial/tutorial_slack_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Slack sınayın.

Tek çalışmak için oturum açma için Azure AD ne Slack karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve Slack ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Slack içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Slack ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Bir Slack test kullanıcısı oluşturma](#creating-a-slack-test-user)**  - kullanıcı Azure AD gösterimini bağlı Slack içinde bir karşılığı Britta simon'un sağlamak için.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Slack uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma Slack ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Slack** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/slack-tutorial/tutorial_slack_samlbase.png)

1. Üzerinde **Slack etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/slack-tutorial/tutorial_slack_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<companyname>.slack.com`

    b. İçinde **tanımlayıcı** metin kutusuna URL'yi yazın: `https://slack.com`

    > [!NOTE] 
    > Değer, gerçek değil. Değerini gerçek işareti bulunan URL'si ile güncelleştirmeniz gerekir. İlgili kişi [Slack Destek ekibine](https://slack.com/help/contact) değeri alınamıyor.
     
1. Slack uygulama belirli bir biçimde SAML onaylamalarını bekler. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz "**kullanıcı öznitelikleri**" uygulama tümleştirme sayfasında bölümü. Aşağıdaki ekran görüntüsü bunun bir örneği gösterilmektedir.
    
    ![Çoklu oturum açmayı yapılandırın](./media/slack-tutorial/tutorial_slack_attribute.png)

    > [!NOTE] 
    > Atanan kullanıcılar varsa **e-posta adresi** bir Office 365 lisansı değil **User.Email** talep SAML belirteci görünmez. Bu gibi durumlarda kullanarak olan öneririz **user.userprincipalname** olarak **User.Email** öznitelik değeri olarak eşleştirmek için **benzersiz tanımlayıcı** yerine.

1. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda **user.mail** olarak **kullanıcı tanımlayıcısı** ve gösterilen her satır için Aşağıdaki tabloda, aşağıdaki adımları gerçekleştirin:
    
    | Öznitelik adı | Öznitelik değeri |
    | --- | --- |
    | first_name | User.givenName |
    | Soyadı | User.surname |
    | User.Email | User.Mail |  
    | User.Username | User.userPrincipalName |

    a. Tıklayarak **özniteliği** açmak için **özniteliğini Düzenle** iletişim kutusu ve aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/slack-tutorial/tutorial_slack_attribute1.png)

    a. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.

    b. Gelen **değer** listesinde, ilgili satır için gösterilen öznitelik değeri seçin.

    c. Bırakın **Namespace** boş.

    d. **Tamam**’a tıklayın.

1. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/slack-tutorial/tutorial_slack_certificate.png)

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/slack-tutorial/tutorial_general_400.png)

1. Üzerinde **Slack yapılandırma** bölümünde **yapılandırma Slack** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/slack-tutorial/tutorial_slack_configure.png)

1. Farklı bir web tarayıcı penceresinde bir Slack şirketinizin sitesi için bir yönetici olarak oturum açın.

1. Gidin **Microsoft Azure AD** ardından **takım ayarları**.

     ![Çoklu oturum açma uygulama tarafında yapılandırma](./media/slack-tutorial/tutorial_slack_001.png)

1. İçinde **takım ayarları** bölümünde **kimlik doğrulaması** sekmesine ve ardından **ayarlarını değiştir**.

    ![Çoklu oturum açma uygulama tarafında yapılandırma](./media/slack-tutorial/tutorial_slack_002.png)

1. Üzerinde **SAML kimlik doğrulaması ayarlarını** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açma uygulama tarafında yapılandırma](./media/slack-tutorial/tutorial_slack_003.png)

    a.  İçinde **SAML 2.0 uç noktası (HTTP)** metin değerini yapıştırın **SAML çoklu oturum açma hizmeti URL'si**, hangi Azure Portalı'ndan kopyaladığınız.

    b.  İçinde **kimlik sağlayıcısını veren** metin değerini yapıştırın **SAML varlık kimliği**, hangi Azure Portalı'ndan kopyaladığınız.

    c.  İndirilen sertifika dosyasını Not Defteri'nde açın, içeriğini, panoya kopyalayın ve ardından ona yapıştırın **ortak sertifika** metin.

    d. Yukarıdaki üç ayarları, Slack, takımınız için uygun şekilde yapılandırın. Ayarlar hakkında daha fazla bilgi için lütfen bulma **Slack'ın SSO Yapılandırma Kılavuzu** burada. `https://get.slack.help/hc/articles/220403548-Guide-to-single-sign-on-with-Slack%60`

    e.  Tıklayın **yapılandırmayı kaydetmek**.

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/slack-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/slack-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/slack-tutorial/create_aaduser_03.png)

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Bir Azure AD test kullanıcısı oluşturma](./media/slack-tutorial/create_aaduser_04.png)

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.

### <a name="creating-a-slack-test-user"></a>Bir Slack test kullanıcısı oluşturma

Bu bölümün amacı, Slack Britta Simon adlı bir kullanıcı oluşturmaktır. Slack tam zamanında sağlama, varsayılan olarak etkin olan destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Yeni bir kullanıcı henüz mevcut değilse, Slack erişme denemesi sırasında oluşturulur. Slack ayrıca otomatik kullanıcı hazırlama, destekleyen daha fazla ayrıntı bulabilirsiniz [burada](slack-provisioning-tutorial.md) otomatik kullanıcı sağlamayı yapılandırma.

> [!NOTE]
> Bir kullanıcı el ile oluşturmanız gerekiyorsa, iletişime geçmeniz [Slack Destek ekibine](https://slack.com/help/contact).

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için Slack erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200]

**Britta Simon için Slack atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Slack**.

    ![Çoklu oturum açmayı yapılandırın](./media/slack-tutorial/tutorial_slack_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Slack kutucuğa tıkladığınızda, erişim Paneli'nde, otomatik olarak Slack uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
* [Kullanıcı sağlamayı yapılandırma](slack-provisioning-tutorial.md)


<!--Image references-->

[1]: ./media/slack-tutorial/tutorial_general_01.png
[2]: ./media/slack-tutorial/tutorial_general_02.png
[3]: ./media/slack-tutorial/tutorial_general_03.png
[4]: ./media/slack-tutorial/tutorial_general_04.png

[100]: ./media/slack-tutorial/tutorial_general_100.png

[200]: ./media/slack-tutorial/tutorial_general_200.png
[201]: ./media/slack-tutorial/tutorial_general_201.png
[202]: ./media/slack-tutorial/tutorial_general_202.png
[203]: ./media/slack-tutorial/tutorial_general_203.png
