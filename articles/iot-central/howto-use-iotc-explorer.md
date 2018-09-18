---
title: Cihaz bağlantısı Azure IOT Central Gezgini'ni kullanarak izleme
description: Cihaz iletileri izlemeye ve IOT Central Explorer CLI aracılığıyla cihaz ikizi değişiklikleri gözlemleyin.
author: viv-liu
ms.author: viviali
ms.date: 09/12/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 962f394607d20869bf00db624533996b0060eaf2
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "45986311"
---
# <a name="monitor-device-connectivity-using-the-azure-iot-central-explorer"></a>Cihaz bağlantısı Azure IOT Central Gezgini'ni kullanarak izleme

*Bu konu, Oluşturucular ve Yöneticiler için geçerlidir.*

Cihazlarınızı IOT Central göndermek ve IOT hub'ı ikizindeki değişiklikleri gözlemlemek iletilerini görmek için IOT Central Explorer CLI kullanın. Cihaz bağlantısı durumunu daha derin bir anlayış kazanmak ve bulutta veya cihaz ikizi değişiklikleri vermiyor ulaşmasını değil cihaz iletilerini sorunları tanılamak için bu açık kaynaklı araç kullanabilirsiniz.

## <a name="visit-the-iotc-explorer-repo-in-githubhttpsakamsiotciotcexplorercligithub"></a>[GitHub iotc Gezgini depoda ziyaret edin](https://aka.ms/iotciotcexplorercligithub)

## <a name="prerequisites"></a>Önkoşullar
+ Node.js sürümü 8.x veya üzeri - https://nodejs.org
+ İotc Gezgini'nde kullanabilmeniz için bir erişim belirteci oluşturmak için bir yönetici, uygulamanızın almanız gerekir

## <a name="installing-iotc-explorer"></a>İotc Gezgini yükleniyor

Yüklemek için komut satırından aşağıdaki komutu çalıştırın:

```
npm install -g iotc-explorer
```

> [!NOTE]
> Genellikle birlikte yükleme komutunu çalıştırmanız gerekir `sudo` Unix benzeri ortamlarda.

## <a name="running-iotc-explorer"></a>Çalışan iotc Gezgini

Bazı komutlar ve kullanırken çalıştırmak üzere ortak seçenekler aşağıda belirtilmiştir `iotc-explorer`. Eksiksiz bir listesi komutları ve seçenekleri görüntülemek için geçirebilirsiniz `--help` için `iotc-explorer` veya herhangi bir alt komutları.

### <a name="login"></a>Oturum Aç

Kullanmaya başlamanıza, IOT Central uygulamanızın kullanabilmeniz için bir erişim belirteci almak için bir yönetici olması gerekir. Yönetici, aşağıdaki adımları gerçekleştirir:
1. Git **yönetim/erişim belirteçleri**. 
1. Tıklayın **oluşturmak**.
![Erişim belirteci sayfası ekran görüntüsü](media/howto-use-iotc-explorer/accesstokenspage.png)

1. Belirteç adı girip tıklayın **sonraki**, ve **belirteç değeri kopyalayın**.
    > [!NOTE]
    > İletişim kutusunu kapatmadan önce kopyalanmalıdır için belirteç değeri yalnızca bir kez gösterilecektir. İletişim kutusu kapatıldıktan sonra hiçbir zaman yeniden gösterilir.

    ![Erişim belirteci iletişim ekran görüntüsünü Kopyala](media/howto-use-iotc-explorer/copyaccesstoken.png)

Ardından, çalıştırarak CLI için oturum açmak için bu belirteci kullanabilirsiniz:

```sh
iotc-explorer login "<Token value>"
```

Siz değil olurdu belirteç, kabuk geçmişine kalıcı, belirteç bırakabilirsiniz bulun ve bunun yerine, istendiğinde sağlayın:

```
iotc-explorer login
```

### <a name="monitor-device-messages"></a>İzleyici cihaz iletileri

Belirli bir cihaz veya uygulama kullanarak tüm cihazlar gelen iletileri izleyebilirsiniz `monitor-messages` komutu. Bu, gelen yeni iletiler sürekli olarak çıkarır bir izleyici başlatır.

Uygulamanızdaki tüm aygıtları izlemek için aşağıdaki komutu çalıştırın:

```
iotc-explorer monitor-messages
```
Çıkış: ![izleme iletileri komut çıktısı](media/howto-use-iotc-explorer/monitormessages.PNG)

Belirli bir cihazdaki izlemek için yalnızca cihaz Kimliğini komutunun sonuna ekleyin:

```
iotc-explorer monitor-messages <your-device-id>
```

Komut bir makine kullanımı daha kolay biçimde ekleyerek çıktısını bulundurabilirsiniz `--raw` komut seçeneği:

```
iotc-explorer monitor-messages --raw
```

### <a name="get-device-twin"></a>Cihaz ikizi Al

Kullanabileceğiniz `get-twin` için bir IOT Central cihaz ikizi içeriğini almak için komutu. Bunu yapmak için aşağıdaki komutu çalıştırın:

```
iotc-explorer get-twin <your-device-id>
```

Çıkış: ![get-twin komut çıktısı](media/howto-use-iotc-explorer/getdevicetwin.PNG)

Olduğu gibi `monitor-messages`, çıktı geçirerek daha fazla makine-kolay alabilirsiniz `--raw` seçeneği:

```
iotc-explorer get-twin <your-device-id> --raw
```

## <a name="next-steps"></a>Sonraki adımlar
IOT Central Gezgini'ni kullanma hakkında bilgi edindiniz, önerilen sonraki keşfetmek için adımdır [cihazları IOT Central](howto-manage-devices.md).
