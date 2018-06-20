---
title: 'Öğretici: Azure Active Directory Tümleştirme OneTrust gizlilik yönetim yazılımı ile | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ve OneTrust gizlilik yönetim yazılımı arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 71c2b6d0-3d28-4130-a2c8-1e72ab3d5814
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: jeedes
ms.openlocfilehash: 744211174440b4bb60700f6d6d71cac289f7b56e
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36230311"
---
# <a name="tutorial-azure-active-directory-integration-with-onetrust-privacy-management-software"></a>Öğretici: Azure Active Directory Tümleştirme OneTrust gizlilik yönetim yazılımı ile

Bu öğreticide, Azure Active Directory (Azure AD) ile OneTrust gizlilik yönetim yazılımı tümleştirmek öğrenin.

OneTrust gizlilik yönetim yazılımı Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- OneTrust gizlilik yönetim yazılımı erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak OneTrust gizlilik yönetim yazılımı (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme OneTrust gizlilik yönetim yazılımı ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir OneTrust gizlilik yönetim yazılımı çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden OneTrust gizlilik yönetim yazılımı ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-onetrust-privacy-management-software-from-the-gallery"></a>Galeriden OneTrust gizlilik yönetim yazılımı ekleme
Azure AD OneTrust gizlilik yönetim yazılımı tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden OneTrust gizlilik yönetim yazılımı eklemeniz gerekir.

**Galeriden OneTrust gizlilik yönetim yazılımı eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna yazın **OneTrust gizlilik yönetim yazılımı**seçin **OneTrust gizlilik yönetim yazılımı** sonuç panelinden ardından **Ekle** Ekle düğmesi uygulama.

    ![Sonuçlar listesinde OneTrust gizlilik yönetim yazılımı](./media/onetrust-tutorial/tutorial_onetrust_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı OneTrust gizlilik yönetim yazılımı ile test etme.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen OneTrust gizlilik yönetim yazılımı, bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının OneTrust gizlilik yönetim yazılımı ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Değeri OneTrust gizlilik yönetim yazılımda atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma OneTrust gizlilik yönetim yazılımı ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[OneTrust gizlilik yönetim yazılımı test kullanıcısı oluşturma](#create-a-onetrust-privacy-management-software-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı OneTrust gizlilik yönetim yazılımı sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma OneTrust gizlilik yönetim yazılımı uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma OneTrust gizlilik yönetim yazılımı ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **OneTrust gizlilik yönetim yazılımı** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/onetrust-tutorial/tutorial_onetrust_samlbase.png)

3. Üzerinde **OneTrust gizlilik yönetim yazılımı etki alanı ve URL'leri** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** modu tarafından başlatılan:

    ![OneTrust gizlilik yönetim yazılımı etki alanı ve URL'leri tek oturum açma bilgileri](./media/onetrust-tutorial/tutorial_onetrust_url.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, bir URL yazın: `https://www.onetrust.com/saml2`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<subdomain>.onetrust.com/auth/consumerservice`

4. Denetleme **Göster Gelişmiş URL ayarları** ve uygulamada yapılandırmak istiyorsanız aşağıdaki adımı gerçekleştirin **SP** modunda başlatılan:

    ![OneTrust gizlilik yönetim yazılımı etki alanı ve URL'leri tek oturum açma bilgileri](./media/onetrust-tutorial/tutorial_onetrust_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<subdomain>.onetrust.com/auth/login`
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Fiili yanıt URL'si ve oturum açma URL'si ile bu değerleri güncelleştirin. Kişi [OneTrust gizlilik yönetim yazılımı istemci destek ekibi](mailto:support@onetrust.com) bu değerleri almak için. 

5. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/onetrust-tutorial/tutorial_onetrust_certificate.png) 

6. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/onetrust-tutorial/tutorial_general_400.png)

7. Çoklu oturum açma yapılandırmak için **OneTrust gizlilik yönetim yazılımı** yan, indirilen göndermek için ihtiyacınız **meta veri XML** için [OneTrust gizlilik yönetim yazılımı destek ekibi](mailto:support@onetrust.com). Bunlar, her iki tarafta da ayarlamanızı SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/onetrust-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/onetrust-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/onetrust-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/onetrust-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-onetrust-privacy-management-software-test-user"></a>OneTrust gizlilik yönetim yazılımı test kullanıcısı oluşturma

Bu bölümün amacı Britta Simon OneTrust gizlilik yönetim yazılım adlı bir kullanıcı oluşturmaktır. OneTrust gizlilik yönetim yazılımı yalnızca zaman sağlama, varsayılan olarak etkin olduğu destekler. Bu bölümde, eylem öğe yok. Yeni bir kullanıcı henüz yoksa OneTrust gizlilik yönetim yazılımı erişme denemesi sırasında oluşturulur.

>[!Note]
>Bir kullanıcı el ile oluşturmanız gerekirse başvurun [OneTrust gizlilik yönetim yazılımı destek ekibi](mailto:support@onetrust.com).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta OneTrust gizlilik yönetim yazılımı erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon OneTrust gizlilik yönetim yazılımı atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **OneTrust gizlilik yönetim yazılımı**.

    ![Uygulamalar listesinde OneTrust gizlilik yönetim yazılımı bağlantı](./media/onetrust-tutorial/tutorial_onetrust_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli OneTrust gizlilik yönetim yazılımı parçasında tıklattığınızda, otomatik olarak OneTrust gizlilik yönetim yazılımı uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/onetrust-tutorial/tutorial_general_01.png
[2]: ./media/onetrust-tutorial/tutorial_general_02.png
[3]: ./media/onetrust-tutorial/tutorial_general_03.png
[4]: ./media/onetrust-tutorial/tutorial_general_04.png

[100]: ./media/onetrust-tutorial/tutorial_general_100.png

[200]: ./media/onetrust-tutorial/tutorial_general_200.png
[201]: ./media/onetrust-tutorial/tutorial_general_201.png
[202]: ./media/onetrust-tutorial/tutorial_general_202.png
[203]: ./media/onetrust-tutorial/tutorial_general_203.png

