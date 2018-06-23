---
title: 'Öğretici: Azure Active Directory Tümleştirme yenilik Hub ile | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ve yenilik Hub arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: d72e4da0-0123-409b-96c2-e613f3f83fb1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2018
ms.author: jeedes
ms.openlocfilehash: 612cce99b1a510dfb53ec4a6c1e2a40185367db1
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36320754"
---
# <a name="tutorial-azure-active-directory-integration-with-innovation-hub"></a>Öğretici: Azure Active Directory Tümleştirme yenilik Hub ile

Bu öğreticide, Azure Active Directory (Azure AD) ile yenilik Hub tümleştirmek öğrenin.

Yenilik hub'ı Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Yenilik Hub erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak yenilik hub'a (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme yenilik Hub ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir yenilik Hub çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden yenilik Hub ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-innovation-hub-from-the-gallery"></a>Galeriden yenilik Hub ekleme
Azure AD yenilik Hub tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden yenilik Hub eklemeniz gerekir.

**Galeriden yenilik Hub eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **yenilik Hub**seçin **yenilik Hub** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde yenilik Hub](./media/innovationhub-tutorial/tutorial_innovationhub_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma yenilik "Britta Simon" adlı bir test kullanıcı tabanlı Hub ile test etme.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen yenilik Hub'ındaki bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve yenilik Hub ilgili kullanıcı arasındaki bağlantıyı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma yenilik Hub ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir yenilik Hub test kullanıcısı oluşturma](#create-an-innovation-hub-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı yenilik Hub sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma yenilik Hub uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma yenilik Hub ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **yenilik Hub** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/innovationhub-tutorial/tutorial_innovationhub_samlbase.png)

3. Üzerinde **yenilik Hub etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Yenilik Hub etki alanı ve URL'leri tek oturum açma bilgileri](./media/innovationhub-tutorial/tutorial_innovationhub_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<domainname>.innohb.com/auth/saml2/login`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<domainname>.innohb.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Kişi [yenilik Hub istemci destek ekibi](mailto:support@readify.net) bu değerleri almak için.

4. Yenilik Hub uygulama SAML onaylar belirli bir biçimde bekler. Lütfen bu uygulama için aşağıdaki talep yapılandırın. Bu öznitelik değerlerini yönetebilirsiniz "**kullanıcı öznitelikleri**" uygulama tümleştirmesi sayfasında bölüm. Aşağıdaki ekran görüntüsünde bunun bir örneği gösterir.

    ![Çoklu oturum açmayı yapılandırın](./media/innovationhub-tutorial/attribute.png)

5. Tıklatın **Görünüm ve diğer tüm kullanıcı özniteliklerini düzenleme** onay kutusu **kullanıcı öznitelikleri** öznitelikleri genişletmek için bölüm. Her biri görüntülenen öznitelikleri - üzerinde aşağıdaki adımları uygulayın

    | Öznitelik Adı | Öznitelik Değeri | Namespace değeri|
    | ---------------| --------------- |----------------|
    | görünen adı | User.userPrincipalName | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims`|
    | | |

    a. Tıklatın **Ekle özniteliği** açmak için **özniteliği eklemek** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/innovationhub-tutorial/tutorial_attribute_04.png)

    ![Çoklu oturum açmayı yapılandırın](./media/innovationhub-tutorial/tutorial_attribute_05.png)

    b. İçinde **adı** metin kutusuna, ilgili satır için gösterilen öznitelik adı yazın.

    c. Gelen **değeri** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    d. Gelen **Namespace değeri** listesinde, ilgili satır için gösterilen ad alanı değeri yazın.

    e. **Tamam**’a tıklayın.

4. Üzerinde **SAML imzalama sertifikası** bölümünde, kopyalamak için Kopyala düğmesini tıklatın **uygulama Federasyon meta veri URL'sini** ve Not Defteri'ne yapıştırın.

    ![Sertifika indirme bağlantısı](./media/innovationhub-tutorial/tutorial_innovationhub_certificate.png)

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/innovationhub-tutorial/tutorial_general_400.png)

6. Çoklu oturum açma yapılandırmak için **yenilik Hub** yan, kopyalanan göndermek için ihtiyacınız **Federasyon meta veri URL'sini** için [yenilik Hub destek ekibi](mailto:support@readify.net). Bunlar, her iki tarafta da ayarlamanızı SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/innovationhub-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/innovationhub-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/innovationhub-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/innovationhub-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-an-innovation-hub-test-user"></a>Bir yenilik Hub test kullanıcısı oluşturma

Bu bölümün amacı Britta Simon yenilik Hub adı verilen bir kullanıcı oluşturmaktır. Yenilik Hub yalnızca zaman sağlama, varsayılan olarak etkin olduğu destekler. Bu bölümde, eylem öğe yok. Yeni bir kullanıcı henüz yoksa yenilik Hub erişme denemesi sırasında oluşturulur.
>[!Note]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [yenilik Hub destek ekibi](mailto:support@readify.net).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta yenilik Hub'ına erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Yenilik Hub'ına Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **yenilik Hub**.

    ![Uygulamalar listesinde yenilik Hub bağlantı](./media/innovationhub-tutorial/tutorial_innovationhub_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli yenilik Hub parçasında tıklattığınızda, otomatik olarak yenilik hub'ı uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)
<!--Image references-->

[1]: ./media/innovationhub-tutorial/tutorial_general_01.png
[2]: ./media/innovationhub-tutorial/tutorial_general_02.png
[3]: ./media/innovationhub-tutorial/tutorial_general_03.png
[4]: ./media/innovationhub-tutorial/tutorial_general_04.png

[100]: ./media/innovationhub-tutorial/tutorial_general_100.png

[200]: ./media/innovationhub-tutorial/tutorial_general_200.png
[201]: ./media/innovationhub-tutorial/tutorial_general_201.png
[202]: ./media/innovationhub-tutorial/tutorial_general_202.png
[203]: ./media/innovationhub-tutorial/tutorial_general_203.png

