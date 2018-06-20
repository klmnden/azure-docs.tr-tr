---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Pingboard | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile Pingboard arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2018
ms.author: jeedes
ms.openlocfilehash: e07e85e60c8a4b93e4b0fd7bf43f470c4e3acc61
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36221198"
---
# <a name="tutorial-azure-active-directory-integration-with-pingboard"></a>Öğretici: Azure Active Directory Tümleştirme Pingboard ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Pingboard tümleştirmek öğrenin.

Pingboard Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Pingboard erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için Pingboard (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Pingboard ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Pingboard çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Pingboard ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-pingboard-from-the-gallery"></a>Galeriden Pingboard ekleme
Azure AD Pingboard tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Pingboard eklemeniz gerekir.

**Galeriden Pingboard eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kuruluş uygulamaları][2]

3. Tıklatın **Ekle** iletişim kutusunun üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Pingboard**seçin **Pingboard** sonuç paneli ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Sonuçlar listesinde Pingboard](./media/pingboard-tutorial/tutorial_pingboard_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Pingboard sınayın.

Tekli çalışmaya oturum için Azure AD Pingboard karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Pingboard ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Bu bağlantı değeri atayarak ilişkisi **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** Pingboard içinde.

Yapılandırma ve Azure AD çoklu oturum açma Pingboard ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Pingboard test kullanıcısı oluşturma](#create-a-pingboard-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Pingboard sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Pingboard uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Pingboard yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Pingboard** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2.  Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma iletişim kutusu](./media/pingboard-tutorial/tutorial_pingboard_samlbase.png)

3. Üzerinde **Pingboard etki alanı ve URL'leri** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** modu tarafından başlatılan:

    ![Pingboard etki alanı ve URL'leri tek oturum açma bilgilerini IDP](./media/pingboard-tutorial/tutorial_pingboard_url.png)

    a. İçinde **tanımlayıcısı** metin değeri olarak yazın: `http://app.pingboard.com/sp`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<entity-id>.pingboard.com/auth/saml/consume`

4. Denetleme **Göster Gelişmiş URL ayarları**, uygulamada yapılandırmak istiyorsanız **SP** modunda başlatılan:

    ![Pingboard etki alanı ve URL'leri tek oturum açma bilgilerini SP](./media/pingboard-tutorial/tutorial_pingboard_sp_initiated01.png)

     İçinde **oturum açma URL'si** metin kutusuna, şu biçimi kullanarak URL'yi yazın: `https://<sub-domain>.pingboard.com/sign_in`

    > [!NOTE]
    > Lütfen bu değerleri gerçek olmadığına dikkat edin. Bu değerler gerçek yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Kişi [Pingboard istemci destek ekibi](https://support.pingboard.com/) bu değerleri almak için.

5. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve XML dosyayı bilgisayarınıza kaydedin.

    ![Pingboard meta veri XML'i](./media/pingboard-tutorial/tutorial_pingboard_certificate.png)

6. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/pingboard-tutorial/tutorial_general_400.png)

7. SSO Pingboard tarafında yapılandırmak için yeni bir tarayıcı penceresi açın ve Pingboard hesabınızda oturum açın. Çoklu oturum açma ayarlamak için Pingboard yönetici olması gerekir.

8. Üstteki menüden,, seçin **uygulamaları > tümleştirmeler**

    ![Çoklu oturum açmayı yapılandırın](./media/pingboard-tutorial/Pingboard_integration.png)

9. Üzerinde **tümleştirmeler** sayfasında, Bul **"Azure Active Directory"** döşeme ve tıklatın.

    ![Oturum açma Pingboard tek tümleştirme](./media/pingboard-tutorial/Pingboard_aad.png)

10. Tıklatın izleyen kalıcı olarak **"Yapılandırma"**

    ![Pingboard Yapılandırma düğmesi](./media/pingboard-tutorial/Pingboard_configure.png)

11. Aşağıdaki sayfada, "Azure SSO tümleştirme etkinleştirildiğini" dikkat edin. İndirilen meta veri XML dosyasını Not Defteri'nde açın ve içeriği yapıştırmak **IDP meta verileri**.

    ![Pingboard SSO yapılandırma ekranında](./media/pingboard-tutorial/Pingboard_sso_configure.png)

12. Dosya doğrulanır ve her şey doğruysa, çoklu oturum açma şimdi etkinleştirilir.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](./media/pingboard-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/pingboard-tutorial/create_aaduser_02.png)

3. İletişim kutusunun üstündeki **Ekle** açmak için **kullanıcı** iletişim.

    ![Ekle düğmesi](./media/pingboard-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/pingboard-tutorial/create_aaduser_04.png)

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.

### <a name="create-a-pingboard-test-user"></a>Pingboard test kullanıcısı oluşturma

Bu bölümün amacı Britta Simon içinde Pingboard adlı bir kullanıcı oluşturmaktır. Pingboard otomatik kullanıcı hazırlama, varsayılan olarak etkin olduğu destekler. Daha fazla ayrıntı bulabilirsiniz [burada](pingboard-provisioning-tutorial.md) otomatik kullanıcı sağlamayı yapılandırma.

**Kullanıcı el ile oluşturmanız gerekiyorsa, şu adımları gerçekleştirin:**

1. Pingboard şirket sitenize yönetici olarak oturum açın.

2. Tıklatın **"Çalışan Ekle"** düğmesini **Directory** sayfası.

    ![Çalışanı ekleyin](./media/pingboard-tutorial/create_testuser_add.png)

3. Üzerinde **"Çalışan Ekle"** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Kişileri davet edin](./media/pingboard-tutorial/create_testuser_name.png)

    a. İçinde **tam adı** kullanıcı türü tam adı metin kutusuna, ister **Britta Simon**.

    b. İçinde **e-posta** metin kutusuna, kullanıcının e-posta adresi türü ister **brittasimon@contoso.com**.

    c. İçinde **iş unvanı** metin kutusuna, Britta Simon iş unvanı yazın.

    d. İçinde **konumu** açılan listesinde, Britta Simon konumunu seçin.

    e. **Ekle**'ye tıklayın.

4. Kullanıcı eklenmesi onaylamak için bir onay ekranı göründüğünde.

    ![Onayla](./media/pingboard-tutorial/create_testuser_confirm.png)

    > [!NOTE]
    > Azure Active Directory hesap sahibi bir e-posta alır ve bunu etkinleştirilmeden önce kendi hesabı onaylamak için bir bağlantı izler.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Pingboard için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Pingboard için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Pingboard**.

    ![Uygulamalar listesinde Pingboard bağlantı](./media/pingboard-tutorial/tutorial_pingboard_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md).

Erişim paneli Pingboard parçasında tıklattığınızda, otomatik olarak Pingboard uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)
* [Kullanıcı sağlamayı Yapılandır](pingboard-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/pingboard-tutorial/tutorial_general_01.png
[2]: ./media/pingboard-tutorial/tutorial_general_02.png
[3]: ./media/pingboard-tutorial/tutorial_general_03.png
[4]: ./media/pingboard-tutorial/tutorial_general_04.png

[100]: ./media/pingboard-tutorial/tutorial_general_100.png

[200]: ./media/pingboard-tutorial/tutorial_general_200.png
[201]: ./media/pingboard-tutorial/tutorial_general_201.png
[202]: ./media/pingboard-tutorial/tutorial_general_202.png
[203]: ./media/pingboard-tutorial/tutorial_general_203.png