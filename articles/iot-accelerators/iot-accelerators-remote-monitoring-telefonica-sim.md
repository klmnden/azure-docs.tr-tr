---
title: Uzaktan izleme çözümünde - Azure SIM verilerini tümleştirme | Microsoft Docs
description: Bu makalede Uzaktan izleme çözümüne Telefónica SIM verilerini tümleştirme açıklar.
author: hegate
manager: ''
ms.author: hegate
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 05/15/2018
ms.topic: conceptual
ms.openlocfilehash: b07e21131d9560a49d99644525835ac5ee3bac9e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61442248"
---
# <a name="integrate-sim-data-in-the-remote-monitoring-solution"></a>Uzaktan izleme çözümünde SIM verilerini tümleştirme

IOT cihazları genellikle her yerden veri akışları göndererek izin veren bir SIM kart kullanarak buluta bağlayın. İşleçleri de IOT SIM tarafından sağlanan verileri aracılığıyla cihaz durumunu takip edebilmeniz, Azure IOT Uzaktan izleme çözüm IOT yönetilen bağlantı veri tümleştirmesini sağlar.

Uzaktan izleme Telefónica IOT bağlantısı ile tümleştirmesi dışında IOT bağlantı platformu kullanan müşteriler kendi cihazı, çözümlerine SIMs bağlantı verileri eşitleme sunar. Diğer IOT bağlantı sağlayıcıları GitHub aracılığıyla desteklemek için bu çözüm Genişletilebilir [depo](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet).

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

* Uzaktan izleme çözümüne Telefónica IOT SIM verilerini tümleştirme
* Gerçek zamanlı telemetri verilerini görüntüleme
* SIM verilerini görüntüleme

## <a name="telefnica-iot-integration-setup"></a>Telefónica IOT tümleştirme Kurulumu

### <a name="prerequisites"></a>Önkoşullar

Bu ek Uzaktan izleme özelliği şu anda Önizleme aşamasındadır. Azure Uzaktan izleme çözümüne bağlantı verilerinizi eşitlemek için bu adımları izleyin:

1. Bir istek dolgu [Telefónica'nın site](https://iot.telefonica.com/contact), seçeneğini **Azure Uzaktan izleme**, kişi verilerinizi de dahil olmak üzere.
2. Hesabınızı Telefónica etkinleştirir.
3. Telefónica istemci henüz olmamak ve istiyorsanız bunu veya diğer IOT bağlantı hazır bulut Hizmetleri, ziyaret [Telefónica'nın site](https://iot.telefonica.com/) ve seçeneğini **bağlantı**.

### <a name="telefnica-sim-setup"></a>Telefónica SIM Kurulumu
Cihaz kimliği ilişkisi Telefónica SIM & Azure İkizi Telefónica IOT SIM "diğer" özelliği temel alır. 

Gidin [Telefónica IOT bağlantı platformu portalı](https://m2m-movistar-es.telefonica.com/) > SIM Stok >, SIM seçin ve her SIM "diğer" istenen İkizi Deviceıd'nizi ile güncelleştirin. Bu görevi, toplu iş modunda yapılabilir (Telefónica IOT bağlantı platformu için kullanıcı kılavuzlarına bakın).

Bu görevi, toplu iş modunda yapılabilir (Telefónica IOT bağlantı platformu için kullanıcı kılavuzlarına bakın)

![Telefónica güncelleştirme](./media/iot-accelerators-remote-monitoring-telefonica-sim/telefonica_site.png)

Cihazınızı Uzaktan izleme bağlanmak için bu öğreticileri kullanarak izleyebilirsiniz [C](iot-accelerators-connecting-devices-linux.md) veya [düğüm](iot-accelerators-connecting-devices-node.md). 

## <a name="view-device-telemetry-and-sim-properties"></a>Cihaz telemetrisini görüntüleme ve SIM özellikleri

Telefónica hesabınızın düzgün yapılandırıldığından ve Cihazınızı bağlı sonra cihaz ayrıntıları ve SIM verilerini görüntüleyebilirsiniz.

Aşağıdaki bağlantı parametreleri yayımlanır:

* ICCID
* IP
* Ağ varlığı
* SIM durumu
* Ağ tabanlı konum
* Tüketilen veri trafiği

![Pano](./media/iot-accelerators-remote-monitoring-telefonica-sim/dashboard.png)

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT Uzaktan izleme SIM verilerini tümleştirme nasıl bir genel bakış olduğuna göre Çözüm Hızlandırıcıları için önerilen sonraki adımlar şunlardır:

* [Azure IOT Uzaktan izleme çözümü çalıştırma](quickstart-remote-monitoring-deploy.md)
* [Gelişmiş izleme gerçekleştirme](iot-accelerators-remote-monitoring-monitor.md)
* [Cihazlarınızı yönetme](iot-accelerators-remote-monitoring-manage.md)
* [Cihaz sorunlarını giderme](iot-accelerators-remote-monitoring-maintain.md)

