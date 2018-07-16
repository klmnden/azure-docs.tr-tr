---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle ScaleX Kurumsal | Microsoft Docs'
description: Azure Active Directory ve ScaleX kuruluş arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: c2379a8d-a659-45f1-87db-9ba156d83183
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/20/2017
ms.author: jeedes
ms.openlocfilehash: 3b2da2680adbc92655030351cc9e1269a4cccccd
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39041014"
---
# <a name="tutorial-azure-active-directory-integration-with-scalex-enterprise"></a>Öğretici: Azure Active Directory ScaleX Enterprise ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile ScaleX Kurumsal tümleştirme konusunda bilgi edinin.

Azure AD ile ScaleX Kurumsal tümleştirme ile aşağıdaki avantajları sağlar:

- ScaleX Kurumsal erişimi, Azure AD'de denetleyebilirsiniz
- Azure AD hesaplarına otomatik olarak imzalanan ScaleX kuruluş (çoklu oturum açma) açma, kullanıcılarınızın etkinleştirebilirsiniz
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. Uygulama erişim ve çoklu oturum açma ile ilgili yenilikler [Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Kurumsal ScaleX yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Bir ScaleX Kurumsal çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Bu gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden ScaleX Kurumsal ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-scalex-enterprise-from-the-gallery"></a>Galeriden ScaleX Kurumsal ekleme
Azure AD'ye ScaleX kuruluşta tümleştirmesini yapılandırmak için ScaleX Kurumsal Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden ScaleX kuruluş eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Tıklayın **Ekle** iletişim kutusunun üst kısmındaki düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **ScaleX Kurumsal**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/scalexenterprise-tutorial/tutorial_scalexenterprise_search.png)

5. Sonuçlar panelinde seçin **ScaleX Kurumsal**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/scalexenterprise-tutorial/tutorial_scalexenterprise_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırmanız ve ScaleX Enterprise ile Azure AD çoklu oturum açmayı test "Britta Simon." adlı bir test kullanıcı tabanlı

Tek çalışmak için oturum açma için Azure AD ne karşılık gelen kullanıcı ScaleX kurumsal bir kullanıcının Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve ilgili kullanıcı ScaleX kuruluştaki arasında bir bağlantı ilişki kurulması gerekir.

Değerini atayarak bu bağlantı ilişki kurulduktan **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** ScaleX Kurumsal.

Yapılandırma ve Azure AD çoklu oturum açma ScaleX Enterprise ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[ScaleX Kurumsal test kullanıcısı oluşturma](#creating-a-scalex-enterprise-test-user)**  - ScaleX kuruluştaki kullanıcı Azure AD gösterimini bağlı Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve ScaleX Kurumsal uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ScaleX Enterprise ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **ScaleX Kurumsal** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda olarak **modu** seçin **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/scalexenterprise-tutorial/tutorial_scalexenterprise_samlbase.png)

3. Üzerinde **ScaleX Kurumsal etki alanı ve URL'ler** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** başlatılan modu:

    ![Çoklu oturum açmayı yapılandırın](./media/scalexenterprise-tutorial/tutorial_scalexenterprise_url1.png)

    a. İçinde **tanımlayıcı** metin kutusuna şu biçimi kullanarak değeri yazın: `https://platform.rescale.com/saml2/<company id>/`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://platform.rescale.com/saml2/<company id>/acs/`

4. Denetleme **Gelişmiş URL ayarlarını göster**, uygulamada yapılandırmak istiyorsanız **SP** başlatılan modu:

    ![Çoklu oturum açmayı yapılandırın](./media/scalexenterprise-tutorial/tutorial_scalexenterprise_url2.png)

    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak değeri yazın: `https://platform.rescale.com/saml2/<company id>/sso/`
     
    > [!NOTE] 
    > Bunlar gerçek değerlerin değildir. Bu değerler gerçek tanımlayıcı, yanıt URL'si veya oturum açma URL'si ile güncelleştirin. İlgili kişi [ScaleX Kurumsal İstemci Destek ekibine](http://info.rescale.com/contact_sales) bu değerleri almak için. 

5. ScaleX uygulamanız SAML onaylamalarını SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemelerini değiştirmek gerektiren belirli bir biçimde bekliyor. Tıklayın **görünümü ve diğer tüm kullanıcı özniteliklerini düzenleyin** özel açmak için onay kutusunu öznitelikleri ayarlar.

    ![Çoklu oturum açmayı yapılandırın](./media/scalexenterprise-tutorial/scalex_attributes.png)
    
    a. Öznitelik sağ tıklayın **adı** ve Sil'e tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/scalexenterprise-tutorial/delete_attribute_name.png)

    b. Tıklayın **emailaddress** özniteliğini Düzenle açmak için özniteliği. Değerini değiştirmek **user.mail** için **user.userprincipalname** ve Tamam'a tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/scalexenterprise-tutorial/edit_email_attribute.png) 
    
5. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/scalexenterprise-tutorial/tutorial_scalexenterprise_certificate.png) 

6. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/scalexenterprise-tutorial/tutorial_general_400.png)
    
7. Üzerinde **ScaleX Kurumsal yapılandırma** bölümünde **ScaleX Kurumsal yapılandırma** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML varlık kimliği** ve **SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/scalexenterprise-tutorial/tutorial_scalexenterprise_configure.png) 

