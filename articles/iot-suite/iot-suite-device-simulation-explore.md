---
title: "Cihaz benzetimi çözüm - Azure kullanmaya başlama | Microsoft Docs"
description: "IOT paketi benzetimi çözüm geliştirme ve test bir IOT çözüm yardımcı olmak üzere kullanılan bir araçtır. Benzetim, yüklenebilir diğer önceden yapılandırılmış çözümleri ile birlikte veya kendi özel çözümler ile kullanılan sunumu tek başına bir hizmettir."
services: iot device simulation
suite: iot-suite
author: troyhopwood
manager: timlt
ms.author: troyhop
ms.service: iot-suite
ms.date: 12/15/2017
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 25b9d1c7debe0d98a87b7d0b47dea6ab6f36c8af
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/20/2017
---
# <a name="device-simulation-walkthrough"></a>Cihaz benzetimi gözden geçirme

Azure IOT cihaz benzetimi geliştirme ve test bir IOT çözüm yardımcı olmak üzere kullanılan bir araçtır. Cihaz benzetimi, kendi özel çözümler veya diğer önceden yapılandırılmış çözümleri ile birlikte kullanabileceğiniz sunan bir tek başına ' dir.

Bu öğreticide, bazı aygıt benzetimi özellikleri açıklanmaktadır. Size nasıl çalışır ve kendi IOT çözümleriniz test etmek için kullanmanızı sağlayan gösterir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

>[!div class="checklist"]
> * Bir benzetimi yapılandırın
> * Başlatma ve durdurma bir benzetimi
> * Telemetri metrikleri görüntüleyin

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için Azure IOT cihaz benzetimi dağıtılan bir örneğini Azure aboneliğinizde gerekir.

Cihaz benzetimi dağıtılan henüz henüz tamamlanmış olmalıdır, [dağıtmak Azure IOT cihaz benzetimi](iot-suite-device-simulation-explore.md) Öğreticisi.

## <a name="configuring-device-simulation"></a>Cihaz benzetimi yapılandırma

