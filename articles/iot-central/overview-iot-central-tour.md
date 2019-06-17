---
title: Azure IoT Central Kullanıcı Arabirimi turuna katılın | Microsoft Docs
description: Oluşturucu olarak, bir IoT çözümü oluşturmak için kullanabileceğiniz Azure IoT Central kullanıcı arabiriminin temel alanlarını tanıyın.
author: dominicbetts
ms.author: dobett
ms.date: 06/09/2019
ms.topic: overview
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: philmea
ms.openlocfilehash: 56ff40cb2b103c620b880792571549e2bdb17064
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67064361"
---
# <a name="take-a-tour-of-the-azure-iot-central-ui"></a>Azure IoT Central Kullanıcı Arabirimi turuna katılın

Bu makalede, Microsoft Azure IoT Central kullanıcı arabirimi tanıtılmaktadır. Kullanıcı arabirimini kullanarak bir Azure IoT Central çözümü ile bağlı cihazlarını oluşturabilir, yönetebilir ve kullanabilirsiniz.

_Oluşturucu_ olarak, Azure IoT Central kullanıcı arabirimini kullanarak Azure IoT Central çözümünüzü tanımlayabilirsiniz. Kullanıcı arabirimini kullanarak şunları yapabilirsiniz:

- Çözümünüze bağlanan cihaz türlerini tanımlama.
- Cihazlarınız için kurallar ve eylemler yapılandırma.
- Kullanıcı arabirimini, çözümünüzü kullanan bir _operatör_ için özelleştirme.

_Operatör_ olarak, Azure IoT Central kullanıcı arabirimini kullanarak Azure IoT Central çözümünüzü yönetebilirsiniz. Kullanıcı arabirimini kullanarak şunları yapabilirsiniz:

- Cihazlarınızı izleme.
- Cihazlarınızı yapılandırma.
- Cihazlarınızla ilgili sorunları giderme ve düzeltme.
- Yeni cihazları hazırlama.

## <a name="use-the-left-navigation-menu"></a>Sol gezinti menüsünü kullanma

Sol gezinti menüsünde, uygulamanın farklı alanlara erişmek için kullanın. Genişlet veya seçerek gezinti çubuğunu Daralt **<** veya **>** :

| Menü | Açıklama |
| ---- | ----------- |
| ![Sol gezinti menüsü](media/overview-iot-central-tour/navigationbar.png) | <ul><li>**Pano** düğmesi uygulama Panonuzda görüntüler. Bir oluşturucu, işleçleri için panoyu özelleştirebilirsiniz. Kullanıcılar ayrıca kendi panolarınızı oluşturabilirsiniz.</li><li>**Device Explorer** düğmesi, uygulamadaki her bir cihaz şablonuyla ilişkili sanal ve gerçek cihazları listeler. Operatör olarak, **Device Explorer**’ı kullanarak bağlı cihazlarınızı yönetebilirsiniz.</li><li>**Cihaz Kümeleri** düğmesi, cihaz kümelerini görüntüleyip oluşturmanızı sağlar. Operatör olarak, bir sorgu tarafından belirtilen cihazların mantıksal bir koleksiyonu olarak cihaz kümeleri oluşturabilirsiniz.</li><li>**Analiz** düğmesi, cihazlar ve cihaz kümeleri için cihaz telemetrisinden türetilen analizleri gösterir. Operatör olarak, uygulamanızdan içgörüler türetmek için cihaz verileri üzerinde özel görünümler oluşturabilirsiniz.</li><li>**İşler** düğmesi, uygun ölçekte güncelleştirmeler gerçekleştirmek için iş oluşturmanız ve çalıştırmanız gerekmeden toplu cihaz yönetimi sağlar.</li><li>**Cihaz şablonları** düğmesini gösteren bir oluşturucu kullanan cihaz şablonları oluşturmak ve yönetmek için Araçlar.</li><li>**Verileri sürekli dışarı aktarma** depolama ve kuyruk gibi diğer Azure Hizmetleri için sürekli dışarı aktarma yapılandırmak için yönetici düğmesi.</li><li>**Yönetim** düğmesi, bir yöneticinin uygulama ayarlarını, kullanıcıları ve rolleri yönetebildiği uygulama yönetimi sayfalarını gösterir.</li></ul> |

## <a name="search-help-and-support"></a>Arama, yardım ve destek

Üstteki menü her sayfada görünür:

![Araç Çubuğu](media/overview-iot-central-tour/toolbar.png)

- Cihaz şablonlarını ve cihazlar için arama yapmak için girin bir **arama** değeri.
- Kullanıcı Arabirimi dilini veya tema değiştirme tercih **ayarları** simgesi.
- Uygulamadaki oturumunu oturum açmayı tercih **hesabı** simgesi.
- Yardım ve destek almak için, kaynak listesinden **Yardım** açılır listesini seçin. Bir deneme uygulamasında, destek kaynakları erişimini içerir. [canlı sohbet](howto-show-hide-chat.md).

Kullanıcı arabirimi için açık renkli tema veya koyu renkli temayı seçebilirsiniz:

![Kullanıcı arabirimi için tema seçme](media/overview-iot-central-tour/themes.png)

> [!NOTE]
> Açık ve koyu temalar arasında seçim yapma olanağı, uygulama için özel bir tema yöneticinize yapılandırdıysa kullanılamaz.