8. Çoklu oturum açmayı yapılandırma **ScaleX Kurumsal** tarafı, ScaleX Kuruluş şirket Web sitesinin bir yönetici olarak oturum açın.

9. Menüsünü seçin ve sağ üst kısımdaki **Contoso Yönetim**.

    > [!NOTE] 
    > Contoso yalnızca örnek olarak verilmiştir. Bu, gerçek şirket adınızı olmalıdır. 

    ![Çoklu oturum açmayı yapılandırın](./media/scalexenterprise-tutorial/Test_Admin.png) 

10. Seçin **tümleştirmeler** seçin ve üstteki menüden **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın](./media/scalexenterprise-tutorial/admin_sso.png) 

11. Formu aşağıdaki gibi doldurun:

    ![Çoklu oturum açmayı yapılandırın](./media/scalexenterprise-tutorial/scalex_admin_save.png) 
    
    a. Seçin **"SSO ile kimlik doğrulaması yapabilen herhangi bir kullanıcı oluşturun."**

    b. **Hizmet sağlayıcısı saml**: değer yapıştırın ***urn: OASIS: adları: tc: SAML:2.0:nameid-biçimi: kalıcı***

    c. **Kimlik sağlayıcısı e-posta alanı ACS yanıt adını**: değer yapıştırın `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`

    d. **Kimlik sağlayıcısı EntityDescriptor varlık kimliği:** Yapıştır **SAML varlık kimliği** değer, Azure portalından kopyalanır.

    e. **Kimlik sağlayıcısı SingleSignOnService URL'si:** Yapıştır **SAML çoklu oturum açma hizmeti URL'si** Azure portalından.

    f. **Kimlik sağlayıcısı genel X509 sertifikası:** açık X509 sertifika Defteri'nde azure'dan indirilir ve içeriği bu kutuya yapıştırın. Sertifika içeriği ortadaki hiçbir satır sonlarını emin olun.
    
    g. Aşağıdaki onay kutularını işaretleyin: **etkin, Nameıd şifrelemek ve oturum AuthnRequests.**

    h. Tıklayın **güncelleştirme SSO ayarlarını** ayarları kaydetmek için.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/scalexenterprise-tutorial/create_aaduser_01.png) 

2. Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar** kullanıcılar listesini görüntüleyin.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/scalexenterprise-tutorial/create_aaduser_02.png) 

3. İletişim kutusunun en üstünde tıklayın **Ekle** açmak için **kullanıcı** iletişim.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/scalexenterprise-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
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

2. Uygulamalar listesinde **ScaleX Kurumsal**.

    ![Çoklu oturum açmayı yapılandırın](./media/scalexenterprise-tutorial/tutorial_scalexenterprise_app.png) 

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.

### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde ScaleX Kurumsal kutucuğuna tıklayın, otomatik olarak imzalanmış ScaleX Kurumsal uygulamanıza açma. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).


## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)



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

