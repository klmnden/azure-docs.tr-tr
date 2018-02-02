---
title: "Azure Güvenlik Merkezi ile uygulama ağ geçidi tümleştirme | Microsoft Docs"
description: "Bu sayfa, uygulama ağ geçidi Azure Güvenlik Merkezi ile nasıl tümleştirildiği bilgi sağlar."
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: 
ms.assetid: e5ea5cf9-3b41-4b85-a12c-e758bff7f3ec
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: 
ms.workload: infrastructure-services
ms.date: 06/07/2017
ms.author: davidmu
ms.openlocfilehash: e0ae5d797a2a502cceeb1be20af4953f16f1320a
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="overview-of-integration-between-application-gateway-and-azure-security-center"></a>Uygulama ağ geçidi ve Azure Güvenlik Merkezi arasında tümleştirme genel bakış

Uygulama ağ geçidi ve Güvenlik Merkezi, web uygulaması kaynaklarınızı korumanıza nasıl yardımcı öğrenin. Uygulama ağ geçidi web uygulaması Güvenlik Duvarı (WAF) ile tümleşir [Güvenlik Merkezi](../security-center/security-center-intro.md) önlemek için sorunsuz bir görünümünü sağlamak için algılamak ve korumasız web uygulamaları, ortamınızdaki tehditlere yanıt verir.

## <a name="overview"></a>Genel Bakış

Uygulama ağ geçidi WAF Güvenlik Merkezi'nde açığından yararlanma girişimi ve güvenlik açıkları web uygulamaları korumak için önerilir. WAF tarafından korunmayan etkin web kaynakları Güvenlik Merkezi'nde yüksek önem derecesi öneriler göster. Web uygulaması güvenlik duvarı için öneriler üzerinde gösterilen **genel bakış** sayfasında **uygulamaları**.

![Güvenlik Merkezi ile tümleştirme][1]

İlgili web uygulaması güvenlik duvarı öneri ayrıntılarını gösteren yeni bir dikey pencere açılır herhangi bir önerimiz'yı tıklatın.

## <a name="add-a-web-application-firewall-to-an-existing-resource"></a>Mevcut bir kaynağı bir web uygulaması güvenlik duvarı ekleme

Gidin **daha Hizmetleri** > **güvenlik + kimlik** > **Güvenlik Merkezi** ve **Güvenlik Merkezi - genel bakış** dikey penceresinde tıklatın **uygulamaları**. Üzerinde **Güvenlik Merkezi - uygulamaları** dikey penceresinde, tablo, Güvenlik Merkezi, aboneliğinizde algıladı uygulamaların bir listesini içerir.

![web uygulaması][3]

Kritik bir sorunu web uygulamasıyla tıklayarak, get **uygulama güvenlik durumu** dikey. Aşağıdaki resimde, bir web uygulaması güvenlik duvarı tarafından korumalı olmayan web uygulaması. 

![korumalı web kaynakları][2]

Tıklatın **bir web uygulaması güvenlik duvarı ekleme** altında **önerileri** açmak için **bir Web uygulaması güvenlik duvarı ekleme** dikey.

Olmayan mevcut bir uygulama ağ geçidi, veya yeni bir tane oluşturmak istediğiniz varsa, tıklatın **Yeni Oluştur** ve **yeni Web uygulaması güvenlik duvarı oluşturma** dikey penceresinde ve tıklatın **Microsoft - uygulama ağ geçidi**. Bu uygulama ağ geçidi oluşturmak için adımlara götürür. Bu noktada, bu kaynak bir web uygulaması güvenlik duvarı tarafından korunduğunu şimdi Güvenlik Merkezi korumalı bir kaynağın izler, web uygulamanızın eklenir. Bu, arka uç havuzu üye olarak eklemez.

Mevcut bir uygulama ağ geçidi varsa, bunun altında seçebilirsiniz **var olan çözümünü kullanma**

![Web uygulaması güvenlik duvarı Ekle dikey penceresi][4]

