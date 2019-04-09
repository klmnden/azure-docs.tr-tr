---
title: Uzaktan izleme - Azure toplu olarak bağlı cihazları yönetme | Microsoft Docs
description: Bu öğreticide, bir uzaktan izleme çözümünde toplu bağlı cihazların nasıl yönetileceğini öğrenin.
author: aditidugar
manager: philmea
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: tutorial
ms.date: 11/29/2018
ms.author: adugar
ms.openlocfilehash: 8a5c74c76662a089675fcbdcd8d5a7ea54b58fd1
ms.sourcegitcommit: e43ea344c52b3a99235660960c1e747b9d6c990e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2019
ms.locfileid: "59009676"
---
# <a name="tutorial-manage-your-connected-devices-in-bulk"></a>Öğretici: Bağlı cihazlarınızın toplu yönetme

Bu öğreticide, Uzaktan izleme çözüm Hızlandırıcısını bağlı cihazlarınızın toplu yapılandırmasını yönetmek için kullanabilirsiniz.

Contoso'da bir operatör olarak, yeni bir üretici yazılımı sürümü ile grubunu yapılandırmanız gerekir. Tek tek her cihazdaki üretici yazılımını güncelleştirmek olmasını istemezsiniz. Bir grubu cihaz üretici yazılımını güncelleştirmek için Uzaktan izleme çözüm hızlandırıcısının cihaz grupları ve otomatik cihaz yönetimi kullanabilirsiniz. Cihaz çevrimiçi duruma hemen sonra cihaz gruba eklediğiniz herhangi bir CİHAZDAN en son üretici yazılımına alır.

Bu öğreticide şunları yaptınız:

>[!div class="checklist"]
> * Bir cihaz grubu oluşturma
> * Hazırlama ve üretici yazılımı barındırın
> * Azure portalında cihaz yapılandırmasını oluşturma
> * Uzaktan izleme çözümünüze cihaz Yapılandırması Al
> * Cihazları cihaz grubu yapılandırmasını dağıtmak
> * Dağıtımı izleme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

<!--
If this is going to be a tutorial - we need to split this include into two so that we can accommodate the additional prerequisites:

[!INCLUDE [iot-accelerators-tutorial-prereqs](../../includes/iot-accelerators-tutorial-prereqs.md)]
-->

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi takip etmek için Azure aboneliğinizde Uzaktan İzleme çözümü hızlandırıcısının dağıtılmış örneğine sahip olmanız gerekir.

Uzaktan İzleme çözümü hızlandırıcısını henüz dağıtmadıysanız [Bulut tabanlı uzaktan izleme çözümü dağıtma](quickstart-remote-monitoring-deploy.md) hızlı başlangıcını tamamlamanız gerekir.

Üretici yazılımı dosyalarınızı barındırmak için bir Azure depolama hesabı gerekir. Mevcut bir depolama hesabı kullanabilir veya [yeni depolama hesabı oluşturma](../storage/common/storage-quickstart-create-account.md) aboneliğinizdeki.

