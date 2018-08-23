---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Salesforce korumalı alan | Microsoft Docs'
description: Azure Active Directory ve Salesforce korumalı alan arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: ee54c39e-ce20-42a4-8531-da7b5f40f57c
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/07/2018
ms.author: jeedes
ms.openlocfilehash: 6feafba41cf65a752dd5bf0819b0b93bacff0aff
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2018
ms.locfileid: "42061075"
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce-sandbox"></a>Öğretici: Azure Active Directory Tümleştirme ile Salesforce korumalı alan

Bu öğreticide, Azure Active Directory (Azure AD) ile Salesforce korumalı alan tümleştirme konusunda bilgi edinin.

Sanal amaçları, geliştirme, test ve eğitim, verilere ve uygulamalara, Salesforce üretimde ödün vermeden gibi çeşitli ayrı ortamlarda kuruluşunuz birden çok kopyasını oluşturma olanağı sunar Kuruluş.
Daha fazla ayrıntı için [korumalı alan genel bakış](https://help.salesforce.com/articleView?id=create_test_instance.htm&language=en_us&type=5).

Salesforce korumalı alan Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Salesforce korumalı alan erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan (çoklu oturum açma) Salesforce korumalı alan için açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Salesforce korumalı alan yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Salesforce korumalı alan çoklu oturum açma abonelik etkin.

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Salesforce korumalı alan ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-salesforce-sandbox-from-the-gallery"></a>Galeriden Salesforce korumalı alan ekleme

Azure AD'de Salesforce korumalı alan tümleştirmesini yapılandırmak için Salesforce korumalı alan Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Salesforce korumalı alan eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Salesforce korumalı alan**seçin **Salesforce korumalı alan** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Salesforce korumalı alan](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırmanız ve Salesforce korumalı alanı ile Azure AD çoklu oturum açmayı test "Britta Simon" adlı bir test kullanıcı tabanlı.

Tek çalışmak için oturum açma için Azure AD ne karşılık gelen kullanıcı Salesforce korumalı alan içinde bir kullanıcının Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve Salesforce korumalı alan ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Salesforce korumalı alanında değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Salesforce korumalı alanı ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Salesforce korumalı alan test kullanıcısı oluşturma](#create-a-salesforce-sandbox-test-user)**  - kullanıcı Azure AD gösterimini bağlı Salesforce korumalı Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Salesforce korumalı alan uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Salesforce korumalı alan yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Salesforce korumalı alan** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma iletişim kutusu](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_samlbase.png)

3. Üzerinde **Salesforce korumalı alan etki alanı ve URL'ler** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** başlatılan modu:

   ![Salesforce korumalı alan etki alanı ve URL'ler tek oturum açma bilgileri](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_url1.png)

   İçinde **yanıt URL'si** metin yazın, kuruluşunuzun özel **yanıt URL'si**.

   > [!NOTE]
   > Yanıt URL'si değeri, bu öğreticinin ilerleyen bölümlerinde açıklanan gerçek yanıt URL'si ile güncelleştirin.

4. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(RAW)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_certificate.png)

5. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/salesforce-sandbox-tutorial/tutorial_general_400.png)

6. Üzerinde **Salesforce korumalı alan yapılandırma** bölümünde **yapılandırma Salesforce korumalı alan** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_configure.png)

7. Tarayıcınızda yeni bir sekme açın ve Salesforce korumalı alan yönetici hesabınızda oturum açın.

8. Tıklayarak **Kurulum** altında **ayarlar simgesine** sayfanın sağ üst köşesinde.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/configure1.png)

9. Ekranı aşağı kaydırarak **ayarları** sol gezinti bölmesinden **kimlik** ilgili bölümü genişletin. Ardından **çoklu oturum açma ayarları**.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/sf-admin-sso.png)

10. Üzerinde **çoklu oturum açma ayarları** sayfasında **Düzenle** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/configure3.png)

11. Seçin **SAML etkin**ve ardından **Kaydet**.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/sf-enable-saml.png)

12. SAML çoklu oturum açma ayarlarınızı yapılandırmak için tıklayın **yeni**.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/sf-admin-sso-new.png)

