---
title: "Öğretici: Azure Active Directory Tümleştirme ScaleX kuruluş ile | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile ScaleX kuruluş arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: c2379a8d-a659-45f1-87db-9ba156d83183
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/20/2017
ms.author: jeedes
ms.openlocfilehash: f83d817647a5339176260bfcf73005045f9ead54
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-scalex-enterprise"></a>Öğretici: Azure Active Directory Tümleştirme ScaleX kuruluş ile

Bu öğreticide, Azure Active Directory (Azure AD) ile ScaleX Kurumsal tümleştirme öğrenin.

Azure AD ile ScaleX Kurumsal tümleştirme ile aşağıdaki avantajları sağlar:

- ScaleX Kurumsal erişimi, Azure AD'de kontrol edebilirsiniz
- Azure AD hesaplarına otomatik olarak ScaleX kuruluş (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. Uygulama erişimi ve çoklu oturum açma ile nedir [Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme ScaleX Enterprise ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir ScaleX Kurumsal çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Bu gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden ScaleX Kurumsal ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-scalex-enterprise-from-the-gallery"></a>Galeriden ScaleX Kurumsal ekleme
Azure ad ScaleX kuruluşta tümleştirmesini yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden ScaleX Kurumsal eklemeniz gerekir.

**Galeriden ScaleX Kurumsal eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Tıklatın **Ekle** iletişim kutusunun üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **ScaleX Kurumsal**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_search.png)

5. Sonuçlar panelinde seçin **ScaleX Kurumsal**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırmanız ve ScaleX kuruluş ile Azure AD çoklu oturum açmayı test "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı

Tekli çalışmaya oturum için Azure AD ne karşılık gelen ScaleX kuruluştaki bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ScaleX Kurumsal ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Bu bağlantı değeri atayarak ilişkisi **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** ScaleX kuruluştaki.

Yapılandırma ve Azure AD çoklu oturum açma ScaleX kuruluş ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[ScaleX Kurumsal test kullanıcısı oluşturma](#creating-a-scalex-enterprise-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı ScaleX Kurumsal sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma ScaleX Kurumsal uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ScaleX Enterprise ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **ScaleX Kurumsal** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda, olarak **modu** seçin **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_samlbase.png)

3. Üzerinde **ScaleX kuruluş etki alanı ve URL'leri** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** modu tarafından başlatılan:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_url1.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, şu biçimi kullanarak değeri yazın:`https://platform.rescale.com/saml2/<company id>/`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://platform.rescale.com/saml2/<company id>/acs/`

4. Denetleme **Göster Gelişmiş URL ayarları**, uygulamada yapılandırmak istiyorsanız **SP** modunda başlatılan:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_url2.png)

    İçinde **oturum açma URL'si** metin kutusuna, şu biçimi kullanarak değeri yazın:`https://platform.rescale.com/saml2/<company id>/sso/`
     
    > [!NOTE] 
    > Bunlar gerçek değerleri değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si veya oturum açma URL'si ile güncelleştirin. Kişi [ScaleX Kurumsal İstemci destek ekibi](http://info.rescale.com/contact_sales) bu değerleri almak için. 

5. SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemelerini değiştirmek gerektiren belirli bir biçimde SAML onaylar ScaleX uygulamanızı bekliyor. Tıklatın **Görünüm ve diğer tüm kullanıcı özniteliklerini düzenleme** özel açmak için onay kutusunu öznitelikleri ayarlar.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-scalexenterprise-tutorial/scalex_attributes.png)
    
    a. Öznitelik sağ tıklayın **adı** ve Sil'i tıklatın.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-scalexenterprise-tutorial/delete_attribute_name.png)

    b. Tıklatın **emailaddress** öznitelik öznitelik Düzenle penceresini açın. Kendi değerini değiştirmek **user.mail** için **user.userprincipalname** ve Tamam'ı tıklatın.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-scalexenterprise-tutorial/edit_email_attribute.png)   
    
5. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_certificate.png) 

6. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_400.png)
    
7. Üzerinde **ScaleX Kurumsal yapılandırma** 'yi tıklatın **ScaleX Kurumsal yapılandırma** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML varlık kimliği** ve **SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_configure.png) 

8. Çoklu oturum açma yapılandırmak için **ScaleX Kurumsal** tarafı, ScaleX Kurumsal şirket Web sitesinin yönetici olarak oturum açın.

9. Menüsünü seçin ve sağ üst **Contoso Yönetim**.

    > [!NOTE] 
    > Contoso yalnızca bir örnektir. Bu, gerçek şirket adınızı olmalıdır. 

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-scalexenterprise-tutorial/Test_Admin.png) 

10. Seçin **tümleştirmeler** seçin ve üstteki menüden **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-scalexenterprise-tutorial/admin_sso.png) 

11. Form aşağıdaki gibi tamamlayın:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-scalexenterprise-tutorial/scalex_admin_save.png) 
    
    a. Seçin **"SSO ile doğrulanabilir herhangi bir kullanıcı oluşturun."**

    b. **Hizmet sağlayıcısı saml**: değer Yapıştır ***urn: OASIS: adları: tc: SAML:2.0:nameid-biçimi: kalıcı***

    c. **ACS yanıt kimlik sağlayıcısı e-posta alanının adı**: değer yapıştırın`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`

    d. **Kimlik sağlayıcısı EntityDescriptor varlık Tanıtıcısı:** Yapıştır **SAML varlık kimliği** değer Azure Portalı'ndan kopyalanır.

    e. **Kimlik sağlayıcısı SingleSignOnService URL'si:** Yapıştır **SAML çoklu oturum açma hizmet URL'si** Azure portalından.

    f. **Kimlik sağlayıcısı ortak X509 sertifikası:** açık X509 sertifika Not Defteri'nde Azure indirilir ve içeriği bu kutuya yapıştırın. Sertifika içeriği ortadaki hiçbir satır sonlarını emin olun.
    
    g. Aşağıdaki onay kutularını kontrol edin: **etkin, NameID şifrelemek ve oturum AuthnRequests.**

    h. Tıklatın **güncelleştirme SSO ayarlarını** ayarları kaydetmek için.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-scalexenterprise-tutorial/create_aaduser_01.png) 

2. Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar** kullanıcıların listesini görüntülemek için.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-scalexenterprise-tutorial/create_aaduser_02.png) 

3. İletişim kutusunun üstündeki **Ekle** açmak için **kullanıcı** iletişim.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-scalexenterprise-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-scalexenterprise-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın.
 
### <a name="creating-a-scalex-enterprise-test-user"></a>ScaleX Kurumsal test kullanıcısı oluşturma

ScaleX Kurumsal oturum açmak Azure AD kullanıcıları etkinleştirmek için bunlar içinde ScaleX kuruluş sağlanmalıdır. ScaleX Kurumsal durumunda sağlama otomatik bir görevdir ve el ile yapılan hiçbir adım gerekli değildir. SSO kimlik bilgileriyle kimlik doğrulamasını başarıyla herhangi bir kullanıcı otomatik olarak ScaleX tarafında sağlanacak.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta ScaleX kuruluş için kullanıcı erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Britta Simon ScaleX kuruluş atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **ScaleX Kurumsal**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.

### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde ScaleX Kurumsal kutucuğuna tıklayın, otomatik olarak imzalanmış ScaleX Kurumsal uygulamanıza açma. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://msdn.microsoft.com/library/dn308586).


## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_203.png

