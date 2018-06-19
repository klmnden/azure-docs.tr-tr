---
title: 'Öğretici: Azure Active Directory Tümleştirme ile uygunluk NetBenefits | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ve uygunluk NetBenefits arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 77dc8a98-c0e7-4129-ab88-28e7643e432a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/12/2018
ms.author: jeedes
ms.openlocfilehash: facc2e0444219977bb20aef400dbdbba09af63c6
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2018
ms.locfileid: "35971959"
---
# <a name="tutorial-azure-active-directory-integration-with-fidelity-netbenefits"></a>Öğretici: Azure Active Directory Tümleştirme uygunluğunu NetBenefits ile

Bu öğreticide, aslına uygunluk NetBenefits Azure Active Directory (Azure AD) ile tümleştirme öğrenin.

Doğruluk NetBenefits Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Doğruluk NetBenefits erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak (çoklu oturum açma) için uygunluk NetBenefits açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme uygunluğunu NetBenefits ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir doğruluk NetBenefits çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden uygunluğunu NetBenefits ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-fidelity-netbenefits-from-the-gallery"></a>Galeriden uygunluğunu NetBenefits ekleme
Azure AD uygunluğunu NetBenefits tümleştirilmesi yapılandırmak için uygunluk NetBenefits Galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden uygunluğunu NetBenefits eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna yazın **uygunluğunu NetBenefits**seçin **uygunluğunu NetBenefits** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde uygunluğunu NetBenefits](./media/fidelitynetbenefits-tutorial/tutorial_fidelitynetbenefits_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma uygunluğunu "Britta Simon" adlı bir test kullanıcı tabanlı NetBenefits ile test etme.

Tekli çalışmaya oturum için Azure AD uygunluğunu NetBenefits karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının uygunluğunu NetBenefits ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Doğruluk NetBenefits içinde **kullanıcı** eşleme ile yapılması **Azure AD kullanıcısının** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma uygunluğunu NetBenefits ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Doğruluk NetBenefits test kullanıcısı oluşturma](#create-a-fidelity-netbenefits-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı uygunluğunu NetBenefits sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma uygunluğunu NetBenefits uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile uygunluk NetBenefits yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **uygunluğunu NetBenefits** uygulama tümleştirmesi sayfasında, tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/fidelitynetbenefits-tutorial/tutorial_fidelitynetbenefits_samlbase.png)

3. Üzerinde **uygunluğunu NetBenefits etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Doğruluk NetBenefits etki alanı ve URL'leri tek oturum açma bilgileri](./media/fidelitynetbenefits-tutorial/tutorial_fidelitynetbenefits_url.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, bir URL yazın:

    Sınama ortamı için:  `urn:sp:fidelity:geninbndnbparts20:uat:xq1`

    Üretim ortamı için:  `urn:sp:fidelity:geninbndnbparts20`

    b. İçinde **yanıt URL'si** metin kutusuna, bir URL yazın:

    Sınama ortamı için:  `https://loginxq1.fidelity.com/ftgw/Fas/NBExternal/NBPartSSO/InboundSSO/consumer/sp/ACS.saml2`

    Üretim ortamı için:  `https://login.fidelity.com/ftgw/Fas/NBExternal/NBPartSSO/InboundSSO/consumer/sp/ACS.saml2`
 
4. Doğruluk NetBenefits uygulaması SAML onaylar belirli bir biçimde bekliyor. Biz eşledikten **kullanıcı tanımlayıcısı** ile **user.userprincipalname**. Bu harita **EmployeeID** veya, kuruluşunuz için geçerli olan diğer talep **kullanıcı tanımlayıcısı**. Aşağıdaki ekran görüntüsünde bunun için yalnızca bir örneği gösterir.

    ![Doğruluk NetBenefits özniteliği](./media/fidelitynetbenefits-tutorial/tutorial_fidelitynetbenefits_attribute.png)

    >[!Note]
    >Doğruluk NetBenefits statik ve dinamik Federasyon destekler. Statik yalnızca zaman sağlama kullanıcı destekleyen yalnızca zaman kullanıcı sağlama ve dinamik anlamına gelir. temel SAML kullanmayacak anlamına gelir. JIT kullanmaya ilişkin temel sağlama müşteriler kullanıcının doğum tarihi vb. gibi Azure AD'de biraz daha fazla talep eklemeniz gerekir. Bu ayrıntılar tarafından sağlanan [uygunluğunu NetBenefits destek ekibi](mailto:SSOMaintenance@fmr.com) ve bunların dinamik bu Federasyon Örneğiniz için etkinleştirmeniz gerekir.
    
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/fidelitynetbenefits-tutorial/tutorial_fidelitynetbenefits_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/fidelitynetbenefits-tutorial/tutorial_general_400.png)

