---
title: Oluşturma ve Azure IOT Central, uygulamanızda telemetri kurallarını yönetin | Microsoft Docs
description: Azure IOT Central telemetri kuralları cihazlarınızı neredeyse gerçek zamanlı izleme ve otomatik olarak kural tetiklendiğinde, bir e-posta gönderme gibi eylemleri çağırmak için etkinleştirin.
services: iot-central
author: tanmaybhagwat
ms.author: tanmayb
ms.date: 04/16/2018
ms.topic: article
ms.prod: microsoft-iot-central
manager: timlt
ms.openlocfilehash: 42516e4dd6a85e0d07d4a8e70e958b2ec6e84aad
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2018
ms.locfileid: "39225209"
---
# <a name="create-a-telemetry-rule-and-set-up-an-action-in-your-azure-iot-central-application"></a>Bir telemetri kuralı oluşturabilir ve Azure IOT Central, uygulamanızda bir eylem ayarlama

Bağlı cihazlarınızın uzaktan izlemek için Microsoft Azure IOT Central kullanabilirsiniz. Azure IOT Central kuralları cihazlarınızı neredeyse gerçek zamanlı izleme ve otomatik olarak bir e-posta gönderme veya kural koşulları karşılandığında, Microsoft Flow, iş akışı tetikleyen gibi eylemleri çağırmak için etkinleştirin. Yalnızca birkaç tıklamayla, cihaz verilerinizi izleyin ve çağrılacak eylem yapılandırmak için koşullar tanımlayabilirsiniz. Bu makalede ayrıntılı telemetri kural açıklanmaktadır.

Azure IOT Central'ı kullanan [telemetri ölçümleri](howto-set-up-template.md) cihaz verilerini yakalamak için. Her ölçüm türü ölçüm tanımlayan anahtar özniteliklere sahip. Her cihaz ölçüm türü izlemek ve kural tetiklendiğinde uyarılar oluşturmak için kurallar oluşturabilirsiniz. Seçili cihaz telemetrisi belirtilen eşiği aştığında bir telemetri kural tetikler.

## <a name="create-a-telemetry-rule"></a>Telemetri kuralı oluşturma

Bu bölümde, bir telemetri kuralının nasıl oluşturulacağını gösterir. Bu örnek, sıcaklık ve nem telemetrisini gönderen bir bağlı bir klima cihaz kullanır. Kural, cihaz tarafından bildirilen sıcaklık izler ve 80 derecenin üzerinde olduğunda bir e-posta gönderir.

1. Kurala eklemekte olduğunuz cihaz için cihaz ayrıntıları sayfasına gidin.

1. Herhangi bir kural henüz oluşturmadıysanız, aşağıdaki ekranı görürsünüz:

    ![Henüz hiçbir kural](media/howto-create-telemetry-rules/image1.png)

1. Üzerinde **kuralları** sekmesini, **+ yeni kural** kuralları oluşturmak için kullanabileceğiniz türlerini görmek için.

    ![Kural türü](media/howto-create-telemetry-rules/image2.png)

1. Seçin **Telemetri** kutucuğunu kuralı oluşturmak için formu açın.

    ![Telemetri kuralı](media/howto-create-telemetry-rules/image3.png)

1. Bu cihaz şablonu kuralında tanımlamanıza yardımcı olacak bir ad seçin.

1. Kural bu şablondan oluşturulan tüm cihazlar için hemen etkinleştirmek için geçiş **etkinleştirme kuralı**.

### <a name="configure-the-rule-condition"></a>Kural koşulu yapılandırma

Bu bölümde, sıcaklık telemetri izlemek için bir koşul ekleme gösterir.

1. Seçin **+** yanındaki **koşul**.

1. Seçin **sıcaklık** açılır listeden telemetri türü. Ardından işleci seçin ve bir eşik değeri sağlayın. Birden fazla telemetri koşullar ekleyebilirsiniz. Birden çok koşulu belirtildiğinde, kural tetiklemek tüm koşulların karşılanması gerekir.

    ![Telemetri koşul Ekle](media/howto-create-telemetry-rules/image4.png)

    > [!NOTE]
    > Telemetri Kural koşulu tanımlarken en az bir telemetri ölçümü seçin.

