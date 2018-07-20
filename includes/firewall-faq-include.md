---
title: include dosyası
description: include dosyası
services: firewall
author: vhorne
ms.service: ''
ms.topic: include
ms.date: 7/19/2018
ms.author: victorh
ms.custom: include file
ms.openlocfilehash: 1db5f0a62a21d040949c9f4e4c42f80c86e76506
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2018
ms.locfileid: "39164189"
---
### <a name="what-is-azure-firewall"></a>Azure Güvenlik Duvarı nedir?

Azure güvenlik duvarı, Azure sanal ağ kaynaklarını koruyan bir yönetilen, bulut tabanlı bir ağ güvenlik hizmetidir. Bir durum bilgisi olan tam olarak güvenlik duvarı olarak-hizmet ile yerleşik yüksek kullanılabilirlik ve ölçeklenebilirlik sınırsız bulut var. Merkezi olarak oluşturma, zorlama ve abonelikleri ve sanal ağlar arasında bağlantı ilkeleri uygulama ve ağ oturum. Azure Güvenlik Duvarı şu anda genel Önizleme aşamasındadır.

### <a name="what-capabilities-are-supported-in-the-azure-firewall-public-preview-release"></a>Azure güvenlik duvarı genel Önizleme sürümünde hangi özellikler destekleniyor?  

* Hizmet olarak durum bilgisi olan bir güvenlik duvarı
* Sınırsız bulut ölçeklenebilirliği içeren yerleşik yüksek kullanılabilirlik
* FQDN filtreleme 
* Ağ trafiğini filtreleme kuralları
* Giden SNAT desteği
* Merkezi olarak oluşturma, zorlama ve Azure abonelikleri ve sanal ağlar arasında bağlantı ilkeleri uygulama ve ağ oturum
* Günlüğe kaydetme ve analiz için tamamen tümleşik Azure İzleyici 

### <a name="how-can-i-join-the-azure-firewall-public-preview"></a>Azure güvenlik duvarı genel önizlemeye nasıl birleştirebilir miyim?

Azure Güvenlik Duvarı şu anda Azure güvenlik duvarı genel Önizleme belgelerinde açıklandığı gibi Register-AzureRmProviderFeature PowerShell komutunu kullanarak katılabilirsiniz yönetilen bir genel Önizleme aşamasındadır.

### <a name="what-is-the-pricing-for-azure-firewall"></a>Azure Güvenlik Duvarı için fiyatlandırma nedir?

Azure güvenlik duvarı sabit maliyete + değişken maliyet vardır. Fiyatlar aşağıda verilmiştir ve bunlar daha genel Önizleme süresince % 50'ye indirildiğini.

* Sabit Ücret: $1.25/firewall/hour
* Değişken ücret: $0.03/ GB (giriş veya çıkış) duvarıyla işlenir.

### <a name="what-is-the-typical-deployment-model-for-azure-firewall"></a>Azure Güvenlik Duvarı için tipik bir dağıtım modeli nedir?

Azure güvenlik duvarını herhangi bir sanal ağı dağıtmak mümkün olsa da, müşterilerin Azure güvenlik duvarı merkezi bir vnet'te dağıtma genellikle ve diğer sanal ağlara, Hub ve bağlı bileşen modeli eş. Varsayılan yol, bu Merkezi güvenlik duvarı sanal ağa işaret edecek şekilde eşlenmiş sanal ağlar'dan sonra ayarlayabilirsiniz.

### <a name="how-can-i-install-the-azure-firewall"></a>Azure Güvenlik Duvarı'nı nasıl yükleyebilirim?

Azure güvenlik duvarı, Azure portalı, PowerShell, REST API veya şablonları ayarlayabilirsiniz. Bkz: [Öğreticisi: dağıtma ve Azure Azure portalını kullanarak güvenlik duvarı yapılandırma](../articles/firewall/tutorial-firewall-deploy-portal.md) adım adım yönergeler için.

### <a name="what-are-some-azure-firewall-concepts"></a>Bazı Azure güvenlik duvarı kavramları nelerdir?

Azure güvenlik duvarı kuralları ve kural koleksiyonlarınızı destekler. Bir kural koleksiyonu aynı sıraya ve önceliği paylaşan bir kurallar kümesidir. Kural koleksiyonları, öncelik sırasına göre yürütülür, ağ kural topluluklarıdır daha yüksek önceliğe uygulama kural koleksiyonları, tüm kuralları sonlandırılıyor.
Kural koleksiyonları iki tür vardır:

* Uygulama kuralları: bir alt ağdan erişilebilen, tam etki alanı adlarını (FQDN) yapılandırmanıza olanak tanır. 
* Ağ kuralları: kaynak adresi, protokol, hedef bağlantı noktası ve hedef adres içeren kuralları yapılandırmanıza olanak tanır. 

