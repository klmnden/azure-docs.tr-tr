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
ms.openlocfilehash: 780a215b66fec845bc1df639fedda870881b4027
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39189516"
---
> [!div class="op_single_selector"]
> * [Windows üzerinde C](../articles/iot-accelerators/iot-accelerators-connecting-devices.md)
> * [Linux üzerinde C](../articles/iot-accelerators/iot-accelerators-connecting-devices-linux.md)
> * [Node.js (genel)](../articles/iot-accelerators/iot-accelerators-connecting-devices-node.md)
> * [Raspberry Pi üzerinde Node.js](../articles/iot-accelerators/iot-accelerators-connecting-pi-node.md)
> * [Raspberry Pi üzerinde C](../articles/iot-accelerators/iot-accelerators-connecting-pi-c.md)

Bu öğreticide, uygulamanız bir **Soğutucu** Uzaktan izleme için aşağıdaki telemetri gönderen cihaz [Çözüm Hızlandırıcısı](../articles/iot-accelerators/about-iot-accelerators.md):

* Sıcaklık
* Basınç
* Nem oranı

Kolaylık olması için örnek telemetri değerleri için kod oluşturur **Soğutucu**. Gerçek algılayıcılar cihazınıza bağlanma ve gerçek telemetri verileri göndererek örneği genişletebilirsiniz.

Örnek cihaz ayrıca:

* Meta veri özelliklerini tanımlamak için çözüme gönderir.
* Gelen tetiklenen eylemler yanıtlar **cihazları** çözümdeki sayfası.
* Yapılandırma değişiklikleri yanıtlar göndermek gelen **cihazları** çözümdeki sayfası.

Bu öğreticiyi tamamlamak için etkin bir Azure hesabınızın olması gerekir. Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](http://azure.microsoft.com/pricing/free-trial/).

## <a name="before-you-start"></a>Başlamadan önce

Cihazınız için kod yazmadan önce Uzaktan izleme çözüm Hızlandırıcısını dağıtma ve yeni bir fiziksel cihaz çözüme ekleyin.

### <a name="deploy-your-remote-monitoring-solution-accelerator"></a>Uzaktan izleme çözüm Hızlandırıcısını dağıtma

**Soğutucu** Bu öğreticide oluşturduğunuz cihazın gönderdiği verileri örneğine [Uzaktan izleme](../articles/iot-accelerators/quickstart-remote-monitoring-deploy.md) çözüm Hızlandırıcı. Uzaktan izleme çözüm Hızlandırıcısını zaten Azure hesabınızda hazırlamadıysanız bkz [Uzaktan izleme çözüm Hızlandırıcısını dağıtma](../articles/iot-accelerators/quickstart-remote-monitoring-deploy.md)

Uzaktan izleme çözümü dağıtım işlemi bittiğinde, bilgisayarınızı **başlatma** çözüm panosunu tarayıcınızda açmak için.

![Çözüm Panosu](media/iot-suite-selector-connecting/dashboard.png)

### <a name="add-your-device-to-the-remote-monitoring-solution"></a>Cihazınızı Uzaktan izleme çözümü Ekle

> [!NOTE]
> Çözümünüzde zaten bir cihaz eklediyseniz, bu adımı atlayabilirsiniz. Ancak, sonraki adım, cihaz bağlantısı dizeniz gerektirir. Bir cihazın bağlantı dizesinden alabilirsiniz [Azure portalında](https://portal.azure.com) veya bu adı kullanıyor [az IOT](https://docs.microsoft.com/cli/azure/iot?view=azure-cli-latest) CLI aracı.

Bir cihazın çözüm hızlandırıcısına bağlamayı için kendini IOT hub'a geçerli kimlik bilgilerini kullanarak tanımlamanız gerekir. Cihazı eklediğinizde, bu kimlik bilgilerini içeren cihaz bağlantı dizesini çözümü kaydetmek için fırsatınız. Bu öğreticinin sonraki adımlarında istemci uygulamanıza cihaz bağlantı dizesini içerir.

Uzaktan izleme çözümünüze cihaz eklemek için aşağıdaki adımları tamamlayın **cihazları** çözümdeki sayfası:

1. Seçin **+ yeni cihaz**ve ardından **fiziksel** olarak **cihaz türü**:

    ![Fiziksel bir cihaz ekleyin](media/iot-suite-selector-connecting/devicesprovision.png)

1. Girin **fiziksel Soğutucu** olarak cihaz kimliği. Seçin **simetrik anahtar** ve **anahtarları otomatik olarak oluştur** seçenekleri:

    ![Cihaz seçenekleri seçin](media/iot-suite-selector-connecting/devicesoptions.png)

1. Seçin **uygulamak**. Ardından, Not **cihaz kimliği**, **birincil anahtar**, ve **bağlantı dizesi birincil anahtarı** değerleri:

    ![Kimlik bilgilerini alma](media/iot-suite-selector-connecting/credentials.png)

Ekledikten artık fiziksel bir cihaz için Uzaktan izleme çözüm Hızlandırıcısını ve cihaz bağlantı dizesini Not. Aşağıdaki bölümlerde, cihaz bağlantı dizesini çözümünüze bağlanmak için kullandığı istemci uygulaması yürütürsünüz.

İstemci uygulamasının yerleşik uygulayan **Soğutucu** cihaz modeli. Bir çözüm Hızlandırıcı cihaz modeli, bir cihaz hakkında aşağıdakileri belirtir:

* Çözüme cihaz raporlarına özellikleri. Örneğin, bir **Soğutucu** cihaz, üretici yazılımı ve konumu hakkında bilgi bildirir.
* Telemetri türlerini çözüme cihaz gönderir. Örneğin, bir **Soğutucu** sıcaklık ve nem basınç değerlerini cihazı gönderir.
* Yöntemi cihazda çalıştırılacak çözümden zamanlayabilirsiniz. Örneğin, bir **Soğutucu** cihaz uygulanmalı **yeniden**, **FirmwareUpdate**, **EmergencyValveRelease**, ve  **IncreasePressure** yöntemleri.