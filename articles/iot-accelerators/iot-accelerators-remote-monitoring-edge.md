---
title: Bir çözüm öğretici - Azure anormallikleri uçta | Microsoft Docs
description: Bu öğreticide, Uzaktan izleme çözüm Hızlandırıcısını kullanarak IOT Edge cihazlarınıza izlemeyi öğrenin.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 11/08/2018
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: a812155474b244682613b38b9b9379fa6cdcdcd8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66117695"
---
# <a name="tutorial-detect-anomalies-at-the-edge-with-the-remote-monitoring-solution-accelerator"></a>Öğretici: Uzaktan izleme çözüm hızlandırıcısının uçta anormallikleri

Bu öğreticide, bir IOT Edge cihazı tarafından algılanan anomalileri yanıt vermek için Uzaktan izleme çözümünüzü yapılandırın. IOT Edge cihazları izin işlem uçta çözüme gönderilen telemetri hacmini azaltmak ve cihazlarda olaylara daha hızlı yanıt'ı etkinleştirmek için telemetri. Uç işleme avantajları hakkında daha fazla bilgi için bkz: [Azure IOT Edge nedir](../iot-edge/about-iot-edge.md).

Uzaktan izleme ile işleme edge tanıtmak için bir sanal Petrol pompa jack cihaz Bu öğreticide kullanılır. Bu Petrol pompa jack Contoso adlı bir kuruluş tarafından yönetilen ve Uzaktan izleme çözüm hızlandırıcısının bağlanır. Petrol pompa jack sensörlerini sıcaklığı ve Basıncı ölçün. Contoso'da işleçleri sıcaklık olağan dışı bir artış Petrol pompa jack yavaşlamasına neden olabileceğini bildirin. Contoso'da işleçleri, normal bir aralıkta olduğunda cihazın sıcaklık izleyin gerekmez.

Contoso Intelligent edge modülü sıcaklık anomalileri algılar Petrol pompa jack dağıtmak istiyor. Başka bir edge modülü, Uzaktan izleme çözümü uyarılar gönderir. Bir uyarı aldığınızda, Contoso işleci bir bakım teknisyen gönderebilecek. Contoso, çözüm bir uyarı aldığında çalıştırılacak bir e-posta gönderme gibi otomatik bir eylemi de yapılandırabilirsiniz.

Aşağıdaki diyagramda, öğretici senaryoda anahtar bileşenleri gösterilmektedir:

![Genel Bakış](media/iot-accelerators-remote-monitoring-edge/overview.png)

Bu öğreticide şunları yaptınız:

>[!div class="checklist"]
> * Çözüme bir IOT Edge cihazı Ekle
> * Bir Edge bildirimi oluşturma
> * Modüller cihazda çalıştırılacak tanımlayan bir paket olarak bildirim alma
> * Paketi, IOT Edge cihazınıza dağıtma
> * Bir CİHAZDAN Uyarıları görüntüle

IOT Edge cihazında:

* Çalışma zamanı paketi alır ve modülleri yükler.
* Stream analytics modülü, içinde pompa sıcaklık anomalileri algılar ve komutları sorunu gönderir.
* Stream analytics modülü çözüm Hızlandırıcı için filtrelenmiş verileri iletir.

