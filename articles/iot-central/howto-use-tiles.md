---
title: Azure IOT Central uygulaması Pano kutucukları kullanma | Microsoft Docs
description: Bir oluşturucu, uygulama panoları, cihaz kümesi panoları ve cihaz Pano kutucukları kullanmayı öğrenin.
author: v-krghan
ms.author: v-krghan
ms.date: 06/27/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 26322461e4f53bffff2fb182df6c45ed3b83c4bf
ms.sourcegitcommit: 837dfd2c84a810c75b009d5813ecb67237aaf6b8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67503532"
---
# <a name="how-to-use-tiles"></a>Döşemeleri kullanma
Kutucukları uygulama panoları, cihaz panolarınızı özelleştirmek için kullanabileceğiniz ve cihaz panolar ayarlayın. Birden çok kutucuk bir panoya eklemek ve bu kutucuklar, uygulamanız ile ilgili bilgileri gösterecek şekilde özelleştirebilirsiniz. Kutucukları yeniden boyutlandırma ve herhangi bir Pano üzerinde düzeni özelleştirme. Uygulama Panosu farklı kutucuklar içeren aşağıdaki ekran görüntüsünde gösterilmektedir.

![Uygulama panosu](media/howto-use-tiles/image1a.png)


Azure IOT Central kutucuklar kullanımını aşağıdaki tabloda özetlenmiştir:

 
| kutucuğu | Pano | Açıklama
| ----------- | ------- | ------- |
| Bağlantı | Uygulama ve cihaz kümesi panoları |Bağlantı, başlık ve açıklama metnini tıklanabilir kutucukları döşemeleri. Bağlantı kutucuğu, uygulamanız ile ilgili bir URL'ye giderek bir kullanıcının etkinleştirmek için kullanın. |
| Image | Uygulama ve cihaz kümesi panoları |Resim kutucukları, özel bir görüntüyü ve tıklanabilir olabilir. Resim kutucuğunda bir panoya grafik ekleyin ve isteğe bağlı olarak, uygulamanızla ilgili bir URL'ye giderek bir kullanıcının etkinleştirmek için kullanın.|
| Etiket | Uygulama panoları |Etiket kutucukları özel metin bir Panoda görüntüleyin. Metin boyutu seçebilirsiniz. Bu tür açıklamaları, kişi ayrıntıları veya Yardım ilgili bilgileri panoya eklemek için bir etiket kutucuk kullanın|
| Eşleme | Uygulama ve cihaz kümesi panoları |Harita kutucukları, bir cihazın durumunu ve konumunu bir haritada görüntüler. Örneğin, bir cihaz olduğu ve fan açık olup olmadığını görüntüleyebilirsiniz.|
| Çizgi grafik | Uygulama ve cihaz panolar |Çizgi grafik kutucuklarının ölçü birleşik bir cihaz için bir süre için bir grafik görüntüler. Örneğin, ortalama sıcaklığı ve Basıncı bir cihazda son bir saat için gösteren bir çizgi grafiği görüntüleyebilirsiniz.|
| Çubuk grafik | Uygulama ve cihaz panolar |Çubuk grafik kutucukları bir süre için toplam ölçümlerin bir cihaz için bir grafik görüntüler. Örneğin, ortalama sıcaklığı ve Basıncı bir cihazda son bir saat için gösteren bir çubuk grafiği görüntüleyebilirsiniz. |
| Olay geçmişi | Uygulama ve cihaz panolar |Olay geçmişi kutucuk bir süre boyunca bir cihaz için olayları görüntüler. Örneğin, sıcaklık değişiklikleri bir cihaz için son bir saat boyunca oluştu göstermek için kullanabilirsiniz. |
| Durum geçmişi | Uygulama ve cihaz panolar |Durum geçmişi kutucuk bir süre için ölçüm değerlerini görüntüler. Örneğin, bir cihaz için sıcaklık değerleri son bir saat boyunca göstermek için kullanabilirsiniz.|
| KPI | Uygulama ve cihaz panolar | Bir süre için birleşik bir telemetri veya olay ölçümü KPI kutucuğu görüntülenir. Örneğin, bir cihaz için son bir saat boyunca ulaşıldı. maksimum sıcaklık göstermek için kullanabilirsiniz.|
| Son bilinen değer | Uygulama ve cihaz panolar |Son bilinen değer kutucuk telemetri ya da durum ölçüm için en son değeri görüntüler. Örneğin, sıcaklık, baskısı ve nem bir cihaz için en son ölçümleri görüntülemek için bu kutucuğa kullanabilirsiniz.|
| Cihaz ayarlama Kılavuzu | Uygulama ve cihaz kümesi panoları | Cihaz, cihaz hakkındaki bilgileri görüntüler kılavuz ayarlayın. Cihaz kümesinde adını, konumunu ve modeli tüm cihazların gibi bilgileri göstermek için bir cihaz kümesi kılavuz kutucuk kullanın.|


Azure IOT Central uygulamanızda Pano yapılandırma hakkında daha fazla bilgi için bkz. [yapılandırma uygulama Panosu](howto-configure-homepage.md).