13. Üzerinde **çoklu oturum açma ayarları** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/sf-saml-config1.png)

    a. Seçin **SAML etkin** onay kutusu.

    b. İçinde **veren** alan, değerini yapıştırın **SAML varlık kimliği**, hangi Azure Portalı'ndan kopyaladığınız.

    c. Karşıya yüklenecek **kimlik sağlayıcısı sertifikası**, tıklayın **Gözat** ve Azure portalından indirdiğiniz sertifika dosyasını seçmek için.

    d. İçinde **kimlik sağlayıcısı oturum açma URL'si** metin değerini yapıştırın **çoklu oturum açma hizmeti URL'si**, hangi Azure Portalı'ndan kopyaladığınız.

    e. Olarak **SAML kimlik türü**, aşağıdaki seçeneklerden birini belirleyin:

      * Seçin **onaylamayı kullanıcının Salesforce kullanıcı adını içeren**, kullanıcının Salesforce kullanıcı adı SAML onayı iletilmezse

      * Seçin **onaylamayı içeren kullanıcı nesnesinden Federasyon kimliği**, SAML onaylama işlemi içinde geçirilen kullanıcı nesnesinden Federasyon kimliği
  
    f. Olarak **SAML kimlik konumu**seçin **kimliğidir Attribute öğesi**.

    g. SAML oturum kapatma SFDC desteklemez.  Geçici bir çözüm olarak yapıştırmak `https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0` içine **özel oturum kapatma URL'si** metin.

    h. **Kaydet**’e tıklayın.

14. Üzerinde **çoklu oturum açma ayarları** sayfasında **meta verileri indirme** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/configure4.png)

15. İndirilen meta verileri farklı bir tarayıcı penceresi ve kopyalama açın **konumu** yapıştırın ve değer **yanıt URL'si** metin üzerinde **Salesforce korumalı alan etki alanı ve URL'ler**bölümünde Azure portalında.  

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/configure5.png)

16. Uygulamada yapılandırmak istiyorsanız **SP** başlatılan modu, aşağıdaki ilgili Önkoşullar şunlardır:

    a. Doğrulanmış bir etki alanı olmalıdır.

    b. Salesforce korumalı alan etki alanınızda etkinleştirmeniz ve yapılandırmanız gerekir, bu adımlar, bu öğreticinin ilerleyen bölümlerinde açıklanmıştır.

    c. Azure portalında, üzerinde **Salesforce korumalı alan etki alanı ve URL'ler** bölümünde onay **Gelişmiş URL ayarlarını göster** ve aşağıdaki adımı uygulayın:
  
    ![Salesforce korumalı alan etki alanı ve URL'ler tek oturum açma bilgileri](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_url.png)

    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak değeri yazın: `https://<instancename>--Sandbox.<entityid>.my.salesforce.com`

    > [!NOTE]
    > Etki alanı etkinleştirdikten sonra bu değer Salesforce korumalı alan portaldan kopyalanmalıdır.

17. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(RAW)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_certificate.png)

18. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/salesforce-sandbox-tutorial/tutorial_general_400.png)

19. Üzerinde **Salesforce korumalı alan yapılandırma** bölümünde **yapılandırma Salesforce korumalı alan** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_configure.png)

20. Tarayıcınızda yeni bir sekme açın ve Salesforce korumalı alan yönetici hesabınızda oturum açın.

21. Tıklayarak **Kurulum** altında **ayarlar simgesine** sayfanın sağ üst köşesinde.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/configure1.png)

22. Ekranı aşağı kaydırarak **ayarları** sol gezinti bölmesinden **kimlik** ilgili bölümü genişletin. Ardından **çoklu oturum açma ayarları**.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/sf-admin-sso.png)

23. Üzerinde **çoklu oturum açma ayarları** sayfasında **Düzenle** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/configure3.png)

24. Seçin **SAML etkin**ve ardından **Kaydet**.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/sf-enable-saml.png)

25. SAML çoklu oturum açma ayarlarınızı yapılandırmak için tıklayın **yeni**.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/sf-admin-sso-new.png)

