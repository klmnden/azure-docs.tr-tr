---
title: 'Öğretici: Qlik algılama Enterprise ile Azure Active Directory Tümleştirmesi | Microsoft Docs'
description: Azure Active Directory ve Qlik algılama kuruluş arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 8c27e340-2b25-47b6-bf1f-438be4c14f93
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/17/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 31df4cb9163e598bfde0c491d8088398c3204119
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65407987"
---
# <a name="tutorial-azure-active-directory-integration-with-qlik-sense-enterprise"></a>Öğretici: Qlik algılama Enterprise ile Azure Active Directory Tümleştirmesi

Bu öğreticide, Azure Active Directory (Azure AD) ile Qlik anlamda Kurumsal tümleştirme konusunda bilgi edinin.
Azure AD ile Qlik anlamda Kurumsal tümleştirme ile aşağıdaki avantajları sağlar:

* Qlik anlamda Kurumsal erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak (çoklu oturum açma) Qlik algılama kuruluş oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Qlik algılama Enterprise ile yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Abonelik Qlik anlamda Kurumsal çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Qlik anlamda Kurumsal destekler **SP** tarafından başlatılan

## <a name="adding-qlik-sense-enterprise-from-the-gallery"></a>Galeriden Qlik anlamda Kurumsal ekleme

Azure AD'de Qlik anlamda Kurumsal tümleştirmesini yapılandırmak için Qlik anlamda Kurumsal Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Qlik anlamda Kurumsal eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Qlik anlamda Kurumsal**seçin **Qlik anlamda Kurumsal** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde Qlik anlamda Kurumsal](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Qlik algılama adlı bir test kullanıcı tabanlı Kurumsal test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ve Qlik anlamda Kurumsal ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Qlik algılama Enterprise ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Qlik anlamda Kurumsal çoklu oturum açmayı yapılandırma](#configure-qlik-sense-enterprise-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Qlik anlamda Kurumsal test kullanıcısı oluşturma](#create-qlik-sense-enterprise-test-user)**  - Qlik algılama kuruluşta, kullanıcının Azure AD gösterimini bağlı Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma Qlik algılama Enterprise ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Qlik anlamda Kurumsal** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Qlik anlamda Kurumsal etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier-reply.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<Qlik Sense Fully Qualifed Hostname>:4443/azure/hub`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak:

    | |
    |--|
    | `https://<Qlik Sense Fully Qualifed Hostname>.qlikpoc.com`|
    | `https://<Qlik Sense Fully Qualifed Hostname>.qliksense.com`|
    | |

    c. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak:

    `https://<Qlik Sense Fully Qualifed Hostname>:4443/samlauthn/`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si, tanımlayıcıya ve yanıt URL'si, Bu öğretici veya kişi daha sonra açıklanacak olan güncelleştirme [Qlik anlamda Kurumsal İstemci Destek ekibine](https://www.qlik.com/us/services/support) bu değerleri almak için.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

### <a name="configure-qlik-sense-enterprise-single-sign-on"></a>Qlik anlamda Kurumsal çoklu oturum açmayı yapılandırın

1. Böylece, Qlik Sense sunucusuna yükleyebilirsiniz, Federasyon meta veri XML dosyasını hazırlayın.

    > [!NOTE]
    > Qlik Sense sunucuya IDP meta verilerini karşıya yüklemeden önce dosyayı Azure AD arasında düzgün çalışmasını sağlamak için bilgileri kaldırmak için düzenlenmesi gereken ve Qlik Sense sunucusu.

    ![QlikSense][qs24]

    a. Bir metin düzenleyicisinde Azure portalından indirilen FederationMetaData.xml dosyasını açın.

    b. Ara değer **verilerde Securitytokenservicetype**.  Dört girdi (açılış ve kapanış etiketlerinin öğesi, iki çiftleri) vardır.

    c. Verilerde Securitytokenservicetype etiketler ve tüm bilgileri arasında dosyadan silin.

    d. Dosyayı kaydedin ve bu belgenin ilerleyen bölümlerinde kullanmak için yakın tutun.

2. Qlik algılama Qlik Yönetim Konsolu (QMC) için sanal proxy yapılandırmaları oluşturabilen bir kullanıcı olarak gidin.

3. QMC içinde tıklayarak **sanal proxy'leri** menü öğesi.

    ![QlikSense][qs6]

