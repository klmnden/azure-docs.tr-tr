---
title: Üst bilgi tabanlı kimlik doğrulaması ile PingAccess için Azure AD uygulama proxy'si | Microsoft Docs
description: Üst bilgi tabanlı kimlik doğrulamasını desteklemek için PingAccess ve uygulama ara sunucusu ile uygulama yayımlayın.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/08/2019
ms.author: celested
ms.reviewer: harshja
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 54a99d001f8cb59af3042ce8b6849a2cd9480e99
ms.sourcegitcommit: 0ebc62257be0ab52f524235f8d8ef3353fdaf89e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67724002"
---
# <a name="header-based-authentication-for-single-sign-on-with-application-proxy-and-pingaccess"></a>Üst bilgi tabanlı kimlik doğrulaması için uygulama proxy'si ile PingAccess ile çoklu oturum açma

Azure AD müşterileriniz uygulamanızın daha fazla erişebilmesi için azure Active Directory (Azure AD) uygulama proxy'si ile PingAccess ortak çalışma yürütmektedir. PingAccess genişletir [mevcut uygulama proxy'si teklifleri](application-proxy.md) üst bilgileri için kimlik doğrulaması kullanan uygulamalar için çoklu oturum açma erişimi eklenecek.

## <a name="whats-pingaccess-for-azure-ad"></a>Azure AD için PingAccess nedir?

Azure AD için PingAccess ile kimlik doğrulaması için üst bilgi kullanan uygulamalar, kullanıcılar erişim ve çoklu oturum açma (SSO) verebilirsiniz. Uygulama Ara sunucusu, bu uygulamaları diğer, Azure AD kimlik doğrulaması yapmak için kullanarak ve ardından bağlayıcı hizmetini üzerinden trafiği geçirerek gibi davranır. PingAccess uygulamaları önünde yer alır ve Azure AD'den erişim belirteci bir üstbilgisine çevirir. Uygulama kimlik doğrulaması okuyabilmesi biçiminde alır.

Kurumsal uygulamalarınıza kullanmak oturum açtığında, kullanıcılar farklı bir şey fark olmaz. Bunlar yine de her yerde her cihazda çalışabilirsiniz. Bunlar yine de yükleri otomatik olarak dengelemek şekilde uygulama ara sunucusu bağlayıcıları doğrudan bakılmaksızın kendi kimlik doğrulama türü, tüm uygulamalar için uzaktan trafiği.

## <a name="how-do-i-get-access"></a>Erişimi nasıl alabilirim?

Bu senaryo Azure Active Directory ve PingAccess iş ortaklığı geldiğinden, iki hizmet için Lisansınızın olması gerekir. Ancak, en fazla 20 uygulamaları kapsayan temel bir PingAccess lisansı Azure Active Directory Premium abonelikleri içerir. 20'den fazla üst bilgi tabanlı uygulamaları yayımlamak gerekiyorsa, PingAccess ek lisans satın alabilirsiniz.

Daha fazla bilgi için bkz. [Azure Active Directory sürümleri](../fundamentals/active-directory-whatis.md).

## <a name="publish-your-application-in-azure"></a>Uygulamanızı Azure'da yayımlayın

Bu makale, bu senaryo ile bir uygulama ilk kez yayımlamak üzere kişiler için yazılmıştır. Yayımlama adımları gerçekleşen yanı sıra, uygulama proxy'si ve PingAccess ile çalışmaya başlama sırasında size kılavuzluk eder. Her iki hizmet zaten yapılandırdınız ancak yayımlama adımları tazelemek istiyorsanız, atlamak [uygulamanızı Azure ad uygulama ara sunucusu ile ekleme](#add-your-application-to-azure-ad-with-application-proxy) bölümü.

> [!NOTE]
> Bu senaryo Azure AD arasındaki iş ortaklığı olduğundan ve PingAccess, alan yönergelerden bazılarını var Ping Identity sitesinde.

### <a name="install-an-application-proxy-connector"></a>Uygulama Ara sunucusu bağlayıcısını yükleme

Uygulama Ara sunucusu, etkin ve bir bağlayıcı zaten yüklü, etkinleştirdiyseniz bu bölümü atlayın ve Git [uygulamanızı Azure ad uygulama ara sunucusu ile ekleme](#add-your-application-to-azure-ad-with-application-proxy).

Uygulama Ara sunucusu Bağlayıcısı'nı yayımladığınız uygulamalarda uzak çalışanlarınız gelen trafiği yönlendiren bir Windows Server hizmetidir. Daha ayrıntılı yükleme yönergeleri için bkz. [Öğreticisi: Azure Active Directory Uygulama proxy'si aracılığıyla uzaktan erişim için şirket içi uygulama ekleme](application-proxy-add-on-premises-application.md).

