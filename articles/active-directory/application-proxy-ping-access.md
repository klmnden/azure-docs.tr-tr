---
title: "Üstbilgi tabanlı kimlik doğrulaması ile PingAccess Azure AD uygulama proxy'si için | Microsoft Docs"
description: "Üstbilgi tabanlı kimlik doğrulamayı destekleyecek şekilde PingAccess ve uygulama proxy'si ile uygulama yayımlama."
services: active-directory
documentationcenter: 
author: kgremban
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/11/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 7c2e56a5f747aa2a37fc4bed0e3f3877b64f2be2
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="header-based-authentication-for-single-sign-on-with-application-proxy-and-pingaccess"></a>Uygulama proxy'si ve PingAccess ile çoklu oturum açma için üstbilgi tabanlı kimlik doğrulaması

Azure Active Directory Uygulama proxy'si ve PingAccess Azure Active Directory müşteriler daha fazla uygulama bile erişimle sağlamak üzere birlikte ortaklık. PingAccess genişletir [varolan uygulama proxy'si teklifleri](active-directory-application-proxy-get-started.md) üstbilgileri için kimlik doğrulaması kullanan uygulamalar için çoklu oturum açma erişimini eklenecek.

## <a name="what-is-pingaccess-for-azure-ad"></a>Azure AD için PingAccess nedir?

Azure Active Directory için PingAccess, üstbilgi için kimlik doğrulaması kullanan uygulamalar için kullanıcıların erişim ve çoklu oturum açma vermenizi sağlar PingAccess bir tekliftir. Uygulama proxy'si Bu uygulamaları diğer, erişimde kimlik doğrulaması için Azure AD kullanarak ve bağlayıcı hizmeti trafiğinin geçirme gibi davranır. PingAccess önünde uygulamaları bulunur ve uygulamanın kimlik doğrulaması okuyamamasını biçiminde alması Azure AD erişim belirtecinden bir üstbilgisine çevirir.

Kullanıcılarınızın, şirket uygulamalarınızı kullanmak oturum zaman farklı bir şey fark olmaz. Bunlar hala herhangi bir yerden herhangi bir cihazda çalışabilirsiniz. 

Uygulama proxy'si bağlayıcılar doğrudan kendi kimlik doğrulama türü ne olursa olsun tüm uygulamalar için Uzak trafiğe olduğundan, Yük Dengelemesi otomatik olarak da devam edeceğiz.

## <a name="how-do-i-get-access"></a>Erişim nasıl sağlarım?

Bu senaryo, Azure Active Directory PingAccess arasındaki iş ortaklığına aracılığıyla sunulur olduğundan, her iki hizmet için lisans gerekir. Ancak, Azure Active Directory Premium aboneliği en fazla 20 uygulamalarını kapsamaktadır temel bir PingAccess lisans içerir. 20'den fazla üstbilgi tabanlı uygulamaları yayımlamak gerekiyorsa, PingAccess ek lisans satın alabilirsiniz. 

Daha fazla bilgi için bkz. [Azure Active Directory sürümleri](active-directory-editions.md).

## <a name="publish-your-application-in-azure"></a>Azure'da uygulamanızı yayımlama

