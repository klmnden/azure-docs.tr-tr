---
title: Azure IoT Central Kullanıcı Arabirimi turuna katılın | Microsoft Docs
description: Oluşturucu olarak, bir IoT çözümü oluşturmak için kullanabileceğiniz Azure IoT Central kullanıcı arabiriminin temel alanlarını tanıyın.
author: dominicbetts
ms.author: dobett
ms.date: 01/24/2019
ms.topic: overview
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: philmea
ms.openlocfilehash: a5457299d0a0112685b194887802b4808a21d983
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2019
ms.locfileid: "55666602"
---
# <a name="take-a-tour-of-the-azure-iot-central-ui-new-ui-design"></a>Azure IOT Central kullanıcı Arabirimi (yeni kullanıcı Arabirimi tasarımı) ilişkin tura katılın

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

[!INCLUDE [iot-central-experimental-note](../../includes/iot-central-experimental-note.md)]

## <a name="use-the-left-navigation-menu"></a>Sol gezinti menüsünü kullanma

Uygulamanın farklı alanlarına erişmek için sol gezinti menüsünü kullanın:

| Menü | Açıklama |
| ---- | ----------- |
| ![Sol gezinti menüsü](media/overview-iot-central-tour-experimental/navigationbar.png) | <ul><li>**Giriş** düğmesi, uygulamanızın giriş sayfasını gösterir. Oluşturucu olarak, bu giriş sayfasını operatörleriniz için özelleştirebilirsiniz.</li><li>**Device Explorer** düğmesi, uygulamadaki her bir cihaz şablonuyla ilişkili sanal ve gerçek cihazları listeler. Operatör olarak, **Device Explorer**’ı kullanarak bağlı cihazlarınızı yönetebilirsiniz.</li><li>**Cihaz Kümeleri** düğmesi, cihaz kümelerini görüntüleyip oluşturmanızı sağlar. Operatör olarak, bir sorgu tarafından belirtilen cihazların mantıksal bir koleksiyonu olarak cihaz kümeleri oluşturabilirsiniz.</li><li>**Analiz** düğmesi, cihazlar ve cihaz kümeleri için cihaz telemetrisinden türetilen analizleri gösterir. Operatör olarak, uygulamanızdan içgörüler türetmek için cihaz verileri üzerinde özel görünümler oluşturabilirsiniz.</li><li>**İşler** düğmesi, uygun ölçekte güncelleştirmeler gerçekleştirmek için iş oluşturmanız ve çalıştırmanız gerekmeden toplu cihaz yönetimi sağlar.</li><li>**Cihaz şablonları** düğmesini gösteren bir oluşturucu kullanan cihaz şablonları oluşturmak ve yönetmek için Araçlar.</li><li>**Verileri sürekli dışarı aktarma** depolama ve kuyruk gibi diğer Azure Hizmetleri için sürekli dışarı aktarma yapılandırmak için yönetici düğmesi.</li><li>**Yönetim** düğmesi, bir yöneticinin uygulama ayarlarını, kullanıcıları ve rolleri yönetebildiği uygulama yönetimi sayfalarını gösterir.</li></ul> |

## <a name="search-help-and-support"></a>Arama, yardım ve destek

Üstteki menü her sayfada görünür:

![Araç Çubuğu](media/overview-iot-central-tour-experimental/toolbar.png)

- Cihaz şablonlarını ve cihazları aramak için **Arama** simgesini seçin.
- Kullanıcı Arabirimi dilini değiştirmek için seçin **dil** simgesi.
- Yardım ve destek almak için, kaynak listesinden **Yardım** açılır listesini seçin.
- Kullanıcı Arabirimi temasını değiştirme veya uygulamadaki oturumunu oturum açmak için seçin **hesabı** simgesi.

Kullanıcı arabirimi için açık renkli tema veya koyu renkli temayı seçebilirsiniz:

![Kullanıcı arabirimi için tema seçme](media/overview-iot-central-tour-experimental/themes.png)

## <a name="home-page"></a>Giriş sayfası

![Giriş sayfası](media/overview-iot-central-tour-experimental/homepage.png)

Giriş sayfası, Azure IoT Central uygulamanızda oturum açtığınızda gördüğünüz birinci sayfadır. Oluşturucu olarak, uygulamaya kutucuklar ekleyerek uygulamanın diğer kullanıcıları için giriş sayfasını özelleştirebilirsiniz. Daha fazla bilgi almak için [Azure IoT Central operatör görünümünü özelleştirme](tutorial-customize-operator-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json) öğreticisine bakın.

