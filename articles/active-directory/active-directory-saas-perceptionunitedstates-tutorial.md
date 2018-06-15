---
title: 'Öğretici: Azure Active Directory Tümleştirmesi algısına Amerika Birleşik Devletleri (Non-UltiPro) ile | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile algısına Amerika Birleşik Devletleri (Non-UltiPro) arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: b4a8f026-cb5f-41eb-9680-68eddc33565e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: da0529897bb02745a2346f6a0282be86923468ba
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34350126"
---
# <a name="tutorial-azure-active-directory-integration-with-perception-united-states-non-ultipro"></a>Öğretici: Azure Active Directory Tümleştirmesi algısına Amerika Birleşik Devletleri (Non-UltiPro) ile

Bu öğreticide, Azure Active Directory (Azure AD) ile algısına Amerika Birleşik Devletleri (Non-UltiPro) tümleştirme öğrenin.

Algısına Amerika Birleşik Devletleri (Non-UltiPro) Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Erişimi algısına Amerika Birleşik Devletleri (Non-UltiPro) için Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak algısına Amerika Birleşik Devletleri (Non-UltiPro) (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi algısına Amerika Birleşik Devletleri (Non-UltiPro) yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir algısına Amerika Birleşik Devletleri (Non-UltiPro) çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden algısına Amerika Birleşik Devletleri (Non-UltiPro) ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-perception-united-states-non-ultipro-from-the-gallery"></a>Galeriden algısına Amerika Birleşik Devletleri (Non-UltiPro) ekleme
Azure AD ile tümleştirme, algısına Amerika Birleşik Devletleri (Non-UltiPro) yapılandırmak için algısına Amerika Birleşik Devletleri (Non-UltiPro) eklemeniz Galeriden yönetilen SaaS uygulamaları listenize gerekir.

**Galeriden algısına Amerika Birleşik Devletleri (Non-UltiPro) eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **algısına Amerika Birleşik Devletleri (Non-UltiPro)** seçin **algısına Amerika Birleşik Devletleri (Non-UltiPro)** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde algısına Amerika Birleşik Devletleri (Non-UltiPro)](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma ile algısına "Britta Simon" adlı bir test kullanıcı tabanlı ABD (UltiPro olmayan) test etme.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen algısına Amerika Birleşik Devletleri (Non-UltiPro) içinde bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve ilgili kullanıcı içinde algısına Amerika Birleşik Devletleri (Non-UltiPro) arasında bir bağlantı ilişkisi kurulması gerekir.

Değeri algısına Amerika Birleşik Devletleri (UltiPro olmayan), Ata **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma algısına Amerika Birleşik Devletleri (Non-UltiPro) ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Algısına Amerika Birleşik Devletleri (Non-UltiPro) test kullanıcısı oluşturma](#create-a-perception-united-states-non-ultipro-test-user)**  - Britta Simon, karşılık gelen içinde algısına kullanıcı Azure AD gösterimini bağlantılı ABD (UltiPro olmayan) sahip.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma algısına Amerika Birleşik Devletleri (Non-UltiPro) uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma algısına Amerika Birleşik Devletleri (Non-UltiPro) yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **algısına Amerika Birleşik Devletleri (Non-UltiPro)** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_samlbase.png)

3. Üzerinde **algısına Amerika Birleşik Devletleri (UltiPro olmayan) etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Algısına Amerika Birleşik Devletleri (UltiPro olmayan) etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_url.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, URL'yi yazın: `https://perception.kanjoya.com/sp`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://perception.kanjoya.com/sso?idp=<entity_id>`

    > [!NOTE] 
    > Değer gerçek değil. Değer, gerçek yanıt, öğreticide daha sonra açıklanan URL ile güncelleştirir.
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_400.png)

