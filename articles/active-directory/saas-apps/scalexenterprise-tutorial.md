---
title: 'Öğretici: ScaleX Enterprise ile Azure Active Directory Tümleştirmesi | Microsoft Docs'
description: Azure Active Directory ve ScaleX kuruluş arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: c2379a8d-a659-45f1-87db-9ba156d83183
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/20/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 64edf2aa47211c1d2a598417a7b2edc00f260075
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60321442"
---
# <a name="tutorial-azure-active-directory-integration-with-scalex-enterprise"></a>Öğretici: ScaleX Enterprise ile Azure Active Directory Tümleştirmesi

Bu öğreticide, Azure Active Directory (Azure AD) ile ScaleX Kurumsal tümleştirme konusunda bilgi edinin.

Azure AD ile ScaleX Kurumsal tümleştirme ile aşağıdaki avantajları sağlar:

- ScaleX Kurumsal erişimi, Azure AD'de denetleyebilirsiniz
- Azure AD hesaplarına otomatik olarak imzalanan ScaleX kuruluş (çoklu oturum açma) açma, kullanıcılarınızın etkinleştirebilirsiniz
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. Uygulama erişim ve çoklu oturum açma ile ilgili yenilikler [Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Kurumsal ScaleX yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir ScaleX Kurumsal çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Bu gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden ScaleX Kurumsal ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-scalex-enterprise-from-the-gallery"></a>Galeriden ScaleX Kurumsal ekleme
Azure AD'ye ScaleX kuruluşta tümleştirmesini yapılandırmak için ScaleX Kurumsal Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden ScaleX kuruluş eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Tıklayın **Ekle** iletişim kutusunun üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **ScaleX Kurumsal**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/scalexenterprise-tutorial/tutorial_scalexenterprise_search.png)

1. Sonuçlar panelinde seçin **ScaleX Kurumsal**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/scalexenterprise-tutorial/tutorial_scalexenterprise_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırmanız ve ScaleX Enterprise ile Azure AD çoklu oturum açmayı test "Britta Simon." adlı bir test kullanıcı tabanlı

Tek çalışmak için oturum açma için Azure AD ne karşılık gelen kullanıcı ScaleX kurumsal bir kullanıcının Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve ilgili kullanıcı ScaleX kuruluştaki arasında bir bağlantı ilişki kurulması gerekir.

Değerini atayarak bu bağlantı ilişki kurulduktan **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** ScaleX Kurumsal.

Yapılandırma ve Azure AD çoklu oturum açma ScaleX Enterprise ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[ScaleX Kurumsal test kullanıcısı oluşturma](#creating-a-scalex-enterprise-test-user)**  - ScaleX kuruluştaki kullanıcı Azure AD gösterimini bağlı Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve ScaleX Kurumsal uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ScaleX Enterprise ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **ScaleX Kurumsal** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda olarak **modu** seçin **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/scalexenterprise-tutorial/tutorial_scalexenterprise_samlbase.png)

1. Üzerinde **ScaleX Kurumsal etki alanı ve URL'ler** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** başlatılan modu:

    ![Çoklu oturum açmayı yapılandırın](./media/scalexenterprise-tutorial/tutorial_scalexenterprise_url1.png)

    a. İçinde **tanımlayıcı** metin kutusuna şu biçimi kullanarak değeri yazın: `https://platform.rescale.com/saml2/<company id>/`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://platform.rescale.com/saml2/<company id>/acs/`

1. Denetleme **Gelişmiş URL ayarlarını göster**, uygulamada yapılandırmak istiyorsanız **SP** başlatılan modu:

    ![Çoklu oturum açmayı yapılandırın](./media/scalexenterprise-tutorial/tutorial_scalexenterprise_url2.png)

    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak değeri yazın: `https://platform.rescale.com/saml2/<company id>/sso/`
     
    > [!NOTE] 
    > Bunlar gerçek değerlerin değildir. Bu değerler gerçek tanımlayıcı, yanıt URL'si veya oturum açma URL'si ile güncelleştirin. İlgili kişi [ScaleX Kurumsal İstemci Destek ekibine](https://info.rescale.com/contact_sales) bu değerleri almak için. 

1. ScaleX uygulamanız SAML onaylamalarını SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemelerini değiştirmek gerektiren belirli bir biçimde bekliyor. Tıklayın **görünümü ve diğer tüm kullanıcı özniteliklerini düzenleyin** özel açmak için onay kutusunu öznitelikleri ayarlar.

    ![Çoklu oturum açmayı yapılandırın](./media/scalexenterprise-tutorial/scalex_attributes.png)
    
    a. Öznitelik sağ tıklayın **adı** ve Sil'e tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/scalexenterprise-tutorial/delete_attribute_name.png)

    b. Tıklayın **emailaddress** özniteliğini Düzenle açmak için özniteliği. Değerini değiştirmek **user.mail** için **user.userprincipalname** ve Tamam'a tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/scalexenterprise-tutorial/edit_email_attribute.png) 
    
1. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/scalexenterprise-tutorial/tutorial_scalexenterprise_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/scalexenterprise-tutorial/tutorial_general_400.png)
    
1. Üzerinde **ScaleX Kurumsal yapılandırma** bölümünde **ScaleX Kurumsal yapılandırma** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML varlık kimliği** ve **SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/scalexenterprise-tutorial/tutorial_scalexenterprise_configure.png) 

1. Çoklu oturum açmayı yapılandırma **ScaleX Kurumsal** tarafı, ScaleX Kuruluş şirket Web sitesinin bir yönetici olarak oturum açın.

1. Menüsünü seçin ve sağ üst kısımdaki **Contoso Yönetim**.

    > [!NOTE] 
    > Contoso yalnızca örnek olarak verilmiştir. Bu, gerçek şirket adınızı olmalıdır. 

    ![Çoklu oturum açmayı yapılandırın](./media/scalexenterprise-tutorial/Test_Admin.png) 

1. Seçin **tümleştirmeler** seçin ve üstteki menüden **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın](./media/scalexenterprise-tutorial/admin_sso.png) 

