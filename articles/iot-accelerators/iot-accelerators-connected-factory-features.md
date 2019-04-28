---
title: Bağlı Fabrika çözümü özellikler - Azure | Microsoft Docs
description: Önceden yapılandırılmış bağlı Fabrika çözümünün özelliklerine genel bakış.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 03/08/2019
ms.author: dobett
ms.openlocfilehash: 25812e10b4c2b90aa4f40c38610c30a26ab11d9d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61450009"
---
# <a name="what-is-connected-factory-iot-solution-accelerator"></a>Bağlı Fabrika IOT Çözüm Hızlandırıcısı nedir?

Bağlı Fabrika, Microsoft'un Azure endüstriyel IOT başvuru mimarisi, açık kaynak çözümü olarak paketlenmiş bir uygulamasıdır. Bu bir ticari ürün için başlangıç noktası olarak kullanabilirsiniz. Bağlı Fabrika çözümü önceden oluşturulmuş bir sürümünü Azure aboneliğinizden içine dağıtabileceğiniz [Azure IOT Çözüm Hızlandırıcıları](https://www.azureiotsolutions.com/#solutions/types/CF).

![Bağlı Fabrika çözümü Panosu](./media/iot-accelerators-connected-factory-features/dashboard.png)

Bağlı Fabrika çözüm Hızlandırıcısını [kodu Github'da kullanılabilir](https://github.com/Azure/azure-iot-connected-factory).

Bağlı Fabrika, aşağıdaki özellikleri içerir:

## <a name="industrial-device-interoperability"></a>Endüstriyel cihaz birlikte çalışabilirliği

- Bir OPC UA arabirimiyle endüstriyel varlıklara bağlanın.
- Sanal Üretim hattını (OPC UA sunucuları Docker kapsayıcılarında çalışan), bunlardan alınan Canlı telemetri görmek için kullanın.
- Bir bulut panosundan OPC UA sunucuları OPC UA bilgi modelinin göz atın.

## <a name="remote-management"></a>Uzaktan yönetim

- OPC UA varlıklarınızı bulut panosundan yapılandırın (yöntem çağırma, veri okuma ve yazma).
- OPC UA varlıklarınızdan toplanan telemetri varlıklarını bir bulut panosundan yayımlayın ve yayından kaldırın.

## <a name="cloud-dashboard"></a>Bulut panosu

- Telemetri önizlemeleri, doğrudan bir bulut Panoda görüntüleyin.
- Time Series Insights Gezgini panosunu kullanarak Telemetri verilerindeki eğilimleri görün ve bağıntılar oluşturun.
- Genel donanım verimliliğini (OEE) ve ana performans göstergeleri (KPI) hesaplanan bir bulut panodan bakın.
- Görünüm endüstriyel varlık hiyerarşileri etkileşimli bir harita yanı sıra bir ağaç topolojisi içinde.
- Görüntülemek, kabul etme ve bulut Panosu uyarıları kapatın.

## <a name="azure-time-series-insights"></a>Azure Zaman Serisi Görüşleri

- [Azure Time Series Insights](../time-series-insights/time-series-insights-overview.md) depolanması, Görselleştirme ve büyük miktarda zaman serisi verilerini sorgulama için oluşturulmuştur. Fabrika yararlanır, bu hizmete bağlı.
- Bağlı Fabrika bu ile tümleştirilir, hizmeti cihaz verilerinizi ayrıntılı, gerçek zamanlı analiz gerçekleştirin.

## <a name="rules-and-alerts"></a>Kurallar ve uyarılar

[Eşik tabanlı uyarılar kurallarını yapılandırmak](iot-accelerators-connected-factory-configure.md).

## <a name="end-to-end-security"></a>Uçtan uca güvenlik

- Rol Tabanlı Erişim Denetimi’ni (RBAC) kullanarak kullanıcılar için güvenlik izinlerini yapılandırın.
- OPC UA kimlik doğrulama (X.509 sertifikaları kullanarak) ve bunun yanı sıra güvenlik belirteçleri kullanarak uçtan uca şifreleme uygulanır.

## <a name="customizability"></a>Özelleştirmeyi ölçme

- Özel iş gereksinimlerinizi karşılayacak şekilde çözümü özelleştirin.
- Tam çözüm kaynak kodu Github'da kullanılabilir. Bkz: [önceden yapılandırılmış bağlı Fabrika çözümünüz](https://github.com/Azure/azure-iot-connected-factory) depo.

## <a name="next-steps"></a>Sonraki adımlar

Önceden yapılandırılmış bağlı Fabrika çözümü hakkında daha fazla bilgi için aşağıdaki makaleleri okuyarak öğrenin:

* [Önceden yapılandırılmış bağlı Fabrika çözüm Kılavuzu](iot-accelerators-connected-factory-sample-walkthrough.md)
* [Bağlı Fabrika için ağ geçidi dağıtma]( iot-accelerators-connected-factory-gateway-deployment.md)
