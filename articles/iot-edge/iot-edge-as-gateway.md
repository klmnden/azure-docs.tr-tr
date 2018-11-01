---
title: Azure IOT Edge cihazları ağ geçidi olarak kullanma | Microsoft Docs
description: Saydam, donuk oluşturmak için Azure IOT Edge ya da verileri birden çok akış CİHAZDAN buluta gönderiyor veya yerel olarak işleme proxy ağ geçidi cihazı kullanın.
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 09/21/2017
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 71fce04eb4f21065184d86301689039294da8b61
ms.sourcegitcommit: ae45eacd213bc008e144b2df1b1d73b1acbbaa4c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2018
ms.locfileid: "50741921"
---
# <a name="how-an-iot-edge-device-can-be-used-as-a-gateway"></a>Bir ağ geçidi olarak IOT Edge cihazının nasıl kullanılabileceğini

IOT çözümlerinde ağ geçitleri amacı çözüme özgüdür ve cihaz bağlantısı, uç Analizi ile birleştirin. Azure IOT Edge, bir IOT ağ geçidi bağlantısı, kimlik veya uç analizi ilişkili oldukları bağımsız olarak tüm ihtiyaçlarınızı karşılamak için kullanılabilir. Bu makalede bir ağ geçidi desenleri özellikleri aşağı akış cihaz bağlantısı ve cihaz kimliği için cihaz verilerini ağ geçidi üzerinde nasıl işleneceğini değil yalnızca bakın.

## <a name="patterns"></a>Desenler
Bir ağ geçidi olarak IOT Edge cihazı kullanmak için üç desen vardır: saydam, protokol çevirisi ve kimlik çeviri:
* **Saydam** – teorik olarak IOT Hub'ına bağlanabilir cihazlara bağlanabilir bir ağ geçidi cihazı için bunun yerine. Aşağı Akış cihazlar IOT hub'ı kimliklerinin ve MQTT, AMQP veya HTTP protokolünü kullanarak. Ağ geçidi, yalnızca IOT hub'ı ve cihazlar arasında iletişim geçirir. Cihazlar bir ağ geçidi üzerinden bulutla iletişim kurma ve IOT hub'ında cihazlarla etkileşim bir kullanıcı bir ara ağ geçidi cihazı habersizdir farkında değildir. Bu nedenle, ağ geçidi saydamdır. Başvurmak [saydam bir ağ geçidi oluşturma] [ lnk-iot-edge-as-transparent-gateway] saydam bir ağ geçidi olarak IOT Edge cihazı kullanma özellikleri için nasıl yapılır.
* **Protokol çevirisi** – donuk bir ağ geçidi düzeni, bir ağ geçidi cihazı IOT Hub'ına veri göndermek için MQTT, AMQP veya HTTP Kullan'ı desteklemeyen cihazlar olarak da bilinir. Ağ geçidi, aşağı akış cihazlar tarafından kullanılan bu protokolü anlamak akıllı; Ancak, IOT Hub'ında kimlik olan yalnızca cihazdır. Tüm bilgiler, bir CİHAZDAN bir ağ geçidi geliyor gibi görünüyor. Cihaz başına üzerindeki verileri neden bulut uygulamaları istiyorsanız aşağı akış cihazları, iletileri ek kimlik bilgileri katıştırmanız gerekir. Buna ek olarak, IOT hub'ı temelleri ikizi ve gibi yöntemler yalnızca olmayan aşağı akış cihazların ağ geçidi cihazı için kullanılabilir.
* **Kimlik çeviri** -IOT Hub'ına bağlanamıyor cihazları, bunun yerine bir ağ geçidi cihazı bağlayın. Ağ geçidi, IOT Hub adına aşağı akış cihazları kimlik ve protokol çevirisi sağlar. Aşağı Akış cihazlar tarafından kullanılan protokol anlamanıza, bunları kimlik sağlama ve IOT hub'ı temelleri çevirmek akıllı geçididir. Aşağı Akış cihazlar IOT Hub'ında birinci sınıf cihaz ikizleri ve yöntemler olarak görünür. Bir kullanıcı, cihazlar IOT hub'ında etkileşim kurabilir ve ara ağ geçidi cihazı farkında değildir.

![Ağ geçidi desenlerinin diyagramları](./media/iot-edge-as-gateway/edge-as-gateway.png)

## <a name="use-cases"></a>Uygulama alanları
Tüm ağ geçidi desenler aşağıdaki avantajları sağlar:
* **Kenar analytics** – tam uygunluk telemetri buluta göndermeden aşağı akış cihazlardan gelen verileri yerel olarak işlemek için kullanım yapay ZEKA Hizmetleri. Bul ve yerel olarak ınsights'a react ve yalnızca IOT Hub'ına verilerin bir alt kümesini göndermek. 
* **Aşağı Akış cihaz yalıtım** – ağ geçidi cihazı internet maruz kalma riskinizi tüm aşağı akış cihazlarından korumalı yapabilmeniz için. Bağlantı yok. bir OT ağ ile web erişim sağlayan bir BT ağ aralığındaki yerleştirilebilir. 
* **Bağlantı çoğullama** - IOT hub'a bağlanan tüm cihazlar bir IOT Edge cihazı aynı temel alınan bağlantı kullanır.
* **Trafik düzeltmesi** -IOT Edge cihazı tarafından otomatik olarak uygulanır üstel geri alma IOT Hub iletilerini yerel olarak kalıcı hale getirilmesine trafiği kısıtlarsa. Bu Avantajdan çözümünüzü karşı dayanıklı trafiğindeki ani artışları getirir.
* **Sınırlı çevrimdışı desteği** - ağ geçidi cihazı iletileri yerel olarak depolayacak ve ikizi güncelleştirmeleri IOT Hub'ına teslim edilemiyor.

Bir ağ geçidi yoksa protokol çevirisi, uç analizi, cihaz yalıtımı, trafik düzeltmesi ve var olan cihazların ve kısıtlı kaynak yeni cihazları için çevrimdışı desteği de gerçekleştirebilirsiniz. Birçok mevcut cihaz iş öngörüleri genome veri oluşturduğunu; ancak bunlar bulut bağlantısı aklınızda tasarlanmamıştır. Donuk bir ağ geçidi bu verileri kilidi açılabilir ve uçtan uca IOT çözümlerinin kullanılan izin verin.

Kimlik çeviri yapan ağ geçidi protokol çevirisi avantajlarını sağlar ve ayrıca aşağı akış cihazları buluttan tam yönetilebilirlik sağlar. IOT çözüm gösterisini IOT hub'ında protokolü ile bunlar bağımsız olarak tüm cihazları konuşun.

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
IOT Edge cihazı olarak kullanan bir [saydam bir ağ geçidi][lnk-iot-edge-as-transparent-gateway] 

[lnk-iot-edge-as-transparent-gateway]: ./how-to-create-transparent-gateway-linux.md
