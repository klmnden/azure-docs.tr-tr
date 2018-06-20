---
title: 'Öğretici: Azure Active Directory Tümleştirme Keeper parola Yöneticisi & dijital kasası ile | Microsoft Docs'
description: Çoklu oturum açma Keeper parola Yöneticisi & dijital kasası ve Azure Active Directory arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: e1a98f6a-2dae-4734-bdbf-4fba742a61d2
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 3ef20cdb0f6a7b5afd624a7910495ee784140c48
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36219991"
---
# <a name="tutorial-azure-active-directory-integration-with-keeper-password-manager--digital-vault"></a>Öğretici: Azure Active Directory Tümleştirme Keeper parola Yöneticisi & dijital kasası

Bu öğreticide, Azure Active Directory (Azure AD) ile Keeper parola Yöneticisi & dijital kasası tümleştirme öğrenin.

Keeper parola Yöneticisi & dijital kasası Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Keeper parola Yöneticisi & dijital kasa erişimi, Azure AD'de kontrol edebilirsiniz
- Azure AD hesaplarına otomatik olarak Keeper parola Yöneticisi & dijital kasası (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Keeper parola Yöneticisi & dijital kasası ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Keeper parola Yöneticisi & dijital kasası çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Keeper parola Yöneticisi & dijital kasası ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-keeper-password-manager--digital-vault-from-the-gallery"></a>Galeriden Keeper parola Yöneticisi & dijital kasası ekleme
Azure AD Keeper parola Yöneticisi & dijital kasası tümleştirilmesi yapılandırmak için Keeper parola Yöneticisi & dijital kasası Galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Keeper parola Yöneticisi & dijital kasası Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Keeper parola Yöneticisi & dijital kasa**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/keeperpasswordmanager-tutorial/tutorial_keeper_search.png)

5. Sonuçlar panelinde seçin **Keeper parola Yöneticisi & dijital kasa**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/keeperpasswordmanager-tutorial/tutorial_keeper_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Keeper parola Yöneticisi & dijital "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı kasası ile test etme

Tekli çalışmaya oturum için Azure AD ne karşılık gelen Keeper parola Yöneticisi & dijital kasa içinde bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Keeper parola Yöneticisi & dijital kasası ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Keeper parola Yöneticisi & dijital kasa içinde değerini Ata **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Keeper parola Yöneticisi & dijital kasası ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Keeper parola Yöneticisi & dijital kasası test kullanıcısı oluşturma](#creating-a-keeperpasswordmanager-test-user)**  - Britta Simon Keeper parola Yöneticisi & kullanıcı Azure AD gösterimini bağlı dijital kasası, karşılık gelen sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Keeper parola Yöneticisi & dijital kasası uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Keeper parola Yöneticisi & dijital kasası ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Keeper parola Yöneticisi & dijital kasa** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/keeperpasswordmanager-tutorial/tutorial_keeper_samlbase.png)

3. Üzerinde **Keeper parola Yöneticisi & dijital kasası etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/keeperpasswordmanager-tutorial/tutorial_keeper_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://{SSO CONNECT SERVER}/sso-connect/saml/login`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://{SSO CONNECT SERVER}/sso-connect/saml/sso`

    c. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://{SSO CONNECT SERVER}/sso-connect`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Kişi [Keeper parola Yöneticisi ve dijital kasası istemci desteği takım](https://keepersecurity.com/contact.html) bu değerleri almak için. 

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/keeperpasswordmanager-tutorial/tutorial_keeper_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/keeperpasswordmanager-tutorial/tutorial_general_400.png)
    
6. Üzerinde **Keeper parola Yöneticisi & dijital kasası yapılandırma** 'yi tıklatın **Keeper parola Yöneticisi yapılandırma & dijital kasa** açmak için **yapılandırma oturum açma** penceresini açın. Kopya **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/keeperpasswordmanager-tutorial/tutorial_keeper_configure.png) 

7. Çoklu oturum açma yapılandırmak için **Keeper parola Yöneticisi & dijital kasası yapılandırma** tarafı, sırasında verilen yönergeleri izleyin [Keeper destek Kılavuzu](https://keepersecurity.com/assets/pdf/SettingupAzurewithKeeperSSOConnect.pdf)

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/keeperpasswordmanager-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/keeperpasswordmanager-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/keeperpasswordmanager-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/keeperpasswordmanager-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-keeper-password-manager--digital-vault-test-user"></a>Keeper parola Yöneticisi & dijital kasası test kullanıcısı oluşturma

Azure AD kullanıcılarının Keeper parola Yöneticisi & dijital kasası oturum açmayı etkinleştirmek için bunlar Keeper parola Yöneticisi & dijital kasası sağlanmalıdır. Uygulama, süresi kullanıcı sağlama ve kimlik doğrulama kullanıcılar uygulamada otomatik olarak oluşturulacak sonra hemen destekler. Sizinle iletişim [Keeper Destek](https://keepersecurity.com/contact.html), kullanıcıları el ile ayarlamak istiyorsanız.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta Keeper parola Yöneticisi & dijital kasası erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Britta Simon Keeper parola Yöneticisi & dijital kasası atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Keeper parola Yöneticisi & dijital kasa**.

    ![Çoklu oturum açmayı yapılandırın](./media/keeperpasswordmanager-tutorial/tutorial_keeper_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Keeper parola Yöneticisi & dijital kasası parçasında tıkladığınızda, oturum açma sayfasına Keeper parola Yöneticisi & dijital kasası uygulamasının almanız gerekir. Başarılı kimlik doğrulaması sırasında uygulamasına almanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/keeperpasswordmanager-tutorial/tutorial_general_01.png
[2]: ./media/keeperpasswordmanager-tutorial/tutorial_general_02.png
[3]: ./media/keeperpasswordmanager-tutorial/tutorial_general_03.png
[4]: ./media/keeperpasswordmanager-tutorial/tutorial_general_04.png

[100]: ./media/keeperpasswordmanager-tutorial/tutorial_general_100.png

[200]: ./media/keeperpasswordmanager-tutorial/tutorial_general_200.png
[201]: ./media/keeperpasswordmanager-tutorial/tutorial_general_201.png
[202]: ./media/keeperpasswordmanager-tutorial/tutorial_general_202.png
[203]: ./media/keeperpasswordmanager-tutorial/tutorial_general_203.png

