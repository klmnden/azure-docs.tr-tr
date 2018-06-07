---
title: Fabrika çözüm özellikleri - Azure bağlı | Microsoft Docs
description: Önceden yapılandırılmış Fabrika bağlı çözüm özelliklerine genel bakış.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 04/20/2018
ms.author: dobett
ms.openlocfilehash: 3478217771418ab31772d6a42a7ed8d8a2e8069a
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34626505"
---
# <a name="what-is-connected-factory-iot-solution-accelerator"></a>Bağlı Fabrika IOT Çözüm Hızlandırıcısı nedir?

Bağlı Fabrika Microsoft'un Azure endüstriyel IOT başvuru mimarisi, açık kaynak çözümü olarak paketlenmiş bir uygulamasıdır. Ticari bir ürün için bir başlangıç noktası olarak kullanabilirsiniz. Azure aboneliğinizden içine bağlı Fabrika çözümü önceden oluşturulmuş bir sürümü dağıtabilirsiniz [Azure IOT Çözüm Hızlandırıcıları](https://www.azureiotsolutions.com/#solutions/types/CF).

![Bağlı Fabrika çözüm Panosu](./media/iot-accelerators-connected-factory-features/dashboard.png)

Bağlı Fabrika aşağıdaki özellikleri içerir:

## <a name="industrial-device-interoperability"></a>Endüstriyel aygıt birlikte çalışabilirlik

- OPC UA arabirime sahip endüstriyel varlıkların bağlayın.
- Bunları dinamik telemetrisinden görmek için (OPC UA sunucuları Docker kapsayıcılarında çalışan) Benzetimli Üretim satırları kullanın.
- Bir bulut Pano OPC UA sunucularından OPC UA bilgi modelinin göz atın.

## <a name="remote-management"></a>Uzaktan Yönetim

- OPC UA varlıklarınızı bulut panodan (yöntemler, okumak ve yazmak çağrısı) yapılandırın.
- Yayımlama ve telemetri verilerini OPC UA varlıklarınızı bulut panosundan Yayımdan Kaldır.

## <a name="cloud-dashboard"></a>Bulut Panosu

- Telemetri önizlemeleri doğrudan bulut panosunda göster.
- Telemetri verileri eğilimleri görüntüleme ve zaman serisi Öngörüler Explorer Panoyu kullanarak bağıntıları oluşturun.
- Hesaplanan genel donanım verimliliği (OEE) ve ana performans göstergelerini (KPI'lar) bulut panodan bakın.
- Görünüm endüstriyel varlık hiyerarşileri ve etkileşimli bir haritaya aynı zamanda bir ağaç topolojisi.
- Görüntülemek, kabul ve bulut panosundan uyarıları kapatın.

## <a name="azure-time-series-insights"></a>Azure Zaman Serisi Görüşleri

- [Azure zaman serisi Öngörüler](../time-series-insights/time-series-insights-overview.md) depolamak, Görselleştirme ve büyük miktarlarda zaman serisi veri sorgulama için oluşturulmuştur. Fabrika yararlanır bu hizmete bağlı.
- Bağlı Fabrika bu ile tümleşir sağlayarak hizmet cihaz verilerinizin derin, gerçek zamanlı analiz gerçekleştirin.

## <a name="rules-and-alerts"></a>Kurallar ve uyarılar

[Uyarılar için kurallarını eşiğini tabanlı yapılandırma](iot-accelerators-connected-factory-configure.md).

## <a name="end-to-end-security"></a>Uçtan uca güvenlik

- Rol tabanlı erişim denetimi (RBAC) kullanarak kullanıcılar için güvenlik izinlerini yapılandırın.
- Uçtan uca şifreleme OPC UA kimlik doğrulaması (X.509 sertifikaları kullanarak) ve bunun yanı sıra güvenlik belirteçleri kullanılarak uygulanır.

## <a name="customizability"></a>Özelleştirilebilirliğini

- Belirli iş gereksinimlerini karşılamak için çözüm özelleştirin.
- Tam çözüm kaynak kodu Github'da kullanılabilir. Bkz: [bağlı Fabrika önceden yapılandırılmış çözüm](https://github.com/Azure/azure-iot-connected-factory) deposu.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makaleleri okuyarak bağlı Fabrika önceden yapılandırılmış çözüm hakkında daha fazla bilgi:

* [Bağlı Fabrika önceden yapılandırılmış çözümü gözden geçirme](iot-accelerators-connected-factory-sample-walkthrough.md)
* [Bağlı Fabrika için ağ geçidi dağıtma]( iot-accelerators-connected-factory-gateway-deployment.md)
