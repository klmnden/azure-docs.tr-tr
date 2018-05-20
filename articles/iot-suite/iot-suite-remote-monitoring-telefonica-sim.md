---
title: Uzaktan izleme çözümünde - Azure SIM veri tümleştirme | Microsoft Docs
description: Bu makalede Uzaktan izleme çözümü haline Telefónica SIM verilerini tümleştirmek açıklar.
services: iot-suite
suite: iot-suite
author: hegate
manager: timlt
ms.author: hegate
ms.service: iot-suite
ms.date: 05/15/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 25fefa23eaa0ca38f8d3d888f5782ece221e8b1a
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="integrate-sim-data-in-the-remote-monitoring-solution"></a>Uzaktan izleme çözümünde SIM veri tümleştirme

IOT cihazları genellikle veri akışlarını yerden göndermesine izin veren bir SIM kart kullanarak bulut bağlayın. İşleçler da cihazı IOT SIM tarafından sağlanan verileri üzerinden durumunu izleyebilmeniz için Azure IOT Uzaktan izleme çözümü IOT yönetilen bağlantı verilerini tümleştirme sağlar.

Kendi IOT bağlantı Platform kullanan müşteriler cihazlarını SIMs bağlantı verilerini çözümleri için eşitleme izin vererek Telefónica IOT bağlantısıyla kutusunu tümleştirme dışında Uzaktan izleme sağlar. Bu çözüm diğer IOT bağlantı sağlayıcıları GitHub aracılığıyla desteklemek için Genişletilmiş [depo](http://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet).

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

* Uzaktan izleme çözümü haline Telefónica IOT SIM veri tümleştirme
* Görünüm gerçek zamanlı telemetri
* SIM verileri görüntüleme

## <a name="telefnica-iot-integration-setup"></a>Telefónica IOT tümleştirme Kurulumu

### <a name="prerequisites"></a>Önkoşullar

Bu ek Uzaktan izleme özelliği şu anda önizlemede değil. Azure Uzaktan izleme çözümüne bağlantısını verilerinizi eşitlemek için aşağıdaki adımları izleyin:

1. Bir isteğiyle dolgu [Telefónica'nın site](https://iot.Telefónica.com/contact), seçeneğini **Azure Uzaktan izleme**, kişi verilerinizi dahil olmak üzere.
2. Telefónica hesabınızı etkinleştirir.
3. Bir Telefónica istemci henüz burada değildir ve isterseniz bu veya diğer IOT bağlantı bulut hazır Hizmetleri keyfini adresini ziyaret edin [Telefónica'nın site](https://iot.Telefónica.com/contact) ve seçeneğini **bağlantı**.

### <a name="telefnica-sim-setup"></a>Telefónica SIM Kurulumu
Telefónica SIM & Azure Twin aygıt kimliği ilişkilendirmesi Telefónica IOT SIM "diğer" özelliği üzerinde temel alır. 

Gidin [Telefónica IOT bağlantı Platform Portal](https://m2m-movistar-es.telefonica.com/) > SIM Stok >, SIM seçin ve her SIM "diğer", istenen Twin DeviceID ile güncelleştirin. Bu görev, toplu modda yapılabilir (Telefónica IOT bağlantı Platform kullanıcı kılavuzları bakın).

Bu görev, toplu modda yapılabilir (Telefónica IOT bağlantı Platform kullanıcı kılavuzları bakın)

![Telefónica güncelleştirme](media/iot-suite-remote-monitoring-telefonica/telefonica_site.png)

Uzaktan izleme için Cihazınızı bağlamak için bu öğreticileri kullanarak izleyebileceğiniz [C](iot-suite-connecting-devices-linux.md) veya [düğümü](iot-suite-connecting-devices-node.md). 

## <a name="view-device-telemetry-and-sim-properties"></a>Cihaz telemetrisi ve SIM özellikleri görüntüleyin

Telefónica hesabınızı düzgün yapılandırıldığından ve Cihazınızı bağlı sonra cihaz ayrıntıları ve SIM verileri görüntüleyebilirsiniz.

Aşağıdaki bağlantı parametreleri yayımlanır:

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

