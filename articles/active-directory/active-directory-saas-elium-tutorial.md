---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Elium | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile Elium arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: fae344b3-5bd9-40e2-9a1d-448dcd58155f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/03/2018
ms.author: jeedes
ms.openlocfilehash: 6a72cc1829b7b8a5c7c588543d0b5c91f9f36bf5
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="tutorial-azure-active-directory-integration-with-elium"></a>Öğretici: Azure Active Directory Tümleştirme Elium ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Elium tümleştirmek öğrenin.

Elium Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Elium erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak için Elium (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Elium ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Elium çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Elium ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-elium-from-the-gallery"></a>Galeriden Elium ekleme
Azure AD Elium tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Elium eklemeniz gerekir.

**Galeriden Elium eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Elium**seçin **Elium** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde Elium](./media/active-directory-saas-elium-tutorial/tutorial_elium_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Elium sınayın.

Tekli çalışmaya oturum için Azure AD Elium karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Elium ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Elium ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir Elium test kullanıcısı oluşturma](#create-an-elium-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Elium sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Elium uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Elium yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Elium** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-elium-tutorial/tutorial_elium_samlbase.png)

3. Üzerinde **Elium etki alanı ve URL'leri** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** modu tarafından başlatılan:

    ![Elium etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-elium-tutorial/tutorial_elium_url.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<platform-domain>.elium.com/login/saml2/metadata`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<platform-domain>.elium.com/login/saml2/acs`

4. Denetleme **Göster Gelişmiş URL ayarları** ve uygulamada yapılandırmak istiyorsanız aşağıdaki adımı gerçekleştirin **SP** modunda başlatılan:

    ![Elium etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-elium-tutorial/tutorial_elium_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: ` https://<platform-domain>.elium.com/login/saml2/login`
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerleri alırsınız **SP meta veri dosyası** adresindeki indirilebilir `https://<platform-domain>/login/saml2/metadata`, bu öğreticinin ilerleyen bölümlerinde açıklanmıştır.

5. Elium uygulaması SAML onaylar SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemelerini ekleyin gerektiren belirli bir biçimde bekliyor. Bu uygulama için aşağıdaki talep yapılandırın. Bu öznitelik değerlerini yönetebilirsiniz "**kullanıcı öznitelikleri**" uygulama tümleştirmesi sayfasında bölüm.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-elium-tutorial/tutorial_attribute.png)

6. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, önceki görüntüde gösterildiği gibi SAML belirteci özniteliği yapılandırın ve aşağıdaki adımları gerçekleştirin:
           
    | Öznitelik Adı | Öznitelik Değeri |   
    | ---------------| ----------------|
    | e-posta   |user.mail |
    | first_name| user.givenname |
    | last_name| user.surname|
    | job_title| user.jobtitle|
    | Şirket| user.companyname|
    
    > [!NOTE]
    > Bu varsayılan taleplerdir. **Yalnızca e-posta talebi gereklidir**. Yalnızca e-posta ayrıca sağlama JIT için talep zorunludur. Diğer özel talep başka bir müşteri platform için bir müşteri platformundan farklılık gösterebilir.

    a. Tıklatın **Ekle özniteliği** açmak için **özniteliği eklemek** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-elium-tutorial/tutorial_attribute_04.png)

    b. İçinde **adı** metin kutusuna, ilgili satır için gösterilen öznitelik adı yazın.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-elium-tutorial/tutorial_attribute_05.png)

    c. Gelen **değeri** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    d. Ad alanı boş bırakın.
    
    e. **Tamam**’a tıklayın. 

5. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-elium-tutorial/tutorial_elium_certificate.png) 

6. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-elium-tutorial/tutorial_general_400.png)
    
7. Farklı web tarayıcısı penceresinde Elium şirket sitenize yönetici olarak oturum açın.

8. Tıklayın **kullanıcı profili** sağ üst köşesinde ve ardından **Yönetim**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-elium-tutorial/user1.png)

9. Seçin **güvenlik** sekmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-elium-tutorial/user2.png)

10. Ekranı aşağı kaydırarak **çoklu oturum açma (SSO)** bölümünde ve aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-elium-tutorial/user3.png)

    a. Değerini kopyalayın **SAML2 kimlik doğrulamanın çalıştığını hesabınız için Doğrula** ve yapıştırın **oturum açma URL'si** textbox üzerinde **Elium etki alanı ve URL'leri** Azure bölümünde Portal.

    > [!NOTE]
    > SSO yapılandırdıktan sonra aşağıdaki URL'de varsayılan uzaktan oturum açma sayfası her zaman erişebilirsiniz: `https://<platform_domain>/login/regular/login` 

    b. Seçin **etkinleştirmek SAML2 Federasyon** onay kutusu.

    c. Seçin **JIT sağlama** onay kutusu.

    d. Açık **SP meta veri** tıklayarak **karşıdan** düğmesi.

    e. Arama **Entityıd** içinde **SP meta veri** dosya, kopya **Entityıd** değer ve yapıştırın **tanımlayıcısı** textbox üzerinde **Elium etki alanı ve URL'leri** Azure portalı bölümünde. 

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-elium-tutorial/user4.png)

    f. Arama **AssertionConsumerService** içinde **SP meta veri** dosya, kopya **konumu** değer ve yapıştırın **yanıt URL'si** TextBox üzerinde **Elium etki alanı ve URL'leri** Azure portalı bölümünde.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-elium-tutorial/user5.png)

    g. Not Defteri'ne Azure Portalı'ndan indirilen meta veri dosyası açın, içeriğini kopyalayın ve yapıştırın **IDP meta veri** metin kutusu.

    h. **Kaydet**’e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-elium-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-elium-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-elium-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-elium-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-an-elium-test-user"></a>Bir Elium test kullanıcısı oluşturma

Bu bölümün amacı Britta Simon içinde Elium adlı bir kullanıcı oluşturmaktır. Elium yalnızca zaman sağlama, varsayılan olarak etkin olduğu destekler. Bu bölümde, eylem öğe yok. Yeni bir kullanıcı henüz yoksa Elium erişme denemesi sırasında oluşturulur.
>[!Note]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [Elium destek ekibi](mailto:support@elium.com).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Elium için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Elium için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Elium**.

    ![Uygulamalar listesinde Elium bağlantı](./media/active-directory-saas-elium-tutorial/tutorial_elium_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Elium parçasında tıklattığınızda, otomatik olarak Elium uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-elium-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-elium-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-elium-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-elium-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-elium-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-elium-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-elium-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-elium-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-elium-tutorial/tutorial_general_203.png

