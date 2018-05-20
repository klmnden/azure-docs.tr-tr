---
title: 'Öğretici: Azure Active Directory Tümleştirme Qlik algılama kuruluş ile | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile Qlik algılama kuruluş arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 8c27e340-2b25-47b6-bf1f-438be4c14f93
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: jeedes
ms.openlocfilehash: 8cca4fb3ee10d56481cefccb7ea869b6bf13109c
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-azure-active-directory-integration-with-qlik-sense-enterprise"></a>Öğretici: Azure Active Directory Tümleştirme Qlik algılama kuruluş ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Qlik algılama Kurumsal tümleştirme öğrenin.

Qlik algılama kuruluş Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Qlik algılama Kurumsal erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak (çoklu oturum açma) Qlik algılama kuruluş açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Qlik algılama Enterprise ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Qlik algılama Kurumsal çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, burada bir aylık deneme elde edebilirsiniz: [deneme teklifi](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Qlik algılama Kurumsal ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-qlik-sense-enterprise-from-the-gallery"></a>Galeriden Qlik algılama Kurumsal ekleme
Azure AD Qlik algılama Kurumsal tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Qlik algılama Kurumsal eklemeniz gerekir.

**Galeriden Qlik algılama Kurumsal eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna yazın **Qlik algılama Kurumsal**seçin **Qlik algılama Kurumsal** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde Qlik algılama Enterprise](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Qlik algılama "Britta Simon" adlı bir test kullanıcı tabanlı kuruluş ile test etme.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen Qlik algılama kuruluştaki bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve ilgili kullanıcı Qlik algılama kuruluştaki arasında bir bağlantı ilişkisi kurulması gerekir.

Değeri Qlik algılama kuruluşta atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Qlik algılama kuruluş ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Qlik algılama Kurumsal test kullanıcısı oluşturma](#create-a-qlik-sense-enterprise-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Qlik algılama Kurumsal sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Qlik algılama Kurumsal uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Qlik algılama Enterprise ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Qlik algılama Kurumsal** uygulama tümleştirmesi sayfasında, tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_samlbase.png)

3. Üzerinde **Qlik algılama kuruluş etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Qlik algılama kuruluş etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<Qlik Sense Fully Qualifed Hostname>:443//samlauthn/`
    
    > [!NOTE]
    > Bu URI, sonunda eğik unutmayın. Gerekli değildir.
    
    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın:
    | |
    |--|
    | `https://<Qlik Sense Fully Qualifed Hostname>.qlikpoc.com`|
    | `https://<Qlik Sense Fully Qualifed Hostname>.qliksense.com`|

    > [!NOTE] 
    > Bu değerler gerçek değildir. Daha sonra Bu öğreticide veya kişi içinde açıklanan gerçek oturum açma URL'si ve tanımlayıcı, bu değerleri güncelleştirmek [Qlik algılama Kurumsal İstemci destek ekibi](https://www.qlik.com/us/services/support) bu değerleri almak için. 

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_400.png)

6. Böylece, Qlik algılama sunucuya yükleyebilirsiniz Federasyon meta veri XML dosyasını hazırlayın.
   
    > [!NOTE]
    > IDP meta veri Qlik algılama sunucuya karşıya yüklemeden önce dosyayı Azure AD arasında düzgün çalışmasını sağlamak için bilgileri kaldırmak için düzenlenmesi gerekir ve Qlik algılama sunucu.
    
    ![QlikSense][qs24]
   
    a. Bir metin düzenleyicisinde Azure portalından indirmiş FederationMetaData.xml dosyasını açın.
   
    b. Arama için değer **RoleDescriptor**.  Dört girdi (açma ve kapatma öğesi etiketleri iki çiftleri) vardır.
   
    c. RoleDescriptor etiketleri ve tüm bilgileri arasında dosyadan silin.
   
    d. Dosyayı kaydedin ve yakındaki bu belgenin sonraki bölümlerinde kullanmak için saklayın.

7. Qlik algılama Qlik Yönetim Konsolu (QMC) için sanal proxy yapılandırmaları oluşturabilirsiniz bir kullanıcı olarak gidin.

8. QMC içinde tıklayın **sanal proxy'leri** menü öğesi.
   
    ![QlikSense][qs6] 

9. Ekranın alt kısmındaki tıklatın **Yeni Oluştur** düğmesi.
   
    ![QlikSense][qs7]

10. Sanal proxy düzenleme ekranı görüntülenir.  Ekranın sağ tarafında yapılandırma seçenekleri görünür yapmak için bir menü ' dir.
   
    ![QlikSense][qs9]

