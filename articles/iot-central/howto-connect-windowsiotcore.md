---
title: Bir Windows IOT Core aygıt Azure IOT merkezi bağlamak | Microsoft Docs
description: Cihaz geliştiricisi olarak, Azure IOT merkezi uygulamanıza MXChip IOT DevKit aygıt bağlayacağınızı öğrenin.
author: miriambrus
ms.author: mriamb
ms.date: 04/09/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: c36a9798718c37fba889323830b76cf8201785cf
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35261908"
---
# <a name="connect-a-windows-iot-core-device-to-your-azure-iot-central-application"></a>Azure IOT merkezi uygulamanıza Windows IOT Core aygıtı bağlayın

Bu makalede Windows IOT Core cihaz, Microsoft Azure IOT merkezi uygulamaya bağlanmak için bir aygıt geliştiricisi olarak nasıl.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makaledeki adımları tamamlayabilmeniz için şunlar gereklidir:

1. Oluşturulan Azure IOT Merkezi uygulama **örnek Devkits** uygulama şablonu. Daha fazla bilgi için bkz: [Azure IOT merkezi uygulamanızı oluşturma](howto-create-application.md).
2. Windows 10 IOT Core işletim sistemini çalıştıran bir aygıt. Bu kılavuz için Raspberry Pi'yi kullanacağız

Oluşturulan bir uygulamayı **örnek Devkits** uygulama şablonu içeren bir **Windows IOT Core** cihaz şablonu aşağıdaki özelliklere sahip:

### <a name="telemetry-measurements"></a>Telemetri ölçümleri

| Alan adı     | Birimler  | Minimum | Maksimum | Ondalık basamak sayısı |
| -------------- | ------ | ------- | ------- | -------------- |
| nem oranı       | %      | 0       | 100     | 0              |
| Temp           | ° C     | -40     | 120     | 0              |
| basınç       | hPa    | 260     | 1260    | 0              |

### <a name="settings"></a>Ayarlar

Sayısal ayarları

| Görünen ad | Alan adı | Birimler | Ondalık basamak sayısı | Minimum | Maksimum | İlk |
| ------------ | ---------- | ----- | -------------- | ------- | ------- | ------- |
| Fan hızı    | fanSpeed   | RPM   | 0              | 0       | 1000    | 0       |


### <a name="properties"></a>Özellikler

| Tür            | Görünen ad | Alan adı | Veri türü |
| --------------- | ------------ | ---------- | --------- |
| Cihaz özelliği | Sayı öldürmüş   | dieNumber  | number    |
| Metin            | Konum     | location   | Yok       |

## <a name="add-a-real-device"></a>Gerçek cihaz ekleme

Azure IOT merkezi uygulamanızda gerçek bir aygıttan ekleyin **Windows IOT Core** cihaz şablonu ve cihaz bağlantı dizesini Not. Daha fazla bilgi için bkz: [gerçek bir cihazı Azure IOT merkezi uygulamanıza eklemek](tutorial-add-device.md).

### <a name="prepare-the-windows-iot-core-device"></a>Windows IOT Core cihazı hazırlama

Ayarlamak için bir Windows IOT Core aygıt lütfen izleyin [Windows IOT Core cihaz ayarlama] adresindeki adım adım Kılavuzu'na (https://github.com/Microsoft/microsoft-iot-central-firmware/tree/master/WindowsIoT#setup-a-physical-device).

### <a name="add-a-real-device"></a>Gerçek cihaz ekleme

Azure IOT merkezi uygulamanızda gerçek bir aygıttan ekleyin **Windows IOT Core** cihaz şablonu ve cihaz bağlantı dizesini Not. Daha fazla bilgi için bkz: [gerçek bir cihazı Azure IOT merkezi uygulamanıza eklemek](tutorial-add-device.md).

## <a name="prepare-the-windows-10-iot-core-device"></a>Windows 10 IOT Core cihazı hazırlama

