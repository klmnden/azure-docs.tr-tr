---
title: Bağlı Fabrika ağ geçidi - Azure'ı dağıtma | Microsoft Docs
description: Bağlı Fabrika çözüm Hızlandırıcısını bağlantıyı etkinleştirmek için Windows veya Linux üzerinde bir ağ geçidi dağıtma
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 03/08/2019
ms.author: dobett
ms.openlocfilehash: 99953b486fbd1daa9800aa850684447465eead9e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61450201"
---
# <a name="deploy-an-edge-gateway-for-the-connected-factory-solution-accelerator-on-windows-or-linux"></a>Bir edge ağ geçidi için Windows veya Linux üzerinde bağlı Fabrika çözüm Hızlandırıcısını dağıtma

Bir edge ağ geçidi dağıtmak için iki yazılım bileşenlerini ihtiyacınız *Connected Factory* Çözüm Hızlandırıcısı:

-  *OPC Proxy* Connected Factory bir bağlantı kurar. OPC Proxy çalıştıran bağlı Fabrika çözüm portalında tümleşik OPC Browser komut ve Denetim iletileri ardından bekler.

-  *OPC yayımcı* iletir telemetri iletilerini bunları bağlı Fabrika için ve mevcut şirket içi OPC UA sunucularına bağlanır. Bir OPC Klasik cihaz kullanarak bağlantı kurabilir [OPC UA için OPC Klasik bağdaştırıcısı](https://github.com/OPCFoundation/UA-.NETStandard/blob/master/ComIOP/README.md).

Her iki bileşenler açık kaynaklı ve GitHub üzerinde kaynak ve DockerHub Docker kapsayıcıları olarak kullanılabilir:

| GitHub | DockerHub |
| ------ | --------- |
| [OPC Publisher](https://github.com/Azure/iot-edge-opc-publisher) | [OPC Publisher](https://hub.docker.com/r/microsoft/iot-edge-opc-publisher/)   |
| [OPC Proxy](https://github.com/Azure/iot-edge-opc-proxy)         | [OPC Proxy](https://hub.docker.com/r/microsoft/iot-edge-opc-proxy/) |

Her iki bileşen için genel kullanıma yönelik IP adresi veya ağ geçidi Güvenlik Duvarı'nda açık gelen bağlantı noktası gerekmez. OPC Proxy ve OPC yayımcısı bileşenlerini yalnızca giden bağlantı noktası 443'ü kullanın.

Bu makaledeki adımlarda, Windows veya Linux'ta Docker'ı kullanarak bir sınır ağ geçidi dağıtma gösterilmektedir. Ağ geçidi bağlı Fabrika çözüm Hızlandırıcısını bağlantısı sağlar. Bağlı Fabrika bileşenleri de kullanabilirsiniz.

> [!NOTE]
> Her iki bileşenin modülleri olarak kullanılabilir [Azure IOT Edge](https://github.com/Azure/iot-edge).

## <a name="choose-a-gateway-device"></a>Bir ağ geçidi cihazını seçin

Bir ağ geçidi cihazı henüz yoksa, Microsoft ticari ağ geçidi ortaklarından birinden satın önerir. Bağlı Fabrika çözümü ile uyumlu bir ağ geçidi cihazlar listesi için ziyaret [Azure IOT cihaz Kataloğu](https://catalog.azureiotsolutions.com/?q=opc). Ağ geçidini ayarlamak için aygıtla birlikte gelen yönergeleri izleyin.

Var olan bir ağ geçidi cihazı el ile yapılandırmanız gerekiyorsa, aşağıdaki yönergeleri kullanın.

## <a name="install-and-configure-docker"></a>Yükleme ve Docker'ı yapılandırma

Yükleme [için Docker Windows](https://www.docker.com/docker-windows) Windows tabanlı ağ geçidi cihazı veya Linux tabanlı ağ geçidi cihazı docker yüklemek için bir paket Yöneticisi'ni kullanın.

Docker için Windows Kurulumu sırasında ana makinenizde Docker ile paylaşmak için bir sürücü seçin. Aşağıdaki ekran görüntüsünde gösterilmiştir paylaşımı **D** sürücü Windows sisteminize. Bir sürücü paylaşımı bir docker kapsayıcısı içinde konak sürücüden erişim sağlar:

![Windows için Docker'ı yükleyin](./media/iot-accelerators-connected-factory-gateway-deployment/image1.png)

> [!NOTE]
> Docker'ndan yükledikten sonra bu adımı gerçekleştirebilir **ayarları** iletişim. Sağ **Docker** Windows Sistem tepsisindeki simgeye ve **ayarları**. Windows Fall Creators update gibi önemli Windows güncelleştirmelerini sisteme dağıtılmışsa, sürücüleri paylaşımını ve erişim haklarını yeniden yenilemek için paylaşın.

Linux kullanıyorsanız, dosya sistemi erişimi etkinleştirmek için ek yapılandırma gerekir.

Windows, sürücüdeki Docker ile paylaşılan bir klasör oluşturun, Linux'ta kök dosya sistemine altında bir klasör oluşturun. Bu kılavuzda bu klasöre başvuruyor `<SharedFolder>`.

Ne zaman başvurmak için `<SharedFolder>` işletim sisteminiz için doğru sözdizimi kullanmak bir Docker komut unutmayın. İki örnek, bir Windows için ve biri Linux için aşağıda verilmiştir:

- Varsa olduğunuz klasörü kullanılarak `D:\shared` Windows üzerinde `<SharedFolder>`, Docker komut söz dizimi `d:/shared`.

- Varsa olduğunuz klasörü kullanılarak `/shared` Linux'ta, `<SharedFolder>`, Docker komut söz dizimi `/shared`.

Daha fazla bilgi için [birimler kullanmak](https://docs.docker.com/engine/admin/volumes/volumes/) docker altyapısı başvurusu.

## <a name="configure-the-opc-components"></a>OPC bileşenlerini yapılandırma

OPC bileşenlerini yüklemeden önce ortamınızı hazırlamak için aşağıdaki adımları tamamlayın:

1. Ağ geçidi dağıtımı tamamlamak için ihtiyacınız **iothubowner** Connected Factory dağıtımınızdaki IOT hub'ı bağlantı dizesi. İçinde [Azure portalında](https://portal.azure.com/), bağlı Fabrika çözümünü dağıttığınızda oluşturulan kaynak grubunda IOT hub'ınıza gidin. Tıklayın **paylaşılan erişim ilkeleri** erişimi **iothubowner** bağlantı dizesi:

    ![IOT Hub bağlantı dizesini bulun](./media/iot-accelerators-connected-factory-gateway-deployment/image2.png)

    Kopyalama **bağlantı dizesi-birincil anahtar** değeri.

1. Docker kapsayıcıları arasında iletişime izin vermek için bir kullanıcı tanımlı köprü ağı gerekir. Kapsayıcılarınızı köprü ağı oluşturmak için bir komut isteminde aşağıdaki komutları çalıştırın:

    ```cmd/sh
    docker network create -d bridge iot_edge
    ```

    Doğrulanacak **iot_edge** köprü ağı oluşturulduğu, aşağıdaki komutu çalıştırın:

    ```cmd/sh
    docker network ls
    ```

    **İot_edge** köprü ağı ağlar listesinde yer almaktadır.

OPC Publisher'ı çalıştırmak için bir komut isteminde aşağıdaki komutu çalıştırın:

```cmd/sh
docker run --rm -it -v <SharedFolder>:/docker -v x509certstores:/root/.dotnet/corefx/cryptography/x509stores --network iot_edge --name publisher -h publisher -p 62222:62222 --add-host <OpcServerHostname>:<IpAddressOfOpcServerHostname> microsoft/iot-edge-opc-publisher:2.1.4 publisher "<IoTHubOwnerConnectionString>" --lf /docker/publisher.log.txt --as true --si 1 --ms 0 --tm true --vc true --di 30
```

- [OPC yayımcı GitHub](https://github.com/Azure/iot-edge-opc-publisher) ve [docker run başvuru](https://docs.docker.com/engine/reference/run/) hakkında daha fazla bilgi sağlar:

  - Belirtilen kapsayıcı adından önce docker komut satırı seçenekleri (`microsoft/iot-edge-opc-publisher:2.1.4`).
  - Belirtilen kapsayıcı adından sonra OPC yayımcı komut satırı parametreleri anlamını (`microsoft/iot-edge-opc-publisher:2.1.4`).

- `<IoTHubOwnerConnectionString>` Olduğu **iothubowner** paylaşılan erişim ilkesi bağlantı dizesini Azure portalından. Bu bağlantı dizesi bir önceki adımda kopyaladığınız. OPC Publisher'ın ilk çalıştırmak için yalnızca bu bağlantı dizesi gerekir. Bir güvenlik riski oluşturduğundan sonraki çalışır, bu atmanız gerekir.

- `<SharedFolder>` Kullanın ve söz dizimini bölümünde açıklanan [yükleyin ve Docker yapılandırma](#install-and-configure-docker). OPC Publisher kullandığı `<SharedFolder>` için:

    - Okuma ve yazma için OPC yayımcı yapılandırma dosyası.
    - Günlük dosyasına yazın.
    - Bu dosyaların her kapsayıcının dışından erişilebilir.

- OPC Publisher okur yapılandırmasını **publishednodes.json** okunan ve yazılan dosyasını `<SharedFolder>/docker` klasör. Bu yapılandırma dosyası, OPC yayımcı abone olmalıdır, belirli bir OPC UA sunucu üzerindeki OPC UA düğüm veri tanımlar. Tam sözdizimini **publishednodes.json** dosya çubuğunda açıklanmıştır [OPC yayımcı](https://github.com/Azure/iot-edge-opc-publisher) GitHub sayfasında. Bir ağ geçidi eklediğinizde, boş bir yerleştirme **publishednodes.json** klasörüne:

    ```json
    [
    ]
    ```

- OPC UA sunucusu OPC yayımcısı veri değişikliği bildirir her yeni değerini IOT Hub'ına gönderilir. Toplu işlem ayarlara bağlı olarak, öncelikle OPC yayımcı accumulate verileri, verilerin IOT Hub'ına bir toplu işte gönderilmeden önce.

- Docker, NetBIOS ad çözümlemesi, yalnızca DNS ad çözümlemesi desteklemiyor. Ağda bir DNS sunucusu yoksa, komut satırı önceki örnekte gösterilen geçici çözüm kullanabilirsiniz. Önceki komut satırı örnekte `--add-host` kapsayıcıları hosts dosyasına bir giriş eklemek için parametre. Bu giriş için ana bilgisayar adı arama sağlayan belirli `<OpcServerHostname>`, belirli IP adresi çözümlenirken `<IpAddressOfOpcServerHostname>`.

- OPC UA kimlik doğrulama ve şifreleme için X.509 sertifikaları kullanır. OPC yayımcı sertifikası, OPC yayımcı güvenleri emin olmak için bağlandığınız OPC UA sunucusuna yerleştirin. OPC Publisher sertifika deposunda bulunan `<SharedFolder>/CertificateStores` klasör. OPC Publisher sertifikayı bulabilirsiniz `trusted/certs` klasöründe `CertificateStores` klasör.

  OPC UA sunucusu yapılandırma adımları, kullanmakta olduğunuz cihaz üzerinde bağlıdır. Bu adımlar, OPC UA server'ın kullanıcı el genellikle belgelenmiştir.

OPC Proxy yüklemek için bir komut isteminde aşağıdaki komutu çalıştırın:

```cmd/sh
docker run -it --rm -v <SharedFolder>:/mapped --network iot_edge --name proxy --add-host <OpcServerHostname>:<IpAddressOfOpcServerHostname> microsoft/iot-edge-opc-proxy:1.0.4 -i -c "<IoTHubOwnerConnectionString>" -D /mapped/cs.db
```

Yüklemeyi bir kez bir sistemde çalıştırılması yeterlidir.

OPC Proxy çalıştırmak için aşağıdaki komutu kullanın:

```cmd/sh
docker run -it --rm -v <SharedFolder>:/mapped --network iot_edge --name proxy --add-host <OpcServerHostname>:<IpAddressOfOpcServerHostname> microsoft/iot-edge-opc-proxy:1.0.4 -D /mapped/cs.db
```

OPC Proxy yüklenirken bağlantı dizesini kaydeder. Bir güvenlik riski oluşturduğundan sonraki çalışır, bağlantı dizesini atmanız gerekir.

## <a name="enable-your-gateway"></a>Ağ geçidini etkinleştir

Bağlı Fabrika Çözüm Hızlandırıcısı, ağ geçidi'ni etkinleştirmek için aşağıdaki adımları tamamlayın:

1. Her iki bileşenin çalıştırırken Gözat **kendi OPC UA sunucusunu bağlama** bağlı Fabrika çözüm portalında sayfası. Bu sayfa yalnızca çözümde yöneticiler tarafından kullanılabilir. Yayımcı uç nokta URL'sini girin (opc.tcp://publisher: 62222) tıklayıp **Connect**.

1. OPC Publisher ve bağlı Fabrika portal arasında bir güven ilişkisi oluşturun. Bir sertifika uyarısı gördüğünüzde tıklayın **İlerle**. OPC Publisher UA Web istemcisi güvenmediğini bildiren bir hata görürsünüz. Bu hatayı gidermek için kopyalama **UA Web istemcisi** gelen sertifika `<SharedFolder>/CertificateStores/rejected/certs` klasörüne `<SharedFolder>/CertificateStores/trusted/certs` ağ geçidi üzerinde klasör. Ağ geçidini yeniden başlatma gerekmez.

Artık buluttan ağ geçidine bağlanabilir ve OPC UA sunucuları çözüme eklemek hazırsınız.

## <a name="add-your-own-opc-ua-servers"></a>Kendi OPC UA sunucuları ekleme

Bağlı Fabrika çözüm hızlandırıcısına kendi OPC UA sunucuları eklemek için:

1. Gözat **kendi OPC UA sunucusunu bağlama** bağlı Fabrika çözüm portalında sayfası.

    1. OPC UA sunucusu bağlamak istediğiniz başlatın. OPC Publisher ve OPC Proxy kapsayıcısı içinde çalışırken OPC UA sunucunuzu erişilebildiğini kontrol edin. Ad çözümlemesi hakkında önceki yorumlara bakın.
    1. OPC UA sunucunuzun uç nokta URL'sini girin (`opc.tcp://<host>:<port>`) tıklayıp **Connect**.
    1. Bağlantı Kurulum işlemi bağlı Fabrika portal (OPC UA istemcisi) ve OPC UA sunucusuna bağlanmaya çalıştığınız arasında bir güven ilişkisi oluşturur. Bağlı Fabrika Panoda size bir **bağlanmak istediğiniz sunucunun sertifikası doğrulanamıyor** uyarı. Bir sertifika uyarısı gördüğünüzde tıklayın **İlerle**.
    1. Sertifika yapılandırması, bağlanmaya çalıştığınız OPC UA sunucusunun kurulumunu daha zordur. Bilgisayar tabanlı OPC UA sunucuları için kabul etmek Panoda yalnızca bir uyarı iletişim kutusu alabilirsiniz. Katıştırılmış OPC UA sunucu sistemleri için bu görevi nasıl yapıldığını aramak için OPC UA sunucunuzun belgelerine bakın. Bu görevi tamamlamak için bağlı Fabrika portal'ın OPC UA istemci sertifikasını gerekebilir. Bu sertifika indirebilirsiniz yönetici **kendi OPC UA sunucusunu bağlama** sayfası:

        ![Çözüm portalı](./media/iot-accelerators-connected-factory-gateway-deployment/image4.png)

1. OPC UA sunucunuzun OPC UA düğümleri ağacına göz atın, sağ tıklayın, istediğiniz değerleri bağlı Fabrika için Gönder ve OPC düğümlerine **yayımlama**.

1. Telemetri, ağ geçidi cihazı artık akar. Telemetriyi görüntüleyebilirsiniz **Fabrika konumları** Connected Factory portalının görünümünü **yeni Fabrika**.

## <a name="next-steps"></a>Sonraki adımlar

Bağlı Fabrika çözüm Hızlandırıcısını mimarisi hakkında daha fazla bilgi için bkz: [bağlı Fabrika Çözüm Hızlandırıcısı Kılavuzu](iot-accelerators-connected-factory-sample-walkthrough.md).

Hakkında bilgi edinin [OPC Publisher başvuru uygulaması](https://docs.microsoft.com/azure/iot-suite/iot-suite-connected-factory-publisher).