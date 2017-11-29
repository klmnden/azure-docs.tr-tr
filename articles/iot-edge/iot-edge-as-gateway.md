---
title: "Diğer cihazlar için nasıl bir Azure IOT sınır cihazı bir ağ geçidi olarak kullanılabileceğini anlamak | Microsoft Docs"
description: "Saydam, donuk oluşturmak için Azure IOT kenar veya birden çok aşağı akış aygıtlardan buluta veri gönderir veya yerel olarak işlemleri proxy ağ geçidi cihazı kullanın."
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 11/27/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: c1ae74127fce40a6f1ab412f25797076dda9d888
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2017
---
# <a name="how-an-iot-edge-device-can-be-used-as-a-gateway---preview"></a>Bir IOT sınır cihazı bir ağ geçidi olarak - kullanılabilir nasıl Önizleme

Ağ geçitleri IOT çözümlerinde amacı çözüme özeldir ve cihaz bağlantısı kenar analytics ile birleştirin. Azure IOT kenar bağlantısı, kimlik veya edge analytics ilgili bağımsız olarak bir IOT ağ geçidi için tüm gereksinimlerini karşılamak için kullanılabilir. Bu makalede ağ geçidi desenleri nasıl aygıt verilerini ağ geçidinde işlenmez özellikleri aşağı akış cihaz bağlantısı ve cihaz kimliği için yalnızca bakın.

## <a name="patterns"></a>Desenler
Bir IOT sınır cihazı bir ağ geçidi olarak kullanma üç desenleri vardır: saydam protokol çevirisi ve kimlik çeviri:
* **Saydam** – teorik olarak IOT Hub'ına bağlantısı kurulamadı. aygıtları bağlanabileceği bir ağ geçidi aygıtına bunun yerine. Bu, aşağı akış cihazları kendi IOT hub'ı kimliklerine sahip ve MQTT, AMQP veya HTTP protokollerden herhangi birini kullanarak anlamına gelir. Ağ geçidi, yalnızca IOT Hub ve aygıtlar arasında iletişimi geçirir. Cihazlar bir ağ geçidi üzerinden buluta ile iletişim kurmasını ve IOT Hub'cihazları etkileşimde bir kullanıcı ara ağ geçidi aygıtı farkında değildir farkında değildir. Bu nedenle, ağ geçidi saydamdır. Başvurmak [saydam bir ağ geçidi oluşturmak] [ lnk-iot-edge-as-transparent-gateway] IOT sınır cihazı saydam bir ağ geçidi olarak kullanma özellikleri için yapılır.
* **Protokol çevirisi** – MQTT, AMQP veya HTTP desteklemeyen cihazlar IOT Hub'ına veri göndermek için bir ağ geçidi cihazı kullanın. Ağ geçidi aşağı akış cihazlar tarafından kullanılan bu protokolü anlamak akıllı; ancak IOT hub'da kimliğe sahip yalnızca aygıt kullanılır. Bir aygıttan ağ geçidi gelen tüm bilgileri görülüyor. Bu neden aygıt başına temelinde verilerle ilgili bulut uygulamalarını istiyorsanız, Aşağı Akış aygıtları ek tanımlayıcı bilgileri, iletilerinde gömülü gerekir anlamına gelir. Ayrıca, IOT hub'ı temelleri twin ister ve yöntemleri yalnızca olmayan aşağı akış cihazların ağ geçidi aygıtı için kullanılabilir.
* **Kimlik çeviri** -IOT Hub'ına bağlanamıyor cihazları bağlamak için aşağı akış aygıtlar adına kimlik ve protokol çevirisi IOT hub'ı sağlayan bir ağ geçidi cihazı. Aşağı Akış cihazlar tarafından kullanılan protokol anlamak, kimlik sağlama ve IOT hub'ı temelleri çevirmek akıllı ağ geçididir. Aşağı Akış cihazlar IOT Hub ' birinci sınıf cihazlarla çiftlerini ve yöntemleri olarak görünür. Bir kullanıcı aygıtları IOT hub ile etkileşim kurabilir vermez ara ağ geçidi aygıtı farkında değildir.

![Ağ geçidi desenleri diyagramları][1]

