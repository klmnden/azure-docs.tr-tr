---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile HR2day Merces tarafından | Microsoft Docs'
description: Azure Active Directory ve Merces tarafından HR2day arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 853d08c9-27b1-48d4-b8e7-3705140eb67f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/24/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 378aab82fac5298c3785f752478e3bfc3c6e325b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60275449"
---
# <a name="tutorial-azure-active-directory-integration-with-hr2day-by-merces"></a>Öğretici: Azure Active Directory tarafından Merces HR2day ile tümleştirmesi

Bu öğreticide, HR2day Merces tarafından Azure Active Directory (Azure AD) ile tümleştirmeyi öğrenin.

HR2day Merces tarafından Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- HR2day Merces tarafından erişebilir, Azure AD'de kontrol edebilirsiniz.
- Kullanıcılarınızın otomatik olarak için HR2day Merces tarafından kendi Azure AD hesapları ile oturum için etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda--Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi HR2day Merces tarafından ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz.
- Bir HR2day Merces çoklu oturum açma tarafından abonelik etkin.

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamında kullanımı önerilmemektedir.

Bu öğreticideki adımları test etmek için aşağıdaki önerileri uygulayın:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Alma bir [bir aylık ücretsiz deneme, Azure AD'nin](https://azure.microsoft.com/pricing/free-trial/) zaten sahip değilseniz.  

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Burada özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden HR2day Merces tarafından ekleniyor.
1. Yapılandırma ve test Azure AD çoklu oturum açma.

## <a name="add-hr2day-by-merces-from-the-gallery"></a>Galeriden HR2day Merces tarafından Ekle
Azure AD'ye Merces tarafından HR2day tümleştirmesini yapılandırmak için Galeriden HR2day Merces tarafından yönetilen SaaS uygulamaları listenize ekleyin.

**Galeriden HR2day Merces tarafından eklemek için aşağıdaki adımları uygulayın:**

1. İçinde [Azure portalında](https://portal.azure.com), sol gezinti bölmesinde seçin **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Git **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni bir uygulama eklemek için seçin **yeni uygulama** iletişim kutusunun üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **Merces tarafından HR2day**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/hr2day-tutorial/tutorial_hr2daybymerces_search.png)

1. Sonuçlar panelinde seçin **Merces tarafından HR2day**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/hr2day-tutorial/tutorial_hr2daybymerces_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." adlı bir test kullanıcı tabanlı Merces tarafından HR2day ile test etme

Tek iş için oturum açma için Azure AD HR2day Merces tarafından karşılık gelen kullanıcının Azure AD'de bir kullanıcı için olan bilmesi gerekir. Diğer bir deyişle, HR2day Merces tarafından içinde bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı kurmak gerekir.

Ata tarafından Merces HR2day içinde **kullanıcı adı** için Azure AD'de **kullanıcıadı** ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma HR2day Merces tarafından ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. Azure AD çoklu oturum açmayı yapılandırın: Bu özelliği kullanmak etkinleştirin.
1. Bir Azure AD test kullanıcısı oluşturun: Azure AD çoklu oturum açma Britta Simon ile test edin.
1. Merces test kullanıcı tarafından bir HR2day oluşturun: Kullanıcı Azure AD gösterimini bağlı Merces tarafından HR2day bir karşılığı Britta simon'un oluşturun.
1. Azure AD test kullanıcısı atayın: Britta Simon, Azure AD çoklu oturum açmayı kullanmak etkinleştirin.
1. Çoklu oturum açmayı test edin: Yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Merces uygulama tarafından HR2day içinde yapılandırın.

**Azure AD çoklu oturum açma HR2day Merces tarafından ile yapılandırmak için aşağıdaki adımları uygulayın:**

1. Azure portalında, üzerinde **Merces tarafından HR2day** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Çoklu oturum açma, etkinleştirmek için **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma**.
 
    ![Çoklu oturum açmayı yapılandırma](./media/hr2day-tutorial/tutorial_hr2daybymerces_samlbase.png)

1. İçinde **Merces etki alanı ve URL'ler tarafından HR2day** bölümünde, aşağıdaki adımları uygulayın:

    ![Çoklu oturum açmayı yapılandırma](./media/hr2day-tutorial/tutorial_hr2daybymerces_url.png)

    a. İçinde **oturum açma URL'si** kutusuna şu biçimi kullanarak bir URL yazın: `https://<tenantname>.force.com/<instancename>`.

    b. İçinde **tanımlayıcı** kutusuna şu biçimi kullanarak bir URL yazın: `https://hr2day.force.com/<companyname>`.

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerleri tanımlayıcısı ve gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [Merces istemci destek ekibi tarafından HR2day](mailto:servicedesk@merces.nl) bu değerleri almak için. 
 


1. Üzerinde **SAML imzalama sertifikası** bölümünden **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırma](./media/hr2day-tutorial/tutorial_hr2daybymerces_certificate.png) 

1. Bu bölümde, kullanıcıların hesaplarıyla HR2day Merces tarafından için Azure AD'de kimlik doğrulaması sağlamak açıklar. SAML Protokolü temelinde Federasyon kullanarak bunu.

    HR2day Merces uygulama tarafından özel öznitelik eşlemelerini SAML belirtecinize ekleme gerektiren belirli bir biçimde SAML onaylamalarını bekliyor. Bunun bir örneğini aşağıdaki ekran gösterilir. 

    ![Çoklu oturum açmayı yapılandırma](./media/hr2day-tutorial/tutorial_hr2day_00.png)
    
   > [!NOTE]
   >  SAML onaylaması yapılandırmadan önce başvurmanız gerekir [Merces istemci destek ekibi tarafından HR2day](mailto:servicedesk@merces.nl) ve kiracınız için benzersiz tanımlayıcı özniteliği değeri isteyin. Sonraki bölümde yer alan adımları tamamlamak için bu değere ihtiyacınız. 

1. İçinde **çoklu oturum açma** iletişim kutusundaki **kullanıcı öznitelikleri** bölümünde, aşağıdaki görüntüde gösterildiği gibi SAML belirteci özniteliği yapılandırabilirsiniz. Ardından aşağıdaki adımları uygulayın.
    
      | Öznitelik adı    |   Öznitelik değeri |  
    | ------------------- | -------------------- |    
    | ATTR_LOGINCLAIM | `join([mail],"102938475Z","@"` |
    
      a. Açmak için **öznitelik Ekle** iletişim kutusunda **eklemek agentconfigutil**.

    ![Çoklu oturum açmayı yapılandırma](./media/hr2day-tutorial/tutorial_attribute_04.png)

    ![Çoklu oturum açmayı yapılandırın](./media/hr2day-tutorial/tutorial_attribute_05.png)

    b. İçinde **adı** kutusuna **ATTR_LOGINCLAIM**.

    c. Gelen **değer** listesinden **Join()**.

    d. Gelen **Dize1** listesinden **user.mail**.

    e. İçin **dize2**, HR2day takımınız tarafından sağlanan benzersiz tanımlayıcısını girin.

    f. İçinde **ayırıcı** kutusuna **\@**.
    
    g. **Tamam**’ı seçin.

1. **Kaydet** düğmesini seçin.

    ![Çoklu oturum açmayı yapılandırma](./media/hr2day-tutorial/tutorial_general_400.png)

1. İçinde **Merces yapılandırması HR2day** bölümünden **yapılandırma HR2day Merces tarafından** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si**, **SAML varlık kimliği**, ve **SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru** bölümü.

    ![Çoklu oturum açmayı yapılandırma](./media/hr2day-tutorial/tutorial_hr2daybymerces_configure.png) 

1. Uygulamanız için SSO'yu yapılandırmak için kişi [Merces istemci destek ekibi tarafından HR2day](mailTo:servicedesk@merces.nl). İndirilen ekleme **Certificate(Base64)** dosyasını, e-posta. Ayrıca sağlamak **oturum kapatma URL'si**, **SAML varlık kimliği**, ve **SAML çoklu oturum açma hizmeti URL'si** böylece SSO tümleştirme için yapılandırılabilir.

    > [!NOTE]
    >Bahsetme Merces takıma Bu tümleştirme desen ile ayarlamak için varlık kimliği gerektiğini **https://hr2day.force.com/INSTANCENAME**.

    > [!TIP]
    >İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory** > **kurumsal uygulamalar** bölümünden **çoklu oturum açma** sekmesi. Ekli belgelerin sonra erişim **yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği hakkında [katıştırılmış belgeleri Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985).
   > 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları uygulayın:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde seçin **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/hr2day-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/hr2day-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/hr2day-tutorial/create_aaduser_03.png) 

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları uygulayın:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/hr2day-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola**ve ardından parolayı not alın.

    d. **Oluştur**’u seçin.
 
