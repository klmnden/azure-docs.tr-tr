---
title: "Öğretici: Azure Active Directory Tümleştirme Cezanne ik yazılımıyla | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ve Cezanne ik yazılımı arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: fb8f95bf-c3c1-4e1f-b2b3-3b67526be72d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: 623c438edfce5f98c2d32d8bb25a97d86aa77909
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-integrate-azure-active-directory-with-cezanne-hr-software"></a>Öğretici: Azure Active Directory Cezanne ik yazılım ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Cezanne ik yazılım tümleştirmek öğrenin.

Cezanne HR yazılım Azure AD ile tümleştirme ile aşağıdaki yararları sağlar. Şunları yapabilirsiniz:

- Cezanne HR yazılım erişimi, Azure AD'de denetler.
- Çoklu oturum açma (SSO) ile Azure AD hesaplarına Cezanne ik yazılımı için otomatik olarak oturum açmak etkinleştirin.
- Hesaplarınızı tek bir merkezi konumda yönetme: Azure portal.

Azure AD ile hizmet (SaaS) uygulaması tümleştirme olarak yazılım hakkında daha fazla bilgi edinmek için [uygulama erişimi ve SSO ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme Cezanne ik yazılımıyla yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Cezanne HR yazılım abonelik SSO'su etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamında kullanmamanızı öneririz.

Bu öğreticide adımları test etmek için aşağıdaki önerileri uygulayın:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD SSO bir test ortamında test. 

Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

* Galeriden Cezanne ik yazılım ekleme
* Yapılandırma ve Azure AD SSO test etme

## <a name="add-cezanne-hr-software-from-the-gallery"></a>Galeriden Cezanne ik yazılım Ekle
Azure AD Cezanne ik yazılım tümleştirilmesi yapılandırmak için Cezanne ik yazılım Galeriden yönetilen SaaS uygulamaları listenize ekleyin.

Galeriden Cezanne ik yazılım eklemek için aşağıdakileri yapın:

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol bölmede seçin **Azure Active Directory** düğmesi. 

    !["Azure Active Directory" düğmesi][1]

2. Seçin **kurumsal uygulamalar** > **tüm uygulamaları**.

    !["Tüm uygulamaları" bağlantı][2]
    
3. En üstte yeni bir uygulama eklemek için **tüm uygulamaları** iletişim kutusunda **yeni uygulama**.

    !["Yeni uygulama" düğmesi][3]

4. Arama kutusuna **Cezanne ik yazılım**.

    ![Arama kutusu](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_search.png)

5. Sonuçlar listesinde **Cezanne ik yazılım** ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Sonuçları listesi](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme
Bu bölümde, yapılandırma ve Azure AD SSO Cezanne ik yazılım "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı ile test etme

Çalışmak SSO için Azure AD Azure AD kullanıcı Cezanne ik yazılım karşılık gelen bilmesi gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının ve ilgili kullanıcı arasındaki bağlantıyı ilişki Cezanne ik yazılımda oluşturmanız gerekir.

Bağlantı ilişkisi oluşturmak için Cezanne ik yazılım atamak **kullanıcı adı** değeri olarak Azure AD **kullanıcıadı** değeri.

Yapılandırmak ve Cezanne ik yazılımı kullanarak Azure AD SSO sınamak için aşağıdaki yapı taşları tamamlayın.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO yapılandırın

Bu bölümde, Azure portalında Azure AD SSO'yu etkinleştirmek ve aşağıdakileri yaparak Cezanne ik yazılım uygulamanızda SSO yapılandırın:

1. Azure portalında üzerinde **Cezanne ik yazılım** uygulama tümleştirmesi sayfasında, **çoklu oturum açma**.

    !["Çoklu oturum açmayı" komutu][4]

2. SSO, etkinleştirmek için **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma**.
 
    !["Modu" kutusu](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_samlbase.png)

3. Altında **Cezanne ik yazılım etki alanı ve URL'leri**, aşağıdakileri yapın:

    !["Cezanne ik yazılım etki alanı ve URL'leri" bölümü](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_url.png)

    a. İçinde **oturum açma URL'si** kutusunda, aşağıdaki söz dizimini bir URL yazın:`https://w3.cezanneondemand.com/cezannehr/-/<tenant id>`

    b. İçinde **yanıt URL'si** kutusunda, aşağıdaki söz dizimini bir URL yazın:`https://w3.cezanneondemand.com:443/<tenantid>`    
     
    > [!NOTE] 
    > Yukarıdaki değerleri gerçek değildir. Bunları, gerçek yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Değerleri almak için başvurun [Cezanne ik yazılım istemci destek ekibi](mailto:info@cezannehr.com).

4. Altında **SAML imzalama sertifikası**seçin **sertifika (Base64)**ve ardından sertifika dosyayı bilgisayarınıza kaydedin.

    !["SAML imzalama sertifikası" bölümü](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_certificate.png) 

5. **Kaydet**’i seçin.

    !["Kaydet" düğmesi](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_400.png)
    
6. Altında **Cezanne ik yazılım yapılandırma**seçin **Cezanne ik yazılımı Yapılandır** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML varlık kimliği** ve **SAML çoklu oturum açma hizmeti** URL'den **hızlı başvuru** bölümü.

    !["Cezanne ik yazılım yapılandırma" bölümü](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_configure.png) 

7. Farklı web tarayıcısı penceresinde Cezanne ik yazılım Kiracı yönetici olarak oturum açma.

8. Sol bölmede seçin **sistem kurulumu**. Seçin **güvenlik ayarları** > **tek oturum açma yapılandırması**.

    !["Yapılandırma tek oturum açma" bağlantısı](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_000.png)

9. İçinde **kullanıcıların aşağıdaki çoklu oturum açma (SSO) Hizmetleri kullanarak oturum açmasına izin** bölmesinde, **SAML 2.0** onay kutusunu seçip alt **Gelişmiş Yapılandırma** seçeneği.

    ![Çoklu oturum açma hizmetleri seçenekleri](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_001.png)

10. Seçin **yeni Ekle**.

    !["Yeni Ekle" düğmesi](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_002.png)

11. Altında **SAML 2.0 kimlik sağlayıcısı**, aşağıdakileri yapın:

    !["SAML 2.0 kimlik sağlayıcısı" bölümü](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_003.png)
    
    a. İçinde **görünen adı** kutusuna, kimlik sağlayıcısının adını girin.

    b. İçinde **varlık tanımlayıcısı** kutusunda, yapıştırma **SAML varlık kimliği** Azure portalından kopyaladığınız. 

    c. İçinde **SAML bağlama** liste kutusunda **POST**.

    d. İçinde **güvenlik belirteci Hizmeti uç noktası** kutusunda, yapıştırma **SAML çoklu oturum açma hizmeti** Azure portalından kopyalandığından URL. 
    
    e. İçinde **kullanıcı kimliği öznitelik adı** kutusuna `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.
    
    f. Azure AD'den indirilen sertifikayı karşıya yüklemek için seçin **karşıya** düğmesi.
    
    g. **Tamam**’ı seçin. 

12. **Kaydet**’i seçin.

    !["Kaydet" düğmesi](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_004.png)

> [!TIP]
> Uygulama ayarlama gibi önceki yönergeleri kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com). Uygulamadan ekledikten sonra **Active Directory** > **kurumsal uygulamalar** bölümünde, select **çoklu oturum açma** sekmesi. Katıştırılmış belgelerinden erişim **yapılandırma** bölümü. 

Embedded belgeler özelliği hakkında daha fazla bilgi için bkz: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümde, Azure portalında test kullanıcısı Britta Simon oluşturun.

![Test kullanıcısı Britta Simon][100]

Azure AD'de bir sınama kullanıcısı oluşturmak için aşağıdakileri yapın:

1. İçinde **Azure portal**, sol bölmede seçin **Azure Active Directory** düğmesi.

    !["Azure Active Directory" düğmesi](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için seçin **kullanıcılar ve gruplar** > **tüm kullanıcılar**.
    
    !["Tüm kullanıcılar" bağlantı](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_02.png) 
    
    **Tüm kullanıcılar** iletişim kutusu açılır.

3. Açmak için **kullanıcı** iletişim kutusunda **Ekle**.
 
    !["Ekle" düğmesi](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_03.png) 

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdakileri yapın:
 
    !["Kullanıcı" iletişim kutusu](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kutusuna kullanıcı Britta Simon'ın yazın **e-posta adresi**.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından oluşturulduğu değerini not edin **parola** kutusu.

    d. **Oluştur**’u seçin.
 
### <a name="create-a-cezanne-hr-software-test-user"></a>Cezanne HR yazılım test kullanıcısı oluşturma

Azure AD Cezanne ik yazılım oturum açmalarını etkinleştirmek için bunlar Cezanne ik yazılımına sağlanmalıdır. Cezanne HR söz konusu olduğunda, sağlama el ile bir görev yazılımdır.

Aşağıdakileri yaparak bir kullanıcı hesabı sağlayın:

1.  Cezanne HR yazılım şirket sitenizin yönetici olarak oturum açın.

2.  Sol bölmede seçin **sistem kurulumu** > **kullanıcıları yönetme** > **yeni kullanıcı Ekle**.

    !["Yeni Kullanıcı Ekle" bağlantı](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_005.png "yeni kullanıcı")

3.  Altında **kişi ayrıntıları**, aşağıdakileri yapın:

    !["Kişi Ayrıntıları" bölümü](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_006.png "yeni kullanıcı")
    
    a. Ayarlama **iç kullanıcı** olarak **OFF**.
    
    b. İçinde **ad** kutusunda, kullanıcının ilk adını, örneğin, **Britta**.  
 
    c. İçinde **Soyadı** kullanıcının soyadını, örneğin, yazın **Simon**.
    
    d. İçinde **e-posta** kutusuna, örneğin, kullanıcının e-posta adresini yazın Brittasimon@contoso.com.

4.  Altında **hesap bilgilerini**, aşağıdakileri yapın:

    !["Hesap bilgisi" bölümünde](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_007.png "yeni kullanıcı")
    
    a. İçinde **kullanıcıadı** kutusuna, örneğin, kullanıcının e-posta adresini yazın Brittasimon@contoso.com.
    
    b. İçinde **parola** kullanıcının parolasını yazın.
    
    c. İçinde **güvenlik rolü** kutusunda **ik Professional**.
    
    d. **Tamam**’ı seçin.

5. Üzerinde **çoklu oturum açma** sekmesinde **SAML 2.0 tanımlayıcıları** bölümünde, select **yeni Ekle**.

    !["Yeni Ekle" düğmesi](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_008.png "kullanıcı")

6. İçinde **kimlik sağlayıcısı** liste kutusunda, kimlik sağlayıcısı seçin. İçinde **kullanıcı tanımlayıcısı** kutusunda, test kullanıcı Britta Simon'ın hesap için e-posta adresi girin.

    !["Kimlik sağlayıcısı" ve "Kullanıcı tanımlayıcısı" kutuları](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_009.png "kullanıcı")
    
7. **Kaydet**’i seçin.

    !["Kaydet" düğmesini](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_010.png "kullanıcı")

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, test kullanıcısı Britta Cezanne ik yazılım erişim vererek Azure SSO kullanılacak Simon etkinleştirin.

![Test kullanıcı erişimi][200] 

1. Azure Portalı'ndaki uygulamaları görünümünü açın ve dizin görünümüne gidin. Seçin **kurumsal uygulamalar** > **tüm uygulamaları**.

    !["Tüm uygulamaları" bağlantı][201] 

2. Uygulamalar listesinde **Cezanne ik yazılım**.

    !["Uygulamaları" listesi](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_app.png) 

3. Soldaki menüde seçin **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. **Add (Ekle)** seçeneğini belirleyin. Ardından **eklemek atama** iletişim kutusunda **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][203]

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **kullanıcılar** listesinde **Britta Simon**.

6. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **seçin**.

7. İçinde **eklemek atama** iletişim kutusunda **atamak**.
    
### <a name="test-sso"></a>Test SSO

Bu bölümde, erişim paneli kullanarak Azure AD SSO yapılandırmanızı sınayın.

Ne zaman Cezanne ik yazılım uygulamanızı otomatik olarak oturum erişim panelinde Cezanne ik yazılım döşeme seçin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve SSO ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_203.png

