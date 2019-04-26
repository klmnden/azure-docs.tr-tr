---
title: 'Öğretici: LinkedIn Sales Navigator ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve LinkedInSalesNavigator arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 7a9fa8f3-d611-4ffe-8d50-04e9586b24da
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 63e3e98c2c3dc8f99e733174c86965304fe483ce
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60257650"
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-sales-navigator"></a>Öğretici: LinkedIn Sales Navigator ile Azure Active Directory Tümleştirme

Bu öğreticide, LinkedIn Sales Navigator Azure Active Directory (Azure AD) ile tümleştirmeyi öğrenin.

LinkedIn Sales Navigator Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- LinkedIn Sales Navigator erişimi, Azure AD'de denetleyebilirsiniz
- Azure AD hesaplarına otomatik olarak imzalanan (çoklu oturum açma) için LinkedIn Sales Navigator açma, kullanıcılarınızın etkinleştirebilirsiniz
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, Gözat [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile LinkedIn Sales Navigator yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir LinkedIn Sales Navigator çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. LinkedIn Sales Navigator galeri ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-linkedin-sales-navigator-from-the-gallery"></a>LinkedIn Sales Navigator galeri ekleme
Azure AD'de LinkedIn Sales Navigator tümleştirmesini yapılandırmak için LinkedIn Sales Navigator Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden LinkedIn Sales Navigator eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Tıklayın **yeni uygulama** iletişim kutusunun üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **LinkedIn Sales Navigator**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_search.png)

1. Sonuçlar panelinde seçin **LinkedIn Sales Navigator**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırın ve LinkedIn Sales Navigator "Britta Simon" adlı bir test kullanıcı tabanlı Azure AD çoklu oturum açmayı sınayın.

Tek iş için oturum açma için Azure AD ne LinkedIn Sales Navigator karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve LinkedIn Sales Navigator ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Değerini atayarak bu bağlantı ilişki kurulduktan **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** LinkedIn Sales Navigator içinde.

Yapılandırma ve Azure AD çoklu oturum açma LinkedIn Sales Navigator ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Bir LinkedIn Sales Navigator test kullanıcısı oluşturma](#creating-a-linkedin-sales-navigator-test-user)**  - kullanıcı Azure AD gösterimini bağlı LinkedIn Sales Navigator Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve LinkedIn Sales Navigator uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma LinkedIn Sales Navigator ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **LinkedIn Sales Navigator** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim, **modu** seçin **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_samlbase.png)

1. Farklı bir web tarayıcı penceresinde için oturum açma, **LinkedIn Sales Navigator** yönetici olarak Web sitesi.

1. İçinde **hesap Merkezi**, tıklayın **genel ayarları** altında **ayarları**. Ayrıca, seçin **Sales Navigator** aşağı açılan listeden.

    ![Çoklu oturum açmayı yapılandırın](./media/linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_01.png)

1. Tıklayın **veya yüklemek ve tek tek alanları formdan kopyalamak için burayı tıklatın** kopyalayıp **varlık kimliği** ve **onaylama tüketici erişim (ACS) URL'si**.

    ![Çoklu oturum açmayı yapılandırın](./media/linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_031.png)

1. Azure Portal'da altında **LinkedIn Sales Navigator etki alanı ve URL'ler** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** modu başlattı.

    ![Çoklu oturum açmayı yapılandırın](./media/linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_url1.png)

    a. İçinde **tanımlayıcı** metin girin **varlık kimliği** LinkedIn portaldan kopyaladığınız 

    b. İçinde **yanıt URL'si** metin girin **onaylama tüketici erişim (ACS) URL'si** LinkedIn portaldan kopyaladığınız

1. Denetleme **Gelişmiş URL ayarlarını göster**, uygulamada yapılandırmak istiyorsanız **SP** modu başlattı.

    ![Çoklu oturum açmayı yapılandırın](./media/linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_url2.png)

    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak değeri yazın: `https://www.linkedin.com/checkpoint/enterprise/login/<account id>?application=salesNavigator`

1. **LinkedIn Sales Navigator** uygulama, özel öznitelik eşlemelerini SAML belirteci öznitelikleri yapılandırmanıza ekleyin gerektiren belirli bir biçimde SAML onaylamalarını bekler. Aşağıdaki anlık görüntüde bir örnek gösterilmektedir. Varsayılan değer olan **kullanıcı tanımlayıcısı** olduğu **user.userprincipalname** ancak LinkedIn Sales Navigator, kullanıcının e-posta adresi ile eşlenmesini bekliyor. Kullanabileceğiniz **user.mail** listeden öznitelik veya kuruluş yapılandırmanıza göre uygun öznitelik değeri kullanın. 

    ![Çoklu oturum açmayı yapılandırın](./media/linkedinsalesnavigator-tutorial/updateusermail.png)
    
1. İçinde **kullanıcı öznitelikleri** bölümünde **görünümü ve diğer tüm kullanıcı özniteliklerini düzenleyin** ve özniteliklerini ayarlayın. Kullanıcının adlı dört talep eklemek gereken **e-posta**, **departmanı**, **firstname**, ve **lastname** ve ileeşlenecekdeğerise**user.mail**, **user.department**, **user.givenname**, ve **user.surname** sırasıyla

    | Öznitelik Adı | Öznitelik Değeri |
    | --- | --- |    
    | e-posta| User.Mail |
    | Bölüm| User.Department |
    | firstName| User.givenName |
    | Soyadı| User.surname |
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/linkedinsalesnavigator-tutorial/userattribute.png)
    
    a. Tıklayarak **öznitelik Ekle** özniteliği iletişim kutusunu açın.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/linkedinsalesnavigator-tutorial/tutorial_attribute_04.png)
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/linkedinsalesnavigator-tutorial/tutorial_attribute_05.png)
   
    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.
    
    c. Gelen **değer** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.
    
    d. Tıklayın **Tamam**