## <a name="dashboard"></a>Pano

![Pano](media/overview-iot-central-tour/homepage.png)

* Azure IOT Central uygulamanıza oturum açtığınızda gördüğünüz ilk sayfa panodur. Bir oluşturucu, diğer kullanıcılar için uygulama Panosu döşemeler ekleyerek özelleştirebilirsiniz. Daha fazla bilgi almak için [Azure IoT Central operatör görünümünü özelleştirme](tutorial-customize-operator.md) öğreticisine bakın.

* Bir operatör olarak, kişiselleştirilmiş panolar oluşturun ve bunları ve varsayılan pano arasında geçiş yapabilirsiniz. Daha fazla bilgi için bkz. [oluştur ve kişisel panoları Yönet](howto-personalize-dashboard.md) nasıl yapılır makalesi.

## <a name="device-explorer"></a>Device Explorer

![Gezgin sayfası](media/overview-iot-central-tour/explorer.png)

Gezgini sayfa gösterir _cihazları_ göre gruplandırılmış uygulamanızda Azure IOT Central _cihaz şablonu_.

* Cihaz şablonu, uygulamanıza bağlanabilen bir cihaz türünü tanımlar. Daha fazla bilgi almak için bkz. [Azure IoT Central uygulamanızda yeni bir cihaz türü tanımlama](tutorial-define-device-type.md).
* Cihaz, uygulamanızdaki gerçek veya sanal bir cihazı temsil eder. Daha fazla bilgi almak için bkz. [Azure IoT Central uygulamanıza yeni bir cihaz ekleme](tutorial-add-device.md).

## <a name="device-sets"></a>Cihaz kümeleri

![Cihaz Kümeleri sayfası](media/overview-iot-central-tour/devicesets.png)

_Cihaz kümeleri_ sayfası, oluşturucu tarafından oluşturulan cihaz kümelerini gösterir. Cihaz kümesi, ilişkili cihazların bir koleksiyonudur. Oluşturucu, bir cihaz kümesine eklenen cihazları belirlemek üzere bir sorgu tanımlar. Uygulamanızdaki analizleri özelleştirirken cihaz kümelerini kullanırsınız. Daha fazla bilgi almak için [Azure IoT Central uygulamanızda cihaz kümelerini kullanma](howto-use-device-sets.md) makalesine bakın.

## <a name="analytics"></a>Analiz

![Analiz sayfası](media/overview-iot-central-tour/analytics.png)

Analiz sayfasında, uygulamanıza bağlı cihazların nasıl davrandığını anlamanıza yardımcı olan grafikler gösterilir. Operatör bu sayfayı kullanarak bağlı cihazlarla ilgili sorunları izler ve araştırır. Oluşturucu, bu sayfada gösterilen grafikleri tanımlayabilir. Daha fazla bilgi almak için [Azure IoT Central uygulamanız için özel analizler oluşturma](howto-use-device-sets.md) makalesine bakın.

## <a name="jobs"></a>İşler

![İşler sayfası](media/overview-iot-central-tour/jobs.png)

İşler sayfası toplu cihaz yönetimi işlemleri cihazlarınızda çalıştırmanızı sağlar. Oluşturucu, cihaz özelliklerini, ayarları ve komutları güncelleştirmek için bu sayfayı kullanır. Daha fazla bilgi edinmek için [İş çalıştırma](howto-run-a-job.md) makalesine bakın.

## <a name="device-templates"></a>Cihaz şablonları

![Cihaz şablonlarını](media/overview-iot-central-tour/templates.png)

Cihaz şablonları burada bir oluşturucu oluşturur ve uygulamanın cihaz şablonlarında yönetir sayfasıdır. Bir cihaz şablon gibi cihaz özelliklerini belirtir:

- Telemetri, durum ve olay ölçümleri.
- Ayarları ve özellikleri.
- Komutları.
- Olayları veya telemetri değerleri temel alan kuralları.

Daha fazla bilgi almak için [Azure IoT Central uygulamanızda yeni bir cihaz türü tanımlama](tutorial-define-device-type.md) öğreticisine bakın.

## <a name="continuous-data-export"></a>Verileri sürekli dışarı aktarma

![Sürekli veri dışarı aktarma sayfası](media/overview-iot-central-tour/export.png)

Sürekli veri dışarı aktarma Burada, uygulamadan telemetri gibi bir veri akışını nasıl bir yöneticinin tanımladığı sayfasıdır. Diğer hizmetler, dışarı aktarılan verileri saklamak veya analiz için kullanın. Daha fazla bilgi için bkz. [Azure IOT Central verilerinizi dışarı](howto-export-data.md) makalesi.

## <a name="administration"></a>Yönetim

![Yönetim sayfası](media/overview-iot-central-tour/administration.png)

Yönetim sayfasını yönetici kullanıcıları ve rolleri uygulamasında tanımlama ve kullanıcı arabirimini özelleştirme gibi kullanan araçlarına bağlantılar içerir. Daha fazla bilgi almak için [Azure IoT Central uygulamanızı yönetme](howto-administer.md) makalesine bakın.

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT Central’a genel bir bakış elde ettiğinize ve kullanıcı arabiriminin düzenini tanıdığınıza göre önerilen sıradaki adım, [Azure IoT Central uygulaması oluşturma](quick-deploy-iot-central.md) hızlı başlangıcının tamamlanmasıdır.