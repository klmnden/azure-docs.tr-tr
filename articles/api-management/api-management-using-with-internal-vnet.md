---
title: İç sanal ağlar ile Azure API Management'ı kullanma | Microsoft Docs
description: Ayarlama ve Azure API Management'ı bir iç sanal ağ yapılandırma hakkında bilgi edinin
services: api-management
documentationcenter: ''
author: vladvino
manager: kjoshi
editor: ''
ms.assetid: dac28ccf-2550-45a5-89cf-192d87369bc3
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/11/2019
ms.author: apimpm
ms.openlocfilehash: 7db40de921c0eb8826a2fee832c1a51c57796f6d
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64919830"
---
# <a name="using-azure-api-management-service-with-an-internal-virtual-network"></a>Azure API Management hizmeti bir iç sanal ağ ile kullanma
Azure sanal ağlar ile Azure API Management API'leri değil internet üzerinden erişilebilen yönetebilirsiniz. VPN'si teknolojileri birkaç bağlantı kurmak kullanılabilir. API Management, iki ana modda bir sanal ağ içinde dağıtılabilir:
* Dış
* İç

API yönetimi, dahili sanal ağ modunda dağıttığında, tüm hizmet uç noktaları (ağ geçidi, Geliştirici Portalı, Azure portalı, doğrudan yönetim ve Git) yalnızca erişimini denetleyen bir sanal ağ içinde görülebilir. Hizmet uç noktalarını hiçbiri genel DNS sunucusunda kayıtlı.

API Management, iç modda kullanma, aşağıdaki senaryoları elde edebilirsiniz:

* Siteden siteye veya Azure ExpressRoute VPN bağlantıları kullanarak özel veri merkezinizde güvenli bir şekilde erişmesini dışındaki üçüncü taraflarca barındırılan API'ler olun.
* Karma bulut senaryolarında, bulut tabanlı API'ler ve ortak bir ağ geçidi üzerinden şirket içi API'ler göstererek etkinleştirin.
* Apı'lerinizi tek bir ağ geçidi uç noktası kullanarak birden çok coğrafi bölgelerde barındırılan yönetin.

