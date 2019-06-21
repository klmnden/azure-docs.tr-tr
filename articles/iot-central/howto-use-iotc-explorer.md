---
title: Cihaz bağlantısı Azure IOT Central Gezgini'ni kullanarak izleme
description: Cihaz iletileri izlemeye ve IOT Central Explorer CLI aracılığıyla cihaz ikizi değişiklikleri gözlemleyin.
author: viv-liu
ms.author: viviali
ms.date: 06/17/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 4d17f0e5273c7397bd9c6a71d14b7992d8652768
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67165875"
---
# <a name="monitor-device-connectivity-using-the-azure-iot-central-explorer"></a>Cihaz bağlantısı Azure IOT Central Gezgini'ni kullanarak izleme

*Bu konu, Oluşturucular ve Yöneticiler için geçerlidir.*

Cihazlarınızı IOT Central göndermek ve IOT hub'ı ikizindeki değişiklikleri gözlemlemek iletilerini görmek için IOT Central Explorer CLI kullanın. Cihaz bağlantısı durumunu daha derin bir anlayış kazanmak ve bulutta veya cihaz ikizi değişiklikleri vermiyor ulaşmasını değil cihaz iletilerini sorunları tanılamak için bu açık kaynaklı araç kullanabilirsiniz.

[GitHub iotc Gezgini depoda ziyaret edin.](https://aka.ms/iotciotcexplorercligithub)

## <a name="prerequisites"></a>Önkoşullar

+ Node.js sürümü 8.x veya üzeri - https://nodejs.org
+ Yönetici, uygulamanızın iotc Gezgini'nde kullanabilmeniz için bir erişim belirteci oluşturmanız gerekir

## <a name="install-iotc-explorer"></a>İotc Gezgini'ni yükleyin

Yüklemek için komut satırından aşağıdaki komutu çalıştırın:

```cmd/sh
npm install -g iotc-explorer
```

> [!NOTE]
> Genellikle install komutuyla çalıştırmanız gereken `sudo` Unix benzeri ortamlarda.

## <a name="run-iotc-explorer"></a>İotc explorer'ı çalıştırın

Aşağıdaki bölümlerde, sık kullanılan komutlar ve çalıştırdığınızda kullanabileceğiniz seçenekler açıklanmaktadır `iotc-explorer`. Eksiksiz bir listesi komutları ve seçenekleri görüntülemek için geçirmek `--help` için `iotc-explorer` veya herhangi bir alt komutları.

### <a name="login"></a>Oturum Aç

Kullanmaya başlamanıza, IOT Central uygulamanızın kullanabilmeniz için bir erişim belirteci almak için bir yönetici olması gerekir. Yönetici, aşağıdaki adımları gerçekleştirir:

1. Gidin **Yönetim** ardından **erişim belirteçlerini**.
1. Seçin **belirteç üretmek**.
    ![Erişim belirteci sayfası ekran görüntüsü](media/howto-use-iotc-explorer/accesstokenspage.png)

1. Belirteç adı girin, seçin **sonraki**, ardından **kopyalama**.
    > [!NOTE]
    > İletişim kutusunu kapatmadan önce kopyalanmalıdır için belirteç değeri yalnızca bir kere gösterilir. İletişim kutusu kapatıldıktan sonra hiçbir zaman yeniden gösterilir.

    ![Erişim belirteci iletişim ekran görüntüsünü Kopyala](media/howto-use-iotc-explorer/copyaccesstoken.png)

Belirteç için CLI'yı şu şekilde oturum açmak için kullanabilirsiniz:

```cmd/sh
iotc-explorer login "<Token value>"
```

Sahip tercih ederseniz, kabuk geçmişine belirteç kalıcı, belirteç bırakabilirsiniz bulun ve bunun yerine, istendiğinde sağlayın:

```cmd/sh
iotc-explorer login
```

### <a name="monitor-device-messages"></a>İzleyici cihaz iletileri

Belirli bir cihaz veya uygulama kullanarak tüm cihazlar gelen iletileri izleyebilirsiniz `monitor-messages` komutu. Bu komut geldikçe veren sürekli olarak yeni iletiler İzleyicisi'ni başlatır:

Uygulamanızdaki tüm aygıtları izlemek için aşağıdaki komutu çalıştırın:

```cmd/sh
iotc-explorer monitor-messages
```

Çıktı:

![izleme iletileri komut çıktısı](media/howto-use-iotc-explorer/monitormessages.png)

Belirli bir cihazdaki izlemek için yalnızca cihaz kimliğini komutunun sonuna ekleyin:

```cmd/sh
iotc-explorer monitor-messages <your-device-id>
```

Ekleyerek bir makine kullanımı daha kolay biçimde çıkarabilirsiniz `--raw` komut seçeneği:

```
iotc-explorer monitor-messages --raw
```

### <a name="get-device-twin"></a>Cihaz ikizi Al

Kullanabileceğiniz `get-twin` için bir IOT Central cihaz ikizi içeriğini almak için komutu. Bunu yapmak için aşağıdaki komutu çalıştırın:

```cmd/sh
iotc-explorer get-twin <your-device-id>
```

Çıktı:

![Get-twin komut çıktısı](media/howto-use-iotc-explorer/getdevicetwin.png)

Olduğu gibi `monitor-messages`, çıktı geçirerek daha fazla makine-kolay alabilirsiniz `--raw` seçeneği:

```cmd/sh
iotc-explorer get-twin <your-device-id> --raw
```

## <a name="next-steps"></a>Sonraki adımlar

IOT Central Gezgini'nin nasıl kullanılacağı öğrendiniz, önerilen sonraki keşfetmek için adımdır [cihazları IOT Central](howto-manage-devices.md).
