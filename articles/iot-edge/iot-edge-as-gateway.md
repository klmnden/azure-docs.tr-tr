---
title: Ağ geçitleri için aşağı akış cihazları - Azure IOT Edge | Microsoft Docs
description: Azure IOT Edge, verileri birden çok akış CİHAZDAN buluta gönderiyor veya yerel olarak işleyen bir saydam, opak ya da proxy ağ geçidi cihazı oluşturmak için kullanın.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 02/25/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: c3a49c4333838652f7063d6a89cfd8cceace1cf8
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67054203"
---
# <a name="how-an-iot-edge-device-can-be-used-as-a-gateway"></a>Bir ağ geçidi olarak IOT Edge cihazının nasıl kullanılabileceğini

IOT Edge çözümlerini ağ geçitleri, cihaz bağlantısı ve aksi takdirde bu özellikler mıydı IOT cihazları uç analizi sağlar. Azure IOT Edge, bir IOT ağ geçidi bağlantısı, kimlik veya uç analizi ilişkili oldukları bağımsız olarak tüm ihtiyaçlarınızı karşılamak için kullanılabilir. Bu makalede bir ağ geçidi desenleri özellikleri aşağı akış cihaz bağlantısı ve cihaz kimliği için cihaz verilerini ağ geçidi üzerinde nasıl işleneceğini değil yalnızca bakın.

## <a name="patterns"></a>Desenler

Bir ağ geçidi olarak IOT Edge cihazı kullanmak için üç desen vardır: saydam, protokol çevirisi ve kimlik çeviri:
* **Saydam** – teorik olarak IOT Hub'ına bağlanabilir cihazlara bağlanabilir bir ağ geçidi cihazı için bunun yerine. Aşağı Akış cihazlar IOT hub'ı kimliklerinin ve MQTT, AMQP veya HTTP protokolünü kullanarak. Ağ geçidi, yalnızca IOT hub'ı ve cihazlar arasında iletişim geçirir. Cihazlar bir ağ geçidi üzerinden bulutla iletişim kurma ve IOT hub'ında cihazlarla etkileşim bir kullanıcı bir ara ağ geçidi cihazı habersizdir farkında değildir. Bu nedenle, ağ geçidi saydamdır. Başvurmak [saydam bir ağ geçidi oluşturma](how-to-create-transparent-gateway.md) saydam bir ağ geçidi olarak IOT Edge cihazı kullanma özellikleri için.
* **Protokol çevirisi** – bir donuk bir ağ geçidi düzeni da bilinen MQTT, AMQP veya HTTP desteklemeyen cihazlar bir ağ geçidi cihazı IOT Hub'ına onların adına veri göndermesini kullanabilirsiniz. Ağ geçidi, aşağı akış cihazlar tarafından kullanılan protokolü anlar ve IOT Hub'ında bir kimliğe sahiptir yalnızca cihaz. Tüm bilgiler, bir CİHAZDAN bir ağ geçidi geliyor gibi görünüyor. Aşağı Akış cihazları ek kimlik bilgileri, bulut uygulamalarını cihaz başına temelinde verileri çözümlemek istiyorsanız, iletilerinde katıştırmanız gerekir. Buna ek olarak, IOT hub'ı temelleri ikizlerini ve gibi yöntemler yalnızca olmayan aşağı akış cihazların ağ geçidi cihazı için kullanılabilir.
* **Kimlik çeviri** -IOT Hub'ına bağlanamıyor cihazlara bağlanabilir bir ağ geçidi cihazı için bunun yerine. Ağ geçidi, IOT Hub adına aşağı akış cihazları kimlik ve protokol çevirisi sağlar. Aşağı Akış cihazlar tarafından kullanılan protokol anlamanıza, bunları kimlik sağlama ve IOT hub'ı temelleri çevirmek akıllı geçididir. Aşağı Akış cihazlar IOT Hub'ında birinci sınıf cihaz ikizleri ve yöntemler olarak görünür. Bir kullanıcı, cihazlar IOT hub'ında etkileşim kurabilir ve ara ağ geçidi cihazı farkında değildir.

![Diyagram - saydam, protokolü ve kimlik ağ geçidi desenleri](./media/iot-edge-as-gateway/edge-as-gateway.png)