1. Oturum [Azure Active Directory portalında](https://aad.portal.azure.com/) uygulama yöneticisi olarak. **Azure Active Directory Yönetim Merkezi** sayfası görüntülenir.
1. Seçin **Azure Active Directory** > **uygulama proxy'si** > **bağlayıcı hizmeti indir**. **Uygulama Proxy Bağlayıcısı indirme** sayfası görüntülenir.

   ![Uygulama Ara sunucusu Bağlayıcısı indirme](./media/application-proxy-configure-single-sign-on-with-ping-access/application-proxy-connector-download.png)

1. Yükleme yönergelerini izleyin.

Bağlayıcıyı indirdiğinizde otomatik olarak etkinleştirmelisiniz uygulama proxy'si dizininiz için ancak Aksi takdirde, seçebileceğiniz **uygulama ara sunucusunu etkinleştirme**.

### <a name="add-your-application-to-azure-ad-with-application-proxy"></a>Uygulamanızı Azure ad uygulama ara sunucusu ile ekleme

Azure portalında atmanız gereken iki eylemler vardır. İlk olarak, uygulama ara sunucusu ile uygulamanızı yayımlamak gerekir. Ardından, bazı PingAccess adımları sırasında kullanabileceğiniz uygulamayla ilgili bilgileri toplamak gerekir.

#### <a name="publish-your-application"></a>Uygulamanızı yayımlama

İlk uygulamanızı yayımlamak zorunda kalırsınız. Bu eylem içerir:

- Şirket içi uygulamanızı Azure AD'ye ekleme
- Uygulamayı test etme ve üst bilgi tabanlı SSO seçmek için kullanıcı atama
- Uygulamanın yeniden yönlendirme URL'si ayarlama
- Kullanıcıların ve diğer uygulamalara şirket içi uygulamanız için izinleri veriliyor

Kendi şirket içi uygulamanızı yayımlamak için:

1. Son bölümde Aksi takdirde oturum [Azure Active Directory portalında](https://aad.portal.azure.com/) uygulama yöneticisi olarak.
1. Seçin **kurumsal uygulamalar** > **yeni uygulama** > **şirket içi uygulama**. **Kendi şirket içi uygulamanızı ekleme** sayfası görüntülenir.

   ![Kendi şirket içi uygulamanızı ekleme](./media/application-proxy-configure-single-sign-on-with-ping-access/add-your-own-on-premises-application.png)
1. Yeni uygulamanız hakkındaki bilgilerle gerekli alanları doldurun. Ayarları için aşağıdaki yönergeleri kullanın.

   > [!NOTE]
   > Bu adımın daha ayrıntılı bir kılavuz için bkz. [şirket içi bir uygulamayı Azure AD'ye ekleme](application-proxy-add-on-premises-application.md#add-an-on-premises-app-to-azure-ad).

   1. **İç URL**: Normalde şirket ağında olduğunuzda, sizi uygulamanın oturum açma sayfasına götürür URL'sini sağlayın. Bu senaryo için bağlayıcı PingAccess proxy uygulamanın ön sayfa olarak ele almanız gerekir. Bu biçimi kullanın: `https://<host name of your PingAccess server>:<port>`. Varsayılan olarak 3000 bağlantı noktasıdır, ancak PingAccess yapılandırabilirsiniz.

      > [!WARNING]
      > Bu tür çoklu oturum açma için iç URL kullanmalısınız `https` ve `http`.

   1. **Ön kimlik doğrulama yöntemi**: Seçin **Azure Active Directory**.
   1. **Bilgilerde URL'yi çevir**: Seçin **Hayır**.

   > [!NOTE]
   > Bu ilk uygulamanızı ise, bağlantı noktası 3000 başlatıp PingAccess yapılandırmanızı değiştirirseniz bu ayarını güncelleştirmek için geri dönen kullanın. Sonraki uygulamalar için bağlantı noktası PingAccess yapılandırdığınız dinleyici eşleşmesi gerekir. Daha fazla bilgi edinin [dinleyicileri PingAccess içinde](https://documentation.pingidentity.com/pingaccess/pa31/index.shtml#Listeners.html).

1. **Add (Ekle)** seçeneğini belirleyin. Yeni uygulama için genel bakış sayfası görüntülenir.

Şimdi uygulamayı test etmek için kullanıcı atama ve üst bilgi tabanlı çoklu oturum açma seçin:

1. Uygulama kenar çubuğundan seçin **kullanıcılar ve gruplar** > **Kullanıcı Ekle** > **kullanıcılar ve gruplar (\<numarası > Seçili)** . Aralarından seçim yapabileceğiniz kullanıcıların ve grupların listesi görünür.

   ![Kullanıcıların ve grupların listesini gösterir](./media/application-proxy-configure-single-sign-on-with-ping-access/users-and-groups.png)

1. Uygulamayı test etmek için bir kullanıcı seçip **seçin**. Bu test hesabı şirket içi uygulamaya erişimi olduğundan emin olun.
1. **Ata**'yı seçin.
1. Uygulama kenar çubuğundan seçin **çoklu oturum açma** > **üst bilgi tabanlı**.

   > [!TIP]
   > Bu üst bilgi tabanlı çoklu oturum açma kullanarak ilk kez ise, PingAccess yüklemeniz gerekir. Azure aboneliğinizi PingAccess yüklemenizle birlikte otomatik olarak ilişkilendirilir emin olmak için PingAccess indirmek için bu tek oturum açma sayfasında bağlantıyı kullanın. Şimdi indirme sitesi açmak veya bu sayfada daha sonra tekrar deneyin.

   ![Üst bilgi tabanlı oturum açma ekranını ve PingAccess gösterir](./media/application-proxy-configure-single-sign-on-with-ping-access/sso-header.png)

1. **Kaydet**’i seçin.

Ardından, yeniden yönlendirme URL'si dış URL'nizi ayarlanır emin olun:

1. Gelen **Azure Active Directory Yönetim Merkezi** kenar seçme **Azure Active Directory** > **uygulama kayıtları**. Uygulamaların listesi görüntülenir.
1. Uygulamanızı seçin.
1. Yanındaki bağlantıyı seçin **yeniden yönlendirme URI'leri**, yeniden yönlendirme URI'leri zaman ayarlanan web ve genel istemcilerinin sayısını gösteren. **\<Uygulama adı >-kimlik doğrulaması** sayfası görüntülenir.
1. Daha önce uygulamaya atanmış bir dış URL içinde olup olmadığını denetleyin **yeniden yönlendirme URI'leri** listesi. Değilse, dış URL'yi ekleyin artık, bir yeniden yönlendirme URI'si türünü kullanan **Web**seçip **Kaydet**.

Son olarak, şirket içi uygulamanız kullanıcıların okuma erişimi ve diğer uygulamalara okuma/yazma erişimine sahip olacak şekilde ayarlayın:

1. Gelen **uygulama kayıtları** , uygulamanız için kenar seçin **API izinleri** > **bir izin eklemek**  >   **Microsoft API'leri** > **Microsoft Graph**. **İstek API izinleri** sayfasındaki **Microsoft Graph** görünür API'ler için Windows Azure Active Directory içerir.

   ![İstek API izinleri sayfası gösterilir.](./media/application-proxy-configure-single-sign-on-with-ping-access/required-permissions.png)

1. Seçin **temsilci izinleri** > **kullanıcı** > **User.Read**.
1. Seçin **uygulama izinleri** > **uygulama** > **Application.ReadWrite.All**.
1. Seçin **izinleri eklemek**.
1. İçinde **API izinleri** sayfasında **vermek için yönetici onayı \<dizin adınız >** .

#### <a name="collect-information-for-the-pingaccess-steps"></a>PingAccess adımlar için bilgi toplama

Bu üç parça bilgi (PingAccess uygulamanızla ayarlamak için tüm GUID'ler) toplamak gerekir:

| Azure AD alanının adı | PingAccess alan adı | Veri biçimi |
| --- | --- | --- |
| **Uygulama (istemci) kimliği** | **İstemci kimliği** | GUID |
| **(Kiracı) dizin kimliği** | **Veren** | GUID |
| `PingAccess key` | **İstemci gizli anahtarı** | Rastgele dize |

Bu bilgileri toplamak için:

1. Gelen **Azure Active Directory Yönetim Merkezi** kenar seçme **Azure Active Directory** > **uygulama kayıtları**. Uygulamaların listesi görüntülenir.
1. Uygulamanızı seçin. **Uygulama kayıtları** sayfası uygulamanızın görünür.

   ![Bir uygulama için kayıt genel bakış](./media/application-proxy-configure-single-sign-on-with-ping-access/registration-overview-for-an-application.png)

1. Yanındaki **uygulama (istemci) kimliği** değeri, select **Panoya Kopyala** simgesini, daha sonra kopyalayın ve kaydedin. Bu değer daha sonra PingAccess'ın istemci kimliği belirtin.
1. Sonraki **dizin (Kiracı) kimliği** değeri, aynı zamanda seçin **Panoya Kopyala**, daha sonra kopyalayın ve kaydedin. Bu değer daha sonra PingAccess'ın veren belirtin.
1. Kenar **uygulama kayıtları** uygulamanız için seçin **sertifikalarını ve gizli dizilerini** > **yeni gizli**. **İstemci gizli dizi eklemek** sayfası görüntülenir.

   ![İstemci gizli Sayfası Ekle gösterir](./media/application-proxy-configure-single-sign-on-with-ping-access/add-a-client-secret.png)

1. İçinde **açıklama**, türü `PingAccess key`.
1. Altında **Expires**, PingAccess anahtarını ayarlamak nasıl seçin: **1 yıl içinde**, **2 yıl içinde**, veya **hiçbir zaman**.
1. **Add (Ekle)** seçeneğini belirleyin. PingAccess anahtar istemci gizli tablosunda görünür bir rastgele bu autofills içinde dize **değer** alan.
1. PingAccess anahtarının yanındaki **değer** alanın, Seç **Panoya Kopyala** simgesini, daha sonra kopyalayın ve kaydedin. Bu değer daha sonra PingAccess'ın gizli belirtin.

### <a name="update-graphapi-to-send-custom-fields-optional"></a>Özel alanlar (isteğe bağlı) göndermek için GraphAPI güncelleştir

İçindeki diğer belirteç gönderen bir özel talep gerekiyorsa access_token PingAccess tarafından tüketilen, Ayarla `acceptMappedClaims` uygulama alanına `True`. Graph Gezgini kullanabilirsiniz veya bu değişikliği yapmak için Azure AD portalının uygulama bildirimi.

**Bu örnek Graph Gezgini kullanır:**

```
PATCH https://graph.windows.net/myorganization/applications/<object_id_GUID_of_your_application>

{
  "acceptMappedClaims":true
}
```

**Bu örnekte [Azure Active Directory portalında](https://aad.portal.azure.com/) güncelleştirilecek `acceptMappedClaims` alan:**

1. Oturum [Azure Active Directory portalında](https://aad.portal.azure.com/) uygulama yöneticisi olarak.
1. Seçin **Azure Active Directory** > **uygulama kayıtları**. Uygulamaların listesi görüntülenir.
1. Uygulamanızı seçin.
1. Kenar **uygulama kayıtları** uygulamanızın, seçin sayfasında **bildirim**. Uygulamanızın kayıt bildirim JSON kodunu görünür.
1. Arama `acceptMappedClaims` alan ve değere değiştirin `True`.
1. **Kaydet**’i seçin.

### <a name="use-of-optional-claims-optional"></a>İsteğe bağlı taleplerin (isteğe bağlı) kullanın

İsteğe bağlı bir talep, her kullanıcı ve Kiracı standard-but-not-included-by-default talep eklemenize olanak sağlar. Uygulama bildirimini değiştirerek, uygulamanız için isteğe bağlı bir talep yapılandırabilirsiniz. Daha fazla bilgi için bkz. [Azure AD uygulama bildirim makaleyi anlama](https://docs.microsoft.com/azure/active-directory/develop/reference-app-manifest/)

PingAccess tüketecektir access_token ile e-posta adresi eklemek için örnek:
```
    "optionalClaims": {
        "idToken": [],
        "accessToken": [
            {
                "name": "email",
                "source": null,
                "essential": false,
                "additionalProperties": []
            }
        ],
        "saml2Token": []
    },
```

### <a name="use-of-claims-mapping-policy-optional"></a>Talep eşleme ilkesi (isteğe bağlı) kullanın

[Talep eşleme ilkesi (Önizleme)](https://docs.microsoft.com/azure/active-directory/develop/active-directory-claims-mapping#claims-mapping-policy-properties) AzureAD içinde yok öznitelikler için. ADFS veya kullanıcı nesneleri tarafından desteklenen ek özel talep ekleyerek, eski şirket içi uygulamaları buluta geçirmek talep eşleme sağlar

Uygulamanızın özel talep kullanın ve ek alanları dahil olmak üzere seçtiğiniz emin olması da [bir özel talep İlkesi eşlemesi oluşturulur ve uygulamaya atanan](../develop/active-directory-claims-mapping.md#claims-mapping-policy-assignment).

> [!NOTE]
> Bir özel talep kullanmak için tanımlanan ve uygulamaya atanan özel bir ilke olmalıdır. Bu ilke, tüm gerekli özel öznitelikler içermelidir.
>
> İlke tanımı ve PowerShell, Azure AD Graph Gezgini ya da Microsoft Graph aracılığıyla atanabilecek yapabilirsiniz. PowerShell'de bunları yapıyorsanız, ilk kullanmanız gerekebilir `New-AzureADPolicy` ve uygulama ile atamak `Add-AzureADServicePrincipalPolicy`. Daha fazla bilgi için [talep eşleme ilke ataması](../develop/active-directory-claims-mapping.md#claims-mapping-policy-assignment).

Örnek:
```powershell
$pol = New-AzureADPolicy -Definition @('{"ClaimsMappingPolicy":{"Version":1,"IncludeBasicClaimSet":"true", "ClaimsSchema": [{"Source":"user","ID":"employeeid","JwtClaimType":"employeeid"}]}}') -DisplayName "AdditionalClaims" -Type "ClaimsMappingPolicy"

Add-AzureADServicePrincipalPolicy -Id "<<The object Id of the Enterprise Application you published in the previous step, which requires this claim>>" -RefObjectId $pol.Id
```

### <a name="enable-pingaccess-to-use-custom-claims"></a>Özel talepler kullanmak PingAccess etkinleştir

Özel talepler kullanmak PingAccess ek talep kullanan uygulamaya bekliyorsanız ancak gerekli, isteğe bağlı etkinleştirmektir.

Aşağıdaki adımda PingAccess yapılandıracağınız, Web oturumu oluşturur (Ayarlar -> erişim -> Web oturumları) olmalıdır **istek profili** seçimi ve **kullanıcı özniteliklerini Yenile** kümesine **yok**

## <a name="download-pingaccess-and-configure-your-application"></a>PingAccess indirin ve uygulamanızı yapılandırın

Tüm Azure Active Directory kurulum adımlarını tamamladığınıza göre PingAccess yapılandırmaya geçebilirsiniz.

Ayrıntılı adımlar için bu senaryonun PingAccess parçası Ping Identity belgelerinde devam edin. Bölümündeki yönergeleri [Microsoft Azure AD uygulama proxy'si kullanarak yayımlanmış uygulamaları korumak Azure AD için PingAccess yapılandırma](https://docs.pingidentity.com/bundle/paaad_m_ConfigurePAforMSAzureADSolution_paaad43/page/pa_c_PAAzureSolutionOverview.html) Ping Identity web sitesinde.

Bu adımlar PingAccess yükleyin ve PingAccess hesabınızı ayarlayın (zaten yoksa) yardımcı olur. Ardından, bir Azure AD Openıd Connect (OIDC) bağlantı oluşturmak için bir belirteç sağlayıcısı ile ayarlamanız **dizin (Kiracı) kimliği** Azure AD Portalı'ndan kopyaladığınız değeri. Ardından, üzerinde PingAccess web oturumu oluşturmak için kullandığınız **uygulama (istemci) kimliği** ve `PingAccess key` değerleri. Bundan sonra kimlik eşlemeyi ayarlamanız ve bir sanal ana bilgisayar, site ve uygulama oluşturun.

### <a name="test-your-application"></a>Uygulamanızı test edin

Tüm adımları tamamladıktan sonra uygulamanız ve çalışıyor olması gerekir. Test etmek için bir tarayıcı açın ve uygulamayı Azure'da yayımladığınızda, oluşturduğunuz dış URL'sine gidin. Uygulamaya atanan test hesapla oturum açın.

## <a name="next-steps"></a>Sonraki adımlar

- [Microsoft Azure AD uygulama proxy'si kullanarak yayımlanmış uygulamaları korumak Azure AD için PingAccess yapılandırın](https://docs.pingidentity.com/bundle/paaad_m_ConfigurePAforMSAzureADSolution_paaad43/page/pa_c_PAAzureSolutionOverview.html)
- [Azure Active Directory'de uygulamalar için çoklu oturum açma](what-is-single-sign-on.md)
- [Uygulama proxy'si sorunlarını ve hata iletileri sorunlarını giderme](application-proxy-troubleshoot.md)
