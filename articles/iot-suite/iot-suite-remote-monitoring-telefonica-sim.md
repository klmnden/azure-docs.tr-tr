---
title: Uzaktan izleme çözümünde - Azure SIM veri tümleştirme | Microsoft Docs
description: Bu makalede Uzaktan izleme çözümü haline Telefonica SIM verilerini tümleştirmek açıklar.
services: iot-suite
suite: iot-suite
author: hegate
manager: corywink
ms.author: hegate
ms.service: iot-suite
ms.date: 04/30/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: c82bb8ff3d0f903e1de627f13337ae5fc633458c
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="integrate-sim-data-in-the-remote-monitoring-solution"></a>Uzaktan izleme çözümünde SIM veri tümleştirme

## <a name="overview"></a>Genel Bakış
IOT cihazları genellikle veri akışlarını yerden göndermesine izin veren bir SIM kart kullanarak bulut bağlayın. İşleçler de SIM tarafından sağlanan verileri aygıttan durumunu izleyebilmeniz için Azure IOT Uzaktan izleme çözümü SIM yönetim veri tümleştirme sağlar. Uzaktan izleme Telefonica IOT kutusunu tümleşme dışında kendi IOT bağlantı Platform kullanan müşteriler cihazlarını eşitleme izin vererek SIMs bağlantı verilerini çözümleri sağlar. Bu çözüm, diğer telefon şirket sağlayıcıları GitHub deposunu aracılığıyla desteklemek için genişletilebilir.
Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
* Uzaktan izleme çözümü haline SIM veri tümleştirme
* Görünüm gerçek zamanlı telemetri
* SIM verileri görüntüleme 

## <a name="telefonica-iot-integration-setup"></a>Telefonica IOT tümleştirme Kurulumu

### <a name="prerequisites"></a>Önkoşullar
Azure Uzaktan izleme çözümüne bağlantısını verilerinizi eşitlemek için aşağıdaki adımları izleyin:

1.  Bir isteğiyle dolgu [Telefonica'nın site](https://iot.telefonica.com/contact), seçeneğini **Azure Uzaktan izleme**, kişi verilerinizi dahil olmak üzere.
2.  Telefonica hesabınızı etkinleşir. 
3.  Bir Telefónica istemci henüz burada değildir ve isterseniz bu veya diğer IOT bağlantı bulut hazır Hizmetleri keyfini adresini ziyaret edin [Telefonica'nın site](https://iot.telefonica.com/contact) ve seçeneğini **bağlantı**.

### <a name="telefonica-sim-setup"></a>Telefonica SIM Kurulumu
Telefónica SIM & Azure Twin aygıt kimliği ilişkilendirmesi Telefónica IOT SIM "diğer" özelliğine dayalı. 

Gidin [Telefónica IOT bağlantı Platform Portal](https://m2m-movistar-es.telefonica.com/) > SIM Stok >, SIM seçin ve her SIM "diğer", istenen Twin DeviceID ile güncelleştirin. 

Bu görev, toplu modda yapılabilir (Telefónica IOT bağlantı Platform kullanıcı kılavuzları bakın)

![Telefonica güncelleştirme](media/iot-suite-remote-monitoring-telefonica/telefonica_site.png)

Uzaktan izleme için Cihazınızı bağlamak için bu öğreticileri kullanarak izleyebileceğiniz [C](iot-suite-connecting-devices-linux.md) veya [düğümü](iot-suite-connecting-devices-node.md). 

## <a name="view-device-telemetry-and-sim-properties"></a>Cihaz telemetrisi ve SIM özellikleri görüntüleyin
Telefonica hesabınızı düzgün yapılandırıldığından ve Cihazınızı bağlı sonra cihaz ayrıntıları ve SIM verileri görüntüleyebilirsiniz.
Aşağıdaki bağlantı parametreleri yayımlanabilir:
* ICCID
* IP
* Ağ varlığı
* SIM durumu
* Ağ tabanlı konum
* Tüketilen veri trafiği

![Pano](media/iot-suite-remote-monitoring-telefonica/dashboard.png)
 
## <a name="next-steps"></a>Sonraki adımlar

Azure IOT Uzaktan izleme SIM veri tümleştirme genel bakış sahip olduğunuza göre çözümleri Hızlandırıcıları için önerilen sonraki adımlar şunlardır:

* [Azure IOT Uzaktan izleme çözümü çalışmayabilir](iot-suite-remote-monitoring-explore.md)
* [Gelişmiş izleme gerçekleştirme](iot-suite-remote-monitoring-monitor.md)
* [Cihazlarınızı yönetme](iot-suite-remote-monitoring-manage.md)
* [Cihaz sorunlarını giderme](iot-suite-remote-monitoring-maintain.md)

