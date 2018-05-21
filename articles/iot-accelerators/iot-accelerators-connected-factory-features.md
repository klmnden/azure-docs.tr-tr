---
title: Fabrika çözüm özellikleri - Azure bağlı | Microsoft Docs
description: Önceden yapılandırılmış Fabrika bağlı çözüm özelliklerine genel bakış.
services: iot-suite
suite: iot-suite
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: ''
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/20/2018
ms.author: dobett
ms.openlocfilehash: 7ee86a59e7c47406ea1a890f1a9d1eb2ed4ad772
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="what-is-azure-iot-suite-connected-factory"></a>Azure IOT paketi bağlı Fabrika nedir?

Bağlı Fabrika Microsoft'un Azure endüstriyel IOT başvuru mimarisi, açık kaynak çözümü olarak paketlenmiş bir uygulamasıdır. Ticari bir ürün için bir başlangıç noktası olarak kullanabilirsiniz. Azure aboneliğinizden içine bağlı Fabrika çözümü önceden oluşturulmuş bir sürümü dağıtabilirsiniz [Azure IOT paketi](https://www.azureiotsuite.com/#solutions/types/CF).

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

- [Özelleştirme](../iot-suite/iot-suite-v1-guidance-on-customizing-preconfigured-solutions.md) belirli iş gereksinimlerini karşılamak için çözüm.
- Tam çözüm kaynak kodu Github'da kullanılabilir. Bkz: [bağlı Fabrika önceden yapılandırılmış çözüm](https://github.com/Azure/azure-iot-connected-factory) deposu.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makaleleri okuyarak bağlı Fabrika önceden yapılandırılmış çözüm hakkında daha fazla bilgi:

* [Bağlı Fabrika önceden yapılandırılmış çözümü gözden geçirme](iot-accelerators-connected-factory-sample-walkthrough.md)
* [Bağlı Fabrika için ağ geçidi dağıtma]( iot-accelerators-connected-factory-gateway-deployment.md)
