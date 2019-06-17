---
title: Azure Güvenlik Merkezi ile uygulama ağ geçidi tümleştirme | Microsoft Docs
description: Bu sayfada Application Gateway Azure Güvenlik Merkezi ile nasıl tümleştirildiği hakkında bilgi sağlar.
documentationcenter: na
services: application-gateway
author: vhorne
manager: jpconnock
editor: ''
ms.assetid: e5ea5cf9-3b41-4b85-a12c-e758bff7f3ec
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: ''
ms.workload: infrastructure-services
ms.date: 06/07/2017
ms.author: victorh
ms.openlocfilehash: 10f115b64f0bd3f7e557da2bedbf3327d0ef483d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62122310"
---
# <a name="overview-of-integration-between-application-gateway-and-azure-security-center"></a>Azure Güvenlik Merkezi ile uygulama ağ geçidi arasındaki tümleştirme genel bakış

Application Gateway ve Güvenlik Merkezi web uygulama kaynaklarınızın korunmasına nasıl yardımcı olduğunu öğrenin. Application gateway web uygulaması Güvenlik Duvarı (WAF) ile tümleşir [Güvenlik Merkezi](../security-center/security-center-intro.md) önlemek için sorunsuz bir görünümünü sağlamak için algılamak ve korumasız web uygulamalarını, ortamınızdaki tehditlere yanıt verin.

## <a name="overview"></a>Genel Bakış

Application Gateway WAF, web uygulamaları açıklarına ve güvenlik açıklarından korumak için Güvenlik Merkezi'nde bir kullanılması önerilir. WAF tarafından korunmayan etkin web kaynaklarını Güvenlik Merkezi'nde yüksek önem derecesi öneriler olarak gösterir. Web uygulaması güvenlik duvarlarını önerileri gösterilir **genel bakış** sayfasındaki **uygulamaları**.

![Güvenlik Merkezi ile tümleştirme][1]

İlgili web uygulaması güvenlik duvarı öneri ayrıntılarını gösteren yeni bir sayfa açar herhangi bir önerimiz'yı tıklatın.

## <a name="add-a-web-application-firewall-to-an-existing-resource"></a>Bir web uygulaması güvenlik duvarı için mevcut bir kaynak ekleyin

Gidin **tüm hizmetleri** > **güvenlik + kimlik** > **Güvenlik Merkezi** ve **Güvenlik Merkezi - genel bakış**, tıklayın **uygulamaları**. Üzerinde **Güvenlik Merkezi - uygulama**, tablo, Güvenlik Merkezi aboneliğinizde algılanan uygulamaların bir listesini içerir.

![Web uygulamaları][3]

Kritik bir sorunu ile bir web uygulaması tıklayarak, alma **uygulama güvenlik durumu** sayfası. Aşağıdaki resimde, bir web uygulaması güvenlik duvarı tarafından korunmayan web uygulaması. 

![korumalı web kaynakları][2]

Tıklayın **bir web uygulaması güvenlik duvarı ekleme** altında **önerileri** açmak için **bir Web uygulaması güvenlik duvarı ekleme** sayfası.

Değil mevcut bir Application Gateway, veya yeni bir tane oluşturmak isterseniz, tıklayın **Yeni Oluştur** ve **yeni bir Web uygulaması güvenlik duvarı oluşturma**, tıklatıp **Microsoft - Application Gateway** . Bu, bir uygulama ağ geçidi oluşturma adımlarında size rehberlik eder. Bu noktada, bu kaynak bir web uygulaması güvenlik duvarı tarafından korunur, Güvenlik Merkezi artık korumalı bir kaynağın izler, web uygulamanızın eklenir. Bu, bir arka uç havuzu üyesi olarak eklemez.

Mevcut uygulama ağ geçidi varsa, altında seçebilirsiniz **var olan çözümü kullanma**

![Web uygulaması güvenlik duvarı Ekle sayfası][4]

