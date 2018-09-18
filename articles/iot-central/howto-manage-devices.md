---
title: Azure IOT Central uygulamanızdaki cihazları yönetme | Microsoft Docs
description: Bir operatör olarak, Azure IOT Central uygulamanızdaki cihazların nasıl yönetileceğini öğrenin.
author: ellenfosborne
ms.author: elfarber
ms.date: 01/21/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 4b4ded86075e49277bca84f5261b6762b0f4fcae
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2018
ms.locfileid: "45737320"
---
# <a name="manage-devices-in-your-azure-iot-central-application"></a>Azure IOT Central uygulamanızdaki cihazları yönetme

Bu makalede, Microsoft Azure IOT Central uygulamanızdaki cihazları yönetmek için bir işleç olarak nasıl. Bir operatör olarak, şunları yapabilirsiniz:

- Kullanım **Gezgini** sayfa görüntüleme, ekleme ve Azure IOT Central uygulamanıza bağlı cihazları silin.
- Cihazlarınızı güncel envanterini korur.
- Cihaz meta verilerinizi güncel cihaz özelliklerinde depolanan değerleri değiştirerek tutun.
- Belirli bir CİHAZDAN bir ayar güncelleştirerek cihazlarınızı davranışını denetleyen **ayarları** sayfası.

## <a name="view-your-devices"></a>Cihazlarınızı görüntüleme

Tek bir cihaza görüntülemek için:

1. Seçin **Gezgini** sol gezinti menüsünde. Burada, bir listesini görürsünüz, [cihaz şablonları](howto-set-up-template.md).

1. Seçin bir **cihaz şablonu** soldaki bölmede.