6. Üzerinde **algısına Amerika Birleşik Devletleri (Non-UltiPro) yapılandırma** 'yi tıklatın **yapılandırma algısına Amerika Birleşik Devletleri (Non-UltiPro)** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML varlık kimliği** gelen **hızlı başvuru bölümü.**

    a. **Algısına Amerika Birleşik Devletleri (Non-UltiPro)** uygulama gerektirir **SAML varlık kimliği** uri ile kodlanan olması için kopyaladığınız değeri. Uri ile kodlanan değerini almak için aşağıdaki bağlantıyı kullanın:**http://www.url-encode-decode.com/**.

    b. URI edindikten sonra kodlanmış değeriyle birleştiğinde ile **yanıt URL'si** aşağıdaki - belirtildiği gibi

    `https://perception.kanjoya.com/sso?idp=<URI encooded entity_id>`
    
    c. Yukarıdaki değeri yapıştırın **yanıt URL'si** metin kutusuna **algısına Amerika Birleşik Devletleri (UltiPro olmayan) etki alanı ve URL'leri** bölümü.

    ![Algısına Amerika Birleşik Devletleri (UltiPro olmayan) yapılandırma](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_configure.png) 

7. Başka bir tarayıcı penceresinde algısına Amerika Birleşik Devletleri (Non-UltiPro) şirket sitenize yönetici olarak oturum açma.

8. Ana araç çubuğunda tıklatın **hesap ayarlarını**.

    ![Algısına Amerika Birleşik Devletleri (Non-UltiPro) kullanıcı](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_user.png)

9. Üzerinde **hesap ayarlarını** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Algısına Amerika Birleşik Devletleri (Non-UltiPro) kullanıcı](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_account.png)

    a. İçinde **şirket adı** metin kutusuna, adı **şirket**.
    
    b. İçinde **hesap adı** metin kutusuna, adı **hesap**.

    c. İçinde **varsayılan yanıt-e-posta** metin kutusunda, geçerli **e-posta**.

    d. Seçin **SSO kimlik sağlayıcısı** olarak **SAML 2.0**.

10. Üzerinde **SSO yapılandırma** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Algısına Amerika Birleşik Devletleri (UltiPro olmayan) SSOConfig](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_ssoconfig.png)

    a. Seçin **SAML NameID türü** olarak **e-posta**.

    b. İçinde **SSO yapılandırma adı** metin kutusuna, adını yazın, **yapılandırma**.
    
    c. İçinde **kimlik sağlayıcı adı** metin değerini yapıştırın **SAML varlık kimliği**, Azure portalından kopyalanan. 

    d. İçinde **SAML etki alanı metin kutusu**, etki alanı gibi girin **@contoso.com**.

    e. Tıklayın **yeniden karşıya** karşıya yüklemek için **meta veri XML** dosya.

    f. Tıklatın **güncelleştirme**.


> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-perceptionunitedstates-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-perceptionunitedstates-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-perceptionunitedstates-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-perceptionunitedstates-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
  
### <a name="create-a-perception-united-states-non-ultipro-test-user"></a>Algısına Amerika Birleşik Devletleri (Non-UltiPro) test kullanıcısı oluşturma

Bu bölümde, Britta Simon algısına Amerika Birleşik Devletleri (Non-UltiPro) adlı bir kullanıcı oluşturun. Çalışmak [algısına Amerika Birleşik Devletleri (Non-UltiPro) destek ekibi](http://www.ultimatesoftware.com/Contact/ContactUs) algısına Amerika Birleşik Devletleri (Non-UltiPro) platform kullanıcıları eklemek için.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta algısına Amerika Birleşik Devletleri (Non-UltiPro) erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon algısına Amerika Birleşik Devletleri (Non-UltiPro) atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **algısına Amerika Birleşik Devletleri (Non-UltiPro)**.

    ![Uygulamalar listesinde algısına Amerika Birleşik Devletleri (Non-UltiPro) bağlantısı](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli algısına Amerika Birleşik Devletleri (Non-UltiPro) parçasında tıklattığınızda, otomatik olarak algısına Amerika Birleşik Devletleri (Non-UltiPro) uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_203.png

