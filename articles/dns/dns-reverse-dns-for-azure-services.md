---
title: Azure Hizmetleri için ters DNS | Microsoft Docs
description: Azure'da barındırılan hizmetleri için ters DNS Arama yapılandırma hakkında bilgi edinin
services: dns
documentationcenter: na
author: vhorne
manager: timlt
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/29/2017
ms.author: victorh
ms.openlocfilehash: e162d838cb4895841428a827b56bec28e3e16b8a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66160922"
---
# <a name="configure-reverse-dns-for-services-hosted-in-azure"></a>Azure'da barındırılan hizmetleri için ters DNS yapılandırma

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu makalede, Azure'da barındırılan hizmetleri için ters DNS Arama yapılandırma açıklanmaktadır.

Azure hizmetlerinde, Azure tarafından atanan ve Microsoft'a ait IP adresleri kullanın. Bu ters DNS kayıtlarını (PTR kayıtları) karşılık gelen Microsoft'a ait ters DNS arama bölgeleri içinde oluşturulmuş olması gerekir. Bu makalede bunun nasıl yapılacağı açıklanmaktadır.

Bu senaryo özelliği ile karıştırılmamalıdır [, atanan IP aralıkları Azure DNS'de ters DNS arama bölgeleri barındırma](dns-reverse-dns-hosting.md). Bu durumda, geriye doğru arama bölgesi tarafından temsil edilen IP aralıklarını kuruluşunuz için genellikle ISS'niz tarafından atanmış olması gerekir.

Bu makalede okumadan önce bu bilgi sahibi olmanız [geriye doğru DNS ve Azure desteği'na genel bakış](dns-reverse-dns-overview.md).

Azure DNS'de işlem kaynakları (örneğin, sanal makineler, sanal makine ölçek kümeleri veya Service Fabric kümeleri) Publicıpaddress kaynağı aracılığıyla kullanıma sunulur. Ters DNS araması Publicıpaddress 'ReverseFqdn' özelliği kullanılarak yapılandırılır.


Azure App Service için ters DNS şu anda desteklenmiyor.

## <a name="validation-of-reverse-dns-records"></a>Ters DNS kayıtlarını doğrulama

Bir üçüncü taraf kendi DNS etki alanlarınızı Azure hizmet eşleme için ters DNS kayıtlarını oluşturmak mümkün olmaması gerekir. Bunu önlemek için Azure yalnızca geriye doğru DNS kaydında belirtilen etki alanı adı ile aynı olduğu geriye doğru DNS kaydı oluşturulmasına izin verir veya DNS adı veya IP adresi Publicıpaddress veya Bulut hizmeti aynı Azure aboneliğinde giderir.

Ters DNS kaydı ayarlayın veya değiştirildiğinde, bu doğrulama yalnızca gerçekleştirilir. Düzenli aralıklarla yeniden doğrulama yapılmaz.

Örneğin: Publicıpaddress kaynak IP adresi 23.96.52.53 ve DNS adı contosoapp1.northus.cloudapp.azure.com sahip olduğunu varsayın. ReverseFqdn için Publicıpaddress olarak belirtilebilir:
* Publicıpaddress için DNS adını contosoapp1.northus.cloudapp.azure.com
* Aynı abonelikte contosoapp2.westus.cloudapp.azure.com gibi farklı bir Publicıpaddress için DNS adı
* Bu ad olduğu sürece bir gösterim DNS, app1.contoso.com gibi ad *ilk* bir CNAME contosoapp1.northus.cloudapp.azure.com ya da aynı Abonelikteki farklı bir Publicıpaddress olarak yapılandırılmış.
* Bu ad olduğu sürece bir gösterim DNS, app1.contoso.com gibi ad *ilk* bir A kaydı 23.96.52.53 IP adresine veya IP adresiyle aynı Abonelikteki farklı bir Publicıpaddress olarak yapılandırılmış.

Bulut Hizmetleri için ters DNS aynı kısıtlamalar uygulanır.


## <a name="reverse-dns-for-publicipaddress-resources"></a>PublicIpAddress kaynaklarını için ters DNS

Bu bölümde, Azure PowerShell, Azure Klasik CLI veya Azure CLI kullanarak Resource Manager dağıtım modelinde PublicIpAddress kaynaklarını için ters DNS yapılandırma hakkında ayrıntılı yönergeler sağlar. PublicIpAddress kaynaklarını için ters DNS yapılandırma, Azure portalı üzerinden şu anda desteklenmiyor.

Azure şu anda desteklediği yalnızca IPv4 PublicIpAddress kaynaklarını için ters DNS. IPv6 için desteklenmiyor.

### <a name="add-reverse-dns-to-an-existing-publicipaddresses"></a>Mevcut bir Publicıpaddresses için ters DNS Ekle

#### <a name="powershell"></a>PowerShell

Mevcut bir Publicıpaddress'e ters DNS eklemek için:

```powershell
$pip = Get-AzPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
$pip.DnsSettings.ReverseFqdn = "contosoapp1.westus.cloudapp.azure.com."
Set-AzPublicIpAddress -PublicIpAddress $pip
```

Ters DNS'yi bir DNS adı zaten sahip olmayan var olan bir Publicıpaddress'e eklemek için de bir DNS adı belirtmeniz gerekir:

```powershell
$pip = Get-AzPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
$pip.DnsSettings = New-Object -TypeName "Microsoft.Azure.Commands.Network.Models.PSPublicIpAddressDnsSettings"
$pip.DnsSettings.DomainNameLabel = "contosoapp1"
$pip.DnsSettings.ReverseFqdn = "contosoapp1.westus.cloudapp.azure.com."
Set-AzPublicIpAddress -PublicIpAddress $pip
```

#### <a name="azure-classic-cli"></a>Azure klasik CLI

Mevcut bir Publicıpaddress'e ters DNS eklemek için:

```azurecli
azure network public-ip set -n PublicIp -g MyResourceGroup -f contosoapp1.westus.cloudapp.azure.com.
```

Ters DNS'yi bir DNS adı zaten sahip olmayan var olan bir Publicıpaddress'e eklemek için de bir DNS adı belirtmeniz gerekir:

```azurecli
azure network public-ip set -n PublicIp -g MyResourceGroup -d contosoapp1 -f contosoapp1.westus.cloudapp.azure.com.
```

#### <a name="azure-cli"></a>Azure CLI'si

Mevcut bir Publicıpaddress'e ters DNS eklemek için:

```azurecli
az network public-ip update --resource-group MyResourceGroup --name PublicIp --reverse-fqdn contosoapp1.westus.cloudapp.azure.com.
```

Ters DNS'yi bir DNS adı zaten sahip olmayan var olan bir Publicıpaddress'e eklemek için de bir DNS adı belirtmeniz gerekir:

```azurecli
az network public-ip update --resource-group MyResourceGroup --name PublicIp --reverse-fqdn contosoapp1.westus.cloudapp.azure.com --dns-name contosoapp1
```

### <a name="create-a-public-ip-address-with-reverse-dns"></a>Ters DNS'ye genel bir IP adresi oluşturma

Ters DNS özelliği zaten belirtilen yeni Publicıpaddress oluşturmak için:

#### <a name="powershell"></a>PowerShell

```powershell
New-AzPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup" -Location "WestUS" -AllocationMethod Dynamic -DomainNameLabel "contosoapp2" -ReverseFqdn "contosoapp2.westus.cloudapp.azure.com."
```

#### <a name="azure-classic-cli"></a>Azure klasik CLI

```azurecli
azure network public-ip create -n PublicIp -g MyResourceGroup -l westus -d contosoapp3 -f contosoapp3.westus.cloudapp.azure.com.
```

#### <a name="azure-cli"></a>Azure CLI'si

```azurecli
az network public-ip create --name PublicIp --resource-group MyResourceGroup --location westcentralus --dns-name contosoapp1 --reverse-fqdn contosoapp1.westcentralus.cloudapp.azure.com
```

### <a name="view-reverse-dns-for-an-existing-publicipaddress"></a>Mevcut bir Publicıpaddress için ters DNS görüntüle

Mevcut bir Publicıpaddress için yapılandırılmış bir değeri görüntülemek için:

#### <a name="powershell"></a>PowerShell

```powershell
Get-AzPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
```

#### <a name="azure-classic-cli"></a>Azure klasik CLI

```azurecli
azure network public-ip show -n PublicIp -g MyResourceGroup
```

#### <a name="azure-cli"></a>Azure CLI'si

```azurecli
az network public-ip show --name PublicIp --resource-group MyResourceGroup
```

### <a name="remove-reverse-dns-from-existing-public-ip-addresses"></a>Ters DNS var olan ortak IP adreslerini kaldırın.

Mevcut bir Publicıpaddress bir ters DNS özelliği kaldırmak için:

#### <a name="powershell"></a>PowerShell

```powershell
$pip = Get-AzPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
$pip.DnsSettings.ReverseFqdn = ""
Set-AzPublicIpAddress -PublicIpAddress $pip
```

#### <a name="azure-classic-cli"></a>Azure klasik CLI

```azurecli
azure network public-ip set -n PublicIp -g MyResourceGroup –f ""
```

#### <a name="azure-cli"></a>Azure CLI'si

```azurecli
az network public-ip update --resource-group MyResourceGroup --name PublicIp --reverse-fqdn ""
```


## <a name="configure-reverse-dns-for-cloud-services"></a>Bulut Hizmetleri için ters DNS yapılandırma

Bu bölümde ayrıntılı Azure PowerShell kullanarak Klasik dağıtım modelinde, Cloud Services için ters DNS yapılandırma hakkında yönergeler sağlar. Bulut Hizmetleri için ters DNS yapılandırma Azure portalında, Klasik Azure CLI veya Azure CLI desteklenmiyor.

### <a name="add-reverse-dns-to-existing-cloud-services"></a>Var olan bulut Hizmetleri için ters DNS Ekle

Ters DNS kaydı var olan bir bulut hizmetine eklemek için:

```powershell
Set-AzureService –ServiceName "contosoapp1" –Description "App1 with Reverse DNS" –ReverseDnsFqdn "contosoapp1.cloudapp.net."
```

### <a name="create-a-cloud-service-with-reverse-dns"></a>Ters DNS ile bulut hizmeti oluşturma

Ters DNS özelliği zaten belirtilen yeni bir bulut hizmeti oluşturmak için:

```powershell
New-AzureService –ServiceName "contosoapp1" –Location "West US" –Description "App1 with Reverse DNS" –ReverseDnsFqdn "contosoapp1.cloudapp.net."
```

### <a name="view-reverse-dns-for-existing-cloud-services"></a>Var olan bulut Hizmetleri için ters DNS görüntüle

Var olan bir bulut hizmeti için ters DNS özelliği görüntülemek için:

```powershell
Get-AzureService "contosoapp1"
```

### <a name="remove-reverse-dns-from-existing-cloud-services"></a>Ters DNS var olan bulut hizmetlerinden Kaldır

Var olan bir bulut hizmetinden bir ters DNS özelliği kaldırmak için:

```powershell
Set-AzureService –ServiceName "contosoapp1" –Description "App1 with Reverse DNS" –ReverseDnsFqdn ""
```

## <a name="faq"></a>SSS

### <a name="how-much-do-reverse-dns-records-cost"></a>DNS kayıtları maliyeti ne kadar ters?

Bu fırsatlar ücretsizdir!  Ters DNS kayıtlarını ya da sorguları için ek ücret yoktur.

### <a name="will-my-reverse-dns-records-resolve-from-the-internet"></a>My ters DNS kayıtlarını internet'ten çözer?

Evet. Azure service için ters DNS özelliği ayarladıktan sonra Azure tüm gerekli tüm Internet kullanıcılar için ters DNS kaydı çözümlendiğinden emin olmak için DNS bölgeleri ve DNS temsilcilerine yönetir.

### <a name="are-default-reverse-dns-records-created-for-my-azure-services"></a>My Azure Hizmetleri için ters DNS kayıtlarını varsayılan oluşturulur?

Hayır. Ters DNS, bir katılım özelliğidir. Bunları yapılandırmak isterseniz varsayılan ters DNS kayıtlarını oluşturulur.

### <a name="what-is-the-format-for-the-fully-qualified-domain-name-fqdn"></a>Tam etki alanı adı (FQDN) biçimi nedir?

FQDN'leri ileriye doğru sırada belirtilir ve bir nokta (örneğin, "app1.contoso.com.") ile bitmelidir.

### <a name="what-happens-if-the-validation-check-for-the-reverse-dns-ive-specified-fails"></a>Başarısız doğrulama denetimi için ters DNS ı belirttiğiniz ne olur?

Ters DNS doğrulama denetimi başarısız olan yerlerde, ters DNS kaydını yapılandırma işlemi başarısız olur. Ters DNS değeri gerekli olarak düzeltin ve yeniden deneyin.

### <a name="can-i-configure-reverse-dns-for-azure-app-service"></a>Azure App Service için ters DNS yapılandırabilir miyim?

Hayır. Ters DNS, Azure App Service için desteklenmiyor.

### <a name="can-i-configure-multiple-reverse-dns-records-for-my-azure-service"></a>Birden çok ters DNS kayıtlarını Azure Hizmetimi yapılandırabilir miyim?

Hayır. Azure, her Azure bulut hizmeti veya Publicıpaddress için tek bir ters DNS kaydı destekler.

### <a name="can-i-configure-reverse-dns-for-ipv6-publicipaddress-resources"></a>IPv6 Publicıpaddress'e kaynaklar için ters DNS yapılandırabilirim?

Hayır. Azure şu anda desteklediği yalnızca IPv4 PublicIpAddress kaynaklarını ve bulut Hizmetleri için ters DNS.

### <a name="can-i-send-emails-to-external-domains-from-my-azure-compute-services"></a>Azure işlem Hizmetlerim e-postaları dış etki alanlarına gönderebilirim?

Teknik bir Azure dağıtımını doğrudan e-posta gönderme olanağı, abonelik türüne göre değişir. Abonelik türü ne olursa olsun, giden posta göndermek için güvenilen posta geçiş hizmetlerini kullanarak Microsoft önerir. Daha fazla ayrıntı için bkz. [göndermek için Azure güvenlik Gelişmiş e-Kasım 2017 güncelleştirmesi](https://blogs.msdn.microsoft.com/mast/2017/11/15/enhanced-azure-security-for-sending-emails-november-2017-update/).

## <a name="next-steps"></a>Sonraki adımlar

Ters DNS hakkında daha fazla bilgi için bkz. [Wikipedia geriye doğru DNS araması](https://en.wikipedia.org/wiki/Reverse_DNS_lookup).
<br>
Bilgi edinmek için nasıl [barındırmak için ISS atanmış IP aralığınızı Azure DNS'de geriye doğru arama bölgesi](dns-reverse-dns-for-azure-services.md).

