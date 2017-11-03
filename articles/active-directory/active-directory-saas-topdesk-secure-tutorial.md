---
title: "Öğretici: Azure Active Directory Tümleştirme TOPdesk - güvenli ile | Microsoft Docs"
description: "TOPdesk - kullanmayı öğrenin Azure çoklu oturum açma, otomatik sağlama ve daha fazlasını sağlamak için Active Directory'ye ile güvenli!."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 8e149d2d-7849-48ec-9993-31f4ade5fdb4
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/22/2017
ms.author: jeedes
ms.openlocfilehash: 28f0542dbe87bb34c83a7852db7c3a9fef055ce9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-topdesk---secure"></a>Öğretici: Azure Active Directory Tümleştirme TOPdesk - güvenli ile
Bu öğreticinin amacı, Azure ve TOPdesk - güvenli tümleştirmesini göstermektir.  
Bu öğreticide gösterilen senaryo, aşağıdaki öğeleri zaten sahip olduğunuzu varsayar:

* Geçerli bir Azure aboneliği
* TOPdesk - bir abonelik etkin güvenli çoklu oturum açma

Bu öğreticiyi tamamladıktan sonra Azure AD kullanıcıları için TOPdesk - güvenli atamış tek oturum açın, TOPdesk - güvenli şirket site (servis sağlayıcı tarafından başlatılan oturum açma) veya kullanarak uygulamayı yapabilirsiniz [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md).

Bu öğreticide gösterilen senaryo, aşağıdaki yapı taşlarını oluşur:

1. Uygulama tümleştirmesi TOPdesk - güvenli için etkinleştirme
2. Çoklu oturum açmayı yapılandırma
3. Kullanıcı hazırlama işleminin yapılandırılması
4. Kullanıcılar atama

![Senaryo](./media/active-directory-saas-topdesk-secure-tutorial/IC790596.png "senaryosu")

## <a name="enabling-the-application-integration-for-topdesk---secure"></a>Uygulama tümleştirmesi TOPdesk - güvenli için etkinleştirme
Bu bölümün amacı TOPdesk - güvenli için uygulama tümleştirme sağlamak üzere nasıl anahat sağlamaktır.

### <a name="to-enable-the-application-integration-for-topdesk---secure-perform-the-following-steps"></a>TOPdesk - uygulama tümleştirmesini etkinleştirmek için güvenli, aşağıdaki adımları gerçekleştirin:
1. Azure Klasik portalında, sol gezinti bölmesinde tıklatın **Active Directory**.
   
    ![Active Directory](./media/active-directory-saas-topdesk-secure-tutorial/IC700993.png "Active Directory")

2. Gelen **Directory** listesinde, directory tümleştirmesini etkinleştirmek istediğiniz dizini seçin.

3. Dizin görünümünde uygulamaları görünümü açmak için **uygulamaları** üst menüde.
   
    ![Uygulamaları](./media/active-directory-saas-topdesk-secure-tutorial/IC700994.png "uygulamalar")

4. Tıklatın **Ekle** sayfanın sonundaki.
   
    ![Uygulama ekleme](./media/active-directory-saas-topdesk-secure-tutorial/IC749321.png "uygulama ekleme")

5. Üzerinde **ne yapmak istiyorsunuz** iletişim kutusunda, tıklatın **Galeriden bir uygulama eklemek**.
   
    ![Galeriden bir uygulama eklemek](./media/active-directory-saas-topdesk-secure-tutorial/IC749322.png "Galeriden bir uygulama ekleme")

6. İçinde **arama kutusu**, türü **TOPdesk - güvenli**.
   
    ![Uygulama Galerisi](./media/active-directory-saas-topdesk-secure-tutorial/IC790597.png "uygulama Galerisi")

7. Sonuçlar bölmesinde seçin **TOPdesk - güvenli**ve ardından **tam** uygulama eklemek için.
   
    ![TOPdesk - güvenli](./media/active-directory-saas-topdesk-secure-tutorial/IC791933.png "TOPdesk - güvenli")

## <a name="configuring-single-sign-on"></a>Çoklu oturum açmayı yapılandırma
Bu bölümün amacı kullanıcıların TOPdesk için - kimlik doğrulaması sağlamak nasıl ana hatlarını belirlemek için olduğundan SAML protokolünü temel Federasyon kullanarak Azure AD ile kendi hesaplarını güvenli.  
Yapılandırma tek oturum açma için TOPdesk - güvenli bir logo simgesini dosyayı karşıya yüklemeyi gerektirir. Simge dosyası almak için TOPdesk Destek ekibine başvurun.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>Çoklu oturum açma yapılandırmak için aşağıdaki adımları gerçekleştirin:
1. Oturum, **TOPdesk - güvenli** yönetici olarak şirket site.
2. İçinde **TOPdesk** menüsünde tıklatın **ayarları**.
   
    ![Ayarları](./media/active-directory-saas-topdesk-secure-tutorial/IC790598.png "ayarları")

