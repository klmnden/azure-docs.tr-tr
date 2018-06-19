---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Rollbar | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile Rollbar arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 57537e54-9388-4272-a610-805ce45a451f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 1/04/2017
ms.author: jeedes
ms.openlocfilehash: 8dcf30a5dc018b396501563044a8c1ca1cb3b7d9
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2018
ms.locfileid: "35972188"
---
# <a name="tutorial-azure-active-directory-integration-with-rollbar"></a>Öğretici: Azure Active Directory Tümleştirme Rollbar ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Rollbar tümleştirmek öğrenin.

Rollbar Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Rollbar erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak için Rollbar (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Rollbar ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Rollbar çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Rollbar ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-rollbar-from-the-gallery"></a>Galeriden Rollbar ekleme
Azure AD Rollbar tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Rollbar eklemeniz gerekir.

**Galeriden Rollbar eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Rollbar**seçin **Rollbar** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde Rollbar](./media/rollbar-tutorial/tutorial_rollbar_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Rollbar sınayın.

Tekli çalışmaya oturum için Azure AD Rollbar karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Rollbar ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Rollbar içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Rollbar ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Rollbar test kullanıcısı oluşturma](#create-a-rollbar-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Rollbar sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Rollbar uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Rollbar yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Rollbar** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/rollbar-tutorial/tutorial_rollbar_samlbase.png)

3. Üzerinde **Rollbar etki alanı ve URL'leri** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** modu tarafından başlatılan:

    ![Rollbar etki alanı ve URL'leri tek oturum açma bilgileri](./media/rollbar-tutorial/tutorial_rollbar_url.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, URL'yi yazın: `https://saml.rollbar.com`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://rollbar.com/<accountname>/saml/sso/azure/`

4. Denetleme **Göster Gelişmiş URL ayarları** ve uygulamada yapılandırmak istiyorsanız aşağıdaki adımı gerçekleştirin **SP** modunda başlatılan:

    ![Rollbar etki alanı ve URL'leri tek oturum açma bilgileri](./media/rollbar-tutorial/tutorial_rollbar_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://rollbar.com/<accountname>/saml/login/azure/`
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Kişi [Rollbar istemci destek ekibi](mailto:support@rollbar.com) bu değerleri almak için. 

5. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/rollbar-tutorial/tutorial_rollbar_certificate.png) 

6. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/rollbar-tutorial/tutorial_general_400.png)
    
7. Farklı web tarayıcısı penceresinde Rollbar şirket sitenize yönetici olarak oturum açın.

8. Tıklayın **profil ayarları** 'ye tıklayın ve sağ üst köşesinde **hesap adı ayarları**.
    
    ![Yapılandırma](./media/rollbar-tutorial/general.png)

9. Tıklatın **kimlik sağlayıcısı** güvenlik altında.

    ![Yapılandırma](./media/rollbar-tutorial/configure1.png)

10. İçinde **SAML kimlik sağlayıcısı** bölümünde, aşağıdaki adımları gerçekleştirin:
    
    ![Yapılandırma](./media/rollbar-tutorial/configure2.png)

    a. Seçin **AZURE** gelen **SAML kimlik sağlayıcısı** açılır.

    b. Meta veri dosyasını Not Defteri'nde açın, içeriğini, panoya kopyalayın ve yapıştırın kendisine **SAML meta veri** metin kutusu.

    c. **Kaydet**’e tıklayın.

11. Kaydet'ı tıklattıktan sonra düğmesi, ekran olacak şuna benzer:
    
    ![Yapılandırma](./media/rollbar-tutorial/configure3.png)
    > [!NOTE] 
    > Aşağıdaki adımı tamamlamak için önce kendiniz bir kullanıcı olarak Azure Rollbar uygulamada eklemeniz gerekir.
    a. Tüm kullanıcıların Azure üzerinden kimlik doğrulaması ve ardından gerekli olmasını istediğiniz **kimlik sağlayıcınız oturum açma** Azure üzerinden yeniden kimlik doğrulaması için.  

    b.  Ekrana dönersiniz sonra seçeneğini **SAML kimlik sağlayıcısı üzerinden oturum açma gerektirir** onay kutusu.

    b. **Kaydet**’e tıklayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/rollbar-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/rollbar-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/rollbar-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/rollbar-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-rollbar-test-user"></a>Rollbar test kullanıcısı oluşturma

Azure AD kullanıcıları için Rollbar oturum açmak etkinleştirmek için bunların Rollbar sağlanmalıdır. Rollbar söz konusu olduğunda, sağlama bir el ile bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Rollbar şirket sitenize yönetici olarak oturum açın.

2. Tıklayın **profil ayarları** 'ye tıklayın ve sağ üst köşesinde **hesap adı ayarları**.

    ![Kullanıcı](./media/rollbar-tutorial/general.png)

3. **Kullanıcılar**’a tıklayın.
    
    ![Çalışanı ekleyin](./media/rollbar-tutorial/user1.png)

4. Tıklatın **ekip üyelerinin davet**.

    ![Kişileri davet edin](./media/rollbar-tutorial/user2.png)

5. Metin kutusuna gibi kullanıcı adını girin **brittasimon@contoso.com** ve **Ekle/davet**.

    ![Kişileri davet edin](./media/rollbar-tutorial/user3.png)

6. Kullanıcı bir davet alır ve kabul ettikten sonra klasöründe sistemde oluşturulur.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Rollbar için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Rollbar için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Rollbar**.

    ![Uygulamalar listesinde Rollbar bağlantı](./media/rollbar-tutorial/tutorial_rollbar_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Rollbar parçasında tıklattığınızda, otomatik olarak Rollbar uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/rollbar-tutorial/tutorial_general_01.png
[2]: ./media/rollbar-tutorial/tutorial_general_02.png
[3]: ./media/rollbar-tutorial/tutorial_general_03.png
[4]: ./media/rollbar-tutorial/tutorial_general_04.png

[100]: ./media/rollbar-tutorial/tutorial_general_100.png

[200]: ./media/rollbar-tutorial/tutorial_general_200.png
[201]: ./media/rollbar-tutorial/tutorial_general_201.png
[202]: ./media/rollbar-tutorial/tutorial_general_202.png
[203]: ./media/rollbar-tutorial/tutorial_general_203.png