[!INCLUDE [premium-dev.md](../../includes/api-management-availability-premium-dev.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu makalede açıklanan adımları gerçekleştirmek için aşağıdakiler gerekir:

+ **Etkin bir Azure aboneliğine**.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

+ **Azure API Management örneği**. Daha fazla bilgi için [Azure API Management örneği oluşturma](get-started-create-service-instance.md).
+ Bir sanal ağ içinde bir API Management hizmet dağıtıldığında bir [bağlantı noktalarının listesi](./api-management-using-with-vnet.md#required-ports) kullanılır ve açılması gerekir. 

## <a name="enable-vpn"> </a>API yönetimi bir iç sanal ağ oluşturma
API Management hizmeti bir iç sanal ağ içinde arkasında barındırılan bir [iç yük dengeleyici (Klasik)](https://docs.microsoft.com/azure/load-balancer/load-balancer-get-started-ilb-classic-cloud). Bu, kullanılabilecek tek seçenek budur ve değiştirilemez.

### <a name="enable-a-virtual-network-connection-using-the-azure-portal"></a>Azure portalını kullanarak bir sanal ağ bağlantısını etkinleştirme

1. Azure API Management Örneğinize göz atın [Azure portalında](https://portal.azure.com/).
2. **Sanal ağ**'ı seçin.
3. Sanal ağ içinde dağıtılmak üzere API Management örneği yapılandırın.

    ![Bir Azure API Yönetimi'nde bir iç sanal ağ ayarlama menüsü][api-management-using-internal-vnet-menu]

4. **Kaydet**’i seçin.

Dağıtım, görmelisiniz başarılı olduktan sonra **özel** sanal IP adresi ve **genel** API Yönetimi hizmetiniz genel bakış dikey penceresinde, sanal IP adresi. **Özel** sanal IP adresine sahip yük API Management dengeli IP adresi temsilci alt ağ üzerinde `gateway`, `portal`, `management` ve `scm` uç noktaları erişilebilir. **Genel** sanal IP adresi kullanılır **yalnızca** denetim düzlemi trafiği için `management` uç nokta üzerinde bağlantı noktası 3443 ve aşağı kilitli [ApiManagement] [ ServiceTags] servicetag.

![API Yönetimi panosu ile yapılandırılmış bir iç sanal ağ][api-management-internal-vnet-dashboard]

> [!NOTE]
> Azure portalında kullanılabilir Test Konsolu çalışmaz **dahili** VNET dağıtılan hizmet, ağ geçidi URL'si Genel DNS kayıtlı değil. Sağlanan Test Konsolu kullanmalısınız **Geliştirici Portalı**.

### <a name="enable-a-virtual-network-connection-by-using-powershell-cmdlets"></a>PowerShell cmdlet'lerini kullanarak bir sanal ağ bağlantısını etkinleştirme

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

PowerShell cmdlet'lerini kullanarak, sanal ağ bağlantısı da etkinleştirebilirsiniz.

* Bir sanal ağ içinde bir API Management hizmeti oluşturun: Cmdlet'i kullanmak [yeni AzApiManagement](/powershell/module/az.apimanagement/new-azapimanagement) bir sanal ağ içinde bir Azure API Management hizmeti oluşturma ve iç sanal ağ türü kullanacak şekilde yapılandırın.

* Var olan bir dağıtımının bir sanal ağ içinde bir API Management hizmeti güncelleştirin: Cmdlet'i kullanmak [güncelleştirme AzApiManagementRegion](/powershell/module/az.apimanagement/update-azapimanagementregion) bir API Yönetimi hizmetiniz bir sanal ağ içinde hareket ve iç sanal ağ türü kullanacak şekilde yapılandırın.

## <a name="apim-dns-configuration"></a>DNS yapılandırması
API Management, dış sanal ağ modunda olduğunda, DNS, Azure tarafından yönetilir. İç sanal ağ modu için kendi yönlendirme yönetmek zorunda.

> [!NOTE]
> API Management hizmeti, IP adreslerinden mi geliyor isteklerini dinlemez. Bu yalnızca, hizmet uç noktaları üzerinde yapılandırılan ana bilgisayar adı için isteklere yanıt verir. Bu uç noktaları, ağ geçidi, Azure portalı ve Geliştirici Portalı, doğrudan yönetim uç noktası ve Git içerir.

### <a name="access-on-default-host-names"></a>Varsayılan ana bilgisayar adları üzerinde erişim
Örneğin, "contosointernalvnet" adlı API Management hizmeti,'ı oluşturduğunuzda, aşağıdaki hizmet uç noktaları varsayılan olarak yapılandırılır:

   * Ağ geçidi veya proxy: contosointernalvnet.azure-api.net

   * Azure portalı ve Geliştirici Portalı: contosointernalvnet.portal.azure-api.net

   * Doğrudan yönetim uç noktası: contosointernalvnet.management.azure-api.net

   * Git: contosointernalvnet.scm.azure-api.net

Bu API Management hizmet uç noktalarına erişmek için API Management dağıtıldığı sanal ağa bağlı bir alt ağdaki bir sanal makine oluşturabilirsiniz. Hizmetiniz için iç sanal IP adresi 10.1.0.5 olduğu varsayıldığında, ana bilgisayarlar dosyası, % SystemDrive%\drivers\etc\hosts, şu şekilde eşleyebilirsiniz:

   * 10.1.0.5 contosointernalvnet.azure-api.net

   * 10.1.0.5 contosointernalvnet.portal.azure-api.net

   * 10.1.0.5     contosointernalvnet.management.azure-api.net

   * 10.1.0.5     contosointernalvnet.scm.azure-api.net

Tüm hizmet uç noktaları, oluşturduğunuz sanal makineden erişebilirsiniz.
Bir sanal ağda özel DNS sunucusu kullanıyorsanız, ayrıca bir DNS kayıtları oluşturmak ve bu uç noktalar her yerden erişim sanal ağınızda.

### <a name="access-on-custom-domain-names"></a>Özel etki alanı adları hakkında daha fazla erişim

1. API Management hizmeti ile varsayılan konak adları erişmek istemiyorsanız, aşağıdaki görüntüde gösterildiği gibi tüm hizmet uç noktalarınıza için özel etki alanı adlarını ayarlayabilirsiniz:

   ![API Management için özel bir etki alanı ayarlama][api-management-custom-domain-name]

2. Ardından, yalnızca sanal ağınızdaki erişilebilir uç noktalarına erişmek için DNS sunucunuzun kayıtlarını oluşturabilirsiniz.

## <a name="routing"> </a> Yönlendirme
+ Yük dengeli özel bir sanal IP adresi alt ağı aralığından ayrılmış ve sanal ağ içindeki API Management hizmet uç noktalarından erişmek için kullanılır.
+ Yük dengeli genel IP adresi (VIP), Yönetim Hizmeti uç noktasına erişim yalnızca bağlantı noktası üzerinden 3443 sağlamak için ayrıca ayrılacaktır.
+ Bir alt ağ IP aralığı (DIP) ağdan bir IP adresi, sanal ağ içindeki kaynaklara erişmek için kullanılacak ve sanal ağ dışındaki kaynaklara erişmek için genel bir IP adresi (VIP) kullanılır.
+ Genel yük dengeli ve özel IP adresleri, Azure portalında genel bakış/Essentials dikey penceresinde bulunabilir.

## <a name="related-content"> </a>İlgili içerik
Daha fazla bilgi için aşağıdaki makalelere bakın:
* [Bir sanal ağda Azure API Management ayarlanıyor sırasında ortak ağ yapılandırma sorunları][Common network configuration problems]
* [Sanal ağ hakkında SSS](../virtual-network/virtual-networks-faq.md)
* [DNS'de bir kaydı oluşturma](/previous-versions/windows/it-pro/windows-2000-server/bb727018(v=technet.10))

[api-management-using-internal-vnet-menu]: ./media/api-management-using-with-internal-vnet/api-management-using-with-internal-vnet.png
[api-management-internal-vnet-dashboard]: ./media/api-management-using-with-internal-vnet/api-management-internal-vnet-dashboard.png
[api-management-custom-domain-name]: ./media/api-management-using-with-internal-vnet/api-management-custom-domain-name.png

[Create API Management service]: get-started-create-service-instance.md
[Common network configuration problems]: api-management-using-with-vnet.md#network-configuration-issues

[ServiceTags]: ../virtual-network/security-overview.md#service-tags

