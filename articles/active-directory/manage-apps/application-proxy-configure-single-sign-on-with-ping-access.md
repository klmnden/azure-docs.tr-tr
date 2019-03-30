---
title: Üst bilgi tabanlı kimlik doğrulaması ile PingAccess için Azure AD uygulama proxy'si | Microsoft Docs
description: Üst bilgi tabanlı kimlik doğrulamasını desteklemek için PingAccess ve uygulama ara sunucusu ile uygulama yayımlayın.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/11/2017
ms.author: celested
ms.reviewer: harshja
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 319791c2436395c00dafc744fb6fcb1ff18b0750
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58652340"
---
# <a name="header-based-authentication-for-single-sign-on-with-application-proxy-and-pingaccess"></a>Üst bilgi tabanlı kimlik doğrulaması için uygulama proxy'si ile PingAccess ile çoklu oturum açma

Azure Active Directory Uygulama proxy'si ve PingAccess bile daha fazla uygulama erişimi ile Azure Active Directory müşterilerine sağlamak için birlikte kurdu. PingAccess genişletir [mevcut uygulama proxy'si teklifleri](application-proxy.md) üst bilgileri için kimlik doğrulaması kullanan uygulamalar için çoklu oturum açma erişimi eklenecek.

## <a name="what-is-pingaccess-for-azure-ad"></a>Azure AD için PingAccess nedir?

Azure Active Directory için PingAccess kullanıcılara erişim ve çoklu oturum açma kimlik doğrulaması için üst bilgi kullanan uygulamalar için vermenizi sağlayan pingaccess'in bir tekliftir. Uygulama Ara sunucusu, bu uygulamaları diğer, Azure AD kimlik doğrulaması yapmak için kullanarak ve ardından bağlayıcı hizmetini üzerinden trafiği geçirerek gibi davranır. PingAccess uygulamaları önünde yer alan ve uygulama kimlik doğrulaması okuyabilmesi biçiminde alır. böylece, Azure AD'den erişim belirteci bir üstbilgisine çevirir.

Kurumsal uygulamalarınızı kullanmak oturum açtığında, kullanıcılar farklı bir şey fark olmaz. Bunlar yine de her yerde her cihazda çalışabilirsiniz. 

Uygulama Ara sunucusu bağlayıcıları doğrudan kendi kimlik doğrulama türü ne olursa olsun tüm uygulamalara uzaktan trafiği olduğundan, yük dengelemek otomatik olarak da devam edeceğiz.

## <a name="how-do-i-get-access"></a>Erişimi nasıl alabilirim?

Bu senaryo, Azure Active Directory ve PingAccess arasındaki iş ortaklığı ile sunulan olduğundan, her iki hizmet için Lisansınızın olması gerekir. Ancak, en fazla 20 uygulamaları kapsayan temel bir PingAccess lisansı Azure Active Directory Premium abonelikleri içerir. 20'den fazla üst bilgi tabanlı uygulamaları yayımlamak gerekiyorsa, PingAccess ek lisans satın alabilirsiniz. 

Daha fazla bilgi için bkz. [Azure Active Directory sürümleri](../fundamentals/active-directory-whatis.md).

## <a name="publish-your-application-in-azure"></a>Uygulamanızı Azure'da yayımlayın

