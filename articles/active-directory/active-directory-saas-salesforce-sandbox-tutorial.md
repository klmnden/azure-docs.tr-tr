---
title: "Öğretici: Salesforce korumalı alan Azure Active Directory Tümleştirme | Microsoft Docs"
description: "Salesforce korumalı alan Azure Active Directory ile çoklu oturum açma, otomatik sağlama ve daha fazlasını sağlamak için nasıl kullanılacağını öğrenin!."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: ee54c39e-ce20-42a4-8531-da7b5f40f57c
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/21/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: 32835e79188806bb2ff319eea23b1b52ab585ab1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce-sandbox"></a>Öğretici: Azure Active Directory Tümleştirme ile Salesforce korumalı alan

Bu öğreticinin amacı, Azure ve Salesforce korumalı alan tümleştirmesini göstermektir.  

>[!TIP]
>Geri bildirim için bkz: [Azure destek sayfası](http://go.microsoft.com/fwlink/?LinkId=521878). 
> 

Korumalı alanlar çeşitli geliştirme, test ve eğitim, Salesforce üretim kuruluşunuzdaki uygulamaları ve verileri ödün vermeden gibi amaçlar için ayrı ortamlarda, kuruluşunuzun birden çok kopya oluşturma olanağı verir.  

Daha fazla ayrıntı için bkz: [korumalı alan genel bakış](https://help.salesforce.com/HTViewHelpDoc?id=create_test_instance.htm&language=en_US)

Bu öğreticide gösterilen senaryo, aşağıdaki öğeleri zaten sahip olduğunuzu varsayar:

* Geçerli bir Azure aboneliği
* Bir korumalı alan Salesforce.com'da

Geçerli bir korumalı alan Salesforce.com'da henüz yoksa, Salesforce başvurmanız gerekir.

Bu öğreticide gösterilen senaryo, aşağıdaki yapı taşlarını oluşur:

1. Salesforce korumalı alan için uygulama tümleştirmesi etkinleştirme
2. Çoklu oturum açma (SSO) yapılandırma
3. Etki alanınızı etkinleştirme
4. Kullanıcı hazırlama işleminin yapılandırılması
5. Kullanıcılar atama

![Senaryo](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769571.png "senaryosu")

## <a name="enable-the-application-integration-for-salesforce-sandbox"></a>Salesforce korumalı alan için uygulama tümleştirmeyi etkinleştir
Bu bölümün amacı Salesforce korumalı alan için uygulama tümleştirme sağlamak üzere nasıl anahat sağlamaktır.

**Salesforce korumalı alan için uygulama tümleştirmesini etkinleştirmek için aşağıdaki adımları gerçekleştirin:**

1. Azure Klasik portalında, sol gezinti bölmesinde tıklatın **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-salesforce-sandbox-tutorial/IC700993.png "Active Directory")
2. Gelen **Directory** listesinde, directory tümleştirmesini etkinleştirmek istediğiniz dizini seçin.
3. Dizin görünümünde uygulamaları görünümü açmak için **uygulamaları** üst menüde.
   
   ![Uygulamaları](./media/active-directory-saas-salesforce-sandbox-tutorial/IC700994.png "uygulamalar")
4. Açmak için **uygulama Galerisi**, tıklatın **uygulama Ekle**ve ardından **kullanılacak Kuruluşum için uygulama ekleme**.
   
   ![Ne yapmak istiyorsunuz? ] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC700995.png "Ne yapmak istiyorsunuz?")
5. İçinde **arama kutusu**, türü **Salesforce korumalı alan**.
   
   ![Uygulama Galerisi](./media/active-directory-saas-salesforce-sandbox-tutorial/IC710978.png "uygulama Galerisi")
6. Sonuçlar bölmesinde seçin **Salesforce korumalı alan**ve ardından **tam** uygulama eklemek için.
   
   ![Salesforce korumalı alan](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746474.png "Salesforce korumalı alan")
   
## <a name="configur-single-sign-on-sso"></a>Configur çoklu oturum açma (SSO)

Bu bölümün amacı kullanıcıların Salesforce kendi hesabıyla SAML protokolünü temel Federasyon kullanarak Azure AD kimlik doğrulaması sağlamak nasıl anahat sağlamaktır.

**Çoklu oturum açma yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure Klasik portalında üzerinde **Salesforce korumalı alan** uygulama tümleştirmesi sayfasında, tıklatın **çoklu oturum açma özelliğini yapılandırın** açmak için **tek oturum yapılandırma üzerinde aktar** iletişim.
   
   ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-salesforce-sandbox-tutorial/IC749323.png "çoklu oturum açmayı yapılandırın")
2. Üzerinde **Salesforce korumalı alan oturum açmasını nasıl istiyorsunuz** sayfasında, **Microsoft Azure AD çoklu oturum açma**ve ardından **sonraki**.
   
   ![Salesforce korumalı alan](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746479.png "Salesforce korumalı alan")