### <a name="does-azure-firewall-support-inbound-traffic-filtering"></a>Azure güvenlik duvarı, gelen trafiği filtreleme destekliyor mu?

Azure güvenlik duvarı genel Önizleme, yalnızca giden filtrelemeyi destekler. Gelen HTTP/S dışındaki protokoller için koruma (örn: RDP, SSH, FTP) için Azure güvenlik duvarı büyüyecek kesin planlanmaktadır  
 
### <a name="what-logginganalytics-is-supported-by-the-azure-firewall"></a>Hangi günlüğe kaydetme/analiz Azure güvenlik duvarı tarafından destekleniyor mu?

Azure güvenlik duvarı, görüntüleme ve güvenlik duvarı günlükleri analiz etmek için Azure İzleyici ile tümleşiktir. Günlükleri Log Analytics, Azure depolama veya olay hub'ı gönderilebilir. Bunlar, Log analytics'te veya Excel ve Power BI gibi farklı araçları tarafından çözümlenebilir. Bkz: [öğretici: Azure Güvenlik Duvarı İzleme günlükleri](../articles/firewall/tutorial-diagnostics.md) daha fazla ayrıntı için.

### <a name="how-does-azure-firewall-work-relative-to-existing-like-nvas-in-the-marketplace"></a>Azure güvenlik duvarı Market'te nva'ları gibi varolan göre nasıl çalışır?

Azure güvenlik duvarı, belirli müşteri senaryoları ele bir temel güvenlik duvarı hizmetidir. Müşterilerin bir birleşiminden oluşan 3 taraf nva'ları ve Azure güvenlik duvarı beklenen ve birden çok daha iyi birlikte fırsatlarda ortaklarımızla birlikte çalışıyoruz. 
 
### <a name="what-is-the-difference-between-application-gateway-waf-and-azure-firewall"></a>Azure güvenlik duvarı ile Application Gateway WAF arasındaki fark nedir?

Web uygulaması Güvenlik Duvarı (WAF), web uygulamalarınızda açıklardan yararlanmaya ve güvenlik açıkları gelen merkezi koruma sağlayan bir Application Gateway özelliğidir. Azure Güvenlik Duvarı tüm bağlantı noktaları ve protokoller için giden ağ düzeyinde koruma ve uygulama düzeyinde koruma için giden HTTP/s sunar. HTTP/S olmayan protokolleri (örneğin, RDP, SSH, FTP) için gelen koruma için Azure güvenlik duvarı büyüyecek kesin planlanan

### <a name="what-is-the-difference-between-network-security-groups-nsg-and-azure-firewall"></a>Ağ güvenlik grupları (NSG) ve Azure güvenlik duvarı arasındaki fark nedir?

Azure Güvenlik Duvarı hizmeti, bizim var olan ağ güvenlik grubu işlevselliği tamamlar ve birlikte daha iyi savunma ağ güvenliği sağlar. Nsg'ler trafik her bir Abonelikteki sanal ağlar içindeki kaynaklara sınırlamak için filtreleme dağıtılmış ağ katmanı trafiği sağlar.  Farklı Aboneliklerde ve sanal ağları (Vnet) arasında ağ ve uygulama düzeyinde koruma sağlayarak bir tam durum bilgisi olan, merkezi bir ağ güvenlik duvarı bir hizmet olarak, Azure Güvenlik Duvarı var. 

### <a name="how-do-i-set-up-azure-firewall-with-my-service-endpoints"></a>My hizmet uç noktaları ile Azure Güvenlik Duvarı'nı nasıl ayarlayabilirim?

PaaS hizmetlerine güvenli erişim için hizmet uç noktaları önerilir. Azure güvenlik duvarı müşterileri, Azure güvenlik duvarı alt ağdaki hizmet uç noktalarının etkinleştirilmesi ve her iki özelliği – hizmet uç nokta güvenliği ve tüm trafik için Merkezi günlük teknolojisinden yararlanan için bağlı uç sanal ağları devre dışı bırakmak seçebilirsiniz.

### <a name="what-are-the-known-service-limits"></a>Bilinen hizmet sınırları nelerdir?

* Azure güvenlik duvarı 1000 TB/güvenlik duvarı/ay yazılım sınırı vardır. 
* Bir merkezi VNET'te çalışan azure güvenlik duvarı olup sanal ağ eşleme sınırlamalara tabi: en çok 50 uç sanal ağları.  
* Güvenlik Duvarı'nın en az bir dağıtım bölge başına böylece müşteriler, azure güvenlik duvarı genel eşleme ile çalışmaz.