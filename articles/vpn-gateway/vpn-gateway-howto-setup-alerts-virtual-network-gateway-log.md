---
title: Azure VPN Gateway'den tanılama günlüğü olayları uyarıları ayarlama
description: VPN ağ geçidi tanılama günlüğü olayları uyarılarını yapılandırma adımları
services: vpn-gateway
author: anzaman
ms.service: vpn-gateway
ms.topic: conceptional
ms.date: 04/22/2019
ms.author: alzam
ms.openlocfilehash: 3880c847c54136dfd3ba1ecfe0178565091e229f
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65510210"
---
# <a name="set-up-alerts-on-diagnostic-log-events-from-vpn-gateway"></a>VPN ağ geçidinden tanılama günlüğü olayları uyarıları ayarlama

Bu makalede Azure VPN ağ geçidi tanılama günlüğü olaylarını göre uyarılar ayarlamanıza yardımcı olur.

## <a name="setup"></a>Uyarılar ayarlayın

Aşağıdaki örnek adımlarda, siteden siteye VPN tüneli gerektirir bir bağlantı kesme olayı için bir uyarı oluşturacak:


1. Azure portalında arama **Log Analytics** altında **tüm hizmetleri** seçip **Log Analytics çalışma alanları**.

   ![Log Analytics çalışma alanına giderek seçimleri](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert0.png "oluştur")

2. Seçin **Oluştur** üzerinde **Log Analytics** sayfası.

   ![Log Analytics sayfası oluştur düğmesi](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert1.png  "seçin")

3. Seçin **Yeni Oluştur** ve ayrıntıları girin.

   ![Bir Log Analytics çalışma alanı oluşturmak için ayrıntıları](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert2.png  "seçin")

4. VPN ağ geçidiniz bulmak **İzleyici** > **tanılama ayarları** dikey penceresi.

   ![VPN ağ geçidi tanılama ayarlarında bulma seçimlerini](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert3.png  "seçin")

5. Tanılamayı etkinleştirmek için ağ geçidi'ne çift tıklayın ve ardından **tanılamayı Aç**.

   ![Tanılamayı açma seçimleri](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert4.png  "seçin")

6. Ayrıntıları girin ve emin **Log Analytics'e gönderme** ve **TunnelDiagnosticLog** seçilir. 3. adımda oluşturduğunuz Log Analytics çalışma alanı seçin.

   ![Onay kutularının seçili](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert5.png  "seçin")

7. Sanal ağ geçidi kaynağı genel bakış bölümüne gidin ve seçin **uyarılar** gelen **izleme** sekmesi. Ardından yeni bir uyarı kuralı oluşturmak veya mevcut bir uyarı kuralı düzenleyin.

   ![Yeni bir uyarı kuralı oluşturmak için seçim](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert6.png  "seçin")

   ![Noktadan siteye](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert6.png  "seçin")
8. Log Analytics çalışma alanını ve kaynak seçin.

   ![Çalışma alanını ve kaynak seçimlerini](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert7.png  "seçin")

9. Seçin **özel günlük araması** altında sinyal mantığını olarak **koşul Ekle**.

   ![Özel günlük araması seçimlerini](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert8.png  "seçin")

10. **Arama sorgusu** metin kutusuna aşağıdaki sorguyu girin. Uygun şekilde <> değerleri değiştirin.

     `AzureDiagnostics |
     where Category  == "TunnelDiagnosticLog" and ResourceId == toupper("<RESOURCEID OF GATEWAY>") and TimeGenerated > ago(5m) and
     remoteIP_s == "<REMOTE IP OF TUNNEL>" and status_s == "Disconnected"`

    Eşik değeri 0 olarak ayarlayın ve seçin **Bitti**.

    ![Bir sorgu girme ve bir eşik seçerek](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert9.png  "seçin")

11. Üzerinde **oluşturma kuralı** sayfasında **Yeni Oluştur** altında **Eylem grupları** bölümü. Ayrıntıları ve select doldurun **Tamam**.

    ![Yeni bir eylem grubu ayrıntılarını](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert10.png  "seçin")

12. Üzerinde **oluşturma kuralı** sayfasında, ayrıntıları doldurun **özelleştirme eylemleri** ve doğru ad içinde görünür olduğundan emin olun **eylem grubu adı** bölümü. Seçin **uyarı kuralı oluştur** kuralı oluşturmak için.

    ![Bir kural oluşturmak için seçim](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert11.png  "seçin")

## <a name="next-steps"></a>Sonraki adımlar

Tünel ölçümler üzerinde uyarılar yapılandırmak için bkz [VPN ağ geçidi ölçümler üzerinde uyarılar ayarlayın](vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric.md).
