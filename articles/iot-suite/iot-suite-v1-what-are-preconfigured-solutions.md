---
title: Azure IoT önceden yapılandırılmış çözümleri | Microsoft Docs
description: Azure IoT önceden yapılandırılmış çözümlerin ve ek kaynaklara bağlantısı olan mimarisinin bir açıklaması.
services: ''
suite: iot-suite
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: 59009f37-9ba0-4e17-a189-7ea354a858a2
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: dobett
ms.openlocfilehash: d860c768a73737e6c8c52a8652d6b43434a3a07d
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34361997"
---
# <a name="what-are-the-azure-iot-suite-preconfigured-solutions"></a>Azure IoT Paketi önceden yapılandırılmış çözümleri nelerdir?

Azure IoT Paketi önceden yapılandırılmış çözümleri, aboneliği kullanarak Azure’e dağıtabildiğiniz yaygın IoT çözüm modellerinin uygulamalarıdır. Önceden yapılandırılmış çözümleri kullanabilirsiniz:

* Kendi IoT çözümleriniz için bir başlangıç noktası olarak.
* IoT çözüm tasarımı ve geliştirmesinde ortak yaklaşımlar hakkında bilgi edinmek için.

Önceden yapılandırılmış her çözüm, telemetri oluşturmak için tam, uçtan uca sanal cihazları kullanan bir uygulamadır.

Kendi özel IoT gereksinimlerinizi karşılayacak şekilde çözümü özelleştirmek ve genişletmek için tam kaynak kodunu indirebilirsiniz.

> [!NOTE]
> Önceden yapılandırılmış çözümlerden birini dağıtmak için [Microsoft Azure IoT Paketi][lnk-azureiotsuite] sayfasını ziyaret edin. [IoT önceden yapılandırılmış çözümlerine giriş][lnk-getstarted-preconfigured] makalesi çözümlerden birinin dağıtılması ve çalıştırılması hakkında daha fazla bilgi sağlamaktadır.

Aşağıdaki tabloda, çözümlerin belirli IoT özelliklerini nasıl karşıladığı gösterilmektedir:

| Çözüm | Veri alımı | Cihaz kimliği | Cihaz yönetimi | Komut ve denetim | Kurallar ve eylemler | Tahmine dayalı analiz |
| --- | --- | --- | --- | --- | --- | --- |
| [Uzaktan izleme][lnk-getstarted-preconfigured] |Yes |Yes |Yes |Yes |Yes |- |
| [Tahmine dayalı bakım][lnk-predictive-maintenance] |Yes |Yes |- |Yes |Yes |Yes |
| [Bağlı fabrika][lnk-getstarted-factory] |Yes |Yes |Yes |Yes |Yes |- |

* *Veri alımı*: Bulut ölçeğinde veri girişi.
* *Cihaz kimliği*: Benzersiz cihaz kimliklerini yönetin ve çözüme cihaz erişimini denetleyin.
* *Cihaz yönetimi*: Cihaz meta verilerini yönetin ve cihazı yeniden başlatma ile üretici yazılımını yükseltme gibi işlemleri gerçekleştirin.
* *Komuta ve denetim*: Cihazın bir işlem yapmasını sağlamak için buluttan cihaza ileti gönderme.
* *Kurallar ve eylemler*: Çözüm, belirli bir cihazdan buluta verilerde işlem yapmak için arka uç kuralları kullanır.
* *Tahmine dayalı analiz*: Çözüm arka ucu, belirli işlemlerin ne zaman gerçekleştirileceğini tahmin etmek için cihazdan buluta verileri analiz eder. Örneğin, motor bakımının ne zaman olacağını saptamak için uçak motoru telemetrisinin analiz edilmesi.

## <a name="remote-monitoring-preconfigured-solution-overview"></a>Önceden yapılandırılmış Uzaktan İzleme çözümüne genel bakış

Diğer çözümlerin de paylaştığı çok sayıda ortak tasarım öğesi gösterdiğinden, bu makalede önceden yapılandırılmış uzaktan izleme çözümünü seçtik.

Aşağıdaki diyagram uzaktan izleme çözümünün önemli öğelerin göstermektedir. Aşağıdaki bölümlerde bu öğeler hakkında daha fazla bilgi verilmektedir.