## <a name="use-cases"></a>Uygulama alanları
Tüm ağ geçidi desenleri aşağıdaki avantajları sağlar:
* **Kenar analytics** – kullanım AI Hizmetleri yerel olarak göndermeden aşağı akış aygıtlardan gelen verileri işlemek için tam buluta uygunluğunu telemetri. Bul ve yerel olarak ınsights'a tepki ve yalnızca bir veri alt kümesini IOT Hub'ına gönderir. 
* **Aşağı Akış aygıt yalıtım** – ağ geçidi aygıtı Internet maruz tüm aşağı akış aygıtlardan kalkanı. Bağlantısı olmayan bir OT ağ ve web erişim sağlayan bir BT ağ Between sit. 
* **Bağlantı çoğullama** - IOT Hub'ına bağlanan tüm cihazlar IOT kenar aynı temel alınan bağlantı aygıt kullanır.
* **Trafik düzgünleştirme** -IOT sınır cihazı otomatik olarak IOT Hub'ın azaltma, iletileri yerel olarak kalıcı ederken kaybedildiği üstel geri alma uygulaması. Bu, çözümünüzün dayanıklı trafiğinin ani için hale getirir.
* **Sınırlı Çevrimdışı Destek** - ağ geçidi aygıtı iletileri yerel olarak depolamak ve twin güncelleştirmeleri IOT Hub'ına teslim edilemiyor.

Bir ağ geçidi mu protokol çevirisi kenar analytics, cihaz yalıtım, trafiği düzgünleştirme ve var olan cihazların ve kısıtlı kaynak yeni cihazları için çevrimdışı destek de gerçekleştirebilirsiniz. Birçok var olan cihazları iş öngörüleri güç veri oluşturduğunu; ancak bunlar aklınızda bulut bağlantı tasarlanmamıştır. Donuk ağ geçitleri kilidi açılabilir ve uçtan uca IOT çözümünü göstermesi bu veri izin verin.

Kimlik çeviri yapan ağ geçidi protokol çevirisi yararları sağlamak ve ayrıca bulutta aşağı akış aygıtların tam yönetilebilirlik izin verir. IOT çözüm gösterisini IOT Hub'ındaki bunlar protokolüyle bakılmaksızın tüm cihazların konuşun.

## <a name="cheat-sheet"></a>Kopya kağıdı
Burada, IOT hub'ı temelleri kullanırken saydam, donuk karşılaştırır sayfası ve proxy ağ geçitleri hızlı bilgi verilmiştir.

| &nbsp; | Saydam ağ geçidi | Protokol çevirisi | Kimlik çevirisi |
|--------|-------------|--------|--------|
| IOT Hub kimlik kayıt defterinde depolanan kimlikleri | Tüm bağlı cihazların kimlikleri | Yalnızca ağ geçidi cihaz kimliği | Tüm bağlı cihazların kimlikleri |
| Cihaz çifti | Kendi cihaz çifti bağlı her cihazın vardır | Yalnızca ağ geçidi aygıtı ve modülü çiftlerini vardır | Kendi cihaz çifti bağlı her cihazın vardır |
| Doğrudan yöntemleri ve bulut-cihaz iletilerini | Bulut bağlı her aygıt ayrı ayrı ele alabilir | Bulut yalnızca ağ geçidi aygıtı adres | Bulut bağlı her aygıt ayrı ayrı ele alabilir |
| [IOT hub'ı kısıtlamaları ve kotaları][lnk-iothub-throttles-quotas] | Her bir aygıtı Uygula | Ağ geçidi aygıtı Uygula | Her bir aygıtı Uygula |

Donuk ağ geçidi deseni kullanılırken, bu ağ geçidi üzerinden bağlanan tüm cihazlar en çok 50 iletileri içerebilir aynı bulut cihaz sırası paylaşır. Donuk ağ geçidi deseni yalnızca çok az aygıtları her alan ağ geçidi üzerinden bağlanan ve kendi bulut cihaz trafiğinin düşük olduğu zaman kullanılmalıdır olduğunu izler.

## <a name="next-steps"></a>Sonraki adımlar
Bir IOT sınır cihazı olarak kullanın bir [saydam ağ geçidi][lnk-iot-edge-as-transparent-gateway] 

[lnk-iot-edge-as-transparent-gateway]: ./how-to-create-transparent-gateway.md
[lnk-iothub-throttles-quotas]: ../iot-hub/iot-hub-devguide-quotas-throttling.md

[1]: ./media/iot-edge-as-gateway/edge-as-gateway.png