## <a name="use-cases"></a>Uygulama alanları
Tüm ağ geçidi desenler aşağıdaki avantajları sağlar:
* **Analiz** – buluta tam uygunluklu telemetri göndermeden aşağı akış cihazlardan gelen verileri yerel olarak işlemek için kullanım yapay ZEKA Hizmetleri. Bul ve yerel olarak ınsights'a react ve yalnızca IOT Hub'ına verilerin bir alt kümesini göndermek. 
* **Aşağı Akış cihaz yalıtım** – ağ geçidi cihazı internet maruz kalma riskinizi tüm aşağı akış cihazlarından korumalı yapabilmeniz için. Bağlantı yok. bir OT ağ ile web erişim sağlayan bir BT ağ aralığındaki yerleştirilebilir. 
* **Bağlantı çoğullama** - aynı temel alınan bağlantı IOT Edge ağ geçidi kullanarak bir IOT hub'a bağlanan tüm cihazlar.
* **Trafik düzeltmesi** -IOT Edge cihazı tarafından otomatik olarak uygulanır üstel geri alma IOT Hub iletilerini yerel olarak kalıcı hale getirilmesine trafiği kısıtlarsa. Bu Avantajdan çözümünüzü karşı dayanıklı trafiğindeki ani artışları getirir.
* **Çevrimdışı Destek** - ağ geçidi cihazı iletileri depolar ve ikizi güncelleştirmeleri IOT Hub'ına teslim edilemiyor.

Çeviri protokol ağ geçidi, uç analizi, cihaz yalıtımı, trafik düzeltmesi ve var olan cihazların ve kısıtlı kaynak yeni cihazları için çevrimdışı desteği de gerçekleştirebilirsiniz. Birçok mevcut cihaz iş öngörüleri genome veri oluşturduğunu; ancak bunlar bulut bağlantısı aklınızda tasarlanmamıştır. Donuk bir ağ geçidi bu verileri kilidi açılabilir ve IOT çözümlerinin kullanılan izin verin.

Kimlik çeviri yapan ağ geçidi protokol çevirisi avantajlarını sağlar ve ayrıca aşağı akış cihazları buluttan tam yönetilebilirlik sağlar. Kullandıkları protokolün bağımsız olarak IOT Hub, IOT çözümünüzün tüm cihazlar gösterilir.

## <a name="cheat-sheet"></a>Kopya kağıdı
IOT hub'ı temelleri saydam kullanırken karşılaştıran bir hızlı bilgi sayfası İşte opak (Protokolü) ve proxy ağ geçitleri.

| &nbsp; | Saydam bir ağ geçidi | Protokol çevirisi | Kimlik çeviri |
|--------|-------------|--------|--------|
| IOT Hub kimlik kayıt defterinde depolanan kimlikleri | Tüm bağlı cihazlarda kimlikleri | Yalnızca ağ geçidi cihazı kimliği | Tüm bağlı cihazlarda kimlikleri |
| Cihaz çifti | Kendi cihaz ikizi bağlı her cihaza sahip | Yalnızca ağ geçidi cihazı ve modül ikizlerini sahiptir. | Kendi cihaz ikizi bağlı her cihaza sahip |
| Doğrudan yöntemler ve bulut-cihaz iletilerini | Buluta bağlı her cihaz tek tek ele | Bulutta yalnızca ağ geçidi cihazı adres | Buluta bağlı her cihaz tek tek ele |
| [IOT hub'ı kısıtlamalar ve kotalar](../iot-hub/iot-hub-devguide-quotas-throttling.md) | Her cihaz için geçerlidir | Ağ geçidi cihazı için geçerlidir | Her cihaz için geçerlidir |

Donuk ağ geçidi (Protokol çevirisi) deseni kullanılırken, bu ağ geçidi üzerinden bağlanan tüm cihazlar en fazla 50 iletileri içerebilir aynı bulut-cihaz kuyruk paylaşın. Donuk ağ geçidi düzeni yalnızca birkaç cihaz her alan ağ geçidi üzerinden bağlanıyorsanız ve bunların bulut-cihaz trafiğinin düşük olduğu durumlarda kullanılmalıdır, takip eder.

## <a name="next-steps"></a>Sonraki adımlar

Saydam bir ağ geçidi ayarlama işlemleri gerçekleştirmeyi öğreneceksiniz: 

* [Saydam bir ağ geçidi olarak görev yapacak bir IOT Edge cihazı yapılandırma](how-to-create-transparent-gateway.md)
* [Azure IOT hub'a bir aşağı akış cihaz kimlik doğrulaması](how-to-authenticate-downstream-device.md)
* [Bir Azure IOT Edge ağ geçidi için bir aşağı akış cihazı bağlayın](how-to-connect-downstream-device.md)