Bir web uygulaması Güvenlik Merkezi aracılığıyla bir uygulama ağ geçidi için bir arka uç havuzu üye olarak kaynak eklemez ekleme, bu uygulama ağ geçidi kaynağı doğrudan yapılmalıdır.

## <a name="add-a-resource-to-an-existing-web-application-firewall"></a>Bir kaynak için var olan bir web uygulaması güvenlik duvarı ekleyin

Gidin **daha Hizmetleri** > **güvenlik + kimlik** > **Güvenlik Merkezi** ve **Güvenlik Merkezi - genel bakış** dikey penceresinde tıklatın **iş ortağı çözümleri**. Var olan Güvenlik Merkezi kullanan bir uygulama ağ geçitleri Göster **iş ortağı çözümleri** dikey.

![iş ortağı çözümleri][7]

Tıklatın **bağlantı uygulaması** açmak için **bağlantı uygulamaları** dikey penceresinde, burada, belirtilen olduğundan seçeneklerini mevcut uygulamaları seçin. Koruma ve uygulamaları seçmek **Tamam**. Bu web uygulaması uygulama ağ geçidi arka uç havuzuna eklemez. Güvenlik Merkezi izlemek için bu kaynakları korumalı bir kaynak olarak ayarlar. Kaynak arka uç havuzu üye olarak eklemek için bu uygulama ağ geçidi tıklayabilirsiniz geçerli dikey penceresinden yapılmalıdır **çözüm konsolunu** uygulama ağ geçidi kaynağı'na, web uygulaması arka uç havuzuna ekleyebileceğiniz gerçekleştirilecek.

![iş ortağı çözümleri uygulamaları][6]

## <a name="finalize-configuration"></a>Yapılandırma Sonlandır

Güvenlik Merkezi, bir uygulama ağ geçidi korunan bir kaynağa olarak eklenen uygulamalar izler.  Bu kaynak izler ve onu bir uygulama ağ geçidi tarafından korunmasını sağlar. Özel IP, genel IP veya NIC, sanal makinenin uygulama ağ geçidi arka uç havuzuna eklemek için sonraki adım olacaktır. Bu, ek bir öneri yapılana kadar **uygulama korumayı Sonlandır** kaynağı eklenene kadar gösterilir.

![Web uygulaması güvenlik duvarı Ekle dikey penceresi][5]

## <a name="security-alerts"></a>Güvenlik Uyarıları

Güvenlik Merkezi içinde gitmek **ALGILAMA** > **güvenlik uyarıları**.  Burada, uygulama ağ geçitleri için WAF uyarıları bulun. Uyarıları WAF kural tarafından bölünür.

![Güvenlik Uyarıları][8]

Bir kural tıklatarak uyarıların bir listesi için bu belirli WAF kuralı sağlar. Her uyarı bulma hakkında ek ayrıntılar gösterir. Uygulama ağ geçidi için bağlantı ayrıntıları sağlayın.
 
![Uyarı ayrıntıları][9]

## <a name="next-steps"></a>Sonraki adımlar

Web uygulaması güvenlik duvarı var olan bir uygulama ağ geçidi'nı etkinleştirme hakkında bilgi için [oluştur veya güncelleştir Azure uygulama ağ geçidi web uygulaması güvenlik duvarı ile](application-gateway-web-application-firewall-portal.md).

[1]: ./media/application-gateway-integration-security-center/figure1.png
[2]: ./media/application-gateway-integration-security-center/figure2.png
[3]: ./media/application-gateway-integration-security-center/figure3.png
[4]: ./media/application-gateway-integration-security-center/figure4.png
[5]: ./media/application-gateway-integration-security-center/figure5.png
[6]: ./media/application-gateway-integration-security-center/figure6.png
[7]: ./media/application-gateway-integration-security-center/figure7.png
[8]: ./media/application-gateway-integration-security-center/securitycenter.png
[9]: ./media/application-gateway-integration-security-center/figure9.png