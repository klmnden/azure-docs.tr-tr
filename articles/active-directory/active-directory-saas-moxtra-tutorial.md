---
title: "Öğretici: Azure Active Directory Tümleştirme ile Moxtra | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile Moxtra arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 2aed2d4b-1dcd-4839-8fed-9419d107c61c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.openlocfilehash: db2f041a44b6771b0a4f734e58d899298ef0847b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-moxtra"></a>Öğretici: Azure Active Directory Tümleştirme Moxtra ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Moxtra tümleştirmek öğrenin.

Moxtra Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Moxtra erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için Moxtra (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme Moxtra ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Moxtra çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Moxtra ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-moxtra-from-the-gallery"></a>Galeriden Moxtra ekleme
Azure AD Moxtra tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Moxtra eklemeniz gerekir.

**Galeriden Moxtra eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Moxtra**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_search.png)

5. Sonuçlar panelinde seçin **Moxtra**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Moxtra sınayın.

Tekli çalışmaya oturum için Azure AD Moxtra karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Moxtra ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Moxtra içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Moxtra ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Moxtra test kullanıcısı oluşturma](#creating-a-moxtra-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Moxtra sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Moxtra uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Moxtra yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Moxtra** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_samlbase.png)

3. Üzerinde **Moxtra etki alanı ve URL'leri** bölümünde, aşağıdaki adımı gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_url.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL'yi yazın:`https://www.moxtra.com/service/#login`

4. Moxtra uygulaması SAML onaylar belirli bir biçimde bekliyor. Bu uygulama için aşağıdaki talep yapılandırın. Bu öznitelik değerlerini yönetebilirsiniz "**kullanıcı öznitelikleri**" uygulama tümleştirmesi sayfasında bölüm. Aşağıdaki ekran görüntüsünde, bu yapılandırma için bir örneği gösterir. 

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_attributes.png)
    
5. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, SAML belirteci özniteliği görüntüde gösterildiği gibi yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Öznitelik adı | Öznitelik değeri |
    | ------------------- | -------------------- |    
    | FirstName | User.givenName |
    | Soyadı | User.surname |
    | idpid    | < SAML varlık kimliği > 

    > [!Note]
    > Değeri **idpid** özniteliği gerçek değil. Gerçek değerini alabilir **hızlı başvuru** altında bölümünde **Moxtra yapılandırma**.
    
    a. Tıklatın **Ekle özniteliği** açmak için **özniteliği eklemek** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-moxtra-tutorial/tutorial_attribute_04.png)

    b. İçinde **adı** metin kutusuna, ilgili satır için gösterilen öznitelik adı yazın.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-moxtra-tutorial/tutorial_attribute_05.png)

    c. Gelen **değeri** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    d. **Tamam**’a tıklayın.
    
5. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_certificate.png) 

6. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-moxtra-tutorial/tutorial_general_400.png)

7. Üzerinde **Moxtra yapılandırma** 'yi tıklatın **yapılandırma Moxtra** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_configure.png) 

8. Başka bir tarayıcı penceresinde Moxtra şirket sitenize yönetici olarak oturum açma.

9. Sol taraftaki araç çubuğunda tıklatın **Yönetici Konsolu > SAML çoklu oturum açma**ve ardından **yeni**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_06.png) 

10. Üzerinde **SAML** sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_08.png)   
 
    a. İçinde **adı** metin kutusuna, yapılandırmanız için bir ad yazın (örneğin: *SAML*). 
  
    b. İçinde **IDP varlık kimliği** metin değerini yapıştırın **SAML varlık kimliği** Azure portalından kopyalanan. 
 
    c. İçinde **oturum açma URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmet URL'si** Azure portalından kopyalanan. 
 
    d. İçinde **AuthnContextClassRef** metin kutusuna, türü **urn: OASIS: adları: tc: SAML:2.0:ac:classes:Password**. 
 
    e. İçinde **NameID biçimi** metin kutusuna, türü **urn: OASIS: adları: tc: SAML:1.1:nameid-biçimi: emailAddress**. 
 
    f. Not Defteri'nde, Azure Portalı'ndan indirilen açık sertifika içeriği Kopyala ve ardından yapıştırın **sertifika** metin kutusu.    
 
    g. SAML e-posta etki alanı metin kutusuna, SAML e-posta etki alanı yazın.    
  
    >[!NOTE]
    >Etki alanı doğrulama adımlarını görmek için tıklatın "**ı**" altında.

    h. Tıklatın **güncelleştirme**.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-moxtra-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-moxtra-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-moxtra-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-moxtra-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın.
 
### <a name="creating-a-moxtra-test-user"></a>Moxtra test kullanıcısı oluşturma

Bu bölümün amacı Britta Simon içinde Moxtra adlı bir kullanıcı oluşturmaktır.

**İçinde Moxtra Britta Simon adlı bir kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Moxtra şirket sitenize yönetici olarak oturum açma.

2. Sol taraftaki araç çubuğunda tıklatın **Yönetici Konsolu > Kullanıcı Yönetimi**ve ardından **Kullanıcı Ekle**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_10.png) 

3. Üzerinde **Kullanıcı Ekle** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
  
    a. İçinde **ad** metin kutusuna, türü **Britta**.
  
    b. İçinde **Soyadı** metin kutusuna, türü **Simon**.
  
    c. İçinde **e-posta** metin kutusuna, Britta'nın e-posta adresi ile aynı Azure Portal'da türü.
  
    d. İçinde **bölme** metin kutusuna, türü **geliştirme**.
  
    e. İçinde **departmanı** metin kutusuna, türü **BT**.
  
    f. Seçin **yönetici**.
  
    g. **Ekle**'ye tıklayın.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta Moxtra için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Moxtra için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Moxtra**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Moxtra parçasında tıklattığınızda, otomatik olarak Moxtra uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_203.png