Bir uygulama ağ geçidi Güvenlik Merkezi aracılığıyla bir web uygulamasına eklerken, kaynağın bir arka uç havuzu üyesi olarak eklemez. Bu doğrudan uygulama ağ geçidi kaynağı yapılmalıdır.

## <a name="add-a-resource-to-an-existing-web-application-firewall"></a>Bir kaynak için var olan bir web uygulaması güvenlik duvarı ekleme

Gidin **tüm hizmetleri** > **güvenlik + kimlik** > **Güvenlik Merkezi** ve **Güvenlik Merkezi - genel bakış**, tıklayın **iş ortağı çözümleri**. Var olan Güvenlik Merkezi'ni kullanan uygulama ağ geçitleri Göster **iş ortağı çözümlerini** sayfası.

![İş ortağı çözümleri][7]

Tıklayın **uygulamayı Bağla** açmak için **bağlantı uygulamaları**, burada, mevcut uygulamaları seçmek için seçenekleri sunulur. Koruma ve uygulamaları belirleyin **Tamam**. Bu web uygulamasını application gateway, arka uç havuzuna eklemez. Güvenlik Merkezi izlemek için bu kaynakları korumalı bir kaynak olarak ayarlar. Kaynak bir arka uç havuzu üyesi olarak eklemek için bu application Gateway'de tıklayabilirsiniz geçerli sayfasından yapılmalıdır **çözüm Konsolu** web uygulamasına ekleyebileceğiniz uygulama ağ geçidi kaynağına ulaşmak için arka uç havuzu.

![iş ortağı çözümleri uygulamalar][6]

## <a name="finalize-configuration"></a>Yapılandırmayı son haline getir

Güvenlik Merkezi, bir uygulama ağ geçidine korumalı bir kaynak olarak eklenen uygulamalar izler.  Bu kaynak izler ve bu uygulama ağ geçidi tarafından korunmasını sağlar. Sonraki adım özel IP, genel IP veya NIC, sanal makinenizin uygulama ağ geçidinin arka uç havuzuna eklemektir. Bu, ek bir öneri yapılmadan **uygulama korumasını sonlandırma** kaynağı eklenene kadar gösterilir.

![Web uygulaması güvenlik duvarı Ekle sayfası][5]

## <a name="security-alerts"></a>Güvenlik Uyarıları

Güvenlik Merkezi'nde gidin **ALGILAMA** > **güvenlik uyarıları**.  Burada, WAF uyarıları, uygulama ağ geçitleri için bulabilirsiniz. Uyarılar, WAF kural tarafından ayrılır.

![Güvenlik Uyarıları][8]

Bir kural'ı tıklatarak uyarıların bir listesi için ilgili WAF kuralı sağlar. Her uyarı bulma ile ilgili ek ayrıntılar gösterir. Uygulama ağ geçidi bağlantı ayrıntılarını sağlayın.
 
![Uyarı ayrıntıları][9]

## <a name="next-steps"></a>Sonraki adımlar

Web uygulaması güvenlik duvarı var olan bir uygulama ağ geçidinde etkinleştirme konusunda bilgi edinmek için [oluşturma veya güncelleştirme Azure Application Gateway web uygulaması güvenlik duvarı ile](application-gateway-web-application-firewall-portal.md).

[1]: ./media/application-gateway-integration-security-center/figure1.png
[2]: ./media/application-gateway-integration-security-center/figure2.png
[3]: ./media/application-gateway-integration-security-center/figure3.png
[4]: ./media/application-gateway-integration-security-center/figure4.png
[5]: ./media/application-gateway-integration-security-center/figure5.png
[6]: ./media/application-gateway-integration-security-center/figure6.png
[7]: ./media/application-gateway-integration-security-center/figure7.png
[8]: ./media/application-gateway-integration-security-center/securitycenter.png
[9]: ./media/application-gateway-integration-security-center/figure9.png