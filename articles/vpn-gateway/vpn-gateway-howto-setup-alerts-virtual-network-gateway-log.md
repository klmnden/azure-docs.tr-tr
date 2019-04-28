---
title: Azure VPN ağ geçidi tanılama günlüğü olayları ile ilgili uyarılar kurma
description: VPN ağ geçidi tanılama günlüğü olayları uyarılarını yapılandırma adımları
services: vpn-gateway
author: anzaman
ms.service: vpn-gateway
ms.topic: conceptional
ms.date: 04/22/2019
ms.author: alzam
ms.openlocfilehash: 39a6d2e6201dd0635149159cb3727fd3c40f710a
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63769744"
---
# <a name="setting-up-alerts-on-vpn-gateway-diagnostic-log-events"></a>VPN ağ geçidi tanılama günlüğü olayları ile ilgili uyarılar ayarlama

Bu makalede VPN Gateway Tanılama Günlüğü etkinliklere göre uyarılar ayarlamanıza yardımcı olur.


## <a name="setup"></a>Portalı kullanarak Tanılama Günlüğü etkinliklere göre Azure İzleyici uyarıları ayarlama

Aşağıdaki örnek adımlarda bir siteden siteye VPN tüneli bağlantı kesme olayı için uyarı oluşturur



1. Tüm hizmetler "Log Analytics" için arama ve "Log Analytics çalışma alanları" çekme ![noktadan siteye](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert0.png "oluştur")

2. Log Analytics sayfasında "Oluştur"'a tıklayın.
![Noktadan siteye](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert1.png  "seçin")

3. "Yeni Oluştur" çalışma alanı denetleyin ve ayrıntıları girin.
![Noktadan siteye](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert2.png  "seçin")

4. "İzleme" altında VPN ağ geçidinizi bulun > "Tanılama ayarlar" dikey penceresindeki ![noktadan siteye](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert3.png  "seçin")

5. Tanılamayı etkinleştirmek için ağ geçidi'ne çift tıklayın ve ardından "Tanılama Aç" üzerinde ![noktadan siteye](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert4.png  "seçin")

6. Ayrıntıları girin ve "Göndermek için Log Analytics" ve "TunnelDiagnosticLog" seçildiğinden emin olun. 3. adımda oluşturulan Log Analytics çalışma alanı seçin.
![Noktadan siteye](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert5.png  "seçin")

7. Sanal ağ geçidi kaynak genel bakış için gidin ve izleme sekmesinde, "Uyarı" seçin sonra yeni bir uyarı kuralı oluşturma veya mevcut bir uyarı kuralı düzenleyin.
![Noktadan siteye](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert6.png  "seçin")

8. Log Analytics çalışma alanını ve kaynak seçin.
![Noktadan siteye](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert7.png  "seçin")

9. Select "özel günlük araması" sinyal mantığını olarak "Koşul Ekle" altında ![noktadan siteye](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert8.png  "seçin")

10. "Arama sorgusu" metin kutusuna <> uygun değerleri değiştirerek aşağıdaki sorguyu girin.

    AzureDiagnostics | Burada kategorisi "TunnelDiagnosticLog" ve ResourceId == toupper == ("<RESOURCEID OF GATEWAY>") ve TimeGenerated > ago(5m) ve remoteIP_s == "<REMOTE IP OF TUNNEL>" ve status_s "Bağlantı kesildi" ==

    Eşik değeri 0 ve "Bitti" seçeneğini ayarlayın.

    ![Noktadan siteye](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert9.png  "seçin")

11. Eylem grupları bölümü altında "Oluşturma kuralı" sayfasında "Yeni Oluştur" tıklayın. Ayrıntıları girin ve Tamam'a tıklayın

![Noktadan siteye](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert10.png  "seçin")

12. "Oluşturma kuralı" sayfasında, Ayrıntıları "İçin eylem özelleştirme" girin ve doğru eylem grubu adı "Eylem grubu adı" bölümünde göründüğünden emin olun. "Uyarı kuralı oluştur" düğmesine tıklayıp kuralı oluşturmak için.
![Noktadan siteye](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert11.png  "seçin")

## <a name="next-steps"></a>Sonraki adımlar

Tünel ölçümler üzerinde uyarılar yapılandırmak için bkz [VPN ağ geçidi ölçümlere ilişkin uyarıları ayarlama](vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric.md).
