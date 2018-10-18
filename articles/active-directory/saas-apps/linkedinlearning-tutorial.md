---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle LinkedIn Learning | Microsoft Docs'
description: Azure Active Directory ve LinkedIn Learning arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: d5857070-bf79-4bd3-9a2a-4c1919a74946
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2018
ms.author: jeedes
ms.openlocfilehash: 618437d0007668800e0a14e8233db1676be2a364
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49379272"
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-learning"></a>Öğretici: Azure Active Directory LinkedIn Learning ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile LinkedIn Learning tümleştirme konusunda bilgi edinin.

LinkedIn Learning, Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- LinkedIn Learning erişimi, Azure AD'de denetleyebilirsiniz
- Azure AD hesaplarına otomatik olarak imzalanan (çoklu oturum açma) için LinkedIn Learning açma, kullanıcılarınızın etkinleştirebilirsiniz
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile LinkedIn Learning yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir LinkedIn Learning çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. LinkedIn Learning galeri ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-linkedin-learning-from-the-gallery"></a>LinkedIn Learning galeri ekleme
Azure AD'de LinkedIn Learning tümleştirmesini yapılandırmak için LinkedIn Learning Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**LinkedIn Learning Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Tıklayın **Ekle** iletişim kutusunun üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **LinkedIn Learning**. Sonuçlar panelinde tıklayın **LinkedIn Learning** uygulama eklemek için.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/linkedinlearning-tutorial/tutorial-linkedinlearning_000.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırın ve LinkedIn Learning "Britta Simon" adlı bir test kullanıcı tabanlı Azure AD çoklu oturum açmayı sınayın.

Tek iş için oturum açma için Azure AD ne LinkedIn Learning karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve LinkedIn Learning ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Değerini atayarak bu bağlantı ilişki kurulduktan **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** LinkedIn Learning.

Yapılandırma ve Azure AD çoklu oturum açma LinkedIn Learning ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Bir LinkedIn Learning test kullanıcısı oluşturma](#creating-a-linkedin-learning-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve LinkedIn Learning uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile LinkedIn Learning yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **LinkedIn Learning** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/linkedinlearning-tutorial/tutorial-linkedin_01.png)

1. Farklı bir web tarayıcı penceresinde LinkedIn Learning kiracınıza yönetici olarak oturum.

1. İçinde **hesap Merkezi**, tıklayın **genel ayarları** altında **ayarları**. Ayrıca, seçin **öğrenme - varsayılan** aşağı açılan listeden.

    ![Çoklu oturum açmayı yapılandırın](./media/linkedinlearning-tutorial/tutorial_linkedin_admin_01.png)

1. Tıklayın **veya yüklemek ve tek tek alanları formdan kopyalamak için burayı tıklatın** kopyalayıp **varlık kimliği** ve **onaylama tüketici hizmeti (ACS) URL'si**

    ![Çoklu oturum açmayı yapılandırın](./media/linkedinlearning-tutorial/tutorial_linkedin_admin_03.png)

1. Azure Portal'da altında **LinkedIn Learning etki alanı ve URL'ler**, çoklu oturum AÇMAYA yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP tarafından başlatılan** modu

    ![Çoklu oturum açmayı yapılandırın](./media/linkedinlearning-tutorial/tutorial_linkedin_signon_01.png)

    a. İçinde **tanımlayıcı** metin girin **varlık kimliği** LinkedIn portaldan kopyaladığınız 

    b. İçinde **yanıt URL'si** metin girin **onaylama tüketici hizmeti (ACS) URL'si** LinkedIn portaldan kopyaladığınız

1. Çoklu oturum AÇMAYA yapılandırmak istiyorsanız **SP tarafından başlatılan**, Gelişmiş URL Göster ayarı seçeneğiniz belirttiğiniz oturum açma URL'sini yapılandırma bölümündeki'ye tıklayın.  Oturum açma URL'si kopyanızı oluşturmak için **onaylama tüketici hizmeti (ACS) URL'si** /saml//oturum açma/ile değiştirin.   Yaptıktan sonra oturum açma URL'si şu desende olmalıdır:

    `https://www.linkedin.com/checkpoint/enterprise/login/<AccountId>?application=learning&applicationInstanceId=<InstanceId>`

    ![Çoklu oturum açmayı yapılandırın](./media/linkedinlearning-tutorial/tutorial_linkedin_signon_02.png)   
    
1. LinkedIn Learning uygulamanız SAML onaylamalarını özel öznitelik eşlemelerini SAML belirteci öznitelikleri yapılandırmanıza ekleyin gerektiren belirli bir biçimde bekliyor. Aşağıdaki ekran görüntüsü bunun bir örneği gösterilmektedir. Varsayılan değer olan **kullanıcı tanımlayıcısı** olduğu **user.userprincipalname** ancak LinkedIn Learning bu kullanıcının e-posta adresi ile eşlenmesini bekliyor. Bunun için kullanabileceğiniz **user.mail** listeden öznitelik veya kuruluş yapılandırmanıza göre uygun öznitelik değeri kullanın. 

    ![Çoklu oturum açmayı yapılandırın](./media/linkedinlearning-tutorial/updateusermail.png)
    
1. İçinde **kullanıcı öznitelikleri** bölümünde **görünümü ve diğer tüm kullanıcı özniteliklerini düzenleyin** ve özniteliklerini ayarlayın. Kullanıcının adlı dört talep eklemek gereken **e-posta**, **departmanı**, **firstname**, ve **lastname** ve ileeşlenecekdeğerise**user.mail**, **user.department**, **user.givenname**, ve **user.surname** sırasıyla

    | Öznitelik Adı | Öznitelik Değeri |
    | --- | --- |
    | e-posta| User.Mail |    
    | Bölüm| User.Department |
    | firstName| User.givenName |
    | Soyadı| User.surname |
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/linkedinlearning-tutorial/userattribute.png)
    
    a. Tıklayın **öznitelik Ekle** özniteliği iletişim kutusunu açın.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/linkedinlearning-tutorial/tutorial_attribute_04.png)

    ![Bir Azure AD test kullanıcısı oluşturma](./media/linkedinlearning-tutorial/tutorial_attribute_05.png)
    
    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.
    
    c. Gelen **değer** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.
    
    d. Tıklayın **Tamam**