![Önceden yapılandırılmış Uzaktan İzleme çözümü mimarisi][img-remote-monitoring-arch]

## <a name="devices"></a>Cihazlar

Önceden yapılandırılmış uzaktan izleme çözümünü dağıttığınızda, dört sanal cihaz bir soğutma cihazının benzetimini yapan çözümde önceden hazırlanır. Bu sanal cihazlarda telemetri yayan yerleşik bir sıcaklık ve nem modeline bulunur. Bu sanal cihazlar aşağıdaki amaçlar için dahil edilir:

- Verilerin çözümde uçtan uca akışını göstermek.
- Kullanışlı bir telemetri kaynağı sağlamak.
- Çözümü özel bir uygulama için başlangıç noktası olarak kullanan bir arka uç geliştiriciyseniz, yöntemler veya komutlar için bir hedef sağlayın.

Çözümdeki sanal cihazlar aşağıdaki buluttan cihaza iletişimlere yanıt verebilir:

- *Yöntemler ([doğrudan yöntemler][lnk-direct-methods])*: Bağlı bir cihazın hemen yanıt vermesinin beklendiği iki yönlü bir iletişim yöntemi.
- *Komutlar (buluttan cihaza iletiler)*: Bir cihazın dayanıklı bir kuyruktan komut aldığı tek yönlü bir iletişim yöntemi.

Bu farklı yaklaşımların bir karşılaştırması için bkz. [Buluttan cihaza iletişim kılavuzu][lnk-c2d-guidance].

Cihaz önceden yapılandırılmış çözümde IoT Hub’a ilk kez bağlandığında hub’a bir cihaz bilgi iletisi gönderir. Bu ileti, cihazın karşılık verebildiği metotların listesini oluşturur. Önceden yapılandırılmış uzaktan izleme çözümünde, sanal cihazlar şu yöntemleri destekler:

* *Üretici Yazılımı Güncelleştirmelerini Başlatma*: bu yöntem cihaz üzerinde bir üretici yazılımı güncelleştirmesi gerçekleştirmek üzere zaman uyumsuz bir görev başlatır. Zaman uyumsuz görev, çözüm panosuna durum güncelleştirmelerini iletmek üzere bildirilen özellikleri kullanır.
* *Yeniden başlatma*: bu yöntem sanal cihazın yeniden başlatılmasına neden olur.
* *FactoryReset*: bu yöntem sanal cihaz üzerinde bir fabrika sıfırlaması tetikler.

Cihaz önceden yapılandırılmış çözümde IoT Hub’a ilk kez bağlandığında hub’a bir cihaz bilgi iletisi gönderir. Bu ileti cihazın karşılık verebildiği komutların listesini oluşturur. Önceden yapılandırılmış uzaktan izleme çözümünde, sanal cihazlar şu komutları destekler:

* *Cihaza Ping Yap*: Cihaz bu komutu bir bildirimle yanıtlar. Bu komut, cihazın halen etkin ve dinleniyor olduğunu denetlemek için yararlıdır.
* *Telemetriyi Başlat*: Cihaza telemetri göndermeye başlaması talimatı verir.
* *Telemetriyi Durdur*: Cihaza telemetri göndermeyi durdurması talimatı verir.
* *Sıcaklık Ayar Noktasını Değiştir*: Cihazın gönderdiği benzetimli sıcaklık telemetri değerlerini denetler. Bu komut, arka uç mantığının test edilmesi için yararlıdır.
* *Telemetriyi Tanıla*: Cihazın dış sıcaklığı telemetri olarak gönderip göndermediğini denetler.
* *Cihaz Durumunu Değiştir*: Cihaz durumu meta veri özelliğini cihaz raporlarına ait olarak ayarlar. Bu komut, arka uç mantığının test edilmesi için yararlıdır.

Aynı telemetriyi yayan ve aynı yöntem ile komutu yanıtlayan çözüme daha fazla sanal cihaz ekleyebilirsiniz.

