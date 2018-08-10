---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle Qlik anlamda Kurumsal | Microsoft Docs'
description: Azure Active Directory ve Qlik algılama kuruluş arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 8c27e340-2b25-47b6-bf1f-438be4c14f93
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/06/2018
ms.author: jeedes
ms.openlocfilehash: a8816451b45171e0ba8cbd7acc937201c587c481
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39627959"
---
# <a name="tutorial-azure-active-directory-integration-with-qlik-sense-enterprise"></a>Öğretici: Azure Active Directory Qlik algılama Enterprise ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Qlik anlamda Kurumsal tümleştirme konusunda bilgi edinin.

Azure AD ile Qlik anlamda Kurumsal tümleştirme ile aşağıdaki avantajları sağlar:

- Qlik anlamda Kurumsal erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan (çoklu oturum açma) Qlik algılama kuruluş açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Qlik algılama Enterprise ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik Qlik anlamda Kurumsal çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme burada alabilirsiniz: [deneme teklifi](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Qlik anlamda Kurumsal ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-qlik-sense-enterprise-from-the-gallery"></a>Galeriden Qlik anlamda Kurumsal ekleme
Azure AD'de Qlik anlamda Kurumsal tümleştirmesini yapılandırmak için Qlik anlamda Kurumsal Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Qlik anlamda Kurumsal eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Qlik anlamda Kurumsal**seçin **Qlik anlamda Kurumsal** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Qlik anlamda Kurumsal](./media/qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Qlik algılama "Britta Simon" adlı bir test kullanıcı tabanlı Kurumsal Azure AD çoklu oturum açmayı sınayın.

Tek iş için oturum açma için Azure AD ne karşılık gelen kullanıcı Qlik anlamda kurumsal bir kullanıcının Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve Qlik anlamda Kurumsal ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Qlik algılama kuruluşta değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Qlik algılama Enterprise ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on) ** - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user) ** - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Qlik anlamda Kurumsal test kullanıcısı oluşturma](#create-a-qlik-sense-enterprise-test-user) ** - Qlik algılama kuruluşta, kullanıcının Azure AD gösterimini bağlı Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user) ** - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on) ** - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Qlik anlamda Kurumsal uygulamanızda çoklu oturum açmayı yapılandırma.

**Azure AD çoklu oturum açma Qlik algılama Enterprise ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Qlik anlamda Kurumsal** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma iletişim kutusu](./media/qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_samlbase.png)

3. Üzerinde **Qlik anlamda Kurumsal etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Qlik anlamda Kurumsal etki alanı ve URL'ler tek oturum açma bilgileri](./media/qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_url.png)

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

4. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_certificate.png)

5. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/qliksense-enterprise-tutorial/tutorial_general_400.png)

6. Böylece, Qlik Sense sunucusuna yükleyebilirsiniz, Federasyon meta veri XML dosyasını hazırlayın.

    > [!NOTE]
    > Qlik Sense sunucuya IDP meta verilerini karşıya yüklemeden önce dosyayı Azure AD arasında düzgün çalışmasını sağlamak için bilgileri kaldırmak için düzenlenmesi gereken ve Qlik Sense sunucusu.

    ![QlikSense][qs24]

    a. Bir metin düzenleyicisinde Azure portalından indirilen FederationMetaData.xml dosyasını açın.

    b. Ara değer **verilerde Securitytokenservicetype**.  Dört girdi (açılış ve kapanış etiketlerinin öğesi, iki çiftleri) vardır.

    c. Verilerde Securitytokenservicetype etiketler ve tüm bilgileri arasında dosyadan silin.

    d. Dosyayı kaydedin ve bu belgenin ilerleyen bölümlerinde kullanmak için yakın tutun.

7. Qlik algılama Qlik Yönetim Konsolu (QMC) için sanal proxy yapılandırmaları oluşturabilen bir kullanıcı olarak gidin.

8. QMC içinde tıklayarak **sanal proxy'leri** menü öğesi.

    ![QlikSense][qs6]

9. Ekranın alt kısmında tıklayın **Yeni Oluştur** düğmesi.

    ![QlikSense][qs7]

10. Sanal proxy düzenleme ekranında görüntülenir.  Ekranın sağ tarafında yapılandırma seçenekleri görünür yapmak için bir menü kalır.

    ![QlikSense][qs9]

