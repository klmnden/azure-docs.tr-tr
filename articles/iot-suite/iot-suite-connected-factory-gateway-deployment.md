---
title: "Bağlı Fabrika gateway - Azure dağıtımı | Microsoft Docs"
description: "Bir ağ geçidi bağlı Fabrika bağlantıyı etkinleştirmek için Windows veya Linux üzerinde dağıtma önceden yapılandırılmış çözümü."
services: 
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/14/2017
ms.author: dobett
ms.openlocfilehash: f6a69ecbeb09dc042eff7c1f95ee518e701b0507
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2017
---
# <a name="deploy-a-gateway-on-windows-or-linux-for-the-connected-factory-preconfigured-solution"></a>Windows veya Linux üzerinde bir ağ geçidi bağlı Fabrika önceden yapılandırılmış çözümü dağıtma

Bağlı Fabrika önceden yapılandırılmış çözüm için bir ağ geçidi dağıtmak için gerekli yazılımı iki bileşenden oluşur:

* *OPC Proxy* IOT Hub bağlantı kurar. *OPC Proxy* tarayıcıdan tümleşik OPC bağlı Fabrika çözüm Portalı'nda çalışan komut ve Denetim iletileri bekler.
* *OPC yayımcı* var olan şirket içi OPC UA sunucularına bağlanır ve iletir telemetri iletilerini onlardan IOT Hub'ına.

Her iki bileşenler açık kaynaklı ve GitHub kaynağına ve Docker kapsayıcılar olarak kullanılabilir:

| GitHub | DockerHub |
| ------ | --------- |
| [OPC yayımcı][lnk-publisher-github] | [OPC yayımcı][lnk-publisher-docker] |
| [OPC Proxy][lnk-proxy-github] | [OPC Proxy][lnk-proxy-docker] |

Genel kullanıma yönelik IP adresi veya ağ geçidi Güvenlik Duvarı'nda delik ya da bileşen için gerekli değildir. OPC Proxy ve OPC yayımcı yalnızca giden bağlantı noktası 443, 5671 ve 8883 kullanın.

