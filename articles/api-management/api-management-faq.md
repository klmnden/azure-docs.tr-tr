---
title: Azure API Management ile ilgili SSS | Microsoft Docs
description: "Sık sorulan sorular, desenleri ve en iyi uygulamalar Azure API Management'te yanıtlarını öğrenin."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 2fa193cd-ea71-4b33-a5ca-1f55e5351e23
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: a9740cf527e4a9811b510ad5c96e5ab769efc2d9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-api-management-faqs"></a>Azure API Yönetimi SSS
Sık sorulan sorular, desenleri ve en iyi yöntemler yanıtlarını için Azure API Management alın.

## <a name="contact-us"></a>Bizimle iletişim kurun
* [Nasıl t Microsoft Azure API Management ekibi bir soru sorabilir miyim?](#how-can-i-ask-the-microsoft-azure-api-management-team-a-question)


## <a name="frequently-asked-questions"></a>Sık sorulan sorular
* [Bir özelliğin önizlemede olduğunda ne anlama geliyor?](#what-does-it-mean-when-a-feature-is-in-preview)
* [API Yönetimi ağ geçidi ve arka uç Hizmetlerim arasındaki bağlantı güvenliğini nasıl sağlayabilirsiniz?](#how-can-i-secure-the-connection-between-the-api-management-gateway-and-my-back-end-services)
* [Yeni bir örneğine nasıl my API Management hizmet örneği kopyalayın?](#how-do-i-copy-my-api-management-service-instance-to-a-new-instance)
* [My API Management örneği program aracılığıyla yönetebilir miyim?](#can-i-manage-my-api-management-instance-programmatically)
* [Yöneticiler grubuna nasıl kullanıcı eklensin mi?](#how-do-i-add-a-user-to-the-administrators-group)
* [Neden ilke düzenleyicisinde kullanılamaz eklemek istediğiniz ilke mi?](#why-is-the-policy-that-i-want-to-add-unavailable-in-the-policy-editor)
* [API sürümü oluşturma API Management'te nasıl kullanabilirim?](#how-do-i-use-api-versioning-in-api-management)
* [Tek bir API birden çok ortamında nasıl ayarlayabilirim?](#how-do-i-set-up-multiple-environments-in-a-single-api)
* [API Management ile SOAP kullanabilir miyim?](#can-i-use-soap-with-api-management)
* [API Yönetimi ağ geçidi IP adresi sabit mi? Bu güvenlik duvarı kurallarında kullanabilir miyim?](#is-the-api-management-gateway-ip-address-constant-can-i-use-it-in-firewall-rules)
* [Bir OAuth 2.0 yetkilendirme sunucusu ile AD FS güvenliği yapılandırabilir miyim?](#can-i-configure-an-oauth-20-authorization-server-with-adfs-security)
* [Hangi yönlendirme yöntemini API Management birden çok coğrafi konumlara dağıtımlarında kullanıyor mu?](#what-routing-method-does-api-management-use-in-deployments-to-multiple-geographic-locations)
* [API Management hizmet örneği oluşturmak için bir Azure Resource Manager şablonu kullanabilir miyim?](#can-i-use-an-azure-resource-manager-template-to-create-an-api-management-service-instance)
* [Otomatik olarak imzalanan bir SSL sertifikası bir arka uç için kullanabilir miyim?](#can-i-use-a-self-signed-ssl-certificate-for-a-back-end)
* [Bir GIT deposuna kopyalamak çalıştığımda neden bir kimlik doğrulama hatası alabilir?](#why-do-i-get-an-authentication-failure-when-i-try-to-clone-a-git-repository)
* [API Management Azure ExpressRoute ile çalışır mı?](#does-api-management-work-with-azure-expressroute)
* [Neden API Management içine dağıtıldığında biz sanal ağlar Resource Manager stilde ayrılmış bir alt ağ gerektiriyor mu?](#why-do-we-require-a-dedicated-subnet-in-resource-manager-style-vnets-when-api-management-is-deployed-into-them)
* [API Management bir VNET dağıtırken gereken en düşük alt ağ boyutu nedir?](#what-is-the-minimum-subnet-size-needed-when-deploying-api-management-into-a-vnet)
* [I bir API Management hizmeti bir abonelikten diğerine taşıyabilir miyim?](#can-i-move-an-api-management-service-from-one-subscription-to-another)
* [Kısıtlamalar veya my API içeri aktarma bilinen sorunlar vardır?](#are-there-restrictions-on-or-known-issues-with-importing-my-api)

### <a name="how-can-i-ask-the-microsoft-azure-api-management-team-a-question"></a>Nasıl t Microsoft Azure API Management ekibi bir soru sorabilir miyim?
Bize bu seçeneklerden birini kullanarak başvurabilir:

* Sorularınızı sonrası bizim [API Management MSDN Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=azureapimgmt).
* E-posta Gönder < mailto:apimgmt@microsoft.com >.
* Bize özellik isteği gönderin [Azure geri bildirim Forumunda](https://feedback.azure.com/forums/248703-api-management).

### <a name="what-does-it-mean-when-a-feature-is-in-preview"></a>Bir özelliğin önizlemede olduğunda ne anlama geliyor?
Bir özelliğin önizlemede olduğunda, biz etkin olarak özellik sizin için nasıl çalıştığını hakkında geri bildirim aramayı olduğunu anlamına gelir. Bir özellik Önizleme'de işlevsel olarak tamamlanır ancak biz müşteri geri bildirimine yanıt değiştirmek sonu hale getireceğiz mümkündür. Üretim ortamınızda önizleme özelliği üzerinde bağlı verme öneririz. Önizleme özelliklerini herhangi bir Geribildiriminiz varsa lütfen aracılığıyla kişi seçeneklerden birini bize [nasıl miyim isteyin Microsoft Azure API Management ekibi soru?](#how-can-i-ask-the-microsoft-azure-api-management-team-a-question).

### <a name="how-can-i-secure-the-connection-between-the-api-management-gateway-and-my-back-end-services"></a>API Yönetimi ağ geçidi ve arka uç Hizmetlerim arasındaki bağlantı güvenliğini nasıl sağlayabilirsiniz?
API Yönetimi ağ geçidi ve arka uç hizmetlerini arasındaki bağlantının güvenli hale getirmek için birkaç seçeneğiniz vardır. Şunları yapabilirsiniz:

* HTTP temel kimlik doğrulaması kullanın. Daha fazla bilgi için bkz: [API yapılandırma ayarları](api-management-howto-create-apis.md#configure-api-settings).
* Bölümünde açıklandığı gibi SSL karşılıklı kimlik doğrulaması kullanmak [arka uç hizmetlerini istemcisini kullanarak Azure API Management'te sertifika kimlik doğrulaması güvenliğini sağlamak nasıl](api-management-howto-mutual-certificates.md).
* IP uygulamaları güvenilir listeye almayı arka uç hizmet kullanın. Standart veya Premium katman API Management örneği varsa, ağ geçidinin IP adresi sabit kalır. Bu IP adreslerine izin vermek için beyaz liste ayarlayabilirsiniz. Azure Portalı'ndaki Panoda API Management Örneğinize IP adresini elde edebilirsiniz.
* API Management örneği bir Azure sanal ağına bağlayın.

### <a name="how-do-i-copy-my-api-management-service-instance-to-a-new-instance"></a>Yeni bir örneğine nasıl my API Management hizmet örneği kopyalayın?
API Management örneği için yeni bir örnek kopyalamak istiyorsanız, birkaç seçeneğiniz vardır. Şunları yapabilirsiniz:

* Yedekleme ve geri yükleme API Management işlevi. Daha fazla bilgi için bkz: [olağanüstü durum kurtarma hizmeti Yedekleme kullanarak uygulama ve Azure API Management'te geri yükleme](api-management-howto-disaster-recovery-backup-restore.md).
* Kendi yedeği oluşturmak ve kullanarak geri yükleme özelliğini [API Management REST API](https://msdn.microsoft.com/library/azure/dn776326.aspx). Kaydet ve varlıkları istediğiniz hizmet örneğinden geri yüklemek için REST API kullanın.
* Git kullanarak hizmet yapılandırmasını indirmek ve yeni bir örneğine yükleyin. Daha fazla bilgi için bkz: [kaydetmek ve API Management hizmeti yapılandırmanızı Git kullanarak yapılandırmak nasıl](api-management-configuration-repository-git.md).

### <a name="can-i-manage-my-api-management-instance-programmatically"></a>My API Management örneği program aracılığıyla yönetebilir miyim?
Evet, kullanarak API Management program aracılığıyla yönetebilirsiniz:

* [API Management REST API](https://msdn.microsoft.com/library/azure/dn776326.aspx).
* [Microsoft Azure ApiManagement Hizmet Yönetimi Kitaplığı SDK](http://aka.ms/apimsdk).
* [Hizmet dağıtımı](https://msdn.microsoft.com/library/mt619282.aspx) ve [Hizmet Yönetimi](https://msdn.microsoft.com/library/mt613507.aspx) PowerShell cmdlet'leri.

### <a name="how-do-i-add-a-user-to-the-administrators-group"></a>Yöneticiler grubuna nasıl kullanıcı eklensin mi?
Bir kullanıcı Administrators grubuna nasıl ekleyebileceğiniz aşağıda verilmiştir:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Güncelleştirmek istediğiniz API Management örneğinin kaynak grubuna gidin.
3. API Yönetimi'nde, Ata **API Management katkıda bulunan** kullanıcı rolüne.

Artık yeni eklenen katkıda bulunan Azure PowerShell kullanarak [cmdlet'leri](https://msdn.microsoft.com/library/mt613507.aspx). Yönetici olarak oturum açma şöyledir:

1. Kullanım `Login-AzureRmAccount` cmdlet'ini oturum açın.
2. Kullanarak hizmet bulunduğundan abonelik bağlamını ayarlayın `Set-AzureRmContext -SubscriptionID <subscriptionGUID>`.
3. Kullanarak tek bir oturum açma URL'sini alma `Get-AzureRmApiManagementSsoToken -ResourceGroupName <rgName> -Name <serviceName>`.
4. Yönetim Portalı'na erişmek için URL'yi kullanın.

### <a name="why-is-the-policy-that-i-want-to-add-unavailable-in-the-policy-editor"></a>Neden ilke düzenleyicisinde kullanılamaz eklemek istediğiniz ilke mi?
Eklemek istediğiniz ilke soluk veya İlke Düzenleyicisi'nde gölgeli, ilke için doğru kapsamında olduğundan emin olun görünüyorsa. Her ilke bildirimi belirli kapsamlar ve ilke bölümlerde kullanmanız için tasarlanmıştır. İlke bölüm ve bir ilke kapsamları gözden geçirmek için ilkenin kullanım bölümüne bakın. [API Management ilkeleri](https://msdn.microsoft.com/library/azure/dn894080.aspx).

### <a name="how-do-i-use-api-versioning-in-api-management"></a>API sürümü oluşturma API Management'te nasıl kullanabilirim?
API sürümü oluşturma API Management'te kullanmak için birkaç seçeneğiniz vardır:

* API Yönetimi'nde, API'ları farklı sürümlerini temsil yapılandırabilirsiniz. Örneğin, iki farklı API'leri, MyAPIv1 ve MyAPIv2 olabilir. Bir geliştirici Geliştirici kullanmak isterse sürümü seçebilirsiniz.
* API'nizi sürüm segment, örneğin, https://my.api içermeyen bir hizmet URL'si ile de yapılandırabilirsiniz. Ardından, her işlemin üzerinde bir sürümü kesimi yapılandırmak [URL yeniden yazma](https://msdn.microsoft.com/library/azure/dn894083.aspx#RewriteURL) şablonu. Örneğin, bir işlem olabilir bir [URL şablonu](api-management-howto-add-operations.md#url-template) /Resource çağrılır ve bir [URL yeniden yazma](api-management-howto-add-operations.md#rewrite-url-template) şablonu adlı/v1/kaynak. Her işlem için ayrı ayrı sürüm kesim değeri değiştirebilirsiniz.
* Seçili işlemlerini API'nin hizmeti URL'si "varsayılan" sürüm kesimi tutmak istiyorsanız kullanan bir ilke kümesi [ayarlamak arka uç hizmetini](https://msdn.microsoft.com/library/azure/dn894083.aspx#SetBackendService) arka uç istek yolu değiştirmek için ilke.

### <a name="how-do-i-set-up-multiple-environments-in-a-single-api"></a>Tek bir API birden çok ortamında nasıl ayarlayabilirim?
Birden çok ortamı, örneğin, bir test ortamı ve bir üretim ortamında, tek bir API ayarlamak için iki seçeneğiniz vardır. Şunları yapabilirsiniz:

* Ana bilgisayar üzerinde aynı Kiracı farklı API'leri.
* Farklı kiracıların aynı API'leri barındırır.

### <a name="can-i-use-soap-with-api-management"></a>API Management ile SOAP kullanabilir miyim?
[SOAP doğrudan](http://blogs.msdn.microsoft.com/apimanagement/2016/10/13/soap-pass-through/) desteği artık kullanılabilir durumdadır. Yöneticiler, SOAP hizmetini WSDL içe aktarabilir ve Azure API Management SOAP ön uç oluşturacak. Geliştirici portal belgeleri, test konsol, ilkeleri ve analizi SOAP Hizmetleri için kullanılabilir.

### <a name="is-the-api-management-gateway-ip-address-constant-can-i-use-it-in-firewall-rules"></a>API Yönetimi ağ geçidi IP adresi sabit mi? Bu güvenlik duvarı kurallarında kullanabilir miyim?
Standart ve Premium katmanlar, API Management Kiracı ortak IP adresi (VIP) Kiracı bazı özel durumlar ile ömrü statik içindir. Bu durumlarda IP adresi değişiklikleri:

* Hizmet silinir ve yeniden oluşturulacak.
* Hizmet aboneliği [askıya](https://github.com/Azure/azure-resource-manager-rpc/blob/master/v1.0/subscription-lifecycle-api-reference.md#subscription-states) veya [uyarı](https://github.com/Azure/azure-resource-manager-rpc/blob/master/v1.0/subscription-lifecycle-api-reference.md#subscription-states) (örneğin, nonpayment) ve ardından reinstated.
* Azure sanal ağı ekleyip (yalnızca Geliştirici sanal ağ kullanabilir ve Premium katmanı).

Bölgeli dağıtımlar için bölgesel adresi değişirse bölge vacated ve ardından reinstated (yalnızca Premium katmanı çok bölge dağıtımı kullanabilirsiniz).

Bölgeli dağıtımı için yapılandırılmış premium katmanı kiracılar her bölge bir genel IP adresi atanır.

Azure portalında Kiracı sayfasında, IP adresi (veya adresleri, bölgeli dağıtım) alabilirsiniz.

### <a name="can-i-configure-an-oauth-20-authorization-server-with-ad-fs-security"></a>Bir OAuth 2.0 yetkilendirme sunucusu ile AD FS güvenliği yapılandırabilir miyim?
Bir OAuth 2.0 yetkilendirme sunucusu ile Active Directory Federasyon Hizmetleri (AD FS) güvenliği yapılandırma konusunda bilgi edinmek için [kullanarak ADFS API Management](https://phvbaars.wordpress.com/2016/02/06/using-adfs-in-api-management/).

### <a name="what-routing-method-does-api-management-use-in-deployments-to-multiple-geographic-locations"></a>Hangi yönlendirme yöntemini API Management birden çok coğrafi konumlara dağıtımlarında kullanıyor mu?
API Management kullanır [performans trafik yönlendirme yöntemini](../traffic-manager/traffic-manager-routing-methods.md#priority) birden çok coğrafi konumlara dağıtımlarında. Gelen trafik için en yakın API ağ geçidi yönlendirilir. Bir bölge çevrimdışı olursa, gelen trafik için sonraki en yakın ağ geçidi otomatik olarak yönlendirilir. Yönlendirme yöntemleri hakkında daha fazla bilgi [Traffic Manager yönlendirme yöntemleri](../traffic-manager/traffic-manager-routing-methods.md).

### <a name="can-i-use-an-azure-resource-manager-template-to-create-an-api-management-service-instance"></a>API Management hizmet örneği oluşturmak için bir Azure Resource Manager şablonu kullanabilir miyim?
Evet. Bkz: [Azure API Management hizmeti](http://aka.ms/apimtemplate) hızlı başlangıç şablonları.

### <a name="can-i-use-a-self-signed-ssl-certificate-for-a-back-end"></a>Otomatik olarak imzalanan bir SSL sertifikası bir arka uç için kullanabilir miyim?
Evet. Bu, PowerShell üzerinden veya doğrudan API için gönderme tarafından yapılabilir. Bu sertifika zinciri doğrulamasını devre dışı bırakır ve otomatik olarak imzalanan veya özel olarak imzalanan sertifikaları arka uç hizmetlerini API Yönetimi'nden iletişim kurarken kullanması olanak sağlar.

#### <a name="powershell-method"></a>PowerShell yöntemi ####
Kullanım [ `New-AzureRmApiManagementBackend` ](https://docs.microsoft.com/en-us/powershell/module/azurerm.apimanagement/new-azurermapimanagementbackend) (için yeni arka uç) veya [ `Set-AzureRmApiManagementBackend` ](https://docs.microsoft.com/en-us/powershell/module/azurerm.apimanagement/set-azurermapimanagementbackend) (için varolan arka uç) PowerShell cmdlet'lerini ve `-SkipCertificateChainValidation` parametresi `True`. 

```
$context = New-AzureRmApiManagementContext -resourcegroup 'ContosoResourceGroup' -servicename 'ContosoAPIMService'
New-AzureRmApiManagementBackend -Context  $context -Url 'https://contoso.com/myapi' -Protocol http -SkipCertificateChainValidation $true
```

#### <a name="direct-api-update-method"></a>Doğrudan API güncelleştirme yöntemi ####
1. Oluşturma bir [arka uç](https://msdn.microsoft.com/library/azure/dn935030.aspx) API Management kullanarak varlık.       
2. Ayarlama **skipCertificateChainValidation** özelliğine **doğru**.     
3. Artık otomatik olarak imzalanan sertifikalar izin vermek istiyorsanız, arka uç varlık silin veya ayarlayın **skipCertificateChainValidation** özelliğine **false**.

### <a name="why-do-i-get-an-authentication-failure-when-i-try-to-clone-a-git-repository"></a>Bir Git deposuna kopyalamak çalıştığımda neden bir kimlik doğrulama hatası alabilir?
Git kimlik bilgisi Yöneticisi'ni kullanın ya da Visual Studio kullanarak bir Git deposuna kopyalamak çalışıyorsanız, Windows kimlik bilgileri iletişim kutusu ile ilgili bilinen bir sorun içine çalışabilir. Parola uzunluğu 127 karakter olarak iletişim kutusunda sınırlar ve Microsoft tarafından oluşturulan parola tamsayıya dönüştürür. Parola kısaltmayı üzerinde çalışıyoruz. Şimdilik, Git Bash Git deponuzu kopyalamak için lütfen kullanın.

### <a name="does-api-management-work-with-azure-expressroute"></a>API Management Azure ExpressRoute ile çalışır mı?
Evet. API Management Azure ExpressRoute ile çalışır.

### <a name="why-do-we-require-a-dedicated-subnet-in-resource-manager-style-vnets-when-api-management-is-deployed-into-them"></a>Neden API Management içine dağıtıldığında biz sanal ağlar Resource Manager stilde ayrılmış bir alt ağ gerektiriyor mu?
API Management ayrılmış bir alt ağ gereksinimini (PAAS V1 katman) Klasik dağıtım modelinde oluşturulmuş olgu, gelir. Resource Manager VNET'i (V2 katman) dağıtabilmeniz için karşın, o sonuçları vardır. Bir kaynak V2 katmanda oluşturursanız Azure Klasik dağıtım modelinde sıkı bir şekilde Resource Manager modeli ve bunu eşleşmediğinden, V1 katman hakkında bilmiyor ve API (V2 üzerinde oluşturulur) bir NIC önceden ayrılmış bir IP'nin çalışılırken yönetimi gibi sorunlar oluşabilir.
Bilgi edinmek için Azure Klasik ve Resource Manager modellerin fark hakkında daha fazla başvurmak [dağıtım modelleri arasında fark](../azure-resource-manager/resource-manager-deployment-model.md).

### <a name="what-is-the-minimum-subnet-size-needed-when-deploying-api-management-into-a-vnet"></a>API Management bir VNET dağıtırken gereken en düşük alt ağ boyutu nedir?
API Management dağıtmak için gereken en düşük alt ağ boyutu [/29](../virtual-network/virtual-networks-faq.md#configuration), Azure desteklediği en düşük alt ağ boyutunu olduğu.

### <a name="can-i-move-an-api-management-service-from-one-subscription-to-another"></a>I bir API Management hizmeti bir abonelikten diğerine taşıyabilir miyim?
Evet. Bilgi edinmek için bkz [bir yeni kaynak grubu veya abonelik kaynaklarını taşıma](../azure-resource-manager/resource-group-move-resources.md).

### <a name="are-there-restrictions-on-or-known-issues-with-importing-my-api"></a>Kısıtlamalar veya my API içeri aktarma bilinen sorunlar vardır?
[Bilinen sorunlar ve kısıtlamalar](api-management-api-import-restrictions.md) açık API(Swagger) için WSDL ve WADL biçimlendirir.
