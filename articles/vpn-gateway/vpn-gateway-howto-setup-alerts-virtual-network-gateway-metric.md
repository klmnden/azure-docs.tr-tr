---
title: Azure VPN ağ geçidi ölçümler ile ilgili uyarılar kurma
description: VPN ağ geçidi ölçümler üzerinde uyarılar yapılandırma adımları
services: vpn-gateway
author: anzaman
ms.service: vpn-gateway
ms.topic: conceptional
ms.date: 04/22/2019
ms.author: alzam
ms.openlocfilehash: 890b096acba601ec20efaac21155da84e77a1f31
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63769474"
---
# <a name="setting-up-alerts-on-vpn-gateway-metrics"></a>VPN ağ geçidi ölçümler ile ilgili uyarılar ayarlama

Bu makalede VPN ağ geçidi ölçümleri için uyarılar ayarlamanıza yardımcı olur. Azure İzleyici, Azure kaynakları için uyarıları ayarlama olanağı sağlar. "VPN" türünde sanal ağ geçitleri için uyarılar ayarlanabilir.


|**Ölçüm**   | **Birim** | **Ayrıntı düzeyi** | **Açıklama** | 
|---       | ---        | ---       | ---            | ---       |
|**AverageBandwidth**| Bayt/s  | 5 dakika| Ortalama ağ geçidi tüm siteden siteye bağlantılarda birleştirilmiş bant genişliği kullanımı.     |
|**P2SBandwidth**| Bayt/s  | 1 dakika  | Ortalama ağ geçidinde tüm noktadan siteye bağlantıları birleştirilmiş bant genişliği kullanımı.    |
|**P2SConnectionCount**| Sayı  | 1 dakika  | Ağ geçidi sayısı, P2S bağlantıları.   |
|**TunnelAverageBandwidth** | Bayt/s    | 5 dakika  | Ortalama ağ geçidi üzerinde oluşturulan tünelleri bant genişliği kullanımı. |
|**TunnelEgressBytes** | Bayt | 5 dakika | Giden trafiği ağ geçidi üzerinde oluşturulan tünelinde.   |
|**TunnelEgressPackets** | Sayı | 5 dakika | Ağ geçidi üzerinde oluşturulan tünelinde giden paketlerin sayısı.   |
|**TunnelEgressPacketDropTSMismatch** | Sayı | 5 dakika | Giden paketlerin sayısı tünelinde TS uyumsuzluğu nedeniyle bırakıldı. |
|**TunnelIngressBytes** | Bayt | 5 dakika | Ağ geçidi üzerinde oluşturulmuş tüneller gelen trafiği.   |
|**TunnelIngressPackets** | Sayı | 5 dakika | Ağ geçidi üzerinde oluşturulan tünelinde gelen paket sayısı.   |
|**TunnelIngressPacketDropTSMismatch** | Sayı | 5 dakika | TS uyumsuzluğu nedeniyle tünelinde bırakılan gelen paketlerin sayısı. |


## <a name="setup"></a>Portalı kullanarak ölçümlere göre Azure İzleyici uyarıları ayarlama

Aşağıdaki örnek adımlarda, bir ağ geçidi için bir uyarı oluşturacak: <br>

**Ölçüm:** Tünel ortalama bant genişliği <br>
**Koşul:** bant genişliği > 10 bayt / saniye <br>
**Penceresi:** 5 dakika <br>
**Uyarı eylemi:** Email <br>



1. Sanal ağ geçidi kaynağına gidin ve izleme sekmesinde, "Uyarı" seçin sonra yeni bir uyarı kuralı oluşturmak veya mevcut bir uyarı kuralı düzenleyin.

![Noktadan siteye](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric/metric-alert1.png "oluştur")

2. VPN ağ geçidiniz kaynağı seçin.

![Noktadan siteye](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric/metric-alert2.png "seçin")

3. Uyarı için yapılandırmak için bir ölçüm seçin ![noktadan siteye](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric/metric-alert3.png "seçin")
4. Sinyal mantığını yapılandırma. Sinyal mantığını için üç bileşeni vardır:

    a. Boyutlar: Ölçüm, boyutları değiştiyse, uyarı verileri bu boyutun yalnızca değerlendirir. böylece belirli boyut değerleri seçilebilir. Bu isteğe bağlıdır.<br>
    b. Koşul: Ölçüm değeri değerlendirmek için işlem.<br>
    c. Zaman: Ölçüm verilerinin ayrıntı düzeyi ve uyarı değerlendir süreyi belirtin.<br>

![Noktadan siteye](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric/metric-alert4.png "seçin")

5. Yapılandırılmış kurallara görüntülemek için "Üzerinde uyarı kurallarını yönet"'i tıklatın. ![noktadan siteye](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric/metric-alert8.png "seçin")

## <a name="next-steps"></a>Sonraki adımlar

Uyarılar tünel tanılama günlüklerini yapılandırmak için bkz [VPN ağ geçidi tanılama günlükleri ile ilgili uyarıları ayarlama](vpn-gateway-howto-setup-alerts-virtual-network-gateway-log.md).
