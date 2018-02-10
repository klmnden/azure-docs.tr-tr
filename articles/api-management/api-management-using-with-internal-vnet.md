---
title: "Azure API Management iç sanal ağlar ile kullanma | Microsoft Docs"
description: "Ayarlama ve bir iç sanal ağ üzerinde Azure API Management yapılandırma hakkında bilgi edinin"
services: api-management
documentationcenter: 
author: vladvino
manager: kjoshi
editor: 
ms.assetid: dac28ccf-2550-45a5-89cf-192d87369bc3
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/29/2017
ms.author: apimpm
ms.openlocfilehash: cf062cfcbbb2454adf20a06c31c81a60f6f5719f
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="using-azure-api-management-service-with-an-internal-virtual-network"></a>Azure API Management hizmeti bir iç sanal ağ ile kullanma
Azure sanal ağları ile Azure API Management API'leri Internet üzerinden erişilebilir yönetebilirsiniz. VPN teknolojileri çeşitli bağlantı kurmak kullanılabilir. API Management, iki ana modda bir sanal ağ içinde dağıtılabilir:
* Dış
* İç


API Management iç sanal ağ modunda dağıtırken, tüm hizmet uç noktalarına (ağ geçidi, Geliştirici Portalı, Azure portal, doğrudan yönetim ve Git) yalnızca erişimini denetleyen bir sanal ağ içinde görünür. Hizmet uç noktalarına hiçbiri genel DNS sunucusunda kayıtlı.

İç modunda API Yönetimi'ni kullanarak aşağıdaki senaryolar elde edebilirsiniz:
* Siteden siteye veya Azure ExpressRoute VPN bağlantıları kullanarak, özel veri merkezinizde güvenli bir şekilde erişilebilir dışında üçüncü taraflar tarafından barındırılan API'ler olun.
* Karma bulut senaryolarında, bulut tabanlı API'ler ve ortak ağ geçidi üzerinden şirket içi API'leri göstererek etkinleştirin.
* Bir tek ağ geçidi uç noktası kullanarak birden çok coğrafi konumda barındırılan Apı'lerinizi yönetin. 


## <a name="prerequisites"></a>Önkoşullar

Bu makalede açıklanan adımları gerçekleştirmek için şunlara sahip olmalısınız:

+ **Etkin bir Azure aboneliği**.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

+ **Azure API Management örneği**. Daha fazla bilgi için bkz: [bir Azure API Management örneği oluşturma](get-started-create-service-instance.md).

## <a name="enable-vpn"></a>Bir API Management bir iç sanal ağ oluşturma
API Management hizmeti bir iç sanal ağ içinde bir iç yük dengeleyici (ILB) barındırılır.

### <a name="enable-a-virtual-network-connection-using-the-azure-portal"></a>Azure Portalı'nı kullanarak bir sanal ağ bağlantısını etkinleştir

