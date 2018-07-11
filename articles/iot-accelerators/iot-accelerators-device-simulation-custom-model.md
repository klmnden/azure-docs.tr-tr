---
title: Özel cihaz modeli - Azure'ı yapılandırma | Microsoft Docs
description: Bu makalede, bir özel cihaz modeli cihaz benzetimi çözüm Hızlandırıcısını yapılandırma açıklanır.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 07/06/2018
ms.topic: conceptual
ms.openlocfilehash: 61a7c5314df541b4b44a286efe982f4bf93256e3
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37967553"
---
# <a name="device-models"></a>Cihaz modelleri

Kullanmayı seçebileceğiniz bir simülasyon yapılandırdığınızda önceden tanımlanmış bir şablon ile var olan bir cihaz modeli algılayıcılar ayarlayın veya sanal sensörlerden sizin seçtiğiniz bir özel cihaz modeli oluşturun. Özel algılayıcıları daha yakından gerçek cihazlarınızı modeli sağlar.

## <a name="pre-configured-device-models"></a>Önceden yapılandırılmış cihaz modelleri

Cihaz benzetimi çözüm Hızlandırıcısını üç önceden yapılandırılmış cihaz modeli sağlar: Chillers Elevators ve kamyon.

Önceden yapılandırılmış cihaz modelleri, bir JavaScript dosyasında tanımlanan Gelişmiş davranışları ile birden çok algılayıcınız varsa. Web UI'da Bu davranışların yapılandıramazsınız.

Aşağıdaki tabloda her önceden yapılandırılmış cihaz modeli yapılandırmalarını gösterilmektedir:

| Cihaz modeli  | Algılayıcı           | Birim  |
| ------------- | ---------------- | ----- |
| Soğutucu       | nem oranı         | %     |
|               | basınç         | psig  |
|               | sıcaklık      | F     |
| Asansör      | Kat            |       |
|               | Titreşim        | aa    |
|               | Sıcaklık      | F     |
| Kamyon         | Enlem         |       |
|               | Boylam        |       |
|               | hızı            | mil hızla   |
|               | cargotemperature | F     |

## <a name="custom-device-models"></a>Özel cihaz modelleri

Özel cihaz modelleri izin modeli daha yakından kendi aygıtlarını model sensörlerden. Özel bir cihaz, en fazla 10 özel algılayıcıları olabilir.

Özel cihaz modeli türünü seçtiğinizde, yeni sensörlerden tıklayarak eklediğiniz **+ Ekle algılayıcı**.

[![Algılayıcı yapılandırma](./media/iot-accelerators-device-simulation-custom-model/configuresensors-inline.png)](./media/iot-accelerators-device-simulation-custom-model/configuresensors-expanded.png#lightbox)

Özel algılayıcıları aşağıdaki özelliklere sahiptir:

| Alan     | Açıklama |
| --------- | ----------- |
| Algılayıcı adı | Algılayıcı gibi için bir kolay ad **sıcaklık** veya **hızı**.  |
| Davranışı  | Davranışlar bir iletiden sonraki gerçek veri benzetimini yapmak için değişen telemetri verilerini etkinleştirin. **Artırma** minimum değerde başlayıp gönderilen her ileti tek değer artar. En yüksek değer ulaşıldığında, ardından üzerinden yeniden en düşük değer başlar. **Azaltma** aynı şekilde davranır **artışı** ancak değerden aşağı sayıyor. **Rastgele** davranışı ve en küçük değeri en yüksek değerleri arasında rastgele bir değer oluşturur. |
| En düşük değer | Kabul edilebilir aralığınızı düşük numarası. |
| En yüksek değer | Kabul edilebilir aralığınızı en büyük sayı. |
| Birim      | F veya mil hızla ° gibi algılayıcı için ölçü birimi. |

## <a name="next-steps"></a>Sonraki adımlar

Bu nasıl yapılır kılavuzunda, bir benzetimi için bir özel cihaz modeli yapılandırılacağını öğrendiniz. Ardından, bazı diğer incelemek isteyebilirsiniz [IOT Çözüm Hızlandırıcıları](about-iot-accelerators.md).