3. Tıklatın **oturum açma ayarları**.
   
    ![Oturum açma ayarları](./media/active-directory-saas-topdesk-secure-tutorial/IC790599.png "oturum açma ayarları")

4. Genişletme **oturum açma ayarları** menüsüne ve ardından **genel**.
   
    ![Genel](./media/active-directory-saas-topdesk-secure-tutorial/IC790600.png "genel")

5. İçinde **güvenli** bölümünü **SAML oturum açma** yapılandırma bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Teknik ayarları](./media/active-directory-saas-topdesk-secure-tutorial/IC790855.png "teknik ayarları")
   
    a. Tıklatın **karşıdan** ortak meta veri dosyası indirip bilgisayarınıza yerel olarak kaydedin.
   
    b. Meta veri dosyasını açın ve ardından bulun **AssertionConsumerService** düğümü.
    
    ![Onaylama işlemi tüketici hizmeti](./media/active-directory-saas-topdesk-secure-tutorial/IC790856.png "onaylama tüketici hizmeti")
   
    c. Kopya **AssertionConsumerService** değeri.  
      
    > [!NOTE]
    > Değer gerekir **uygulama URL'sini Yapılandır** Bu öğreticinin ilerleyen bölümlerinde.
    > 
    > 

6. Farklı web tarayıcısı penceresinde oturum açın, **Klasik Azure portalı** yönetici olarak.

7. Üzerinde **TOPdesk - güvenli** uygulama tümleştirme sayfası, tıklatın **çoklu oturum açma yapılandırma** açmak için ** tek oturum yapılandırma üzerinde Aktar ** iletişim.
   
    ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-topdesk-secure-tutorial/IC790602.png "çoklu oturum açmayı yapılandırın")

8. Üzerinde **için TOPdesk - güvenli oturum açmasını nasıl istiyorsunuz** sayfasında, **Microsoft Azure AD çoklu oturum açma**ve ardından **sonraki**.
   
    ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-topdesk-secure-tutorial/IC790603.png "çoklu oturum açmayı yapılandırın")

9. Üzerinde **uygulama URL'sini Yapılandır** sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Uygulama URL'sini Yapılandır](./media/active-directory-saas-topdesk-secure-tutorial/IC790604.png "uygulama URL'sini yapılandırın")
   
    a. İçinde **TOPdesk - URL üzerinde güvenli oturum** metin kutusuna, türü URL kullanılan kullanıcılarınız tarafından TOPdesk - güvenli uygulama imzalamak için (örn: "*https://qssolutions.topdesk.net*").
   
    b. İçinde **TOPdesk – ortak yanıt URL'si** metin kutusuna, Yapıştır **TOPdesk - güvenli AssertionConsumerService URL** (örn: "*https://qssolutions.topdesk.net/tas/public/login/saml*")
   
    c. **İleri**’ye tıklayın.

10. Üzerinde **çoklu oturum açma sırasında TOPdesk - güvenli yapılandırma** , meta veriler indirilemedi sayfasını tıklatın **karşıdan meta veri**ve ardından dosyayı bilgisayarınıza yerel olarak kaydedin.
    
    ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-topdesk-secure-tutorial/IC790605.png "çoklu oturum açmayı yapılandırın")

11. Bir sertifika dosyası oluşturmak için aşağıdaki adımları gerçekleştirin:
    
    ![Sertifika](./media/active-directory-saas-topdesk-secure-tutorial/IC790606.png "sertifika")
    
    a. İndirilen meta veri dosyasını açın.
    b. Genişletme **RoleDescriptor** sahip düğümü bir **xsi: type** , **ssas'nin: ApplicationServiceType**.
    c. Değerini kopyalayın **X509Certificate** düğümü.
    d. Kopyalanan Kaydet **X509Certificate** yerel olarak bilgisayarınızda bir dosyada değeri.

12. Şirket site, TOPdesk üzerinde - güvenli **TOPdesk** menüsünde tıklatın **ayarları**.
    
    ![Ayarları](./media/active-directory-saas-topdesk-secure-tutorial/IC790598.png "ayarları")

