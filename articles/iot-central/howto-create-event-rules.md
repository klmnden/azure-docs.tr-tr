---
title: Oluşturma ve Azure IOT Central uygulamanızdaki olay kuralları yönetme | Microsoft Docs
description: Azure IOT Central olay kuralları cihazlarınızı neredeyse gerçek zamanlı izleme ve otomatik olarak kural tetiklendiğinde, bir e-posta gönderme gibi eylemleri çağırmak için etkinleştirin.
services: iot-central
author: ankitgupta
ms.author: ankitgup
ms.date: 04/29/2018
ms.topic: article
ms.prod: microsoft-iot-central
manager: timlt
ms.openlocfilehash: ede7748b1471136cf792c2b30b7c90e12b0b274a
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39006857"
---
# <a name="create-an-event-rule-and-set-up-an-action-in-your-azure-iot-central-application"></a>Bir olay kuralı oluşturma ve Azure IOT Central, uygulamanızda bir eylem ayarlama

Bağlı cihazlarınızın uzaktan izlemek için Microsoft Azure IOT Central kullanabilirsiniz. Azure IOT Central kuralları cihazlarınızı neredeyse gerçek zamanlı izleme ve otomatik olarak bir e-posta gönderme veya kural koşulları karşılandığında, Microsoft Flow, iş akışı tetikleyen gibi eylemleri çağırmak için etkinleştirin. Yalnızca birkaç tıklamayla, cihaz verilerinizi izleyin ve çağrılacak eylem yapılandırmak için koşul tanımlayabilirsiniz. Bu makalede, olay izleme kuralı ayrıntılı açıklanmaktadır.

Azure IOT Central'ı kullanan [olay ölçümü](howto-set-up-template.md) cihaz verilerini yakalamak için. Her ölçüm türü ölçüm tanımlayan anahtar özniteliklere sahip. Her cihaz ölçüm türü izlemek ve kural tetiklendiğinde uyarılar oluşturmak için kurallar oluşturabilirsiniz. Seçili cihaz olayı, cihaz tarafından bildirilen bir olayı kuralı tetikler.

## <a name="create-an-event-rule"></a>Olay kuralı oluşturma

Bu bölüm bir olay kuralının nasıl oluşturulacağını gösterir. Bu örnek, bir refrigerated satış makine cihaz raporları fan motor hata olayı kullanır. Kural, cihaz tarafından bildirilen olay izler ve olay bildirilen her bir e-posta gönderir.

1. Kurala eklemekte olduğunuz cihaz için cihaz ayrıntıları sayfasına gidin.

1. Herhangi bir kural henüz oluşturmadıysanız, aşağıdaki ekranı görürsünüz:

    ![Henüz hiçbir kural](media/howto-create-event-rules/image1.png)

1. Üzerinde **kuralları** sekmesini, **+ yeni kural** kuralları oluşturmak için kullanabileceğiniz türlerini görmek için.

    ![Kural türü](media/howto-create-event-rules/image2.png)

1. Tıklayarak **olay** kuralı oluşturmak için formu açın.

    ![Olayı kuralı](media/howto-create-event-rules/image3.png)

1. Bu cihaz şablonu kuralında tanımlamanıza yardımcı olacak bir ad seçin.

1. Kural bu şablondan oluşturulan tüm cihazlar için hemen etkinleştirmek için geçiş **etkinleştirme kuralı**.

### <a name="configure-the-rule-condition"></a>Kural koşulu yapılandırma

Bu bölümde, Fan motor hata olay ölçümü izlemek için bir koşul ekleme işlemini göstermektedir.

1. Seçin **+** yanındaki **koşul**.

1. Olay ölçümü izlemek istiyorsanız açılan listeden seçin. Bu örnekte **Fan Motor hata** olay seçildi.