1. Azure API Management Örneğiniz için Gözat [Azure portal](https://portal.azure.com/).
2. Seçin **sanal ağ**.
3. API Management örneği sanal ağında dağıtılacak şekilde yapılandırın.

    ![Bir iç sanal ağ bir Azure API Management'te ayarlamak için menüsü][api-management-using-internal-vnet-menu]

4. **Kaydet**’i seçin.

Dağıtım başarılı olduktan sonra Panoda hizmetinizin iç sanal IP adresi görmeniz gerekir.

![API Yönetim Panosu ile yapılandırılmış bir iç sanal ağ][api-management-internal-vnet-dashboard]

### <a name="enable-a-virtual-network-connection-by-using-powershell-cmdlets"></a>PowerShell cmdlet'lerini kullanarak bir sanal ağ bağlantısını etkinleştir
PowerShell cmdlet'lerini kullanarak sanal ağ bağlantısı daha da etkinleştirebilirsiniz.

* Bir sanal ağ içinde bir API Management hizmeti oluşturma: cmdlet'ini kullanın [yeni AzureRmApiManagement](/powershell/module/azurerm.apimanagement/new-azurermapimanagement) bir sanal ağ içinde bir Azure API Management hizmeti oluşturma ve iç sanal ağ türü kullanacak şekilde yapılandırmak için.

* Bir sanal ağ içinde varolan bir API Management hizmetini dağıtma: cmdlet'ini kullanın [güncelleştirme AzureRmApiManagementDeployment](/powershell/module/azurerm.apimanagement/update-azurermapimanagementdeployment) bir API Management hizmetiniz bir sanal ağ içinde taşımak ve iç kullanacak şekilde yapılandırmak için sanal ağ türü.

## <a name="apim-dns-configuration"></a>DNS yapılandırması
API Management dış sanal ağ modunda olduğunda DNS Azure tarafından yönetilir. İç sanal ağ modu için kendi yönlendirme yönetmeniz gerekir.

> [!NOTE]
> API Management hizmeti, IP adreslerinden gelen isteklerini dinlemez. Bunu yalnızca kendi hizmet uç noktaları üzerinde yapılandırılmış ana bilgisayar adı isteklerine yanıt verir. Bu uç noktaları, ağ geçidi, Geliştirici Portalı, Azurethe portal, doğrudan yönetim uç noktası ve Git içerir.

### <a name="access-on-default-host-names"></a>Varsayılan ana bilgisayar adlarını erişimi
Örneğin, "contoso" adlı bir API Management hizmeti oluşturduğunuzda, aşağıdaki hizmet uç noktaları varsayılan olarak yapılandırılır:

   * Ağ geçidi veya proxy: contoso.azure api.net

   * Azure portalı ve Geliştirici Portalı: contoso.portal.azure api.net

   * Doğrudan yönetim uç noktası: contoso.management.azure api.net

   * Git: contoso.scm.azure-api.net

Bu API Management hizmet uç noktalarına erişmek için API Management dağıtıldığı sanal ağa bağlı bir alt ağda bir sanal makine oluşturabilirsiniz. Hizmetiniz için iç sanal IP adresi 10.0.0.5 olduğunu varsayarak, ana bilgisayarlar dosyası, % SystemDrive%\drivers\etc\hosts, aşağıdaki gibi eşleyebilirsiniz:

   * 10.0.0.5     contoso.azure-api.net

   * 10.0.0.5     contoso.portal.azure-api.net

   * 10.0.0.5     contoso.management.azure-api.net

   * 10.0.0.5     contoso.scm.azure-api.net

Daha sonra oluşturduğunuz sanal makineden tüm hizmet uç noktalarına erişebilirsiniz. Sanal bir ağa özel bir DNS sunucusu kullanıyorsanız, ayrıca bir DNS kayıtları oluşturmak ve bu uç noktaların her yerden erişim sanal ağınızda. 

### <a name="access-on-custom-domain-names"></a>Özel etki alanı adlarını erişimi

   1. Varsayılan ana bilgisayar adlarıyla API Management hizmetine erişmek istemiyorsanız, aşağıdaki görüntüde gösterildiği gibi tüm, hizmet uç noktaları için özel etki alanı adlarını ayarlayabilirsiniz: 

   ![API Management için özel bir etki alanı ayarlama][api-management-custom-domain-name]

   2. Ardından, yalnızca sanal ağ içindeki erişilebilir uç noktaları erişmek için DNS sunucunuzun kayıtları oluşturabilirsiniz.

## <a name="routing"></a> Yönlendirme
+ Yük dengeli özel bir sanal IP adresi alt ağı aralığından ayrılmış ve sanal ağ içinden API Management hizmet uç noktalarından erişmek için kullanılır.
+ Bir yük dengeli ortak IP adresi (VIP), yalnızca bağlantı noktası üzerinden 3443 Yönetim Hizmeti uç noktası erişim sağlamak için de ayrılacak.
+ Bir alt ağ IP aralığı (DIP) bir IP adresinden vnet içindeki kaynaklara erişmek için kullanılan ve bir genel IP adresi (VIP) sanal ağ dışında kaynaklara erişmek için kullanılır.
+ Yük dengeli ortak ve özel IP adresleri genel bakış/Essentials dikey penceresinde Azure portalında bulunabilir.

## <a name="related-content"></a>İlgili içerik
Daha fazla bilgi için aşağıdaki makalelere bakın:
* [Sanal bir ağa Azure API Management ayarlanıyor sırasında ortak ağ yapılandırma sorunları][Common network configuration problems]
* [Sanal ağ ile ilgili SSS](../virtual-network/virtual-networks-faq.md)
* [DNS'de bir kayıt oluşturma](https://msdn.microsoft.com/en-us/library/bb727018.aspx)

[api-management-using-internal-vnet-menu]: ./media/api-management-using-with-internal-vnet/api-management-internal-vnet-menu.png
[api-management-internal-vnet-dashboard]: ./media/api-management-using-with-internal-vnet/api-management-internal-vnet-dashboard.png
[api-management-custom-domain-name]: ./media/api-management-using-with-internal-vnet/api-management-custom-domain-name.png

[Create API Management service]: get-started-create-service-instance.md
[Common network configuration problems]: api-management-using-with-vnet.md#network-configuration-issues