3. Üzerinde **uygulama URL'sini Yapılandır** sayfasında **oturum üzerinde URL'si** metin kutusuna, şu biçimi kullanarak URL'nizi yazın `http://company.my.salesforce.com`ve ardından **sonraki**.
   
   ![Uygulama URL'sini Yapılandır](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781022.png "uygulama URL'sini yapılandırın")
4. Zaten başka bir Salesforce korumalı alan örneği için çoklu oturum açmayı dizininizde yapılandırdıktan sonra yapılandırmanız da gerekir **tanımlayıcısı** aynı değere sahip **URL üzerinde oturum**. 
 * **Tanımlayıcısı** alan denetleyerek bulunabilir **Göster Gelişmiş ayarları** onay kutusuna bağlı **uygulama URL'sini Yapılandır** iletişim kutusunun sayfa.
5. Üzerinde **çoklu oturum açma sırasında Salesforce korumalı alan yapılandırmak** sayfasında, **indirme sertifika**ve ardından sertifika dosyayı bilgisayarınıza kaydedin.
   
   ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781023.png "çoklu oturum açmayı yapılandırın")
6. Farklı web tarayıcısı penceresinde, Salesforce korumalı alan yönetici olarak oturum açın.
7. Üstteki menüde tıklatın **Kurulum**.
   
   ![Kurulum](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781024.png "Kurulumu")
8. Sol gezinti bölmesinde tıklayın **güvenlik denetimleri**ve ardından **çoklu oturum açma ayarları**.
   
   ![Çoklu oturum açma ayarları](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781025.png "tek oturum açma ayarları")
9. Çoklu oturum açma ayarları bölümüne aşağıdaki adımları gerçekleştirin:
   
   ![Çoklu oturum açma ayarları](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781026.png "tek oturum açma ayarları")  
 1.  Seçin **SAML etkin**. 
 2.  **Yeni**’ye tıklayın.
10. SAML çoklu oturum açma ayarları bölümüne aşağıdaki adımları gerçekleştirin:
    
    ![SAML çoklu oturum açma ayarları](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781027.png "SAML çoklu oturum açma ayarları")  
 1. Adı metin kutusuna yapılandırma adını yazın (örneğin: *SPSSOWAAD\_Test*). 
 2. Azure Klasik portalında üzerinde **çoklu oturum açma sırasında Salesforce korumalı alan yapılandırmak** iletişim kutusu sayfası, kopya **veren URL'si** değer ve ardından yapıştırın **veren** metin kutusu.
 3. İçinde **varlık kimliği** metin kutusuna, türü **https://test.salesforce.com** bu dizininize eklediğiniz ilk Salesforce korumalı alan örnek ise. Salesforce korumalı alan örneği sonra için eklediyseniz **varlık kimliği** yazın **oturum üzerinde URL'si**, şu biçimde olmalıdır:`http://company.my.salesforce.com`   
 4. Tıklatın **Gözat** indirilen sertifikayı karşıya yüklemek için.  
 5. Olarak **SAML kimlik türü**seçin **onaylamayı içeren kullanıcı nesnesinden Federasyon kimliği**. 
 6. Olarak **SAML kimlik konumu**seçin **kimliktir konu deyimi NameIdentifier öğesinde**.
 7. Azure Klasik portalında üzerinde **çoklu oturum açma sırasında Salesforce korumalı alan yapılandırmak** iletişim kutusu sayfası, kopya **uzaktan oturum açma URL'si** değer ve ardından yapıştırın **kimlik sağlayıcısı oturum açma URL'si** metin kutusu. 
 8. SAML oturum kapatma SFDC desteklemez.  'Https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0' geçici bir çözüm olarak Yapıştır içine **kimlik sağlayıcısı oturum kapatma URL'si** metin kutusu.
 9. Olarak **hizmet sağlayıcısı tarafından başlatılan bağlama isteği**seçin **HTTP POST**. 
 10. **Kaydet** düğmesine tıklayın.
11. Klasik Azure portalı, çoklu oturum açma yapılandırması onay seçin ve ardından **tam** kapatmak için **tek oturum yapılandırma üzerinde aktar** iletişim.
    
    ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781028.png "çoklu oturum açmayı yapılandırın")

## <a name="enable-your-domain"></a>Etki alanınızı etkinleştir
Bu bölümde, bir etki alanı zaten oluşturduğunuzu varsayar.  Daha fazla ayrıntı için bkz: [etki alanı adınız tanımlama](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US).

**Etki alanınızı etkinleştirmek için aşağıdaki adımları gerçekleştirin:**

