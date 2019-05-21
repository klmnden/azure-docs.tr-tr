---
title: 'Öğretici: SAP NetWeaver ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: SAP NetWeaver ile Azure Active Directory arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 1b9e59e3-e7ae-4e74-b16c-8c1a7ccfdef3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/11/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: b648b8458c7f91cae6edb079fbd2ac78553dd969
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65903534"
---
# <a name="tutorial-azure-active-directory-integration-with-sap-netweaver"></a>Öğretici: SAP NetWeaver ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile SAP NetWeaver tümleştirme konusunda bilgi edinin.
SAP NetWeaver'ı Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* SAP NetWeaver erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak (çoklu oturum açma) SAP NetWeaver için oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

SAP NetWeaver ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* SAP NetWeaver çoklu oturum açmayı abonelik etkin.
* SAP NetWeaver V7.20 en az gerekli

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* SAP NetWeaver'ı destekleyen **SP** tarafından başlatılan

## <a name="adding-sap-netweaver-from-the-gallery"></a>SAP NetWeaver galeri ekleme

Azure AD'de SAP NetWeaver tümleştirmesini yapılandırmak için SAP NetWeaver Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**SAP NetWeaver Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **SAP NetWeaver**seçin **SAP NetWeaver** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde SAP NetWeaver](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma SAP NetWeaver tabanlı adlı bir test kullanıcı ile test etme **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ile SAP NetWeaver ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma SAP NetWeaver ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[SAP NetWeaver çoklu oturum açmayı yapılandırma](#configure-sap-netweaver-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[SAP NetWeaver test kullanıcısı oluşturma](#create-sap-netweaver-test-user)**  - kullanıcı Azure AD gösterimini bağlı olduğu SAP NetWeaver Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

SAP NetWeaver ile Azure AD çoklu oturum açmayı yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Yeni bir web tarayıcı penceresi ve günlük SAP NetWeaver şirket sitenize yönetici olarak açın.

2. Emin olun **http** ve **https** Hizmetleri etkin ve uygun bağlantı noktalarının atanır **SMICM** T-kod.

3. SAP SSO gerekli olduğu sisteminin (T01) iş istemcide oturum açan ve HTTP güvenlik oturumu yönetimini etkinleştirin.

    a. İşlem kodu Git **SICF_SESSIONS**. Geçerli değerler ile tüm ilgili profil parametreleri gösterir. Bunlar aşağıda görünmesi:-
    ```
    login/create_sso2_ticket = 2
    login/accept_sso2_ticket = 1
    login/ticketcache_entries_max = 1000
    login/ticketcache_off = 0  login/ticket_only_by_https = 0 
    icf/set_HTTPonly_flag_on_cookies = 3
    icf/user_recheck = 0  http/security_session_timeout = 1800
    http/security_context_cache_size = 2500
    rdisp/plugin_auto_logout = 1800
    rdisp/autothtime = 60
    ```
    >[!NOTE]
    > Yukarıda, kuruluşunuzun gereksinimlerine uygun parametreleri ayarlayın, parametreleri verilen yalnızca bir gösterge olarak burada.

    b. Parametrelerinde SAP sisteminin örnek/varsayılan profil ayarlamak ve SAP sistemi yeniden başlatma gerekli.

    c. Çift ilgili istemci HTTP güvenlik oturumu etkinleştirmek için tıklayın.

    ![Sertifika indirme bağlantısı](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_profileparameter.png)

    d. SICF hizmetleri etkinleştirin:
    ```
    /sap/public/bc/sec/saml2
    /sap/public/bc/sec/cdc_ext_service
    /sap/bc/webdynpro/sap/saml2
    /sap/bc/webdynpro/sap/sec_diag_tool (This is only to enable / disable trace)
    ```
4. İşlem kodu Git **SAML2** iş istemcisinde SAP sistemi T01/122. Bu, bir kullanıcı arabirimi bir tarayıcıda açılır. Bu örnekte, biz 122 SAP business istemcisi varsayılır.

    ![Sertifika indirme bağlantısı](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_sapbusinessclient.png)

5. Kullanıcı adı ve kullanıcı arabiriminde girin ve parola sağlayın **Düzenle**.

    ![Sertifika indirme bağlantısı](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_userpwd.png)

6. Değiştirin **sağlayıcı adı** T01122 için gelen `http://T01122` tıklayın **Kaydet**.

    > [!NOTE]
    > Varsayılan sağlayıcı adı ile gelir olarak `<sid><client>` biçimi ancak Azure AD'ye bekliyor adı biçiminde `<protocol>://<name>`, sağlayıcı adı olarak korumak için önerilen `https://<sid><client>` Azure AD'de yapılandırmak birden fazla SAP NetWeaver ABAP altyapıları izin vermek için.

    ![Sertifika indirme bağlantısı](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_providername.png)

