---
title: 'Öğretici: Salesforce korumalı alan Azure Active Directory Tümleştirme | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ve Salesforce korumalı alan arasında yapılandırmayı öğrenin.
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
ms.date: 05/16/2018
ms.author: jeedes
ms.openlocfilehash: 3d362228406b71e01bf74aa6082a268d4b603830
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36219049"
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce-sandbox"></a>Öğretici: Azure Active Directory Tümleştirme ile Salesforce korumalı alan

Bu öğreticide, Azure Active Directory (Azure AD) ile Salesforce korumalı alan tümleştirmek öğrenin.

Salesforce korumalı alan Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Salesforce korumalı alan erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak Salesforce korumalı alanda (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Salesforce Sandbox ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Salesforce korumalı alan çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Salesforce korumalı alan ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-salesforce-sandbox-from-the-gallery"></a>Galeriden Salesforce korumalı alan ekleme
Azure AD Salesforce korumalı alan tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Salesforce korumalı alan eklemeniz gerekir.

**Galeriden Salesforce korumalı alan eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Salesforce korumalı alan**seçin **Salesforce korumalı alan** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde Salesforce korumalı alan](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırmanız ve "Britta Simon" adlı bir test kullanıcı Salesforce korumalı alan ile Azure AD çoklu oturum açmayı test temel.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen Salesforce korumalı alanı içinde bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Salesforce korumalı alan ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Salesforce korumalı alanda değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Salesforce korumalı alan ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Salesforce korumalı alan test kullanıcısı oluşturma](#create-a-salesforce-sandbox-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Salesforce Sandbox sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Salesforce korumalı alan uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Salesforce Sandbox ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Salesforce korumalı alan** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_samlbase.png)

3. Üzerinde **Salesforce korumalı alan etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Salesforce korumalı alan etki alanı ve URL'leri tek oturum açma bilgileri](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, şu biçimi kullanarak değeri yazın: `https://<instancename>--Sandbox.<entityid>.my.salesforce.com`

    b. İçinde **tanımlayıcısı** metin kutusuna, şu biçimi kullanarak değeri yazın: `https://<instancename>--Sandbox.<entityid>.my.salesforce.com`
    
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerleri tanımlayıcısı ve gerçek oturum açma URL'si ile güncelleştirin. Kişi [Salesforce istemci destek ekibi](https://help.salesforce.com/support) bu değerleri almak için.

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/salesforce-sandbox-tutorial/tutorial_general_400.png)

6. Üzerinde **Salesforce korumalı alan yapılandırma** 'yi tıklatın **yapılandırma Salesforce korumalı alan** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_configure.png) 

7. Tarayıcınızda yeni bir sekme açın ve Salesforce korumalı alan yönetici hesabınızda oturum açın.

8. Tıklayın **Kurulum** altında **ayarlar simgesine** sayfanın sağ üst köşesinde üzerinde.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/configure1.png)

9. Ekranı aşağı kaydırarak **ayarları** Gezinti bölmesinde **kimlik** ilgili bölümü genişletin. Ardından **çoklu oturum açma ayarları**.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/sf-admin-sso.png)

10. Seçin **SAML etkin**ve ardından **kaydetmek**.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/sf-enable-saml.png)

11. SAML çoklu oturum açma ayarlarınızı yapılandırmak için tıklatın **yeni**.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/sf-admin-sso-new.png)

