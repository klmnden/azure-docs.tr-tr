---
title: Cihazları yapılandırma bir uzaktan izleme çözümü öğreticide - Azure | Microsoft Docs
description: Bu öğretici için Uzaktan izleme çözüm Hızlandırıcısını bağlı cihazları yapılandırma gösterilmektedir.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 03/08/2019
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: d23b7c8fa10127094fec67535333ae169f0f38f0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61453216"
---
# <a name="tutorial-configure-devices-connected-to-your-monitoring-solution"></a>Öğretici: İzleme çözümünüze bağlı cihazları yapılandırma

Bu öğreticide bağlı IoT cihazlarınızı yapılandırmak ve yönetmek için Uzaktan İzleme çözümü hızlandırıcısını kullanacaksınız. Yeni bir cihaz için çözüm Hızlandırıcısını ekleyip cihaz yapılandırabilirsiniz.

Contoso, tesislerinden birini genişletmek için yeni makineler sipariş etmiştir. Yeni makinelerin teslim edilmesini beklerken çözümünüzün davranışını test etme amacıyla bir simülasyon çalıştırmak istiyorsunuz. Simülasyonu çalıştırmak için Uzaktan izleme çözüm hızlandırıcısının ve bu sanal cihazı yapılandırma güncelleştirmeleri doğru şekilde yanıt test için yeni bir sanal altyapı cihaz ekleyin. Bu öğreticide, sanal cihazlar kullanırken, bir cihaz Geliştirici doğrudan yöntemler uygulayabilirler bir [gerçek bir cihaz için Uzaktan izleme çözüm Hızlandırıcısını bağlı](iot-accelerators-connecting-devices.md).

Bu öğreticide şunları yaptınız:

>[!div class="checklist"]
> * Sanal cihaz sağlama.
> * Sanal cihazı test etme.
> * Cihazı yeniden yapılandırma.
> * Cihazlarınızı düzenleme.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [iot-accelerators-tutorial-prereqs](../../includes/iot-accelerators-tutorial-prereqs.md)]

## <a name="add-a-simulated-device"></a>Sanal cihaz ekleme

Gidin **Device Explorer** sayfasında çözümde ve ardından **+ yeni cihaz**:

[![Sanal cihaz sağlama](./media/iot-accelerators-remote-monitoring-manage/devicesprovision-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesprovision-expanded.png#lightbox)

**New device** (Yeni cihaz) panelinde **Simulated** (Sanal) öğesini seçin, sağlanacak cihaz sayısını **1** olarak bırakın, **Faulty Engine** (Arızalı Motor) cihaz modelini seçin ve **Apply** (Uygula) öğesine tıklayarak sanal cihazı oluşturun:

[![Sanal motor cihazı sağlama](./media/iot-accelerators-remote-monitoring-manage/devicesprovisionengine-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesprovisionengine-expanded.png#lightbox)

## <a name="test-the-simulated-device"></a>Sanal cihazı test etme

Benzetimli altyapınız test etmek için cihaz telemetrisi ve özellik değerleri, raporlama seçin, cihaz listesinde üzerinde gönderiyor **Device Explorer** sayfası. Motorunuzla ilgili canlı bilgiler **Device Details** (Cihaz Ayrıntıları) panelinde görüntülenir:

[![Yeni sanal motor cihazını görüntüleme](./media/iot-accelerators-remote-monitoring-manage/devicesviewnew-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesviewnew-expanded.png#lightbox)

**Device Details** (Cihaz Ayrıntıları) sayfasında yeni cihazınızın telemetri verileri gönderdiğini doğrulayın. Cihazınızdan gelen titreşim telemetrisi akışını görüntülemek için **Vibration** (Titreşim) öğesine tıklayın:

[![Görüntülenecek telemetri akışını seçme](./media/iot-accelerators-remote-monitoring-manage/devicesvibration-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesvibration-expanded.png#lightbox)

**Device Details** (Cihaz Ayrıntıları) panelinde etiket değerleri, desteklediği metotlar ve cihaz tarafından bildirilen özellikler gibi cihaz hakkındaki diğer bilgiler görüntülenir.

Ayrıntılı tanılama bilgilerini görüntülemek için **Device Details** (Cihaz Ayrıntıları) paneline inerek **Diagnostics** (Tanılama) bölümünü görüntüleyin.

## <a name="reconfigure-a-device"></a>Cihazı yeniden yapılandırma

Sınama altyapının yapılandırma özellikleri güncelleştirmek için cihaz listesinde üzerinde seçin **Device Explorer** sayfası. Ardından **işleri**ve ardından **özellikleri**. İşler panelinde seçilen cihaz için güncelleştirilebilecek özellik değerleri gösterilir:

[![Cihazı yeniden yapılandırma](./media/iot-accelerators-remote-monitoring-manage/devicesreconfigure-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesreconfigure-expanded.png#lightbox)

Motorun konumunu güncelleştirmek için iş adını **UpdateEngineLocation**, boylamı **-122.15**, konumu **Factory 2**, enlemi **47.62** olarak ayarlayıp **Apply** (Uygula) öğesine tıklayın:

[![Cihazın özellik değerini güncelleştirme](./media/iot-accelerators-remote-monitoring-manage/devicesreconfigurephysical-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesreconfigurephysical-expanded.png#lightbox)

İşin durumunu izlemek için **View job status** (İş durumunu görüntüle) öğesine tıklayın:

[![Cihazın özellik değerini güncelleştirme](./media/iot-accelerators-remote-monitoring-manage/locationjobstatus-inline.png)](./media/iot-accelerators-remote-monitoring-manage/locationjobstatus-expanded.png#lightbox)

İş tamamlandıktan sonra **Dashboard** (Pano) sayfasına gidin. Motor cihazı haritada yeni konumunda görüntülenir:

[![Motor konumunu görüntüleme](./media/iot-accelerators-remote-monitoring-manage/enginelocation-inline.png)](./media/iot-accelerators-remote-monitoring-manage/enginelocation-expanded.png#lightbox)

## <a name="organize-your-devices"></a>Cihazlarınızı düzenleme

Operatör olarak cihazlarınızı düzenlemeyi ve yönetmeyi kolaylaştırmak için takım adıyla etiketleyebilirsiniz. Contoso'da saha hizmeti etkinlikleri için iki farklı takım vardır:

* Smart Vehicle takımı tırları ve prototip cihazlarını yönetmektedir.
* Smart Building takımı ise soğutucuları, asansörleri ve motorları yönetmektedir.

Tüm cihazlarınızı görüntülemek için gidin **Device Explorer** sayfasında ve **tüm cihazlar** Filtresi:

[![Tüm cihazları göster](./media/iot-accelerators-remote-monitoring-manage/devicesalldevices-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesalldevices-expanded.png#lightbox)

### <a name="add-tags"></a>Etiket ekleme

**Trucks** (Tırlar) ve **Prototyping** (Prototip) cihazlarını seçin. Ardından **Jobs** (İşler) öğesine tıklayın.

**Jobs** (İşler) panelinde **Tag** (Etiket) öğesini seçin, iş adını **AddConnectedVehicleTag** olarak değiştirip **FieldService** adlı bir metin etiketi ekleyip **ConnectedVehicle** değerini verin. Ardından **Apply** (Uygula) öğesine tıklayın:

[![Prototype (prototip) ve truck (tır) cihazlarına etiket ekleme](./media/iot-accelerators-remote-monitoring-manage/devicesaddtag-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesaddtag-expanded.png#lightbox)

Cihaz sayfasında tüm **Chiller** (Soğutucu), **Elevator** (Asansör) ve **Engine** (Motor) cihazlarını seçin. Ardından **Jobs** (İşler) öğesine tıklayın.

**Jobs** (İşler) panelinde **Tag** (Etiket) öğesini seçin, iş adını **AddSmartBuildingTag** olarak değiştirip **FieldService** adlı bir metin etiketi ekleyip **SmartBuilding** değerini verin. Ardından **Apply** (Uygula) öğesine tıklayın:

[![Chiller (soğutucu), elevator (asansör) ve engine (motor) cihazlarına etiket ekleme](./media/iot-accelerators-remote-monitoring-manage/devicesaddtag2-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesaddtag2-expanded.png#lightbox)

### <a name="create-filters"></a>Filtre oluşturma

Artık bu etiket değerlerini kullanarak filtre oluşturabilirsiniz. Üzerinde **Device Explorer** sayfasında **cihaz gruplarını yönetme**:

[![Cihaz gruplarını yönetme](./media/iot-accelerators-remote-monitoring-manage/devicesmanagefilters-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesmanagefilters-expanded.png#lightbox)

**FieldService** etiket adını ve koşul olarak **SmartBuilding** değerini kullanan bir metin filtresi oluşturun. Filtreyi **Smart Building** adıyla kaydedin:

[![Smart Building adlı bir filtre oluşturma](./media/iot-accelerators-remote-monitoring-manage/smartbuildingfilter-inline.png)](./media/iot-accelerators-remote-monitoring-manage/smartbuildingfilter-expanded.png#lightbox)

**FieldService** etiket adını ve koşul olarak **ConnectedVehicle** değerini kullanan bir metin filtresi oluşturun. Filtreyi **Connected Vehicle** adıyla kaydedin.

[![Connected Vehicle adlı bir filtre oluşturma](./media/iot-accelerators-remote-monitoring-manage/connectedvehiclefilter-inline.png)](./media/iot-accelerators-remote-monitoring-manage/connectedvehiclefilter-expanded.png#lightbox)

Contoso operatörü artık operasyon ekibine göre cihazları sorgulayabilir:

[![Connected Vehicle adlı bir filtre oluşturma](./media/iot-accelerators-remote-monitoring-manage/filterinaction-inline.png)](./media/iot-accelerators-remote-monitoring-manage/filterinaction-expanded.png#lightbox)

[!INCLUDE [iot-accelerators-tutorial-cleanup](../../includes/iot-accelerators-tutorial-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide Uzaktan İzleme çözümü hızlandırıcısına bağlı cihazların nasıl yapılandırılacağı ve yönetileceği gösterilmektedir. Çözüm hızlandırıcısının beklenmeyen bir uyarıda kök neden analizi gerçekleştirmek için nasıl kullanılacağını öğrenmek için sonraki öğreticiye devam edin.

> [!div class="nextstepaction"]
> [Bir uyarıda kök neden analizi yürütme](iot-accelerators-remote-monitoring-root-cause-analysis.md)