26. İkinci bir örneği ekliyorsanız, yukarıda belirtildiği bir etki alanı etkinleştirmeniz gerekiyor (SP tarafından başlatılan durumda). SAML çoklu oturum açma ayarları bölümünde aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/sf-saml-config.png)

    a. İçinde **adı** metin yapılandırma adını yazın (örneğin: *SPSSOWAAD_Test*).

    b. İçinde **veren** alan, değerini yapıştırın **SAML varlık kimliği**, hangi Azure Portalı'ndan kopyaladığınız.

    c. İçinde **varlık kimliği** metin kullanım `https://test.salesforce.com` ilk örnek için değer ve ikinci uygulama örneğinden Kiracı belirli tanımlayıcı değerini kullanabilirsiniz.

    d. Karşıya yüklenecek **kimlik sağlayıcısı sertifikası**, tıklayın **Dosya Seç** ve Azure portalından indirdiğiniz sertifika dosyasını seçmek için.

    e. Olarak **SAML kimlik türü**, aşağıdaki seçeneklerden birini belirleyin:

      * Seçin **onaylamayı kullanıcının Salesforce kullanıcı adını içeren**, kullanıcının Salesforce kullanıcı adı SAML onayı iletilmezse

      * Seçin **onaylamayı içeren kullanıcı nesnesinden Federasyon kimliği**, SAML onaylama işlemi içinde geçirilen kullanıcı nesnesinden Federasyon kimliği

      * Seçin **onaylamayı içeren kullanıcı nesnesinin kullanım Kimliğinden**, SAML onaylama işlemi içinde geçirilen kullanıcı nesnesinden kullanıcı kimliği

    f. Olarak **SAML kimlik konumu**seçin **kimliğidir konu deyiminin NameIdentifier öğesinde**.

    g. Olarak **hizmet sağlayıcısı tarafından başlatılan bağlama isteği**seçin **HTTP POST**.

    h. İçinde **kimlik sağlayıcısı oturum açma URL'si** metin değerini yapıştırın **çoklu oturum açma hizmeti URL'si**, hangi Azure Portalı'ndan kopyaladığınız.

    i. SAML oturum kapatma SFDC desteklemez.  Geçici bir çözüm olarak yapıştırmak `https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0` içine **özel oturum kapatma URL'si** metin.

    j. **Kaydet**’e tıklayın.

27. Salesforce korumalı alan etki alanınızda etkinleştirmek için aşağıdaki adımları gerçekleştirin:

    > [!NOTE]
    > Etki alanı etkinleştirmeden önce aynı Salesforce korumalı alan üzerinde oluşturmanız gerekir. Daha fazla bilgi için [etki alanı adınız tanımlama](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US). Etki alanı oluşturulduktan sonra Lütfen doğru yapılandırıldığından emin olun.

    * Sol gezinti bölmesinde Salesforce korumalı alan içinde tıklatın **şirket ayarları** ilgili bölümü genişletin ve ardından **My Domain**.

         ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/sf-my-domain.png)

    * İçinde **kimlik doğrulama Yapılandırması** bölümünde **Düzenle**.

        ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/sf-edit-auth-config.png)

    * İçinde **kimlik doğrulama Yapılandırması** bölümünde olarak **kimlik doğrulama hizmeti**, SAML çoklu oturum açma Salesforce korumalı alan SSO yapılandırması sırasında ayarlanan ayar adını seçin ve tıklayın **Kaydet**.

        ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/configure2.png)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/salesforce-sandbox-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/salesforce-sandbox-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/salesforce-sandbox-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/salesforce-sandbox-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.

### <a name="create-a-salesforce-sandbox-test-user"></a>Salesforce korumalı alan test kullanıcısı oluşturma

Bu bölümde, Britta Simon adlı bir kullanıcı, Salesforce korumalı alanında oluşturulur. Salesforce korumalı alan tam zamanında sağlama, varsayılan olarak etkin olduğu destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Salesforce korumalı alan erişmeye çalıştığında, Salesforce korumalı alan içinde bir kullanıcı zaten mevcut değilse yeni bir tane oluşturulur. Salesforce korumalı alan da otomatik kullanıcı sağlamayı destekler, daha fazla ayrıntı bulabilirsiniz [burada](salesforce-sandbox-provisioning-tutorial.md) otomatik kullanıcı sağlamayı yapılandırma.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Salesforce korumalı alan erişimi vererek Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200]

**Salesforce korumalı alan Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **Salesforce korumalı alan**.

    ![Uygulamalar listesinde Salesforce korumalı alan bağlantı](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_app.png)  

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Salesforce korumalı alan kutucuğa tıkladığınızda, otomatik olarak Salesforce korumalı alan uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
* [Kullanıcı sağlamayı yapılandırma](salesforce-sandbox-provisioning-tutorial.md)


<!--Image references-->

[1]: ./media/salesforce-sandbox-tutorial/tutorial_general_01.png
[2]: ./media/salesforce-sandbox-tutorial/tutorial_general_02.png
[3]: ./media/salesforce-sandbox-tutorial/tutorial_general_03.png
[4]: ./media/salesforce-sandbox-tutorial/tutorial_general_04.png

[100]: ./media/salesforce-sandbox-tutorial/tutorial_general_100.png

[200]: ./media/salesforce-sandbox-tutorial/tutorial_general_200.png
[201]: ./media/salesforce-sandbox-tutorial/tutorial_general_201.png
[202]: ./media/salesforce-sandbox-tutorial/tutorial_general_202.png
[203]: ./media/salesforce-sandbox-tutorial/tutorial_general_203.png