1. Aşağıdaki adımları gerçekleştirin **adı** öznitelik -

    a. Özniteliği açmak için tıklayın **özniteliğini Düzenle** penceresi.

    ![Çoklu oturum açmayı yapılandırın](./media/linkedinlearning-tutorial/url_update.png)

    b. URL'si değerini Sil **ad alanı**.
    
    c. Tıklayın **Tamam** ayarı kaydedilemiyor.

1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda XML dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/linkedinlearning-tutorial/tutorial-linkedinlearning_certificate.png)

1. **Kaydet**’e tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/linkedinlearning-tutorial/tutorial_general_400.png)

1. Git **LinkedIn yönetici ayarları** bölümü. Karşıya yükleme XML dosyası seçeneğini tıklayarak Azure portalından indirdiğiniz XML dosyasını karşıya yükleyin.

    ![Çoklu oturum açmayı yapılandırın](./media/linkedinlearning-tutorial/tutorial_linkedin_metadata_03.png)

1. Tıklayın **üzerinde** SSO'yu etkinleştirmek üzere. SSO durumu değişir **bağlı** için **bağlandı**

    ![Çoklu oturum açmayı yapılandırın](./media/linkedinlearning-tutorial/tutorial_linkedin_admin_05.png)

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/linkedinlearning-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/linkedinlearning-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/linkedinlearning-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/linkedinlearning-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.

### <a name="creating-a-linkedin-learning-test-user"></a>Bir LinkedIn Learning test kullanıcısı oluşturma

LinkedIn Learning uygulama zamanı kullanıcı sağlamayı ve kimlik doğrulama kullanıcıları otomatik olarak uygulama oluşturulduktan sonra sadece destekler. Yönetici ayarları LinkedIn Learning portal Çevir ' anahtar sayfasında **lisansları otomatik olarak ata** etkin tam zamanında sağlama ve bu da bir lisans kullanıcıya atar.

   ![Bir Azure AD test kullanıcısı oluşturma](./media/linkedinlearning-tutorial/LinkedinUserprovswitch.png)

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, LinkedIn Learning erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon LinkedIn Learning atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201]

1. Uygulamalar listesinde **LinkedIn Learning**.

    ![Çoklu oturum açmayı yapılandırın](./media/linkedinlearning-tutorial/tutorial-linkedinlearning_0001.png)

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.

### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli LinkedIn Learning kutucuğa tıkladığınızda, Azure oturum açma sayfası almanız gerekir ve üzerinde başarılı oturum açma işleminden sonra, LinkedIn Learning uygulamanıza almanız gerekir.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/linkedinlearning-tutorial/tutorial_general_01.png
[2]: ./media/linkedinlearning-tutorial/tutorial_general_02.png
[3]: ./media/linkedinlearning-tutorial/tutorial_general_03.png
[4]: ./media/linkedinlearning-tutorial/tutorial_general_04.png

[100]: ./media/linkedinlearning-tutorial/tutorial_general_100.png

[200]: ./media/linkedinlearning-tutorial/tutorial_general_200.png
[201]: ./media/linkedinlearning-tutorial/tutorial_general_201.png
[202]: ./media/linkedinlearning-tutorial/tutorial_general_202.png
[203]: ./media/linkedinlearning-tutorial/tutorial_general_203.png