Bu öğreticide, Linux sanal makinesi bir IOT Edge cihazı kullanılır. Bir edge modülü Petrol pompa jack cihazının simülasyonunu de yükleyin.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [iot-accelerators-tutorial-prereqs](../../includes/iot-accelerators-tutorial-prereqs.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="add-an-iot-edge-device"></a>IoT Edge cihazı ekle

IOT Edge cihazı, Uzaktan izleme çözüm Hızlandırıcısını eklemek için iki adımı vardır. Bu bölümde, nasıl kullanılacağını gösterir:

* IOT Edge cihazı eklemek **Device Explorer** UI Uzaktan izleme Web sayfası.
* IOT Edge çalışma zamanı, bir Linux sanal makinesi (VM) yükleyin.

### <a name="add-an-iot-edge-device-to-your-solution"></a>IOT Edge cihazı çözümünüze ekleyin.

IOT Edge cihazı için Uzaktan izleme çözüm Hızlandırıcısını eklemek için gidin **Device Explorer** sayfasında web kullanıcı Arabiriminde ve tıklayın **+ yeni cihaz**.

İçinde **yeni cihaz** panelinde öğesini **IOT Edge cihazı** girin **Petrol pompa** olarak cihaz kimliği. Diğer ayarlar için varsayılan değerleri bırakabilirsiniz. Ardından **Apply** (Uygula) öğesine tıklayın:

[![IOT Edge cihazı Ekle](./media/iot-accelerators-remote-monitoring-edge/addedgedevice-inline.png)](./media/iot-accelerators-remote-monitoring-edge/addedgedevice-expanded.png#lightbox)

Cihaz bağlantı dizesini not edin, bu öğreticinin sonraki bölümünde gerekir.

Uzaktan izleme çözüm Hızlandırıcısını IOT hub'ı ile bir cihaz kaydettiğinizde, listelenmiş olup **Device Explorer** sayfası Web kullanıcı Arabiriminde:

[![Yeni IOT Edge cihazı](./media/iot-accelerators-remote-monitoring-edge/newedgedevice-inline.png)](./media/iot-accelerators-remote-monitoring-edge/newedgedevice-expanded.png#lightbox)

Çözümde IOT Edge cihazları yönetmeyi kolaylaştırmak için bir cihaz grubu oluşturmak ve IOT Edge cihazı Ekle:

1. Seçin **Petrol pompa** cihaz listesinde **Device Explorer** sayfasında ve ardından **işleri**.

1. Eklemek için bir iş oluşturma **IsEdge** aşağıdaki ayarları kullanarak cihaz etiketi:

    | Ayar | Değer |
    | ------- | ----- |
    | İş     | Tags  |
    | İş Adı | AddEdgeTag |
    | Anahtar     | IsOilPump |
    | Değer   | E     |
    | Tür    | Text  |

    [![Etiket Ekle](./media/iot-accelerators-remote-monitoring-edge/addtag-inline.png)](./media/iot-accelerators-remote-monitoring-edge/addtag-expanded.png#lightbox)

1. Tıklayın **uygulamak**, ardından **Kapat**.

1. Üzerinde **Device Explorer** sayfasında **cihaz gruplarını yönetme**.

1. Tıklayın **yeni cihaz grubu oluşturma**. Aşağıdaki ayarlarla yeni bir cihaz grubu oluşturun:

    | Ayar | Değer |
    | ------- | ----- |
    | Ad    | OilPumps |
    | Alan   | Tags.IsOilPump |
    | İşleç | = Eşittir |
    | Değer    | E |
    | Tür     | Text |

    [![Cihaz grubu oluşturma](./media/iot-accelerators-remote-monitoring-edge/createdevicegroup-inline.png)](./media/iot-accelerators-remote-monitoring-edge/createdevicegroup-expanded.png#lightbox)

1. **Kaydet**’e tıklayın.

IOT Edge cihazı artık zamanı **OilPumps** grubu.

### <a name="install-the-edge-runtime"></a>Edge çalışma zamanını yükleme

Bir Edge cihazının Edge çalışma zamanı yüklü olmasını gerektirir. Bu öğreticide, senaryoyu test etmek için bir Azure Linux VM'de Edge çalışma zamanını yükleyin. Aşağıdaki adımlar, yükleme, Azure cloud Shell'i kullanmak ve VM yapılandırma:

1. Azure'da bir Linux VM oluşturmak için aşağıdaki komutları çalıştırın. Nerede yakın bir konum kullanabilirsiniz:

    ```azurecli-interactive
    az group create \
      --name IoTEdgeDevices \
      --location eastus
    az vm create \
      --resource-group IoTEdgeDevices \
      --name EdgeVM \
      --image microsoft_iot_edge:iot_edge_vm_ubuntu:ubuntu_1604_edgeruntimeonly:latest \
      --admin-username azureuser \
      --generate-ssh-keys \
      --size Standard_B1ms
    ```

1. Edge çalışma zamanı ile cihaz bağlantı dizesini yapılandırmak için daha önce Not yapılan cihaz bağlantı dizesini kullanarak şu komutu çalıştırın:

    ```azurecli-interactive
    az vm run-command invoke \
      --resource-group IoTEdgeDevices \
      --name EdgeVM \
      --command-id RunShellScript \
      --scripts 'sudo /etc/iotedge/configedge.sh "YOUR_DEVICE_CONNECTION_STRING"'
    ```

    Çift tırnak işareti içinde bağlantı dizesini eklediğinizden emin olun.

Artık, yüklü ve IOT Edge çalışma zamanı bir Linux cihaz üzerinde yapılandırılan. Bu öğreticide daha sonra bu cihaza IOT Edge modüllerini dağıtmak için Uzaktan izleme çözümü kullanın.

## <a name="create-an-edge-manifest"></a>Bir Edge bildirimi oluşturma

Petrol jack pompa cihaz benzetimini yapmak için Edge cihazınıza aşağıdaki modüller eklemeniz gerekir:

* Benzetim modülü sıcaklık.
* Azure Stream Analytics anomali algılama.

Aşağıdaki adımlar bu modüller içeren bir kenar dağıtım bildirimi oluşturmak nasıl gösterir. Bu öğreticinin ilerleyen bölümlerinde bu bildirimi bir paketi Uzaktan izleme çözüm Hızlandırıcısını olarak alın.

### <a name="create-the-azure-stream-analytics-job"></a>Azure Stream Analytics işi oluşturma

Bir Edge modülü paketlemeden önce portalda Stream Analytics işi tanımlayın.

1. Azure portalında, varsayılan seçenekleri kullanarak bir Azure depolama hesabı oluşturma **IoTEdgeDevices** kaynak grubu. Seçtiğiniz adı not edin.

1. Azure portalında oluşturma bir **Stream Analytics işi** içinde **IoTEdgeDevices** kaynak grubu. Yapılandırma değerleri aşağıdaki kullanın:

    | Seçenek | Değer |
    | ------ | ----- |
    | İş adı | EdgeDeviceJob |
    | Abonelik | Azure aboneliğiniz |
    | Kaynak grubu | IoTEdgeDevices |
    | Location | Doğu ABD |
    | Barındırma ortamı | EDGE |
    | Akış birimleri | 1 |

1. Açık **EdgeDeviceJob** Stream Analytics portalında işi, girişleri tıklayın ve Ekle bir **Edge hub'ı** adlı giriş akışı **telemetri**.

1. İçinde **EdgeDeviceJob** portalında Stream Analytics işi tıklayın **çıkışları** ve ekleme bir **Edge hub'ı** adlı çıktı **çıkış**.

1. İçinde **EdgeDeviceJob** portalında Stream Analytics işi tıklayın **çıkışları** ve ikinci bir ekleme **Edge hub'ı** adlı çıktı **uyarı**.

1. İçinde **EdgeDeviceJob** portalında Stream Analytics işi tıklayın **sorgu** ve aşağıdakileri ekleyin **seçin** deyimi:

    ```sql
    SELECT  
    avg(machine.temperature) as temperature, 
    'F' as temperatureUnit 
    INTO output 
    FROM telemetry TIMESTAMP BY timeCreated 
    GROUP BY TumblingWindow(second, 5) 
    SELECT 'reset' as command 
    INTO alert 
    FROM telemetry TIMESTAMP BY timeCreated 
    GROUP BY TumblingWindow(second, 3) 
    HAVING avg(machine.temperature) > 400
    ```

1. İçinde **EdgeDeviceJob** portalında Stream Analytics işi tıklayın **depolama hesabı ayarlarını**. Eklediğiniz depolama hesabı ekleme **IoTEdgeDevices** kaynak grubu, bu bölümün başlangıç olarak. Adlı yeni bir kapsayıcı oluşturmak **edgeconfig**.

Aşağıdaki ekran görüntüsünde, kaydedilmiş Stream Analytics işi gösterir:

[![Stream Analytics işi](./media/iot-accelerators-remote-monitoring-edge/streamjob-inline.png)](./media/iot-accelerators-remote-monitoring-edge/streamjob-expanded.png#lightbox)

Edge Cihazınızda çalıştırmak için bir Stream Analytics işi şimdi tanımladınız. İş 5 saniyelik pencere üzerinde ortalama sıcaklık hesaplar. 400 3 saniyelik bir pencerede ortalama sıcaklık aşması durumunda iş aynı zamanda bir uyarı gönderir.

### <a name="create-the-iot-edge-deployment"></a>IOT Edge dağıtımı oluşturma

Ardından, Edge Cihazınızda çalıştırmak için modülleri tanımlayan bir IOT Edge dağıtımı bildirimi oluşturun. Sonraki bölümde, bir paketi Uzaktan izleme çözümü olarak bu bildirimi alın.

1. Azure portalında, Uzaktan izleme çözümünüzü IOT hub'ına gidin. IOT hub'ı Uzaktan izleme çözümünüzü aynı ada sahip kaynak grubunda bulabilirsiniz.

1. IOT hub'ında tıklayın **IOT Edge** içinde **otomatik cihaz Yönetimi** bölümü. Tıklayın **bir IOT Edge dağıtımı Ekle**.

1. Üzerinde **dağıtım oluşturma > ad ve etiket** sayfasında, bir ad girin **Petrol pompa cihaz**. **İleri**’ye tıklayın.

1. Üzerinde **dağıtım oluşturma > Ekle modülleri** sayfasında **+ Ekle**. Seçin **IOT Edge Modülü**.

1. İçinde **IOT Edge özel modüller** panelinde, girin **sıcaklık algılayıcısı** adı olarak ve **asaedgedockerhubtest/asa-edge-test-module:sensor-ad-linux-amd64** olarak Görüntü URI'si. **Kaydet**’e tıklayın.

1. Üzerinde **dağıtım oluşturma > Ekle modülleri** sayfasında **+ Ekle** ikinci bir modül eklemek için. Seçin **Azure Stream Analytics Modülü**.

1. İçinde **kenar dağıtım** panelinde, aboneliğinizi seçin ve **EdgeDeviceJob** önceki bölümde oluşturduğunuz. **Kaydet**’e tıklayın.

1. Üzerinde **dağıtım oluşturma > Ekle modülleri** sayfasında **sonraki**.

1. Üzerinde **dağıtım oluşturma > Rota belirtme** sayfasında, aşağıdaki kodu ekleyin:

    ```sql
    {
      "routes": { 
        "alertsToReset": "FROM /messages/modules/EdgeDeviceJob/outputs/alert INTO BrokeredEndpoint(\"/modules/temperatureSensor/inputs/control\")", 
        "telemetryToAsa": "FROM /messages/modules/temperatureSensor/* INTO BrokeredEndpoint(\"/modules/EdgeDeviceJob/inputs/telemetry\")", 
        "ASAToCloud": "FROM /messages/modules/EdgeDeviceJob/* INTO $upstream" 
      } 
    }
    ```

    Bu kod, Stream Analytics modülü çıkışı doğru konuma yönlendirir.

    **İleri**’ye tıklayın.

1. Üzerinde **dağıtım oluşturma > belirtin ölçümleri** sayfasında **sonraki**.

1. Üzerinde **dağıtım oluşturma > hedef cihazlar** sayfasında, öncelikli olarak 10 girin. **İleri**’ye tıklayın.

1. Üzerinde **dağıtım oluşturma > İnceleme dağıtım** sayfasında **Gönder**:

    [![Dağıtım gözden geçirin](./media/iot-accelerators-remote-monitoring-edge/reviewdeployment-inline.png)](./media/iot-accelerators-remote-monitoring-edge/reviewdeployment-expanded.png#lightbox)

1. Ana **IOT Edge** sayfasında **IOT Edge dağıtımları**. Gördüğünüz **Petrol pompa cihaz** dağıtımları listesinde.

1. Tıklayın **Petrol pompa cihaz** dağıtım ve ardından **indirme IOT Edge bildirimi**. Dosyayı Farklı Kaydet **Petrol pompa device.json** için yerel makinenizde uygun bir konum. Bu öğreticinin sonraki bölümünde bu dosya gerekir.

Uzaktan izleme çözümünün bir paket olarak almak için bir IOT Edge bildirimi oluşturdunuz. Genellikle, bir geliştirici IOT Edge modülleri ve bildirim dosyası oluşturur.

## <a name="import-a-package"></a>Paketi içeri aktarma

Bu bölümde, bir paketi Uzaktan izleme çözümü olarak Edge bildirim alın.

1. Uzaktan izleme web kullanıcı Arabiriminde, gitmek **paketleri** sayfasında ve tıklayın **+ yeni paketi**:

    [![Yeni paket](./media/iot-accelerators-remote-monitoring-edge/newpackage-inline.png)](./media/iot-accelerators-remote-monitoring-edge/newpackage-expanded.png#lightbox)

1. Üzerinde **yeni paket** panelinde öğesini **Edge bildirim** paket türü ' ı tıklatın **Gözat** bulmak için **Petrol pompa device.json** dosya çubuğunda yerel makine ve tıklatın **karşıya**:

    [![Paket karşıya yükleme](./media/iot-accelerators-remote-monitoring-edge/uploadpackage-inline.png)](./media/iot-accelerators-remote-monitoring-edge/uploadpackage-expanded.png#lightbox)

    Artık paketler listesini içeren **Petrol pompa device.json** paket.

Sonraki bölümde, Edge cihazınıza paket geçerli bir dağıtım oluşturun.

## <a name="deploy-a-package"></a>Bir paketi dağıtmak

Artık paket cihazınıza dağıtmaya hazırsınız.

1. Uzaktan izleme web kullanıcı Arabiriminde, gitmek **dağıtımları** sayfasında ve tıklayın **+ yeni dağıtım**:

    [![Yeni dağıtım](./media/iot-accelerators-remote-monitoring-edge/newdeployment-inline.png)](./media/iot-accelerators-remote-monitoring-edge/newdeployment-expanded.png#lightbox)

1. İçinde **yeni dağıtım** panelinde, aşağıdaki ayarlara sahip bir dağıtım oluşturun:

    | Seçenek | Değer |
    | ------ | ----- |
    | Ad   | OilPumpDevices |
    | Paket türü | Edge bildirimi |
    | Paket | Petrol pompa device.json |
    | Cihaz grubu | OilPumps |
    | Öncelik | 10 |

    [![Dağıtım oluşturma](./media/iot-accelerators-remote-monitoring-edge/createdeployment-inline.png)](./media/iot-accelerators-remote-monitoring-edge/createdeployment-expanded.png#lightbox)

    **Uygula**'ya tıklayın.

Pakette cihazınıza dağıtmaya ve telemetri CİHAZDAN akışa başlamak için birkaç dakika beklemeniz gerekir.

[![Etkin dağıtım](./media/iot-accelerators-remote-monitoring-edge/deploymentactive-inline.png)](./media/iot-accelerators-remote-monitoring-edge/deploymentactive-expanded.png#lightbox)

**Dağıtımları** sayfası aşağıdaki ölçümleri gösterir:

* **Hedeflenen** cihaz grubundaki cihazların sayısını gösterir.
* **Uygulanan** uygulanan dağıtım içeriğine kalmışlardır cihazların sayısını gösterir.
* **Başarılı** IOT Edge istemci çalışma zamanı başarı raporlama dağıtımdaki uç cihazlarına sayısını gösterir.
* **Başarısız** IOT Edge istemci çalışma zamanı hatasından raporlama dağıtımdaki uç cihazlarına sayısını gösterir.

## <a name="monitor-the-device"></a>Cihaz izleme

Uzaktan izleme Web kullanıcı Arabiriminde Petrol pompa cihazınızdan sıcaklık telemetri görüntüleyebilirsiniz:

1. Gidin **Device Explorer** sayfasında ve Petrol pompa Cihazınızı seçin.
1. İçinde **Telemetri** bölümünü **cihaz ayrıntıları** panelinde, tıklayın **sıcaklık**:

    [![Telemetri görüntüleme](./media/iot-accelerators-remote-monitoring-edge/viewtelemetry-inline.png)](./media/iot-accelerators-remote-monitoring-edge/viewtelemetry-expanded.png#lightbox)

Bir eşiğe ulaşması kadar sıcaklık nasıl yükseldiğinde görebilirsiniz. Stream Analytics Edge modülü sıcaklık, bu eşiğe ulaşması ve sıcaklık hemen azaltmak için cihaza bir komut gönderir algılar. Tüm bu işlemleri gerçekleşir cihazda bulutla iletişim kurmadan.

İşleçler Eşiğe ulaşıldığında bildirmek istiyorsanız, Uzaktan izleme web UI'da bir kural oluşturabilirsiniz:

1. Gidin **kuralları** sayfasında ve tıklayın **+ yeni kural**.
1. Aşağıdaki ayarlarla yeni bir kural oluşturun:

    | Seçenek | Değer |
    | ------ | ----- |
    | Kural adı | Petrol pompa sıcaklık |
    | Açıklama | Petrol pompa sıcaklık 300 aşıldı |
    | Cihaz grubu | OilPumps |
    | Hesaplama | Anında |
    | Alan | sıcaklık |
    | İşleç | > |
    | Değer | 300 |
    | Önem düzeyi | Bilgi |

    [![Kural oluşturma](./media/iot-accelerators-remote-monitoring-edge/newrule-inline.png)](./media/iot-accelerators-remote-monitoring-edge/newrule-expanded.png#lightbox)

    **Uygula**'ya tıklayın.

1. Gidin **Pano** sayfası. Bir uyarı gösterir **uyarılar** ne zaman panelinde sıcaklık **Petrol pompa** cihaz 300'den geçer.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide ekleme ve bir IOT Edge cihazı Uzaktan izleme çözüm hızlandırıcısının yapılandırma nasıl oluşturulacağını gösterir. Uzaktan izleme çözümünde IOT Edge paketleri ile çalışma hakkında daha fazla bilgi için aşağıdaki nasıl yapılır kılavuzuna bakın:

> [!div class="nextstepaction"]
> [Bir IOT Edge paketi Uzaktan izleme çözüm Hızlandırıcısını alma](iot-accelerators-remote-monitoring-import-edge-package.md)

IOT Edge çalışma zamanı yükleme hakkında daha fazla bilgi edinmek için [(x64) Linux üzerinde Azure IOT Edge çalışma zamanı yükleme](../iot-edge/how-to-install-iot-edge-linux.md).

Uç cihazlarda Azure Stream Analytics hakkında daha fazla bilgi edinmek için bkz. [Azure Stream Analytics IOT Edge modülü olarak dağıtma](../iot-edge/tutorial-deploy-stream-analytics.md).
