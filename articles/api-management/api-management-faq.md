---
title: Azure API yönetimi hakkında SSS | Microsoft Docs
description: Azure API Yönetimi'nde ' en iyi yöntemler ve desenler için sık sorulan sorular (SSS) yanıtlarını öğrenin.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 2fa193cd-ea71-4b33-a5ca-1f55e5351e23
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/19/2017
ms.author: apimpm
ms.openlocfilehash: 9c0c8adca9d99c00e32127e02a3d68ff668a235e
ms.sourcegitcommit: ad3e63af10cd2b24bf4ebb9cc630b998290af467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58793314"
---
# <a name="azure-api-management-faqs"></a>Azure API Yönetimi SSS
Azure API Management için sık sorulan sorular, desenleri ve en iyi yanıtları alın.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="contact-us"></a>Bizimle iletişim kurun
* [Nasıl ben Microsoft Azure API Management takım soru sorabilir?](#how-can-i-ask-the-microsoft-azure-api-management-team-a-question)

## <a name="frequently-asked-questions"></a>Sık sorulan sorular
* [Bir özellik önizlemede olduğunda ne anlama geliyor?](#what-does-it-mean-when-a-feature-is-in-preview)
* [API Management ağ geçidi ile arka uç hizmetleri arasındaki bağlantının güvenliğini nasıl sağlayabilirim?](#how-can-i-secure-the-connection-between-the-api-management-gateway-and-my-back-end-services)
* [API Management hizmet örneğimi yeni bir örneğe nasıl kopyalayabilirim?](#how-do-i-copy-my-api-management-service-instance-to-a-new-instance)
* [API Management Örneğim programlı olarak yönetebilirim?](#can-i-manage-my-api-management-instance-programmatically)
* [Yöneticiler grubuna nasıl kullanıcı ekleyebilirim?](#how-do-i-add-a-user-to-the-administrators-group)
* [Neden eklemek İlkesi Düzenleyicisi'nde kullanılamayan istediğiniz ilke mi?](#why-is-the-policy-that-i-want-to-add-unavailable-in-the-policy-editor)
* [Tek bir API birden çok ortamda nasıl ayarlayabilirim?](#how-do-i-set-up-multiple-environments-in-a-single-api)
* [SOAP API Management ile kullanabilir miyim?](#can-i-use-soap-with-api-management)
* [API Management ağ geçidi IP adresi sabit mi? Güvenlik duvarı kurallarında kullanabilir miyim?](#is-the-api-management-gateway-ip-address-constant-can-i-use-it-in-firewall-rules)
* Bir OAuth 2.0 yetkilendirme sunucusu ile AD FS güvenliği yapılandırabilir miyim?
* [Yönlendirme yöntemi, birden çok coğrafi konumlara dağıtımlarında API Management kullanıyor mu?](#what-routing-method-does-api-management-use-in-deployments-to-multiple-geographic-locations)
* [API Management hizmet örneği oluşturmak için bir Azure Resource Manager şablonu kullanabilir miyim?](#can-i-use-an-azure-resource-manager-template-to-create-an-api-management-service-instance)
* [Arka uç için otomatik olarak imzalanan bir SSL sertifikası kullanabilir miyim?](#can-i-use-a-self-signed-ssl-certificate-for-a-back-end)
* [GIT deposunu kopyalamak çalıştığınızda neden bir kimlik doğrulama hatası alıyorum?](#why-do-i-get-an-authentication-failure-when-i-try-to-clone-a-git-repository)
* [API Management, Azure ExpressRoute ile çalışır mı?](#does-api-management-work-with-azure-expressroute)
* [API Yönetimi uygulamasına dağıtıldığında sanal ağları Resource Manager stilde ayrılmış alt ağında neden kılarız?](#why-do-we-require-a-dedicated-subnet-in-resource-manager-style-vnets-when-api-management-is-deployed-into-them)
* [API yönetimi bir VNET'e dağıtırken gereken en düşük alt ağ boyutu nedir?](#what-is-the-minimum-subnet-size-needed-when-deploying-api-management-into-a-vnet)
* [API Management hizmetini bir abonelikten diğerine taşıyabilir miyim?](#can-i-move-an-api-management-service-from-one-subscription-to-another)
* [Kısıtlamalar veya API'mi alma bilinen sorunlar vardır?](#are-there-restrictions-on-or-known-issues-with-importing-my-api)

### <a name="how-can-i-ask-the-microsoft-azure-api-management-team-a-question"></a>Nasıl ben Microsoft Azure API Management takım soru sorabilir?
Bu seçeneklerden birini kullanarak bize başvurabilirsiniz:

* Kendi sorularınızı bizim [API Yönetimi MSDN Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=azureapimgmt).
* <mailto:apimgmt@microsoft.com> adresine e-posta gönderin.
* Bize bir özellik isteği gönderin [Azure geri bildirim Forumu](https://feedback.azure.com/forums/248703-api-management).

### <a name="what-does-it-mean-when-a-feature-is-in-preview"></a>Bir özellik önizlemede olduğunda ne anlama geliyor?
Bir özellik Önizleme aşamasındadır, biz etkin bir şekilde özelliği sizin için nasıl çalıştığı hakkında geri bildirim arama, anlamına gelir. Bir özellik önizlemeye sunuldu işlevsel olarak tamamlandı, ancak müşteri geri bildirimleri doğrultusunda Değiştir bir hataya neden oluşturacağız mümkündür. Üretim ortamınızda önizleme özelliğini üzerinde bağımlı olmayan öneririz. Önizleme özellikleri üzerinde herhangi bir Geribildiriminiz varsa lütfen bize başvurun seçeneklerinde biri aracılığıyla bildirin [nasıl miyim sorabileceğiniz Microsoft Azure API Management takım soru?](#how-can-i-ask-the-microsoft-azure-api-management-team-a-question).

### <a name="how-can-i-secure-the-connection-between-the-api-management-gateway-and-my-back-end-services"></a>API Management ağ geçidinde my arka uç hizmetleri arasında bağlantı güvenliğini nasıl sağlayabilirim?
API Management ağ geçidi ve arka uç hizmetlerinizi arasındaki bağlantıyı güvenli hale getirmek için birkaç seçeneğiniz vardır. Şunları yapabilirsiniz:

* HTTP temel kimlik doğrulaması kullanın. Daha fazla bilgi için [içeri aktarma ve ilk API'nizi yayımlama](import-and-publish.md).
* SSL ile karşılıklı kimlik doğrulaması bölümünde anlatıldığı gibi kullanın [arka uç Hizmetleri istemcisini kullanarak sertifika kimlik doğrulaması Azure API Management güvenliğini nasıl](api-management-howto-mutual-certificates.md).
* IP beyaz listesi, arka uç hizmet kullanın. API Management'ın tüm katmanlara ağ geçidinin IP adresi ile birkaç sabit kalır [uyarılar](#is-the-api-management-gateway-ip-address-constant-can-i-use-it-in-firewall-rules). Bu IP adreslerine izin verecek şekilde, beyaz liste ayarlayabilirsiniz. Azure Portalı'ndaki Panoda, API Management örneğinizin IP adresini alabilirsiniz.
* API Management örneğinizin bir Azure sanal ağına bağlayın.

### <a name="how-do-i-copy-my-api-management-service-instance-to-a-new-instance"></a>My API Management hizmet örneği için yeni bir örneğini nasıl kopyalayabilirim?
API Management örneği için yeni bir örneği kopyalamak istiyorsanız birkaç seçeneğiniz vardır. Şunları yapabilirsiniz:

* Yedekleme ve geri yükleme API Management işlevi. Daha fazla bilgi için [olağanüstü durum kurtarma hizmeti yedekleme uygulamak ve Azure API Yönetimi'nde geri yükleme](api-management-howto-disaster-recovery-backup-restore.md).
* Kendi yedekleme oluşturma ve kullanarak geri yükleme özelliğini [API Management REST API](/rest/api/apimanagement/). Kaydet ve varlıkları istediğiniz hizmeti örneğinin geri yüklemek için REST API'yi kullanın.
* Git kullanarak hizmet yapılandırmasını indirin ve ardından yeni bir örneğine karşıya yükleyin. Daha fazla bilgi için [kaydedin ve Git kullanarak API Management hizmet yapılandırmanızı yapılandırma](api-management-configuration-repository-git.md).

### <a name="can-i-manage-my-api-management-instance-programmatically"></a>API Management Örneğim programlı olarak yönetebilirim?
Evet, kullanarak API Management programlı olarak yönetebilirsiniz:

* [API Management REST API](/rest/api/apimanagement/).
* [Microsoft Azure ApiManagement Hizmet Yönetimi Kitaplığı SDK](https://aka.ms/apimsdk).
* [Hizmet dağıtımı](https://docs.microsoft.com/powershell/module/wds) ve [Hizmet Yönetimi](https://docs.microsoft.com/powershell/azure/servicemanagement/overview) PowerShell cmdlet'leri.

### <a name="how-do-i-add-a-user-to-the-administrators-group"></a>Bir kullanıcı Administrators grubuna nasıl ekleyebilirim?
Bir kullanıcı Administrators grubuna nasıl ekleyebileceğinizi aşağıda verilmiştir:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Güncelleştirmek istediğiniz API Management örneği kaynak grubuna gidin.
3. API Yönetimi'nde atamak **API Management katkıda bulunan** kullanıcı rolü.

Yeni eklenen katkıda bulunan Azure PowerShell artık [cmdlet'leri](https://docs.microsoft.com/powershell/azure/overview). Yönetici olarak oturum açma şöyledir:

1. Kullanım `Connect-AzAccount` cmdlet'ini oturum açın.
2. Kullanarak hizmetine sahip olan abonelik bağlamını ayarlayın `Set-AzContext -SubscriptionID <subscriptionGUID>`.
3. Çoklu oturum açma URL'si almak `Get-AzApiManagementSsoToken -ResourceGroupName <rgName> -Name <serviceName>`.
4. Yönetici portalına erişmek için URL'yi kullanın.

### <a name="why-is-the-policy-that-i-want-to-add-unavailable-in-the-policy-editor"></a>Neden eklemek İlkesi Düzenleyicisi'nde kullanılamayan istediğiniz ilke mi?
Eklemek istediğiniz ilke soluk veya İlkesi Düzenleyicisi'nde gölgeli, ilke için doğru kapsamında olduğundan emin görünüyorsa. Her ilke bildirimi, belirli kapsamlar ve ilke bölümlerde kullanabilmeniz için tasarlanmıştır. İlkenin kullanım bölümünde ilke bölüm ve bir ilke için kapsamları gözden geçirmek için bkz [API Management ilkeleri](/azure/api-management/api-management-policies).

### <a name="how-do-i-set-up-multiple-environments-in-a-single-api"></a>Tek bir API birden çok ortamda nasıl ayarlayabilirim?
Birden çok ortamı, örneğin, bir test ortamı ve bir üretim ortamında, tek bir API ayarlamak için iki seçeneğiniz vardır. Şunları yapabilirsiniz:

* Ana bilgisayar üzerinde aynı kiracıda farklı API'ler.
* Farklı kiracıların aynı API'leri barındırın.

### <a name="can-i-use-soap-with-api-management"></a>SOAP API Management ile kullanabilir miyim?
[SOAP geçişi](https://blogs.msdn.microsoft.com/apimanagement/2016/10/13/soap-pass-through/) desteği kullanıma sunuldu. Yöneticiler, SOAP hizmetini WSDL içe aktarabilir ve Azure API Management SOAP ön uç oluşturur. Geliştirici portal belgeleri, test konsolunda, ilkeleri ve analiz SOAP Hizmetleri için kullanılabilir.

### <a name="is-the-api-management-gateway-ip-address-constant-can-i-use-it-in-firewall-rules"></a>API Management ağ geçidi IP adresi sabit mi? Güvenlik duvarı kurallarında kullanabilir miyim?
API Management tüm katmanlarda API Management kiracının genel IP adresi (VIP) Kiracı ömrü boyunca bazı özel durumlar ile statiktir. Bu durumlarda IP adresi değişiklikleri:

* Hizmet silinir ve yeniden oluşturulacak.
* Hizmet aboneliği [askıya](https://github.com/Azure/azure-resource-manager-rpc/blob/master/v1.0/subscription-lifecycle-api-reference.md#subscription-states) veya [uyarı](https://github.com/Azure/azure-resource-manager-rpc/blob/master/v1.0/subscription-lifecycle-api-reference.md#subscription-states) (örneğin, ücretlerin ödenmemesi) ve ardından uzatılamaz.
* Azure sanal ağı ekleyip (geliştiricisi yalnızca sanal ağ kullanabileceğiniz ve Premium katmanı).

Çok bölgeli dağıtımlar için bölgesel adresi değişirse bölge işleci boşaltılmış ve ardından uzatılamaz (yalnızca Premium katmanında çok bölgeli dağıtımlar kullanabilirsiniz).

Çok bölgeli dağıtım için yapılandırılmış olan premium katmanı kiracılar, her bölge bir genel IP adresi atanır.

Azure portalında Kiracı sayfasında, IP adresi (veya adresleri, çok bölgeli dağıtımlar) alabilirsiniz.

### <a name="can-i-configure-an-oauth-20-authorization-server-with-ad-fs-security"></a>Bir OAuth 2.0 yetkilendirme sunucusu ile AD FS güvenliği yapılandırabilir miyim?
Bir OAuth 2.0 yetkilendirme sunucusu ile Active Directory Federasyon Hizmetleri (AD FS) güvenlik yapılandırma konusunda bilgi için bkz: [API Management kullanarak ADFS](https://phvbaars.wordpress.com/2016/02/06/using-adfs-in-api-management/).

### <a name="what-routing-method-does-api-management-use-in-deployments-to-multiple-geographic-locations"></a>Yönlendirme yöntemi, birden çok coğrafi konumlara dağıtımlarında API Management kullanıyor mu?
API Management kullanan [performans trafiği yönlendirme yöntemini](../traffic-manager/traffic-manager-routing-methods.md#performance) dağıtımlarda birden fazla coğrafi konumu. Gelen trafiği en yakın API ağ geçidine yönlendirilir. Bir bölgeyi çevrimdışı olursa, gelen trafiği otomatik olarak sonraki en yakın ağ geçidine yönlendirilir. Yönlendirme yöntemleri hakkında daha fazla bilgi edinin [Traffic Manager yönlendirme yöntemleri](../traffic-manager/traffic-manager-routing-methods.md).

### <a name="can-i-use-an-azure-resource-manager-template-to-create-an-api-management-service-instance"></a>API Management hizmet örneği oluşturmak için bir Azure Resource Manager şablonu kullanabilir miyim?
Evet. Bkz: [Azure API Management hizmeti](https://aka.ms/apimtemplate) hızlı başlangıç şablonları.

### <a name="can-i-use-a-self-signed-ssl-certificate-for-a-back-end"></a>Otomatik olarak imzalanan bir SSL sertifikası bir arka uç için kullanabilir miyim?
Evet. Bu API için doğrudan göndererek veya PowerShell aracılığıyla yapılabilir. Bu sertifika zinciri doğrulamasını devre dışı bırakır ve otomatik olarak imzalanan veya özel olarak imzalanmış sertifikaların API Yönetimi'nden arka uç hizmetlerine iletişim kurarken kullanması sağlayacak.

#### <a name="powershell-method"></a>PowerShell yöntemi ####
Kullanım [ `New-AzApiManagementBackend` ](https://docs.microsoft.com/powershell/module/az.apimanagement/new-azapimanagementbackend) (için yeni arka uç) veya [ `Set-AzApiManagementBackend` ](https://docs.microsoft.com/powershell/module/az.apimanagement/set-azapimanagementbackend) (mevcut arka uç için) PowerShell cmdlet'leri ve `-SkipCertificateChainValidation` parametresi `True`. 

```powershell
$context = New-AApiManagementContext -resourcegroup 'ContosoResourceGroup' -servicename 'ContosoAPIMService'
New-AzApiManagementBackend -Context  $context -Url 'https://contoso.com/myapi' -Protocol http -SkipCertificateChainValidation $true
```

#### <a name="direct-api-update-method"></a>Doğrudan API güncelleştirme yöntemi ####
1. Oluşturma bir [arka uç](/rest/api/apimanagement/) API Management kullanarak varlık.     
2. Ayarlama **skipCertificateChainValidation** özelliğini **true**.     
3. Artık otomatik olarak imzalanan sertifikalar izin vermek istiyorsanız, arka uç varlığı silmek veya ayarlayın **skipCertificateChainValidation** özelliğini **false**.

### <a name="why-do-i-get-an-authentication-failure-when-i-try-to-clone-a-git-repository"></a>Git deposunu kopyalamak çalıştığınızda neden bir kimlik doğrulama hatası alıyorum?
Git kimlik bilgileri Yöneticisi kullanıyorsanız veya Visual Studio kullanarak bir Git deposunu kopyalamak çalışıyorsanız, bilinen bir sorun Windows kimlik bilgileri iletişim kutusu içine çalışabilir. İletişim kutusu, parola uzunluğu 127 karakter sınırlar ve Microsoft tarafından oluşturulan parola keser. Parola kısaltmayı üzerinde çalışıyoruz. Şimdilik, Git Bash Lütfen Git deponuzu kopyalamak için kullanın.

### <a name="does-api-management-work-with-azure-expressroute"></a>API Management, Azure ExpressRoute ile çalışır mı?
Evet. API Management, Azure ExpressRoute ile çalışır.

### <a name="why-do-we-require-a-dedicated-subnet-in-resource-manager-style-vnets-when-api-management-is-deployed-into-them"></a>API Yönetimi uygulamasına dağıtıldığında sanal ağları Resource Manager stilde ayrılmış alt ağında neden kılarız?
API yönetimi için ayrılmış alt ağında gereksinim (PAAS V1 katman) Klasik dağıtım modeline oluşturulmuştur gerçeği, gelir. Biz bir Resource Manager sanal ağı (V2 katman) dağıtabilmenize karşın, o sonuçları vardır. Azure'da Klasik dağıtım modeli Resource Manager modeli ile sıkı bir şekilde eşleşmediğinden ve bu nedenle V2 katmanında bir kaynak oluşturun, S1 katmanı bunu bilmez ve API Management'ın bir NIC'ye önceden ayrılmış bir IP kullanmaya çalışıyor gibi sorunlar oluşabilir  (V2 üzerinde yerleşik).
Bilgi edinmek için Azure Klasik ve Resource Manager modeli fark hakkında daha fazla başvurmak [dağıtım modelleri arasındaki fark](../azure-resource-manager/resource-manager-deployment-model.md).

### <a name="what-is-the-minimum-subnet-size-needed-when-deploying-api-management-into-a-vnet"></a>API yönetimi bir VNET'e dağıtırken gereken en düşük alt ağ boyutu nedir?
API Management'ı dağıtmak için gerekli en düşük alt ağ boyutu [/29](../virtual-network/virtual-networks-faq.md#configuration), Azure'ı destekleyen en az bir alt ağ boyutu olduğu.

### <a name="can-i-move-an-api-management-service-from-one-subscription-to-another"></a>API Management hizmeti bir abonelikten diğerine taşıyabilirim?
Evet. Bilgi edinmek için bkz. nasıl [kaynakları yeni kaynak grubuna veya aboneliğe taşıma](../azure-resource-manager/resource-group-move-resources.md).

### <a name="are-there-restrictions-on-or-known-issues-with-importing-my-api"></a>Kısıtlamalar veya API'mi alma bilinen sorunlar vardır?
[Bilinen sorunlar ve kısıtlamalar](api-management-api-import-restrictions.md) açık API(Swagger) için WSDL ve WADL biçimlendirir.