Bu makalede, bu senaryo ile bir uygulama ilk kez yayımlıyorsunuz kişilere yöneliktir. Bu yayımlama adımları yanı sıra hem uygulama hem de PingAccess, ile çalışmaya başlama konusunda yol göstermektedir. Her iki hizmet zaten yapılandırdınız ancak yayımlama adımları tazelemek istiyorsanız, bağlayıcı yükleme atlayın ve geçin [uygulamanıza Azure ad uygulama ara sunucusu ile ekler](#add-your-app-to-azure-ad-with-application-proxy).

>[!NOTE]
>Bu senaryo Azure AD arasındaki iş ortaklığı olduğundan ve PingAccess, alan yönergelerden bazılarını var Ping Identity sitesinde.

### <a name="install-an-application-proxy-connector"></a>Uygulama Ara sunucusu bağlayıcısını yükleme

Zaten uygulama Proxy etkin olması ve yüklü bir bağlayıcı varsa, bu bölümü atlayın ve geçin [uygulamanıza Azure ad uygulama ara sunucusu ile ekler](#add-your-app-to-azure-ad-with-application-proxy).

Uygulama Ara sunucusu Bağlayıcısı'nı, yayımlanmış uygulamaları uzak çalışanlarınız gelen trafiği yönlendiren bir Windows Server hizmetidir. Daha ayrıntılı yükleme yönergeleri için bkz. [Azure portalında uygulama ara sunucusunu etkinleştirme](application-proxy-add-on-premises-application.md).

1. [Azure portalında](https://portal.azure.com) genel yönetici olarak oturum açın.
2. Seçin **Azure Active Directory** > **uygulama proxy'si**.
3. Seçin **Bağlayıcısı'nı indir** uygulama ara sunucusu Bağlayıcısı indirme başlatılamadı. Yükleme yönergelerini izleyin.

   ![Uygulama Ara sunucusunu etkinleştirme ve bağlayıcıyı indirin](./media/application-proxy-configure-single-sign-on-with-ping-access/install-connector.png)

4. Bağlayıcıyı indirdiğinizde otomatik olarak etkinleştirmelisiniz uygulama proxy'si değil, seçebilirsiniz, ancak dizininizin **uygulama ara sunucusunu etkinleştirme**.


### <a name="add-your-app-to-azure-ad-with-application-proxy"></a>Azure ad uygulama ara sunucusu ile uygulamanıza ekleyin

Azure portalında atmanız gereken iki eylemler vardır. İlk olarak, uygulama ara sunucusu ile uygulamanızı yayımlamak gerekir. Ardından, PingAccess adımları sırasında kullanabileceğiniz bu uygulama hakkında bazı bilgiler toplamak gerekir.

Uygulamanızı yayımlamak için aşağıdaki adımları izleyin. 1-8, bkz: adımlarının daha ayrıntılı için [Azure AD uygulama ara sunucusu kullanarak uygulama yayımlama](application-proxy-add-on-premises-application.md).

1. Son bölümde Aksi takdirde oturum [Azure portalında](https://portal.azure.com) genel Yöneticisi olarak.
2. Seçin **Azure Active Directory** > **kurumsal uygulamalar**.
3. Seçin **Ekle** dikey penceresinin üstünde.
4. Seçin **şirket içi uygulama**.
5. Yeni uygulamanız hakkındaki bilgilerle gerekli alanları doldurun. Ayarları için aşağıdaki yönergeleri kullanın:
   - **İç URL**: Normalde şirket ağında olduğunuzda, sizi uygulamanın oturum açma sayfasına götürür URL'sini sağlayın. Bu senaryo için bağlayıcı PingAccess proxy uygulamanın ön sayfa olarak ele almanız gerekir. Bu biçimi kullanın: `https://<host name of your PA server>:<port>`. Varsayılan olarak 3000 bağlantı noktasıdır, ancak PingAccess yapılandırabilirsiniz.

     > [!WARNING]
     > SSO bu tür, iç URL, https kullanmalıdır ve http kullanamazsınız.

   - **Ön kimlik doğrulama yöntemi**: Azure Active Directory
   - **Bilgilerde URL'yi çevir**: Hayır

   >[!NOTE]
   >Bu ilk uygulamanızı ise, bağlantı noktası 3000 başlatıp PingAccess yapılandırmanızı değiştirirseniz bu ayarını güncelleştirmek için geri dönen kullanın. İkinci veya sonraki uygulamanız varsa bu PingAccess yapılandırdığınız dinleyici eşleşmesi gerekir. Daha fazla bilgi edinin [dinleyicileri PingAccess içinde](https://documentation.pingidentity.com/pingaccess/pa31/index.shtml#Listeners.html).

6. Seçin **Ekle** dikey pencerenin alt kısmındaki. Uygulamanızı eklenir ve Hızlı Başlangıç menüsü açılır.
7. Hızlı Başlangıç menüde **test etmek için kullanıcı atama**, ve en az bir kullanıcı uygulamaya ekleyin. Bu test hesabı şirket içi uygulamaya erişimi olduğundan emin olun.
8. Seçin **atama** test kullanıcı atama kaydetmek için.
9. Uygulama Yönetimi dikey penceresinde seçin **çoklu oturum açma**.
10. Seçin **üst bilgi tabanlı oturum açma** aşağı açılan menüden. **Kaydet**’i seçin.

    >[!TIP]
    >Bu üst bilgi tabanlı çoklu oturum açma kullanarak ilk kez ise, PingAccess yüklemeniz gerekir. Azure aboneliğinizi PingAccess yüklemenizle birlikte otomatik olarak ilişkilendirilir emin olmak için PingAccess indirmek için bu tek oturum açma sayfasında bağlantıyı kullanın. Şimdi indirme sitesi açmak veya bu sayfada daha sonra tekrar deneyin. 

    ![Üst bilgi tabanlı oturum açma'yı seçin](./media/application-proxy-configure-single-sign-on-with-ping-access/sso-header.PNG)

11. Tüm Azure Active Directory menüye dönmek için sol kaydırma ve kurumsal uygulamalar dikey penceresini kapatın.
12. **Uygulama kayıtları**'nı seçin.

    ![Uygulama kayıtlarını seçme](./media/application-proxy-configure-single-sign-on-with-ping-access/app-registrations.png)

13. Yeni eklediğiniz, ardından uygulamayı seçin **yanıt URL'leri**.

    ![Yanıt URL'si seçin](./media/application-proxy-configure-single-sign-on-with-ping-access/reply-urls.png)

14. Dış yanıt URL'leri listesinde uygulamanızı 5. adımda atadığınız URL olup olmadığını denetleyin. Yüklü değilse, şimdi ekleyin.
15. Uygulama ayarları dikey penceresinde seçin **gerekli izinler**.

    ![Gerekli izinleri seçme](./media/application-proxy-configure-single-sign-on-with-ping-access/required-permissions.png)

16. **Add (Ekle)** seçeneğini belirleyin. API için seçin **Windows Azure Active Directory**, ardından **seçin**. İzinlerini seçin **okuma ve tüm uygulamalar yazma** ve **oturum açın ve kullanıcı profilini okuma**, ardından **seçin** ve **Bitti**.  

    ![İzinleri seçin](./media/application-proxy-configure-single-sign-on-with-ping-access/select-permissions.png)

17. İzinleri Ekranı kapatmadan önce izinleri verin. 
    ![İzin ver](./media/application-proxy-configure-single-sign-on-with-ping-access/grantperms.png)

### <a name="collect-information-for-the-pingaccess-steps"></a>PingAccess adımlar için bilgi toplama

1. Uygulama ayarları dikey penceresinde seçin **özellikleri**. 

   ![Özellikleri seçin](./media/application-proxy-configure-single-sign-on-with-ping-access/properties.png)

2. Kaydet **uygulama kimliği** değeri. PingAccess yapılandırdığınızda bu istemci kimliği için kullanılır.
3. Uygulama ayarları dikey penceresinde seçin **anahtarları**.

   ![Anahtarları seçin](./media/application-proxy-configure-single-sign-on-with-ping-access/Keys.png)

4. Anahtar açıklaması girme ve açılan menüden bir sona erme tarihi seçerek bir anahtar oluşturun.
5. **Kaydet**’i seçin. Bir GUID görünür **değer** alan.

   Bu değer, şimdi bu pencereyi kapattıktan sonra yeniden görmeniz mümkün olmayacaktır olarak kaydedin.

   ![Yeni anahtar oluştur](./media/application-proxy-configure-single-sign-on-with-ping-access/create-keys.png)

6. Tüm Azure Active Directory menüye dönmek için sol kaydırma ve uygulama kayıtları dikey penceresini kapatın.
7. Seçin **özellikleri**.
8. Kaydet **dizin kimliği** GUID.

### <a name="optional---update-graphapi-to-send-custom-fields"></a>İsteğe bağlı - güncelleştirme GraphAPI özel alanlar göndermek için

Azure AD kimlik doğrulaması için gönderen güvenlik belirteçleri listesi için bkz. [Azure AD belirteç başvurusu](../develop/v1-id-and-access-tokens.md). Diğer belirteçler gönderen bir özel talep gerekiyorsa, Graph Gezgini ya da bildirim Azure Portalı'nda uygulama için uygulama alanın ayarlamak için kullanın *acceptMappedClaims* için **True**.    

Bu örnek Graph Gezgini kullanır:

```
PATCH https://graph.windows.net/myorganization/applications/<object_id_GUID_of_your_application> 

{
  "acceptMappedClaims":true
}
```
Bu örnekte [Azure portalında](https://portal.azure.com) güncelleştirilecek *acceptedMappedClaims* alan:
1. [Azure portalında](https://portal.azure.com) genel yönetici olarak oturum açın.
2. Seçin **Azure Active Directory** > **uygulama kayıtları**.
3. Uygulamanızı seçin > **bildirim**.
4. Seçin **Düzenle**, arama *acceptedMappedClaims* alan ve değere değiştirin **true**.
![Uygulama bildirimi](./media/application-proxy-configure-single-sign-on-with-ping-access/application-proxy-ping-access-manifest.PNG)
1. **Kaydet**’i seçin.

>[!NOTE]
>Bir özel talep kullanmak için tanımlanan ve uygulamaya atanan özel bir ilke olmalıdır.  Bu ilke, tüm gerekli özel öznitelikler içermelidir.
>
>İlke tanımı ve atama, PowerShell, Azure AD Graph Gezgini veya MS Graph yapılabilir.  Bunu PowerShell'de yapıyorsanız, ilk kullanmanız gerekebilir `New-AzureADPolicy `ve uygulama ile atamak `Set-AzureADServicePrincipalPolicy`.  Daha fazla bilgi için [Azure AD İlkesi belgeleri](../develop/active-directory-claims-mapping.md#claims-mapping-policy-assignment).

### <a name="optional---use-a-custom-claim"></a>İsteğe bağlı - özel talep kullanın
Uygulamanızın özel talep kullanın ve ek alanları dahil olmak üzere, ayrıca sahip olduğunuzdan emin olun [bir özel talep İlkesi eşlemesi oluşturulur ve uygulamaya atanan](../develop/active-directory-claims-mapping.md#claims-mapping-policy-assignment).

## <a name="download-pingaccess-and-configure-your-app"></a>PingAccess indirin ve uygulamanızı yapılandırın

Tüm Azure Active Directory kurulum adımlarını tamamladığınıza göre PingAccess yapılandırmaya geçebilirsiniz. 

Ayrıntılı adımlar için bu senaryonun PingAccess parçası Ping Identity belgelerinde devam [Azure AD için PingAccess yapılandırma](https://docs.pingidentity.com/bundle/paaad_m_ConfigurePAforMSAzureADSolution_paaad43/page/pa_c_PAAzureSolutionOverview.html).

Bu adımlar bir PingAccess hesabı zaten yoksa, alma, PingAccess sunucusu yükleme ve Azure portaldan kopyaladığınız dizin kimliği ile bir Azure AD OIDC sağlayıcı bağlantısı oluşturma işleminde size yol. Ardından, uygulama kimliği ve anahtarı değerleri üzerinde PingAccess Web oturumu oluşturmak için kullanın. Bundan sonra kimlik eşlemeyi ayarlamanız ve bir sanal ana bilgisayar, site ve uygulama oluşturun.

### <a name="test-your-app"></a>Uygulamanızı test etme

Tüm adımları tamamladıktan sonra uygulamanızı ve çalışıyor olması gerekir. Test etmek için bir tarayıcı açın ve Azure'da uygulama yayımlandığında oluşturduğunuz dış URL'sine gidin. Bir uygulamaya atanan test hesapla oturum açın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD için PingAccess yapılandırın](https://docs.pingidentity.com/bundle/paaad_m_ConfigurePAforMSAzureADSolution_paaad43/page/pa_c_PAAzureSolutionOverview.html)
- [Azure AD uygulama proxy'si nasıl çoklu oturum açma sağlar?](application-proxy-single-sign-on.md)
- [Uygulama Ara sunucusu sorunlarını giderme](application-proxy-troubleshoot.md)