1. Aşağıdaki adımları gerçekleştirin **adı** öznitelik -

    a. Özniteliği açmak için tıklayın **özniteliğini Düzenle** penceresi.

    ![Çoklu oturum açmayı yapılandırın](./media/linkedinsalesnavigator-tutorial/url_update.png)

    b. URL'si değerini Sil **ad alanı**.
    
    c. Tıklayın **Tamam** ayarı kaydedilemiyor.

1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda XML dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/linkedinsalesnavigator-tutorial/tutorial_general_400.png)

1. Git **LinkedIn yönetici ayarları** bölümü. Tıklayın **karşıya yükleme XML dosyası** Azure portalından indirdiğiniz meta veri XML dosyasını karşıya yükleyin.

    ![Çoklu oturum açmayı yapılandırın](./media/linkedinsalesnavigator-tutorial/tutorial_linkedin_metadata_03.png)

1. Tıklayın **üzerinde** SSO'yu etkinleştirmek üzere. SSO durumu değişir **bağlı** için **bağlandı**

    ![Çoklu oturum açmayı yapılandırın](./media/linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_05.png)


> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/linkedinsalesnavigator-tutorial/create_aaduser_01.png) 

1. Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/linkedinsalesnavigator-tutorial/create_aaduser_02.png) 

1. İletişim kutusunun en üstünde tıklayın **Ekle** açmak için **kullanıcı** iletişim.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/linkedinsalesnavigator-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/linkedinsalesnavigator-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-linkedin-sales-navigator-test-user"></a>Bir LinkedIn Sales Navigator test kullanıcısı oluşturma

Bağlantılı Sales Navigator uygulama tam zamanında (JIT) kullanıcı sağlama ve kimlik doğrulama kullanıcıları otomatik olarak uygulama oluşturulduktan sonra destekler. Etkinleştirme **otomatik olarak lisans atamasını** kullanıcıya lisans atamak için.
   
   ![Bir Azure AD test kullanıcısı oluşturma](./media/linkedinsalesnavigator-tutorial/LinkedinUserprovswitch.png)

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açmayı kullanmak için LinkedIn Sales Navigator erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon için LinkedIn Sales Navigator atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **LinkedIn Sales Navigator**.

    ![Çoklu oturum açmayı yapılandırın](./media/linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli LinkedIn Sales Navigator kutucuğa tıkladığınızda, kişisel LinkedIn hesabınızın ayrıntıları sağlamak için sahip olduğu kuruluş sayfasına yönlendirilmesi gerekir. Bu, kişisel hesabınızla LinkedIn iş hesabınızın bağlar. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/linkedinsalesnavigator-tutorial/tutorial_general_01.png
[2]: ./media/linkedinsalesnavigator-tutorial/tutorial_general_02.png
[3]: ./media/linkedinsalesnavigator-tutorial/tutorial_general_03.png
[4]: ./media/linkedinsalesnavigator-tutorial/tutorial_general_04.png

[100]: ./media/linkedinsalesnavigator-tutorial/tutorial_general_100.png

[200]: ./media/linkedinsalesnavigator-tutorial/tutorial_general_200.png
[201]: ./media/linkedinsalesnavigator-tutorial/tutorial_general_201.png
[202]: ./media/linkedinsalesnavigator-tutorial/tutorial_general_202.png
[203]: ./media/linkedinsalesnavigator-tutorial/tutorial_general_203.png

