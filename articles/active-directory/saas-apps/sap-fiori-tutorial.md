---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile SAP Fiori | Microsoft Docs'
description: Azure Active Directory ve SAP Fiori arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 77ad13bf-e56b-4063-97d0-c82a19da9d56
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/11/2019
ms.author: jeedes
ms.openlocfilehash: 9e7993ee1cb439ebeaa9f64bee55429aa54f9cee
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65903953"
---
# <a name="tutorial-azure-active-directory-integration-with-sap-fiori"></a>Öğretici: Azure Active Directory Tümleştirmesi ile SAP Fiori

Bu öğreticide, Azure Active Directory (Azure AD) ile SAP Fiori tümleştirme konusunda bilgi edinin.

Azure AD ile SAP Fiori tümleştirme aşağıdaki avantajları sağlar:

* Azure AD ile SAP Fiori erişimi denetimi kullanabilirsiniz.
* Kullanıcıları otomatik olarak SAP Fiori kendi Azure AD hesapları (çoklu oturum açma) ile oturum açmanız.
* Hesaplarınız bir merkezi konumda, Azure portalında yönetebilir.

Azure AD ile bir hizmet (SaaS) uygulamasını tümleştirme olarak yazılım hakkında daha fazla bilgi için bkz. [Azure Active Directory'de uygulamalar için çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile SAP Fiori yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD aboneliğiniz yoksa, oluşturun bir [ücretsiz bir hesap](https://azure.microsoft.com/free/) başlamadan önce.
* Tekli etkin oturum ile SAP Fiori abonelik.
* SAP Fiori 7.20 veya sonraki bir sürümü gereklidir.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin ve Azure AD ile SAP Fiori tümleştirme yapılandırın.

SAP Fiori aşağıdaki özellikleri destekler:

* **SP tarafından başlatılan çoklu oturum açma**

## <a name="add-sap-fiori-in-the-azure-portal"></a>Azure portalında SAP Fiori Ekle

Azure AD ile SAP Fiori tümleştirmek için SAP Fiori yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Sol menüde **Azure Active Directory**.

    ![Azure Active Directory seçeneği](common/select-azuread.png)

1. Seçin **kurumsal uygulamalar** > **tüm uygulamaları**.

    ![Kurumsal uygulamalar bölmesi](common/enterprise-applications.png)

1. Bir uygulama eklemek için seçin **yeni uygulama**.

    ![Yeni uygulama seçeneği](common/add-new-app.png)

1. Arama kutusuna **SAP Fiori**. Arama sonuçlarında seçin **SAP Fiori**ve ardından **Ekle**.

    ![Sonuç listesinde SAP Fiori](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açmayı test etme ile SAP Fiori tabanlı adlı bir test kullanıcısı **Britta Simon**. Tek iş için oturum açma için SAP Fiori içinde bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bağlı bir ilişki oluşturmanız gerekir.

Yapılandırma ve Azure AD çoklu oturum açma ile SAP Fiori sınamak için aşağıdaki yapı taşlarını tamamlamanız gerekir:

| Görev | Açıklama |
| --- | --- |
| **[Azure AD çoklu oturum açmayı yapılandırın](#configure-azure-ad-single-sign-on)** | Bu özelliği kullanmak olanak sağlar. |
| **[SAP Fiori çoklu oturum açmayı yapılandırın](#configure-sap-fiori-single-sign-on)** | Uygulamada çoklu oturum açma ayarları yapılandırır. |
| **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)** | Testleri Azure AD çoklu oturum açma kullanıcı Britta Simon adı. |
| **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)** | Azure AD çoklu oturum açmayı kullanmak Britta Simon sağlar. |
| **[Bir SAP Fiori test kullanıcısı oluşturma](#create-an-sap-fiori-test-user)** | Kullanıcı Azure AD gösterimini bağlı SAP Fiori içinde bir karşılığı Britta simon'un oluşturur. |
| **[Çoklu oturum açma testi](#test-single-sign-on)** | Yapılandırma çalıştığını doğrular. |

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma ile SAP Fiori Azure portalında yapılandırın.

1. Yeni bir web tarayıcı penceresi açın ve SAP Fiori şirketinizin sitesi yönetici olarak oturum açın.

1. Emin olun **http** ve **https** Hizmetleri etkin ve ilgili bağlantı noktaları, işlem kodu atanan **SMICM**.

1. Oturum açmak için SAP Business istemcisi SAP sistemine **T01**, çoklu oturum açma gerekli olduğu. Ardından, HTTP güvenlik oturumu yönetimini etkinleştirin.

    1. İşlem kodu Git **SICF_SESSIONS**. Geçerli değerler ile tüm ilgili profili parametrelerinden gösterilir. Bunlar aşağıdaki örnekteki gibi görünür:

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
        > Kuruluş gereksinimlerinize göre parametreleri ayarlayın. Yukarıdaki parametreleri yalnızca örnek olarak verilmiştir.

    1. Gerekirse, SAP sistemine örnek (varsayılan) profilinde parametreleri ayarlayın ve SAP sistemi yeniden başlatın.

    1. Bir HTTP güvenlik oturumu etkinleştirmek için ilgili istemci çift tıklayın.

        ![Geçerli değerler, ilgili profil parametrelerini sayfanın SAP](./media/sapfiori-tutorial/tutorial-sapnetweaver-profileparameter.png)

    1. Aşağıdaki SICF hizmetleri etkinleştirin:

        ```
        /sap/public/bc/sec/saml2
        /sap/public/bc/sec/cdc_ext_service
        /sap/bc/webdynpro/sap/saml2
        /sap/bc/webdynpro/sap/sec_diag_tool (This is only to enable / disable trace)
        ```

1. İşlem kodu Git **SAML2** SAP sistemine iş istemcisinde [**T01/122**]. Yapılandırma kullanıcı Arabirimi, yeni bir tarayıcı penceresinde açılır. Bu örnekte, SAP sistemine 122 iş istemci kullanırız.

    ![SAP Fiori iş istemci oturum açma sayfası](./media/sapfiori-tutorial/tutorial-sapnetweaver-sapbusinessclient.png)

1. Kullanıcı kimliğiniz ve parolanızı girin ve ardından **oturum**.

    ![SAML 2.0 yapılandırma, ABAP sistem T01/122 sayfanın SAP](./media/sapfiori-tutorial/tutorial-sapnetweaver-userpwd.png)

1. İçinde **sağlayıcı adı** kutusunda, yerine **T01122** ile **http:\//T01122**ve ardından **Kaydet**.

    > [!NOTE]
    > Varsayılan olarak, sağlayıcı adı şu biçimdedir \<SID >\<istemci >. Azure AD bekliyor adı biçiminde \<Protokolü > ://\<adı >. Sağlayıcı adı olarak https korumanızı öneririz\://\<SID >\<istemci > SAP Fiori ABAP altyapıları Azure AD'de yapılandırabilmek için.

    ![SAP SAML 2.0 yapılandırma, ABAP sistem T01/122 sayfasında güncel sağlayıcı adı](./media/sapfiori-tutorial/tutorial-sapnetweaver-providername.png)

1. Seçin **yerel sağlayıcı sekmesi** > **meta verileri**.

1. İçinde **SAML 2.0 meta verilerinin** iletişim kutusunda, oluşturulan meta veri XML dosyasını indirin ve bilgisayarınıza kaydedin.

    ![SAP SAML 2.0 meta verilerinin iletişim kutusunda meta verileri indirme bağlantısı](./media/sapfiori-tutorial/tutorial-sapnetweaver-generatesp.png)

1. İçinde [Azure portalında](https://portal.azure.com/), **SAP Fiori** uygulama tümleştirme bölmesinde **çoklu oturum açma**.

    ![Çoklu oturum açma seçeneği](common/select-sso.png)

1. İçinde **tek bir oturum açma yönteminizi seçmeniz** bölmesinde seçin **SAML** veya **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

1. İçinde **yukarı çoklu oturum açma SAML ile ayarlayın** bölmesinde **Düzenle** (açmak için kalem simgesi) **temel SAML yapılandırma** bölmesi.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. İçinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları tamamlayın:

    1. Seçin **meta veri dosyasını karşıya yükleme**.

        ![Karşıya yükleme meta verileri dosyası seçeneği](common/upload-metadata.png)

   1. Meta veri dosyası seçmek için klasör simgesini seçin ve ardından **karşıya**.

       ![Meta veri dosyası seçin ve ardından karşıya yükleme düğmesini seçin.](common/browse-upload-metadata.png)

1. Meta veri dosyası başarıyla yüklendiğinde **tanımlayıcı** ve **yanıt URL'si** değerleri içinde otomatik olarak doldurulur **temel SAML yapılandırma** bölmesi. İçinde **oturum açma URL'si** kutusunda, aşağıdaki desenin bir URL girin: https:\//\<şirket örneğiniz SAP Fiori\>.

    ![Oturum açma bilgileri tek bir SAP Fiori etki alanı ve URL'ler](common/sp-identifier-reply.png)

    > [!NOTE]
    > Bazı müşterilerin rapor hataları yanlış yapılandırılmış ilgili **yanıt URL'si** değerleri. Bu hatayı görürseniz, Örneğiniz için doğru yanıt URL'sini ayarlamak için aşağıdaki PowerShell betiğini kullanabilirsiniz:
    >
    > ```
    > Set-AzureADServicePrincipal -ObjectId $ServicePrincipalObjectId -ReplyUrls "<Your Correct Reply URL(s)>"
    > ``` 
    > 
    > Ayarlayabileceğiniz `ServicePrincipal` betiği çalıştırmadan önce nesne kendiniz kimliği veya burada geçirebilirsiniz.

1. SAP Fiori uygulama SAML onaylamalarını belirli bir biçimde olmasını bekler. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelik değerleri yönetmek için **yukarı çoklu oturum açma SAML ile ayarlanmış** bölmesinde **Düzenle**.

    ![Kullanıcı öznitelikleri bölmesi](common/edit-attribute.png)

1. İçinde **kullanıcı öznitelikleri ve talepler** bölmesinde, önceki görüntüde gösterildiği gibi SAML belirteci öznitelikleri yapılandırın. Ardından, aşağıdaki adımları tamamlayın:

    1. Seçin **Düzenle** açmak için **yönetmek, kullanıcı talepleri** bölmesi.

    1. İçinde **dönüştürme** listesinden **ExtractMailPrefix()**.

    1. İçinde **parametresi 1** listesinden **user.userprinicipalname**.

    1. **Kaydet**’i seçin.

       ![Yönet kullanıcı talepleri bölmesi](./media/sapfiori-tutorial/nameidattribute.png)

       ![Dönüştürme bölümünde Yönet kullanıcı talepleri bölmesi](./media/sapfiori-tutorial/nameidattribute1.png)


1. İçinde **yukarı çoklu oturum açma SAML ile ayarlanmış** bölmesinde, **SAML imzalama sertifikası** bölümünden **indirme** yanındaki **Federasyon meta verileri XML**. Gereksinimlerinize göre bir indirme seçeneğini seçin. Sertifika bilgisayarınıza kaydedin.

    ![Sertifika yükleme seçeneği](common/metadataxml.png)

1. İçinde **SAP Fiori kümesi** bölümünde, gereksinimlerinize göre aşağıdaki URL'ler kopyalayın:

    * Oturum Açma URL'si:
    * Azure AD Tanımlayıcısı
    * Oturum Kapatma URL'si

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

### <a name="configure-sap-fiori-single-sign-on"></a>SAP Fiori çoklu oturum açmayı yapılandırın

1. SAP sistemine oturum açın ve işlem koduna Git **SAML2**. Yeni bir tarayıcı penceresi ile SAML yapılandırma sayfası açılır.

1. Güvenilen kimlik sağlayıcı (Azure AD) için uç noktaları yapılandırmak için seçin **Güvenilen Yayımcılar** sekmesi.

    ![SAP güvenilir sağlayıcılar sekmede](./media/sapfiori-tutorial/tutorial-sapnetweaver-samlconfig.png)

1. Seçin **Ekle**ve ardından **meta veri dosyasını karşıya yükle** bağlam menüsünden.

    ![SAP Ekle ve meta veri dosyasını karşıya yükleme seçenekleri](./media/sapfiori-tutorial/tutorial-sapnetweaver-uploadmetadata.png)

1. Azure portalında indirdiğiniz meta veri dosyasını karşıya yükleyin. **İleri**’yi seçin.

    ![SAP içinde karşıya yüklemek için meta veri dosyası seçin](./media/sapfiori-tutorial/tutorial-sapnetweaver-metadatafile.png)

1. Sonraki sayfada, **diğer** kutusuna, diğer ad girin. Örneğin, **aadsts**. **İleri**’yi seçin.

    ![Diğer ad kutusuna SAP](./media/sapfiori-tutorial/tutorial-sapnetweaver-aliasname.png)

1. Emin olun değerinde **Özet algoritması** kutusu **SHA-256'yı**. **İleri**’yi seçin.

    ![SAP Özet algoritması değeri doğrulayın](./media/sapfiori-tutorial/tutorial-sapnetweaver-identityprovider.png)

1. Altında **tek oturum açma uç noktaları**seçin **HTTP POST**ve ardından **sonraki**.

    ![SAP çoklu oturum açma uç seçenekleri](./media/sapfiori-tutorial/tutorial-sapnetweaver-httpredirect.png)

1. Altında **çoklu oturum kapatma uç noktaları**seçin **HTTP yeniden yönlendirme**ve ardından **sonraki**.

    ![SAP çoklu oturum kapatma uç seçenekleri](./media/sapfiori-tutorial/tutorial-sapnetweaver-httpredirect1.png)

1. Altında **Yapıt uç noktaları**seçin **sonraki** devam etmek için.

    ![SAP yapıt uç seçenekleri](./media/sapfiori-tutorial/tutorial-sapnetweaver-artifactendpoint.png)

1. Altında **kimlik doğrulama gereksinimleri**seçin **son**.

    ![Kimlik doğrulama gereksinimleri seçenekleri ve SAP bitiş seçeneği](./media/sapfiori-tutorial/tutorial-sapnetweaver-authentication.png)

1. Seçin **güvenilen bir sağlayıcı** > **Kimlik Federasyonu** (sayfanın sonundaki). **Düzenle**’yi seçin.

    ![SAP güvenilen bir sağlayıcı ve Kimlik Federasyonu sekmeleri](./media/sapfiori-tutorial/tutorial-sapnetweaver-trustedprovider.png)

1. **Add (Ekle)** seçeneğini belirleyin.

    ![Kimlik Federasyonu sekmesinde Ekle seçeneği](./media/sapfiori-tutorial/tutorial-sapnetweaver-addidentityprovider.png)

1. İçinde **Nameıd desteklenen biçimler** iletişim kutusunda **belirtilmemiş**. **Tamam**’ı seçin.

    ![SAP seçenekleri ve desteklenen biçimler Nameıd iletişim kutusu](./media/sapfiori-tutorial/tutorial-sapnetweaver-nameid.png)

    Değerleri **kullanıcı kimliği kaynak** ve **kullanıcı kimliği eşleme modunu** SAP kullanıcısı ve Azure AD talep arasındaki bağlantıyı belirler.  

    **Senaryo 1**: Azure AD kullanıcı eşleme için SAP kullanıcısı

    1. SAP içinde altında **Nameıd biçimi, Ayrıntılar "Belirsiz"**, ayrıntılarını not edin:

        ![Nameıd biçimi Ayrıntıları "Belirsiz" iletişim kutusunda SAP](./media/sapfiori-tutorial/nameiddetails.png)

    1. Azure portalında altında **kullanıcı öznitelikleri ve talepler**, Azure AD'den gerekli talep unutmayın.

        ![Azure portalındaki kullanıcı öznitelikleri ve talepler iletişim kutusu](./media/sapfiori-tutorial/claimsaad1.png)

    **Senaryo 2**: SU01 yapılandırılan e-posta adresini temel alarak SAP kullanıcı Kimliğini seçin. Bu durumda, e-posta kimliği SU01 SSO gerektiren her bir kullanıcı için yapılandırılmalıdır.

    1.  SAP içinde altında **Nameıd biçimi, Ayrıntılar "Belirsiz"**, ayrıntılarını not edin:

        ![Nameıd biçimi Ayrıntıları "Belirsiz" iletişim kutusunda SAP](./media/sapfiori-tutorial/tutorial-sapnetweaver-nameiddetails1.png)

    1. Azure portalında altında **kullanıcı öznitelikleri ve talepler**, Azure AD'den gerekli talep unutmayın.

       ![Azure portalındaki kullanıcı öznitelikleri ve talepler iletişim kutusu](./media/sapfiori-tutorial/claimsaad2.png)

1. Seçin **Kaydet**ve ardından **etkinleştirme** kimlik sağlayıcısı etkinleştirmek için.

    ![Kaydet ve etkinleştir SAP içinde seçenekleri](./media/sapfiori-tutorial/configuration1.png)

1. Seçin **Tamam** istendiğinde.

    ![SAP SAML 2.0 yapılandırma iletişim kutusunda Tamam seçeneği](./media/sapfiori-tutorial/configuration2.png)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında Britta Simon adlı bir test kullanıcısı oluşturun.

1. Azure portalında **Azure Active Directory** > **kullanıcılar** > **tüm kullanıcılar**.

    ![Kullanıcılar ve tüm kullanıcılar seçenekleri](common/users.png)

1. Seçin **yeni kullanıcı**.

    ![Yeni kullanıcı seçeneği](common/new-user.png)

1. İçinde **kullanıcı** bölmesinde, aşağıdaki adımları tamamlayın:

    1. İçinde **adı** kutusuna **BrittaSimon**.
  
    1. İçinde **kullanıcı adı** kutusuna **brittasimon\@\<your-şirket etki alanı >.\< Uzantı >**. Örneğin, **brittasimon\@contoso.com**.

    1. Seçin **Show parola** onay kutusu. Görüntülenen değer azaltma **parola** kutusu.

    1. **Oluştur**’u seçin.

    ![Kullanıcı bölmesi](common/user-properties.png)

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Filiz Azure çoklu oturum açma kullanabilmeniz için bu bölümde, Britta Simon SAP Fiori erişim.

1. Azure portalında **kurumsal uygulamalar** > **tüm uygulamaları** > **SAP Fiori**.

    ![Kurumsal uygulamalar bölmesi](common/enterprise-applications.png)

1. Uygulamalar listesinde **SAP Fiori**.

    ![SAP Fiori uygulamalar listesinde](common/all-applications.png)

1. Menüde **kullanıcılar ve gruplar**.

    ![Kullanıcılar ve gruplar seçeneği](common/users-groups-blade.png)

1. Seçin **Kullanıcı Ekle**. Ardından **ataması ekleme** bölmesinde **kullanıcılar ve gruplar**.

    ![Ekle atama bölmesi](common/add-assign-user.png)

1. İçinde **kullanıcılar ve gruplar** bölmesinde **Britta Simon** kullanıcılar listesinde. **Seç**’i seçin.

1. SAML onaylaması rol değeri de beklediğiniz varsa **rol seçme** bölmesinde, listeden kullanıcı için uygun rolü seçin. **Seç**’i seçin.

1. İçinde **atama Ekle** bölmesinde **atama**.

### <a name="create-an-sap-fiori-test-user"></a>Bir SAP Fiori test kullanıcısı oluşturma

Bu bölümde, SAP Fiori içinde Britta Simon adlı bir kullanıcı oluşturun. Şirket içi SAP uzmanları veya kuruluş SAP iş ortağınız SAP Fiori platform kullanıcı eklemeye çalışın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

1. Azure AD kimlik sağlayıcısı SAP Fiori etkinleştirildikten sonra tek (bir kullanıcı adı ve parola istenir olmamalıdır) oturum test etmek için aşağıdaki URL'lerden birini erişmeyi deneyin:

    * https:\//\<sapurl\>/sap/bc/bsp/sap/it00/default.htm
    * https:\//\<sapurl\>/sap/bc/bsp/sap/it00/default.htm

    > [!NOTE]
    > Değiştirin *sapurl* gerçek SAP ana bilgisayar adına sahip.

1. Test URL'si SAP aşağıdaki test uygulama sayfasına almanız gerekir. Sayfa açarsa, Azure AD çoklu oturum açma başarıyla kuruldu.

    ![Standart, içinde SAP uygulama sayfası test](./media/sapfiori-tutorial/testingsso.png)

1. Bir kullanıcı adı ve parola istenirse, sorunun tanılanmasına yardımcı olmak için izlemeyi etkinleştirin. İzleme için şu URL'yi kullanın: https:\//\<sapurl\>/sap/bc/webdynpro/sap/sec_diag_tool? sap istemci = 122 & sap dil = tr #.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bu makaleleri gözden geçirin:

- [İçin Azure Active Directory ile SaaS uygulamalarını tümleştirme konusundaki öğreticilerin listesine](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)
- [Azure Active Directory'de uygulamalar için çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)
- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