6. Üzerinde **uygunluğunu NetBenefits yapılandırma** 'yi tıklatın **yapılandırma uygunluğunu NetBenefits** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Doğruluk NetBenefits yapılandırma](./media/fidelitynetbenefits-tutorial/tutorial_fidelitynetbenefits_configure.png) 

7. Çoklu oturum açma yapılandırmak için **uygunluğunu NetBenefits** yan, indirilen göndermek için ihtiyacınız **meta veri XML**, **SAML çoklu oturum açma hizmet URL'si** ve  **SAML varlık kimliği** için [uygunluğunu NetBenefits destek ekibi](mailto:SSOMaintenance@fmr.com). Bunlar, her iki tarafta da ayarlamanızı SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/fidelitynetbenefits-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/fidelitynetbenefits-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/fidelitynetbenefits-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/fidelitynetbenefits-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
  
### <a name="create-a-fidelity-netbenefits-test-user"></a>Doğruluk NetBenefits test kullanıcısı oluşturma

Bu bölümde, aslına uygunluk NetBenefits Britta Simon adlı bir kullanıcı oluşturun. Lütfen çalışmak statik Federasyon oluşturuyorsanız, [uygunluğunu NetBenefits destek ekibi](mailto:SSOMaintenance@fmr.com) uygunluğunu NetBenefits platform kullanıcıları oluşturmak için. Bu kullanıcılar oluşturulan ve çoklu oturum açma kullanmadan önce etkinleştirilmelidir. 

Dinamik Federasyon için kullanıcıların süre yalnızca kullanıcı sağlamayı kullanılarak oluşturulur. JIT kullanmaya ilişkin temel sağlama müşteriler kullanıcının doğum tarihi vb. gibi Azure AD'de biraz daha fazla talep eklemeniz gerekir. Bu ayrıntılar tarafından sağlanan [uygunluğunu NetBenefits destek ekibi](mailto:SSOMaintenance@fmr.com) ve bunların dinamik bu Federasyon Örneğiniz için etkinleştirmeniz gerekir.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, aslına uygunluk NetBenefits erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Doğruluk NetBenefits Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **uygunluğunu NetBenefits**.

    ![Uygulamalar listesinde uygunluğunu NetBenefits bağlantı](./media/fidelitynetbenefits-tutorial/tutorial_fidelitynetbenefits_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli uygunluğunu NetBenefits parçasında tıklattığınızda, otomatik olarak uygunluğunu NetBenefits uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/fidelitynetbenefits-tutorial/tutorial_general_01.png
[2]: ./media/fidelitynetbenefits-tutorial/tutorial_general_02.png
[3]: ./media/fidelitynetbenefits-tutorial/tutorial_general_03.png
[4]: ./media/fidelitynetbenefits-tutorial/tutorial_general_04.png

[100]: ./media/fidelitynetbenefits-tutorial/tutorial_general_100.png

[200]: ./media/fidelitynetbenefits-tutorial/tutorial_general_200.png
[201]: ./media/fidelitynetbenefits-tutorial/tutorial_general_201.png
[202]: ./media/fidelitynetbenefits-tutorial/tutorial_general_202.png
[203]: ./media/fidelitynetbenefits-tutorial/tutorial_general_203.png

