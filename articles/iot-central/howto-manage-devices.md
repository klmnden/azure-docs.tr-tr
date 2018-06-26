---
title: Azure IOT merkezi uygulamanızda aygıtları yönetme | Microsoft Docs
description: Bir operatör olarak, Azure IOT merkezi uygulamanızda cihazların nasıl yönetileceğini öğrenin.
author: ellenfosborne
ms.author: elfarber
ms.date: 01/21/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: cf803c03d266f2a400e47fc551dea62936456177
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36937627"
---
# <a name="manage-devices-in-your-azure-iot-central-application"></a>Azure IOT merkezi uygulamanızı aygıtları yönetme

Bu makalede, Microsoft Azure IOT merkezi uygulamanızda cihazları yönetmek için bir işleç olarak nasıl. Bir operatör olarak, şunları yapabilirsiniz:

- Kullanım **Explorer** sayfasını görüntülemek, eklemek ve Azure IOT merkezi uygulamanıza bağlı olan aygıtları silmek için.
- Aygıtlarınızı güncel bir stoğunu koruyun.
- Cihaz meta verilerini aygıt özelliklerinde saklanan değerleri değiştirerek güncelliğini.
- Belirli bir CİHAZDAN bir ayar güncelleştirerek aygıtlarınızı davranışını denetleyen **ayarları** sayfası.

## <a name="view-your-devices"></a>Aygıtlarınızı görüntüleyin

Tek bir cihaza görüntülemek için:

1. Seçin **Explorer** sol gezinti menüsünde. Burada bir listesini görürsünüz, [aygıt şablonları](howto-set-up-template.md).

1. Seçin bir **cihaz şablonu** sol bölmedeki.

