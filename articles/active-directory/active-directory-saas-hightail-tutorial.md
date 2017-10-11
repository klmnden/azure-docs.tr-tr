---
title: "Öğretici: Azure Active Directory Tümleştirme ile Hightail | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile Hightail arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e15206ac-74b0-46e4-9329-892c7d242ec0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2017
ms.author: jeedes
ms.openlocfilehash: ba55f9b62d274aa3eb91723c62b53f54de0891b5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-hightail"></a>Öğretici: Azure Active Directory Tümleştirme Hightail ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Hightail tümleştirmek öğrenin.

Hightail Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Hightail erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için Hightail (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme Hightail ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Hightail çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Hightail ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-hightail-from-the-gallery"></a>Galeriden Hightail ekleme
Azure AD Hightail tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Hightail eklemeniz gerekir.

**Galeriden Hightail eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Hightail**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_search.png)

5. Sonuçlar panelinde seçin **Hightail**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Hightail sınayın.

Tekli çalışmaya oturum için Azure AD Hightail karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Hightail ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Hightail içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Hightail ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Hightail test kullanıcısı oluşturma](#creating-a-hightail-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Hightail sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Hightail uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Hightail yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Hightail** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_samlbase.png)

3. Üzerinde **Hightail etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_url.png)

     İçinde **yanıt URL'si** metin olarak URL'yi yazın:`https://www.hightail.com/samlLogin?phi_action=app/samlLogin&subAction=handleSamlResponse`

    > [!NOTE] 
    > Önceki değerin gerçek değeri değil. Değer, gerçek yanıt, öğreticide daha sonra açıklanan URL ile güncelleştirir.
 
4. Üzerinde **Hightail etki alanı ve URL'leri** uygulamada yapılandırmak istiyorsanız, bölüm **SP tarafından başlatılan modu**, aşağıdaki adımları gerçekleştirin:
    
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_url1.png)

    a. Tıklatın **Göster Gelişmiş URL ayarları**.

    b. İçinde **oturum üzerinde URL'si** metin olarak URL'yi yazın:`https://www.hightail.com/loginSSO`

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_certificate.png) 

5. Hightail uygulama belirli bir biçimde SAML onaylar bekler. Lütfen bu uygulama için aşağıdaki talep yapılandırın. Bu öznitelik değerlerini yönetebilirsiniz **"Atrribute"** uygulama sekmesinde. Aşağıdaki ekran görüntüsünde bunun bir örneği gösterir. 

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_attribute.png) 

6. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, SAML belirteci özniteliği görüntüde gösterildiği gibi yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Öznitelik adı | Öznitelik değeri |
    | ------------------- | -------------------- |
    | FirstName | User.givenName |
    | Soyadı | User.surname |
    | E-posta | User.Mail |    
    | Userıdentity | User.Mail |
    
    a. Tıklatın **Ekle özniteliği** açmak için **özniteliği eklemek** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-hightail-tutorial/tutorial_officespace_04.png)

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-hightail-tutorial/tutorial_officespace_05.png)

    b. İçinde **adı** metin kutusuna, ilgili satır için gösterilen öznitelik adı yazın.

    c. Gelen **değeri** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    d. Bırakın **Namespace** boş.
    
    e. **Tamam**’a tıklayın.

7. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-hightail-tutorial/tutorial_general_400.png)

8. Üzerinde **Hightail yapılandırma** 'yi tıklatın **yapılandırma Hightail** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_configure.png) 

    >[!NOTE] 
    >Çoklu oturum açma Hightail uygulamaya yapılandırmadan önce lütfen beyaz liste Hightail ile e-posta etki alanınızı team böylece bu etki alanını kullanan tüm kullanıcılar tekli oturum açma işlevini kullanabilirsiniz.


9. Uygulamanız için yapılandırılmış SSO almak için Hightail kiracınız yönetici olarak oturum gerekir.
   
    a. Üstteki menüde tıklatın **hesap** sekmesinde ve seçin **yapılandırma SAML**.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_001.png) 

    b. Onay kutusunu işaretleyin **SAML kimlik doğrulamasını etkinleştir**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_002.png) 

    c. Base-64 kodlanmış sertifikanızı Azure portalından indirdiğiniz Not Defteri'nde açın, içeriğini, panoya kopyalayın ve yapıştırın kendisine **SAML belirteç imzalama sertifikası** metin kutusu.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_003.png) 

    d. İçinde **SAML yetkilisi (Kimlik sağlayıcısı)** metin kutusuna, değerini yapıştırın **SAML çoklu oturum açma hizmet URL'si** Azure portalından kopyalanır.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_004.png)

    e. Uygulamada yapılandırmak istiyorsanız **IDP başlatılan modu** seçin **"Kimlik sağlayıcıyı (IDP) başlatılan oturum açma"**. Varsa **SP tarafından başlatılan modu** seçin **"Hizmet sağlayıcısı (SP) başlatılan oturum açma"**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_006.png)

    f. Örneğiniz için SAML tüketici URL'sini kopyalayın ve yapıştırın **yanıt URL'si** metin kutusuna **Hightail etki alanı ve URL'leri** Azure Portal'daki bölümü.
    
    g. **Kaydet** düğmesine tıklayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-hightail-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-hightail-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-hightail-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-hightail-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın.
 
### <a name="creating-a-hightail-test-user"></a>Hightail test kullanıcısı oluşturma

Bu bölümün amacı Britta Simon içinde Hightail adlı bir kullanıcı oluşturmaktır. 

Bu bölümde, eylem öğe yok. Yalnızca zaman kullanıcı hazırlama özel taleplere dayanarak destekler hightail. Özel talep bölümünde gösterildiği gibi yapılandırdıysanız  **[yapılandırma Azure AD çoklu oturum açma](#configuring-azure-ad-single-sign-on)**  yukarıdaki kullanıcı henüz yok uygulamada otomatik olarak oluşturulur. 

>[!NOTE]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, başvurmanız gerekir [Hightail destek ekibi](mailto:support@hightail.com). 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta Hightail için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Hightail için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Hightail**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümün amacı erişim paneli kullanılarak Azure AD çoklu oturum açma yapılandırmanızı test etmektir.

Erişim paneli Hightail parçasında tıklattığınızda, otomatik olarak Hightail uygulamanıza açan.


## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_203.png