### <a name="what-youll-need"></a>İhtiyaç duyacaklarınız:

Fiziksel bir Windows 10 IOT Core aygıtı ayarlamak için önce Windows 10 IOT Core çalıştıran bir cihazda olması gerekir. Windows 10 IOT Core aygıtı ayarlama öğrenin [burada](https://developer.microsoft.com/en-us/windows/iot/getstarted/prototype/setupdevice).

Ayrıca, Azure IOT Merkezi ile iletişim kurabilen bir istemci uygulaması gerekir. Azure SDK'sını kullanarak kendi özel uygulaması oluşturma ve Visual Studio kullanarak Cihazınızı dağıtmak veya karşıdan yükleyebileceğiniz bir [önceden oluşturulmuş örnek](https://developer.microsoft.com/en-us/windows/iot/samples) ve sadece dağıtmak ve cihazda çalıştırın. 

### <a name="deploying-the-sample-client-application"></a>Örnek istemci uygulamasını dağıtma

Bunu hazırlamak için istemci uygulamasına önceki adımdan Windows 10 IOT Cihazınızı dağıtmak için:

**Bağlantı dizesi kullanmak için istemci uygulaması için cihazda depolanan emin olun**
* Masaüstünde, connection.string.iothub adlı bir metin dosyasında bağlantı dizesini kaydedin.
* Metin dosyasını cihazın belge klasörüne kopyalayın: `[device-IP-address]\C$\Data\Users\DefaultAccount\Documents\connection.string.iothub`

Bunu yaptıktan sonra açmak gerekir [Windows cihaz portalı](https://docs.microsoft.com/en-us/windows/iot-core/manage-your-device/deviceportal) http://[device-IP-address]:8080 içinde herhangi bir tarayıcıda yazarak.

Orada ve eğer gösterildiği gibi, yapmak isteyebilirsiniz:
1. Sol taraftaki "Uygulamaları" düğümünü genişletin.
2. "Hızlı çalıştırma örnekler"'i tıklatın.
3. "Azure IOT Hub istemci"'yi tıklatın.
4. "Dağıtma ve Çalıştır" seçeneğini tıklatın.

![Azure IOT Hub istemci Windows cihaz portalı üzerinde gif](./media/howto-connect-windowsiotcore/iothubapp.gif)

Başarılı olduğunda, uygulamanın aygıtta başlatın ve şöyle görünür:

![Azure IOT Hub'ının ekran istemci uygulaması](./media/howto-connect-windowsiotcore/IoTHubForegroundClientScreenshot.png)

Azure IOT Merkezi, Raspberry Pi'yi üzerinde çalışan kodu uygulaması ile nasıl etkileşim kurduğunu görebilirsiniz:

* Üzerinde **ölçümleri** sayfa gerçek cihazınız için telemetri görebilirsiniz.
* Üzerinde **özellikleri** sayfasında, bildirilen öldürmüş numarası özelliğinin değeri görebilirsiniz.
* Üzerinde **ayarları** sayfasında Raspberry Pi'yi voltaj ve fan hızı gibi çeşitli ayarları değiştirebilirsiniz.

## <a name="download-the-source-code"></a>Kaynak kodu indirme

Keşfetmek ve istemci uygulaması için kaynak kodunu değiştirmek istiyorsanız, Github'dan indirebilirsiniz [burada](https://github.com/Microsoft/Windows-iotcore-samples/tree/develop/Samples/Azure/IoTHubClients). Kod değiştirmenin planlıyorsanız, Benioku dosyasında bu yönergeleri izlemeniz gereken [burada](https://github.com/Microsoft/Windows-iotcore-samples) masaüstü işletim sistemi için.

> [!NOTE]
> Varsa **git** yüklü değil, geliştirme ortamınızda buradan indirebilirsiniz [ https://git-scm.com/download ](https://git-scm.com/download).