Yapılandırın ve Pano içinde tamamen gelen cihaz benzetimi çalıştırın. IOT paketi panoyu açmak [sağlanan çözümleri](https://www.azureiotsuite.com/) sayfası. Tıklatın **başlatma** yeni cihaz benzetimi dağıtımınızı altında.

### <a name="target-iot-hub"></a>Hedef IOT hub'ı

Cihaz benzetimi önceden sağlanan bir IOT hub ile veya başka bir IOT hub ile kullanabilirsiniz:

![Hedef IOT hub'ı](media/iot-suite-device-simulation-explore/targethub.png)

> [!NOTE]
> Önceden sağlanan bir IOT hub'ı kullanma seçeneği, yalnızca yeni bir IOT Hub cihaz benzetimi dağıtıldığında oluşturmayı seçerseniz kullanılabilir. IOT hub'ı yoksa, her zaman öğesinden yeni bir tane oluşturabilirsiniz [Azure portal](https://portal.azure.com).

Belirli bir IOT hub hedeflemek için sağlamanız **iothubowner** bağlantı dizesi. Bu bağlantı dizesinden alabilirsiniz [Azure portal](https://portal.azure.com):

1. Azure portalında IOT Hub yapılandırma sayfasında, tıklatın **paylaşılan erişim ilkeleri**.
1. Tıklatın **iothubowner**.
1. Birincil veya ikincil anahtar kopyalayın.
1. Bu değer, hedef IOT hub'ı altındaki metin kutusuna yapıştırın.

![Hedef IOT hub'ı](media/iot-suite-device-simulation-explore/connectionstring.png)

### <a name="device-model"></a>Cihaz modeli

Cihaz modeli benzetimini yapmak için aygıt türünü seçmenize olanak tanır. Önceden yapılandırılmış aygıt modelleri birini seçin veya bir özel cihaz modeli için algılayıcılar listesini tanımlayın:

![Cihaz modeli](media/iot-suite-device-simulation-explore/devicemodel.png)

#### <a name="pre-configured-device-models"></a>Önceden yapılandırılmış aygıt modelleri

Cihaz benzetimi üç önceden yapılandırılmış cihaz modeli sağlar. Chillers, asansörler ve kamyonlar için aygıt modelleri kullanılabilir.

Önceden yapılandırılmış aygıt modelleri önceden belirlenen telemetri sıklık birden çok algılayıcılar içerir. Bu aygıtlar için telemetri sıklığı özelleştiremezsiniz.

Aşağıdaki tabloda her önceden yapılandırılmış cihaz modeli için yapılandırmaları bir listesini gösterir:

| Cihaz modeli | Algılayıcı | Birim | Telemetri sıklığı
| -------------| ------ | -----| --------------------|
| Soğutucu | nem oranı | % | 5 saniye |
| | basınç | psig | 5 saniye |
| | Sıcaklık | F | 5 saniye |
| Fırsatınızdır | Kat | | 5 saniye |
| | Titreşim | mm | 5 saniye |
| | Sıcaklık | F | 5 saniye |
| Kamyon | Enlem | | 3 saniye |
| | Boylam | | 3 saniye |
| | Hızı | mil hızla | 5 saniye |
| | cargotemperature | F | 5 saniye |

#### <a name="custom-device-model"></a>Özel aygıt modeli

Özel aygıt modelleri daha yakından kendi aygıtlarını temsil eden modeli algılayıcılar etkinleştirin. Özel bir aygıt en fazla 10 özel algılayıcılar olabilir.

Özel aygıt modeli türünü seçtiğinizde, tıklayarak yeni algılayıcılar ekleyebilirsiniz **+ Ekle algılayıcı**.

![Algılayıcılar ekleme](media/iot-suite-device-simulation-explore/customsensors.png)

Özel algılayıcılar aşağıdaki özelliklere sahiptir:

| Alan | Açıklama |
| ----- | ----------- |
| Algılayıcı adı | Gibi algılayıcı için kolay bir ad **sıcaklık** veya **hızı**. |
| Davranış | Davranışları telemetri verilerini bir iletiden gerçek veri benzetimini yapmak için göre değişir etkinleştirin. **Artırma** minimum değerde başlayıp gönderilen her ileti tek değer artar. En büyük değer karşılanır sonra ardından üzerinden yeniden en düşük değer başlar. **Azaltma** aynı şekilde davranır **artırma** ancak sayar. **Rastgele** davranışı en düşük değer ile en yüksek değerleri arasında rastgele bir değer oluşturur. |
| En küçük değer | Kabul edilebilir aralık temsil eden düşük sayı. |
| En yüksek değeri | Kabul edilebilir aralık temsil eden en büyük sayı. |
| Birim | ° F veya mil hızla gibi algılayıcı için ölçü birimi. |

### <a name="number-of-devices"></a>Aygıt sayısı

Cihaz benzetimi şu anda en çok 1.000 aygıtları benzetimini yapmak etkinleştirir.

![Aygıt sayısı](media/iot-suite-device-simulation-explore/numberofdevices.png)

### <a name="telemetry-frequency"></a>Telemetri sıklığı

Telemetri sıklığı sanal cihazlar IOT hub'ına verileri ne sıklıkla göndermelisiniz belirtmenize olanak sağlar. Sık her 10 saniye olarak seyrek olarak veya veri gönderebilir 99 saat, 59 dakika, 59 saniye olarak her.

![Telemetri sıklığı](media/iot-suite-device-simulation-explore/frequency.png)

> [!NOTE]
> IOT hub'ı önceden sağlanan IOT hub'ı dışında kullanıyorsanız, hedef IOT hub'ınız için ileti sınırlarını düşünmelisiniz. Örneğin, 10 dakikada bir S1 hub'ına telemetri göndermesini 1.000 sanal cihazlar varsa, yalnızca bir saatten fazla hub'ı için telemetri sınırına ulaştığında.

### <a name="simulation-duration"></a>Benzetim süresi

Belirli bir süre için benzetim çalıştırmak için veya açıkça durduruncaya kadar çalışmaya seçebilirsiniz. Belirli bir süre boyunca seçtiğinizde, tüm süresi 10 dakika 99 saatleri, 59 dakika ve 59 saniye kadar arasından seçim yapabilirsiniz.

![Benzetim süresi](media/iot-suite-device-simulation-explore/duration.png)

### <a name="start-and-stop-the-simulation"></a>Benzetim durdurup başlatın

Form için tüm gerekli yapılandırma verileri ekledikten sonra **benzetimi Başlat** düğmesi etkindir. Benzetimi başlatmak için bu düğmeye tıklayın.

![Benzetimi başlat](media/iot-suite-device-simulation-explore/start.png)

Benzetim için belirli bir süre belirtilmişse, zaman erişildiğinde sonra otomatik olarak durdurur. Her zaman benzetimi erken tıklayarak durdurabilirsiniz **benzetimi durdurun.**

Benzetim süresiz olarak çalıştırmayı seçerseniz sonra tıklattığınız kadar çalışır **benzetimi durdurmak**. Tarayıcınızı kapatın ve herhangi bir zamanda, benzetimi durdurmak için cihaz benzetimi sayfasına geri dönün.

![Benzetimi durdur](media/iot-suite-device-simulation-explore/stop.png)

> [!NOTE]
> Aynı anda yalnızca bir benzetimi çalıştırabilirsiniz. Yeni bir benzetimi başlamadan önce şu anda çalışan benzetimi durdurmanız gerekir.