1. İsteğe bağlı olarak belirli bir cihaz tarafından bildirilen olay değerini izlemek istediğiniz durumunda bir değer de sağlayabilirsiniz. Cihaz ardından kuralın koşuluyla değer olarak hata kodu sağlayan farklı hata kodlarıyla aynı olay bildirirse yalnızca cihaz olay yükü bu özel değer gönderdiğinde örneği için kural tetiklendiğinde emin olur. Bu alanı boş bırakın, CİHAZDAN gönderilen olay Olay değerinden bağımsız olarak her kural tetikleyecek anlamına gelir.

    ![Olay koşulu Ekle](media/howto-create-event-rules/image4.png)

    > [!NOTE]
    > Bir olay Kural koşulu tanımlarken en az bir olay ölçümü seçmeniz gerekir.

1. Tıklayın **Kaydet** , kuralını kaydetmek için. Kural, birkaç dakika içinde etkin hale gelir ve uygulamanıza gönderilen olaylar izlemeye başlar.

### <a name="add-an-action"></a>Eylem ekleme

Bu bölümde, bir kural için bir eylem eklemek gösterilir. Bu e-posta eylemi ekleme işlemi gösterilmektedir, ancak ayrıca [Microsoft Flow Eylem Ekle](howto-add-microsoft-flow.md) için kuralınızın kuralı tetiklendiğinde Microsoft Flow bir iş akışında kazandırın.

> [!NOTE]
> Yalnızca 1 eylem şu anda tek bir kural için ilişkili olabilir.

1. Seçin **+** yanındaki **eylemleri**. Burada, kullanılabilir eylemler listesini görürsünüz.

    ![Eylem Ekle](media/howto-create-event-rules/image5.png)

1. Seçin **e-posta** eylemi, bir geçerli e-posta adresi girerek **için** alan ve kural tetiklendiğinde e-postanın gövdesinde görüntülenen bir not girin.

    > [!NOTE]
    > E-postaları, yalnızca uygulamaya eklenen ve en az bir kez oturum kullanıcılara gönderilir. Daha fazla bilgi edinin [kullanıcı yönetimi](howto-administer.md) Azure IOT Central içinde.

   ![Eylem yapılandırma](media/howto-create-event-rules/image6.png)

1. **Kaydet**’e tıklayın. Kural, birkaç dakika içinde etkin hale gelir ve uygulamanıza gönderilen olaylar izlemeye başlar. Kuralda belirtilen koşul eşleştiğinde kural yapılandırılan e-posta eylemi tetikler.

## <a name="parameterize-the-rule"></a>Kural Parametreleştirme

Eylemleri kullanarak yapılandırılabilir **cihaz özelliği** bir parametre olarak. Bir cihaz özelliği depolanan bir e-posta adresi sonra tanımlarken kullanılabilir **için** adresi.

## <a name="delete-a-rule"></a>Kural silme

Bir kural artık ihtiyacınız kalmadığında, kural açarak ve silmek **Sil**. Kural siliniyor cihaz şablonunu ve ilişkili tüm cihazlardan kaldırır.

## <a name="enable-or-disable-a-rule-for-a-device-template"></a>Etkinleştirmek veya devre dışı bir cihaz şablonu için bir kural

Cihaza gidin ve etkinleştirme veya devre dışı bırakmak istediğiniz kuralı seçin. Geçiş **Bu şablon, tüm cihazlar için etkinleştirme kuralı** kural düğmesini etkinleştirir veya cihaz şablonuyla ilişkili tüm cihazlar için kuralı devre dışı bırakır.

## <a name="enable-or-disable-a-rule-for-a-device"></a>Etkinleştirmek veya devre dışı bir cihaz için bir kural

Cihaza gidin ve etkinleştirme veya devre dışı bırakmak istediğiniz kuralı seçin. İki durumlu **bu cihaz için etkinleştirme kuralı** düğmesini etkinleştirin veya bu cihaz için kuralı devre dışı bırak.

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT Central uygulamanızda kuralları oluşturulacağını öğrendiniz, önerilen sonraki adım aşağıda verilmiştir:

> [!div class="nextstepaction"]
> [Bir kural için bir Microsoft Flow eylemi ekleme](howto-add-microsoft-flow.md)
> [cihazlarınızı yönetmek nasıl](howto-manage-devices.md).
