---
title: include dosyası
description: include dosyası
services: iot-suite
author: dominicbetts
ms.service: iot-suite
ms.topic: include
ms.date: 04/24/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 28290718d15a893c5d676c887b9f810449075746
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34666019"
---
> [!div class="op_single_selector"]
> * [Windows üzerinde C](../articles/iot-accelerators/iot-accelerators-connecting-devices.md)
> * [Linux üzerinde C](../articles/iot-accelerators/iot-accelerators-connecting-devices-linux.md)
> * [Node.js (genel)](../articles/iot-accelerators/iot-accelerators-connecting-devices-node.md)
> * [Raspberry Pi üzerinde Node.js](../articles/iot-accelerators/iot-accelerators-connecting-pi-node.md)
> * [Raspberry Pi üzerinde C](../articles/iot-accelerators/iot-accelerators-connecting-pi-c.md)

Bu öğreticide, uygulamanız bir **Soğutucu** Uzaktan izleme için aşağıdaki telemetri gönderen aygıt [Çözüm Hızlandırıcısı](../articles/iot-accelerators/iot-accelerators-what-are-solution-accelerators.md):

* Sıcaklık
* Basınç
* Nem oranı

Kolaylık olması için örnek telemetri değerleri için kod oluşturur **Soğutucu**. Örnek gerçek algılayıcılar aygıtınıza bağlanma ve gerçek telemetri göndermesini genişletebilirsiniz.

Örnek cihaz ayrıca:

* Meta veri özelliklerini tanımlamak için çözüme gönderir.
* Gelen tetiklenen eylemler yanıtlar **aygıtları** çözümdeki sayfası.
* Yapılandırma değişiklikleri yanıtlar göndermek **aygıtları** çözümdeki sayfası.

Bu öğreticiyi tamamlamak için etkin bir Azure hesabınızın olması gerekir. Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](http://azure.microsoft.com/pricing/free-trial/).

## <a name="before-you-start"></a>Başlamadan önce

Cihazınız için herhangi bir kod yazmadan önce Uzaktan izleme Çözüm Hızlandırıcısı dağıtmak ve yeni bir fiziksel aygıt ekleyin.

### <a name="deploy-your-remote-monitoring-solution-accelerator"></a>Uzaktan izleme Çözüm Hızlandırıcısı dağıtma

**Soğutucu** Bu öğreticide oluşturduğunuz cihaz örneğine verileri gönderir [Uzaktan izleme](../articles/iot-accelerators/iot-accelerators-remote-monitoring-explore.md) Çözüm Hızlandırıcısı. Azure hesabınızda Uzaktan izleme Çözüm Hızlandırıcısı sağlanan yüklemediyseniz, bkz: [Uzaktan izleme Çözüm Hızlandırıcısı dağıtma](../articles/iot-accelerators/iot-accelerators-remote-monitoring-deploy.md)

Uzaktan izleme çözümü için dağıtım işlemi bittiğinde, bilgisayarınızı **başlatma** tarayıcınızda çözüm panosunu açın.

![Çözüm Panosu](media/iot-suite-selector-connecting/dashboard.png)

### <a name="add-your-device-to-the-remote-monitoring-solution"></a>Cihazınızı Uzaktan izleme çözümü ekleme

> [!NOTE]
> Çözümünüze bir aygıt zaten eklediyseniz, bu adımı atlayabilirsiniz. Ancak, sonraki adım, aygıt bağlantı dizesi gerektirir. Bir aygıtın bağlantı dizesinden alabilir [Azure portal](https://portal.azure.com) veya kullanarak [az IOT](https://docs.microsoft.com/cli/azure/iot?view=azure-cli-latest) CLI aracı.

Bir aygıt için Çözüm Hızlandırıcısı bağlanmak, kendi IOT hub'a geçerli kimlik bilgilerini kullanarak tanımlamanız gerekir. Çözüm cihazı eklediğinizde, bu kimlik bilgileri içeren cihaz bağlantı dizesini kaydedin bildirme fırsatı bulabilirsiniz. Bu öğreticide daha sonra istemci uygulamanızda cihaz bağlantı dizesini içerir.

Uzaktan izleme çözümünüz için bir aygıt eklemek için aşağıdaki adımları tamamlayın **aygıtları** çözümü sayfasında:

1. Seçin **+ yeni cihaz**ve ardından **fiziksel** olarak **aygıt türü**:

    ![Bir fiziksel aygıt ekleme](media/iot-suite-selector-connecting/devicesprovision.png)

1. Girin **fiziksel Soğutucu** aygıt kimliği olarak Seçin **simetrik anahtar** ve **otomatik oluştur anahtarları** seçenekleri:

    ![Aygıt seçenekleri seçin](media/iot-suite-selector-connecting/devicesoptions.png)

1. Seçin **uygulamak**. Not edin **cihaz kimliği**, **birincil anahtar**, ve **bağlantı dize birincil anahtarı** değerler:

    ![Kimlik bilgilerini alma](media/iot-suite-selector-connecting/credentials.png)

Şimdi fiziksel bir aygıtı için Uzaktan izleme Çözüm Hızlandırıcısı eklenir ve kendi cihaz bağlantı dizesini Not. Aşağıdaki bölümlerde, çözümünüz için bağlanmak için cihaz bağlantı dizesini kullanır istemci uygulaması yürütürsünüz.

İstemci uygulaması yerleşik uygulayan **Soğutucu** cihaz modeli. Bir Çözüm Hızlandırıcısı cihaz modeli bir cihaz hakkında aşağıdakileri belirtir:

* Çözüme raporları cihazı özellikleri. Örneğin, bir **Soğutucu** aygıt, bellenim ve konumu hakkında bilgi raporlar.
* Telemetri türlerini cihaz çözüme gönderir. Örneğin, bir **Soğutucu** cihaz sıcaklık ve nem baskısı değerleri gönderir.
* Yöntemleri cihazda çalıştırmak için çözümden zamanlayabilirsiniz. Örneğin, bir **Soğutucu** aygıt uygulanmalı **yeniden**, **FirmwareUpdate**, **EmergencyValveRelease**, ve  **IncreasePressure** yöntemleri.