11. Kimlik menü seçeneğinin işaretli sonra Azure sanal proxy yapılandırması için tanımlayıcı bilgileri girin.
    
    ![QlikSense][qs8]  
    
    a. **Açıklama** sanal proxy yapılandırması için bir kolay ad alanıdır.  Bir değer için bir açıklama girin.
    
    b. **Önek** alan Qlik algılama ile Azure AD çoklu oturum açma bağlanmak için sanal proxy endpoint tanımlar.  Bu sanal proxy için bir benzersiz önek adı girin.
    
    c. **Oturum etkin olmama zaman aşımı (dakika)** bu sanal proxy üzerinden bağlantıları için zaman aşımı değeri.
    
    d. **Oturum tanımlama bilgisi üstbilgisi adı** oturum tanımlayıcıyı bir kullanıcı alır başarılı kimlik doğrulamasından sonra Qlik algılama oturumu için depolama tanımlama bilgisi adıdır.  Bu ad benzersiz olmalıdır.

12. Görünür hale getirmek için kimlik doğrulama menü seçeneğine tıklayın.  Kimlik doğrulama ekranı görüntülenir.
    
    ![QlikSense][qs10]
    
    a. **Anonim erişim modu** açılan belirler anonim kullanıcılar, sanal proxy üzerinden Qlik algılama erişim durumunda.  Anonim kullanıcı varsayılan seçenektir.
    
    b. **Kimlik doğrulama yöntemini** açılan belirler sanal proxy kimlik doğrulama düzenini kullanır.  SAML aşağı açılan listeden seçin.  Daha fazla seçenek sonuç olarak görünür.
    
    c. İçinde **SAML konak URI alan**, ana bilgisayar adı kullanıcıları girin Qlik algılama bu SAML sanal proxy üzerinden erişmek için giriş.  Ana Qlik algılama server URI'sini adıdır.
    
    d. İçinde **SAML varlık kimliği**, SAML konak URI alan için girilen aynı değeri girin.
    
    e. **SAML IDP meta veri** daha önce düzenlenebilir dosya **Düzenle Federasyon meta verilerini Azure AD yapılandırma** bölümü.  **IDP meta veriler karşıya yüklemeden önce dosyasının düzenlenmesi gerekir** Azure AD arasında düzgün çalışmasını sağlamak için bilgileri kaldırmak için ve Qlik algılama sunucu.  **Düzenlenecek dosyayı henüz varsa lütfen yukarıda verilen yönergeleri bakın.**  Dosyayı düzenlerseniz sanal proxy yapılandırmasını karşıya yüklemek için düzenlenen meta veri dosyası seçin ve Gözat düğmesine tıklayın.
    
    f. SAML gösteren özniteliği için öznitelik adı veya şema başvurusu girin **UserID** Azure AD Qlik algılama sunucusuna gönderir.  Şema başvurusu bilgileri Azure uygulaması ekranlar post yapılandırmada kullanılabilir.  Name özniteliğini kullanmak için girin `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.
    
    g. Değerini girmeniz **kullanıcı dizini** , bağlı kullanıcılara Azure AD üzerinden Qlik algılama sunucusuna kimlik doğrulaması sırasında.  Sabit kodlanmış değerler arasına, tarafından **köşeli ayraçlar []**.  Azure AD SAML onayı gönderilen bir öznitelik kullanmak için bu metin kutusunda öznitelik adını girin **olmadan** köşeli ayraç.
    
    h. **SAML imzalama algoritmasını** sanal proxy yapılandırması için imzalama hizmet sağlayıcısı (Server'daki bu servis talebi Qlik algılama) sertifika ayarlar.  Qlik algılama sunucusu Microsoft Gelişmiş RSA ve AES şifreleme sağlayıcısı kullanılarak oluşturulan güvenilen bir sertifika kullanıyorsa, SAML imzalama algoritmasını değiştirmek **SHA-256**.
    
    i. Güvenlik kuralları kullanmak için Qlik algılama gönderilecek grupları gibi ek öznitelikler için SAML özniteliği eşleme bölümü sağlar.

13. Tıklayın **yük DENGELEME** görünür hale getirmek için menü seçeneği.  Yük Dengeleme ekranı görüntülenir.
    
    ![QlikSense][qs11]

14. Tıklayın **Ekle yeni sunucu düğümü** düğme, select altyapısı düğüm veya düğümler Qlik algılama gönderecek Yük Dengeleme amaçları için öğesini tıklatıp oturumları **Ekle** düğmesi.
    
    ![QlikSense][qs12]

15. Görünür hale getirmek için Gelişmiş menü seçeneğine tıklayın. Gelişmiş ekranı görüntülenir.
    
    ![QlikSense][qs13]
    
    Ana bilgisayar beyaz liste Qlik algılama sunucusuna bağlanırken kabul ana bilgisayar adlarını tanımlar.  **Kullanıcıların Qlik algılama sunucusuna bağlanırken belirteceği ana bilgisayar adı girin.** Ana bilgisayar adı https:// olmadan SAML konak URI değeri ile aynıdır.

16. Tıklatın **Uygula** düğmesi.
    
    ![QlikSense][qs14]

17. Sanal proxy sunucusuna bağlı proxy'leri başlatılacak belirten uyarı iletisini kabul etmek için Tamam'ı tıklatın.
    
    ![QlikSense][qs15]

18. Ekranın sağ tarafta ilişkili öğeler menüsü görüntülenir.  Tıklayın **proxy'leri** menü seçeneği.
    
    ![QlikSense][qs16]

19. Proxy ekranı görüntülenir.  Tıklatın **bağlantı** proxy sanal proxy sunucuya bağlanmak için altındaki düğmesini.
    
    ![QlikSense][qs17]

20. Bu sanal proxy bağlantı destekleyen ve'ı tıklatın proxy düğümünü seçin **bağlantı** düğmesi.  Bağladıktan sonra proxy ilişkili proxy'leri altında listelenir.
    
    ![QlikSense][qs18]
  
    ![QlikSense][qs19]

21. Yaklaşık beş ila on saniye sonra Yenile QMC iletisi görüntülenir.  Tıklatın **yenileme QMC** düğmesi.
    
    ![QlikSense][qs20]

22. QMC yenilendiğinde tıklayın **sanal proxy'leri** menü öğesi. Yeni SAML sanal proxy giriş ekranında tablosundaki listelenir.  Sanal proxy girişini tek tıklatın.
    
    ![QlikSense][qs51]

23. Ekranın alt kısmındaki karşıdan SP meta verileri düğmesi etkinleşir.  ' I tıklatın **karşıdan SP meta veri** düğmesi meta verileri bir dosyaya kaydedin.
    
    ![QlikSense][qs52]

24. Sp meta veri dosyasını açın.  Gözlemlemek **Entityıd** girişi ve **AssertionConsumerService** girişi.  Bu değerleri eşit olan **tanımlayıcısı** ve **URL üzerinde oturum** Azure AD uygulama yapılandırması. Bu değerleri Yapıştır **Qlik algılama kuruluş etki alanı ve URL'leri** Azure AD uygulaması Yapılandırma Sihirbazı'nda değiştirmelisiniz sonra bunlar, eşleşen değil, Azure AD uygulama yapılandırma bölümünde.
    
    ![QlikSense][qs53]

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

   ![Azure Active Directory düğmesi](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

   !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

   ![Ekle düğmesi](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

   ![Kullanıcı iletişim kutusu](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_04.png)

   a. İçinde **adı** kutusuna **BrittaSimon**.

   b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

   c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

   d. **Oluştur**’a tıklayın.
 
### <a name="create-a-qlik-sense-enterprise-test-user"></a>Qlik algılama Kurumsal test kullanıcısı oluşturma

Bu bölümde, Britta Simon Qlik algılama kuruluşta adlı bir kullanıcı oluşturun. Çalışmak [Qlik algılama Kurumsal İstemci destek ekibi](https://www.qlik.com/us/services/support) Qlik algılama Kurumsal platform kullanıcıları eklemek için. Kullanıcıların oluşturulan ve çoklu oturum açma kullanmadan önce etkinleştirilmelidir. 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Qlik algılama kuruluş erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Qlik algılama kuruluş atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Qlik algılama Kurumsal**.

    ![Uygulamalar listesinde Qlik algılama Kurumsal bağlantı](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Qlik algılama Kurumsal parçasında tıklattığınızda, otomatik olarak Qlik algılama Kurumsal uygulamanıza açan. 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_203.png

[qs6]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_06.png
[qs7]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_07.png
[qs8]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_08.png
[qs9]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_09.png
[qs10]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_10.png
[qs11]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_11.png
[qs12]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_12.png
[qs13]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_13.png
[qs14]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_14.png
[qs15]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_15.png
[qs16]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_16.png
[qs17]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_17.png
[qs18]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_18.png
[qs19]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_19.png
[qs20]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_20.png
[qs24]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_24.png
[qs51]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_51.png
[qs52]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_52.png
[qs53]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_53.png