4. Ekranın alt kısmında tıklayın **Yeni Oluştur** düğmesi.

    ![QlikSense][qs7]

5. Sanal proxy düzenleme ekranında görüntülenir.  Ekranın sağ tarafında yapılandırma seçenekleri görünür yapmak için bir menü kalır.

    ![QlikSense][qs9]

6. İşaretli kimliği menü seçeneği ile Azure sanal proxy yapılandırması için tanımlayıcı bilgileri girin.

    ![QlikSense][qs8]  

    a. **Açıklama** sanal proxy yapılandırması için bir kolay ad alanıdır.  Bir değer için bir açıklama girin.

    b. **Önek** alan Qlik Sense ile Azure AD çoklu oturum açma bağlanmak için sanal proxy uç nokta tanımlar.  Bu sanal proxy'si için bir benzersiz ön ek adı girin.

    c. **Oturum etkin olmama zaman aşımı (dakika)** sanal bu Ara sunucu üzerinden bağlantılar için zaman aşımı.

    d. **Oturum tanımlama bilgisi üstbilgisi adı** oturum tanımlayıcıyı bir kullanıcı alır başarılı kimlik doğrulamasından sonra Qlik Sense oturumu için depolama tanımlama bilgisi adı.  Bu ad benzersiz olmalıdır.

