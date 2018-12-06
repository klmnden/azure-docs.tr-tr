---
title: Azure IOT Edge sertifikaları anlama | Microsoft Docs
description: Azure IOT Edge sertifikaları ve bunların nasıl kullanıldığı hakkında bilgi edinin.
author: stevebus
manager: philmea
ms.author: stevebus
ms.date: 09/13/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: b1b8c660dfd6ea6890a940a97f5ff1ec1617c28d
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52963394"
---
# <a name="azure-iot-edge-certificate-usage-detail"></a>Azure IOT Edge sertifika kullanım ayrıntısı

IOT Edge sertifikalar kullanılır modülleri ve aşağı akış IOT cihazları için kimlik ve yasallığı doğrulamak için [Edge hub'ı](iot-edge-runtime.md#iot-edge-hub) bağlandığı Çalışma Zamanı Modülü. Bu doğrulamaları TLS (Aktarım Katmanı Güvenliği) arasında güvenli bir bağlantı çalışma zamanı, modülleri ve IOT cihazları etkinleştirin. IOT Hub kendisi gibi IOT Edge güvenli gerektirir ve aşağı yönde IOT bağlantısından şifreli (veya yaprak) cihazlar ve IOT Edge modülleri. TLS güvenli bir bağlantı kurmak için Edge hub'ı modülü bir sunucuya bağlanan istemcilerin kimliğini doğrulamak için'edebilmeleri için sertifika zinciri sunar.

Ölçekli senaryolar, yanı sıra geliştirme ve test senaryoları IOT Edge sertifikaları nasıl çalışabileceğini Karışıklıktan sertifikaların IOT Edge'nın kullanımı hakkındaki temizlemek için bu makalede açıklanır. Farklı (bash ve Powershell) komut dosyalarını olsa da, Linux ve Windows arasında aynı kavramlardır.

## <a name="iot-edge-certificates"></a>IoT Edge sertifikaları

Çoğu durumda, üreticiler bağımsız bir Edge cihazının son kullanıcılardan varlıklardır. Bazen yalnızca üretici işleci arasındaki bir ticari olarak satışta Edge cihazı satın ilişkidir. Diğer durumlarda, bir uç cihazın adına işleci oluşturmak için sözleşme altında çalışan üreticisi. IOT Edge sertifika tasarım, her iki senaryoyu dikkate dener.

Aşağıdaki şekilde, sertifikaların IOT Edge'nın kullanımı gösterilmektedir. Sıfır, bir veya birçok Ara İmzalama sertifikaları kök CA sertifikasını ve cihaz CA sertifikası arasında olabilir. Katılan varlıklar sayısına bağlı olarak, burada bir servis talebi göstereceğiz.

![Tipik bir sertifika ilişkileri diyagramı](./media/iot-edge-certs/edgeCerts-general.png)

### <a name="certificate-authority"></a>Sertifika yetkilisi

Sertifika yetkilisi veya kısaca, ' CA' Dijital sertifikaları veren bir varlıktır. Bir sertifika yetkilisi, sahibi ve sertifika alıcısı arasında güvenilir bir üçüncü taraf olarak görev yapar. Bir dijital sertifika ortak anahtar sertifika alıcısı tarafından sahipliğini onaylar. Başlangıçta yetkilisi tarafından verilen tüm sertifikaları güvende temeli bir kök sertifika vererek sertifika güven zinciri çalışır. Ardından, sahibi, kök sertifika ek ara sertifikaları ('yaprak' Sertifikalar) kullanabilirsiniz.

### <a name="root-ca-certificate"></a>Kök CA sertifikası

Bir kök CA sertifikasını güven tüm işlemin köküdür. Üretim senaryolarında bu genellikle Baltimore, Verisign, DigiCert, vb. gibi güvenilen ticari sertifika yetkilisinden satın alınmış bir CA sertifikası zordur. Edge cihazlarınıza bağlanma cihazları üzerinde tam denetime sahip olmalıdır, bir kurumsal düzey sertifika yetkilisi kullanmak da mümkündür. Yaprak IOT cihazları kök sertifikaya güvenmeleri gerekir böylece ya da bir olay Edge Hub'ından tüm sertifika zinciriyle, toplar. Kök CA sertifikasını güvenilen kök sertifika yetkilisi deposunda ya da depolamak veya IOT istemci IOT Edge bağlandığında kök CA sertifika ayrıntıları kodunuzda sağlayın.

### <a name="intermediate-certificates"></a>Ara sertifikaları

Tipik bir üretim sürecinin güvenli cihazlar oluşturmak için bir kök CA sertifikaları nadiren doğrudan, öncelikle riskini sızıntısını veya nedeniyle kullanılır. Genellikle, bir veya daha fazla ara CA sertifikalarının oluşturulur ve kök CA sertifikası tarafından imzalanmış. Yalnızca olabilir bir ya da bu Ara sertifikaları zincirine olabilir. Ara sertifikaları zincirine gerektiren senaryolar şunlardır:

* Bir üretici anomaly hiyerarşisi.

* Bir cihazın seri olarak üretim dahil birden çok şirket.

* Müşterinin adıma yaptıkları cihazları imzalamak için bir kök CA satın alma ve imzalama sertifikası üretici türetme bir müşteri.

Her iki durumda da üretici uç cihazda yerleştirilen cihaz CA sertifikasını imzalamak için bu zincir sonunda bir ara CA sertifikasını kullanır. Genellikle, bu Ara sertifikaları üretim tesisindeki yakından korumalı. Bunlar, hem fiziksel hem de kullanımları elektronik katı süreçler geçeriz.

### <a name="device-ca-certificate"></a>Cihaz CA sertifikası

Cihaz CA sertifikası oluşturulan ve işlem son ara CA sertifikası tarafından imzalanmış. Bu sertifika, Edge cihazının kendisinde tercihen bir donanım güvenlik modülü (HSM) gibi güvenli depolama yüklenir. Ayrıca, bir cihaz CA sertifikası, bir IOT Edge cihazı benzersiz olarak tanımlar. IOT Edge için cihaz CA sertifikalarını diğer verebilen sertifikadır. Örneğin, cihaz CA sertifikası için cihazların kimliklerini doğrulamak için kullanılan cihaz sertifikaları yaprak sorunları [Azure IOT cihaz sağlama hizmeti](../iot-dps/about-iot-dps.md).

### <a name="iot-edge-workload-ca"></a>IOT Edge iş yükü CA

Bu sertifika, ilk işlem "işleci" tarafındaki tarafından oluşturulan [IOT Edge Güvenlik Yöneticisi](iot-edge-security-manager.md) IOT Edge ilk kez başlatıldığında. Bu sertifika oluşturulan ve "cihaz CA sertifikası" tarafından imzalanmış. Başka bir ara imzalama sertifikası olduğundan, bu sertifika, oluşturmak ve IOT Edge çalışma zamanı tarafından kullanılan sertifikaları imzalamak için kullanılır. Günümüzde, öncelikli olarak aşağıdaki bölümde açıklanan Edge hub'ı sunucu sertifikasıdır ancak ileride IOT Edge bileşenleri kimlik doğrulaması için diğer sertifikalar içerebilir.

### <a name="edge-hub-server-certificate"></a>Edge hub'ı sunucu sertifikası

Edge hub'ı sunucu sertifikası yaprak cihazlar ve modüller tarafından IOT Edge gerekli TLS bağlantının kurulması sırasında kimlik doğrulaması için sunulan gerçek sertifikadır. Bu sertifikayı yaprak IOT cihaz güvenmelidir kök CA sertifikasını kadar oluşturmak için kullanılan imzalama sertifikalarının, tam zincir sunar. IOT Edge güvenlik yöneticisi olarak, ortak ad (CN) oluşturduğunda bu Edge hub'ı sonra küçük harfe dönüştürme config.yaml dosyasındaki 'hostname' özelliği tarafından sağlanan ad sertifika ayarlanır. Bu, IOT Edge ile karışıklık ortak bir kaynaktır.

## <a name="production-implications"></a>Üretim uygulamaları

Makul bir soru olabilir "neden IOT Edge gerekmez 'iş yükü CA' ek sertifika? Doğrudan Edge hub'ı sunucusu sertifikasını oluşturmak için cihaz CA sertifikası kullanamadık? ". Teknik olarak verebilir. Ancak, bu "iş yükü" Ara Sertifika amacı cihaz üreticisi ve cihaz işleci arasında kaygıları ayırmaktır. Burada bir IOT Edge cihazı satılan veya başka bir müşteriden aktarılan bir senaryo düşünün. Büyük olasılıkla sabit olmasını üreticisi tarafından sağlanan cihaz CA sertifikası istersiniz. Ancak, cihazın bir işlem için belirli bir "iş yükü" sertifikaları sonlandırılana ve yeni dağıtım için yeniden.

Üretici ve işleci işlemleri ayrıldığı için üretim cihazları için IOT Edge sunmaya için ilgili göz önünde bulundurmanız gereken birkaç etkileri vardır.

* Tüm sertifika tabanlı işlem ile kök CA sertifikasını ve tüm ara CA sertifikalarının güvenli ve izlenen tüm IOT Edge cihazı alma işlemi sırasında. IOT Edge cihaz üreticisi, uygun depolama ve bunların Ara sertifikaların kullanımını güçlü işlemler olması gerekir. Ayrıca, cihaz CA sertifikası cihaz üzerinde tercihen bir donanım güvenlik modülü mümkün olduğunca güvenli depolama alanı olarak tutulmalıdır.

* Edge hub'ı sunucu sertifikası Edge hub'ı bağlanan istemci cihazları ve modüller, cihaz CA sertifikanın CN sunulacak beri **olmamalıdır** "config.yaml IOT Edge üzerinde kullanılacak ana bilgisayar adı" ile aynı cihaz. IOT Edge (örneğin aracılığıyla bağlantı dizesi veya MQTT CONNECT komutta GatewayHostName parametresi) bağlanmak için istemcileri tarafından kullanılan ad **olamaz** cihaz CA sertifikada kullanılan ortak ad ile aynı. Edge hub'ı doğrulama istemcileri tarafından tüm sertifika zinciriyle sunan olmasıdır. Edge hub'ı sunucu sertifikası ve her ikisi de aynı CN sahip cihaz CA sertifikasını, bir doğrulama döngüde alın ve sertifika geçersiz kılar

* Cihaz CA sertifikası, son IOT Edge sertifikalarını oluşturmak için IOT Edge güvenlik arka plan programı tarafından kullanıldığından, tek başına sertifika imzalama özellikleri olduğu anlamına gelen bir imza sertifikası olmalıdır. Bu imzalama bir CA sertifikası olun, temelde "V3 temel kısıtlamalar CA:True" cihaz CA sertifikası için uygulama otomatik olarak gerekli anahtar kullanımı özelliklerini ayarlar.

>[!Tip]
> Zaten IOT Edge kurulumunu bizim "kullanışlı betik" kullanarak geliştirme/test senaryosunda saydam bir ağ geçidi olarak incelediğinize varsa (sonraki bölüme bakın) ve aynı konak adını config.yaml içinde konak adı için yaptığınız gibi cihaz CA sertifikasını oluştururken kullandığınız , size neden kadar işe yaradığını merak ediyor olabilirsiniz. Kolaylık betikleri geliştirilmiştir Geliştirici deneyimini basitleştirmek için komut dosyasına geçirmek adının sonuna üzerinde ".ca" ekler. Her iki cihaz adınızı betikler ve config.yaml ana bilgisayar adı için "mygateway" kullandıysanız, bu nedenle, örneğin, önceki mygateway.ca cihaz CA sertifikası için CN kullanılmadan önce kapatılacak.

## <a name="devtest-implications"></a>Geliştirme ve Test uygulamaları

Microsoft geliştirme sürecini kolaylaştırır ve test senaryoları için bir dizi sağlar [kolaylık betikleri](https://github.com/Azure/azure-iot-sdk-c/tree/master/tools/CACertificates) saydam bir ağ geçidi senaryosunda IOT Edge için uygun olmayan üretim sertifikası oluşturmak için. Komut dosyalarını nasıl örnekler için bkz [saydam bir ağ geçidi olarak görev yapacak bir IOT Edge cihazı yapılandırma](how-to-create-transparent-gateway.md).

Bu betikler, bu makalede açıklanan sertifika zinciri yapısına uygun sertifikaları oluşturur. Aşağıdaki komutlar, "kök CA sertifikasını" ve tek "ara CA sertifikası" oluşturur.

```bash
./certGen.sh create_root_and_intermediate 
```

```Powershell
New-CACertsCertChain rsa 
```

Benzer şekilde, bu komutlar "Cihaz CA sertifika" oluşturun.

```bash
./certGen.sh create_edge_device_certificate "<gateway device name>"
```

```Powershell
New-CACertsEdgeDevice "<gateway device name>"
```

* **\<Ağ geçidi cihaz adı\>** söz konusu betikleri geçirilen **gerekir** config.yaml "ana bilgisayar adı" parametresi ile aynı olmalıdır. Belirtildiği gibi komut, ".ca" dizeye ekleyerek herhangi bir sorunu önlemenize yardımcı **\<ağ geçidi cihaz adı\>** ad çakışması durumunda her iki yerde de aynı adı kullanarak bir kullanıcının Edge ayarlar önlemek için Ancak, adın aynısını kullanmaktan kaçının iyi bir uygulamadır.

>[!Tip]
> Cihaz IOT "yaprak" cihazlarınızdan ve IOT Edge üzerinden bizim IOT cihaz SDK'sını kullanan uygulamaları bağlamak için isteğe bağlı bir GatewayHostName parametre cihazın bağlantı dizesinin sonuyla açın eklemeniz gerekir. Edge hub'ı sunucu sertifikası oluşturulduğunda, ana bilgisayar adını config.yaml küçük harfleri bir sürümünü temel alır, bu nedenle, eşleşme ve TLS sertifika doğrulama işleminin başarılı olması için adları için GatewayHostName parametresi küçük girmeniz gerekir.

## <a name="example-of-iot-edge-certificate-hierarchy"></a>IOT Edge sertifika hiyerarşisi örneği

Bu sertifika yolu örneği göstermek için aşağıdaki ekran görüntüsünde çalışan bir IOT Edge cihazı saydam bir ağ geçidi olarak ayarlama arasındadır. OpenSSL sertifikalara dökümü Edge Hub'ına bağlanmak ve doğrulamak için kullanılır.

![Sertifika hiyerarşisi her düzeyde ekran görüntüsü](./media/iot-edge-certs/iotedge-cert-chain.png)

Ekran görüntüsünde gösterilen sertifika derinliği hiyerarşisini görebilirsiniz:

| Kök CA sertifikası         | Azure IOT hub'ı CA sertifikası yalnızca sınama                                                                           |
|-----------------------------|-----------------------------------------------------------------------------------------------------------|
| Ara CA sertifikası | Azure IOT hub'ı ara sertifika yalnızca sınama                                                                 |
| Cihaz CA sertifikası       | iotgateway.CA ("iotgateway" geçirildi < ağ geçidi ana bilgisayar adı > kolaylık betikleri)      |
| İş yükü CA sertifikası     | iotedge iş yükü ca                                                                                       |
| Edge hub'ı sunucu sertifikası | iotedgegw.local ('hostname' config.yaml eşleşir)                                                |

## <a name="next-steps"></a>Sonraki adımlar

[Azure IOT Edge modüllerini anlama](iot-edge-modules.md)

[Saydam bir ağ geçidi olarak görev yapacak bir IOT Edge cihazı yapılandırma](how-to-create-transparent-gateway.md)