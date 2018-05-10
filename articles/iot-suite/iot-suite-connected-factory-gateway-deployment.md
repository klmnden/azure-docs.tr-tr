---
title: Fabrika bağlı ağ geçidiniz - Azure dağıtma | Microsoft Docs
description: Nasıl bir ağ geçidi bağlı Fabrika Çözüm Hızlandırıcısı bağlantıyı etkinleştirmek için Windows veya Linux üzerinde dağıtılır.
services: iot-suite
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: ''
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/17/2018
ms.author: dobett
ms.openlocfilehash: 956da99a5d67d7a2225ab3ea64b4e5a9d41ee3a1
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="deploy-an-edge-gateway-for-the-connected-factory-solution-accelerator-on-windows-or-linux"></a>Windows veya Linux bağlı Fabrika Çözüm Hızlandırıcısı için bir sınır ağ geçidi dağıtma

İki yazılım bileşenleri için bir sınır ağ geçidi dağıtmak için gereken *bağlı Fabrika* Çözüm Hızlandırıcısı:

- *OPC Proxy* bağlı Fabrika bir bağlantı kurar. OPC Proxy sonra komut ve Denetim iletileri bağlı Fabrika çözüm Portalı'nda çalışan tümleşik OPC tarayıcıdan bekler.

- *OPC yayımcı* var olan şirket içi OPC UA sunucularına bağlanır ve iletir telemetri iletilerini onlardan bağlı üreteci. Bir OPC Klasik aygıt kullanarak bağlanabilir [OPC Klasik bağdaştırıcısı OPC UA için](https://github.com/OPCFoundation/UA-.NETStandard/blob/master/ComIOP/README.md).

Her iki bileşenler açık kaynaklı ve GitHub kaynağına ve DockerHub Docker kapsayıcılarında olarak kullanılabilir:

| GitHub | DockerHub |
| ------ | --------- |
| [OPC yayımcı](https://github.com/Azure/iot-edge-opc-publisher) | [OPC yayımcı](https://hub.docker.com/r/microsoft/iot-edge-opc-publisher/)   |
| [OPC Proxy](https://github.com/Azure/iot-edge-opc-proxy)         | [OPC Proxy](https://hub.docker.com/r/microsoft/iot-edge-opc-proxy/) |

Ya da bileşen için genel kullanıma yönelik IP adresini veya ağ geçidi Güvenlik Duvarı'nda Aç gelen bağlantı noktalarının gerekmez. OPC Proxy ve OPC yayımcı bileşenleri, yalnızca giden bağlantı noktası 443'ü kullanın.

Bu makaledeki adımları Windows ya da Linux Docker kullanarak bir sınır ağ geçidi dağıtılacağı gösterilmektedir. Ağ geçidi bağlı Fabrika Çözüm Hızlandırıcısı bağlantı sağlar. Bağlı Fabrika bileşenleri de kullanabilirsiniz.

> [!NOTE]
> Her iki bileşenin modülleri olarak kullanılabilir [Azure IOT kenar](https://github.com/Azure/iot-edge).

## <a name="choose-a-gateway-device"></a>Bir ağ geçidi cihazı seçin

Bir ağ geçidi cihazı henüz yoksa, Microsoft ticari bir ağ geçidi ortaklarından birinden satın önerir. Ağ geçidi aygıtların bağlı Fabrika çözümüyle uyumlu bir listesi için ziyaret [Azure IOT cihaz katalog](https://catalog.azureiotsuite.com/?q=opc). Ağ geçidi kurun aygıtla birlikte gelen yönergeleri izleyin.

Alternatif olarak, var olan bir ağ geçidi aygıtı el ile yapılandırmak için aşağıdaki yönergeleri kullanın.

## <a name="install-and-configure-docker"></a>Yükleme ve Docker yapılandırma

Yükleme [Windows için Docker](https://www.docker.com/docker-windows) Windows tabanlı ağ geçidi aygıtı veya Linux tabanlı ağ geçidi aygıtınızda docker yüklemek için bir paket Yöneticisi'ni kullanın.

Windows için Docker Kurulum sırasında Docker ile paylaşmak için konak makinesi üzerinde bir sürücü seçin. Aşağıdaki ekran gösterilir paylaşımı **D** sürücü Windows sisteminizde docker kapsayıcısı içinde ana bilgisayar sürücüsünden erişmesine izin vermek için:

![Windows için Docker yükleyin](./media/iot-suite-connected-factory-gateway-deployment/image1.png)

> [!NOTE]
> Docker gelen yükledikten sonra da bu adımı gerçekleştirebilirsiniz **ayarları** iletişim. Sağ **Docker** Windows sistem tepsisi simgesi ve **ayarları**. Windows sonbaharda oluşturucuları güncelleştirme gibi önemli Windows güncelleştirmelerini sisteme dağıtılmışsa, sürücüleri paylaşımını ve bunları yeniden erişim haklarını yenilemek için paylaşabilir.

Linux kullanıyorsanız, dosya sistemi erişimi etkinleştirmek için ek yapılandırma gerekir.

Windows, Docker ile paylaşılan sürücü üzerinde bir klasör oluşturun, Linux kök dosya sistemi altında bir klasör oluşturun. Bu klasörü olarak bu kılavuzda başvurduğu `<SharedFolder>`.

Ne zaman başvurmak için `<SharedFolder>` Docker komutta, işletim sistemi için doğru söz dizimi kullandığınızdan emin olun. İki örnek, bir Windows ve Linux için bir şunlardır:

- Varsa olduğunuz klasörünü kullanarak `D:\shared` Windows, `<SharedFolder>`, Docker komut sözdizimi `d:/shared`.

- Varsa olduğunuz klasörünü kullanarak `/shared` Linux'ta, `<SharedFolder>`, Docker komut sözdizimi `/shared`.

Daha fazla bilgi için bkz: [birimler kullanmak](https://docs.docker.com/engine/admin/volumes/volumes/) docker altyapısı başvuru.

## <a name="configure-the-opc-components"></a>OPC bileşenlerini yapılandırma

OPC bileşenlerini yüklemeden önce ortamınızı hazırlamak için aşağıdaki adımları tamamlayın:

1. Ağ geçidi dağıtımını tamamlamak için ihtiyacınız **iothubowner** bağlı Fabrika dağıtımınızdaki IOT hub bağlantı dizesi. İçinde [Azure portal](http://portal.azure.com/), IOT hub'ınıza bağlı Fabrika çözümü dağıttığınızda oluşturulan kaynak grubunda gidin. Tıklatın **paylaşılan erişim ilkeleri** erişimi **iothubowner** bağlantı dizesi:

    ![IOT Hub bağlantı dizesini bulun](./media/iot-suite-connected-factory-gateway-deployment/image2.png)

    Kopya **bağlantı dize birincil anahtarı** değeri.

1. Docker kapsayıcıları arasında iletişim sağlamak için bir kullanıcı tarafından tanımlanan köprüsü ağ gerekir. Kapsayıcılarınızı köprüsü ağı oluşturmak için bir komut isteminde aşağıdaki komutları çalıştırın:

    ```cmd/sh
    docker network create -d bridge iot_edge
    ```

    Doğrulamak için **iot_edge** köprüsü ağ oluşturuldu, aşağıdaki komutu çalıştırın:

    ```cmd/sh
    docker network ls
    ```

    **İot_edge** köprüsü ağ ağ listesinde yer almaktadır.

OPC yayımcı çalıştırmak için komut isteminde aşağıdaki komutu çalıştırın:

```cmd/sh
docker run --rm -it -v <SharedFolder>:/docker -v x509certstores:/root/.dotnet/corefx/cryptography/x509stores --network iot_edge --name publisher -h publisher -p 62222:62222 --add-host <OpcServerHostname>:<IpAddressOfOpcServerHostname> microsoft/iot-edge-opc-publisher:2.1.3 publisher "<IoTHubOwnerConnectionString>" --lf /docker/publisher.log.txt --as true --si 1 --ms 0 --tm true --vc true --di 30
```

- [OPC yayımcı GitHub](https://github.com/Azure/iot-edge-opc-publisher) ve [başvuru çalıştırmak docker](https://docs.docker.com/engine/reference/run/) hakkında daha fazla bilgi sağlar:

  - Belirtilen kapsayıcı adı önce docker komut satırı seçenekleri (`microsoft/iot-edge-opc-publisher:2.1.3`).
  - Belirtilen kapsayıcı adı sonra OPC yayımcı komut satırı parametreleri anlamını (`microsoft/iot-edge-opc-publisher:2.1.3`).

- `<IoTHubOwnerConnectionString>` Olan **iothubowner** paylaşılan erişim ilkesi bağlantı dizesi Azure portalından. Bu bağlantı dizesi bir önceki adımda kopyaladığınız. Yalnızca OPC Publisher'ın ilk çalıştırmak için bu bağlantı dizesi gerekir. Bir güvenlik riski oluşturduğundan sonraki çalışır, bu atlayın.

- `<SharedFolder>` Kullanmanız ve sözdizimi bölümünde açıklanan [yükleyin ve Docker yapılandırma](#install-and-configure-docker). OPC Publisher kullandığı `<SharedFolder>` okuma ve yazma OPC yayımcı yapılandırma dosyasına için günlük dosyasına yazmak ve bu dosyaların her kapsayıcı dışında kullanılabilir yapın.

- OPC yayımcı okur yapılandırmasını **publishednodes.json** okuma ve için yazılan dosya `<SharedFolder>/docker` klasör. Bu yapılandırma dosyası OPC UA düğümü veri OPC yayımcıya abone olmalısınız verilen OPC UA sunucusundaki tanımlar. Tam söz dizimi **publishednodes.json** dosya açıklanmaktadır [OPC yayımcı](https://github.com/Azure/iot-edge-opc-publisher) GitHub sayfasında. Bir ağ geçidi eklediğinizde, boş bir yerleştirme **publishednodes.json** klasörüne:

    ```json
    [
    ]
    ```

- Bir veri değişikliği OPC yayımcısına OPC UA sunucusuna bildirir olduğunda, yeni değer IOT Hub'ına gönderilir. Toplu ayarlara bağlı olarak, öncelikle OPC yayımcı accumulate verileri, veri IOT Hub'ına bir toplu işlemde gönderilmeden önce.

- Docker NetBIOS ad çözümlemesi, yalnızca DNS ad çözümlemesi desteklemez. Ağdaki bir DNS sunucusuna sahip değilseniz, önceki komut satırı örnekte gösterilen geçici çözüm kullanabilirsiniz. Önceki komut satırı örnek kullanır `--add-host` kapsayıcıları hosts dosyasına bir giriş eklemek için parametre. Bu giriş için ana bilgisayar adı aramayı etkinleştirir verilen `<OpcServerHostname>`, verilen IP adresi çözümleme `<IpAddressOfOpcServerHostname>`.

- OPC UA, kimlik doğrulama ve şifreleme için X.509 sertifikaları kullanır. OPC yayımcı sertifikası, OPC yayımcı güvenleri emin olmak için bağlandığınız OPC UA sunucuya yerleştirmeye gerekir. OPC yayımcı sertifika deposunda bulunan `<SharedFolder>/CertificateStores` klasör. OPC yayımcı sertifikası bulabilirsiniz `trusted/certs` klasöründe `CertificateStores` klasörü.

  OPC UA sunucu yapılandırmak için gereken adımları kullandığınız cihazda bağlıdır. Bu adımları genellikle OPC UA sunucunun kullanıcı kılavuzda belirtilmiştir.

OPC Proxy yüklemek için komut isteminde aşağıdaki komutu çalıştırın:

```cmd/sh
docker run -it --rm -v <SharedFolder>:/mapped --network iot_edge --name proxy --add-host <OpcServerHostname>:<IpAddressOfOpcServerHostname> microsoft/iot-edge-opc-proxy:1.0.2 -i -c "<IoTHubOwnerConnectionString>" -D /mapped/cs.db
```

Yalnızca yükleme bir sistemde bir kez çalıştırmanız gerekir.

OPC Proxy çalıştırmak için aşağıdaki komutu kullanın:

```cmd/sh
docker run -it --rm -v <SharedFolder>:/mapped --network iot_edge --name proxy --add-host <OpcServerHostname>:<IpAddressOfOpcServerHostname> microsoft/iot-edge-opc-proxy:1.0.2 -D /mapped/cs.db
```

OPC Proxy bağlantı dizesi yükleme sırasında kaydeder. Sonraki çalışır bir güvenlik riski oluşturduğundan bağlantı dizesi atlayın.

## <a name="enable-your-gateway"></a>Ağ geçidini etkinleştir

Bağlı Fabrika Çözüm Hızlandırıcısı, ağ geçidi etkinleştirmek için aşağıdaki adımları tamamlayın:

1. Hem bileşenleri çalıştırırken, Gözat **kendi OPC UA sunucusuna** bağlı Fabrika çözüm portalında sayfası. Bu sayfa yalnızca çözümde yöneticiler tarafından kullanılabilir. Yayımcı uç noktasının URL'sini girin (opc.tcp://publisher: 62222) tıklatıp **Bağlan**.

1. OPC yayımcı ve bağlı Fabrika portal arasında bir güven ilişkisi oluşturun. Bir sertifika uyarısı gördüğünüzde, tıklatın **İlerle**. Ardından, OPC yayımcı UA Web istemcisi güvenilir olmayan bir hata görürsünüz. Bu hatayı gidermek için kopyalama **UA Web istemcisi** gelen sertifika `<SharedFolder>/CertificateStores/rejected/certs` klasörüne `<SharedFolder>/CertificateStores/trusted/certs` ağ geçidinde klasör. Ağ geçidi yeniden başlatmanız gerekmez.

Ağ geçidi buluttan şimdi bağlanabilir ve çözüme OPC UA sunucuları eklemek hazırsınız.

## <a name="add-your-own-opc-ua-servers"></a>Kendi OPC UA sunucuları ekleme

Bağlı Fabrika Çözüm Hızlandırıcısı için kendi OPC UA sunucuları eklemek için:

1. Gözat **kendi OPC UA sunucusuna** bağlı Fabrika çözüm portalında sayfası.

    1. Bağlanmak istediğiniz sunucunun OPC UA başlatın. OPC UA sunucunuz OPC yayımcı ve OPC kapsayıcıda çalışan Proxy ulaşılabildiğinden emin olun (önceki açıklamaları ad çözümlemesi hakkında bakın).
    1. OPC UA sunucunuzun uç nokta URL'sini girin (`opc.tcp://<host>:<port>`) tıklatıp **Bağlan**.
    1. Bağlantı kurulumunun bir parçası olarak, bağlı Fabrika portal (OPC UA istemci) ve bağlanmaya çalıştığınız OPC UA sunucu arasında bir güven ilişkisi oluşturulur. Bağlı Fabrika Panoda size bir **bağlanmak istediğiniz sunucunun sertifikasının doğrulanamıyor** uyarı. Bir sertifika uyarısı gördüğünüzde, tıklatın **İlerle**.
    1. Bağlanmaya çalıştığınız OPC UA sunucusunun sertifika yapılandırması için Kurulum daha zordur. PC tabanlı OPC UA sunucuları için yalnızca bir uyarı iletişim kutusu kabul panosunda elde edebilirsiniz. Katıştırılmış OPC UA server sistemleri için bu görevi nasıl yapıldığını aramak için OPC UA sunucunuzun belgelerine bakın. Bu görevi tamamlamak için Fabrika bağlı portal'ın OPC UA istemci sertifikasını gerekebilir. Bir yönetici üzerinde bu sertifikayı indirebilirsiniz **kendi OPC UA sunucusuna** sayfa:

        ![Çözüm portalı](./media/iot-suite-connected-factory-gateway-deployment/image4.png)

1. OPC UA sunucunuzun OPC UA düğümleri ağaç göz atın, istediğiniz değerleri bağlı Fabrika göndermek ve seçmek için OPC düğümlerinin sağ **yayımlama**.

1. Telemetri şimdi ağ geçidi aygıttan akar. Telemetriyi de görüntüleyebilirsiniz **Fabrika konumları** altında bağlı Fabrika portal görünümünü **yeni Fabrika**.

## <a name="next-steps"></a>Sonraki adımlar

Bağlı Fabrika Çözüm Hızlandırıcısı mimarisi hakkında daha fazla bilgi için bkz: [bağlı Fabrika Çözüm Hızlandırıcısı izlenecek](https://docs.microsoft.com/azure/iot-suite/iot-suite-connected-factory-sample-walkthrough).

Hakkında bilgi edinin [OPC yayımcı başvuru uygulaması](https://docs.microsoft.com/azure/iot-suite/iot-suite-connected-factory-publisher).