1. Formu aşağıdaki gibi doldurun:

    ![Çoklu oturum açmayı yapılandırın](./media/scalexenterprise-tutorial/scalex_admin_save.png) 
    
    a. Seçin **"SSO ile kimlik doğrulaması yapabilen herhangi bir kullanıcı oluşturun."**

    b. **Hizmet sağlayıcısı saml**: Değer yapıştırın ***urn: OASIS: adları: tc: SAML:2.0:nameid-biçimi: kalıcı***

    c. **Kimlik sağlayıcısı e-posta alanı ACS yanıt adını**: Değer yapıştırın `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`

    d. **Kimlik sağlayıcısı EntityDescriptor varlık kimliği:** Yapıştırma **SAML varlık kimliği** değer, Azure portalından kopyalanır.

    e. **Kimlik sağlayıcısı SingleSignOnService URL'si:** Yapıştırma **SAML çoklu oturum açma hizmeti URL'si** Azure portalından.

    f. **Kimlik sağlayıcısı genel X509 sertifikası:** Açık X509 sertifika Defteri'nde azure'dan indirilir ve içeriği bu kutuya yapıştırın. Sertifika içeriği ortadaki hiçbir satır sonlarını emin olun.
    
    g. Aşağıdaki onay kutularını işaretleyin: **Etkin Nameıd şifrelemek ve AuthnRequests oturum açın.**

    h. Tıklayın **güncelleştirme SSO ayarlarını** ayarları kaydetmek için.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/scalexenterprise-tutorial/create_aaduser_01.png) 

1. Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar** kullanıcılar listesini görüntüleyin.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/scalexenterprise-tutorial/create_aaduser_02.png) 

1. İletişim kutusunun en üstünde tıklayın **Ekle** açmak için **kullanıcı** iletişim.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/scalexenterprise-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/scalexenterprise-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-scalex-enterprise-test-user"></a>ScaleX Kurumsal test kullanıcısı oluşturma

ScaleX kuruluş oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunlar içinde ScaleX kuruluş sağlanması gerekir. ScaleX Enterprise, söz konusu olduğunda sağlama otomatik bir görevdir ve el ile bir adım gerekli değildir. SSO kimlik bilgileriyle kimlik doğrulamasını başarıyla herhangi bir kullanıcı ScaleX tarafında otomatik olarak sağlanır.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma ScaleX kuruluş kullanıcı erişimi vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon ScaleX kuruluş atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **ScaleX Kurumsal**.

    ![Çoklu oturum açmayı yapılandırın](./media/scalexenterprise-tutorial/tutorial_scalexenterprise_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.

### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde ScaleX Kurumsal kutucuğuna tıklayın, otomatik olarak imzalanmış ScaleX Kurumsal uygulamanıza açma. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).


## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/scalexenterprise-tutorial/tutorial_general_01.png
[2]: ./media/scalexenterprise-tutorial/tutorial_general_02.png
[3]: ./media/scalexenterprise-tutorial/tutorial_general_03.png
[4]: ./media/scalexenterprise-tutorial/tutorial_general_04.png

[100]: ./media/scalexenterprise-tutorial/tutorial_general_100.png

[200]: ./media/scalexenterprise-tutorial/tutorial_general_200.png
[201]: ./media/scalexenterprise-tutorial/tutorial_general_201.png
[202]: ./media/scalexenterprise-tutorial/tutorial_general_202.png
[203]: ./media/scalexenterprise-tutorial/tutorial_general_203.png