1. Sol gezinti bölmesinde **etki alanı yönetimi**ve ardından **My etki alanı.**
   
   ![Etki alanım](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781029.png "etki alanım")
   
   >[!NOTE]
   >Lütfen etki alanınızı doğru yapılandırılmış olduğundan emin olun. 
   > 
2. İçinde **oturum açma sayfası ayarları** 'yi tıklatın **Düzenle**, daha sonra olarak **kimlik doğrulama hizmeti**, önceki bölümden oturum açma SAML tek ayar adını seçin ve son olarak tıklatın **kaydetmek**.
   
   ![Etki alanım](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781030.png "etki alanım")

Yapılandırılmış bir etki alanınız hemen kullanıcılarınızın Salesforce korumalı alan oturum açma etki alanı URL'si kullanmanız gerekir.  

URL değerini almak için önceki bölümde oluşturduğunuz SSO profili tıklatın.

## <a name="configure-user-provisioning"></a>Kullanıcı sağlamayı Yapılandır
Bu bölümün amacı Salesforce korumalı alan Active Directory kullanıcı hesaplarının kullanıcı sağlamayı etkinleştirme anahat sağlamaktır.

**Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Salesforce portalında, üst gezinti çubuğunda kullanıcı menünüze genişletmek için adınızı seçin:
   
   ![Ayarlarımı](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698773.png "ayarlarım")
2. Kullanıcı menüsünden seçin **My ayarları** açmak için **My ayarları** sayfası.
3. Sol bölmede **kişisel** kişisel bölümünü genişletin ve ardından **sıfırlama My güvenlik belirteci**:
   
   ![Ayarlarımı](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698774.png "ayarlarım")
4. Üzerinde **sıfırlama My güvenlik belirteci** sayfasında, **güvenlik belirteci sıfırlama** Salesforce.com güvenlik belirteci içeren bir e-posta istemek için.
   
   ![Yeni bir belirteç](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698776.png "yeni belirteci")
5. Bir e-posta ile Salesforce.com için e-posta kutunuzu kontrol edin "**salesforce.com.com güvenlik onaylama**" konu olarak.
6. Bu e-posta gözden geçirin ve güvenlik belirteci değerini kopyalayın.
7. Azure Klasik portalında üzerinde **salesforce korumalı alan** uygulama tümleştirmesi sayfasında, tıklatın **kullanıcı sağlamayı Yapılandır** açmak için **kullanıcı sağlamayı yapılandırın** iletişim.
   
   ![Kullanıcı sağlamayı Yapılandır](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769573.png "kullanıcı sağlamayı Yapılandır")
8. Üzerinde **otomatik kullanıcı sağlamayı etkinleştirmek için Salesforce korumalı alan kimlik bilgilerinizi girin** sayfasında, aşağıdaki yapılandırma ayarlarını sağlayın:
   
   ![Salesforce korumalı alan](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746476.png "Salesforce korumalı alan")   
 1. İçinde **Salesforce korumalı alan yönetici kullanıcı adı** metin kutusuna, Salesforce korumalı alan adı hesap türü **Sistem Yöneticisi** atanan Salesforce.com profilinde.
 2. İçinde **Salesforce korumalı alan yönetici parolası** metin kutusuna, bu hesabın parolasını yazın.
 3. İçinde **kullanıcı güvenlik belirteci** metin kutusuna, güvenlik belirteci değeri yapıştırın.
 4. Tıklatın **doğrulama** yapılandırmanızı doğrulayın.
 5. Tıklatın **sonraki** açmak için düğmeye **onay** sayfası.
9. Üzerinde **onay** sayfasında, **tam** yapılandırmanızı kaydetmek için.
   
## <a name="assigning-users"></a>Kullanıcılar atama

Yapılandırmanızı test etmek için uygulama erişimi atayarak kullanarak izin vermek istediğiniz Azure AD kullanıcılarının vermeniz gerekir.

**Salesforce korumalı alan kullanıcılara atamak için aşağıdaki adımları gerçekleştirin:**

1. Klasik Azure portalında bir test hesabı oluşturun.
2. Üzerinde ** Salesforce korumalı alan ** uygulama tümleştirme sayfasını tıklatın **kullanıcı atama**.
   
   ![Kullanıcılar atama](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769574.png "kullanıcı atama")
3. Test kullanıcınız seçin, **atamak**ve ardından **Evet** , atama onaylamak için.
   
   ![Evet](./media/active-directory-saas-salesforce-sandbox-tutorial/IC767830.png "Evet")

Şimdi, 10 dakika bekleyin ve hesap Salesforce korumalı alan eşitlendiğinden doğrulamanız gerekir.

SSO ayarlarınızı test etmek isterseniz, erişim paneli açın. Erişim paneli hakkında daha fazla ayrıntı için bkz: [erişim Paneli'ne giriş](https://msdn.microsoft.com/library/dn308586).

