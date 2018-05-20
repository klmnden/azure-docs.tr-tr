---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Intralinks | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile Intralinks arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 147f2bf9-166b-402e-adc4-4b19dd336883
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 6b0d6c86d6377a3d21aaa8d045d952ebe925c4bb
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-azure-active-directory-integration-with-intralinks"></a>Öğretici: Azure Active Directory Tümleştirme Intralinks ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Intralinks tümleştirmek öğrenin.

Intralinks Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Intralinks erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için Intralinks (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Intralinks ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Intralinks çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Intralinks ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-intralinks-from-the-gallery"></a>Galeriden Intralinks ekleme
Azure AD Intralinks tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Intralinks eklemeniz gerekir.

**Galeriden Intralinks eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Intralinks**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_search.png)

5. Sonuçlar panelinde seçin **Intralinks**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Intralinks sınayın.

Tekli çalışmaya oturum için Azure AD Intralinks karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Intralinks ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Intralinks içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Intralinks ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir Intralinks test kullanıcısı oluşturma](#creating-an-intralinks-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Intralinks sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Intralinks uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Intralinks yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Intralinks** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_samlbase.png)

3. Üzerinde **Intralinks etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_url.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:  `https://<company name>.Intralinks.com/?PartnerIdpId=https://sts.windows.net/<AzureADTenantID>`

    > [!NOTE] 
    > Bu değer gerçek değil. Bu değer gerçek oturum açma URL'si ile güncelleştirin. Kişi [Intralinks istemci destek ekibi](https://www.intralinks.com/contact-1) bu değeri alınamıyor. 
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-intralinks-tutorial/tutorial_general_400.png)

6. Çoklu oturum açma yapılandırmak için **Intralinks** yan, indirilen göndermek için ihtiyacınız **meta veri XML** [Intralinks destek ekibi](https://www.intralinks.com/contact-1). Bunlar, her iki tarafta da ayarlamanızı SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-intralinks-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-intralinks-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-intralinks-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-intralinks-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-an-intralinks-test-user"></a>Bir Intralinks test kullanıcısı oluşturma

Bu bölümde, Intralinks içinde Britta Simon adlı bir kullanıcı oluşturun. Lütfen çalışmak [Intralinks destek ekibi](https://www.intralinks.com/contact-1) Intralinks platform kullanıcıları eklemek için.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta Intralinks için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Intralinks için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Intralinks**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.

### <a name="add-intralinks-via-or-elite-application"></a>Intralinks aracılığıyla veya Elite uygulama ekleme

Intralinks aynı SSO kimlik platformu anlaşma Nexus uygulama hariç olmak üzere diğer tüm Intralinks uygulamalar için kullanır. Başka bir Intralinks uygulama kullanmayı planlıyorsanız, bu nedenle daha sonra ilk, SSO yukarıda açıklanan yordamı kullanarak birincil Intralinks uygulama için yapılandırmanız gerekir.

Bundan sonra takip ettiğiniz başka bir Intralinks uygulamaya SSO için birincil bu uygulama yararlanabilirsiniz, Kiracı eklemek için yordam aşağıda. 

>[!NOTE]
>Bu özellik yalnızca Azure AD Premium SKU müşteriler için kullanılabilir ve ücretsiz veya temel SKU müşteriler için kullanılamaz.

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]


2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Intralinks**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_search.png)

5. Üzerinde **uygulama Intralinks Ekle** aşağıdaki adımları gerçekleştirin:

    ![Intralinks aracılığıyla veya Elite uygulama ekleme](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_addapp.png)

    a. İçinde **adı** metin kutusuna, uygun uygulama örneğin adını **Intralinks Elite**.

    b. Tıklatın **Ekle** düğmesi.

6.  Azure portalında üzerinde **Intralinks** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

7. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **bağlantılı oturum açma**.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_linkedsignon.png)

8. Alma SP tarafından başlatılan SSO'yu URL'den [Intralinks takım](https://www.intralinks.com/contact-1) diğer Intralinks uygulama için ve içinde girin **yapılandırma oturum açma URL'si** aşağıda gösterildiği gibi. 
    
     ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_customappurl.png)
    
     Oturum üzerinde URL'si metin kutusuna, kullanıcılarınıza oturum açma şu biçimi kullanarak Intralinks uygulamanız tarafından kullanılan URL'yi yazın:
   
    `https://<company name>.Intralinks.com/?PartnerIdpId=https://sts.windows.net/<AzureADTenantID>`

9. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-intralinks-tutorial/tutorial_general_400.png)

10. Kullanıcı veya gruplara uygulama bölümünde gösterildiği gibi atama  **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**.

### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Intralinks parçasında tıklattığınızda, otomatik olarak Intralinks uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)


<!--Image references-->

[1]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_203.png