Bu makale, bir uygulamanın bu senaryo ile ilk kez yayımlama kişiler için tasarlanmıştır. Hem uygulama hem de PingAccess, yayımlama adımlara ek olarak çalışmaya nasıl başlayacağınız size yol göstermektedir. Her iki hizmet zaten yapılandırdıktan ancak yayımlama adımlar Yenileyici istiyorsanız Bağlayıcısı yükleme atlayın ve üzerinde gitme [Azure AD uygulama proxy'si ile uygulamanızı eklemek](#add-your-app-to-Azure-AD-with-Application-Proxy).

>[!NOTE]
>Bu senaryo Azure AD arasında ortaklık olduğundan ve PingAccess, bazı yönergeler var Ping kimlik sitesinde.

### <a name="install-an-application-proxy-connector"></a>Bir uygulama ara sunucusu Bağlayıcısı'nı yüklemek

Zaten uygulama Proxy etkin olması ve yüklü bir bağlayıcı varsa, bu bölüm atlayın ve üzerinde gitme [Azure AD uygulama proxy'si ile uygulamanızı eklemek](#add-your-app-to-azure-ad-with-application-proxy).

Uygulama Ara sunucusu Bağlayıcısı'nı uzaktan çalışanlarınızın trafiği yayımlanan uygulamalarınızı yönlendiren bir Windows Server hizmetidir. Daha ayrıntılı yükleme yönergeleri için bkz: [Azure portalında uygulama ara sunucusunu etkinleştirme](active-directory-application-proxy-enable.md).

1. Oturum [Azure portal](https://portal.azure.com) genel yönetici olarak.
2. Seçin **Azure Active Directory** > **uygulama proxy'si**.
3. Seçin **Bağlayıcısı'nı indir** uygulama Proxy Bağlayıcısı yüklemeyi başlatmak için. Yükleme yönergelerini izleyin.

   ![Uygulama Ara sunucusunu etkinleştirme ve Bağlayıcısı'nı indirin](./media/application-proxy-ping-access/install-connector.png)

4. Bağlayıcı yükleme otomatik olarak etkinleştirmelisiniz uygulama proxy'si değil, seçebilirsiniz, ancak dizininiz için **uygulama ara sunucusunu etkinleştirme**.


### <a name="add-your-app-to-azure-ad-with-application-proxy"></a>Azure AD uygulama proxy'si ile uygulamanızı ekleyin

Azure portalında uygulamanız gereken iki eylemler vardır. İlk olarak, uygulama proxy'si ile uygulamanızı yayımlamak için gerekir. Ardından, PingAccess adımları sırasında kullanabilirsiniz uygulama hakkında biraz bilgi toplamanız gerekir.

Uygulamanızı yayımlamak için aşağıdaki adımları izleyin. 1-8, bkz: izlenecek adımların daha ayrıntılı için [Azure AD uygulama proxy'si ile uygulama yayımlama](application-proxy-publish-azure-portal.md).

1. Son bölümde almadıysanız, oturum [Azure portal](https://portal.azure.com) genel yönetici olarak.
2. Seçin **Azure Active Directory** > **kurumsal uygulamalar**.
3. Seçin **Ekle** dikey pencerenin üstündeki.
4. Seçin **şirket içi uygulama**.
5. Yeni uygulamanızı hakkındaki bilgilerle gerekli alanları doldurun. Ayarları için aşağıdaki yönergeleri kullanın:
   - **İç URL**: normalde şirket ağında olduğunuzda, uygulamanın oturum açma sayfası götüren URL'sini sağlayın. Bu senaryo için bağlayıcı PingAccess proxy uygulama ön sayfası olarak işlemek gerekir. Şu biçimi kullanın: `https://<host name of your PA server>:<port>`. Bağlantı noktası, varsayılan değer 3000 olmakla birlikte PingAccess yapılandırabilirsiniz.
   - **Ön kimlik doğrulama yöntemi**: Azure Active Directory
   - **Üstbilgiler URL'de çevir**: Hayır

   >[!NOTE]
   >Bu ilk uygulamanızı ise, bağlantı noktası 3000 başlatmak ve PingAccess yapılandırmanızı değiştirirseniz, bu ayar güncelleştirmek için geri dönün için kullanın. İkinci veya sonraki uygulamanız varsa bu PingAccess içinde yapılandırdığınız dinleyicisi eşleşmesi gerekir. Daha fazla bilgi edinmek [PingAccess dinleyicileri](https://documentation.pingidentity.com/pingaccess/pa31/index.shtml#Listeners.html).

6. Seçin **Ekle** dikey pencerenin altındaki. Uygulamanızı eklenir ve Hızlı Başlat menüsü açılır.
7. Hızlı Başlangıç menüsünde seçin **test etmek için bir kullanıcı atamak**, ve uygulama için en az bir kullanıcı ekleyebilirsiniz. Bu test hesabının şirket içi uygulama erişimi olduğundan emin olun.
8. Seçin **atamak** test kullanıcı ataması kaydetmek için.
9. Uygulama Yönetimi dikey penceresinde, seçin **çoklu oturum açma**.
10. Seçin **üstbilgi tabanlı oturum açma** açılır menüsünden. **Kaydet**’i seçin.

   >[!TIP]
   >Üstbilgi tabanlı çoklu oturum açma kullanarak, ilk kez kullanıyorsanız PingAccess yüklemeniz gerekir. Azure aboneliğiniz PingAccess yüklemenizle birlikte otomatik olarak ilişkili olduğundan emin olmak için PingAccess indirmek için bu tek oturum açma sayfasında bağlantıyı kullanın. Şimdi indirme sitesi açmak veya daha sonra bu sayfaya geri dönerseniz. 

   ![Üstbilgi tabanlı oturum açma seçin](./media/application-proxy-ping-access/sso-header.PNG)

11. Kurumsal uygulamalar dikey veya Azure Active Directory menüye dönmek için kaydırıcıyı sola kaydırma kapatın.
12. Seçin **uygulama kayıtlar**.

   ![Uygulama kayıtlar seçin](./media/application-proxy-ping-access/app-registrations.png)

13. Yeni eklediğiniz, ardından uygulamayı seçin **yanıt URL'leri**.

   ![Yanıt URL'leri seçin](./media/application-proxy-ping-access/reply-urls.png)

14. Dış yanıt URL'leri listesindeki uygulamanızı 5. adımda atadığınız URL olup olmadığını denetleyin. Değilse, şimdi ekleyin.
15. Uygulama ayarları dikey penceresinde, seçin **gerekli izinleri**.

  ![Gerekli izinleri seçin](./media/application-proxy-ping-access/required-permissions.png)

16. **Add (Ekle)** seçeneğini belirleyin. API için seçin **Windows Azure Active Directory**, ardından **seçin**. İzinlerini seçin **okuma ve tüm uygulamaları yazma** ve **oturum açın ve kullanıcı profilini okuma**, ardından **seçin** ve **Bitti**.  

  ![İzinleri seçin](./media/application-proxy-ping-access/select-permissions.png)

17. İzinleri Ekranı kapatmadan önce izinleri verin. 
![İzinleri](media/application-proxy-ping-access/grantperms.png)

### <a name="collect-information-for-the-pingaccess-steps"></a>PingAccess adımlar için bilgi toplama

1. Uygulama ayarları dikey üzerinde seçin **özellikleri**. 

  ![Özellikler'i seçin](./media/application-proxy-ping-access/properties.png)

2. Kaydet **uygulama kimliği** değeri. PingAccess yapılandırdığınızda, bu istemci kimliği için kullanılır.
3. Uygulama ayarları dikey penceresinde, seçin **anahtarları**.

  ![Anahtarları seçin](./media/application-proxy-ping-access/Keys.png)

4. Bir anahtar açıklama girerek ve sona erme tarihi açılan menüden seçerek bir anahtar oluşturun.
5. **Kaydet**’i seçin. Bir GUID görünür **değeri** alan.

  Bu değer, artık bu pencereyi kapattıktan sonra yeniden görmek açamazsınız olarak kaydedin.

  ![Yeni anahtar oluştur](./media/application-proxy-ping-access/create-keys.png)

6. Uygulama kayıtlar dikey veya Azure Active Directory menüye dönmek için kaydırıcıyı sola kaydırma kapatın.
7. Seçin **özellikleri**.
8. Kaydet **dizin kimliği** GUID.

### <a name="optional---update-graphapi-to-send-custom-fields"></a>İsteğe bağlı - güncelleştirme GraphAPI özel alanlar göndermek için

Azure AD kimlik doğrulaması için gönderir güvenlik belirteçleri listesi için bkz: [Azure AD belirteç başvurusu](./develop/active-directory-token-and-claims.md). Diğer belirteçleri gönderir bir özel talep gerekiyorsa, GraphAPI uygulaması alanı ayarlamak için kullanın *acceptMappedClaims* için **doğru**. Azure AD Graph Explorer'a yalnızca bu yapılandırma yapmak için de kullanabilirsiniz. 

Bu örnek Graph Explorer'a kullanır:

```
PATCH https://graph.windows.net/myorganization/applications/<object_id_GUID_of_your_application> 

{
  "acceptMappedClaims":true
}
```

>[!NOTE]
>Bir özel talep kullanmak için de tanımlanır ve uygulamaya atanan özel bir ilke olması gerekir.  Bu ilke tüm gerekli özel öznitelikleri içermelidir.
>
>İlke tanımı ve atama PowerShell, Azure AD Graph Explorer'a veya MS Graph yapılabilir.  PowerShell'de bu yapıyorsanız, önce kullanmanız gerekebilir `New-AzureADPolicy `ve uygulama ile atamak `Set-AzureADServicePrincipalPolicy`.  Daha fazla bilgi için bkz: [Azure AD İlkesi belgeleri](active-directory-claims-mapping.md#claims-mapping-policy-assignment).

### <a name="optional---use-a-custom-claim"></a>İsteğe bağlı - özel talep kullanın
Uygulamanızın bir özel talep kullanın ve ek alanları dahil olmak üzere de sahip olduğundan emin olun [ilke eşleme bir özel talep oluşturulur ve uygulamaya atanan](active-directory-claims-mapping.md#claims-mapping-policy-assignment).

## <a name="download-pingaccess-and-configure-your-app"></a>PingAccess indirin ve uygulamanızı yapılandırma

Tüm Azure Active Directory kurulum adımlarını tamamladığınıza göre size PingAccess yapılandırmaya geçebilirsiniz. 

Ayrıntılı adımlar için bu senaryonun PingAccess parçası Ping kimlik belgelerinde devam [yapılandırma PingAccess Azure AD için](https://docs.pingidentity.com/bundle/paaad_m_ConfigurePAforMSAzureADSolution_paaad43/page/pa_c_PAAzureSolutionOverview.html).

Bu adımları, zaten yoksa, PingAccess hesabı alma, PingAccess sunucusunu yükleme ve Azure portalından kopyaladığınız dizini Kimliğine sahip bir Azure AD OIDC sağlayıcı bağlantısı oluşturma işleminde size yol. Ardından, PingAccess üzerinde Web oturum oluşturmak için uygulama kimliği ve anahtar değerlerini kullanın. Bundan sonra kimlik eşlemesi ayarlamanız ve bir sanal ana bilgisayar, site ve uygulama oluşturun.

### <a name="test-your-app"></a>Uygulamanızı test etme

Tüm bu adımları tamamladıktan sonra uygulamanızı açık ve çalışıyor olması. Test etmek için bir tarayıcı açın ve uygulama Azure'da yayımlandığında oluşturduğunuz dış URL'ye gidin. Uygulamaya atanan test hesabınızla oturum açın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD için PingAccess yapılandırın](https://docs.pingidentity.com/bundle/paaad_m_ConfigurePAforMSAzureADSolution_paaad43/page/pa_c_PAAzureSolutionOverview.html)
- [Azure AD uygulama proxy'si nasıl çoklu oturum açma sağlar?](application-proxy-sso-overview.md)
- [Uygulama proxy'si sorun giderme](active-directory-application-proxy-troubleshoot.md)
