---
title: Azure IOT Central uygulamanızdaki cihazları yönetme | Microsoft Docs
description: Bir operatör olarak, Azure IOT Central uygulamanızdaki cihazların nasıl yönetileceğini öğrenin.
author: ellenfosborne
ms.author: elfarber
ms.date: 01/30/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: e4746620f083996bf64e77617ec472c3d3894d91
ms.sourcegitcommit: e6d53649bfb37d01335b6bcfb9de88ac50af23bd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65464271"
---
# <a name="manage-devices-in-your-azure-iot-central-application"></a>Azure IOT Central uygulamanızdaki cihazları yönetme

Bu makalede, Azure IOT Central uygulamanızdaki cihazları yönetmek için bir işleç olarak nasıl. Bir operatör olarak, şunları yapabilirsiniz:

- Kullanım **Device Explorer** sayfa görüntüleme, ekleme ve Azure IOT Central uygulamanıza bağlı cihazları silin.
- Cihazlarınızı güncel envanterini korur.
- Cihaz meta verilerinizi güncel cihaz özelliklerinde depolanan değerleri değiştirerek tutun.
- Belirli bir CİHAZDAN bir ayar güncelleştirerek cihazlarınızı davranışını denetleyen **ayarları** sayfası.

## <a name="view-your-devices"></a>Cihazlarınızı görüntüleme

Tek bir cihaza görüntülemek için:

1. Seçin **Device Explorer** sol gezinti menüsünde. Burada, bir listesini görürsünüz, [cihaz şablonları](howto-set-up-template.md).

1. Bir cihaz şablonunda seçin **şablonları** listesi.

1. Sağ bölmede **Device Explorer** sayfasında, cihaz şablondan oluşturulan cihazların bir listesini görürsünüz. Bu cihaz için cihaz ayrıntıları sayfasına görmek için tek bir cihaz seçin:

    ![Cihaz ayrıntıları sayfasına](./media/howto-manage-devices/devicelist.png)

## <a name="add-a-device"></a>Cihaz ekleme

Azure IOT Central uygulamanız için bir cihaz eklemek için:

1. Seçin **Device Explorer** sol gezinti menüsünde.

1. Bir cihaz oluşturmak istediğiniz cihaz şablonu seçin.

1. Seçin + **yeni**.

1. Seçin **gerçek** veya **benzetimli**. Azure IOT Central uygulamanızı bağlamak için fiziksel bir cihaz gerçek bir cihaz içindir. Örnek veriler sizin için Azure IOT Central tarafından oluşturulan bir simülasyon cihazı vardır.

## <a name="import-devices"></a>Cihazları İçeri Aktar

Çok sayıda cihaz uygulamanızı bağlamak için bir CSV dosyasından içeri aktarma cihazları topluca ekleyebilirsiniz. CSV dosyası aşağıdaki sütunları ve üst bilgileri sahip olmalıdır:

* **IOTC_DeviceID** -cihaz kimliği tamamı küçük harf olması gerekir.
* **IOTC_DeviceName** -Bu isteğe bağlı bir sütundur.

Toplu kaydı uygulamanızdaki cihazlar için:

1. Seçin **Device Explorer** sol gezinti menüsünde.

1. Sol panelde, toplu olarak istediğiniz cihaz şablonu oluşturma cihazları seçin.

    > [!NOTE]
    > Bir cihaz şablonuna sahip olmayan henüz altında cihazları içeri aktarabilirsiniz **ilişkili değil, cihazları** ve bunları bir şablonu kaydedin. Cihazlar içeri aktardıktan sonra bunları daha sonra şablon ile ilişkilendirebilirsiniz.

1. Seçin **alma**.

    ![Alma eylemi](./media/howto-manage-devices/bulkimport1a.png)

1. İçeri aktarılacak cihaz kimlikleri listesi olan CSV dosyasını seçin.

1. Dosya yüklendikten sonra aygıt alma başlar. İçeri aktarma durumu üst cihaz kılavuzunun takip edebilirsiniz.

1. İçeri aktarma işlemi tamamlandıktan sonra cihaz Kılavuzu'nun bir başarı iletisi gösterilir.

    ![İçeri aktarma başarılı](./media/howto-manage-devices/bulkimport3a.png)

Aygıt alma işlemi başarısız olursa cihaz Kılavuzu'nun bir hata iletisi görürsünüz. İndirebilirsiniz, tüm hataları yakalamaya bir günlük dosyası oluşturulur.

**Cihazlar bir şablon ile ilişkilendirme**

Cihazları içeri aktarma altında başlatarak kaydederseniz **ilişkili değil, cihazları**, cihazları cihaz şablonu ilişkilere oluşturulmuştur. Cihazlar, veriler ve cihaz hakkında diğer ayrıntıları keşfetmek için bir şablonu ile ilişkili olmalıdır. Cihazlar bir şablon ile ilişkilendirmek için aşağıdaki adımları izleyin:

1. Seçin **Device Explorer** sol gezinti menüsünde.

1. Sol panelde seçin **ilişkili değil, cihazları**:

    ![İlişkilendirilmemiş cihazlar](./media/howto-manage-devices/unassociateddevices1a.png)

1. Şablon ile ilişkilendirmek istediğiniz cihazları seçin:

1. Seçin **ilişkilendirmek**:

    ![Cihazları İlişkilendir](./media/howto-manage-devices/unassociateddevices2a.png)

1. Şablonu kullanılabilir şablonlar listesinden seçip **ilişkilendirmek**.

1. Seçili cihazların, seçtiğiniz cihaz şablonuyla ilişkilendirilir.

> [!NOTE]
> Bir cihaz bir şablon ile ilişkilendirildikten sonra ilişkili değil veya farklı bir şablonla ilişkili.

## <a name="export-devices"></a>Cihazlar dışarı aktarma

Gerçek bir cihaz IOT Central bağlanmak için bağlantı dizesi gerekir. Cihaz ayrıntıları toplu cihaz bağlantı dizesi oluşturmak ihtiyacınız olan bilgileri almak için dışarı aktarabilirsiniz. Dışarı aktarma işlemi, cihaz kimliği, cihaz adını ve anahtarlarını tüm seçili cihazlar için bir CSV dosyası oluşturur.

Uygulamanızdan dışarı aktarma cihazları toplu olarak:

1. Seçin **Device Explorer** sol gezinti menüsünde.

1. Sol panelde, cihazları vermek istediğiniz cihaz şablonu seçin.

1. Dışarı aktarma ve ardından istediğiniz cihazları seçin **dışarı** eylem.

    ![Dışarı Aktarma](./media/howto-manage-devices/export1a.png)

1. Dışarı aktarma işlemini başlatır. Izgaranın üst durumunu izleyebilirsiniz.

1. Dışarı aktarma tamamlandığında, bir başarı iletisi oluşturulan dosyasını indirmek için bir bağlantı ile birlikte gösterilir.

1. Seçin **başarılı iletisi** disk üzerindeki yerel bir klasöre dosya indirilemedi.

    ![Dışarı aktarma başarılı](./media/howto-manage-devices/export2a.png)

1. Dışarı aktarılan CSV dosyasını aşağıdaki sütunları içerir: cihaz kimliği, cihaz adı, cihaz anahtarları ve X509 sertifika parmak izleri:

    * IOTC_DEVICEID
    * IOTC_DEVICENAME
    * IOTC_SASKEY_PRIMARY
    * IOTC_SASKEY_SECONDARY
    * IOTC_X509THUMBPRINT_PRIMARY
    * IOTC_X509THUMBPRINT_SECONDARY

Bkz: [Azure IOT Central, cihaz bağlantısı](concepts-connectivity.md), bağlantı dizeleri ve IOT Central uygulamanıza bağlanan gerçek cihazlar hakkında daha fazla bilgi için.

## <a name="delete-a-device"></a>Bir cihazı silme

Azure IOT Central uygulamanızdan ya da bir gerçek veya sanal cihazı silmek için:

1. Seçin **Device Explorer** Gezinti menüsünde.

1. Cihaz şablonu silmek istediğiniz cihazı seçin.

1. Silmek için cihazın yanındaki kutuyu işaretleyin.

1. Seçin **Sil**.

## <a name="change-a-device-setting"></a>Bir cihaz ayarını değiştirin

Ayarları bir cihaz davranışını denetler. Diğer bir deyişle, cihazınıza girişleri sağlıyor. Görüntüleyebilir ve cihaz ayarları güncelleştirme **cihaz ayrıntıları** sayfası.

1. Seçin **Device Explorer** Gezinti menüsünde.

1. Cihaz şablonu ayarlarını değiştirmek istediğiniz cihazı seçin.

1. Seçin **ayarları** sekmesi. Burada, Cihazınızı sahip tüm ayarlar ve geçerli değerlerini görürsünüz. Her ayar için cihaz hala eşitleme görebilirsiniz.

1. İhtiyacınız olan değerlere için ayarları değiştirin. Aynı anda birden çok ayarları değiştirme ve tümünü aynı anda güncelleştirebilirsiniz.

1. Seçin **güncelleştirme**. Değerler, cihaza gönderilir. Cihaz ayar değişikliğinin onaylarsa, bir ayar durumu geri gider **eşitlenen**.

## <a name="change-a-property"></a>Bir özelliği değiştirmek

Şehir ve seri numarası gibi cihaz ile ilişkili cihaz meta verilerini özelliklerdir. Görüntüleyin ve güncelleştirme özellikleri **cihaz ayrıntıları** sayfası.

1. Seçin **Device Explorer** Gezinti menüsünde.

1. Cihaz şablonu özelliklerini değiştirmek istediğiniz cihazı seçin.

1. Seçin **özellikleri** sekmesi, gördüğünüz tüm özellikleri.

1. İhtiyacınız olan değerlere için uygulama özelliklerini değiştirin. Aynı anda birden çok özellik değiştirme ve tümünü aynı anda güncelleştirebilirsiniz. Seçin **güncelleştirme**.

> [!NOTE]
> Değerini değiştiremezsiniz _cihaz özelliklerini_. Cihaz özellikleri cihaz tarafından belirlenir ve Azure IOT Central uygulamada salt okunurdur.

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT Central uygulamanızdaki cihazların nasıl yönetileceğini öğrendiniz, önerilen sonraki adım aşağıda verilmiştir:

> [!div class="nextstepaction"]
> [Cihaz kümeleri kullanma](howto-use-device-sets.md)

<!-- Next how-tos in the sequence -->