Öğreticide bir [IOT DevKit](https://microsoft.github.io/azure-iot-developer-kit/) örnek cihaz olarak cihaz.

Yerel makinenizde aşağıdaki yazılımlar ihtiyacınız vardır:

* [Visual Studio Code'u (VS Code)](https://code.visualstudio.com/).
* [Azure IOT Workbench](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-iot-workbench) VS Code uzantısı.

Başlamadan önce:

* Emin olun [IOT DevKit Cihazınızda şifresizdir olan sürüm 1.4.0 veya daha yüksek](https://microsoft.github.io/azure-iot-developer-kit/docs/firmware-upgrading/).
* IOT DevKit SDK'sı şifresizdir olarak aynı sürümde olduğundan emin olun. VS Code'da Azure IOT Workbench kullanarak IOT DevKit SDK'sı güncelleştirebilirsiniz. Komut paletini açın ve girin **Arduino: Pano Yöneticisi'ni**. Daha fazla bilgi için [geliştirme ortamınızı hazırlama](../iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started.md#prepare-the-development-environment).

Ayrıca, en az bir IOT DevKit cihaz, Uzaktan izleme çözüm hızlandırıcısına bağlamayı gerekir. Bir IOT DevKit cihaz bağlamadıysanız bkz [MXChip IOT DevKit AZ3166 bağlanmak için IOT Uzaktan izleme çözüm Hızlandırıcısını](iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2.md).

## <a name="navigate-to-the-dashboard"></a>Panoya gidin

Uzaktan İzleme çözümü panosunu tarayıcınızda görüntülemek için önce [Microsoft Azure IoT Çözüm Hızlandırıcıları](https://www.azureiotsolutions.com/Accelerators#dashboard) sayfasına gidin. Azure aboneliği kimlik bilgilerinizi kullanarak oturum açmanız istenebilir.

Ardından [Hızlı başlangıç](quickstart-remote-monitoring-deploy.md) ile dağıttığınız Uzaktan İzleme çözüm hızlandırıcısına ait kutucukta bulunan **Başlat** öğesine tıklayın.

## <a name="create-a-device-group"></a>Bir cihaz grubu oluşturma

Otomatik olarak grubunu bellenimini güncelleştirmek için cihazlar, Uzaktan izleme çözümündeki cihaz grubunun üyeleri olmalıdır:

1. Üzerinde **cihazları** sayfasında, tüm **IOT DevKit** çözüm hızlandırıcısına bağlantılı cihazlar. Ardından **Jobs** (İşler) öğesine tıklayın.

1. İçinde **işleri** paneli, select **etiketleri**, iş adı kümesine **AddDevKitTag**ve ardından adlı bir metin etiketi ekleyin **IsDevKitDevice** bir değerle **Y**. Ardından **Uygula**.

1. Artık bir cihaz grubu oluşturmak için etiket değerlerini kullanabilirsiniz. Üzerinde **cihazları** sayfasında **cihaz gruplarını yönetme**.

1. Etiket adı kullanan bir metin filtresi oluşturma **IsDevKitDevice** ve değer **Y** koşulunda. Cihaz grubu olarak Kaydet **IOT DevKit cihazları**.

Bu öğreticide daha sonra tüm üyelerin Bellenim güncelleştirmeleri bir aygıt yapılandırmasını uygulamak için bu cihaz grubu kullanın.

## <a name="prepare-and-host-the-firmware"></a>Hazırlama ve üretici yazılımı barındırın

[Azure IOT Workbench](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-iot-workbench) VS Code uzantısı üretici yazılımı güncelleştirmesi örnek cihaz kodunu içerir.

### <a name="open-the-firmware-ota-sample-in-vs-code"></a>Üretici yazılımı OTA örnek VS Code'da açın

1. Kendi IOT DevKit bilgisayarınıza bağlı olmadığından emin olun. VS Code'u başlatın ve ardından DevKit bilgisayarınıza bağlayın.

1. Tuşuna **F1** komut paletini açın için girin ve seçin **IOT Workbench: Örnekler**. Ardından **IOT DevKit** panonun olarak.

1. Bulma **bellenim OTA** tıklatıp **açık örnek**. Yeni bir VS Code penceresinin açılır ve gösterir **firmware_ota** proje klasörü:

    ![IOT Workbench, üretici yazılımı OTA örnek seçin](media/iot-accelerators-remote-monitoring-bulk-configuration-update/iot-workbench-firmware-example.png)

### <a name="build-the-new-firmware"></a>Yeni bellenim oluşturun

İlk cihaz üretici yazılımını 1.0.0 sürümüdür. Yeni bellenim daha yüksek bir sürüm numarası olmalıdır.

1. VS Code'da açın **FirmwareOTA.ino** dosya ve değiştirme `currentFirmwareVersion` gelen `1.0.0` için `1.0.1`:

    ![Üretici yazılımı sürümünü Değiştir](media/iot-accelerators-remote-monitoring-bulk-configuration-update/version-1-0-1.png)

1. Komut paletini açın seçin ve ardından yazın **IOT Workbench: Cihaz**. Ardından **cihaz derleme** Kodu derlemek için:

    ![Cihaz derleme](media/iot-accelerators-remote-monitoring-bulk-configuration-update/iot-workbench-device-compile.png)

    VS Code kaydeder derlenmiş dosyasında `.build` proje klasöründe. Ayarlarınıza bağlı olarak, VS Code Gizle `.build` Gezgini görünümünde klasörü.

### <a name="generate-the-crc-value-and-calculate-the-firmware-file-size"></a>CRC değeri üretmek ve üretici yazılımı dosya boyutu hesaplanamadı

1. Komut paletini açın seçin ve ardından yazın **IOT Workbench: Cihaz**. Ardından **oluşturmak CRC**:

    ![CRC oluştur](media/iot-accelerators-remote-monitoring-bulk-configuration-update/iot-workbench-device-crc.png)

1. VS Code oluşturur ve CRC değeri, üretici yazılımı dosya adı ve yolu ve dosya boyutu çıktı penceresinde yazdırır. Bu değerler daha sonra kullanmak üzere not edin:

    ![CRC bilgileri](media/iot-accelerators-remote-monitoring-bulk-configuration-update/crc-info.png)

### <a name="upload-the-firmware-to-the-cloud"></a>Buluta üretici yazılımı yükleyin

Yeni bellenim dosyanızı bulutta barındırmak için Azure depolama hesabınızı kullanın.

1. Azure portalda depolama hesabınıza gidin. Hizmetler bölümünde **Blobları**. Adlı bir ortak kapsayıcı oluşturma **bellenim** bellenim dosyalarınızı depolamak için:

    ![Klasör Oluştur](media/iot-accelerators-remote-monitoring-bulk-configuration-update/blob-folder.png)

1. Üretici yazılımı dosyanızı kapsayıcıya yüklemek için seçin **bellenim** kapsayıcı ve tıklatın **karşıya**.

1. Seçin **FirmwareOTA.ino.bin**. Tam yolunu not edin, önceki bölümde bu dosyaya yapılan.

1. Sonra bellenim dosyayı karşıya yükleme tamamlandı, dosyasının URL'sini not edin.

### <a name="build-and-upload-the-original-firmware-to-the-iot-devkit-device"></a>Derleme ve özgün üretici yazılımı IOT DevKit cihaza yükle

1. VS Code'da açın **FirmwareOTA.ino** dosya ve değiştirme `currentFirmwareVersion` geri `1.0.0`:

    ![Sürüm 1.0.0](media/iot-accelerators-remote-monitoring-bulk-configuration-update/version-1-0-1.png)

1. Komut paletini açın seçin ve ardından yazın **IOT Workbench: Cihaz**. Ardından **cihaz yükleme**:

    ![Cihaz yükleme](media/iot-accelerators-remote-monitoring-bulk-configuration-update/device-upload.png)

1. VS Code doğrular ve kod IOT DevKit cihazınıza yükler.

1. Karşıya yükleme tamamlandığında, IOT DevKit cihazı yeniden başlatır. Yeniden başlatma tamamlandıktan sonra IOT DevKit ekranını gösterir **FW sürümü: 1.0.0**, ve yeni bellenim için denetlediği:

    ![OTA-1](media/iot-accelerators-remote-monitoring-bulk-configuration-update/ota-1.jpg)

## <a name="create-a-device-configuration"></a>Cihaz yapılandırması oluşturma

Cihaz yapılandırması cihazlarınızı istenen durumunu belirtir. Genellikle, bir geliştirici [yapılandırması oluşturur](../iot-hub/iot-hub-automatic-device-management.md#create-a-configuration) üzerinde **IOT cihaz Yapılandırması** Azure portalında sayfası. Cihaz yapılandırması cihazlarınızı istenen durumunu ve bir ölçüm kümesini belirten bir JSON belgesidir.

Aşağıdaki yapılandırmayı adlı dosya olarak kaydetmek **bellenim update.json** yerel makinenizde. Değiştirin `YOURSTRORAGEACCOUNTNAME`, `YOURCHECKSUM`, ve `YOURPACKAGESIZE` yer tutucuları daha önce Not yapılan değerleriyle:

```json
{
  "id": "firmware-update",
  "schemaVersion": null,
  "labels": {
    "Version": "1.0.1",
    "Devices": "IoT DevKit"
  },
  "content": {
    "deviceContent": {
      "properties.desired.firmware": {
        "fwVersion": "1.0.1",
        "fwPackageURI": "https://YOURSTRORAGEACCOUNTNAME.blob.core.windows.net/firmware/FirmwareOTA.ino.bin",
        "fwPackageCheckValue": "YOURCHECKSUM",
        "fwSize": YOURPACKAGESIZE
      }
    }
  },
  "targetCondition": "",
  "priority": 10,
  "systemMetrics": {
    "results": {
      "targetedCount": 0,
      "appliedCount": 0
    },
    "queries": {
      "targetedCount": "",
      "appliedCount": "select deviceId from devices where configurations.[[firmware-update]].status = 'Applied'"
    }
  },
  "metrics": {
    "results": {
      "Current": 1
    },
    "queries": {
      "Current": "SELECT deviceId FROM devices WHERE properties.reported.firmware.fwUpdateStatus='Current' AND properties.reported.firmware.type='IoTDevKit'",
      "Downloading": "SELECT deviceId FROM devices WHERE properties.reported.firmware.fwUpdateStatus='Downloading' AND properties.reported.firmware.type='IoTDevKit'",
      "Verifying": "SELECT deviceId FROM devices WHERE properties.reported.firmware.fwUpdateStatus='Verifying' AND properties.reported.firmware.type='IoTDevKit'",
      "Applying": "SELECT deviceId FROM devices WHERE properties.reported.firmware.fwUpdateStatus='Applying' AND properties.reported.firmware.type='IoTDevKit'",
      "Error": "SELECT deviceId FROM devices WHERE properties.reported.firmware.fwUpdateStatus='Error' AND properties.reported.firmware.type='IoTDevKit'"
    }
  }
}
```

Aşağıdaki bölümde bu yapılandırma dosyasını kullanırsınız.

## <a name="import-a-configuration"></a>Yapılandırma içeri aktarma

Bu bölümde, cihaz yapılandırması Uzaktan izleme çözüm Hızlandırıcısını bir paket olarak alın. Genellikle, bu görev bir işleç tamamlar.

1. Uzaktan izleme web kullanıcı Arabiriminde, gitmek **paketleri** sayfasında ve tıklayın **+ yeni paketi**:

    ![Yeni paket](media/iot-accelerators-remote-monitoring-bulk-configuration-update/packagepage.png)

1. İçinde **yeni paket** panelinde öğesini **cihaz Yapılandırması** paket türü olarak ve **bellenim** yapılandırma türü olarak. Tıklayın **Gözat** bulunacak **bellenim update.json** dosya yerel makinenizde ve ardından **karşıya**:

    ![Paket karşıya yükleme](media/iot-accelerators-remote-monitoring-bulk-configuration-update/uploadpackage.png)

1. Artık paketler listesini içeren **üretici yazılımı güncelleştirmesi** paket.

## <a name="deploy-the-configuration-to-your-devices"></a>Yapılandırma cihazlarınıza dağıtın

Bu bölümde, oluşturun ve IOT DevKit cihazlarınıza cihaz yapılandırması uygulayan bir dağıtım yürütün.

1. Uzaktan izleme web kullanıcı Arabiriminde, gitmek **dağıtımları** sayfasında ve tıklayın **+ yeni dağıtım**:

    ![Yeni dağıtım](media/iot-accelerators-remote-monitoring-bulk-configuration-update/deploymentpage.png)

1. İçinde **yeni dağıtım** panelinde, aşağıdaki ayarlara sahip bir dağıtım oluşturun:

    |Seçenek|Değer|
    |---|---|
    |Ad|Üretici yazılımı güncelleştirmesi dağıtma|
    |Paket türü|Cihaz Yapılandırması|
    |Yapılandırma türü|Üretici yazılımı|
    |Paket|üretici yazılımı update.json|
    |Cihaz grubu|Cihazları IOT DevKit|
    |Öncelik|10|

    ![Dağıtım oluşturma](media/iot-accelerators-remote-monitoring-bulk-configuration-update/newdeployment.png)

    **Uygula**'ya tıklayın. Yeni bir dağıtımda gördüğünüz **dağıtımları** sayfasını aşağıdaki ölçümlerini gösterir:

    * **Hedeflenen** cihaz grubundaki cihazların sayısını gösterir.
    * **Uygulanan** yapılandırma içeriklerle güncelleştirildi cihazların sayısını gösterir.
    * **Başarılı** cihaz sayısını dağıtımında söz konusu raporu başarılı gösterir.
    * **Başarısız** cihaz sayısını dağıtımında söz konusu raporu hatası gösterir.

## <a name="monitor-the-deployment"></a>Dağıtımı izleme

Birkaç dakika sonra IOT DevKit yeni bellenim bilgilerini alır ve cihaza indirilmeye başlar:

![OTA 2](media/iot-accelerators-remote-monitoring-bulk-configuration-update/ota-2.jpg)

Ağınızın hızına bağlı olarak, yükleme birkaç dakika sürebilir. Cihaz, üretici yazılımı yüklendikten sonra dosya boyutu ve CRC değeri doğrular. MXChip ekranında görüntülenen **geçirilen** doğrulama başarılı olursa.

![OTA 3](media/iot-accelerators-remote-monitoring-bulk-configuration-update/ota-3.jpg)

Onay başarılı olursa, cihazı yeniden başlatır. Geri sayım gelen gördüğünüz **5** için **0** önce yeniden başlatma gerçekleşir.

![OTA-4](media/iot-accelerators-remote-monitoring-bulk-configuration-update/ota-4.jpg)

Yeniden başlatıldıktan sonra IOT DevKit şifresizdir üretici yazılımı yeni sürüme yükseltir. Yükseltme, birkaç saniye sürebilir. Bu aşamasında, cihaz RGB LED'i kırmızı ve boş bir ekrandır.

![OTA 5](media/iot-accelerators-remote-monitoring-bulk-configuration-update/ota-5.jpg)

Yeniden başlatma tamamlandıktan sonra IOT DevKit cihazınızın üretici yazılımı 1.0.1 sürümü artık çalışıyor.

![OTA-6](media/iot-accelerators-remote-monitoring-bulk-configuration-update/ota-6.jpg)

Üzerinde **dağıtımları** sayfasında, bunlar güncelleştirirken cihazlarınızın durumunu görmek için bir dağıtım'a tıklayın. Cihaz grubunuz ve tanımladığınız özel ölçümler, her cihazın durumunu görebilirsiniz.

![Dağıtım ayrıntıları](media/iot-accelerators-remote-monitoring-bulk-configuration-update/deploymentstatus.png)

[!INCLUDE [iot-accelerators-tutorial-cleanup](../../includes/iot-accelerators-tutorial-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir grubun çözümünüze bağlı cihazların üretici yazılımını güncelleştirmek nasıl gösterilmiştir. Cihazları güncelleştirmek için otomatik cihaz Yönetimi çözümünüzü kullanır. Çözümünüzün temel IOT hub otomatik cihaz yönetimi özelliği hakkında daha fazla bilgi için bkz: [yapılandırma ve IOT cihazlarını uygun ölçekte Azure portalını kullanarak izleme](../iot-hub/iot-hub-auto-device-config.md).