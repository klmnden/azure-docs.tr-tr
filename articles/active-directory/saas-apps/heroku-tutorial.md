---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Heroku | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile Heroku arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: d7d72ec6-4a60-4524-8634-26d8fbbcc833
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: a3ce04b090f53222a4cb58714afea418d39730d7
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36225856"
---
# <a name="tutorial-azure-active-directory-integration-with-heroku"></a>Öğretici: Azure Active Directory Tümleştirme Heroku ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Heroku tümleştirmek öğrenin.

Heroku Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Heroku erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için Heroku (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Heroku ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Heroku çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Heroku ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-heroku-from-the-gallery"></a>Galeriden Heroku ekleme
Azure AD Heroku tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Heroku eklemeniz gerekir.

**Galeriden Heroku eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Heroku**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/heroku-tutorial/tutorial_heroku_search.png)

5. Sonuçlar panelinde seçin **Heroku**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/heroku-tutorial/tutorial_heroku_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı Heroku ile test etme

Tekli çalışmaya oturum için Azure AD Heroku karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Heroku ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Heroku içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Heroku ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Heroku test kullanıcısı oluşturma](#creating-a-heroku-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Heroku sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Heroku uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Heroku yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Heroku** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/heroku-tutorial/tutorial_heroku_samlbase.png)

3. Üzerinde **Heroku etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/heroku-tutorial/tutorial_heroku_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:    
    `https://sso.heroku.com/saml/<company-name>/init`

    b. İçinde **tanımlayıcı URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:            
    `https://sso.heroku.com/saml/<company-name>`

    > [!NOTE]
    >Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Bu makalenin sonraki bölümlerinde açıklanan Heroku ekibinden bu değerleri alır. 
        
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/heroku-tutorial/tutorial_heroku_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/heroku-tutorial/tutorial_general_400.png)

6. Heroku SSO'yu etkinleştirmek için aşağıdaki adımları gerçekleştirin:
   
    a. Heroku hesabı yönetici olarak oturum açın.

    b. **Ayarlar** sekmesine tıklayın.

    c. Üzerinde **tek oturum açma sayfasına**, tıklatın **meta veriler karşıya**.

    d. Azure Portalı'ndan indirilen meta veri dosyasını karşıya yükleyin.

    e. Kurulum başarılı olduğunda, Yöneticiler bir onay iletişim kutusu bakın ve son kullanıcılar için SSO oturum açma URL'sini görüntülenir. 

    f. Kopya **Heroku oturum açma URL'si** ve **Heroku varlık kimliği** değerleri ve geri dönüp **Heroku etki alanı ve URL'leri** bölümünde Azure portalında ve bu değerleri içine yapıştırma  **Oturum açma URL'si** ve **tanımlayıcısı** kutularındaki sırasıyla.

    ![Çoklu oturum açmayı yapılandırın](./media/heroku-tutorial/tutorial_heroku_52.png) 
    
8. **İleri**’ye tıklayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory kuruluş uygulamaları** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir  **Yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/heroku-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/heroku-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/heroku-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/heroku-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-heroku-test-user"></a>Heroku test kullanıcısı oluşturma

Bu bölümde, Heroku içinde Britta Simon adlı bir kullanıcı oluşturun. Heroku yalnızca zaman sağlama, varsayılan olarak etkin olduğu destekler.

Bu bölümde, eylem öğe yok. Yeni bir kullanıcı, kullanıcı henüz yoksa Heroku erişirken oluşturulur. Hesabı sağlandıktan sonra son kullanıcı bir doğrulama e-posta alır ve bildirim bağlantısına tıklayın gerekiyor.

>[!NOTE]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, başvurmanız gerekir [Heroku istemci destek ekibi](https://www.heroku.com/support).
>  

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta Heroku için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Heroku için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Heroku**.

    ![Çoklu oturum açmayı yapılandırın](./media/heroku-tutorial/tutorial_heroku_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Heroku parçasında tıklattığınızda, otomatik olarak Heroku uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/heroku-tutorial/tutorial_general_01.png
[2]: ./media/heroku-tutorial/tutorial_general_02.png
[3]: ./media/heroku-tutorial/tutorial_general_03.png
[4]: ./media/heroku-tutorial/tutorial_general_04.png

[100]: ./media/heroku-tutorial/tutorial_general_100.png

[200]: ./media/heroku-tutorial/tutorial_general_200.png
[201]: ./media/heroku-tutorial/tutorial_general_201.png
[202]: ./media/heroku-tutorial/tutorial_general_202.png
[203]: ./media/heroku-tutorial/tutorial_general_203.png
