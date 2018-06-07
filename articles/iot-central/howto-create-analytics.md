---
title: Azure IOT merkezi uygulamanız için özel analytics oluşturun | Microsoft Docs
description: Bir operatör olarak Azure IOT merkezi uygulamanız için özel analytics oluşturma.
author: tbhagwat3
ms.author: tanmayb
ms.date: 04/16/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 5facdf3f02b71e154a23d8f26c7bcc40b5c71e35
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34629312"
---
# <a name="how-to-use-analytics-to-analyze-your-device-data"></a>Cihaz verilerinizi çözümlemek için Analytics kullanma

Microsoft Azure IOT Merkezi, büyük miktarlarda veri cihazlarınızdan anlamlı zengin analytics yetenekleri sağlar. Analytics görüntülemek ve verileri analiz etmek için kullanabileceğiniz bir [aygıt kümesi](howto-use-device-sets.md) uygulamanızda. Bir cihaz kümesi aygıtlarınızın kullanıcı tanımlı bir gruptur. Cihazlar, küçük bir kümesini veya tek bir cihazı çözümleme daraltabilirsiniz.

Bir operatör seçin **Analytics** sol gezinti menüsünde, bir cihaz kümesi seçin ve ardından grafikte görüntülenecek ölçümleri. Daha fazla veri dilimi için kullanabileceğiniz birkaç araçlar şunlardır:

* **Ölçümler:** sıcaklık ve nem gibi görüntülemek için ölçümleri seçin.
* **Toplama:** ölçümleri toplama işlevi seçin. Örneğin, **Minimum** veya **ortalama**.
* **Bölünmüş tarafından:** detaya gitme aygıt özelliklerini veya aygıt adı verilerle bölerek. Örneğin, aygıt konuma göre böl:

     ![Analitik ana sayfası](media\howto-create-analytics\analytics-main.png)

* **Zaman aralığı:** zaman aralığı tanımlanmış bir zaman aralıkları birini veya bir özel zaman aralığı oluşturun: ![Analytics zaman aralığı](media\howto-create-analytics\analytics-time-range.png)

## <a name="change-the-visualizations"></a>Görselleştirmeleri değiştirme

Grafikler üç modlarından birini seçerek görselleştirme gereksinimlerinizi karşılayacak şekilde değiştirebilirsiniz:

* **Yığılmış:** her ölçüm için bir grafik Yığılmış olduğunda ve her grafikleri kendi y ekseni. Yığılmış grafik seçili birden çok ölçümleri sahip olduğunuzda ve farklı bir görünüm Bu ölçümlerin istediğinizde faydalıdır.
* **Yığılmamış:** bir grafik her ölçü bir y ekseni karşı çizilme ancak y değerleri değiştirilen vurgulanan ölçümü temel alarak. Yığılmamış grafikleri, birden çok ölçü kaplama ve aynı zaman aralığına ilişkin Bu ölçümler arasında desenleri görmek istediğinizde yararlıdır.
* **Paylaşılan y ekseni:** tüm grafikleri aynı y ekseni paylaşabilir ve eksen için değerleri değiştirmeyin. Paylaşılan y ekseni grafikleri bölünmüş tarafından verilerle dilimleme sırasında tek bir ölçü aramak istediğinizde faydalıdır.

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT merkezi uygulamanız için özel analytics oluşturmak öğrendiniz, önerilen sonraki adıma şöyledir:

> [!div class="nextstepaction"]
> [Hazırlama ve bir Node.js uygulaması bağlanın](howto-connect-nodejs.md)