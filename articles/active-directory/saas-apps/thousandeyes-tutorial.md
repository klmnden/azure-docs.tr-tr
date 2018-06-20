---
title: 'Öğretici: Azure Active Directory Tümleştirme ile ThousandEyes | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile ThousandEyes arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 790e3f1e-1591-4dd6-87df-590b7bf8b4ba
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2018
ms.author: jeedes
ms.openlocfilehash: 0bbab96988c801f4078fb3a543cbcc20b781a6ec
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36218386"
---
# <a name="tutorial-azure-active-directory-integration-with-thousandeyes"></a>Öğretici: Azure Active Directory Tümleştirme ThousandEyes ile

Bu öğreticide, Azure Active Directory (Azure AD) ile ThousandEyes tümleştirmek öğrenin.

ThousandEyes Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- ThousandEyes erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için ThousandEyes (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme ThousandEyes ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir ThousandEyes çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, burada bir aylık deneme elde edebilirsiniz: [deneme teklifi](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden ThousandEyes ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-thousandeyes-from-the-gallery"></a>Galeriden ThousandEyes ekleme
Azure AD ThousandEyes tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden ThousandEyes eklemeniz gerekir.

**Galeriden ThousandEyes eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **ThousandEyes**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/thousandeyes-tutorial/tutorial_thousandeyes_search.png)

5. Sonuçlar panelinde seçin **ThousandEyes**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/thousandeyes-tutorial/tutorial_thousandeyes_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı ThousandEyes sınayın.

Tekli çalışmaya oturum için Azure AD ThousandEyes karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ThousandEyes ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

ThousandEyes içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma ThousandEyes ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[ThousandEyes test kullanıcısı oluşturma](#creating-a-thousandeyes-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı ThousandEyes sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma ThousandEyes uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile ThousandEyes yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **ThousandEyes** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açmayı yapılandırın](./media/thousandeyes-tutorial/tutorial_thousandeyes_samlbase.png)

3. Üzerinde **ThousandEyes etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/thousandeyes-tutorial/tutorial_thousandeyes_url.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL'yi yazın: `https://app.thousandeyes.com/login/sso`

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/thousandeyes-tutorial/tutorial_thousandeyes_certificate.png)

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/thousandeyes-tutorial/tutorial_general_400.png)

6. Üzerinde **ThousandEyes yapılandırma** 'yi tıklatın **yapılandırma ThousandEyes** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/thousandeyes-tutorial/tutorial_thousandeyes_configure.png) 

7. Farklı web tarayıcısı penceresinde oturum açın, **ThousandEyes** yönetici olarak şirket site.

8. Üstteki menüde tıklatın **ayarları**.

    ![Ayarları](./media/thousandeyes-tutorial/ic790066.png "ayarları")

9. Tıklatın **hesabı**

    ![Hesap](./media/thousandeyes-tutorial/ic790067.png "hesabı")

10. Tıklatın **güvenlik ve kimlik doğrulama** sekmesi.

    ![Güvenlik ve kimlik doğrulama](./media/thousandeyes-tutorial/ic790068.png "güvenlik ve kimlik doğrulama")

11. İçinde **Kurulum çoklu oturum açma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açma Kurulum](./media/thousandeyes-tutorial/ic790069.png "Kurulum çoklu oturum açma")

    a. Seçin **çoklu oturum açmayı etkinleştir**.

    b. İçinde **oturum açma sayfası URL'si** metin kutusuna, Yapıştır **SAML çoklu oturum açma hizmet URL'si**, Azure portalından kopyalanan.

    c. İçinde **oturum kapatma sayfası URL'si** metin kutusuna, Yapıştır **Sign-Out URL**, Azure portalından kopyalanan.

    d. **Kimlik sağlayıcısı veren** metin kutusuna, Yapıştır **SAML varlık kimliği**, hangi Azure portalından kopyalanır.

    e. İçinde **doğrulama sertifikası**, tıklatın **dosya**ve Azure portalından indirdiğiniz sertifikasını karşıya yükleyin.

    f. **Kaydet**’e tıklayın.

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/thousandeyes-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/thousandeyes-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/thousandeyes-tutorial/create_aaduser_03.png)

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Bir Azure AD test kullanıcısı oluşturma](./media/thousandeyes-tutorial/create_aaduser_04.png)

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.

### <a name="creating-a-thousandeyes-test-user"></a>ThousandEyes test kullanıcısı oluşturma

Bu bölümün amacı Britta Simon içinde ThousandEyes adlı bir kullanıcı oluşturmaktır. ThousandEyes otomatik kullanıcı hazırlama, varsayılan olarak etkin olduğu destekler. Daha fazla ayrıntı bulabilirsiniz [burada](thousandeyes-provisioning-tutorial.md) otomatik kullanıcı sağlamayı yapılandırma.

**Kullanıcı el ile oluşturmanız gerekiyorsa, şu adımları gerçekleştirin:**

1. ThousandEyes şirket sitenize yönetici olarak oturum açın.

2. Tıklatın **ayarları**.

    ![Ayarları](./media/thousandeyes-tutorial/IC790066.png "ayarları")

3. Tıklatın **hesap**.

    ![Hesap](./media/thousandeyes-tutorial/IC790067.png "hesabı")

4. Tıklatın **hesapları k & ullanıcıların** sekmesi.

    ![K & ullanıcıların hesapları](./media/thousandeyes-tutorial/IC790073.png "hesapları ve kullanıcılar")

5. İçinde **Kullanıcı Ekle & hesapları** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı hesapları ekleme](./media/thousandeyes-tutorial/IC790074.png "kullanıcı hesapları ekleme")

    a. İçinde **adı** metin kutusu, kullanıcı adını yazın ister **Britta Simon**.

    b. İçinde **e-posta** metin kutusu, kullanıcı e-posta türünü ister **brittasimon@contoso.com**.

    b. Tıklatın **yeni kullanıcı hesabına eklemek**.

    > [!NOTE]
    > Azure Active Directory hesap sahibi onaylayın ve hesabını etkinleştirmek için bir bağlantı içeren bir e-posta alırsınız.

> [!NOTE]
> API tarafından ThousandEyes sağlamak için Azure Active Directory kullanıcı hesapları sağlanan veya herhangi diğer ThousandEyes kullanıcı hesabı oluşturma araçlarını kullanabilirsiniz.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta ThousandEyes için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**ThousandEyes için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **ThousandEyes**.

    ![Çoklu oturum açmayı yapılandırın](./media/thousandeyes-tutorial/tutorial_thousandeyes_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli ThousandEyes parçasında tıklattığınızda, otomatik olarak ThousandEyes uygulamanıza açan.

Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)
* [Kullanıcı sağlamayı Yapılandır](thousandeyes-provisioning-tutorial.md)


<!--Image references-->

[1]: ./media/thousandeyes-tutorial/tutorial_general_01.png
[2]: ./media/thousandeyes-tutorial/tutorial_general_02.png
[3]: ./media/thousandeyes-tutorial/tutorial_general_03.png
[4]: ./media/thousandeyes-tutorial/tutorial_general_04.png

[100]: ./media/thousandeyes-tutorial/tutorial_general_100.png

[200]: ./media/thousandeyes-tutorial/tutorial_general_200.png
[201]: ./media/thousandeyes-tutorial/tutorial_general_201.png
[202]: ./media/thousandeyes-tutorial/tutorial_general_202.png
[203]: ./media/thousandeyes-tutorial/tutorial_general_203.png
