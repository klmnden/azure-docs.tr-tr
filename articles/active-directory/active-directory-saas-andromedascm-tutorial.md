---
title: "Öğretici: Azure Active Directory Tümleştirme Andromeda SCM ile | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile Andromeda SCM arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 7a142c86-ca0c-4915-b1d8-124c08c3e3d8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2018
ms.author: jeedes
ms.openlocfilehash: 72b66eec34995c334c6d65a1d03637fe21b9dc80
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="tutorial-azure-active-directory-integration-with-andromeda-scm"></a>Öğretici: Azure Active Directory Tümleştirme Andromeda SCM ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Andromeda SCM tümleştirmek öğrenin.

Andromeda SCM Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Andromeda SCM erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak Andromeda SCM (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Andromeda SCM ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Andromeda SCM çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Andromeda SCM ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-andromeda-scm-from-the-gallery"></a>Galeriden Andromeda SCM ekleme
Azure AD Andromeda SCM tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Andromeda SCM eklemeniz gerekir.

**Galeriden Andromeda SCM eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Andromeda SCM**seçin **Andromeda SCM** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde Andromeda SCM](./media/active-directory-saas-andromedascm-tutorial/tutorial_andromedascm_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Andromeda "Britta Simon" adlı bir test kullanıcı tabanlı SCM ile test etme.

Tekli çalışmaya oturum için Azure AD Andromeda SCM karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Andromeda SCM ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Andromeda SCM ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir Andromeda SCM test kullanıcısı oluşturma](#create-an-andromeda-scm-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Andromeda SCM sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Andromeda SCM uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Andromeda SCM ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Andromeda SCM** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-andromedascm-tutorial/tutorial_andromedascm_samlbase.png)

3. Üzerinde **Andromeda SCM etki alanı ve URL'leri** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** modu tarafından başlatılan:

    ![Andromeda SCM etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-andromedascm-tutorial/tutorial_andromedascm_url.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<tenantURL>`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<tenantURL>`

4. Denetleme **Göster Gelişmiş URL ayarları** ve uygulamada yapılandırmak istiyorsanız aşağıdaki adımı gerçekleştirin **SP** modunda başlatılan:

    ![Andromeda SCM etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-andromedascm-tutorial/tutorial_andromedascm_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<tenantURL>/SAMLLogon.aspx`
     
    > [!NOTE] 
    > Önceki değerin gerçek değeri değil. Değer, gerçek tanımlayıcısı, yanıt URL'si ve oturum açma, öğreticide daha sonra açıklanan URL'si ile güncelleştirir.

5. Andromeda SCM uygulaması SAML onaylar belirli bir biçimde bekliyor. Bu uygulama için aşağıdaki talep yapılandırın. Bu öznitelik değerlerini yönetebilirsiniz **kullanıcı öznitelikleri** uygulama tümleştirmesi sayfasında bölüm. Aşağıdaki ekran görüntüsünde bunun bir örneği gösterir.
    
    ![Çoklu oturum açma attb yapılandırın](./media/active-directory-saas-andromedascm-tutorial/tutorial_andromedascm_attribute.png)

    > [!Important]
    > Bunlar kurma sırasında temizleyin ad alanı tanımları çıkışı.
    
6. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, SAML belirteci özniteliği görüntüde gösterildiği gibi yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Öznitelik Adı | Öznitelik Değeri |
    | ------------------- | -------------------- |    
    | rol        | DEMO |
    | type        | VARSAYILAN |
    | Şirket       | COMP02    |

    > [!NOTE]
    > Gerçek değer değildir. Bu değerleri yalnızca Tanıtım amaçlı için lütfen prosedürlerini rollerinizi kullanın.

    a. Tıklatın **Ekle özniteliği** açmak için **özniteliği eklemek** iletişim.

    ![Çoklu oturum açma yapılandırma ekleme](./media/active-directory-saas-andromedascm-tutorial/tutorial_attribute_04.png)

    ![Çoklu oturum açma Addattb yapılandırın](./media/active-directory-saas-andromedascm-tutorial/tutorial_attribute_05.png)

    b. İçinde **adı** metin kutusuna, ilgili satır için gösterilen öznitelik adı yazın.

    c. Gelen **değeri** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    d. Bırakın **Namespace** boş.
    
    e. **Tamam**’a tıklayın.

7. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-andromedascm-tutorial/tutorial_andromedascm_certificate.png) 

8. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-andromedascm-tutorial/tutorial_general_400.png)
    
9. Üzerinde **Andromeda SCM yapılandırma** 'yi tıklatın **yapılandırma Andromeda SCM** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Andromeda SCM yapılandırma](./media/active-directory-saas-andromedascm-tutorial/tutorial_andromedascm_configure.png)

10. Andromeda SCM şirket sitenize yönetici olarak oturum.

11. Menubar üst kısmında tıklatın **yönetici** gidin **Yönetim**.

    ![Andromeda SCM yönetici](./media/active-directory-saas-andromedascm-tutorial/tutorial_andromedascm_admin.png)

12. Araç çubuğunun altında sol tarafındaki **arabirimleri** 'yi tıklatın **SAML Yapılandırması**.

    ![Andromeda SCM saml](./media/active-directory-saas-andromedascm-tutorial/tutorial_andromedascm_saml.png)

13. Üzerinde **SAML Yapılandırması** bölümünde sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Andromeda SCM yapılandırma](./media/active-directory-saas-andromedascm-tutorial/tutorial_andromedascm_config.png)

    a. Denetleme **etkinleştirmek SAML SSO'su**.

    b. Altında **Andromeda bilgi** bölümünde, kopyalama **SP kimlik** değer ve yapıştırın **tanımlayıcısı** , metin kutusuna **Andromeda SCM etki alanı ve URL'leri** bölümü.

    c. Kopya **tüketici URL** değer ve yapıştırın **yanıt URL'si** , metin kutusuna **Andromeda SCM etki alanı ve URL'leri** bölümü.

    d. Kopya **oturum açma URL'si** değer ve yapıştırın **oturum açma URL'si** , metin kutusuna **Andromeda SCM etki alanı ve URL'leri** bölümü.

    e. Altında **SAML kimlik sağlayıcısı** bölümünde, IDP adınızı yazın.

    f. İçinde **tek oturum üzerinde uç noktası** metin kutusuna, değerini yapıştırın **SAML çoklu oturum açma hizmet URL'si** hangi Azure portalından kopyalanır.

    g. İndirilen açmak **Base64 ile kodlanmış sertifika** Not Defteri'nde Azure portalından yapıştırın **X 509 Sertifika** metin kutusu.
    
    h. Aşağıdaki öznitelikler Azure AD'den SSO oturum açma kolaylaştırmak için karşılık gelen değerle eşleyin. **Kullanıcı kimliği** oturum açma için özniteliği gereklidir. Sağlama, **e-posta**, **şirket**, **UserType** ve **rol** gereklidir. Bu bölümde biz ilişkilendirmek eşleme öznitelikleri (ad ve değerler) için Azure portalı içinde tanımlanan tanımlayın

    ![Andromeda SCM attbmap](./media/active-directory-saas-andromedascm-tutorial/tutorial_andromedascm_attbmap.png)

    i. **Kaydet**’e tıklayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-andromedascm-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-andromedascm-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-andromedascm-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-andromedascm-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-an-andromeda-scm-test-user"></a>Bir Andromeda SCM test kullanıcısı oluşturma

Bu bölümün amacı Britta Simon Andromeda SCM adlı bir kullanıcı oluşturmaktır. Andromeda SCM yalnızca zaman sağlama, varsayılan olarak etkin olduğu destekler. Bu bölümde, eylem öğe yok. Yeni bir kullanıcı henüz yoksa Andromeda SCM erişme denemesi sırasında oluşturulur.

>[!Note]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [Andromeda SCM istemci destek ekibi](https://www.ngcsoftware.com/support/).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Andromeda SCM erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Andromeda SCM atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Andromeda SCM**.

    ![Uygulamalar listesinde Andromeda SCM bağlantı](./media/active-directory-saas-andromedascm-tutorial/tutorial_andromedascm_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Andromeda SCM parçasında tıklattığınızda, otomatik olarak Andromeda SCM uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-andromedascm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-andromedascm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-andromedascm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-andromedascm-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-andromedascm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-andromedascm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-andromedascm-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-andromedascm-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-andromedascm-tutorial/tutorial_general_203.png

