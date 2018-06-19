---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Carlson Wagonlit seyahat | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile Carlson Wagonlit seyahat arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 2745e165-94ab-43b1-970a-4547b4e5b501
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/19/2018
ms.author: jeedes
ms.openlocfilehash: e0fc48ed1c3a34945709c8a7e69dd3e8e37f536f
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2018
ms.locfileid: "35979863"
---
# <a name="tutorial-azure-active-directory-integration-with-carlson-wagonlit-travel"></a>Öğretici: Azure Active Directory Tümleştirme ile Carlson Wagonlit seyahat

Bu öğreticide, Azure Active Directory (Azure AD) ile Carlson Wagonlit seyahat tümleştirmek öğrenin.

Carlson Wagonlit seyahat Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Carlson Wagonlit seyahat erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak (çoklu oturum açma) Carlson Wagonlit seyahat açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Carlson Wagonlit seyahat ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Carlson Wagonlit seyahat çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Carlson Wagonlit seyahat ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-carlson-wagonlit-travel-from-the-gallery"></a>Galeriden Carlson Wagonlit seyahat ekleme
Azure AD Carlson Wagonlit seyahat tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Carlson Wagonlit seyahat eklemeniz gerekir.

**Galeriden Carlson Wagonlit seyahat eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Carlson Wagonlit seyahat**seçin **Carlson Wagonlit seyahat** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde Carlson Wagonlit seyahat](./media/carlsonwagonlit-tutorial/tutorial_carlsonwagonlittravel_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırmak ve Azure AD çoklu oturum açma Carlson Wagonlit "Britta Simon" adlı bir test kullanıcı tabanlı seyahat sınayın.

Tekli çalışmaya oturum için Azure AD Carlson Wagonlit seyahat karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Carlson Wagonlit seyahat ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Carlson Wagonlit seyahat değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Carlson Wagonlit seyahat ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Carlson Wagonlit seyahat test kullanıcısı oluşturma](#create-a-carlson-wagonlit-travel-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Carlson Wagonlit seyahat sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Carlson Wagonlit seyahat uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Carlson Wagonlit seyahat ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Carlson Wagonlit seyahat** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/carlsonwagonlit-tutorial/tutorial_carlsonwagonlittravel_samlbase.png)

3. Üzerinde **Carlson Wagonlit seyahat etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Carlson Wagonlit seyahat etki alanı ve URL'leri tek oturum açma bilgileri](./media/carlsonwagonlit-tutorial/tutorial_carlsonwagonlittravel_url.png)

    İçinde **tanımlayıcısı** metin değeri yazın: `cwt-stage`

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/carlsonwagonlit-tutorial/tutorial_carlsonwagonlittravel_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/carlsonwagonlit-tutorial/tutorial_general_400.png)

6. Çoklu oturum açma yapılandırmak için **Carlson Wagonlit seyahat** yan, indirilen göndermek için ihtiyacınız **meta veri XML** için [Carlson Wagonlit seyahat destek ekibi](http://www.carlsonwagonlit.in/content/cwt/in/en/technical-assistance.html). Bunlar, her iki tarafta da ayarlamanızı SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/carlsonwagonlit-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/carlsonwagonlit-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/carlsonwagonlit-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/carlsonwagonlit-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-carlson-wagonlit-travel-test-user"></a>Carlson Wagonlit seyahat test kullanıcısı oluşturma

Bu bölümde, Carlson Wagonlit seyahat Britta Simon adlı bir kullanıcı oluşturun. Çalışmak [Carlson Wagonlit seyahat destek ekibi](http://www.carlsonwagonlit.in/content/cwt/in/en/technical-assistance.html) Carlson Wagonlit seyahat platform kullanıcıları eklemek için. Kullanıcıların oluşturulan ve çoklu oturum açma kullanmadan önce etkinleştirilmelidir. 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Carlson Wagonlit seyahat erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Carlson Wagonlit seyahat Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Carlson Wagonlit seyahat**.

    ![Uygulamalar listesinde Carlson Wagonlit seyahat bağlantı](./media/carlsonwagonlit-tutorial/tutorial_carlsonwagonlittravel_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Carlson Wagonlit seyahat parçasında tıklattığınızda, otomatik olarak Carlson Wagonlit seyahat uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/carlsonwagonlittravel-tutorial/tutorial_general_01.png
[2]: ./media/carlsonwagonlittravel-tutorial/tutorial_general_02.png
[3]: ./media/carlsonwagonlittravel-tutorial/tutorial_general_03.png
[4]: ./media/carlsonwagonlittravel-tutorial/tutorial_general_04.png

[100]: ./media/carlsonwagonlittravel-tutorial/tutorial_general_100.png

[200]: ./media/carlsonwagonlittravel-tutorial/tutorial_general_200.png
[201]: ./media/carlsonwagonlittravel-tutorial/tutorial_general_201.png
[202]: ./media/carlsonwagonlittravel-tutorial/tutorial_general_202.png
[203]: ./media/carlsonwagonlittravel-tutorial/tutorial_general_203.png