1. Tıklayın **Kaydet** , kuralını kaydetmek için. Kural, birkaç dakika içinde etkin hale gelir ve uygulamanıza gönderilen telemetri izlemeye başlar.

### <a name="add-an-action"></a>Eylem ekleme

Bu örnek, bir kural için bir eylem eklemek nasıl gösterir. Bu e-posta eylemi ekleme işlemi gösterilmektedir, ancak diğer eylemler de ekleyebilirsiniz:
-  [Microsoft Flow eylem](howto-add-microsoft-flow.md) için Microsoft Flow bir iş akışında bir kuralı tetiklendiğinde kazandırın
- [Web kancası eylemi](howto-create-webhooks.md) bir kuralı tetiklendiğinde diğer hizmetleri bildirmek için

> [!NOTE]
> Yalnızca 1 eylem şu anda tek bir kural için ilişkili olabilir.

1. Seçin **+** yanındaki **eylemleri**. Burada, kullanılabilir eylemler listesini görürsünüz.

    ![Eylem Ekle](media/howto-create-telemetry-rules/image5.png)

1. Seçin **e-posta** eylemi, bir geçerli e-posta adresi girerek **için** alan ve kural tetiklendiğinde e-postanın gövdesinde görüntülenen bir not girin.

    > [!NOTE]
    > E-postaları, yalnızca uygulamaya eklenen ve en az bir kez oturum kullanıcılara gönderilir. Daha fazla bilgi edinin [kullanıcı yönetimi](howto-administer.md) Azure IOT Central içinde.

   ![Eylem yapılandırma](media/howto-create-telemetry-rules/image6.png)

1. **Kaydet**’e tıklayın. Kural, birkaç dakika içinde etkin hale gelir ve uygulamanıza gönderilen telemetri izlemeye başlar. Kuralda belirtilen koşul eşleştiğinde kural yapılandırılan e-posta eylemi tetikler.

## <a name="parameterize-the-rule"></a>Kural Parametreleştirme

Kuralları, gelen belirli değerlerinde türetilebilir **cihaz özelliklerini** parametre olarak. Parametreleri kullanarak, burada telemetri eşikleri farklı cihazlar için farklı senaryolarda yararlıdır. Kuralı oluşturduğunuzda, eşik gibi belirten bir cihaz özelliği seçin **Ideal en yüksek eşik**, 80 derece gibi mutlak bir değer sağlamak yerine. Kural yürütüldüğünde, cihaz telemetrisi cihaz özelliğinde sağlanan değer ile eşleşir.

Parametreleri kullanarak, cihaz şablonunu yönetmek için kuralların sayısını azaltmak için etkili bir yoludur.

Eylemleri kullanarak yapılandırılabilir **cihaz özelliği** bir parametre olarak. Bir cihaz özelliği depolanan bir e-posta adresi sonra tanımlarken kullanılabilir **için** adresi.

## <a name="delete-a-rule"></a>Kural silme

Bir kural artık ihtiyacınız kalmadığında, kural açarak ve silmek **Sil**. Kural siliniyor cihaz şablonunu ve ilişkili tüm cihazlardan kaldırır.

## <a name="enable-or-disable-a-rule-for-a-device-template"></a>Etkinleştirmek veya devre dışı bir cihaz şablonu için bir kural

Cihaza gidin ve etkinleştirme veya devre dışı bırakmak istediğiniz kuralı seçin. Geçiş **Bu şablon, tüm cihazlar için etkinleştirme kuralı** kural düğmesini etkinleştirir veya cihaz şablonuyla ilişkili tüm cihazlar için kuralı devre dışı bırakır.

## <a name="enable-or-disable-a-rule-for-a-device"></a>Etkinleştirmek veya devre dışı bir cihaz için bir kural

Cihaza gidin ve etkinleştirme veya devre dışı bırakmak istediğiniz kuralı seçin. İki durumlu **bu cihaz için etkinleştirme kuralı** düğmesini etkinleştirin veya bu cihaz için kuralı devre dışı bırak.

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT Central uygulamanızdaki kuralları düzenlemek öğrendiniz, önerilen sonraki adımlar şunlardır:

> [!div class="nextstepaction"]
> [Bir kural için bir Microsoft Flow eylemi ekleme](howto-add-microsoft-flow.md)
> [cihazlarınızı yönetme](howto-manage-devices.md)
