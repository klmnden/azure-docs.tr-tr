---
title: "Öğretici: Azure Active Directory Tümleştirme SCC yaşam | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory SCC yaşam döngüsü arasındaki yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9748bf38-ffc3-4d51-a1ae-207ce57104fa
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: jeedes
ms.openlocfilehash: 0f8f9d03e8c35109b74088350ef1d68f6b823e8b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-scc-lifecycle"></a>Öğretici: Azure Active Directory Tümleştirme ile SCC yaşam döngüsü

Bu öğreticide, Azure Active Directory (Azure AD) ile SCC yaşam döngüsü tümleştirmek öğrenin.

SCC yaşam döngüsü Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- SCC yaşam döngüsü erişimi, Azure AD'de kontrol edebilirsiniz
- Azure AD hesaplarına otomatik olarak (çoklu oturum açma) SCC yaşam döngüsü için açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme SCC yaşam döngüsü ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir SCC yaşam döngüsü çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, burada bir aylık deneme elde edebilirsiniz: [deneme teklifi](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden SCC yaşam döngüsü ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-scc-lifecycle-from-the-gallery"></a>Galeriden SCC yaşam döngüsü ekleme
Azure AD SCC yaşam döngüsü tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden SCC yaşam döngüsü eklemeniz gerekir.

**Galeriden SCC yaşam döngüsü eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **SCC yaşam döngüsü**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-scclifecycle-tutorial/tutorial_scclifecycle_search.png)

5. Sonuçlar panelinde seçin **SCC yaşam döngüsü**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-scclifecycle-tutorial/tutorial_scclifecycle_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama

Bu bölümde, yapılandırmanız ve SCC yaşam döngüsü ile Azure AD çoklu oturum açmayı test "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı

Tekli çalışmaya oturum için Azure AD ne karşılık gelen SCC yaşam döngüsü içinde bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının SCC yaşam döngüsü ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

SCC çevriminin değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırmak ve Azure AD çoklu oturum açma SCC yaşam sınamak için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[SCC yaşam döngüsü test kullanıcısı oluşturma](#creating-an-scc-lifecycle-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı SCC ömrü sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma SCC yaşam döngüsü uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma SCC yaşam döngüsü ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **SCC yaşam döngüsü** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-scclifecycle-tutorial/tutorial_scclifecycle_samlbase.png)

3. Üzerinde **SCC yaşam döngüsü etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-scclifecycle-tutorial/tutorial_scclifecycle_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<sub-domain>.scc.com/ic7/welcome/customer/PICTtest.aspx`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın:
    | |
    |--|--|
    | `https://bs1.scc.com/<entity>`|
    | `https://lifecycle.scc.com/<entity>`|
    
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Kişi [SCC yaşam döngüsü istemci destek ekibi](mailto:lifecycle.support@scc.com) bu değerleri almak için. 
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-scclifecycle-tutorial/tutorial_scclifecycle_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-scclifecycle-tutorial/tutorial_general_400.png)

6. Çoklu oturum açma yapılandırmak için **SCC yaşam döngüsü** yan, indirilen göndermek için ihtiyacınız **meta veri XML** için [SCC yaşam döngüsü destek ekibi](mailto:lifecycle.support@scc.com). Bunlar, her iki tarafta da ayarlamanızı SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

     >[!NOTE]
   >Çoklu oturum açma SCC yaşam döngüsü destek ekibi tarafından etkinleştirilmesi gerekir.
   > 
   > 

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-scclifecycle-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-scclifecycle-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-scclifecycle-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-scclifecycle-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın.
 
### <a name="creating-an-scc-lifecycle-test-user"></a>SCC yaşam döngüsü test kullanıcısı oluşturma

Azure AD kullanıcıların SCC yaşam döngüsü oturum etkinleştirmek için bunların SCC yaşam döngüsü sağlanmalıdır. Kullanıcı SCC yaşam döngüsü için sağlama yapılandırmanız için eylem öğe yok.

Atanmış bir kullanıcı SCC yaşam döngüsü oturum açmaya çalıştığında SCC yaşam döngüsü hesap gerekirse, otomatik olarak oluşturulur.

> [!NOTE]
    > Azure Active Directory hesap sahibi bir e-posta alır ve bunu etkinleştirilmeden önce kendi hesabı onaylamak için bir bağlantı izler.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta SCC yaşam döngüsü için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**SCC ömrü Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure Portalı'ndaki uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları.**

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **SCC yaşam döngüsü**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-scclifecycle-tutorial/tutorial_scclifecycle_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli SCC yaşam döngüsü parçasında tıklattığınızda, otomatik olarak SCC yaşam döngüsü uygulamaya açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-scclifecycle-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-scclifecycle-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-scclifecycle-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-scclifecycle-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-scclifecycle-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-scclifecycle-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-scclifecycle-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-scclifecycle-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-scclifecycle-tutorial/tutorial_general_203.png