7. **Hizmet sağlayıcısı meta verileri oluşturuluyor**: - şu yapılandırma ile işiniz bittiğinde **yerel sağlayıcı** ve **güvenilir sağlayıcılar** ayarları SAML 2.0 kullanıcı arabiriminde, sonraki adım için olacaktır (Bu, tüm ayarlarını, kimlik doğrulaması bağlamı ve diğer yapılandırmalarda SAP içerir) hizmet sağlayıcısının meta veri dosyası oluşturur. Bu dosyayı oluşturduktan sonra bunu Azure AD'de yüklemeniz gerekir.

    ![Sertifika indirme bağlantısı](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_generatesp.png)

    a. Git **yerel sağlayıcı sekmesi**.

    b. Tıklayarak **meta verileri**.

    c. Oluşturulan Kaydet **meta verileri XML dosyası** bilgisayarınızdaki ve bunu karşıya **temel SAML yapılandırma** otomatik olarak doldurmak için bölümü **tanımlayıcı** ve  **Yanıt URL'si** Azure portalında değerleri.

8. İçinde [Azure portalında](https://portal.azure.com/), **SAP NetWeaver** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

9. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

10. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

11. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    a. Tıklayın **meta veri dosyasını karşıya yükleme** yüklenecek **hizmet sağlayıcısı meta veri dosyası** , daha önce aldığınız.

    ![Meta veri dosyasını yükleyin](common/upload-metadata.png)

    b. Tıklayarak **klasör logosu** meta veri dosyası seçin ve **karşıya**.

    ![meta veri dosyası seçin](common/browse-upload-metadata.png)

    c. Meta veri dosyası başarıyla karşıya yüklendikten sonra **tanımlayıcı** ve **yanıt URL'si** değerlerini alma otomatik olarak doldurulmuş **temel SAML yapılandırma** bölümünde gösterildiği gibi metin kutusu Aşağıda:

    ![SAP NetWeaver etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier-reply.png)

    d. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<your company instance of SAP NetWeaver>`

    > [!NOTE]
    > Kendi örneklerine yapılandırılmış yanlış yanıt URL'sinin bir hata bildirimi, bazı müşterilerin gördük. Herhangi bir hata alırsanız, PowerShell Betiği bir iş yaklaşık Örneğiniz için doğru yanıt URL'sini ayarlamak için kullanabilirsiniz.:
    > ```
    > Set-AzureADServicePrincipal -ObjectId $ServicePrincipalObjectId -ReplyUrls "<Your Correct Reply URL(s)>"
    > ``` 
    > ServicePrincipal nesne kimliği kendiniz ayarlanması ya da, ayrıca burada geçirebilirsiniz.

12. SAP NetWeaver uygulaması belirli bir biçimde SAML onaylamalarını bekliyor. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz **kullanıcı öznitelikleri** uygulama tümleştirme sayfasında bölümü. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için düğmeyi **kullanıcı öznitelikleri** iletişim.

    ![image](common/edit-attribute.png)

13. İçinde **kullanıcı taleplerini** bölümünde **kullanıcı öznitelikleri** iletişim kutusunda, SAML belirteci özniteliği yukarıdaki görüntüde gösterilen şekilde yapılandırın ve aşağıdaki adımları gerçekleştirin:

    a. Tıklayın **düzenleme simgesi** açmak için **yönetmek, kullanıcı talepleri** iletişim.

    ![image](./media/sapnetweaver-tutorial/nameidattribute.png)

    ![image](./media/sapnetweaver-tutorial/nameidattribute1.png)

    b. Gelen **dönüştürme** listesinden **ExtractMailPrefix()**.

    c. Gelen **parametresi 1** listesinden **user.userprinicipalname**.

    d. **Kaydet**’e tıklayın.

14. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

15. Üzerinde **SAP NetWeaver ' ayarlamak** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-sap-netweaver-single-sign-on"></a>SAP NetWeaver tek oturum açmayı yapılandırın

1. SAP sistemine oturum açma ve işlem kodu SAML2 gidin. SAML yapılandırma ekranında ile yeni bir tarayıcı penceresi açar.

2. Sağlayıcı (Azure AD) uç noktalar için güvenilen kimlik yapılandırmak için Git **Güvenilen Yayımcılar** sekmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_samlconfig.png)

3. Tuşuna **Ekle** seçip **meta veri dosyasını karşıya yükle** bağlam menüsünden.

    ![Çoklu oturum açmayı yapılandırın](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_uploadmetadata.png)

4. Azure portalından indirdiğiniz meta veri dosyasını karşıya yükleyin.

    ![Çoklu oturum açmayı yapılandırın](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_metadatafile.png)

5. Sonraki ekranda diğer adı yazın. Örneğin aadsts ve ENTER tuşuna **sonraki** devam etmek için.

    ![Çoklu oturum açmayı yapılandırın](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_aliasname.png)

6. Emin olun, **Özet algoritması** olmalıdır **SHA-256'yı** ve herhangi bir değişiklik gerektirmez ve basın **sonraki**.

    ![Çoklu oturum açmayı yapılandırın](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_identityprovider.png)

7. Üzerinde **tek oturum açma uç noktaları**, kullanın **HTTP POST** tıklatıp **sonraki** devam etmek için.

    ![Çoklu oturum açmayı yapılandırın](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_httpredirect.png)

8. Üzerinde **çoklu oturum kapatma uç noktaları** seçin **HTTPRedirect** tıklatıp **sonraki** devam etmek için.

    ![Çoklu oturum açmayı yapılandırın](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_httpredirect1.png)

9. Üzerinde **Yapıt uç noktaları**, basın **sonraki** devam etmek için.

    ![Çoklu oturum açmayı yapılandırın](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_artifactendpoint.png)

10. Üzerinde **kimlik doğrulama gereksinimleri**, tıklayın **son**.

    ![Çoklu oturum açmayı yapılandırın](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_authentication.png)

11. Go için sekmesinde **güvenilen bir sağlayıcı** > **Kimlik Federasyonu** (Alttan ekranın). **Düzenle**‘ye tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_trustedprovider.png)

12. Tıklayın **Ekle** altında **Kimlik Federasyonu** sekme (alt pencere).

    ![Çoklu oturum açmayı yapılandırın](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_addidentityprovider.png)

13. Açılır penceresinden seçmek **belirtilmemiş** gelen **Nameıd desteklenen biçimler** ve Tamam'a tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_nameid.png)

14. Unutmayın **kullanıcı kimliği kaynak** ve **kullanıcı kimliği eşleme modunu** değerleri SAP kullanıcısı ve Azure AD talep arasındaki bağlantıyı belirler.  

    #### <a name="scenario-sap-user-to-azure-ad-user-mapping"></a>Senaryo: Azure AD kullanıcı eşleme SAP kullanıcı.

    a. SAP Nameıd ayrıntıları ekran görüntüsü.

    ![Çoklu oturum açmayı yapılandırın](./media/sapnetweaver-tutorial/nameiddetails.png)

    b. Azure AD'den gerekli talep bahseden bir ekran görüntüsü.

    ![Çoklu oturum açmayı yapılandırın](./media/sapnetweaver-tutorial/claimsaad1.png)

    #### <a name="scenario-select-sap-user-id-based-on-configured-email-address-in-su01-in-this-case-email-id-should-be-configured-in-su01-for-each-user-who-requires-sso"></a>Senaryo: SU01 yapılandırılan e-posta adresini temel alarak SAP kullanıcı kimliğini seçin. Bu durumda e-posta kimliği su01 SSO gerektiren her bir kullanıcı için yapılandırılmalıdır.

    a.  SAP Nameıd ayrıntıları ekran görüntüsü.

    ![Çoklu oturum açmayı yapılandırın](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_nameiddetails1.png)

    b. Azure AD'den gerekli talep bahseden bir ekran görüntüsü.

    ![Çoklu oturum açmayı yapılandırın](./media/sapnetweaver-tutorial/claimsaad2.png)

15. Tıklayın **Kaydet** ve ardından **etkinleştirme** kimlik sağlayıcısı etkinleştirmek için.

    ![Çoklu oturum açmayı yapılandırın](./media/sapnetweaver-tutorial/configuration1.png)

16. Tıklayın **Tamam** kez istenir.

    ![Çoklu oturum açmayı yapılandırın](./media/sapnetweaver-tutorial/configuration2.png)

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

Bu bölümde, SAP NetWeaver için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **SAP NetWeaver**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **SAP NetWeaver**.

    ![Uygulamalar listesini SAP NetWeaver bağlantıdaki](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-sap-netweaver-test-user"></a>SAP NetWeaver test kullanıcısı oluşturma

Bu bölümde, Britta Simon SAP NetWeaver adlı bir kullanıcı oluşturun. Lütfen şirket içi SAP Uzman takımınızın iş veya SAP NetWeaver platform kullanıcıları eklemek için kuruluş SAP iş ortağınız ile çalışır.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

1. Azure AD kimlik sağlayıcısı etkinleştirildikten sonra SSO denetlemek için URL'ye erişilirken deneyin (kullanıcı adı ve parola için herhangi bir istem olacaktır)

    `https://<sapurl>/sap/bc/bsp/sap/it00/default.htm`

    (veya) aşağıdaki URL'yi kullanın

    `https://<sapurl>/sap/bc/bsp/sap/it00/default.htm`

    > [!NOTE]
    > Sapurl gerçek SAP konak adı ile değiştirin.

2. Yukarıdaki URL için belirtilen ekranın altına almalıdır. En fazla ulaşabilir, sayfanın bir altında Azure AD SSO başarıyla kuruldu.

    ![Çoklu oturum açmayı yapılandırın](./media/sapnetweaver-tutorial/testingsso.png)

3. Kullanıcı adı ve parola istemi meydana gelirse, sorunu aşağıdaki URL'yi kullanarak izlemeyi etkinleştir tarafından Lütfen tanılayın

    `https://<sapurl>/sap/bc/webdynpro/sap/sec_diag_tool?sap-client=122&sap-language=EN#`

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
