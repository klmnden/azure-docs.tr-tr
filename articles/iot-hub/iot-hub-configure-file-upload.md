---
title: Karşıya dosya yüklemeyi yapılandırma için Azure portalını kullanma | Microsoft Docs
description: Bağlı cihazlardan dosya yüklemeleri etkinleştirmek için IOT hub'ınızı yapılandırmak için Azure portalını kullanma ' % S'hedef Azure depolama hesabı yapılandırma hakkında bilgi içerir.
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/03/2017
ms.author: robinsh
ms.openlocfilehash: bd7cc37b8fc81fc9d4109826743f2243913d0604
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60735052"
---
# <a name="configure-iot-hub-file-uploads-using-the-azure-portal"></a>Azure portalını kullanarak dosya yüklemeleri IOT hub'ı yapılandırma

[!INCLUDE [iot-hub-file-upload-selector](../../includes/iot-hub-file-upload-selector.md)]

## <a name="file-upload"></a>Karşıya dosya yükleme

Kullanılacak [dosya karşıya yükleme işlevselliği IOT Hub'ında](iot-hub-devguide-file-upload.md), önce hub'ınıza bir Azure depolama hesabı ilişkilendirmeniz gerekir. Seçin **karşıya dosya yükleme** dosya karşıya yükleme özelliklerini değiştirilmekte olan IOT hub'ının bir listesini görüntüleyin.

![Görünüm IOT hub'ı dosyasını karşıya yükleyin, Portalı'ndaki ayarları](./media/iot-hub-configure-file-upload/file-upload-settings.png)

* **Depolama kapsayıcısı**: Azure portalı ile IOT Hub'ınızı ilişkilendirmek için geçerli Azure aboneliğinizde bir Azure depolama hesabındaki bir blob kapsayıcısını seçmek için kullanın. Gerekirse, bir Azure depolama hesabı oluşturabilirsiniz, **depolama hesapları** dikey penceresinde ve blob kapsayıcısını **kapsayıcıları** dikey penceresi. IOT hub'ı SAS URI'leriyle bu blob kapsayıcısında, karşıya dosya yükleme sırasında kullanılacak cihazlar için yazma izinlerine sahip otomatik olarak oluşturur.

   ![Portalda depolama kapsayıcıları karşıya dosya yükleme için görüntüleme](./media/iot-hub-configure-file-upload/file-upload-container-selection.png)

* **Karşıya yüklenen dosyalar için bildirimlerin**: Etkinleştirmek veya dosya karşıya yükleme bildirimleri aracılığıyla iki durumlu düğme devre dışı.

* **SAS TTL**: Bu ayar, zaman yaşam IOT Hub tarafından cihaza verilen SAS URI'ın desteklenir. Bir saat için varsayılan olarak ayarlanmış, ancak kaydırıcıyı kullanarak diğer değerleri için özelleştirilebilir.

* **Dosya bildirim ayarları varsayılan TTL**: Geçerlilik süresi doluncaya kadar önce zaman yaşam dosyasının bildirim karşıya yükleyin. Bir gün için varsayılan olarak ayarlanmış, ancak kaydırıcıyı kullanarak diğer değerleri için özelleştirilebilir.

* **Dosya bildirim en yüksek teslimat sayısı**: IOT hub'ı bir dosyayı teslim girişiminde sayısı bildirim karşıya yükleyin. Varsayılan olarak 10'a ayarlanmış ancak kaydırıcıyı kullanarak diğer değerleri için özelleştirilebilir.

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