Çözüm, komut ve yöntemleri yanıtlamaya ek olarak [cihaz ikizlerini][lnk-device-twin] kullanır. Cihazlar çözüm arka ucuna özellik değerlerini bildirmek için cihaz ikizlerini kullanır. Çözüm panosu, cihazlar üzerinde istenen yeni özellik değerlerini ayarlamak için cihaz ikizlerini kullanır. Örneğin, üretici yazılımı güncelleştirme işlemi sırasında sanal cihaz, bildirilen özellikleri kullanarak güncelleştirmenin durumunu bildirir.

## <a name="iot-hub"></a>IoT Hub

Önceden yapılandırılmış bu çözümde, IoT Hub'ı örneği tipik bir [IoT çözüm mimarisinde][lnk-what-is-azure-iot] *Bulut Ağ Geçidi*'ne karşılık gelir.

IoT hub’ı telemetriyi tek uç noktada yer alan cihazlardan alır. IoT hub'ı, her cihazın kendisine gönderilen komutları alabildiği cihaza özel uç noktaları da korur.

IoT hub’ı, sunucu tarafı telemetri okuma uç noktasında alınan telemetriyi kullanılabilir hale getirir.

IoT Hub’ının cihaz yönetimi özelliği, cihaz özelliklerini çözüm portalından yönetmenize ve aşağıdaki gibi işlemleri gerçekleştiren işleri zamanlamanıza olanak tanır:

- Cihazları yeniden başlatma
- Cihaz durumlarını değiştirme
- Üretici yazılımı güncelleştirmeleri

## <a name="azure-stream-analytics"></a>Azure Stream Analytics

Önceden yapılandırılmış çözüm, cihazlara ait telemetri akışına filtre uygulamak için üç [Azure Akış Analizi][lnk-asa] (ASA) işini kullanır:

* *DeviceInfo işi* - cihaz kaydına özel iletileri çözüm cihaz kayıt defterine yönlendiren Olay hub’ına verileri çıkarır. Bu cihaz kayıt defteri bir Azure Cosmos DB veritabanıdır. Bir cihaz ilk kez bağlandığında veya **Cihaz durumunu değiştir** komutuna yanıt olarak bu iletiler gönderilir.
* *Telemetry işi* - ham telemetrenin tümünü soğuk depolama için Azure blob depolamaya gönderir ve çözüm panosunda görüntülenen telemetri toplamalarını hesaplar.
* *Rules işi* - kural eşiklerini aşan değerlerle ilgili telemetri akışına filtre uygular ve verilerin çıktısını bir Olay hub’ına alır. Bir kural başlatıldığında, çözüm portalı pano görünümünde bu olay, alarm geçmişi tablosundaki yeni bir satır olarak gösterilir. Bu kurallar ayrıca çözüm portalındaki **Kurallar** ve **Eylemler** görünümlerinde tanımlanan ayarları temel alarak bir eylemi tetikleyebilir.

Önceden yapılandırılmış bu çözümde, ASA işleri tipik bir [IoT çözüm mimarisinde][lnk-what-is-azure-iot] **Iot çözümü arka ucunun** bir parçasını oluşturur.

## <a name="event-processor"></a>Olay işlemcisi

Önceden yapılandırılmış bu çözümde, olay işlemcisi tipik bir [IoT çözüm mimarisinde][lnk-what-is-azure-iot] **Iot çözümü arka ucunun** bir parçasını oluşturur.

**DeviceInfo** ve **Rules** ASA işleri, diğer arka iç hizmetlerine dağıtılması amacıyla kendi çıktılarını Olay hub’larına gönderir. Çözüm, bu Olay Hub’larından iletileri okumak için [WebJob][lnk-web-job]'da çalışan bir [EventProcessorHost][lnk-event-processor] örneğini kullanır. **EventProcessorHost** şunları kullanır:
- Cosmos DB veritabanındaki cihaz verilerini güncelleştirmek için **DeviceInfo** verileri.
- Mantıksal uygulamayı çağırmak ve çözüm portalındaki uyarı görüntüsünü güncelleştirmek için **Kural** verileri.

## <a name="device-identity-registry-device-twin-and-cosmos-db"></a>Cihaz kimliği kayıt defteri, cihaz ikizi ve Cosmos DB