## <a name="device-explorer"></a>Device explorer

![Gezgin sayfası](media/overview-iot-central-tour-experimental/explorer.png)

Gezgini sayfa gösterir _cihazları_ göre gruplandırılmış uygulamanızda Azure IOT Central _cihaz şablonu_.

* Cihaz şablonu, uygulamanıza bağlanabilen bir cihaz türünü tanımlar. Daha fazla bilgi almak için bkz. [Azure IoT Central uygulamanızda yeni bir cihaz türü tanımlama](tutorial-define-device-type-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json).
* Cihaz, uygulamanızdaki gerçek veya sanal bir cihazı temsil eder. Daha fazla bilgi almak için bkz. [Azure IoT Central uygulamanıza yeni bir cihaz ekleme](tutorial-add-device.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json).

## <a name="device-sets"></a>Cihaz kümeleri

![Cihaz Kümeleri sayfası](media/overview-iot-central-tour-experimental/devicesets.png)

_Cihaz kümeleri_ sayfası, oluşturucu tarafından oluşturulan cihaz kümelerini gösterir. Cihaz kümesi, ilişkili cihazların bir koleksiyonudur. Oluşturucu, bir cihaz kümesine eklenen cihazları belirlemek üzere bir sorgu tanımlar. Uygulamanızdaki analizleri özelleştirirken cihaz kümelerini kullanırsınız. Daha fazla bilgi almak için [Azure IoT Central uygulamanızda cihaz kümelerini kullanma](howto-use-device-sets.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json) makalesine bakın.

## <a name="analytics"></a>Analiz

![Analiz sayfası](media/overview-iot-central-tour-experimental/analytics.png)

Analiz sayfasında, uygulamanıza bağlı cihazların nasıl davrandığını anlamanıza yardımcı olan grafikler gösterilir. Operatör bu sayfayı kullanarak bağlı cihazlarla ilgili sorunları izler ve araştırır. Oluşturucu, bu sayfada gösterilen grafikleri tanımlayabilir. Daha fazla bilgi almak için [Azure IoT Central uygulamanız için özel analizler oluşturma](howto-create-analytics.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json) makalesine bakın.

## <a name="jobs"></a>İşler

![İşler sayfası](media/overview-iot-central-tour-experimental/jobs.png)

İşler sayfası, cihazlarınızda toplu cihaz yönetimi işlemleri gerçekleştirmenize olanak sağlar. Oluşturucu, cihaz özelliklerini, ayarları ve komutları güncelleştirmek için bu sayfayı kullanır. Daha fazla bilgi edinmek için [İş çalıştırma](howto-run-a-job.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json) makalesine bakın.

## <a name="device-templates"></a>Cihaz Şablonları

![Cihaz şablonlarını](media/overview-iot-central-tour-experimental/templates.png)

Cihaz şablonları burada bir oluşturucu oluşturur ve uygulamanın cihaz şablonlarında yönetir sayfasıdır. Daha fazla bilgi almak için [Azure IoT Central uygulamanızda yeni bir cihaz türü tanımlama](tutorial-define-device-type-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json) öğreticisine bakın.

## <a name="continuous-data-export"></a>Sürekli Veri Dışarı Aktarma

![Sürekli veri dışarı aktarma sayfası](media/overview-iot-central-tour-experimental/export.png)

Sürekli veri dışarı aktarma burada uygulamadan telemetri gibi verileri dışarı aktarma bir yöneticinin tanımladığı sayfasıdır. Diğer hizmetler, dışarı aktarılan verileri saklamak veya analiz için kullanın. Daha fazla bilgi için bkz. [Azure IOT Central verilerinizi dışarı](howto-export-data.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json) makalesi.

## <a name="administration"></a>Yönetim

![Yönetim sayfası](media/overview-iot-central-tour-experimental/administration.png)

Yönetim sayfası, uygulamadaki kullanıcıları ve rolleri tanımlama gibi bir yöneticinin kullandığı araçların bağlantılarını içerir. Daha fazla bilgi almak için [Azure IoT Central uygulamanızı yönetme](howto-administer.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json) makalesine bakın.

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT Central’a genel bir bakış elde ettiğinize ve kullanıcı arabiriminin düzenini tanıdığınıza göre önerilen sıradaki adım, [Azure IoT Central uygulaması oluşturma](quick-deploy-iot-central.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json) hızlı başlangıcının tamamlanmasıdır.