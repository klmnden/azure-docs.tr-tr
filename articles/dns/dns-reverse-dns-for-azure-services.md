---
title: "DNS geriye doğru için Azure services | Microsoft Docs"
description: "Geriye doğru DNS araması Azure içinde barındırılan hizmetler için yapılandırma hakkında bilgi edinin"
services: dns
documentationcenter: na
author: jtuliani
manager: timlt
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/29/2017
ms.author: jonatul
ms.openlocfilehash: 63701e1ce0c1c6dcf2ce02ebce272b8280395e7f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="configure-reverse-dns-for-services-hosted-in-azure"></a>Azure üzerinde barındırılan hizmetleri için ters DNS yapılandırma

Bu makalede, Azure üzerinde barındırılan hizmetleri için ters DNS araması yapılandırma açıklanmaktadır.

Azure Hizmetleri Azure tarafından atanan ve Microsoft tarafından ait IP adresleri kullanın. Geriye doğru DNS kayıtlarının (PTR kayıtları) karşılık gelen Microsoft ait geriye doğru DNS arama bölgeleri oluşturulması gerekir. Bu makalede bunun nasıl yapılacağı açıklanmaktadır.

Bu senaryo yeteneği ile karıştırılmamalıdır [Azure DNS'de, atanan IP aralıkları için geriye doğru DNS arama bölgeleri barındıran](dns-reverse-dns-hosting.md). Bu durumda, geriye doğru arama bölgesi tarafından temsil edilen IP aralıkları, kuruluşunuz için genellikle ISS'niz tarafından atanmış olması gerekir.

Bu makalede okumadan önce bu bilgi sahibi olmanız [geriye doğru DNS ve Azure desteği'na genel bakış](dns-reverse-dns-overview.md).

Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../azure-resource-manager/resource-manager-deployment-model.md).
* Resource Manager dağıtım modelinde, işlem kaynakları (örneğin, sanal makineler, sanal makine ölçek kümeleri veya Service Fabric kümeleri) Publicıpaddress kaynak sunulur. Geriye doğru DNS araması Publicıpaddress 'ReverseFqdn' özelliği kullanılarak yapılandırılır.
* Klasik dağıtım modeli, bulut hizmetlerini kullanarak işlem kaynaklarını sunulur. Geriye doğru DNS araması bulut hizmetinin 'ReverseDnsFqdn' özelliği kullanılarak yapılandırılır.

Geriye doğru DNS Azure App Service için şu anda desteklenmiyor.

## <a name="validation-of-reverse-dns-records"></a>Geriye doğru DNS kayıtlarını doğrulama

Bir üçüncü taraf kendi Azure hizmeti eşleme, DNS etki alanı için geriye doğru DNS kayıtları oluşturmak mümkün olmaması gerekir. Bunu önlemek için Azure yalnızca geriye doğru DNS kaydında belirtilen etki alanı adı ile aynı olduğu geriye doğru DNS kaydı oluşturulmasına izin verir veya DNS adı veya IP adresini bir Publicıpaddress veya Bulut hizmeti aynı Azure abonelik giderir.

Bu doğrulama yalnızca geriye doğru DNS kaydı ayarlayın veya değiştirilirken gerçekleştirilir. Düzenli aralıklarla yeniden doğrulama yapılmaz.

Örneğin: Publicıpaddress kaynak IP adresi 23.96.52.53 ve DNS adı contosoapp1.northus.cloudapp.azure.com sahip olduğunu varsayın. Publicıpaddress ReverseFqdn olarak belirtilebilir:
* Publicıpaddress için DNS adını contosoapp1.northus.cloudapp.azure.com
* Aynı abonelikte contosoapp2.westus.cloudapp.azure.com gibi farklı bir Publicıpaddress için DNS adı
* Bu ad olduğu sürece bir gösterim DNS, app1.contoso.com gibi ad *ilk* contosoapp1.northus.cloudapp.azure.com veya farklı bir Publicıpaddress aynı abonelik için bir CNAME olarak yapılandırılmış.
* Bu ad olduğu sürece bir gösterim DNS, app1.contoso.com gibi ad *ilk* 23.96.52.53 IP adresine ya da aynı Abonelikteki farklı bir Publicıpaddress IP adresi için bir A kaydı olarak yapılandırılmış.

Aynı kısıtlamalar DNS bulut Hizmetleri için ters geçerlidir.


## <a name="reverse-dns-for-publicipaddress-resources"></a>Geriye doğru DNS Publicıpaddress kaynaklar için

Bu bölümde Azure PowerShell, Azure CLI 1.0 veya Azure CLI 2.0 kullanarak Resource Manager dağıtım modelinde Publicıpaddress kaynaklar için geriye doğru DNS yapılandırma hakkında ayrıntılı yönergeler sağlar. Geriye doğru DNS Publicıpaddress kaynakları için yapılandırma, Azure portalı üzerinden şu anda desteklenmiyor.

Azure şu anda destekler DNS yalnızca IPv4 Publicıpaddress kaynaklar için ters çevrilir. IPv6 için desteklenmiyor.

### <a name="add-reverse-dns-to-an-existing-publicipaddresses"></a>Varolan bir Publicıpaddresses için geriye doğru DNS ekleyin

#### <a name="powershell"></a>PowerShell

Geriye doğru DNS için var olan bir Publicıpaddress eklemek için:

```powershell
$pip = Get-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
$pip.DnsSettings.ReverseFqdn = "contosoapp1.westus.cloudapp.azure.com."
Set-AzureRmPublicIpAddress -PublicIpAddress $pip
```

Geriye doğru DNS zaten bir DNS adı yok var olan bir Publicıpaddress eklemek için de bir DNS adı belirtmeniz gerekir:

```powershell
$pip = Get-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
$pip.DnsSettings = New-Object -TypeName "Microsoft.Azure.Commands.Network.Models.PSPublicIpAddressDnsSettings"
$pip.DnsSettings.DomainNameLabel = "contosoapp1"
$pip.DnsSettings.ReverseFqdn = "contosoapp1.westus.cloudapp.azure.com."
Set-AzureRmPublicIpAddress -PublicIpAddress $pip
```

#### <a name="azure-cli-10"></a>Azure CLI 1.0

Geriye doğru DNS için var olan bir Publicıpaddress eklemek için:

```azurecli
azure network public-ip set -n PublicIp -g MyResourceGroup -f contosoapp1.westus.cloudapp.azure.com.
```

Geriye doğru DNS zaten bir DNS adı yok var olan bir Publicıpaddress eklemek için de bir DNS adı belirtmeniz gerekir:

```azurecli
azure network public-ip set -n PublicIp -g MyResourceGroup -d contosoapp1 -f contosoapp1.westus.cloudapp.azure.com.
```

#### <a name="azure-cli-20"></a>Azure CLI 2.0

Geriye doğru DNS için var olan bir Publicıpaddress eklemek için:

```azurecli
az network public-ip update --resource-group MyResourceGroup --name PublicIp --reverse-fqdn contosoapp1.westus.cloudapp.azure.com.
```

Geriye doğru DNS zaten bir DNS adı yok var olan bir Publicıpaddress eklemek için de bir DNS adı belirtmeniz gerekir:

```azurecli
az network public-ip update --resource-group MyResourceGroup --name PublicIp --reverse-fqdn contosoapp1.westus.cloudapp.azure.com --dns-name contosoapp1
```

### <a name="create-a-public-ip-address-with-reverse-dns"></a>Geriye doğru DNS ile bir ortak IP adresi oluştur

Ters DNS özelliği zaten belirtilen yeni Publicıpaddress oluşturmak için:

#### <a name="powershell"></a>PowerShell

```powershell
New-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup" -Location "WestUS" -AllocationMethod Dynamic -DomainNameLabel "contosoapp2" -ReverseFqdn "contosoapp2.westus.cloudapp.azure.com."
```

#### <a name="azure-cli-10"></a>Azure CLI 1.0

```azurecli
azure network public-ip create -n PublicIp -g MyResourceGroup -l westus -d contosoapp3 -f contosoapp3.westus.cloudapp.azure.com.
```

#### <a name="azure-cli-20"></a>Azure CLI 2.0

```azurecli
az network public-ip create --name PublicIp --resource-group MyResourceGroup --location westcentralus --dns-name contosoapp1 --reverse-fqdn contosoapp1.westcentralus.cloudapp.azure.com
```

### <a name="view-reverse-dns-for-an-existing-publicipaddress"></a>Varolan bir Publicıpaddress için ters DNS görünümü

Varolan bir Publicıpaddress için yapılandırılmış bir değeri görüntülemek için:

#### <a name="powershell"></a>PowerShell

```powershell
Get-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
```

#### <a name="azure-cli-10"></a>Azure CLI 1.0

```azurecli
azure network public-ip show -n PublicIp -g MyResourceGroup
```

#### <a name="azure-cli-20"></a>Azure CLI 2.0

```azurecli
az network public-ip show --name PublicIp --resource-group MyResourceGroup
```

### <a name="remove-reverse-dns-from-existing-public-ip-addresses"></a>Mevcut ortak IP adreslerinden geriye doğru DNS Kaldır

Varolan bir Publicıpaddress ters DNS özelliği kaldırmak için:

#### <a name="powershell"></a>PowerShell

```powershell
$pip = Get-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
$pip.DnsSettings.ReverseFqdn = ""
Set-AzureRmPublicIpAddress -PublicIpAddress $pip
```

#### <a name="azure-cli-10"></a>Azure CLI 1.0

```azurecli
azure network public-ip set -n PublicIp -g MyResourceGroup –f ""
```

#### <a name="azure-cli-20"></a>Azure CLI 2.0

```azurecli
az network public-ip update --resource-group MyResourceGroup --name PublicIp --reverse-fqdn ""
```


## <a name="configure-reverse-dns-for-cloud-services"></a>Bulut Hizmetleri için ters DNS yapılandırma

Bu bölümde Azure PowerShell kullanarak Klasik dağıtım modelinde bulut Hizmetleri için ters DNS yapılandırma hakkında ayrıntılı yönergeler sağlar. Bulut Hizmetleri için ters DNS yapılandırma Azure portalı Azure CLI 1.0 veya Azure CLI 2.0 desteklenmiyor.

### <a name="add-reverse-dns-to-existing-cloud-services"></a>Mevcut bulut Hizmetleri için ters DNS ekleme

Geriye doğru DNS kaydı var olan bir bulut hizmetine eklemek için:

```powershell
Set-AzureService –ServiceName "contosoapp1" –Description "App1 with Reverse DNS" –ReverseDnsFqdn "contosoapp1.cloudapp.net."
```

### <a name="create-a-cloud-service-with-reverse-dns"></a>Geriye doğru DNS ile bir bulut hizmeti oluştur

Ters DNS özelliği zaten belirtilen yeni bir bulut hizmeti oluşturmak için:

```powershell
New-AzureService –ServiceName "contosoapp1" –Location "West US" –Description "App1 with Reverse DNS" –ReverseDnsFqdn "contosoapp1.cloudapp.net."
```

### <a name="view-reverse-dns-for-existing-cloud-services"></a>Mevcut bulut Hizmetleri için ters DNS görünümü

Var olan bir bulut hizmeti için ters DNS özelliği görüntülemek için:

```powershell
Get-AzureService "contosoapp1"
```

### <a name="remove-reverse-dns-from-existing-cloud-services"></a>Mevcut bulut hizmetlerinden geriye doğru DNS Kaldır

Var olan bir bulut hizmetinden geriye doğru DNS özelliğini kaldırmak için:

```powershell
Set-AzureService –ServiceName "contosoapp1" –Description "App1 with Reverse DNS" –ReverseDnsFqdn ""
```

## <a name="faq"></a>SSS

### <a name="how-much-do-reverse-dns-records-cost"></a>DNS kayıtları maliyeti ne kadar ters?

Bunlar boş!  Geriye doğru DNS kayıtlarını veya sorguları için ek bir maliyet yoktur.

### <a name="will-my-reverse-dns-records-resolve-from-the-internet"></a>Geriye doğru DNS kayıtlarımı internet'ten çözer?

Evet. Azure hizmetiniz için ters DNS özelliği ayarladıktan sonra Azure tüm geriye doğru DNS kaydı tüm Internet kullanıcıları için çözümlendiğinden emin olmak için gereken DNS bölgeleri ve DNS temsilcilerine yönetir.

### <a name="are-default-reverse-dns-records-created-for-my-azure-services"></a>Varsayılan geriye doğru DNS kayıtları için Azure Hizmetlerim oluşturulur?

Hayır. Geriye doğru DNS, bir katılımı özelliğidir. Bunları yapılandırmamaya seçerseniz varsayılan geriye doğru DNS kaydı oluşturulur.

### <a name="what-is-the-format-for-the-fully-qualified-domain-name-fqdn"></a>Tam etki alanı adı (FQDN) biçiminde nedir?

FQDN'ler ileriye doğru sırada belirtilir ve bir nokta (örneğin, "app1.contoso.com.") ile bitmelidir.

### <a name="what-happens-if-the-validation-check-for-the-reverse-dns-ive-specified-fails"></a>Başarısız geriye doğru DNS için doğrulama denetimi ı belirttiğiniz ne olur?

Geriye doğru DNS doğrulama denetimi başarısız olduğunda, geriye doğru DNS kaydını yapılandırmak için işlem başarısız olur. Geriye doğru DNS değeri gerektiği gibi düzeltin ve yeniden deneyin.

### <a name="can-i-configure-reverse-dns-for-azure-app-service"></a>Azure App Service için ters DNS yapılandırabilir miyim?

Hayır. Geriye doğru DNS Azure App Service için desteklenmiyor.

### <a name="can-i-configure-multiple-reverse-dns-records-for-my-azure-service"></a>Birden çok geriye doğru DNS kaydı için Azure Hizmetim yapılandırabilir miyim?

Hayır. Azure, her Azure bulut hizmeti veya Publicıpaddress için tek bir geriye doğru DNS kaydını destekler.

### <a name="can-i-configure-reverse-dns-for-ipv6-publicipaddress-resources"></a>IPv6 Publicıpaddress kaynaklar için ters DNS yapılandırabilir miyim?

Hayır. Azure şu anda destekler DNS yalnızca IPv4 Publicıpaddress kaynaklar ve bulut Hizmetleri için ters çevrilir.

### <a name="can-i-send-emails-to-external-domains-from-my-azure-compute-services"></a>Azure işlem Hizmetlerim e-postaları dış etki alanlarına gönderebilirim?

Hayır. [Azure işlem Hizmetleri dış etki alanlarına gönderen e-postaları desteklemez](https://blogs.msdn.microsoft.com/mast/2016/04/04/sending-e-mail-from-azure-compute-resource-to-external-domains/)

## <a name="next-steps"></a>Sonraki adımlar

Geriye doğru DNS hakkında daha fazla bilgi için bkz: [wikipedia'da ters DNS araması](http://en.wikipedia.org/wiki/Reverse_DNS_lookup).
<br>
Bilgi edinmek için nasıl [ISS atanan IP aralığınızı Azure DNS geriye doğru arama bölgesini barındırmak](dns-reverse-dns-for-azure-services.md).