Bu makaledeki adımları Docker üzerinde kullanarak bir ağ geçidi dağıtma Göster [Windows](#windows-deployment) veya [Linux](#linux-deployment). Ağ geçidi bağlı Fabrika önceden yapılandırılmış çözüm bağlantısı sağlar.

> [!NOTE]
> Docker kapsayıcısı içinde çalışan bir ağ geçidi yazılımdır [Azure IOT kenar].

## <a name="windows-deployment"></a>Windows Dağıtım

> [!NOTE]
> Bir ağ geçidi cihazı henüz yoksa, Microsoft ticari bir ağ geçidi ortaklarından birinden satın önerir. Ziyaret [Azure IOT cihaz katalog] ağ geçidi aygıtlarını bağlı Fabrika çözümüyle uyumlu bir listesi. Ağ geçidi kurun aygıtla birlikte gelen yönergeleri izleyin. Alternatif olarak, var olan geçitlerinizin birinde el ile ayarlamak için aşağıdaki yönergeleri kullanın.

### <a name="install-docker"></a>Docker yükleyin

Yükleme [Windows için Docker] , Windows tabanlı ağ geçidi Cihazınızda. Windows Docker Kurulum sırasında Docker ile paylaşmak için konak makinesi üzerinde bir sürücü seçin. Aşağıdaki ekran görüntüsü, D sürücüsündeki Windows sisteminize paylaşımı gösterir:

![Docker yükleyin][img-install-docker]

Adlı bir klasör oluşturun **docker** paylaşılan sürücü kök.
Docker gelen yükledikten sonra da bu adımı gerçekleştirebilirsiniz **ayarları** menüsü.

### <a name="configure-the-gateway"></a>Ağ geçidini yapılandırma

1. Gereksinim duyduğunuz **iothubowner** Azure IOT paketi bağlantı dizesi ağ geçidi dağıtımını tamamlamak için Fabrika dağıtım bağlı. İçinde [Azure portal], IOT Hub'ınıza bağlı Fabrika çözümü dağıttığınızda oluşturduğunuz kaynak grubunda gidin. Tıklatın **paylaşılan erişim ilkeleri** erişimi **iothubowner** bağlantı dizesi:

    ![IOT Hub bağlantı dizesini bulun][img-hub-connection]

    Kopya **bağlantı dizesi--birincil anahtar** değeri.

1. İki ağ geçidi modülleri çalıştırarak ağ geçidi için IOT Hub'ınızı yapılandırma **sonra** ile bir komut isteminden:

    `docker run -it --rm -h <ApplicationName> -v //D/docker:/build/src/GatewayApp.NetCore/bin/Debug/netcoreapp1.0/publish/CertificateStores -v //D/docker:/root/.dotnet/corefx/cryptography/x509stores microsoft/iot-gateway-opc-ua:1.0.0 <ApplicationName> "<IoTHubOwnerConnectionString>"`

    `docker run -it --rm -v //D/docker:/mapped microsoft/iot-gateway-opc-ua-proxy:0.1.3 -i -c "<IoTHubOwnerConnectionString>" -D /mapped/cs.db`

    * **&lt;ApplicationName&gt;**  OPC UA yayımcınıza biçimde sağlamak için ad **yayımcı.&lt; tam etki alanı adınızı&gt;**. Örneğin, Fabrika ağ olarak adlandırılır, **myfactorynetwork.com**, **ApplicationName** değer **publisher.myfactorynetwork.com**.
    * **&lt;IoTHubOwnerConnectionString&gt;**  olan **iothubowner** önceki adımda kopyaladığınız bağlantı dizesi. Bu bağlantı dizesini yalnızca bu adımda kullanılır, aşağıdaki adımlarda gerekmez:

    Eşlenen D: kullandığınız\\docker klasörü ( `-v` bağımsız değişkeni) daha sonra ağ geçidi modülleri kullanan iki X.509 sertifikaları kalıcı hale getirmek için.

### <a name="run-the-gateway"></a>Ağ geçidi çalıştırın

1. Aşağıdaki komutları kullanarak ağ geçidini yeniden başlatın:

    `docker run -it --rm -h <ApplicationName> --expose 62222 -p 62222:62222 -v //D/docker:/build/src/GatewayApp.NetCore/bin/Debug/netcoreapp1.0/publish/Logs -v //D/docker:/build/src/GatewayApp.NetCore/bin/Debug/netcoreapp1.0/publish/CertificateStores -v //D/docker:/shared -v //D/docker:/root/.dotnet/corefx/cryptography/x509stores -e _GW_PNFP="/shared/publishednodes.JSON" microsoft/iot-gateway-opc-ua:1.0.0 <ApplicationName>`

    `docker run -it --rm -v //D/docker:/mapped microsoft/iot-gateway-opc-ua-proxy:0.1.3 -D /mapped/cs.db`

1. Güvenlik nedenleriyle, iki X.509 sertifikalarını D: kalıcı\\docker klasör özel anahtarı içerir. Bu klasör kimlik bilgilerine erişimi sınırlayabilir (genellikle **Yöneticiler**) Docker kapsayıcısı çalıştırmak için kullanın. D: sağ\\docker klasörü seçin **özellikleri**, ardından **güvenlik**ve ardından **Düzenle**. Vermek **Yöneticiler** tam denetim ve diğerlerinin kaldırın:

    ![Docker paylaşım izinleri][img-docker-share]

1. Ağ bağlantısını doğrulayın. Bir komut isteminden aşağıdaki komutu girin `ping publisher.<your fully qualified domain name>` ağ geçidiniz ping işlemi yapmak için. Hedef ulaşılamaz durumda olduğunda, ağ geçidi, ağ geçidi ana bilgisayar dosyasının adını ve IP adresi ekleyin. Hosts dosyasını bulunan **Windows\\System32\\sürücüleri\\vb.** klasör.

1. Ardından, ağ geçidinde çalıştıran yerel bir OPC UA istemcisi kullanarak yayımcı bağlanmayı deneyin. URL OPC UA uç nokta `opc.tcp://publisher.<your fully qualified domain name>:62222`. OPC UA istemci yoksa kullanın indirin ve bir [açık kaynak OPC UA istemci].

1. Bu yerel testleri başarıyla tamamladıktan sonra Gözat **kendi OPC UA sunucusuna** bağlı Fabrika çözüm portalında sayfası. Yayımcı uç noktasının URL'sini girin (`tcp://publisher.<your fully qualified domain name>:62222`) tıklatıp **Bağlan**. Bir sertifika uyarısı almak, ardından tıklatın **devam et.** Sonraki hata, aldığınız UA Web istemcisi yayımcısına güvendiğinizden değil. Bu hatayı gidermek için kopyalama **UA Web istemcisi** gelen sertifika **D:\\docker\\reddetti sertifikaları\\sertifikaları** klasörüne **D: \\docker\\UA uygulamaları\\sertifikaları** ağ geçidinde klasör. Ağ geçidi için yeniden başlatmanız gerekmez. Bu adımı yineleyin. Ağ geçidi buluttan şimdi bağlanabilir ve çözüme OPC UA sunucuları eklemek hazırsınız.

### <a name="add-your-opc-ua-servers"></a>OPC UA sunucularınızı ekleme

1. Gözat **kendi OPC UA sunucusuna** bağlı Fabrika çözüm portalında sayfası. Bağlı Fabrika portal OPC UA sunucusu arasında güven oluşturmak için önceki bölümde olduğu gibi aynı adımları izleyin. Bu adım bağlı Fabrika portal ve OPC UA sunucu sertifikaları karşılıklı güven oluşturur ve bir bağlantı oluşturur.

1. OPC UA sunucunuzun OPC UA düğümleri ağaç gidin, OPC düğümleri sağ tıklatın ve seçin **yayımlama**. Bu şekilde çalışmaya yayımlamak için OPC UA sunucusu ve yayımcı, aynı ağ üzerinde olmalıdır. Diğer bir deyişle, tam etki alanı adı, yayımcı ise **publisher.mydomain.com** OPC UA sunucunun tam etki alanı adını, örneğin, bu olmalıdır **myopcuaserver.mydomain.com**. Kurulumunuzu farklıysa, el ile düğümleri bulunan publishesnodes.json dosya ekleyebileceğiniz **D:\\docker** klasör. Publishesnodes.json dosyanın ilk otomatik olarak oluşturulan başarılı bir OPC düğümünün yayımlayın.

1. Telemetri şimdi ağ geçidi aygıttan akar. Telemetriyi de görüntüleyebilirsiniz **Fabrika konumları** altında bağlı Fabrika portal görünümünü **yeni Fabrika**.

## <a name="linux-deployment"></a>Linux dağıtımı

> [!NOTE]
> Bir ağ geçidi cihazı henüz yoksa, Microsoft ticari bir ağ geçidi ortaklarından birinden satın önerir. Ziyaret [Azure IOT cihaz katalog] ağ geçidi aygıtlarını bağlı Fabrika çözümüyle uyumlu bir listesi. Ağ geçidi kurun aygıtla birlikte gelen yönergeleri izleyin. Alternatif olarak, var olan geçitlerinizin birinde el ile ayarlamak için aşağıdaki yönergeleri kullanın.

[Docker yükleme] Linux ağ geçidi cihazınız üzerinde.

### <a name="configure-the-gateway"></a>Ağ geçidini yapılandırma

1. Gereksinim duyduğunuz **iothubowner** Azure IOT paketi bağlantı dizesi ağ geçidi dağıtımını tamamlamak için Fabrika dağıtım bağlı. İçinde [Azure portal], IOT Hub'ınıza bağlı Fabrika çözümü dağıttığınızda oluşturduğunuz kaynak grubunda gidin. Tıklatın **paylaşılan erişim ilkeleri** erişimi **iothubowner** bağlantı dizesi:

    ![IOT Hub bağlantı dizesini bulun][img-hub-connection]

    Kopya **bağlantı dizesi--birincil anahtar** değeri.

1. İki ağ geçidi modülleri çalıştırarak ağ geçidi için IOT Hub'ınızı yapılandırma **sonra** ile Kabuğu'ndan:

    `sudo docker run -it --rm -h <ApplicationName> -v /shared:/build/src/GatewayApp.NetCore/bin/Debug/netcoreapp1.0/publish/ -v /shared:/root/.dotnet/corefx/cryptography/x509stores microsoft/iot-gateway-opc-ua:1.0.0 <ApplicationName> "<IoTHubOwnerConnectionString>"`

    `sudo docker run --rm -it -v /shared:/mapped microsoft/iot-gateway-opc-ua-proxy:0.1.3 -i -c "<IoTHubOwnerConnectionString>" -D /mapped/cs.db`

    * **&lt;ApplicationName&gt;**  ağ geçidi oluşturur biçiminde OPC UA uygulama adı **yayımcı.&lt; tam etki alanı adınızı&gt;**. Örneğin, **publisher.microsoft.com**.
    * **&lt;IoTHubOwnerConnectionString&gt;**  olan **iothubowner** önceki adımda kopyaladığınız bağlantı dizesi. Bu bağlantı dizesini yalnızca bu adımda kullanılır, aşağıdaki adımlarda gerekmez:

    Kullandığınız **/ paylaşılan** klasörü ( `-v` bağımsız değişkeni) daha sonra ağ geçidi modülleri kullanan iki X.509 sertifikaları kalıcı hale getirmek için.

### <a name="run-the-gateway"></a>Ağ geçidi çalıştırın

1. Aşağıdaki komutları kullanarak ağ geçidini yeniden başlatın:

    `sudo docker run -it -h <ApplicationName> --expose 62222 -p 62222:62222 --rm -v /shared:/build/src/GatewayApp.NetCore/bin/Debug/netcoreapp1.0/publish/Logs -v /shared:/build/src/GatewayApp.NetCore/bin/Debug/netcoreapp1.0/publish/CertificateStores -v /shared:/shared -v /shared:/root/.dotnet/corefx/cryptography/x509stores -e _GW_PNFP="/shared/publishednodes.JSON" microsoft/iot-gateway-opc-ua:1.0.0 <ApplicationName>`

    `sudo docker run -it -v /shared:/mapped microsoft/iot-gateway-opc-ua-proxy:0.1.3 -D /mapped/cs.db`

1. Güvenlik nedenleriyle, kalıcı hale iki X.509 sertifikalarını **/ paylaşılan** klasör özel anahtarı içerir. Bu klasör, Docker kapsayıcısı çalıştırmak için kullandığınız kimlik bilgilerine erişimi sınırlayın. İzinlerini ayarlamak için **kök** yalnızca kullanın `chmod` kabuk klasörü komutu.

1. Ağ bağlantısını doğrulayın. Komutu bir kabuğundan girin `ping publisher.<your fully qualified domain name>` ağ geçidiniz ping işlemi yapmak için. Hedef ulaşılamaz durumda olduğunda, ağ geçidi, ağ geçidinde hosts dosyasına adını ve IP adresi ekleyin. Hosts dosyasını bulunan **/vb.** klasör.

1. Ardından, ağ geçidinde çalıştıran yerel bir OPC UA istemcisi kullanarak yayımcı bağlanmayı deneyin. URL OPC UA uç nokta `opc.tcp://publisher.<your fully qualified domain name>:62222`. OPC UA istemci yoksa kullanın indirin ve bir [açık kaynak OPC UA istemci].

1. Bu yerel testleri başarıyla tamamladıktan sonra Gözat **kendi OPC UA sunucusuna** bağlı Fabrika çözüm portalında sayfası. Yayımcı uç noktasının URL'sini girin (`tcp://publisher.<your fully qualified domain name>:62222`) tıklatıp **Bağlan**. Bir sertifika uyarısı almak, ardından tıklatın **devam et.** Sonraki hata, aldığınız UA Web istemcisi yayımcısına güvendiğinizden değil. Bu hatayı gidermek için kopyalama **UA Web istemcisi** gelen sertifika **/paylaşılan/reddedildi sertifikaları/sertifikaları** klasörüne **/shared/UA uygulamalar/sertifikaları** klasörü ağ geçidi. Ağ geçidi için yeniden başlatmanız gerekmez. Bu adımı yineleyin. Ağ geçidi buluttan şimdi bağlanabilir ve çözüme OPC UA sunucuları eklemek hazırsınız.

### <a name="add-your-opc-ua-servers"></a>OPC UA sunucularınızı ekleme

1. Gözat **kendi OPC UA sunucusuna** bağlı Fabrika çözüm portalında sayfası. Bağlı Fabrika portal OPC UA sunucusu arasında güven oluşturmak için önceki bölümde olduğu gibi aynı adımları izleyin. Bu adım bağlı Fabrika portal ve OPC UA sunucu sertifikaları karşılıklı güven oluşturur ve bir bağlantı oluşturur.

1. OPC UA sunucunuzun OPC UA düğümleri ağaç gidin, OPC düğümleri sağ tıklatın ve seçin **yayımlama**. Bu şekilde çalışmaya yayımlamak için OPC UA sunucusu ve yayımcı, aynı ağ üzerinde olmalıdır. Diğer bir deyişle, tam etki alanı adı, yayımcı ise **publisher.mydomain.com** OPC UA sunucunun tam etki alanı adını, örneğin, bu olmalıdır **myopcuaserver.mydomain.com**. Kurulumunuzu farklıysa, el ile düğümleri bulunan publishesnodes.json dosya ekleyebileceğiniz **/ paylaşılan** klasör. Publishesnodes.json ilk otomatik olarak oluşturulan başarılı bir OPC düğümünün yayımlayın.

1. Telemetri şimdi ağ geçidi aygıttan akar. Telemetriyi de görüntüleyebilirsiniz **Fabrika konumları** altında bağlı Fabrika portal görünümünü **yeni Fabrika**.

## <a name="next-steps"></a>Sonraki adımlar

Bağlı Fabrika önceden yapılandırılmış çözüm mimarisi hakkında daha fazla bilgi için bkz: [bağlı Fabrika önceden yapılandırılmış çözüm izlenecek][lnk-walkthrough].

Hakkında bilgi edinin [OPC yayımcı başvuru uygulaması](iot-suite-connected-factory-publisher.md).

[img-install-docker]: ./media/iot-suite-connected-factory-gateway-deployment/image1.png
[img-hub-connection]: ./media/iot-suite-connected-factory-gateway-deployment/image2.png
[img-docker-share]: ./media/iot-suite-connected-factory-gateway-deployment/image3.png

[Windows için Docker]: https://www.docker.com/docker-windows
[Azure IOT cihaz katalog]: https://catalog.azureiotsuite.com/?q=opc
[Azure portal]: http://portal.azure.com/
[açık kaynak OPC UA istemci]: https://github.com/OPCFoundation/UA-.NETStandardLibrary/tree/master/SampleApplications/Samples/Client.Net4
[Docker yükleme]: https://www.docker.com/community-edition#/download
[lnk-walkthrough]: iot-suite-connected-factory-sample-walkthrough.md
[Azure IOT kenar]: https://github.com/Azure/iot-edge

[lnk-publisher-github]: https://github.com/Azure/iot-edge-opc-publisher
[lnk-publisher-docker]: https://hub.docker.com/r/microsoft/iot-gateway-opc-ua
[lnk-proxy-github]: https://github.com/Azure/iot-edge-opc-proxy
[lnk-proxy-docker]: https://hub.docker.com/r/microsoft/iot-gateway-opc-ua-proxy