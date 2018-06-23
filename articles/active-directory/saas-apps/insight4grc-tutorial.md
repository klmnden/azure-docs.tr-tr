---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Insight4GRC | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile Insight4GRC arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: db3b4bd1-b372-4660-88d7-aea0b0ca962e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2018
ms.author: jeedes
ms.openlocfilehash: 31b14d82142e6b229e779316b46848af28ac915b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "36337775"
---
# <a name="tutorial-azure-active-directory-integration-with-insight4grc"></a>Öğretici: Azure Active Directory Tümleştirme Insight4GRC ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Insight4GRC tümleştirmek öğrenin.

Insight4GRC Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Insight4GRC erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak Insight4GRC için açan kullanıcılarınıza etkinleştirebilirsiniz (çoklu oturum açma) Azure AD hesaplarına sahip.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Insight4GRC ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Insight4GRC çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Insight4GRC ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-insight4grc-from-the-gallery"></a>Galeriden Insight4GRC ekleme
Azure AD Insight4GRC tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Insight4GRC eklemeniz gerekir.

**Galeriden Insight4GRC eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Insight4GRC**seçin **Insight4GRC** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde Insight4GRC](./media/insight4grc-tutorial/tutorial_insight_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Insight4GRC sınayın.

Tekli çalışmaya oturum için Azure AD Insight4GRC karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Insight4GRC ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Insight4GRC ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir Insight4GRC test kullanıcısı oluşturma](#create-an-insight4grc-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Insight4GRC sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Insight4GRC uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Insight4GRC yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Insight4GRC** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/insight4grc-tutorial/tutorial_insight_samlbase.png)

3. Üzerinde **Insight4GRC etki alanı ve URL'leri** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** modu tarafından başlatılan:

    ![Insight4GRC etki alanı ve URL'leri tek oturum açma bilgileri](./media/insight4grc-tutorial/tutorial_insight_url.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<subdomain>.Insight4GRC.com/SAML`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<subdomain>.Insight4GRC.com/Public/SAML/ACS.aspx`

4. Denetleme **Göster Gelişmiş URL ayarları** ve uygulamada yapılandırmak istiyorsanız aşağıdaki adımı gerçekleştirin **SP** modunda başlatılan:

    ![Insight4GRC etki alanı ve URL'leri tek oturum açma bilgileri](./media/insight4grc-tutorial/tutorial_insight_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<subdomain>.Insight4GRC.com/Public/Login.aspx`
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler, gerçek tanımlayıcı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Kişi [Insight4GRC istemci destek ekibi](mailto:support.ss@rsmuk.com) bu değerleri almak için. 

5. Üzerinde **SAML imzalama sertifikası** bölümünde, kopyalamak için Kopyala düğmesini tıklatın **uygulama Federasyon meta veri URL'sini** ve Not Defteri'ne yapıştırın.

    ![Sertifika indirme bağlantısı](./media/insight4grc-tutorial/tutorial_insight_certificate.png) 

6. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/insight4grc-tutorial/tutorial_general_400.png)
    
7. Çoklu oturum açma yapılandırmak için **Insight4GRC** yan, kopyalanan göndermek için ihtiyacınız **uygulama Federasyon meta veri URL'sini** için [Insight4GRC destek ekibi](mailto:support.ss@rsmuk.com). Bunlar, her iki tarafta da ayarlamanızı SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/insight4grc-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/insight4grc-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/insight4grc-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/insight4grc-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-an-insight4grc-test-user"></a>Bir Insight4GRC test kullanıcısı oluşturma

Bu bölümün amacı Britta Simon içinde Insight4GRC adlı bir kullanıcı oluşturmaktır. Insight4GRC yalnızca zaman sağlama, varsayılan olarak etkin olduğu destekler. Bu bölümde, eylem öğe yok. Yeni bir kullanıcı henüz yoksa Insight4GRC erişme denemesi sırasında oluşturulur.

>[!Note]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [Insight4GRC istemci destek ekibi](mailto:support.ss@rsmuk.com).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Insight4GRC için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Insight4GRC için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Insight4GRC**.

    ![Uygulamalar listesinde Insight4GRC bağlantı](./media/insight4grc-tutorial/tutorial_insight_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Insight4GRC parçasında tıklattığınızda, otomatik olarak Insight4GRC uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/insight4grc-tutorial/tutorial_general_01.png
[2]: ./media/insight4grc-tutorial/tutorial_general_02.png
[3]: ./media/insight4grc-tutorial/tutorial_general_03.png
[4]: ./media/insight4grc-tutorial/tutorial_general_04.png

[100]: ./media/insight4grc-tutorial/tutorial_general_100.png

[200]: ./media/insight4grc-tutorial/tutorial_general_200.png
[201]: ./media/insight4grc-tutorial/tutorial_general_201.png
[202]: ./media/insight4grc-tutorial/tutorial_general_202.png
[203]: ./media/insight4grc-tutorial/tutorial_general_203.png