13. Tıklatın **oturum açma ayarları**.
    
    ![Oturum açma ayarları](./media/active-directory-saas-topdesk-secure-tutorial/IC790599.png "oturum açma ayarları")

14. Genişletme **oturum açma ayarları** menüsüne ve ardından **genel**.
    
    ![Genel](./media/active-directory-saas-topdesk-secure-tutorial/IC790600.png "genel")

15. İçinde **ortak** 'yi tıklatın **Ekle**.
    
    ![Ekleme](./media/active-directory-saas-topdesk-secure-tutorial/IC790607.png "ekleme")

16. Üzerinde **SAML Yapılandırması Yardımcısı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
    
    ![SAML Yapılandırması Yardımcısı](./media/active-directory-saas-topdesk-secure-tutorial/IC790608.png "SAML Yapılandırması Yardımcısı")
    
    a. İndirilen meta veri dosyanızı altında karşıya yüklemek için **Federasyon meta verileri**, tıklatın **Gözat**.

    b. Sertifika dosyanızın altında karşıya yüklemek için **sertifika (RSA)**, tıklatın **Gözat**.

    c. Aldığınız TOPdesk destek ekibinden altında logosu dosyayı karşıya yüklemeyi **Logo simgesini**, tıklatın **Gözat**.

    d. İçinde **kullanıcı adı özniteliği** metin kutusuna, türü **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.

    e. İçinde **görünen adı** metin kutusuna, yapılandırmanız için bir ad yazın.

    f. **Kaydet** düğmesine tıklayın.

17. Klasik Azure portalı, çoklu oturum açma yapılandırması onay seçin ve ardından **tam** kapatmak için **tek oturum yapılandırma üzerinde aktar** iletişim.
    
    ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-topdesk-secure-tutorial/IC790609.png "çoklu oturum açmayı yapılandırın")

## <a name="configuring-user-provisioning"></a>Kullanıcı hazırlama işleminin yapılandırılması
Azure AD kullanıcıların TOPdesk - oturum etkinleştirmek için güvenli, bunların TOPdesk - güvenli sağlanmalıdır.  
Söz konusu olduğunda TOPdesk - sağlama el ile bir görev güvenlidir.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları gerçekleştirin:
1. Oturum, **TOPdesk - güvenli** yönetici olarak şirket site.
2. Üstteki menüde tıklatın **TOPdesk \> yeni \> destek dosyalarını \> işleci**.
   
    ![İşleç](./media/active-directory-saas-topdesk-secure-tutorial/IC790610.png "işleci")

3. Üzerinde **New işleci** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
    ![New işleci](./media/active-directory-saas-topdesk-secure-tutorial/IC790611.png "New işleci")
   
    a. Genel sekmesini tıklatın.
   
    b. İçinde **Soyadı** , metin kutusuna **genel** bölümünde, son sağlamak istediğiniz geçerli bir Azure Active Directory hesabının adını yazın.
   
    c. Seçin bir **Site** hesap için **konumu** bölümü.
   
    d. İçinde **oturum açma adı** , metin kutusuna **TOPdesk oturum açma** bölümünde, kullanıcı oturum açma adını yazın.
   
    e. **Kaydet** düğmesine tıklayın.

> [!NOTE]
> Tüm diğer TOPdesk - güvenli kullanıcı hesabı oluşturma araçlarını veya tarafından TOPdesk - AAD kullanıcı hesaplarını sağlamak için güvenli sağlanan API'leri kullanabilirsiniz.
> 
> 

## <a name="assigning-users"></a>Kullanıcılar atama
Yapılandırmanızı test etmek için uygulama erişimi atayarak kullanarak izin vermek istediğiniz Azure AD kullanıcılarının vermeniz gerekir.

### <a name="to-assign-users-to-topdesk---secure-perform-the-following-steps"></a>Kullanıcılar için TOPdesk - atamak için güvenli, aşağıdaki adımları gerçekleştirin:
1. Klasik Azure portalında bir test hesabı oluşturun.
2. Üzerinde ** TOPdesk - güvenli ** uygulama tümleştirme sayfasını tıklatın **kullanıcı atama**.
   
    ![Kullanıcılar atama](./media/active-directory-saas-topdesk-secure-tutorial/IC790612.png "kullanıcı atama")

3. Test kullanıcınız seçin, **atamak**ve ardından **Evet** , atama onaylamak için.
   
    ![Evet](./media/active-directory-saas-topdesk-secure-tutorial/IC767830.png "Evet")

Çoklu oturum açma ayarlarınızı test etmek isterseniz, erişim paneli açın. Erişim paneli hakkında daha fazla ayrıntı için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md).

