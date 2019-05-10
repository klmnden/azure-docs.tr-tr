---
title: Oluşturma ve Azure IOT Central uygulamanızdaki olay kuralları yönetme | Microsoft Docs
description: Azure IOT Central olay kuralları cihazlarınızı neredeyse gerçek zamanlı izleme ve otomatik olarak kural tetiklendiğinde, bir e-posta gönderme gibi eylemleri çağırmak için etkinleştirin.
author: ankitscribbles
ms.author: ankitgup
ms.date: 02/20/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: a2bce535d8612eca565970d4c530a27efb356334
ms.sourcegitcommit: e6d53649bfb37d01335b6bcfb9de88ac50af23bd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65464623"
---
# <a name="create-an-event-rule-and-set-up-notifications-in-your-azure-iot-central-application"></a>Bir olayı kuralı oluşturun ve Azure IOT Central uygulamanızdaki bildirimleri ayarlama

*Bu makale, işleçler, oluşturucular ve yöneticiler için geçerlidir.*

Bağlı cihazlarınızın uzaktan izlemek için Azure IOT Central kullanabilirsiniz. Azure IOT Central kuralları cihazlarınızı neredeyse gerçek zamanlı izleme ve otomatik eylemleri bir e-posta veya gibi Microsoft Flow tetiklemek etkinleştirin. Yalnızca birkaç tıklamayla, cihaz verilerinizi izleyin ve ilgili eylemi yapılandırmak istediğiniz koşulu tanımlayabilirsiniz. Bu makalede, cihaz tarafından gönderilen olayları izlemek için kurallar oluşturma açıklanmaktadır.

Cihazlar, cihaz önemli veya bilgi olayları göndermek için olay ölçümü kullanabilirsiniz. Seçili cihaz olayı, cihaz tarafından bildirilen bir olayı kuralı tetikler.

## <a name="create-an-event-rule"></a>Olay kuralı oluşturma

Olay kuralı oluşturmak için cihaz şablonu en az bir olay ölçümü tanımlanmış olmalıdır. Bu örnekte, fan motor hata olayı raporları bir refrigerated satış makine cihaz kullanır. Kural, cihaz tarafından bildirilen olay izler ve olay bildirilen her bir e-posta gönderir.

1. Kullanarak **cihaz şablonları** sayfasında, kural ekleme cihaz şablonu gidin.

1. Herhangi bir kural henüz oluşturmadıysanız, aşağıdaki ekranı görürsünüz:

    ![Henüz hiçbir kural](media/howto-create-event-rules/rules_landing_page1.png)

1. Üzerinde **kuralları** sekmesinde **+ yeni kural** kuralları oluşturmak için kullanabileceğiniz türlerini görmek için.

1. Seçin **olay** izleme kuralı bir olay oluşturmak için bir kutucuk.

    ![Kural türü](media/howto-create-event-rules/rule_types1.png)

1. Bu cihaz şablonu kuralında tanımlamanıza yardımcı olacak bir ad girin.

1. Kural bu şablondan oluşturulan tüm cihazlar için hemen etkinleştirmek için geçiş **Bu şablon, tüm cihazlar için etkinleştirme kuralı**.

    ![Kural Ayrıntıları](media/howto-create-event-rules/rule_detail1.png)

    Kural cihaz şablonu altındaki tüm cihazlara otomatik olarak uygulanır.

### <a name="configure-the-rule-conditions"></a>Kural koşulları yapılandırın

Koşul, kural tarafından izlenen ölçütleri tanımlar.

1. Seçin **+** yanındaki **koşullar** yeni bir koşul ekleme.

1. Ölçüm açılan listeden izlemek istediğiniz olayı seçin. Bu örnekte, **Fan Motor hata** olay seçildi.

   ![Koşul](media/howto-create-event-rules/condition_filled_out1.png)