11. İşaretli kimliği menü seçeneği ile Azure sanal proxy yapılandırması için tanımlayıcı bilgileri girin.

    ![QlikSense][qs8]  

    a. **Açıklama** sanal proxy yapılandırması için bir kolay ad alanıdır.  Bir değer için bir açıklama girin.

    b. **Önek** alan Qlik Sense ile Azure AD çoklu oturum açma bağlanmak için sanal proxy uç nokta tanımlar.  Bu sanal proxy'si için bir benzersiz ön ek adı girin.

    c. **Oturum etkin olmama zaman aşımı (dakika)** sanal bu Ara sunucu üzerinden bağlantılar için zaman aşımı.

    d. **Oturum tanımlama bilgisi üstbilgisi adı** oturum tanımlayıcıyı bir kullanıcı alır başarılı kimlik doğrulamasından sonra Qlik Sense oturumu için depolama tanımlama bilgisi adı.  Bu ad benzersiz olmalıdır.

12. Görünür yapmak için kimlik doğrulama menü seçeneğine tıklayın.  Kimlik doğrulama ekranı görünür.

    ![QlikSense][qs10]

    a. **Anonim erişim modu** açılan anonim kullanıcılar sanal proxy üzerinden Qlik Sense erişim olmadığını belirler.  Anonim kullanıcı varsayılan seçenektir.

    b. **Kimlik doğrulama yöntemi** açılan belirler sanal proxy kimlik doğrulama düzeni kullanır.  SAML, aşağı açılan listeden seçin.  Sonuç olarak daha fazla seçenek görüntülenir.

    c. İçinde **SAML konak URI alan**, ana bilgisayar adı kullanıcıları girin Qlik Sense bu SAML sanal proxy üzerinden erişmek için giriş.  URI Qlik Sense sunucusunun adıdır.

    d. İçinde **SAML varlık kimliği**, SAML konak URI alan için girdiğiniz aynı değeri girin.

    e. **IDP SAML meta veri** daha önce düzenlenebilir bir dosya **Azure AD yapılandırmasından Federasyon meta verileri Düzenle** bölümü.  **IDP meta verilerini karşıya yüklemeden önce dosyayı düzenlenmesi gereken** Azure AD arasında düzgün çalışmasını sağlamak için bilgileri kaldırmak için ve Qlik Sense sunucusu.  **Düzenlenecek dosyayı henüz varsa lütfen yönergeleri inceleyin.**  Dosyayı düzenlerseniz sanal proxy yapılandırması için yüklenecek düzenlenen meta veri dosyasını seçin ve Gözat düğmesine tıklayın.

    f. SAML gösteren özniteliği için öznitelik adı veya şema başvurusu girin **UserID** Azure AD Qlik Sense sunucusuna gönderir.  Azure uygulama ekranları post yapılandırmada şema başvuru bilgileri kullanılabilir.  Ad özniteliği kullanmak için girin `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.

    g. Si **kullanıcı dizini** , eklenen kullanıcılara Qlik Sense sunucusuna Azure AD üzerinden kimlik doğrulaması sırasında.  Sabit kodlanmış değerler arasına, tarafından **köşeli ayraçlar []**.  Azure AD SAML onaylaması içinde gönderilen bir özniteliği kullanacak şekilde öznitelik adı bu metin kutusuna girin **olmadan** köşeli ayraç.

    h. **SAML imzalama algoritması** sanal proxy yapılandırması için imzalama hizmet sağlayıcısında (büyük/küçük harf bu Qlik Sense sunucusu) sertifika ayarlar.  SAML imzalama algoritması Qlik Sense sunucusu Microsoft Gelişmiş RSA ve AES şifreleme sağlayıcısı kullanılarak oluşturulan güvenilen bir sertifika kullanıyorsa, Değiştir **SHA-256'yı**.

    i. Güvenlik kuralları'nda kullanılmak için Qlik Sense gönderilecek grupları gibi ek öznitelikler için SAML öznitelik eşlemesi bölümü sağlar.

13. Tıklayarak **yük DENGELEME** görünür yapmak için menü seçeneği.  Yük Dengeleme ekranı görüntülenir.

    ![QlikSense][qs11]

14. Tıklayarak **Ekle yeni sunucu düğümü** düğme, select altyapısı düğümü veya düğümleri Qlik Sense gönderecek Yükü Dengeleme amaçlar için ve oturumları **Ekle** düğmesi.

    ![QlikSense][qs12]

15. Görünür yapmak için Gelişmiş menü seçeneğine tıklayın. Gelişmiş ekran görüntülenir.

    ![QlikSense][qs13]

    Konak beyaz liste Qlik Sense sunucuya bağlanırken kabul ana bilgisayar adlarını tanımlar.  **Kullanıcıların Qlik Sense sunucuya bağlanırken belirteceği ana bilgisayar adı girin.** SAML konak Uri'si ' https://'olmadan aynı değere adıdır.

16. Tıklayın **Uygula** düğmesi.

    ![QlikSense][qs14]

17. Sanal Ara sunucuya bağlı proxy'leri yeniden başlatılacak belirten uyarı iletisini kabul etmek için Tamam'a tıklayın.

    ![QlikSense][qs15]

18. Ekranın sağ tarafında, ilişkili öğeleri menü görünür.  Tıklayarak **proxy'leri** menü seçeneği.

    ![QlikSense][qs16]

19. Proxy ekranı görüntülenir.  Tıklayın **bağlantı** sanal proxy sunucusuna bir ara sunucu bağlantısını için alt kısımdaki düğmesi.

    ![QlikSense][qs17]

20. Bu sanal Ara sunucu bağlantısı desteği ve proxy düğümü seçin **bağlantı** düğmesi.  Bağladıktan sonra proxy ilişkili proxy'leri altında listelenir.

    ![QlikSense][qs18]
  
    ![QlikSense][qs19]

21. Yaklaşık beş ila on saniye sonra Yenile QMC iletisi görüntülenir.  Tıklayın **Yenile QMC** düğmesi.

    ![QlikSense][qs20]

22. QMC yenilendiğinde tıklayarak **sanal proxy'leri** menü öğesi. Yeni SAML sanal proxy giriş ekranında tabloda listelenir.  Tek sanal proxy girişe tıklayın.

    ![QlikSense][qs51]

23. Ekranın alt kısmında, SP indirme meta veri düğmesi etkinleşir.  Tıklayın **SP indirme meta veri** düğmesini meta verileri bir dosyaya kaydedin.

    ![QlikSense][qs52]

24. Sp meta veri dosyası açın.  Gözlemleyin **Entityıd** girişi ve **AssertionConsumerService** girişi.  Bu değerleri eşdeğer **tanımlayıcı**, **oturum açma URL'si** ve **yanıt URL'si** Azure AD uygulama yapılandırması. Bu değerleri yapıştırın **Qlik anlamda Kurumsal etki alanı ve URL'ler** Azure AD uygulaması Yapılandırma Sihirbazı'nda değiştirmelisiniz sonra bunlar, eşleşen değil, Azure AD Uygulama Yapılandırması bölümünde.

    ![QlikSense][qs53]

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

   ![Azure Active Directory düğmesi](./media/qliksense-enterprise-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

   !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/qliksense-enterprise-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

   ![Ekle düğmesi](./media/qliksense-enterprise-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

   ![Kullanıcı iletişim kutusu](./media/qliksense-enterprise-tutorial/create_aaduser_04.png)

   a. İçinde **adı** kutusuna **BrittaSimon**.

   b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

   c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

   d. **Oluştur**’a tıklayın.

### <a name="create-a-qlik-sense-enterprise-test-user"></a>Qlik anlamda Kurumsal test kullanıcısı oluşturma

Bu bölümde, Qlik anlamda Kurumsal Britta Simon adlı bir kullanıcı oluşturun. Çalışmak [Qlik anlamda Kurumsal İstemci Destek ekibine](https://www.qlik.com/us/services/support) Qlik anlamda Kurumsal platform kullanıcıları eklemek için. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Qlik algılama kuruluş erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200]

**Britta Simon Qlik algılama kuruluş atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201]

2. Uygulamalar listesinde **Qlik anlamda Kurumsal**.

    ![Uygulamalar listesinde Qlik anlamda Kurumsal bağlantı](./media/qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_app.png)  

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Qlik anlamda Kurumsal kutucuğa tıkladığınızda, otomatik olarak Qlik anlamda Kurumsal uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/qliksense-enterprise-tutorial/tutorial_general_01.png
[2]: ./media/qliksense-enterprise-tutorial/tutorial_general_02.png
[3]: ./media/qliksense-enterprise-tutorial/tutorial_general_03.png
[4]: ./media/qliksense-enterprise-tutorial/tutorial_general_04.png

[100]: ./media/qliksense-enterprise-tutorial/tutorial_general_100.png

[200]: ./media/qliksense-enterprise-tutorial/tutorial_general_200.png
[201]: ./media/qliksense-enterprise-tutorial/tutorial_general_201.png
[202]: ./media/qliksense-enterprise-tutorial/tutorial_general_202.png
[203]: ./media/qliksense-enterprise-tutorial/tutorial_general_203.png

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