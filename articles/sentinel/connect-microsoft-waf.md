---
title: Gözcü Azure önizlemesinde Microsoft web uygulaması güvenlik duvarı veri toplama | Microsoft Docs
description: Gözcü Azure, Microsoft web uygulaması güvenlik duvarı verilerini nasıl toplayacağınızı öğrenin.
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
ms.date: 3/6/2019
ms.author: rkarlin
ms.openlocfilehash: 2238060acb60b1be0d06b81f62fb45a7f1c7a9b6
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2019
ms.locfileid: "58580606"
---
# <a name="collect-data-from-microsoft-web-application-firewall"></a>Microsoft web uygulaması güvenlik duvarı veri topla

> [!IMPORTANT]
> Azure Sentinel şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Azure uygulama ağ geçidinin Microsoft web uygulaması Güvenlik Duvarı (WAF) günlükleri akışını yapabilirsiniz. Bu WAF, SQL ekleme ve siteler arası betik gibi yaygın web güvenlik açıklarına karşı uygulamalarınızı korur ve hatalı pozitif sonuçları azaltmak için kuralları özelleştirmenizi sağlar. Azure Gözcü, Microsoft Web uygulaması güvenlik duvarı günlükleri akışını sağlamak için bu yönergeleri izleyin.


## <a name="prerequisites"></a>Önkoşullar

- Mevcut uygulama ağ geçidi kaynağı

## <a name="connect-to-microsoft-web-application-firewall"></a>Bağlanmak için Microsoft web uygulaması güvenlik duvarı

Microsoft web uygulaması güvenlik duvarı varsa, var olan bir ağ geçidi kaynağı olduğundan emin olun.
Sonra Microsoft web uygulaması güvenlik duvarı dağıtılan ve veri alma, uyarı verileri kolayca Azure Gözcü aktarılabilir.
    
5. Gözcü Azure portalında **veri bağlayıcıları**.
5. Veri bağlayıcıları sayfasında seçmek **WAF** Döşe.
1. Git [Application Gateway kaynağında](https://ms.portal.azure.com/#blade/HubsExtension/BrowseAllResourcesBlade/resourceType/Microsoft.Network%2FapplicationGateways) WAF'nizi seçin.
    1. Seçin **tanılama ayarları**.
    1. Seçin **+ tanılama ayarı ekleme** tablonun altında.
    1. İçinde **tanılama ayarları** sayfasında bir **adı** seçip **Log Analytics'e gönderme**.
    1. Altında **Log Analytics çalışma alanı** Azure Gözcü çalışma alanını seçin.
    1. Analiz etmek istediğiniz günlük türlerini seçin. Önerilir: ApplicationGatewayAccessLog ve ApplicationGatewayFirewallLog.
6. İlgili şema için Microsoft web uygulaması güvenlik duvarı uyarıları Log Analytics'te kullanmak için arama **AzureDiagnostics**.

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Gözcü Azure için Microsoft web uygulaması güvenlik duvarı bağlanma hakkında bilgi edindiniz. Azure Gözcü hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- Bilgi nasıl [görünürlük almak, veri ve olası tehditleri](quickstart-get-visibility.md).
- Başlama [Azure Gözcü kullanarak tehditleri algılama](tutorial-detect-threats.md).
