---
title: 'Öğretici: Salesforce korumalı alanı ile Azure Active Directory Tümleştirmesi | Microsoft Docs'
description: Azure Active Directory ve Salesforce korumalı alan arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: ee54c39e-ce20-42a4-8531-da7b5f40f57c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/10/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6794d7eaccb488bb345227161f0bca02f14bc518
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62104977"
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce-sandbox"></a>Öğretici: Salesforce korumalı alanı ile Azure Active Directory Tümleştirmesi

Bu öğreticide, Azure Active Directory (Azure AD) ile Salesforce korumalı alan tümleştirme konusunda bilgi edinin.

Sanal amaçları, geliştirme, test ve eğitim, verilere ve uygulamalara, Salesforce üretimde ödün vermeden gibi çeşitli ayrı ortamlarda kuruluşunuz birden çok kopyasını oluşturma olanağı sunar Kuruluş.
Daha fazla ayrıntı için [korumalı alan genel bakış](https://help.salesforce.com/articleView?id=create_test_instance.htm&language=en_us&type=5).

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

2. Tıklayın **değişiklik çoklu oturum açma modunu** seçmek için ekranın en üstünde **SAML** modu.

      ![Çoklu oturum açma bağlantısı yapılandırma](./media/salesforce-sandbox-tutorial/tutorial_general_300.png)

3. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunu tıklatın **seçin** için **SAML** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma bağlantısı yapılandırma](./media/salesforce-sandbox-tutorial/tutorial_general_301.png)

4. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için düğmeyi **temel SAML yapılandırma** iletişim.
   
    ![Çoklu oturum açma bağlantısı yapılandırma](./media/salesforce-sandbox-tutorial/tutorial_general_302.png)

5. Üzerinde **temel SAML yapılandırma** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** başlatılan modu:

    a. Tıklayın **meta veri dosyasını karşıya yükleme**.

    ![Meta veri dosyasını yükleyin](./media/salesforce-sandbox-tutorial/upload_metadata.png)

    b. Tıklayarak **klasör logosu** meta veri dosyası seçin ve **karşıya**.

    ![meta veri dosyası seçin](./media/salesforce-sandbox-tutorial/browse_upload_metadata.png)

    > [!NOTE]
    > Hizmet sağlayıcısı, öğreticinin ilerleyen bölümlerinde açıklanan Salesforce korumalı alan Yönetim Portalı'ndan meta veri dosyası alırsınız.

    c. Meta veri dosyası başarıyla karşıya yüklendikten sonra **yanıt URL'si** değer otomatik olarak doldurulmuş elde **yanıt URL'si** metin.

    ![Salesforce korumalı alan etki alanı ve URL'ler tek oturum açma bilgileri](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_url1.png)

6. Üzerinde **SAML imzalama sertifikası** bölümü tıklatın üzerinde **indirme** indirmek için **Federasyon meta verileri XML** ve bilgisayarınızda xml dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_certificate.png)

7. Tarayıcınızda yeni bir sekme açın ve Salesforce korumalı alan yönetici hesabınızda oturum açın.

8. Tıklayarak **Kurulum** altında **ayarlar simgesine** sayfanın sağ üst köşesinde.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/configure1.png)

9. Ekranı aşağı kaydırarak **ayarları** sol gezinti bölmesinden **kimlik** ilgili bölümü genişletin. Ardından **çoklu oturum açma ayarları**.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/sf-admin-sso.png)

10. Üzerinde **çoklu oturum açma ayarları** sayfasında **Düzenle** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/configure3.png)

11. Seçin **SAML etkin**ve ardından **Kaydet**.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/sf-enable-saml.png)

12. SAML çoklu oturum açma ayarlarınızı yapılandırmak için tıklayın **yeni meta veri dosyasından**.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/sf-admin-sso-new.png)

13. Tıklayın **Dosya Seç** Azure portal'ı seçin ve indirilen meta veri XML dosyasını karşıya yüklemek için **Oluştur**.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/xmlchoose.png)

14. Üzerinde **SAML çoklu oturum açma ayarları** sayfasında alanları otomatik olarak doldurur ve Kaydet'e tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/salesforcexml.png)

15. Üzerinde **çoklu oturum açma ayarları** sayfasında, **meta verileri indirme** düğmesi, hizmet sağlayıcısı meta verileri dosyası indirilemedi. Bu dosyada kullanmak **temel SAML yapılandırma** yukarıda açıklandığı gibi gerekli URL'leri yapılandırmak için Azure portalında bölümü.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/configure4.png)

16. Uygulamada yapılandırmak istiyorsanız **SP** başlatılan modu, aşağıdaki ilgili Önkoşullar şunlardır:

    a. Doğrulanmış bir etki alanı olmalıdır.

    b. Salesforce korumalı alan etki alanınızda etkinleştirmeniz ve yapılandırmanız gerekir, bu adımlar, bu öğreticinin ilerleyen bölümlerinde açıklanmıştır.

    c. Azure portalında, üzerinde **temel SAML yapılandırma** bölümünde **ek URL'lerini ayarlayın** ve aşağıdaki adımı uygulayın:
  
    ![Salesforce korumalı alan etki alanı ve URL'ler tek oturum açma bilgileri](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_url.png)

    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak değeri yazın: `https://<instancename>--Sandbox.<entityid>.my.salesforce.com`

    > [!NOTE]
    > Etki alanı etkinleştirdikten sonra bu değer Salesforce korumalı alan portaldan kopyalanmalıdır.

17. Üzerinde **SAML imzalama sertifikası** bölümünde **Federasyon meta verileri XML** ve bilgisayarınızda xml dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_certificate.png)

18. Tarayıcınızda yeni bir sekme açın ve Salesforce korumalı alan yönetici hesabınızda oturum açın.

19. Tıklayarak **Kurulum** altında **ayarlar simgesine** sayfanın sağ üst köşesinde.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/configure1.png)

20. Ekranı aşağı kaydırarak **ayarları** sol gezinti bölmesinden **kimlik** ilgili bölümü genişletin. Ardından **çoklu oturum açma ayarları**.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/sf-admin-sso.png)

21. Üzerinde **çoklu oturum açma ayarları** sayfasında **Düzenle** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/configure3.png)

22. Seçin **SAML etkin**ve ardından **Kaydet**.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/sf-enable-saml.png)

23. SAML çoklu oturum açma ayarlarınızı yapılandırmak için tıklayın **yeni meta veri dosyasından**.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/sf-admin-sso-new.png)

24. Tıklayın **Dosya Seç** meta veri XML dosyasını karşıya yükleyin ve tıklayın **Oluştur**.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/xmlchoose.png)

25. Üzerinde **SAML çoklu oturum açma ayarları** sayfasında alanları otomatik olarak doldurmak, yapılandırma adını yazın (örneğin: *SPSSOWAAD_Test*) içinde **adı** metin kutusu ve Kaydet'i tıklatın.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/sf-saml-config.png)

26. Salesforce korumalı alan etki alanınızda etkinleştirmek için aşağıdaki adımları gerçekleştirin:

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

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    ![Azure AD kullanıcısı oluşturun][100]

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/salesforce-sandbox-tutorial/create_aaduser_01.png) 

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/salesforce-sandbox-tutorial/create_aaduser_02.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü **brittasimon\@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **özellikleri**seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’u seçin.

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

4. Tıklayın **Kullanıcı Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

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
