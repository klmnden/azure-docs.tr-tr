---
title: Karşıya dosya yükleme yapılandırmak için Azure portalını kullanma | Microsoft Docs
description: Azure portalı bağlı dosya yüklemelerini etkinleştirmek için IOT hub'ınızı yapılandırma için nasıl kullanılacağını. Hedef Azure depolama hesabı yapılandırma hakkında bilgi içerir.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/03/2017
ms.author: dobett
ms.openlocfilehash: 0100cbe4bbc66d0c4ef940cc40f4fa3441176a1a
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34633215"
---
# <a name="configure-iot-hub-file-uploads-using-the-azure-portal"></a>IOT Hub'ın Azure Portalı'nı kullanarak dosya yüklemeleri yapılandırın

[!INCLUDE [iot-hub-file-upload-selector](../../includes/iot-hub-file-upload-selector.md)]

## <a name="file-upload"></a>Karşıya dosya yükleme

Kullanılacak [dosya karşıya yükleme işlevselliği IOT hub'da][lnk-upload], bir Azure Storage hesabı hub'ınıza ilk ilişkilendirmeniz gerekir. Seçin **karşıya dosya yükleme** karşıya dosya yükleme özellikleri değiştirilmekte olan IOT hub'ına yönelik bir listesini görüntülemek için.

![Görünüm IOT hub'ı dosyayı karşıya portalındaki ayarları][13]

**Depolama kapsayıcısı**: IOT Hub ile ilişkilendirmek için geçerli Azure aboneliğinizde bir Azure depolama hesabındaki bir blob kapsayıcısını seçmek için Azure portalını kullanın. Gerekirse, bir Azure Storage hesabı oluşturabilirsiniz, **depolama hesapları** dikey ve blob kapsayıcısında **kapsayıcıları** dikey. IOT hub'ı SAS URI'ler dosyaları karşıya yükleme sırasında kullanmak cihazlar için bu blob kapsayıcısına yazma izinlerine sahip otomatik olarak oluşturur.

![Karşıya dosya yükleme için depolama kapsayıcıları portalında görüntüleyin][14]

**Karşıya yüklenen dosyalar için bildirimlerin**: etkinleştirmek veya dosya karşıya yükleme bildirimlerini geçiş aracılığıyla devre dışı.

**SAS TTL**: saat yaşam IOT Hub tarafından cihaza döndürülen SAS URI, bu ayar değildir. Varsayılan olarak bir saat için ayarlanmış ancak kaydırıcıyı kullanarak diğer değerleri için özelleştirilebilir.

**Dosya bildirim ayarları varsayılan TTL**: süresi doldu önce bildirim zaman yaşam dosyasının karşıya. Varsayılan olarak bir gün için ayarlanmış ancak kaydırıcıyı kullanarak diğer değerleri için özelleştirilebilir.

**Dosya bildirim maksimum teslimat sayısı**: IOT hub'ı bir dosya teslim etmek için kaç deneme sayısı bildirim karşıya yükleyin. Varsayılan olarak 10 olarak ayarlandı ancak kaydırıcıyı kullanarak diğer değerleri için özelleştirilebilir.

![IOT hub'ı dosya karşıya yükleme portalında yapılandırın][15]

## <a name="next-steps"></a>Sonraki adımlar

IOT hub'ı dosya karşıya yükleme özellikleri hakkında daha fazla bilgi için bkz: [karşıya bir aygıtı dosyalarından] [ lnk-upload] IOT Hub Geliştirici Kılavuzu'nda.

Azure IOT hub'ı yönetme hakkında daha fazla bilgi için bu bağlantıları izleyin:

* [Toplu IOT cihazları yönetme][lnk-bulk]
* [IOT hub'ı ölçümleri][lnk-metrics]
* [İzleme işlemleri][lnk-monitor]

Daha fazla IOT hub'ı özelliklerini keşfetmek için bkz:

* [IOT Hub Geliştirici Kılavuzu][lnk-devguide]
* [Azure IOT Edge ile sınır cihazlarına Al dağıtma][lnk-iotedge]
* [IOT çözümünüzden zemin oluşturan güvenli][lnk-securing]

[13]: ./media/iot-hub-configure-file-upload/file-upload-settings.png
[14]: ./media/iot-hub-configure-file-upload/file-upload-container-selection.png
[15]: ./media/iot-hub-configure-file-upload/file-upload-selected-container.png

[lnk-upload]: iot-hub-devguide-file-upload.md

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-securing]: iot-hub-security-ground-up.md
