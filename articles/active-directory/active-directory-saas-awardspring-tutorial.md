---
title: 'Öğretici: Azure Active Directory Tümleştirme ile AwardSpring | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile AwardSpring arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 2f115be6-4fbe-42aa-9319-7462e7a75736
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/20/2018
ms.author: jeedes
ms.openlocfilehash: 13736a1125fb57b2fffb8df189dc7227ab4c5e80
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="tutorial-azure-active-directory-integration-with-awardspring"></a>Öğretici: Azure Active Directory Tümleştirme AwardSpring ile

Bu öğreticide, Azure Active Directory (Azure AD) ile AwardSpring tümleştirmek öğrenin.

AwardSpring Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- AwardSpring erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak için AwardSpring (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme AwardSpring ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir AwardSpring çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden AwardSpring ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-awardspring-from-the-gallery"></a>Galeriden AwardSpring ekleme
Azure AD AwardSpring tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden AwardSpring eklemeniz gerekir.

**Galeriden AwardSpring eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **AwardSpring**seçin **AwardSpring** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde AwardSpring](./media/active-directory-saas-awardspring-tutorial/tutorial_awardspring_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı AwardSpring sınayın.

Tekli çalışmaya oturum için Azure AD AwardSpring karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının AwardSpring ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma AwardSpring ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir AwardSpring test kullanıcısı oluşturma](#create-an-awardspring-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı AwardSpring sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma AwardSpring uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile AwardSpring yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **AwardSpring** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-awardspring-tutorial/tutorial_awardspring_samlbase.png)

3. Üzerinde **AwardSpring etki alanı ve URL'leri** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** modu tarafından başlatılan:

    ![AwardSpring etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-awardspring-tutorial/tutorial_awardspring_url.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<subdomain>.awardspring.com/SignIn/SamlMetaData`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<subdomain>.awardspring.com/SignIn/SamlAcs`

4. Denetleme **Göster Gelişmiş URL ayarları** ve uygulamada yapılandırmak istiyorsanız aşağıdaki adımı gerçekleştirin **SP** modunda başlatılan:

    ![AwardSpring etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-awardspring-tutorial/tutorial_awardspring_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<subdomain>.awardspring.com/`
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler, gerçek tanımlayıcı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Kişi [AwardSpring istemci destek ekibi](mailto:support@awardspring.com) bu değerleri almak için. 

5. AwardSpring uygulaması SAML onaylar belirli bir biçimde bekliyor. Bu uygulama için aşağıdaki talep yapılandırın. Bu öznitelik değerlerini yönetebilirsiniz **kullanıcı öznitelikleri** uygulama tümleştirmesi sayfasında bölüm. Aşağıdaki ekran görüntüsünde bunun bir örneği gösterir.
    
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-awardSpring-tutorial/tutorial_awardSpring_attribute.png)

6. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, yukarıdaki resimde gösterildiği gibi SAML belirteci özniteliği yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Öznitelik Adı | Öznitelik Değeri |
    | ---------------| --------------- |    
    | Ad | User.givenName |
    | Soyadı | User.surname |
    | Email | User.Mail |
    | Kullanıcı adı | User.userprinicipalname |
    | studentID | < Öğrenci Kimliği > |

    > [!NOTE]
    > StudentID öznitelik geri Taleplerde geçirilmesi gereken gerçek Öğrenci Kimliği ile eşlenir. Kişi [AwardSpring istemci destek ekibi](mailto:support@awardspring.com) bu değeri alınamıyor.

    a. Varolan öznitelikleri kaldırın ve yeni öznitelikler ekleyin. Tıklatın **Ekle özniteliği** açmak için **özniteliği eklemek** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-awardspring-tutorial/tutorial_attribute_04.png)

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-awardspring-tutorial/tutorial_attribute_05.png)
    
    b. İçinde **adı** metin kutusuna, ilgili satır için gösterilen öznitelik adı yazın.
    
    c. Gelen **değeri** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    d. Bırakın **Namespace** boş.
    
    d. Tıklatın **Tamam**

7. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-awardspring-tutorial/tutorial_awardspring_certificate.png) 

8. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-awardspring-tutorial/tutorial_general_400.png)
    
9. Çoklu oturum açma yapılandırmak için **AwardSpring** yan, indirilen göndermek için ihtiyacınız **meta veri XML** için [AwardSpring destek ekibi](mailto:support@awardspring.com). Bunlar, her iki tarafta da ayarlamanızı SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-awardspring-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-awardspring-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-awardspring-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-awardspring-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-an-awardspring-test-user"></a>Bir AwardSpring test kullanıcısı oluşturma

Bu bölümün amacı Britta Simon içinde AwardSpring adlı bir kullanıcı oluşturmaktır. AwardSpring yalnızca zaman sağlama, varsayılan olarak etkin olduğu destekler. Bu bölümde, eylem öğe yok. Yeni bir kullanıcı henüz yoksa AwardSpring erişme denemesi sırasında oluşturulur.

>[!Note]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [AwardSpring destek ekibi](maito:support@awardspring.com).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta AwardSpring için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**AwardSpring için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **AwardSpring**.

    ![Uygulamalar listesinde AwardSpring bağlantı](./media/active-directory-saas-awardspring-tutorial/tutorial_awardspring_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli AwardSpring parçasında tıklattığınızda, otomatik olarak AwardSpring uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-awardspring-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-awardspring-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-awardspring-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-awardspring-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-awardspring-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-awardspring-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-awardspring-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-awardspring-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-awardspring-tutorial/tutorial_general_203.png

