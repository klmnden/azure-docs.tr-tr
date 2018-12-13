---
title: Bağlı Fabrika çözümü özellikler - Azure | Microsoft Docs
description: Önceden yapılandırılmış bağlı Fabrika çözümünün özelliklerine genel bakış.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 04/20/2018
ms.author: dobett
ms.openlocfilehash: af2a2c84f9eb420a7ca9a8bd5909cbf856d29a5e
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53309204"
---
# <a name="what-is-connected-factory-iot-solution-accelerator"></a>Bağlı Fabrika IOT Çözüm Hızlandırıcısı nedir?

Bağlı Fabrika, Microsoft'un Azure endüstriyel IOT başvuru mimarisi, açık kaynak çözümü olarak paketlenmiş bir uygulamasıdır. Bu bir ticari ürün için başlangıç noktası olarak kullanabilirsiniz. Bağlı Fabrika çözümü önceden oluşturulmuş bir sürümünü Azure aboneliğinizden içine dağıtabileceğiniz [Azure IOT Çözüm Hızlandırıcıları](https://www.azureiotsolutions.com/#solutions/types/CF).

![Bağlı Fabrika çözümü Panosu](./media/iot-accelerators-connected-factory-features/dashboard.png)

Bağlı Fabrika çözüm Hızlandırıcısını [kodu Github'da kullanılabilir](https://github.com/Azure/azure-iot-connected-factory).

Bağlı Fabrika, aşağıdaki özellikleri içerir:

## <a name="industrial-device-interoperability"></a>Endüstriyel cihaz birlikte çalışabilirlik

- Bir OPC UA arabirimiyle endüstriyel varlıklarına bağlanın.
- Sanal Üretim hattını (OPC UA sunucuları Docker kapsayıcılarında çalışan), bunlardan alınan Canlı telemetri görmek için kullanın.
- Bir bulut panosundan OPC UA sunucuları OPC UA bilgi modelinin göz atın.

## <a name="remote-management"></a>Uzaktan Yönetim

- OPC UA varlıklarınızı bulut panosundan (yöntemler, okuma ve yazma veri çağrısı) yapılandırın.
- Yayımlama ve OPC UA varlıklarınızı bulut panosundan telemetri verilerini Yayımdan Kaldır.

## <a name="cloud-dashboard"></a>Bulut Panosu

- Telemetri önizlemeleri, doğrudan bir bulut Panoda görüntüleyin.
- Telemetri verileri eğilimleri görüntülemek ve bağıntılar Time Series Insights Gezgini Panoyu kullanarak oluşturun.
- Genel donanım verimliliğini (OEE) ve ana performans göstergeleri (KPI) hesaplanan bir bulut panodan bakın.
- Görünüm endüstriyel varlık hiyerarşileri etkileşimli bir harita yanı sıra bir ağaç topolojisi içinde.
- Görüntülemek, kabul etme ve bulut Panosu uyarıları kapatın.

## <a name="azure-time-series-insights"></a>Azure Zaman Serisi Görüşleri

- [Azure Time Series Insights](../time-series-insights/time-series-insights-overview.md) depolanması, Görselleştirme ve büyük miktarda zaman serisi verilerini sorgulama için oluşturulmuştur. Fabrika yararlanır, bu hizmete bağlı.
- Bağlı Fabrika bu ile tümleştirilir, hizmeti cihaz verilerinizi ayrıntılı, gerçek zamanlı analiz gerçekleştirin.

## <a name="rules-and-alerts"></a>Kurallar ve uyarılar

[Eşik tabanlı uyarılar kurallarını yapılandırmak](iot-accelerators-connected-factory-configure.md).

## <a name="end-to-end-security"></a>Baştan sona güvenlik

- Rol tabanlı erişim denetimi (RBAC) kullanarak kullanıcıların güvenlik izinlerini yapılandırın.
- OPC UA kimlik doğrulama (X.509 sertifikaları kullanarak) ve bunun yanı sıra güvenlik belirteçleri kullanarak uçtan uca şifreleme uygulanır.

## <a name="customizability"></a>Özelleştirmeyi ölçme

- Özel iş gereksinimlerinizi karşılayacak şekilde çözümü özelleştirin.
- Tam çözüm kaynak kodu Github'da kullanılabilir. Bkz: [önceden yapılandırılmış bağlı Fabrika çözümünüz](https://github.com/Azure/azure-iot-connected-factory) depo.

## <a name="next-steps"></a>Sonraki adımlar

Önceden yapılandırılmış bağlı Fabrika çözümü hakkında daha fazla bilgi için aşağıdaki makaleleri okuyarak öğrenin:

* [Önceden yapılandırılmış bağlı Fabrika çözüm Kılavuzu](iot-accelerators-connected-factory-sample-walkthrough.md)
* [Bağlı Fabrika için ağ geçidi dağıtma]( iot-accelerators-connected-factory-gateway-deployment.md)