1. Sağ taraftaki bölmede, bu cihaz şablondan oluşturulmuş cihazların bir listesini görürsünüz. Görmek için tek bir cihaza seçin **cihaz ayrıntıları** sayfa bu cihaz için:

    [![Cihaz Ayrıntıları sayfası](./media/howto-manage-devices/image1.png)](./media/howto-manage-devices/image1.png#lightbox)

## <a name="add-a-device"></a>Cihaz ekleme

Azure IOT merkezi uygulamanıza bir cihaz eklemek için:

1. Seçin **Explorer** sol gezinti menüsünde.

1. Bir cihaz oluşturmak istediğiniz cihaz şablonu seçin.

1. Seçin + **yeni**.

1. Seçin **gerçek** veya **benzetimli**. Azure IOT merkezi uygulamanıza bağlanan bir fiziksel aygıt gerçek bir cihazı içindir. Bir sanal cihaz sizin için Azure IOT Merkezi tarafından oluşturulan örnek veriler var. Bu örnek, gerçek cihaz kullanır. Seçin **gerçek** gitmek için **cihaz ayrıntıları** yeni cihazınız için sayfa.


## <a name="import-devices"></a>Cihazları içeri aktarma

Uygulamanıza, Azure IOT merkezi sayıda cihazlar bağlanmak için bir CSV dosyası içe aktarma aygıtına teklifleri toplu. 

CSV dosyası gereksinimleri:
1. CSV dosyası cihaz kimliklerini içeren tek bir sütunu olmalıdır.

1. Dosya bir üstbilgi yok.


Toplu kayıt, uygulamanızda aygıtları için:

1. Seçin **Explorer** sol gezinti menüsünde.

1. Sol panelde, toplu istediğiniz cihaz şablonu oluşturma aygıtları seçin.

 >   [!NOTE] 
    Bir aygıt şablonuna sahip olmayan henüz aygıtlar'ın altında içeri aktarabilirsiniz **aygıtları ilişkili değil** ve herhangi bir şablonu kaydedin. Aygıtları içe aktardıktan sonra daha sonra bunları sahip bir şablon bir sonraki adım olarak ilişkilendirebilirsiniz.

1. **İçeri Aktar**’a tıklayın.

    [![Alma eylemi](./media/howto-manage-devices/BulkImport1.png)](./media/howto-manage-devices/BulkImport1.png#lightbox)

1. İçeri aktarılacak cihaz kimlikleri listesine sahip CSV dosyasını seçin.

1. Dosya karşıya sonra cihaz alma başlatır. Cihaz kılavuz üstündeki alma durumunu izleyebilirsiniz.

1. İçeri aktarma işlemi tamamlandıktan sonra cihaz kılavuzda bir başarı iletisi gösterilir.

    [![İçeri aktarma başarılı](./media/howto-manage-devices/BulkImport3.png)](./media/howto-manage-devices/BulkImport3.png#lightbox)

İşlemi başarısız aygıt içe aktarırsanız, cihaz ızgarasında hata iletisi görürsünüz. Tüm hataları yakalama bir günlük dosyası oluşturulur ve hata iletisi tıklayarak indirilebilir.


**Cihazlar bir şablon ile ilişkilendirme**

Al altında başlatarak aygıtları kaydederseniz **aygıtları ilişkili değil**, cihazları cihaz şablonu ilişkilere oluşturulur sonra. Aygıt verileri ve cihaz ile ilgili diğer ayrıntıları keşfetmek için bir şablon ile ilişkilendirilmiş olması gerekir. Cihazlar bir şablon ile ilişkilendirmek için aşağıdaki adımları izleyin:
1. Seçin **Explorer** sol gezinti menüsünde.
1. Sol panelde seçin **ilişkili değil aygıtlarını**.
    [![İlişkilendirilmemiş cihazları](./media/howto-manage-devices/UnassociatedDevices1.png)](./media/howto-manage-devices/UnassociatedDevices1.png#lightbox)
1. Bir şablon ile ilişkilendirmek istediğiniz aygıtları seçin.
1. Tıklatın **ilişkilendirmek** seçeneği.
    [![Aygıtları ilişkilendirme](./media/howto-manage-devices/UnassociatedDevices2.png)](./media/howto-manage-devices/UnassociatedDevices2.png#lightbox)
1. Şablonu kullanılabilir şablonlar listesinden seçin ve'ı tıklatın **ilişkilendirmek** düğmesi.
1. Seçili cihazların ilgili cihaz şablonu altında taşınır.

 >   [!NOTE] 
    Bir aygıt onun ilişkili değil veya farklı bir şablonla ilişkili bir şablon ile ilişkilendirildikten sonra.

## <a name="export-devices"></a>Dışarı aktarma cihazları

Aygıtların IOT Orta bağlanmasını sağlamak için IOT Orta tarafından oluşturulan cihaz bağlantı dizesi gerekir. Bağlantı dizeleri ve diğer özellikleri aygıtların uygulamanızdan toplu olarak almak için dışarı aktarma özelliğini kullanabilirsiniz. Dışa aktarma bir CSV dosyası seçilen tüm aygıtlar için aygıt kimlik, cihaz adı ve birincil bağlantı dizesi oluşturur.

Uygulamanızdan verme cihazları toplu olarak:
1. Seçin **Explorer** sol gezinti menüsünde.

1. Sol panelde aygıtları vermek istediğiniz aygıt şablonunu seçin.

1. Dışarı aktarmak ve ardından istediğiniz aygıtları seçin **verme** eylem.

    [![Dışarı aktarma](./media/howto-manage-devices/Export1.png)](./media/howto-manage-devices/Export1.png#lightbox)

1. Dışa aktarma işlemi başlatılır ve kılavuz üstündeki durumunu izleyebilirsiniz. 

1. Dışa aktarma işlemi tamamlandıktan sonra bir başarı iletisi oluşturulan dosyasını karşıdan yüklemek için bir bağlantı ile birlikte gösterilir.

1. Tıklayın **başarı iletisi** disk üzerindeki yerel bir klasöre dosya indirilemedi.

    [![Dışarı aktarma başarılı](./media/howto-manage-devices/Export2.png)](./media/howto-manage-devices/Export2.png#lightbox)

1. Dışarı aktarılan CSV dosyası aşağıdaki bilgilere sahip:
    1. Ad
    1. Cihaz Kimliği
    1. Birincil bağlantı dizesi


## <a name="delete-a-device"></a>Bir aygıtı silme

Azure IOT merkezi uygulamanızdan ya da gerçek veya sanal cihazı silmek için:

1. Seçin **Explorer** Gezinti menüsünde.

1. Cihaz şablonu silmek istediğiniz cihazı seçin.

1. Silme için aygıt yanındaki kutuyu işaretleyin.

1. Seçin **silmek**.

## <a name="change-a-device-setting"></a>Bir aygıt ayarı değiştirme

Ayarları bir cihaz davranışını denetler. Diğer bir deyişle, bunlar aygıtınıza girişleri yapın olanak sağlar. Görüntüleyebilir ve aygıt ayarlarını güncelleştirmede **cihaz ayrıntıları** sayfası.

1. Seçin **Explorer** Gezinti menüsünde.

1. Cihaz şablon ayarlarını değiştirmek istediğiniz cihazı seçin.

1. Seçin **ayarları** sekmesi. Burada, Cihazınızı sahip tüm ayarlar ve geçerli değerlerini görürsünüz. Her ayar için cihaz yine eşitleniyor görebilirsiniz.

1. İstenen değerleri için ayarları değiştirin. Aynı anda birden çok ayarlarını değiştirin ve bunları tümü aynı anda güncelleştirin.

1. Seçin **güncelleştirme**. Değerleri aygıtınıza gönderilir. Aygıt ayar değişikliğini kabul ettikten sonra ayarın durumu geri gider **eşitlenen**.

## <a name="change-a-property"></a>Bir özellik değiştirme

Şehir ve seri numarası gibi cihaz ile ilişkili aygıt meta verileri özelliklerdir. Görüntüleyin ve güncelleştirme özellikleri **cihaz ayrıntıları** sayfası.

1. Seçin **Explorer** Gezinti menüsünde.

1. Özelliklerini değiştirmek istediğiniz aygıtın aygıt şablonunu seçin.

1. Seçin **özellikleri** sekmesi, tüm özellikler gördüğünüz.

1. İstenen değerleri için özelliklerini değiştirin. Aynı anda birden çok özelliklerini değiştirin ve bunları tüm aynı anda güncelleştirin.

1. Seçin **güncelleştirme**.

> [!NOTE]
> Değeri değiştiremezsiniz _cihaz özellikleri_. Cihaz özellikleri aygıt tarafından ayarlanır ve Azure IOT merkezi uygulamasında salt okunurdur.

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT merkezi uygulamanızda cihazların nasıl yönetileceğini öğrendiniz, önerilen sonraki adım aşağıda verilmiştir:

> [!div class="nextstepaction"]
> [Cihaz kümelerini kullanma](howto-use-device-sets.md)

<!-- Next how-tos in the sequence -->
