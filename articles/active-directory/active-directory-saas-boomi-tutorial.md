---
title: "Öğretici: Azure Active Directory Tümleştirme ile Boomi | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile Boomi arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8e05afa9-2eda-4975-a0cc-6d408065860f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: 1121d22beddf73fd2109a4b410422f76dd37478e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-boomi"></a>Öğretici: Azure Active Directory Tümleştirme Boomi ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Boomi tümleştirmek öğrenin.

Boomi Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Boomi erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için Boomi (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme Boomi ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Boomi çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Boomi ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-boomi-from-the-gallery"></a>Galeriden Boomi ekleme
Azure AD Boomi tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Boomi eklemeniz gerekir.

**Galeriden Boomi eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Boomi**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_search.png)

5. Sonuçlar panelinde seçin **Boomi**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Boomi sınayın.

Tekli çalışmaya oturum için Azure AD Boomi karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Boomi ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Boomi içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Boomi ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Boomi test kullanıcısı oluşturma](#creating-a-boomi-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Boomi sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Boomi uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Boomi yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Boomi** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_samlbase.png)

3. Üzerinde **Boomi etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_url.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://platform.boomi.com/sso/<accountname>/saml`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://platform.boomi.com/sso/<accountname>/saml`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı ve yanıt URL'si ile güncelleştirin. Kişi [Boomi destek ekibi](https://boomi.com/company/contact/) bu değerleri almak için.

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.
    
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_certificate.png)

4. Boomi uygulaması SAML onaylar belirli bir biçimde bekliyor. Lütfen bu uygulama için aşağıdaki talep yapılandırın. Bu öznitelik değerlerini yönetebilirsiniz "**kullanıcı öznitelikleri**" uygulama tümleştirmesi sayfasında bölüm. Aşağıdaki ekran görüntüsünde bunun bir örneği gösterir.
    
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-boomi-tutorial/tutorial_attribute.png)

5. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, aşağıdaki tabloda gösterilen her satır için aşağıdaki adımları gerçekleştirin:

    | Öznitelik adı | Öznitelik değeri |
    | -------------- | --------------- |
    | FEDERATION_ID | User.Mail |
    
    a. Tıklatın **Ekle özniteliği** açmak için **özniteliği eklemek** iletişim.
    
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-boomi-tutorial/tutorial_attribute_04.png)
    
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-boomi-tutorial/tutorial_attribute_05.png)
    
    b. İçinde **adı** metin kutusuna, ilgili satır için gösterilen öznitelik adı yazın.
    
    c. Gelen **değeri** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.
    
    d. **Tamam**’a tıklayın.

6. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-boomi-tutorial/tutorial_general_400.png)

7. Üzerinde **Boomi yapılandırma** 'yi tıklatın **yapılandırma Boomi** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_configure.png) 

8. Farklı web tarayıcısı penceresinde Boomi şirket sitenize yönetici olarak oturum açın. 

9. Gidin **şirket adı** ve Git **ayarlanan**.

10. Tıklatın **SSO seçenekleri** sekmesinde ve aşağıdaki adımları gerçekleştirin.

    ![Çoklu oturum açma uygulama tarafında yapılandırma](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_11.png)

    a. Denetleme **etkinleştirmek SAML çoklu oturum açma** onay kutusu.

    b. Tıklatın **alma** için Azure AD'den indirilen sertifikayı karşıya yüklemek için **kimlik sağlayıcısı sertifikası**.
    
    c. İçinde **kimlik sağlayıcısı oturum açma URL'si** metin kutusuna, put değerini **SAML çoklu oturum açma hizmet URL'si** Azure AD uygulama yapılandırma penceresinden.

    d. Olarak **Federasyon kimliği konumu**seçin **Federasyon kimliğidir FEDERATION_ID özniteliği öğesinde** radyo düğmesi. 

    e. Tıklatın **kaydetmek** düğmesi.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-boomi-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-boomi-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-boomi-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-boomi-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın.
 
### <a name="creating-a-boomi-test-user"></a>Boomi test kullanıcısı oluşturma

Azure AD kullanıcıları için Boomi oturum açmak etkinleştirmek için bunların Boomi sağlanmalıdır. Boomi söz konusu olduğunda, sağlama bir el ile bir görevdir.

### <a name="to-provision-a-user-account-perform-the-following-steps"></a>Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:

1. Boomi şirket sitenize yönetici olarak oturum açın.

2. Oturum açtıktan sonra gidin **kullanıcı yönetimi** ve Git **kullanıcılar**.

    ![Kullanıcıların](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_001.png "kullanıcılar")

3. Tıklatın  **+**  simgesini ve **Ekle/Koru kullanıcı rolleri** iletişim kutusunu açar.

    ![Kullanıcıların](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_002.png "kullanıcılar")

    ![Kullanıcıların](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_003.png "kullanıcılar")

    a. İçinde **kullanıcı e-posta adresi** metin kutusu, kullanıcı e-posta türünü ister BrittaSimon@contoso.com.
    
    b. İçinde **ad** metin kutusu, kullanıcı Britta gibi ilk türünün adı.

    c. İçinde **Soyadı** metin kutusuna, son kullanıcı Simon gibi yazın.
    
    d. Kullanıcının girin **Federasyon kimliği**. Her kullanıcının kullanıcı hesabı içinde benzersiz olarak tanıtan bir Federasyon kimliği olmalıdır.
    
    e. Ata **standart kullanıcı** kullanıcı rolüne. Yönetici rolü, kendisine normal Atmosfer erişim yanı sıra tek oturum açma erişimini vereceği için atamayın.
    
    f. **Tamam** düğmesine tıklayın.
    
    > [!NOTE]
    > Kullanıcı parolasını kimlik sağlayıcısı olarak yönetildiğinden AtomSphere hesabında oturum açma için kullanılan bir parola içeren bir Hoş Geldiniz bildirim e-posta alırsınız. API sağlama AAD kullanıcı hesaplarına Boomi tarafından sağlanan veya herhangi diğer Boomi kullanıcı hesabı oluşturma araçlarını kullanabilir. 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta Boomi için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Boomi için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Boomi**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Boomi parçasında tıklattığınızda, otomatik olarak Boomi uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_203.png