7. Görünür yapmak için kimlik doğrulama menü seçeneğine tıklayın.  Kimlik doğrulama ekranı görünür.

    ![QlikSense][qs10]

    a. **Anonim erişim modu** açılan anonim kullanıcılar sanal proxy üzerinden Qlik Sense erişim olmadığını belirler.  Anonim kullanıcı varsayılan seçenektir.

    b. **Kimlik doğrulama yöntemi** açılan belirler sanal proxy kimlik doğrulama düzeni kullanır.  SAML, aşağı açılan listeden seçin.  Sonuç olarak daha fazla seçenek görüntülenir.

    c. İçinde **SAML konak URI alan**, ana bilgisayar adı kullanıcıları girin Qlik Sense bu SAML sanal proxy üzerinden erişmek için giriş.  URI Qlik Sense sunucusunun adıdır.

    d. İçinde **SAML varlık kimliği**, SAML konak URI alan için girdiğiniz aynı değeri girin.

    e. **IDP SAML meta veri** daha önce düzenlenebilir bir dosya **Azure AD yapılandırmasından Federasyon meta verileri Düzenle** bölümü.  **IDP meta verilerini karşıya yüklemeden önce dosyayı düzenlenmesi gereken** Azure AD arasında düzgün çalışmasını sağlamak için bilgileri kaldırmak için ve Qlik Sense sunucusu.  **Düzenlenecek dosyayı henüz varsa lütfen yönergeleri inceleyin.**  Dosyayı düzenlerseniz sanal proxy yapılandırması için yüklenecek düzenlenen meta veri dosyasını seçin ve Gözat düğmesine tıklayın.

    f. SAML gösteren özniteliği için öznitelik adı veya şema başvurusu girin **UserID** Azure AD Qlik Sense sunucusuna gönderir.  Azure uygulama ekranları post yapılandırmada şema başvuru bilgileri kullanılabilir.  Ad özniteliği kullanmak için girin `https://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.

    g. Si **kullanıcı dizini** , eklenen kullanıcılara Qlik Sense sunucusuna Azure AD üzerinden kimlik doğrulaması sırasında.  Sabit kodlanmış değerler arasına, tarafından **köşeli ayraçlar []**.  Azure AD SAML onaylaması içinde gönderilen bir özniteliği kullanacak şekilde öznitelik adı bu metin kutusuna girin **olmadan** köşeli ayraç.

    h. **SAML imzalama algoritması** sanal proxy yapılandırması için imzalama hizmet sağlayıcısında (büyük/küçük harf bu Qlik Sense sunucusu) sertifika ayarlar.  SAML imzalama algoritması Qlik Sense sunucusu Microsoft Gelişmiş RSA ve AES şifreleme sağlayıcısı kullanılarak oluşturulan güvenilen bir sertifika kullanıyorsa, Değiştir **SHA-256'yı**.

    i. Güvenlik kuralları'nda kullanılmak için Qlik Sense gönderilecek grupları gibi ek öznitelikler için SAML öznitelik eşlemesi bölümü sağlar.

8. Tıklayarak **yük DENGELEME** görünür yapmak için menü seçeneği.  Yük Dengeleme ekranı görüntülenir.

    ![QlikSense][qs11]

9. Tıklayarak **Ekle yeni sunucu düğümü** düğme, select altyapısı düğümü veya düğümleri Qlik Sense gönderecek Yükü Dengeleme amaçlar için ve oturumları **Ekle** düğmesi.

    ![QlikSense][qs12]

10. Görünür yapmak için Gelişmiş menü seçeneğine tıklayın. Gelişmiş ekran görüntülenir.

    ![QlikSense][qs13]

    Konak beyaz liste Qlik Sense sunucuya bağlanırken kabul ana bilgisayar adlarını tanımlar.  **Kullanıcıların Qlik Sense sunucuya bağlanırken belirteceği ana bilgisayar adı girin.** SAML konak Uri'si ' https://'olmadan aynı değere adıdır.

11. Tıklayın **Uygula** düğmesi.

    ![QlikSense][qs14]

12. Sanal Ara sunucuya bağlı proxy'leri yeniden başlatılacak belirten uyarı iletisini kabul etmek için Tamam'a tıklayın.

    ![QlikSense][qs15]

13. Ekranın sağ tarafında, ilişkili öğeleri menü görünür.  Tıklayarak **proxy'leri** menü seçeneği.

    ![QlikSense][qs16]

14. Proxy ekranı görüntülenir.  Tıklayın **bağlantı** sanal proxy sunucusuna bir ara sunucu bağlantısını için alt kısımdaki düğmesi.

    ![QlikSense][qs17]

15. Bu sanal Ara sunucu bağlantısı desteği ve proxy düğümü seçin **bağlantı** düğmesi.  Bağladıktan sonra proxy ilişkili proxy'leri altında listelenir.

    ![QlikSense][qs18]
  
    ![QlikSense][qs19]

16. Yaklaşık beş ila on saniye sonra Yenile QMC iletisi görüntülenir.  Tıklayın **Yenile QMC** düğmesi.

    ![QlikSense][qs20]

17. QMC yenilendiğinde tıklayarak **sanal proxy'leri** menü öğesi. Yeni SAML sanal proxy giriş ekranında tabloda listelenir.  Tek sanal proxy girişe tıklayın.

    ![QlikSense][qs51]

18. Ekranın alt kısmında, SP indirme meta veri düğmesi etkinleşir.  Tıklayın **SP indirme meta veri** düğmesini meta verileri bir dosyaya kaydedin.

    ![QlikSense][qs52]

19. Sp meta veri dosyası açın.  Gözlemleyin **Entityıd** girişi ve **AssertionConsumerService** girişi.  Bu değerleri eşdeğer **tanımlayıcı**, **oturum açma URL'si** ve **yanıt URL'si** Azure AD uygulama yapılandırması. Bu değerleri yapıştırın **Qlik anlamda Kurumsal etki alanı ve URL'ler** Azure AD uygulaması Yapılandırma Sihirbazı'nda değiştirmelisiniz sonra bunlar, eşleşen değil, Azure AD Uygulama Yapılandırması bölümünde.

    ![QlikSense][qs53]

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü **brittasimon\@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Qlik algılama kuruluş erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Qlik anlamda Kurumsal**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde yazın ve **Qlik anlamda Kurumsal**.

    ![Uygulamalar listesinde Qlik anlamda Kurumsal bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-qlik-sense-enterprise-test-user"></a>Qlik anlamda Kurumsal test kullanıcısı oluşturma

Bu bölümde, Qlik anlamda Kurumsal Britta Simon adlı bir kullanıcı oluşturun. Çalışmak [Qlik anlamda Kurumsal Destek ekibine](https://www.qlik.com/us/services/support) Qlik anlamda Kurumsal platform kullanıcıları eklemek için. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Qlik anlamda Kurumsal kutucuğa tıkladığınızda, size otomatik olarak Qlik algılama SSO'yu ayarlayın kuruluş oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

<!--Image references-->

[qs6]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_06.png
[qs7]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_07.png
[qs8]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_08.png
[qs9]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_09.png
[qs10]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_10.png
[qs11]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_11.png
[qs12]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_12.png
[qs13]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_13.png
[qs14]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_14.png
[qs15]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_15.png
[qs16]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_16.png
[qs17]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_17.png
[qs18]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_18.png
[qs19]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_19.png
[qs20]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_20.png
[qs24]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_24.png
[qs51]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_51.png
[qs52]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_52.png
[qs53]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_53.png