### <a name="create-an-hr2day-by-merces-test-user"></a>Bir HR2day tarafından Merces test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon HR2day içinde Merces tarafından adlı bir kullanıcı oluşturmaktır. Kullanıcılar HR2day hesap eklemek için çalışmak [Merces istemci destek ekibi tarafından HR2day](mailto:servicedesk@merces.nl). 

> [!NOTE]
> Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [Merces istemci destek ekibi tarafından HR2day](mailto:servicedesk@merces.nl).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma için HR2day Merces tarafından erişim vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon HR2day Merces tarafından atamak için aşağıdaki adımları uygulayın:**

1. Azure portalında uygulama görünümünü açın, dizin görünümüne gidin ve ardından Git **kurumsal uygulamalar**. Ardından, **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Merces tarafından HR2day**.

    ![Çoklu oturum açmayı yapılandırma](./media/hr2day-tutorial/tutorial_hr2daybymerces_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Seçin **Ekle** düğmesi. Ardından **atama Ekle** iletişim kutusunda **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][203]

1. İçinde **kullanıcılar ve gruplar** iletişim kutusundaki **kullanıcılar** listesinden **Britta Simon**.

1. Tıklayın **seçin** düğmesi.

1. İçinde **atama Ekle** iletişim kutusunda **atama**.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümün amacı, Azure AD çoklu oturum açma yapılandırmanızı erişim panelini kullanarak test sağlamaktır.  

HR2day tarafından erişim panelinde Merces kutucuğu seçtiğinizde, HR2day için Merces uygulama tarafından oturumunuz otomatik.

## <a name="additional-resources"></a>Ek kaynaklar

* [Nasıl Azure Active Directory ile SaaS uygulamalarını tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/hr2day-tutorial/tutorial_general_01.png
[2]: ./media/hr2day-tutorial/tutorial_general_02.png
[3]: ./media/hr2day-tutorial/tutorial_general_03.png
[4]: ./media/hr2day-tutorial/tutorial_general_04.png

[100]: ./media/hr2day-tutorial/tutorial_general_100.png

[200]: ./media/hr2day-tutorial/tutorial_general_200.png
[201]: ./media/hr2day-tutorial/tutorial_general_201.png
[202]: ./media/hr2day-tutorial/tutorial_general_202.png
[203]: ./media/hr2day-tutorial/tutorial_general_203.png

