---
title: 'Öğretici: SAP NetWeaver ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: SAP NetWeaver ile Azure Active Directory arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 1b9e59e3-e7ae-4e74-b16c-8c1a7ccfdef3
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/19/2018
ms.author: jeedes
ms.openlocfilehash: fac22508e679c1e1c93ec62a5b120ba9c7c52317
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2018
ms.locfileid: "52162401"
---
# <a name="tutorial-azure-active-directory-integration-with-sap-netweaver"></a>Öğretici: SAP NetWeaver ile Azure Active Directory tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile SAP NetWeaver tümleştirme konusunda bilgi edinin.

SAP NetWeaver'ı Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- SAP NetWeaver erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan (çoklu oturum açma) SAP NetWeaver için açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Önkoşullar

SAP NetWeaver ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- SAP NetWeaver çoklu oturum açmayı abonelik etkin.
- SAP NetWeaver V7.20 en az gerekli

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin.
Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. SAP NetWeaver galeri ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-sap-netweaver-from-the-gallery"></a>SAP NetWeaver galeri ekleme

Azure AD'de SAP NetWeaver tümleştirmesini yapılandırmak için SAP NetWeaver Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**SAP NetWeaver Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **SAP NetWeaver**seçin **SAP NetWeaver** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde SAP NetWeaver](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve SAP NetWeaver'ın "Britta Simon" adlı bir test kullanıcı tabanlı Azure AD çoklu oturum açmayı sınayın.

Tek iş için oturum açma için Azure AD ne SAP NetWeaver karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile SAP NetWeaver ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma SAP NetWeaver ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[SAP NetWeaver test kullanıcısı oluşturma](#creating-sapnetweaver-test-user)**  - kullanıcı Azure AD gösterimini bağlı olduğu SAP NetWeaver Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve SAP NetWeaver uygulamanızda çoklu oturum açmayı yapılandırın.

**SAP NetWeaver ile Azure AD çoklu oturum açmayı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

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

6. Değiştirin **sağlayıcı adı** T01122 için gelen **http://T01122** tıklayın **Kaydet**.

    > [!NOTE]
    > Varsayılan sağlayıcı adı ile gelir olarak <sid> <client> biçimi ancak Azure AD'ye bekliyor adı biçiminde <protocol>://<name>, sağlayıcı adı ' https://'olarak korumak için önerilen<sid> <client> birden çok SAP izin vermek için Azure AD'de yapılandırmak için NetWeaver ABAP altyapıları.

    ![Sertifika indirme bağlantısı](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_providername.png)

7. **Hizmet sağlayıcısı meta verileri oluşturuluyor**: - şu yapılandırma ile işiniz bittiğinde **yerel sağlayıcı** ve **güvenilir sağlayıcılar** ayarları SAML 2.0 kullanıcı arabiriminde, sonraki adım için olacaktır (Bu, tüm ayarlarını, kimlik doğrulaması bağlamı ve diğer yapılandırmalarda SAP içerir) hizmet sağlayıcısının meta veri dosyası oluşturur. Bu dosyayı oluşturduktan sonra bunu Azure AD'de yüklemeniz gerekir.

    ![Sertifika indirme bağlantısı](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_generatesp.png)

    a. Git **yerel sağlayıcı sekmesi**.

    b. Tıklayarak **meta verileri**.

    c. Oluşturulan Kaydet **meta verileri XML dosyası** bilgisayarınızdaki ve bunu karşıya **temel SAML yapılandırma** polulate otomatik olarak bölüm **tanımlayıcı** ve  **Yanıt URL'si** Azure portalında değerleri.

8. Azure portalında, üzerinde **SAP NetWeaver** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

9. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunu tıklatın **seçin** için **SAML** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açmayı yapılandırın](common/tutorial_general_301.png)

10. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Çoklu oturum açmayı yapılandırın](common/editconfigure.png)

11. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    a. Tıklayın **meta veri dosyasını karşıya yükleme** yüklenecek **hizmet sağlayıcısı meta veri dosyası** , daha önce aldığınız.

    ![Meta veri dosyasını karşıya yükleyin](common/editmetadataupload.png)

    b. Tıklayarak **klasör logosu** meta veri dosyası seçin ve **karşıya**.

    ![Meta veri dosyasını karşıya yükleyin](common/uploadmetadata.png)

    c. Meta veri dosyası başarıyla karşıya yüklendikten sonra **tanımlayıcı** ve **yanıt URL'si** değerlerini alma otomatik olarak doldurulmuş **temel SAML yapılandırma** aşağıda gösterildiği gibi metin bölümü :

    ![SAP NetWeaver etki alanı ve URL'ler tek oturum açma bilgileri](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_url.png)

    d. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<your company instance of SAP NetWeaver>`

12. SAP NetWeaver uygulaması belirli bir biçimde SAML onaylamalarını bekliyor. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz **kullanıcı öznitelikleri** uygulama tümleştirme sayfasında bölümü. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için düğmeyi **kullanıcı öznitelikleri** iletişim.

    ![Öznitelik bölümü](./media/sapnetweaver-tutorial/edit_attribute.png)

13. İçinde **kullanıcı taleplerini** bölümünde **kullanıcı öznitelikleri** iletişim kutusunda, SAML belirteci özniteliği yukarıdaki görüntüde gösterilen şekilde yapılandırın ve aşağıdaki adımları gerçekleştirin:

    a. Tıklayarak **Düzenle** açmak için simgeyi **yönetmek, kullanıcı talepleri** iletişim.
    
    ![Öznitelik bölümü](./media/sapnetweaver-tutorial/nameidattribute.png)

    b. Üzerinde **yönetmek, kullanıcı talepleri** sekmesinde, aşağıdaki adımları gerçekleştirin:

    ![Öznitelik bölümü](./media/sapnetweaver-tutorial/nameidattribute1.png)

    * Seçin **dönüştürme**.
  
    * Gelen **dönüştürme** listesinden `ExtractMailPrefix()`.
  
    * Gelen **parametresi 1** listesinden `user.userprincipalname`.

    * **Kaydet**’e tıklayın.

14. Üzerinde **SAML imzalama sertifikası** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta verileri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_certificate.png)

15. Üzerinde **SAP NetWeaver ' ayarlamak** bölümünde, ihtiyacınıza göre uygun URL'yi kopyalayın.

    a. Oturum açma URL'si

    b. Azure AD tanımlayıcısı

    c. Oturum Kapatma URL'si

    ![SAP NetWeaver yapılandırma](common/configuresection.png)

16. SAP sistemine oturum açma ve işlem kodu SAML2 gidin. SAML yapılandırma ekranında ile yeni bir tarayıcı penceresi açar.

17. Sağlayıcı (Azure AD) uç noktalar için güvenilen kimlik yapılandırmak için Git **Güvenilen Yayımcılar** sekmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_samlconfig.png)

18. Tuşuna **Ekle** seçip **meta veri dosyasını karşıya yükle** bağlam menüsünden.

    ![Çoklu oturum açmayı yapılandırın](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_uploadmetadata.png)

19. Azure portalından indirdiğiniz meta veri dosyasını karşıya yükleyin.

    ![Çoklu oturum açmayı yapılandırın](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_metadatafile.png)

20. Sonraki ekranda diğer adı yazın. Örneğin aadsts ve ENTER tuşuna **sonraki** devam etmek için.

    ![Çoklu oturum açmayı yapılandırın](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_aliasname.png)

21. Emin olun, **Özet algoritması** olmalıdır **SHA-256'yı** ve herhangi bir değişiklik gerektirmez ve basın **sonraki**.

    ![Çoklu oturum açmayı yapılandırın](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_identityprovider.png)

22. Üzerinde **tek oturum açma uç noktaları**, kullanın **HTTP POST** tıklatıp **sonraki** devam etmek için.

    ![Çoklu oturum açmayı yapılandırın](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_httpredirect.png)

23. Üzerinde **çoklu oturum kapatma uç noktaları** seçin **HTTPRedirect** tıklatıp **sonraki** devam etmek için.

    ![Çoklu oturum açmayı yapılandırın](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_httpredirect1.png)

24. Üzerinde **Yapıt uç noktaları**, basın **sonraki** devam etmek için.

    ![Çoklu oturum açmayı yapılandırın](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_artifactendpoint.png)

25. Üzerinde **kimlik doğrulama gereksinimleri**, tıklayın **son**.

    ![Çoklu oturum açmayı yapılandırın](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_authentication.png)

26. Go için sekmesinde **güvenilen bir sağlayıcı** > **Kimlik Federasyonu** (Alttan ekranın). **Düzenle**’ye tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_trustedprovider.png)

27. Tıklayın **Ekle** altında **Kimlik Federasyonu** sekme (alt pencere).

    ![Çoklu oturum açmayı yapılandırın](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_addidentityprovider.png)

28. Açılır penceresinden seçmek **belirtilmemiş** gelen **Nameıd desteklenen biçimler** ve Tamam'a tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_nameid.png)

29. Unutmayın **kullanıcı kimliği kaynak** ve **kullanıcı kimliği eşleme modunu** değerleri SAP kullanıcısı ve Azure AD talep arasındaki bağlantıyı belirler.  

    ####<a name="scenario-sap-user-to-azure-ad-user-mapping"></a>Senaryo: SAP kullanıcı Azure AD kullanıcı eşlemesi.

    a. SAP'den Nameıd ayrıntıları ekran görüntüsü.

    ![Çoklu oturum açmayı yapılandırın](./media/sapnetweaver-tutorial/nameiddetails.png)

    b. Azure AD'den gerekli talep bahseden ekran görüntüsü.

    ![Çoklu oturum açmayı yapılandırın](./media/sapnetweaver-tutorial/claimsaad1.png)

    ####<a name="scenario-select-sap-user-id-based-on-configured-email-address-in-su01-in-this-case-email-id-should-be-configured-in-su01-for-each-user-who-requires-sso"></a>Senaryo: SU01 yapılandırılan e-posta adresini temel alarak SAP kullanıcı kimliği'ni seçin. Bu durumda e-posta kimliği su01 SSO gerektiren her bir kullanıcı için yapılandırılmalıdır.

    a.  SAP'den Nameıd ayrıntıları ekran görüntüsü.

    ![Çoklu oturum açmayı yapılandırın](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_nameiddetails1.png)

    b. Azure AD'den gerekli talep bahseden ekran görüntüsü.

    ![Çoklu oturum açmayı yapılandırın](./media/sapnetweaver-tutorial/claimsaad2.png)

30. Tıklayın **Kaydet** ve ardından **etkinleştirme** kimlik sağlayıcısı etkinleştirmek için.

    ![Çoklu oturum açmayı yapılandırın](./media/sapnetweaver-tutorial/configuration1.png)

31. Tıklayın **Tamam** kez istenir.

    ![Çoklu oturum açmayı yapılandırın](./media/sapnetweaver-tutorial/configuration2.png)

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    ![Azure AD kullanıcısı oluşturun][100]

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Bir Azure AD test kullanıcısı oluşturma](common/create_aaduser_01.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Bir Azure AD test kullanıcısı oluşturma](common/create_aaduser_02.png)

    a. İçinde **adı** alanına **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alanına **brittasimon@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **özellikleri**seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’u seçin.

### <a name="creating-sap-netweaver-test-user"></a>SAP NetWeaver test kullanıcısı oluşturma

Bu bölümde, Britta Simon SAP NetWeaver adlı bir kullanıcı oluşturun. Lütfen şirket içi SAP Uzman takımınızın iş veya SAP NetWeaver platform kullanıcıları eklemek için kuruluş SAP iş ortağınız ile çalışır.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, SAP NetWeaver için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**.

    ![Kullanıcı Ata][201]

2. Uygulamalar listesinde **SAP NetWeaver**.

    ![Çoklu oturum açmayı yapılandırın](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_app.png) 

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. İçinde **atama Ekle** iletişim kutusunda **atama** düğmesi.

### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

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

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: common/tutorial_general_01.png
[2]: common/tutorial_general_02.png
[3]: common/tutorial_general_03.png
[4]: common/tutorial_general_04.png

[100]: common/tutorial_general_100.png

[201]: common/tutorial_general_201.png
[202]: common/tutorial_general_202.png
[203]: common/tutorial_general_203.png
