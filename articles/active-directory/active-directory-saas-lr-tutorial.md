---
title: "Öğretici: Azure Active Directory Tümleştirme ile LoginRadius | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile LoginRadius arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 5715579e-598f-4d2e-970a-107b80b97be4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/13/2018
ms.author: jeedes
ms.openlocfilehash: 22f937a2ae9a2c715ce0866c2db0ac17769f0399
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="tutorial-azure-active-directory-integration-with-loginradius"></a>Öğretici: Azure Active Directory Tümleştirme LoginRadius ile

Bu öğreticide, Azure Active Directory (Azure AD) ile LoginRadius tümleştirmek öğrenin.

LoginRadius Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- LoginRadius erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak için LoginRadius (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme LoginRadius ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir LoginRadius çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden LoginRadius ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-loginradius-from-the-gallery"></a>Galeriden LoginRadius ekleme
Azure AD LoginRadius tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden LoginRadius eklemeniz gerekir.

**Galeriden LoginRadius eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **LoginRadius**seçin **LoginRadius** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde LoginRadius](./media/active-directory-saas-lr-tutorial/tutorial_LoginRadius_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı LoginRadius sınayın.

Tekli çalışmaya oturum için Azure AD LoginRadius karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının LoginRadius ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma LoginRadius ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[LoginRadius test kullanıcısı oluşturma](#create-a-loginradius-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı LoginRadius sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma LoginRadius uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile LoginRadius yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **LoginRadius** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-lr-tutorial/tutorial_LoginRadius_samlbase.png)

3. Üzerinde **LoginRadius etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![LoginRadius etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-lr-tutorial/tutorial_LoginRadius_url.png)

    a.  İçinde **oturum açma URL'si** metin kutusuna, bir URL yazın: `https://secure.loginradius.com/login`

    b. İçinde **tanımlayıcısı** metin kutusuna, bir URL yazın: `https://LoginRadius.hub.loginradius.com/`
     
    > [!NOTE] 
    > Oturum açma URL'si sayfasını açın. Tıklayın **çoklu oturum açma** sekmesinde ve girin **eklenti adı** tarafından verilen [LoginRadius destek ekibi](mailto:support@loginradius.com) ardından **oturum** düğmesi olacaktır oturum açma için Azure AD sayfasına yönlendirilir. 

5. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-lr-tutorial/tutorial_LoginRadius_certificate.png) 

6. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-lr-tutorial/tutorial_general_400.png)
    
7. Çoklu oturum açma yapılandırmak için **LoginRadius** yan, indirilen göndermek için ihtiyacınız **meta veri XML** için [LoginRadius destek ekibi](mailto:support@loginradius.com). Bunlar, her iki tarafta da ayarlamanızı SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-lr-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-lr-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-lr-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-lr-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-loginradius-test-user"></a>LoginRadius test kullanıcısı oluşturma

Bu bölümde, LoginRadius içinde Britta Simon adlı bir kullanıcı oluşturun. Çalışmak [LoginRadius destek ekibi](mailto:support@loginradius.com) LoginRadius platform kullanıcıları eklemek için. Kullanıcıların oluşturulan ve çoklu oturum açma kullanmadan önce etkinleştirilmelidir. 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta LoginRadius için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**LoginRadius için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **LoginRadius**.

    ![Uygulamalar listesinde LoginRadius bağlantı](./media/active-directory-saas-lr-tutorial/tutorial_LoginRadius_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli LoginRadius parçasında tıklattığınızda, otomatik olarak LoginRadius uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-lr-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lr-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lr-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lr-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-lr-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lr-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lr-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-lr-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-lr-tutorial/tutorial_general_203.png

