---
title: "Öğretici: Azure Active Directory Tümleştirme ile Druva | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile Druva arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: ab92b600-1fea-4905-b1c7-ef8e4d8c495c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2017
ms.author: jeedes
ms.openlocfilehash: 52212c44c925598b2c19df1b20eb4e8123f974ba
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2017
---
# <a name="tutorial-azure-active-directory-integration-with-druva"></a>Öğretici: Azure Active Directory Tümleştirme Druva ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Druva tümleştirmek öğrenin.

Druva Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Druva erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak için Druva (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme Druva ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Druva çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Druva ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-druva-from-the-gallery"></a>Galeriden Druva ekleme
Azure AD Druva tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Druva eklemeniz gerekir.

**Galeriden Druva eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Druva**seçin **Druva** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde Druva](./media/active-directory-saas-druva-tutorial/tutorial_druva_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Druva sınayın.

Tekli çalışmaya oturum için Azure AD Druva karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Druva ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Druva içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Druva ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Druva test kullanıcısı oluşturma](#create-a-druva-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Druva sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Druva uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Druva yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Druva** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-druva-tutorial/tutorial_druva_samlbase.png)

3. Üzerinde **Druva etki alanı ve URL'leri** uygulamada yapılandırmak istiyorsanız, bölüm **IDP** modu tarafından başlatılan:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-druva-tutorial/tutorial_druva_url.png)

    İçinde **tanımlayıcısı** metin kutusuna, dize değeri yazın:`druva-cloud`
    
4. Denetleme **Göster Gelişmiş URL ayarları**. Uygulamada yapılandırmak istiyorsanız **SP** modu tarafından başlatılan:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-druva-tutorial/tutorial_druva_url1.png)
    
    İçinde **oturum açma URL'si** metin kutusuna, URL'yi yazın:`https://cloud.druva.com/home`

5. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-druva-tutorial/tutorial_druva_certificate.png) 

6. Özel öznitelik eşlemelerini eklemenizi gerektirir belirli bir biçimde SAML onaylar Druva uygulamanızı bekler, **SAML belirteci öznitelikleri** yapılandırma. 

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-druva-tutorial/tutorial_druva_attribute.png)

7. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, önceki görüntüde gösterildiği gibi SAML belirteci özniteliği yapılandırın ve aşağıdaki adımları gerçekleştirin:

    | Öznitelik Adı      | Öznitelik Değeri      |
    | ------------------- | -------------------- |
    | insync\_auth\_belirteci |Oluşturulan belirteç değerini girin |
    
    a. Tıklatın **Ekle özniteliği** açmak için **özniteliği eklemek** iletişim.
    
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-druva-tutorial/tutorial_attribute_04.png)
    
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-druva-tutorial/tutorial_attribute_05.png)
    
    b. İçinde **adı** metin kutusuna, ilgili satır için gösterilen öznitelik adı yazın.

    c. Gelen **değeri** listesinde, ilgili satır için gösterilen öznitelik değeri yazın. Belirteç oluşturulan değerini daha sonra öğreticide açıklanmıştır.
    
    d. **Tamam**’a tıklayın.    

8. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-druva-tutorial/tutorial_general_400.png)

9. Üzerinde **Druva yapılandırma** 'yi tıklatın **yapılandırma Druva** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-druva-tutorial/tutorial_druva_configure.png) 

10. Farklı web tarayıcısı penceresinde Druva şirket sitenize yönetici olarak oturum açın.

11. Git **yönetmek \> ayarları**.

    ![Ayarları](./media/active-directory-saas-druva-tutorial/ic795091.png "ayarları")

12. Çoklu oturum açma ayarları iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açma ayarları](./media/active-directory-saas-druva-tutorial/ic795092.png "tek oturum açma ayarları")
    
    a. İçinde **kimlik sağlayıcısı oturum açma URL'si** metin değerini yapıştırın **çoklu oturum açma hizmet URL'si**, Azure portalından kopyalanan.
        
    b. İçinde **kimlik sağlayıcısı oturum kapatma URL'si** metin değerini yapıştırın **Sign-Out URL**, Azure portalından kopyalanan
        
    c. Base-64 kodlanmış sertifikanızı Not Defteri'nde açın, içeriğini, panoya kopyalayın ve yapıştırın kendisine **kimlik sağlayıcısının sertifikasını** metin kutusu
     
    d. Açmak için **ayarları** sayfasında, **kaydetmek**.

13. Üzerinde **ayarları** sayfasında, **SSO belirteç Oluştur**.

    ![Ayarları](./media/active-directory-saas-druva-tutorial/ic795093.png "ayarları")

14. Üzerinde **tek oturum açma kimlik doğrulaması belirteci** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![SSO belirteci](./media/active-directory-saas-druva-tutorial/ic795094.png "SSO belirteci")
    
    a. Tıklatın **kopya**, Yapıştır kopyaladığınız değeri **değeri** metin kutusuna **özniteliği eklemek** Azure portalı bölümünde.
    
    b. **Kapat**’a tıklayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-druva-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-druva-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-druva-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-druva-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**'a tıklayın.
 
### <a name="create-a-druva-test-user"></a>Druva test kullanıcısı oluşturma

Azure AD kullanıcıları için Druva oturum açmak etkinleştirmek için bunların Druva sağlanmalıdır. Druva söz konusu olduğunda, sağlama bir el ile bir görevdir.

**Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Oturum, **Druva** yönetici olarak şirket site.

2. Git **yönetmek \> kullanıcılar**.
   
   ![Kullanıcıları yönetme](./media/active-directory-saas-druva-tutorial/ic795097.png "kullanıcıları yönetme")

3. Tıklatın **Yeni Oluştur**.
   
   ![Kullanıcıları yönetme](./media/active-directory-saas-druva-tutorial/ic795098.png "kullanıcıları yönetme")

4. Yeni kullanıcı oluştur iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
   ![NewUser oluşturma](./media/active-directory-saas-druva-tutorial/ic795099.png "NewUser oluşturma")
   
   a. İçinde **e-posta adresi** metin kutusuna, bir kullanıcı gibi e-posta girin  **brittasimon@contoso.com** .
   
   b. İçinde **adı** metin gibi kullanıcı adını girin **BrittaSimon**.
   
   c. Tıklatın **kullanıcı oluşturma**.

>[!NOTE]
>API Azure AD kullanıcı hesaplarını sağlamak için Druva tarafından sağlanan veya herhangi diğer Druva kullanıcı hesabı oluşturma araçlarını kullanabilirsiniz.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Druva için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Druva için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Druva**.

    ![Uygulamalar listesinde Druva bağlantı](./media/active-directory-saas-druva-tutorial/tutorial_druva_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Druva parçasında tıklattığınızda, otomatik olarak Druva uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-druva-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-druva-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-druva-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-druva-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-druva-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-druva-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-druva-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-druva-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-druva-tutorial/tutorial_general_203.png