1. İsteğe bağlı olarak da ayarlayabilirsiniz **sayısı** olarak **toplama** ve karşılık gelen eşiği sağlayın.

   - Toplama, koşulu karşılayan her bir olay veri noktası için kural tetiklenir. Kuralın yapılandırırsanız, örneğin, ne zaman tetiklemek için koşul bir **Fan Motor hata** olayı kuralı cihaz bu olay raporları ne zaman hemen hemen harekete sonra oluşur.
   - Bir toplama işlevi kullanılan sayısı sonra sağlamanız gereken bir **eşiği** ve **toplama zaman penceresi** üzerinden hangi koşulun değerlendirilmesi gerekir. Bu durumda, olay sayısı toplanır ve yalnızca, toplanan olay sayısı eşiği eşleşiyorsa kural tetikler.

     Örneğin, 5 dakika içinde üçten fazla cihaz olayları olduğunda uyar isterseniz, olay'ı seçin ve "count" olarak toplama işlevi, işleci olarak "büyüktür" ve "eşik" 3 olarak ayarlayın. "5 dakika" olarak "toplama süre" olarak ayarlayın. Kural üçten fazla olayları 5 dakika içinde cihaz tarafından gönderildiğinde tetiklenir. Kuralı değerlendirme sıklığı aynıdır **toplama zaman penceresi**, yani, bu örnekte, kural her 5 dakikada bir kez değerlendirilir.

     ![Olay koşulu Ekle](media/howto-create-event-rules/aggregate_condition_filled_out1.png)

     >[!NOTE]
     >Birden fazla olay ölçümü altında eklenebilir **koşul**. Birden çok koşulu belirtildiğinde, kural tetiklemek tüm koşulların karşılanması gerekir. Her koşul, bir 'Ve' yan tümcesi tarafından örtük olarak katıldı. Her ölçü, toplam kullanırken toplanmalıdır.

### <a name="configure-actions"></a>Eylemleri Yapılandır

Bu bölümde, kural tetiklendiğinde gerçekleştirilecek eylemleri ayarlamak işlemini göstermektedir. Eylemler kuralda belirtilen tüm koşulların doğru olarak değerlendirilebilmesi çağrılan.

1. Seçin **+** yanındaki **eylemleri**. Burada, kullanılabilir eylemler listesini görürsünüz.

    ![Eylem Ekle](media/howto-create-event-rules/add_action1.png)

1. Seçin **e-posta** eylemi, bir geçerli e-posta adresi girerek **için** alan ve kural tetiklendiğinde e-postanın gövdesinde görüntülenen bir not girin.

    > [!NOTE]
    > E-postaları, yalnızca uygulamaya eklenen ve en az bir kez oturum kullanıcılara gönderilir. Daha fazla bilgi edinin [kullanıcı yönetimi](howto-administer.md) Azure IOT Central içinde.

   ![Eylem yapılandırma](media/howto-create-event-rules/configure_action1.png)

1. Kuralı kaydetmek için seçin **Kaydet**. Kural, birkaç dakika içinde etkin hale gelir ve uygulamanıza gönderilen olaylar izlemeye başlar. Kuralda belirtilen koşul eşleştiğinde kural yapılandırılan e-posta eylemi tetikler.

Diğer Eylemler gibi Microsoft Flow ve Web kancaları kuralı ekleyebilirsiniz. Kural başına en fazla 5 eylem ekleyebilirsiniz.

- [Microsoft Flow eylem](howto-add-microsoft-flow.md) için Microsoft Flow bir iş akışında bir kuralı tetiklendiğinde kazandırın 
- [Web kancası eylemi](howto-create-webhooks.md) bir kuralı tetiklendiğinde diğer hizmetleri bildirmek için

## <a name="parameterize-the-rule"></a>Kural Parametreleştirme

Eylemleri kullanarak yapılandırılabilir **cihaz özelliği** bir parametre olarak. Bir cihaz özelliği depolanan bir e-posta adresi sonra tanımlarken kullanılabilir **için** adresi.

## <a name="delete-a-rule"></a>Kuralı silme

Bir kural artık ihtiyacınız kalmadığında, kural açarak ve silmek **Sil**. Kural siliniyor cihaz şablonunu ve ilişkili tüm cihazlardan kaldırır.

## <a name="enable-or-disable-a-rule-for-a-device-template"></a>Etkinleştirmek veya devre dışı bir cihaz şablonu için bir kural

Cihaza gidin ve etkinleştirme veya devre dışı bırakmak istediğiniz kuralı seçin. İki durumlu **Bu şablon, tüm cihazlar için etkinleştirme kuralı** düğmesini etkinleştirin veya cihaz şablonu ile ilişkili olan tüm cihazlar için kuralı devre dışı bırak.

## <a name="enable-or-disable-a-rule-for-a-device"></a>Etkinleştirmek veya devre dışı bir cihaz için bir kural

Cihaza gidin ve etkinleştirme veya devre dışı bırakmak istediğiniz kuralı seçin. İki durumlu **bu cihaz için etkinleştirme kuralı** düğmesini etkinleştirin veya bu cihaz için kuralı devre dışı bırak.

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT Central uygulamanızda kuralları oluşturulacağını öğrendiniz, bazı İleri Adım şunlardır:

- [Microsoft Flow eylem kurallarında Ekle](howto-add-microsoft-flow.md)
- [Web kancası eylem kurallarında Ekle](howto-create-webhooks.md)
- [Bir veya daha fazla kurallardan çalıştırmak için birden fazla eylem grubu](howto-use-action-groups.md)
- [Cihazlarınızı yönetme](howto-manage-devices.md)
