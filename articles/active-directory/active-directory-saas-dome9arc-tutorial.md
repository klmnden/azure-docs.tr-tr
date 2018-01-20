---
title: "Öğretici: Azure Active Directory Tümleştirme Dome9 yay ile | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile Dome9 yay arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 4c12875f-de71-40cb-b9ac-216a805334e5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/18/2018
ms.author: jeedes
ms.openlocfilehash: 2ce4bb1be8b0124c69991765e18ce9922bd2f4a4
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/20/2018
---
# <a name="tutorial-azure-active-directory-integration-with-dome9-arc"></a>Öğretici: Azure Active Directory Tümleştirme Dome9 yay ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Dome9 yay tümleştirmek öğrenin.

Dome9 yay Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Dome9 yay erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak Dome9 Yaya (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Dome9 yay yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Dome9 yay çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Dome9 yay ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-dome9-arc-from-the-gallery"></a>Galeriden Dome9 yay ekleme
Azure AD Dome9 yay tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Dome9 yay eklemeniz gerekir.

**Galeriden Dome9 yay eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Dome9 yay**seçin **Dome9 yay** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde Dome9 yay](./media/active-directory-saas-dome9arc-tutorial/tutorial_dome9arc_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırmak ve Azure AD çoklu oturum açma Dome9 "Britta Simon" adlı bir test kullanıcı tabanlı yay sınayın.

Tekli çalışmaya oturum için Azure AD Dome9 yay karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Dome9 yay ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Değeri Dome9 yay içinde atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Dome9 yay ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Dome9 yay test kullanıcısı oluşturma](#create-a-dome9-arc-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Dome9 yay sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Dome9 yay uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Dome9 yay yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Dome9 yay** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-dome9arc-tutorial/tutorial_dome9arc_samlbase.png)

3. Üzerinde **Dome9 yay etki alanı ve URL'leri** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** modu tarafından başlatılan:

    ![Dome9 yay etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-dome9arc-tutorial/tutorial_dome9arc_url.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, URL'yi yazın:`https://secure.dome9.com/`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://secure.dome9.com/sso/saml/yourcompanyname`

    > [!NOTE]
    > Öğreticide daha sonra açıklanan dome9 Yönetim Portalı'nda ve şirket adı değer seçer.

4. Denetleme **Göster Gelişmiş URL ayarları** ve uygulamada yapılandırmak istiyorsanız aşağıdaki adımı gerçekleştirin **SP** modunda başlatılan:

    ![Dome9 yay etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-dome9arc-tutorial/tutorial_dome9arc_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://secure.dome9.com/sso/saml/<yourcompanyname>`
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Kişi [Dome9 yay istemci destek ekibi](https://dome9.com/about/contact-us/) bu değerleri almak için. 

5. Dome9 yay yazılım uygulaması SAML onaylar belirli bir biçimde bekliyor. Bu uygulama için aşağıdaki talep yapılandırın. Bu öznitelik değerlerini yönetebilirsiniz "**kullanıcı öznitelikleri**" uygulama tümleştirmesi sayfasında bölüm. Aşağıdaki ekran görüntüsünde bunun bir örneği gösterir.

    ![Çoklu oturum açma attb yapılandırın](./media/active-directory-saas-dome9arc-tutorial/tutorial_dome9arc_attribute.png)

6. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, yukarıdaki resimde gösterildiği gibi SAML belirteci özniteliği yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Öznitelik Adı  | Öznitelik Değeri | 
    | --------------- | --------------- | 
    | memberof | User.assignedroles | 
    
    a. Tıklatın **Ekle özniteliği** açmak için **özniteliği eklemek** iletişim.

    ![Çoklu oturum açma yapılandırma attb Ekle](./media/active-directory-saas-dome9arc-tutorial/tutorial_dome9_04.png)

    ![Çoklu oturum açma düzenleme attb yapılandırın](./media/active-directory-saas-dome9arc-tutorial/tutorial_attribute_05.png)

    b. İçinde **adı** metin kutusuna, ilgili satır için gösterilen öznitelik adı yazın.

    c. Gelen **değeri** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.
    
    d. **Tamam**’a tıklayın.

7. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-dome9arc-tutorial/tutorial_dome9arc_certificate.png) 

8. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-dome9arc-tutorial/tutorial_general_400.png)
    
9. Üzerinde **Dome9 yay yapılandırma** 'yi tıklatın **yapılandırma Dome9 yay** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Dome9 yay yapılandırma](./media/active-directory-saas-dome9arc-tutorial/tutorial_dome9arc_configure.png) 

10. Farklı web tarayıcısı penceresinde Dome9 yay şirket sitenize yönetici olarak oturum açın.

11. Tıklayın **profil ayarları** 'ye tıklayın ve sağ üst köşesinde **hesap ayarlarını**. 

    ![Dome9 yay yapılandırma](./media/active-directory-saas-dome9arc-tutorial/configure1.png)

12. Gidin **SSO** ve ardından **etkinleştirmek**.

    ![Dome9 yay yapılandırma](./media/active-directory-saas-dome9arc-tutorial/configure2.png)

13. SSO yapılandırma bölümünde aşağıdaki adımları gerçekleştirin:

    ![Dome9 yay yapılandırma](./media/active-directory-saas-dome9arc-tutorial/configure3.png)

    a. Şirket adını girin **hesap kimliği** metin kutusu. Url Azure portal URL bölümünde belirtilen yanıt kullanılmak üzere bu değer olur.

    b. İçinde **veren** metin kutusuna, değerini yapıştırın **SAML varlık kimliği**, hangi form Azure portalı kopyalanır.

    c. İçinde **IDP uç nokta URL'si** metin kutusuna, değerini yapıştırın **SAML çoklu oturum açma hizmet URL'si**, hangi form Azure portalı kopyalanır.

    d. İndirilen Base64 ile kodlanmış sertifikanızı Not Defteri'nde açın, içeriğini, panoya kopyalayın ve yapıştırın kendisine **X.509 sertifikası** metin kutusu.

    e. **Kaydet**’e tıklayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-dome9arc-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-dome9arc-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-dome9arc-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-dome9arc-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-dome9-arc-test-user"></a>Dome9 yay test kullanıcısı oluşturma

Dome9 Yaya oturum açmak Azure AD kullanıcıları etkinleştirmek için bunlar uygulamasına sağlanmalıdır. Yalnızca zaman kaynak sağlamayı Dome9 yay destekler ancak için düzgün çalışması, kullanıcı sahip belirli seçmek **rol** ve aynı kullanıcıya atayın.

   >[!Note] 
   >İçin **rol** oluşturma ve diğer ilgili kişi ayrıntıları [Dome9 yay istemci destek ekibi](https://dome9.com/about/contact-us/).

**Bir kullanıcı hesabını el ile sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Dome9 yay şirket sitenize yönetici olarak oturum açın.

2. Tıklayın **kullanıcıları ve rolleri** ve ardından **kullanıcılar**.

    ![Çalışanı ekleyin](./media/active-directory-saas-dome9arc-tutorial/user1.png)

3. Tıklatın **Kullanıcı Ekle**.

    ![Çalışanı ekleyin](./media/active-directory-saas-dome9arc-tutorial/user2.png)

4. İçinde **kullanıcı oluştur** bölümünde, aşağıdaki adımları gerçekleştirin:
    
    ![Çalışanı ekleyin](./media/active-directory-saas-dome9arc-tutorial/user3.png)

    a. İçinde **e-posta** metin kutusu, kullanıcı e-posta türünü ister Brittasimon@contoso.com.

    b. İçinde **ad** metin kutusuna, adı Britta gibi kullanıcı türü.

    c. İçinde **Soyadı** metin kutusuna, türü kullanıcının soyadını Simon gibi.

    d. Olun **SSO kullanıcı** olarak **üzerinde**.

    e. Tıklatın **oluşturma**.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Dome9 Yaya erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Dome9 Yaya atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Dome9 yay**.

    ![Uygulamalar listesinde Dome9 yay bağlantı](./media/active-directory-saas-dome9arc-tutorial/tutorial_dome9arc_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Dome9 yay parçasında tıklattığınızda, otomatik olarak Dome9 yay uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-dome9arc-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-dome9arc-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-dome9arc-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-dome9arc-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-dome9arc-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-dome9arc-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-dome9arc-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-dome9arc-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-dome9arc-tutorial/tutorial_general_203.png

