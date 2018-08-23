---
title: Karşıya dosya yüklemeyi yapılandırma için Azure portalını kullanma | Microsoft Docs
description: Bağlı cihazlardan dosya yüklemeleri etkinleştirmek için IOT hub'ınızı yapılandırmak için Azure portalını kullanma ' % S'hedef Azure depolama hesabı yapılandırma hakkında bilgi içerir.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/03/2017
ms.author: dobett
ms.openlocfilehash: a9f9eeaed2716c5d492099568fd6f90080471af2
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/17/2018
ms.locfileid: "42061095"
---
# <a name="configure-iot-hub-file-uploads-using-the-azure-portal"></a>Azure portalını kullanarak dosya yüklemeleri IOT hub'ı yapılandırma

[!INCLUDE [iot-hub-file-upload-selector](../../includes/iot-hub-file-upload-selector.md)]

## <a name="file-upload"></a>Karşıya dosya yükleme

Kullanılacak [dosya karşıya yükleme işlevselliği IOT Hub'ında](iot-hub-devguide-file-upload.md), önce hub'ınıza bir Azure depolama hesabı ilişkilendirmeniz gerekir. Seçin **karşıya dosya yükleme** dosya karşıya yükleme özelliklerini değiştirilmekte olan IOT hub'ının bir listesini görüntüleyin.

![Görünüm IOT hub'ı dosyasını karşıya yükleyin, Portalı'ndaki ayarları](./media/iot-hub-configure-file-upload/file-upload-settings.png)

* **Depolama kapsayıcısı**: Azure portalı ile IOT Hub'ınızı ilişkilendirmek için geçerli Azure aboneliğinizde bir Azure depolama hesabındaki bir blob kapsayıcısını seçmek için kullanın. Gerekirse, bir Azure depolama hesabı oluşturabilirsiniz, **depolama hesapları** dikey penceresinde ve blob kapsayıcısını **kapsayıcıları** dikey penceresi. IOT hub'ı SAS URI'leriyle bu blob kapsayıcısında, karşıya dosya yükleme sırasında kullanılacak cihazlar için yazma izinlerine sahip otomatik olarak oluşturur.

   ![Portalda depolama kapsayıcıları karşıya dosya yükleme için görüntüleme](./media/iot-hub-configure-file-upload/file-upload-container-selection.png)

* **Karşıya yüklenen dosyalar için bildirimlerin**: etkinleştirmek veya devre dışı dosya karşıya yükleme bildirimleri aracılığıyla Aç/Kapat.

* **SAS TTL**: zaman yaşam IOT Hub tarafından cihaza verilen SAS URI'ın bu ayardır. Bir saat için varsayılan olarak ayarlanmış, ancak kaydırıcıyı kullanarak diğer değerleri için özelleştirilebilir.

* **Dosya bildirim ayarları varsayılan TTL**: geçerlilik süresi doluncaya kadar önce bildirim zaman yaşam dosyasının karşıya. Bir gün için varsayılan olarak ayarlanmış, ancak kaydırıcıyı kullanarak diğer değerleri için özelleştirilebilir.

* **Dosya bildirim en yüksek teslimat sayısı**: IOT hub'ı bir dosyayı teslim etmek için kaç deneme sayısı bildirim karşıya yükleyin. Varsayılan olarak 10'a ayarlanmış ancak kaydırıcıyı kullanarak diğer değerleri için özelleştirilebilir.

   ![Portalda IOT hub'ı dosya yüklemeyi yapılandırma](./media/iot-hub-configure-file-upload/file-upload-selected-container.png)

## <a name="next-steps"></a>Sonraki adımlar

IOT hub'ı dosya karşıya yükleme özellikleri hakkında daha fazla bilgi için bkz. [dosyaları bir CİHAZDAN karşıya yükle](iot-hub-devguide-file-upload.md) IOT Hub Geliştirici Kılavuzu'nda.

Azure IOT hub'ı yönetme hakkında daha fazla bilgi için bu bağlantıları izleyin:

* [IoT cihazlarını toplu yönetme](iot-hub-bulk-identity-mgmt.md)
* [IOT hub'ı ölçümleri](iot-hub-metrics.md)
* [İşlemleri izleme](iot-hub-operations-monitoring.md)

Daha fazla IOT Hub'ın özelliklerini keşfetmek için bkz:

* [IOT Hub Geliştirici Kılavuzu](iot-hub-devguide.md)
* [Yapay ZEKA, Azure IOT Edge ile uç cihazlarına dağıtma](../iot-edge/tutorial-simulate-device-linux.md)
* [IOT çözümünüzü baştan güvenli hale getirme](../iot-fundamentals/iot-security-ground-up.md)
