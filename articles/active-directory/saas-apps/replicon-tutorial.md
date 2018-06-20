---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Replicon | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile Replicon arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 02a62f15-917c-417c-8d80-fe685e3fd601
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2018
ms.author: jeedes
ms.openlocfilehash: 70a67f5205ee9b1249d3db2cda3eb4af9acabe31
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36230184"
---
# <a name="tutorial-azure-active-directory-integration-with-replicon"></a>Öğretici: Azure Active Directory Tümleştirme Replicon ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Replicon tümleştirmek öğrenin.

Replicon Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Replicon erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak için Replicon (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Replicon ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Replicon çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Replicon ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-replicon-from-the-gallery"></a>Galeriden Replicon ekleme
Azure AD Replicon tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Replicon eklemeniz gerekir.

**Galeriden Replicon eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Replicon**seçin **Replicon** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde replicon](./media/replicon-tutorial/tutorial_replicon_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Replicon sınayın.

Tekli çalışmaya oturum için Azure AD Replicon karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Replicon ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Replicon içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Replicon ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Replicon test kullanıcısı oluşturma](#create-a-replicon-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Replicon sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Replicon uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Replicon yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Replicon** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma iletişim kutusu](./media/replicon-tutorial/tutorial_replicon_samlbase.png)

3. Üzerinde **Replicon etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Replicon etki alanı ve URL'leri tek oturum açma bilgileri](./media/replicon-tutorial/tutorial_replicon_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://na2.replicon.com/<companyname>/saml2/sp-sso/post`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://global.replicon.com/<companyname>`

    c. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://global.replicon.com/!/saml2/<companyname>/sso/post`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler, gerçek oturum açma URL'si tanımlayıcısı ve yanıt URL'si ile güncelleştirin. Kişi [Replicon istemci destek ekibi](https://www.replicon.com/customerzone/contact-support) bu değerleri almak için. 

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/replicon-tutorial/tutorial_replicon_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/replicon-tutorial/tutorial_general_400.png)

6. Farklı web tarayıcısı penceresinde Replicon şirket sitenize yönetici olarak oturum açın.

7. SAML 2.0 yapılandırmak için aşağıdaki adımları gerçekleştirin:

    ![SAML kimlik doğrulamasını etkinleştir](./media/replicon-tutorial/ic777805.png "etkinleştirmek SAML kimlik doğrulaması")

    a. Görüntülenecek **EnableSAML Authentication2** iletişim kutusunda, aşağıdaki URL'nizi, şirket anahtarınızı sonra Ekle: `/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2`

    * Aşağıda tam URL şeması gösterilmektedir: `https://na2.replicon.com/\<YourCompanyKey\>/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2`

   b. Tıklatın **+** genişletmek için **v20Configuration** bölümü.

   c. Tıklatın **+** genişletmek için **metaDataConfiguration** bölümü.

   d. Tıklatın **Dosya Seç**, kimlik sağlayıcısı meta verileri XML dosyasını seçin ve ' **gönderme**.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/replicon-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/replicon-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/replicon-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/replicon-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.

### <a name="create-a-replicon-test-user"></a>Replicon test kullanıcısı oluşturma

Bu bölümün amacı Britta Simon içinde Replicon adlı bir kullanıcı oluşturmaktır. Replicon otomatik kullanıcı hazırlama, varsayılan olarak etkin olduğu destekler. Daha fazla ayrıntı bulabilirsiniz [burada](replicon-provisioning-tutorial.md) otomatik kullanıcı sağlamayı yapılandırma.

**Kullanıcı el ile oluşturmanız gerekiyorsa, şu adımları gerçekleştirin:**

1. Bir web tarayıcısı penceresinde Replicon şirket sitenize yönetici olarak oturum açın.

2. Git **Yönetim \> kullanıcılar**.

    ![Kullanıcıların](./media/replicon-tutorial/ic777806.png "kullanıcılar")

3. Tıklatın **+ kullanıcı ekleme**.

    ![Kullanıcı ekleme](./media/replicon-tutorial/ic777807.png "kullanıcı ekleme")

4. İçinde **kullanıcı profili** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı profili](./media/replicon-tutorial/ic777808.png "kullanıcı profili")

    a. İçinde **oturum açma adı** metin kutusuna, istediğiniz gibi sağlamak için Azure AD kullanıcısının e-posta adresini yazın Azure AD **BrittaSimon@contoso.com**.

    b. Olarak **kimlik doğrulama türü**seçin **SSO**.

    c. İçinde **departmanı** metin kutusuna, kullanıcının bölüm adını yazın.

    d. Olarak **çalışan türü**seçin **yönetici**.

    e. Tıklatın **kullanıcı profili Kaydet**.

>[!NOTE]
>API Azure AD kullanıcı hesaplarını sağlamak için Replicon tarafından sağlanan veya herhangi diğer Replicon kullanıcı hesabı oluşturma araçlarını kullanabilirsiniz.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Replicon için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200]

**Replicon için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201]

2. Uygulamalar listesinde **Replicon**.

    ![Uygulamalar listesinde Replicon bağlantı](./media/replicon-tutorial/tutorial_replicon_app.png)

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Replicon parçasında tıklattığınızda, otomatik olarak Replicon uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)
* [Kullanıcı sağlamayı Yapılandır](replicon-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/replicon-tutorial/tutorial_general_01.png
[2]: ./media/replicon-tutorial/tutorial_general_02.png
[3]: ./media/replicon-tutorial/tutorial_general_03.png
[4]: ./media/replicon-tutorial/tutorial_general_04.png

[100]: ./media/replicon-tutorial/tutorial_general_100.png

[200]: ./media/replicon-tutorial/tutorial_general_200.png
[201]: ./media/replicon-tutorial/tutorial_general_201.png
[202]: ./media/replicon-tutorial/tutorial_general_202.png
[203]: ./media/replicon-tutorial/tutorial_general_203.png
