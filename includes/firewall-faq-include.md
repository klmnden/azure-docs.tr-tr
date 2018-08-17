---
title: include dosyası
description: include dosyası
services: firewall
author: vhorne
ms.service: ''
ms.topic: include
ms.date: 8/13/2018
ms.author: victorh
ms.custom: include file
ms.openlocfilehash: a63a12658bd0a4b4d018d51824af9814691a3cbf
ms.sourcegitcommit: d2f2356d8fe7845860b6cf6b6545f2a5036a3dd6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "40182255"
---
### <a name="what-is-azure-firewall"></a>Azure Güvenlik Duvarı nedir?

Azure Güvenlik Duvarı, Azure Sanal Ağ kaynaklarınızı koruyan yönetilen, bulut tabanlı bir güvenlik hizmetidir. Bir durum bilgisi olan tam olarak güvenlik duvarı olarak-hizmet ile yerleşik yüksek kullanılabilirlik ve ölçeklenebilirlik sınırsız bulut var. Aboneliklerle sanal ağlarda uygulama ve ağ bağlantısı ilkelerini merkezi olarak oluşturabilir, zorlayabilir ve günlüğe alabilirsiniz. Azure Güvenlik Duvarı şu anda genel Önizleme aşamasındadır.

### <a name="which-capabilities-are-supported-in-the-azure-firewall-public-preview-release"></a>Azure güvenlik duvarı genel Önizleme sürümünde hangi özellikler destekleniyor?  

* Hizmet olarak durum bilgisi olan güvenlik duvarı
* Sınırsız bulut ölçeklenebilirliği içeren yerleşik yüksek kullanılabilirlik
* FQDN filtreleme 
* Ağ trafiği filtreleme kuralları
* Giden SNAT desteği
* Merkezi olarak oluşturma yeteneği zorlamak ve Azure abonelikleri ve sanal ağlar arasında bağlantı ilkeleri uygulama ve ağ oturum
* Günlüğe kaydetme ve analiz için Azure İzleyici ile tam tümleştirme 

### <a name="how-can-i-join-the-azure-firewall-public-preview"></a>Azure güvenlik duvarı genel önizlemeye nasıl birleştirebilir miyim?

Azure Güvenlik Duvarı şu anda Register-AzureRmProviderFeature PowerShell komutunu kullanarak katılabilirsiniz yönetilen bir genel Önizleme aşamasındadır. Bu komut Azure güvenlik duvarı genel önizlemeye sunuldu belgelerinde açıklanmıştır.

### <a name="what-is-the-pricing-for-azure-firewall"></a>Azure Güvenlik Duvarı için fiyatlandırma nedir?

Azure Güvenlik Duvarı bir sabit ve değişken maliyeti vardır. Fiyatlar aşağıdaki gibidir ve genel Önizleme süresince % 50 daha fazla indirim uygulanır.

* Sabit Ücret: $1.25/firewall/hour
* Değişken ücreti: (giriş veya çıkış) duvarıyla işlenen $0.03/ GB

### <a name="what-is-the-typical-deployment-model-for-azure-firewall"></a>Azure Güvenlik Duvarı için tipik bir dağıtım modeli nedir?

Azure güvenlik duvarı herhangi bir sanal ağda dağıtabilirsiniz, ancak müşteriler genellikle merkez sanal ağda dağıtmak ve diğer sanal ağlara ona hub-and-spoke modelini eş. Varsayılan yol, bu Merkezi güvenlik duvarı sanal ağa işaret edecek şekilde eşlenen sanal ağlardan sonra ayarlayabilirsiniz.

### <a name="how-can-i-install-the-azure-firewall"></a>Azure Güvenlik Duvarı'nı nasıl yükleyebilirim?

Azure Güvenlik Duvarı'nı, Azure portalı, PowerShell, REST API kullanarak veya şablonları kullanarak ayarlayabilirsiniz. Bkz: [Öğreticisi: dağıtma ve Azure Azure portalını kullanarak güvenlik duvarı yapılandırma](../articles/firewall/tutorial-firewall-deploy-portal.md) adım adım yönergeler için.

### <a name="what-are-some-azure-firewall-concepts"></a>Bazı Azure güvenlik duvarı kavramları nelerdir?

Azure güvenlik duvarı kuralları ve kural koleksiyonlarınızı destekler. Bir kural koleksiyonu, aynı sıraya ve önceliği paylaşan bir kurallar kümesidir. Kural koleksiyonları, öncelik sırasına göre çalıştırılır. Ağ kural topluluklarıdır uygulama kural koleksiyonları daha yüksek bir öncelik ve tüm kuralları sonlandırılıyor.

Kural koleksiyonları iki tür vardır:

* *Uygulama kuralları*: bir alt ağdan erişilebilen, tam etki alanı adlarını (FQDN) yapılandırmanıza olanak sağlar. 
* *Ağ kuralları*: kaynak adresleri, protokolleri, hedef bağlantı noktaları ve adresleri içeren kuralları yapılandırmanıza olanak sağlar. 

### <a name="does-azure-firewall-support-inbound-traffic-filtering"></a>Azure güvenlik duvarı, gelen trafiği filtreleme destekliyor mu?

