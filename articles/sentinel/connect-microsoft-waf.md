---
title: Azure Gözcü önizlemesi için Microsoft web uygulaması güvenlik duvarı veri bağlayın | Microsoft Docs
description: Gözcü Azure için Microsoft web uygulaması güvenlik duvarı veri bağlanmayı öğreneceksiniz.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: bfa2eca4-abdc-49ce-b11a-0ee229770cdd
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2019
ms.author: rkarlin
ms.openlocfilehash: 5316fa7e3aa4465349b762b99bec9171f821062f
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59799006"
---
# <a name="connect-data-from-microsoft-web-application-firewall"></a>Microsoft web uygulaması güvenlik duvarı verileri bağlayın

> [!IMPORTANT]
> Azure Sentinel şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Azure uygulama ağ geçidinin Microsoft web uygulaması Güvenlik Duvarı (WAF) günlükleri akışını yapabilirsiniz. Bu WAF, SQL ekleme ve siteler arası betik gibi yaygın web güvenlik açıklarına karşı uygulamalarınızı korur ve hatalı pozitif sonuçları azaltmak için kuralları özelleştirmenizi sağlar. Azure Gözcü, Microsoft Web uygulaması güvenlik duvarı günlükleri akışını sağlamak için bu yönergeleri izleyin.


## <a name="prerequisites"></a>Önkoşullar

- Mevcut uygulama ağ geçidi kaynağı

## <a name="connect-to-microsoft-web-application-firewall"></a>Bağlanmak için Microsoft web uygulaması güvenlik duvarı

Microsoft web uygulaması güvenlik duvarı varsa, var olan bir ağ geçidi kaynağı olduğundan emin olun.
Sonra Microsoft web uygulaması güvenlik duvarı dağıtılan ve veri alma, uyarı verileri kolayca Azure Gözcü aktarılabilir.
    
1. Gözcü Azure portalında **veri bağlayıcıları**.
1. Veri bağlayıcıları sayfasında seçmek **WAF** Döşe.
1. Git [Application Gateway kaynağında](https://ms.portal.azure.com/#blade/HubsExtension/BrowseAllResourcesBlade/resourceType/Microsoft.Network%2FapplicationGateways) WAF'nizi seçin.
    1. Seçin **tanılama ayarları**.
    1. Seçin **+ tanılama ayarı ekleme** tablonun altında.
    1. İçinde **tanılama ayarları** sayfasında bir **adı** seçip **Log Analytics'e gönderme**.
    1. Altında **Log Analytics çalışma alanı** Azure Gözcü çalışma alanını seçin.
    1. Analiz etmek istediğiniz günlük türlerini seçin. Önerilir: ApplicationGatewayAccessLog ve ApplicationGatewayFirewallLog.
1. İlgili şema için Microsoft web uygulaması güvenlik duvarı uyarıları Log Analytics'te kullanmak için arama **AzureDiagnostics**.

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Gözcü Azure için Microsoft web uygulaması güvenlik duvarı bağlanma hakkında bilgi edindiniz. Azure Gözcü hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- Bilgi nasıl [görünürlük almak, veri ve olası tehditleri](quickstart-get-visibility.md).
- Başlama [Azure Gözcü kullanarak tehditleri algılama](tutorial-detect-threats.md).