12. SAML çoklu oturum açma ayarları bölümüne aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/sf-saml-config.png)

    a. İçinde **adı** metin kutusuna, yapılandırma adını yazın (örneğin: *SPSSOWAAD_Test*). 

    b. İçinde **veren** alan, değerini yapıştırın **SAML varlık kimliği**, Azure portalından kopyalanan

    c. İçinde **varlık kimliği** metin kutusuna, türü `https://<instancename>--Sandbox.<entityid>.my.salesforce.com` dizininize eklediğiniz ilk Salesforce korumalı alan örnek ise. Salesforce korumalı alan örneği sonra için eklediyseniz **varlık kimliği** yazın **oturum üzerinde URL'si**, şu biçimde olmalıdır: `https://<instancename>--Sandbox.<entityid>.my.salesforce.com`

    d. Karşıya yüklemek için **kimlik sağlayıcısı sertifikası**, tıklatın **Dosya Seç** göz atın ve Azure portalından indirdiğiniz sertifika dosyasını seçin.

    e. Olarak **SAML kimlik türü**, aşağıdaki seçeneklerden birini seçin:

      * Seçin **onaylamayı kullanıcının Salesforce kullanıcı adını içeren**, kullanıcının Salesforce kullanıcıadı SAML onayı geçirilirse

      * Seçin **onaylamayı içeren kullanıcı nesnesinden Federasyon kimliği**, Federasyon kimliği kullanıcı nesnesinden SAML onayı geçirilirse

      * Seçin **onaylamayı içeren kullanıcı nesnesi kullanım Kimliğinden**, kullanıcı kimliği kullanıcı nesnesinden SAML onayı geçirilirse

    f. Olarak **SAML kimlik konumu**seçin **kimliktir konu deyimi NameIdentifier öğesinde**.

    g. Olarak **hizmet sağlayıcısı tarafından başlatılan bağlama isteği**seçin **HTTP POST**.

    h. İçinde **kimlik sağlayıcısı oturum açma URL'si** metin değerini yapıştırın **çoklu oturum açma hizmet URL'si**, Azure portalından kopyalanan.

    i. SAML oturum kapatma SFDC desteklemez.  Geçici bir çözüm olarak Yapıştır `https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0` içine **kimlik sağlayıcısı oturum kapatma URL'si** metin kutusu.

    j. **Kaydet**’e tıklayın.

### <a name="enable-your-domain"></a>Etki alanınızı etkinleştir

Bu bölümde, bir etki alanı zaten oluşturduğunuzu varsayar.  Daha fazla bilgi için bkz: [etki alanı adınız tanımlama](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US).

**Etki alanınızı etkinleştirmek için aşağıdaki adımları gerçekleştirin:**

1. Salesforce sol gezinti bölmesinde üzerinde tıklatın **şirket ayarları** ilgili bölümü genişletin ve ardından **My etki alanı**.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/sf-my-domain.png)

   >[!NOTE]
   >Lütfen etki alanınızı doğru yapılandırılmış olduğundan emin olun.

2. İçinde **kimlik doğrulama Yapılandırması** 'yi tıklatın **Düzenle**, daha sonra olarak **kimlik doğrulama hizmeti**, oturum açma SAML tek ayarı adı önceki seçin bölümünde ve son olarak tıklatın **kaydetmek**.

   ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/sf-edit-auth-config.png)

Yapılandırılmış bir etki alanınız hemen kullanıcılarınızın Salesforce korumalı alan oturum açma etki alanı URL'si kullanmanız gerekir.

URL değerini almak için önceki bölümde oluşturduğunuz SSO profili tıklatın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/salesforce-sandbox-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/salesforce-sandbox-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/salesforce-sandbox-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/salesforce-sandbox-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.

### <a name="create-a-salesforce-sandbox-test-user"></a>Salesforce korumalı alan test kullanıcısı oluşturma

Bu bölümde, Britta Simon adlı bir kullanıcı Salesforce korumalı alanda oluşturulur. Salesforce korumalı alan yeni saat sağlama, varsayılan olarak etkin olduğu destekler. Bu bölümde, eylem öğe yok. Salesforce korumalı alan erişmeyi denediğinde Salesforce korumalı alanda bir kullanıcı zaten mevcut değilse yeni bir tane oluşturulur. Salesforce korumalı alan da destekler otomatik kullanıcı hazırlama, daha fazla ayrıntı bulabilirsiniz [burada](salesforce-sandbox-provisioning-tutorial.md) otomatik kullanıcı sağlamayı yapılandırma.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Salesforce korumalı alan için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200]

**Salesforce korumalı alan Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Salesforce korumalı alan**.

    ![Uygulamalar listesinde Salesforce korumalı alan bağlantı](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Salesforce korumalı alan parçasında tıklattığınızda, otomatik olarak Salesforce korumalı alan uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)
* [Kullanıcı sağlamayı Yapılandır](salesforce-sandbox-provisioning-tutorial.md)


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
