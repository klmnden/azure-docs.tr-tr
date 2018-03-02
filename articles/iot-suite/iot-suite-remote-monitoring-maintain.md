---
title: "Uzaktan izleme çözümünde - Azure aygıtlarla ilgili sorunları giderme | Microsoft Docs"
description: "Bu öğretici Uzaktan izleme çözümüne cihaz sorunlarını gidermek ve gösterilmektedir."
services: 
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 02/22/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: dd01246075a5c0db0ed49133ed51fb56d8fcf8e5
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="troubleshoot-and-remediate-device-issues"></a>Aygıt sorunlarını gidermek ve

Bu öğretici nasıl kullanılacağını gösterir **Bakım** sorun giderme ve cihaz sorunlarını düzeltmek için çözümdeki sayfası. Bu özellikler tanıtmak için öğretici senaryo Contoso IOT uygulamada kullanır.

Contoso yeni sınamak **prototip** alanında aygıt. Contoso operatör olarak, test sırasında fark **prototip** sıcaklık alarm Panoda aygıt tetikleme beklenmedik biçimde. Şimdi bu hatalı davranışını araştırmanız gerekir **prototip** aygıt.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

>[!div class="checklist"]
> * Kullanım **Bakım** uyarı araştırmak için sayfası
> * Sorunu düzeltmek için bir aygıt yöntemini çağırın

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi izlemek için Azure aboneliğinizde Uzaktan izleme çözümü dağıtılan bir örneğini gerekir.

Uzaktan izleme çözümü dağıtılan henüz henüz tamamlanmış olmalıdır, [önceden yapılandırılmış Uzaktan izleme çözümü dağıtma](iot-suite-remote-monitoring-deploy.md) Öğreticisi.

## <a name="use-the-maintenance-dashboard"></a>Bakım Panosu kullanın

Üzerinde **Pano** fark ile ilişkilendirilen kural'ten gelen beklenmeyen sıcaklık alarmlar vardır sayfa **prototip** aygıtları:

![Panoda gösteren uyarılar](media/iot-suite-remote-monitoring-maintain/dashboardalarm.png)

Sorunu daha fazla araştırmak için seçin **keşfedin Alarm** uyarının yanındaki seçeneği:

![Panodan alarm keşfedin](media/iot-suite-remote-monitoring-maintain/dashboardexplorealarm.png)

Uyarı ayrıntı görünümünü gösterir:

* Ne zaman uyarı tetiklendi
* Alarm ile ilişkili cihazlar hakkındaki durum bilgilerini
* Telemetri alarm ile ilişkilendirilen aygıtlardan

![Uyarı ayrıntıları](media/iot-suite-remote-monitoring-maintain/maintenancealarmdetail.png)

Uyarı onaylamak için seçin **Alarm oluşum** ve **kabul**. Bu eylem uyarı gördünüz ve üzerinde çalıştığınız görmek diğer işleçler sağlar.

![Alarmlar Onayla](media/iot-suite-remote-monitoring-maintain/maintenanceacknowledge.png)

Uyarı onayladığınızda oluşum durum değişikliklerini **onaylanan**.

Listede, gördüğünüz **prototip** aygıt sıcaklık alarm tetikleme sorumlu:

![Uyarı neden cihazlar listesi](media/iot-suite-remote-monitoring-maintain/maintenanceresponsibledevice.png)

## <a name="remediate-the-issue"></a>Sorunu düzeltmek

Sorunu düzeltmek için **prototip** aygıtı ihtiyacınız çağırmak **DecreaseTemperature** cihazda yöntemi.

Bir cihaz üzerinde çalışmak için aygıtlar listesinde seçin ve ardından **zamanlama**. **Prototip** cihaz modeli belirtir dört yöntem bir aygıtı desteklemesi gerekir:

![Aygıt destekler yöntemlerini görüntülemek](media/iot-suite-remote-monitoring-maintain/maintenancemethods.png)

Seçin **DecreaseTemperature** ve iş adı ayarlamak **DecreaseTemperature**. Ardından **Uygula**:

![Sıcaklık azaltmak için proje oluşturma](media/iot-suite-remote-monitoring-maintain/maintenancecreatejob.png)

İşin durumunu izlemek için **Bakım** sayfasında, **işleri**. Kullanım **işleri** tüm işleri izlemek için görüntüleme ve yöntemini çağıran çözümde:

![Sıcaklık azaltmak için iş İzleyicisi](media/iot-suite-remote-monitoring-maintain/maintenancerunningjob.png)

Belirli bir proje veya yöntem çağrısı ayrıntılarını görüntülemek için listede seçin **işleri** görünümü:

![İş ayrıntılarını görüntüleme](media/iot-suite-remote-monitoring-maintain/maintenancejobdetail.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide gördüğünüz nasıl yapılır:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Kullanım **Bakım** uyarı araştırmak için sayfası
> * Sorunu düzeltmek için bir aygıt yöntemini çağırın

Aygıt sorunları nasıl yöneteceğinizi öğrendiniz artık, önerilen sonraki adıma edinmektir nasıl [çözümünüzü sanal cihazlar ile Test](iot-suite-remote-monitoring-test.md).

<!-- Next tutorials in the sequence -->