---
title: 'Öğretici: Azure Active Directory Tümleştirme ile TurboRater | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile TurboRater arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: abb116b8-8024-4cc6-bc81-f32ef490ea17
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/03/2017
ms.author: jeedes
ms.openlocfilehash: ba7f14066bcad97ea65ad041a61a5fc537b72ef9
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34350092"
---
# <a name="tutorial-azure-active-directory-integration-with-turborater"></a>Öğretici: Azure Active Directory Tümleştirme TurboRater ile

Bu öğreticide, Azure Active Directory (Azure AD) ile TurboRater tümleştirmek öğrenin.

TurboRater Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- TurboRater erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak için TurboRater (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme TurboRater ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir TurboRater çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden TurboRater ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-turborater-from-the-gallery"></a>Galeriden TurboRater ekleme
Azure AD TurboRater tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden TurboRater eklemeniz gerekir.

**Galeriden TurboRater eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **TurboRater**seçin **TurboRater** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde TurboRater](./media/active-directory-saas-turborater-tutorial/tutorial_turborater_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı TurboRater sınayın.

Tekli çalışmaya oturum için Azure AD TurboRater karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının TurboRater ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

TurboRater içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma TurboRater ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[TurboRater test kullanıcısı oluşturma](#create-a-turborater-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı TurboRater sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma TurboRater uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile TurboRater yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **TurboRater** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-turborater-tutorial/tutorial_turborater_samlbase.png)

3. Üzerinde **TurboRater etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![TurboRater etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-turborater-tutorial/tutorial_turborater_url.png)

    a. İçinde **tanımlayıcısı** metin değeri olarak yazın: `https://www.itcdataservices.com`
 
    b. İçinde **yanıt URL'si** metin değeri olarak yazın:
    
    | Ortam | URL'si |
    | ---------------| --------------- |    
    | Test etme  | `https://ratingqa.itcdataservices.com/webservices/imp/saml/login` |
    | Canlı  | `https://www.itcratingservices.com/webservices/imp/saml/login` |

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-turborater-tutorial/tutorial_turborater_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-turborater-tutorial/tutorial_general_400.png)

7. Çoklu oturum açma yapılandırmak için **TurboRater** yan, indirilen göndermek için ihtiyacınız **meta veri XML** için [TurboRater destek ekibi](https://www.getitc.com/support). Bunlar, her iki tarafta da ayarlamanızı SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-turborater-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-turborater-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-turborater-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-turborater-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
  
### <a name="create-a-turborater-test-user"></a>TurboRater test kullanıcısı oluşturma

Azure AD kullanıcıları için TurboRater oturum açmak etkinleştirmek için bunların TurboRater sağlanmalıdır.  
TurboRater söz konusu olduğunda, sağlama bir el ile bir görevdir.
Bir kullanıcı oluşturmak için lütfen çalışmak [TurboRater destek ekibi](https://www.getitc.com/support).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta TurboRater için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**TurboRater için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **TurboRater**.

    ![Uygulamalar listesinde TurboRater bağlantı](./media/active-directory-saas-turborater-tutorial/tutorial_turborater_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli TurboRater parçasında tıklattığınızda, otomatik olarak TurboRater uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/active-directory-saas-turborater-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-turborater-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-turborater-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-turborater-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-turborater-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-turborater-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-turborater-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-turborater-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-turborater-tutorial/tutorial_general_203.png