Azure güvenlik duvarı genel Önizleme, yalnızca giden filtrelemeyi destekler. HTTP/S olmayan protokolleri (örneğin, RDP, SSH veya FTP) için gelen koruma için Azure güvenlik duvarı büyüyecek kesin planlanan  
 
### <a name="which-logging-and-analytics-services-are-supported-by-the-azure-firewall"></a>Günlüğe kaydetme ve Analiz Hizmetleri Azure güvenlik duvarı tarafından destekleniyor mu?

Azure güvenlik duvarı, görüntüleme ve güvenlik duvarı günlükleri analiz etmek için Azure İzleyici ile tümleşiktir. Günlükleri Log Analytics, Azure depolama veya Event Hubs gönderilebilir. Bunlar, Log analytics'te veya Excel ve Power BI gibi farklı araçları tarafından çözümlenebilir. Daha fazla bilgi için [öğretici: Azure Güvenlik Duvarı İzleme günlükleri](../articles/firewall/tutorial-diagnostics.md).

### <a name="how-does-azure-firewall-work-differently-from-existing-services-such-as-nvas-in-the-marketplace"></a>Azure güvenlik duvarı farklı Market'te nva'ları gibi var olan hizmetlerden nasıl sağlanır?

Azure güvenlik duvarı, belirli müşteri senaryoları ele bir temel güvenlik duvarı hizmetidir. Üçüncü taraf nva'ları ve Azure Güvenlik Duvarı bir karışımını olacağını beklenmektedir. Birlikte daha iyi çalışan temel bir önceliktir.
 
### <a name="what-is-the-difference-between-application-gateway-waf-and-azure-firewall"></a>Azure güvenlik duvarı ile Application Gateway WAF arasındaki fark nedir?

Web uygulaması Güvenlik Duvarı (WAF), web uygulamalarınızda açıklardan yararlanmaya ve güvenlik açıkları gelen merkezi koruma sağlayan bir Application Gateway özelliğidir. Azure Güvenlik Duvarı tüm bağlantı noktaları ve protokoller için giden ağ düzeyinde koruma ve uygulama düzeyinde koruma için giden HTTP/s sunar. HTTP/S olmayan protokolleri (örneğin, RDP, SSH, FTP) için gelen koruma için Azure güvenlik duvarı büyüyecek kesin planlanan

### <a name="what-is-the-difference-between-network-security-groups-nsgs-and-azure-firewall"></a>Ağ güvenlik grupları (Nsg'ler) ile Azure güvenlik duvarı arasındaki fark nedir?

Azure Güvenlik Duvarı hizmeti, ağ güvenlik grubu işlevselliğini tamamlar. Birlikte daha iyi "derinlemesine savunma" ağ güvenliği sağlar. Dağıtılmış ağ katmanı trafik filtreleme trafiğini her bir Abonelikteki sanal ağ içindeki kaynaklarla sınırlama için ağ güvenlik grupları belirtin. Bir tam durum bilgisi olan, merkezi bir ağ güvenlik duvarı-farklı abonelikler ve sanal ağları arasında ağ ve uygulama düzeyinde koruma sağlayan hizmet olarak, Azure güvenlik duvarı olup. 

### <a name="how-do-i-set-up-azure-firewall-with-my-service-endpoints"></a>My hizmet uç noktaları ile Azure Güvenlik Duvarı'nı nasıl ayarlayabilirim?

PaaS hizmetlerine güvenli erişim için hizmet uç noktaları önerilir. Azure güvenlik duvarı alt ağdaki hizmet uç noktalarının etkinleştirilmesi ve bağlı uç sanal ağlarında devre dışı bırakmak isteyebilirsiniz. Bu şekilde özellikleri--Hizmeti uç nokta güvenliği ve tüm trafik için Merkezi günlük yararlanır.

### <a name="how-can-i-stop-and-start-azure-firewall"></a>Nasıl durdurun ve Azure güvenlik duvarını başlatma?

Azure PowerShell kullanabilirsiniz *serbest* ve *tahsis* yöntemleri.

Örneğin:

```azurepowershell
# Stop an exisitng firewall

$azfw = Get-AzureRmFirewall -Name "FW Name” -ResourceGroupName "RG Name"
$azfw.Deallocate()
Set-AzureRmFirewall -AzureFirewall $azfw
```

```azurepowershell
#Start a firewall

$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName "RG Name" -Name "VNet Name"
$publicip = Get-AzureRmPublicIpAddress -Name "Public IP Name" -ResourceGroupName " RG Name"
$azfw.Allocate($vnet,$publicip)
Set-AzureRmFirewall -AzureFirewall $azfw
```

### <a name="what-are-the-known-service-limits"></a>Bilinen hizmet sınırları nelerdir?

* Azure güvenlik duvarı, güvenlik duvarı / ay başına 1000 TB yazılım limiti vardır. 
* Azure merkez sanal ağ içinde çalıştırılan Güvenlik Duvarı'nın bir örneğini sanal ağ sınırlamaları, en fazla 50 uç sanal ağ eşlemesi var.  
* Bölge başına en az bir güvenlik duvarı dağıtım olmalıdır böylece azure güvenlik duvarı genel eşleme ile çalışmaz.
* Azure güvenlik duvarı, 10 k uygulama kuralları ve 10 bin ağ kurallarını destekler.