---
title: 'Öğretici: Azure Active Directory Tümleştirme ile LockPath Keylight | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile LockPath Keylight arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 234a32f1-9f56-4650-9e31-7b38ad734b1a
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: d7947a2a294b2232a69a403a765cc2cc29c43405
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36218325"
---
# <a name="tutorial-azure-active-directory-integration-with-lockpath-keylight"></a>Öğretici: Azure Active Directory Tümleştirme LockPath Keylight ile

Bu öğreticide, Azure Active Directory (Azure AD) ile LockPath Keylight tümleştirmek öğrenin.

LockPath Keylight Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- LockPath Keylight erişimi, Azure AD'de kontrol edebilirsiniz
- Azure AD hesaplarına otomatik olarak (çoklu oturum açma) için LockPath Keylight açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme LockPath Keylight ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir LockPath Keylight çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden LockPath Keylight ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-lockpath-keylight-from-the-gallery"></a>Galeriden LockPath Keylight ekleme
Azure AD LockPath Keylight tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden LockPath Keylight eklemeniz gerekir.

**Galeriden LockPath Keylight eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **LockPath Keylight**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/keylight-tutorial/tutorial_keylight_search.png)

5. Sonuçlar panelinde seçin **LockPath Keylight**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/keylight-tutorial/tutorial_keylight_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırmanız ve LockPath Keylight ile Azure AD çoklu oturum açmayı test "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı

Tekli çalışmaya oturum için Azure AD LockPath Keylight karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının LockPath Keylight ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

LockPath Keylight içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma LockPath Keylight ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[LockPath Keylight test kullanıcısı oluşturma](#creating-a-lockpath-keylight-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı LockPath Keylight sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma LockPath Keylight uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma LockPath Keylight ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **LockPath Keylight** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/keylight-tutorial/tutorial_keylight_samlbase.png)

3. Üzerinde **LockPath Keylight etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/keylight-tutorial/tutorial_keylight_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<company name>.keylightgrc.com/`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<company name>.keylightgrc.com`

    c. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<company name>.keylightgrc.com/Login.aspx`
    
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler, gerçek tanımlayıcı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Kişi [LockPath Keylight istemci destek ekibi](https://www.lockpath.com/contact/) bu değerleri almak için. 

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Raw)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/keylight-tutorial/tutorial_keylight_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/keylight-tutorial/tutorial_general_400.png)
    
6. Üzerinde **LockPath Keylight yapılandırma** 'yi tıklatın **yapılandırma LockPath Keylight** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/keylight-tutorial/tutorial_keylight_configure.png) 

7. LockPath Keylight SSO'yu etkinleştirmek için aşağıdaki adımları gerçekleştirin:
   
    a. LockPath Keylight hesabınız yönetici olarak oturum.
    
    b. Üstteki menüde tıklatın **kişi**seçip **Keylight Kurulum**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/keylight-tutorial/401.png) 

    c. Sol taraftaki treeview içinde tıklatın **SAML**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/keylight-tutorial/402.png) 

    d. Üzerinde **SAML ayarları** iletişim kutusunda, tıklatın **Düzenle**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/keylight-tutorial/404.png) 

8. Üzerinde **SAML ayarlarını Düzenle** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açmayı yapılandırın](./media/keylight-tutorial/405.png) 
   
    a. Ayarlama **SAML kimlik doğrulaması** için **etkin**.

    b. Yapıştır **SAML çoklu oturum açma hizmet URL'si** Azure portalından kopyaladığınız değeri **kimlik sağlayıcısı oturum açma URL'si** metin kutusu.

    c. Yapıştır **tek Sign-Out hizmeti URL'si** Azure portalından kopyaladığınız değeri **kimlik sağlayıcısı oturum kapatma URL'si** metin kutusu.

    d. Tıklatın **Dosya Seç** indirilen LockPath Keylight sertifikanızı seçin ve ardından **açık** sertifikayı karşıya yüklemek için.

    e. Ayarlama **SAML kullanıcı kimliği konumu** için **NameIdentifier öğesi konu deyiminin**.
    
    f. Sağlamak **Keylight hizmet sağlayıcısı** şu biçimi kullanarak: **https://&lt;ŞirketAdı&gt;. keylightgrc.com**.
    
    g. Ayarlama **otomatik sağlama kullanıcılar** için **etkin**.

    h. Ayarlama **otomatik sağlama hesap türü** için **tam kullanıcı**.

    i. Ayarlama **otomatik sağlama güvenlik rolü**seçin **SAML sahip standart kullanıcı**.
    
    j. Ayarlama **otomatik sağlama güvenlik config**seçin **standart kullanıcı yapılandırması**.
     
    k. İçinde **e-posta özniteliği** metin kutusuna, türü `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.
    
    l. İçinde **ad özniteliği** metin kutusuna, türü `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.
    
    m. İçinde **son name özniteliği** metin kutusuna, türü `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.
    
    n. **Kaydet**’e tıklayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/keylight-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/keylight-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/keylight-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/keylight-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-lockpath-keylight-test-user"></a>LockPath Keylight test kullanıcısı oluşturma

Bu bölümde, Britta Simon LockPath Keylight adlı bir kullanıcı oluşturun. LockPath Keylight tam zamanı sağlama, varsayılan olarak etkin olduğu destekler.

Bu bölümde, eylem öğe yok. Yeni bir kullanıcı, kullanıcı henüz yoksa LockPath Keylight erişirken oluşturulur. 

>[!NOTE]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, başvurmanız gerekir [LockPath Keylight istemci destek ekibi](https://www.lockpath.com/contact/). 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta LockPath Keylight erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**LockPath Keylight Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **LockPath Keylight**.

    ![Çoklu oturum açmayı yapılandırın](./media/keylight-tutorial/tutorial_keylight_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli LockPath Keylight parçasında tıklattığınızda, otomatik olarak LockPath Keylight uygulamanıza açan. 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/keylight-tutorial/tutorial_general_01.png
[2]: ./media/keylight-tutorial/tutorial_general_02.png
[3]: ./media/keylight-tutorial/tutorial_general_03.png
[4]: ./media/keylight-tutorial/tutorial_general_04.png

[100]: ./media/keylight-tutorial/tutorial_general_100.png

[200]: ./media/keylight-tutorial/tutorial_general_200.png
[201]: ./media/keylight-tutorial/tutorial_general_201.png
[202]: ./media/keylight-tutorial/tutorial_general_202.png
[203]: ./media/keylight-tutorial/tutorial_general_203.png