1. Sağ bölmede, bu cihaz şablondan oluşturulmuş cihazların bir listesini görürsünüz. Görmek için tek bir cihaz seçin **cihaz ayrıntıları** sayfası bu cihaz için:

    [![Cihaz ayrıntıları sayfasına](./media/howto-manage-devices/image1.png)](./media/howto-manage-devices/image1.png#lightbox)

## <a name="add-a-device"></a>Cihaz ekleme

Azure IOT Central uygulamanız için bir cihaz eklemek için:

1. Seçin **Gezgini** sol gezinti menüsünde.

1. Bir cihaz oluşturmak istediğiniz cihaz şablonu seçin.

1. Seçin + **yeni**.

1. Seçin **gerçek** veya **benzetimli**. Azure IOT Central uygulamanızı bağlamak için fiziksel bir cihaz gerçek bir cihaz içindir. Örnek veriler sizin için Azure IOT Central tarafından oluşturulan bir simülasyon cihazı vardır. Bu örnekte gerçek bir cihaz. Seçin **gerçek** gitmek için **cihaz ayrıntıları** yeni cihazınız için sayfa.


## <a name="import-devices"></a>Cihazları İçeri Aktar

Uygulamanızı, Azure IOT Central sayıda cihaza bağlanmak için bir CSV dosyası içeri aktarılırken aygıtına teklifler toplu. CSV dosyası aşağıdaki sütunları (ve üst bilgiler) sahip olmalıdır
1.  IOTC_DeviceID  **<span style="color:Red">(küçük harf olması gerekir)</span>**
1.  IOTC_DeviceName (isteğe bağlı)


Toplu kaydı uygulamanızdaki cihazlar için:

1. Seçin **Gezgini** sol gezinti menüsünde.

1. Sol panelde, toplu olarak istediğiniz cihaz şablonu oluşturma cihazları seçin.

 >   [!NOTE] 
    Bir cihaz şablonuna sahip olmayan henüz altında cihazları içeri aktarabilirsiniz **ilişkili değil, cihazları** ve bunları herhangi bir şablonu kaydedin. Cihazları içe aktardıktan sonra sonra bunları bir şablonu ile bir sonraki adım olarak ilişkilendirebilirsiniz.

1. **İçeri Aktar**’a tıklayın.

    [![Alma eylemi](./media/howto-manage-devices/BulkImport1.png)](./media/howto-manage-devices/BulkImport1.png#lightbox)

1. İçeri aktarılacak cihaz kimlikleri listesi olan CSV dosyasını seçin.

1. Dosya yüklendikten sonra aygıt alma başlar. İçeri aktarma durumu üst cihaz kılavuzunun takip edebilirsiniz.

1. İçeri aktarma işlemi tamamlandıktan sonra cihaz Kılavuzu'nun bir başarı iletisi gösterilir.

    [![İçeri aktarma başarılı](./media/howto-manage-devices/BulkImport3.png)](./media/howto-manage-devices/BulkImport3.png#lightbox)

Aygıt alma işlemi başarısız olursa cihaz Kılavuzu'nun bir hata iletisi görürsünüz. Tüm hataları yakalamaya bir günlük dosyası oluşturulur ve hata iletisini tıklayarak indirilebilir.


**Cihazlar bir şablon ile ilişkilendirme**

Cihazları içeri aktarma altında başlatarak kaydederseniz **ilişkili değil, cihazları**, cihazları cihaz şablonu ilişkilere oluşturulmuştur. Cihaz verileri ve cihaz hakkında diğer ayrıntıları keşfetmek için bir şablonu ile ilişkili olmalıdır. Cihazlar bir şablon ile ilişkilendirmek için aşağıdaki adımları izleyin:
1. Seçin **Gezgini** sol gezinti menüsünde.
1. Sol panelde seçin **ilişkili değil, cihazları**.
    [![İlişkilendirilmemiş cihazlar](./media/howto-manage-devices/UnassociatedDevices1.png)](./media/howto-manage-devices/UnassociatedDevices1.png#lightbox)
1. Şablon ile ilişkilendirmek istediğiniz cihazları seçin.
1. Tıklayın **ilişkilendirmek** seçeneği.
    [![Cihaz ilişkilendirme](./media/howto-manage-devices/UnassociatedDevices2.png)](./media/howto-manage-devices/UnassociatedDevices2.png#lightbox)
1. Şablonu kullanılabilir şablonlar listesinden seçim yapın ve tıklayın **ilişkilendirmek** düğmesi.
1. Seçili cihazların ilgili cihaz taslağı altından taşınır.

 >   [!NOTE] 
    Bir cihaz onun ilişkili değil veya farklı bir şablonla ilişkili bir şablon ile ilişkilendirildikten sonra.

## <a name="export-devices"></a>Cihazlar dışarı aktarma

IOT Central bağlanmak için cihazları sağlamak için IOT Central tarafından üretilen cihaz bağlantı dizesi gerekir. Bağlantı dizelerini ve diğer özellikleri cihazların toplu olarak uygulamanızdan almak için dışarı aktarma özelliğini kullanabilirsiniz. Dışarı aktarma, tüm seçili cihazlar için bir CSV dosyası cihaz kimliği, cihaz adı ve birincil bağlantı dizesi oluşturur.

Uygulamanızdan dışarı aktarma cihazları toplu olarak:
1. Seçin **Gezgini** sol gezinti menüsünde.

1. Sol panelde, cihazları vermek istediğiniz cihaz şablonu seçin.

1. Dışarı aktarma ve ardından istediğiniz cihazları seçin **dışarı** eylem.

    [![Dışarı aktarma](./media/howto-manage-devices/Export1.png)](./media/howto-manage-devices/Export1.png#lightbox)

1. Dışarı aktarma işlemini başlatacak ve üst kılavuza durumunu izleyebilirsiniz. 

1. Dışarı aktarma işlemi tamamlandıktan sonra bir başarı iletisi oluşturulan dosyasını indirmek için bir bağlantı ile birlikte gösterilir.

1. Tıklayarak **başarılı iletisi** disk üzerindeki yerel bir klasöre dosya indirilemedi.

    [![Dışarı aktarma başarılı](./media/howto-manage-devices/Export2.png)](./media/howto-manage-devices/Export2.png#lightbox)

1. Dışarı aktarılan CSV dosyasını aşağıdaki sütunları bilgilere sahip olacağı: **cihaz kimliği, cihaz adı, cihaz Priamry/ikincil anahtarları ve birincil/ikincil sertifika thumbrpints**
    *   IOTC_DEVICEID
    *   IOTC_DEVICENAME
    *   IOTC_SASKEY_PRIMARY
    *   IOTC_SASKEY_SECONDARY
    *   IOTC_X509THUMBPRINT_PRIMARY 
    *   IOTC_X509THUMBPRINT_SECONDARY

## <a name="delete-a-device"></a>Bir cihazı silme

Azure IOT Central uygulamanızdan ya da bir gerçek veya sanal cihazı silmek için:

1. Seçin **Gezgini** Gezinti menüsünde.

1. Cihaz şablonu silmek istediğiniz cihazı seçin.

1. Silmek için cihazın yanındaki kutuyu işaretleyin.

1. Seçin **Sil**.

## <a name="change-a-device-setting"></a>Bir cihaz ayarını değiştirin

Ayarları bir cihaz davranışını denetler. Diğer bir deyişle, cihazınıza girişleri sağlıyor. Görüntüleyebilir ve cihaz ayarları güncelleştirme **cihaz ayrıntıları** sayfası.

1. Seçin **Gezgini** Gezinti menüsünde.

1. Cihaz şablonu ayarlarını değiştirmek istediğiniz cihazı seçin.

1. Seçin **ayarları** sekmesi. Burada, Cihazınızı sahip tüm ayarlar ve geçerli değerlerini görürsünüz. Her ayar için cihaz hala eşitleme görebilirsiniz.

1. İstenen kendi değerlerinize ayarlarını değiştirin. Aynı anda birden çok ayarları değiştirme ve tümünü aynı anda güncelleştirebilirsiniz.

1. Seçin **güncelleştirme**. Değerler, cihaza gönderilir. Cihaz ayar değişikliği bildirir, bir ayar durumu geri gider **eşitlenen**.

## <a name="change-a-property"></a>Bir özelliği değiştirmek

Şehir ve seri numarası gibi cihaz ile ilişkili cihaz meta verilerini özelliklerdir. Görüntüleyin ve güncelleştirme özellikleri **cihaz ayrıntıları** sayfası.

1. Seçin **Gezgini** Gezinti menüsünde.

1. Cihaz şablonu özelliklerini değiştirmek istediğiniz cihazı seçin.

1. Seçin **özellikleri** sekmesi, gördüğünüz tüm özellikleri.

1. İstenen kendi değerlerinize özelliklerini değiştirin. Aynı anda birden çok özellik değiştirme ve tümünü aynı anda güncelleştirebilirsiniz.

1. Seçin **güncelleştirme**.

> [!NOTE]
> Değerini değiştiremezsiniz _cihaz özelliklerini_. Cihaz özellikleri cihaz tarafından belirlenir ve Azure IOT Central uygulamada salt okunurdur.

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT Central uygulamanızdaki cihazların nasıl yönetileceğini öğrendiniz, önerilen sonraki adım aşağıda verilmiştir:

> [!div class="nextstepaction"]
> [Cihaz kümeleri kullanma](howto-use-device-sets.md)

<!-- Next how-tos in the sequence -->
