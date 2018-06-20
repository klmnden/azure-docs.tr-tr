---
title: 'Öğretici: Azure Active Directory Tümleştirme ile FM:Systems | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile FM:Systems arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: f78c58c5-6e98-458b-8991-78624a245665
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: jeedes
ms.openlocfilehash: d089e35e28c466f91c550a41898731f683bf5033
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36216345"
---
# <a name="tutorial-azure-active-directory-integration-with-fmsystems"></a>Öğretici: Azure Active Directory Tümleştirme FM:Systems ile

Bu öğreticide, Azure Active Directory (Azure AD) ile FM:Systems tümleştirmek öğrenin.

FM:Systems Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- FM:Systems erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak Azure AD hesaplarına sahip (çoklu oturum açma) FM:Systems için açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme FM:Systems ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir FM:Systems çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden FM:Systems ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-fmsystems-from-the-gallery"></a>Galeriden FM:Systems ekleme
Azure AD FM:Systems tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden FM:Systems eklemeniz gerekir.

**Galeriden FM:Systems eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **FM:Systems**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/fm-systems-tutorial/tutorial_fmsystems_search.png)

5. Sonuçlar panelinde seçin **FM:Systems**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/fm-systems-tutorial/tutorial_fmsystems_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı FM:Systems sınayın.

Tekli çalışmaya oturum için Azure AD FM:Systems karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının FM:Systems ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

FM:Systems içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma FM:Systems ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir FM:Systems test kullanıcısı oluşturma](#creating-an-fmsystems-test-user)**  - bir Britta Simon karşılık gelen kullanıcı Azure AD gösterimini bağlı FM:Systems içinde olması.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma FM:Systems uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile FM:Systems yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **FM:Systems** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/fm-systems-tutorial/tutorial_fmsystems_samlbase.png)

3. Üzerinde **FM:Systems etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/fm-systems-tutorial/tutorial_fmsystems_url.png)

    İçinde **yanıt URL'si** metin, FM:Systems yazın **yanıt URL'si**, şu biçimi kullanarak URL'yi yazın: `https://<companyname>.fmshosted.com/fminteract/ConsumerService2.aspx`

    > [!NOTE] 
    > Bu değer gerçek değil. Bu değer ile gerçek yanıt URL'si güncelleştirin. Kişi [FM:Systems destek ekibi](https://fmsystems.com/ask-us/) bu değeri alınamıyor.
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/fm-systems-tutorial/tutorial_fmsystems_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/fm-systems-tutorial/tutorial_general_400.png)

6. Çoklu oturum açma yapılandırmak için **FM:Systems** yan, indirilen göndermek için ihtiyacınız **meta veri XML** için [FM:Systems destek ekibi](https://fmsystems.com/ask-us/). Bunlar, her iki tarafta da ayarlamanızı SAML SSO bağlantı sağlamak için bu ayarı ayarlayın. SSO, aboneliğiniz için etkinleştirildiğinde, bir bildirim alırsınız.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/fm-systems-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/fm-systems-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/fm-systems-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/fm-systems-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-an-fmsystems-test-user"></a>Bir FM:Systems test kullanıcısı oluşturma

1. Bir web tarayıcısı penceresinde FM:Systems şirket sitenize yönetici olarak oturum açın.

2. Git **Sistem Yönetimi \> Güvenliği Yönet \> kullanıcılar \> kullanıcı listesi**.
   
    ![Sistem Yönetimi](./media/fm-systems-tutorial/ic795905.png "Sistem Yönetimi")

3. Tıklatın **yeni kullanıcı oluştur**.
   
    ![Yeni kullanıcı oluşturmak](./media/fm-systems-tutorial/ic795906.png "yeni kullanıcı oluşturun")

4. İçinde **kullanıcı oluştur** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Kullanıcı oluşturma](./media/fm-systems-tutorial/ic795907.png "kullanıcı oluştur")
   
    a. Tür **kullanıcıadı**, **parola**, **parolayı onaylayın**, **e-posta** ve **çalışan kimliği** , bir Geçerli bir Azure Active Directory hesabı ilgili metin kutularına içine sağlamak istiyorsunuz.
   
    b. **İleri**’ye tıklayın.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta FM:Systems erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**FM:Systems için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **FM:Systems**.

    ![Çoklu oturum açmayı yapılandırın](./media/fm-systems-tutorial/tutorial_fmsystems_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli FM:Systems parçasında tıklattığınızda, otomatik olarak FM:Systems uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/fm-systems-tutorial/tutorial_general_01.png
[2]: ./media/fm-systems-tutorial/tutorial_general_02.png
[3]: ./media/fm-systems-tutorial/tutorial_general_03.png
[4]: ./media/fm-systems-tutorial/tutorial_general_04.png

[100]: ./media/fm-systems-tutorial/tutorial_general_100.png

[200]: ./media/fm-systems-tutorial/tutorial_general_200.png
[201]: ./media/fm-systems-tutorial/tutorial_general_201.png
[202]: ./media/fm-systems-tutorial/tutorial_general_202.png
[203]: ./media/fm-systems-tutorial/tutorial_general_203.png