Her IoT hub'ında cihaz anahtarlarını depolayan bir [cihaz kimliği kayıt defteri][lnk-identity-registry] vardır. IoT hub'ı bu bilgileri cihazların kimliğini doğrulamak için kullanır - hub’a bağlanmadan önce cihazların kayıtlı ve geçerli bir anahtara sahip olması gerekir.

[Cihaz ikizi][lnk-device-twin], IoT Hub tarafından yönetilen bir JSON belgesidir. Bir cihazın cihaz ikizi şunları içerir:

- Cihaz tarafından hub’a gönderilen bildirilen özellikler. Bu özellikleri çözüm portalında görüntüleyebilirsiniz.
- Cihaza göndermek istediğiniz, istenen özellikler. Bu özellikleri çözüm portalında ayarlayabilirsiniz.
- Yalnızca cihaz ikizinde bulunan ve cihazda mevcut olmayan etiketler. Bu etiketleri kullanarak çözüm portalındaki cihaz listesini filtreleyebilirsiniz.

Bu çözümde cihaz meta verilerini yönetmek için cihaz ikizleri kullanılır. Çözümde ayrıca her bir cihaz tarafından desteklenen komutlar ve komut geçmişi gibi çözüme özel ek cihaz verilerini depolamak için Cosmos DB veritabanı kullanılır.

Çözüm bu bilgileri, Cosmos DB veritabanı içeriğiyle eşitlenmiş cihaz kimliği kayıt defterinde tutmalıdır. **EventProcessorHost**, eşitlemeyi yönetmek için **DeviceInfo** akış analizi işine ait verileri kullanır.

## <a name="solution-portal"></a>Çözüm portalı

![çözüm portalı][img-dashboard]

Çözüm portalı, önceden yapılandırılmış çözümün bir parçası olarak buluta dağıtılan web tabanlı bir UI’dir. Şunları yapmanızı sağlar:

* Bir panoda telemetri ve uyarı geçmişini görüntüleme.
* Yeni cihazları hazırlama.
* Cihazları yönetme ve izleme.
* Komutları belirli cihazlara gönderme.
* Belirli cihazlarda yöntemleri çağırın.
* Kuralları ve eylemleri yönetme.
* Bir veya daha fazla cihazda çalıştırılacak işleri zamanlayın.

Önceden yapılandırılmış bu çözümde, çözüm portalı tipik bir [IoT çözüm mimarisinde][lnk-what-is-azure-iot] **Iot çözümü arka ucunun** bir parçasını ve **İşleme ve iş bağlantısının** bir parçasını oluşturur.

## <a name="next-steps"></a>Sonraki adımlar

IoT çözümü mimarileri hakkında daha fazla bilgi için bkz. [Microsoft Azure IoT hizmetleri: Başvuru Mimarisi][lnk-refarch].

Önceden yapılandırılmış bir çözümün ne olduğunu öğrendiğinize göre önceden yapılandırılmış *uzaktan izleme* çözümünü dağıtarak başlayabilirsiniz: [Önceden yapılandırılmış çözümleri kullanmaya başlama][lnk-getstarted-preconfigured].

[img-remote-monitoring-arch]: ./media/iot-suite-v1-what-are-preconfigured-solutions/remote-monitoring-arch1.png
[img-dashboard]: ./media/iot-suite-v1-what-are-preconfigured-solutions/dashboard.png
[lnk-what-is-azure-iot]: iot-suite-what-is-azure-iot.md
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-event-processor]: ../event-hubs/event-hubs-programming-guide.md#event-processor-host
[lnk-web-job]: ../app-service/web-sites-create-web-jobs.md
[lnk-identity-registry]: ../iot-hub/iot-hub-devguide-identity-registry.md
[lnk-predictive-maintenance]: iot-suite-predictive-overview.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
[lnk-getstarted-preconfigured]: iot-suite-v1-getstarted-preconfigured-solutions.md
[lnk-c2d-guidance]: ../iot-hub/iot-hub-devguide-c2d-guidance.md
[lnk-device-twin]: ../iot-hub/iot-hub-devguide-device-twins.md
[lnk-direct-methods]: ../iot-hub/iot-hub-devguide-direct-methods.md
[lnk-getstarted-factory]:../iot-accelerators/iot-accelerators-connected-factory-overview.md
