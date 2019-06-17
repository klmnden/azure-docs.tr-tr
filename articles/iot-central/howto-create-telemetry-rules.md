---
title: Oluşturma ve Azure IOT Central, uygulamanızda telemetri kurallarını yönetin | Microsoft Docs
description: Azure IOT Central telemetri kuralları cihazlarınızı neredeyse gerçek zamanlı izleme ve otomatik olarak kural tetiklendiğinde, bir e-posta gönderme gibi eylemleri çağırmak için etkinleştirin.
author: ankitgupta
ms.author: ankitgup
ms.date: 06/09/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 8684301b83e01989c745b63848995142cb766188
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67052976"
---
# <a name="create-a-telemetry-rule-and-set-up-notifications-in-your-azure-iot-central-application"></a>Bir telemetri kuralı oluşturabilir ve Azure IOT Central uygulamanızdaki bildirimleri ayarlama

*Bu makale, işleçler, oluşturucular ve yöneticiler için geçerlidir.*

Bağlı cihazlarınızın uzaktan izlemek için Azure IOT Central kullanabilirsiniz. Azure IOT Central kuralları cihazlarınızı neredeyse gerçek zamanlı izleme ve otomatik eylemleri bir e-posta veya gibi Microsoft Flow tetiklemek etkinleştirin. Yalnızca birkaç tıklamayla, cihaz verilerinizi izleyin ve ilgili eylemi yapılandırmak istediğiniz koşulu tanımlayabilirsiniz. Bu makalede, cihaz tarafından gönderilen telemetri izlemek için kurallar oluşturma açıklanmaktadır.

Cihazlar, sayısal veriler CİHAZDAN göndermek için telemetri ölçüm kullanabilirsiniz. Seçili cihaz telemetrisi belirtilen eşiği aştığında bir telemetri kural tetikler.

## <a name="create-a-telemetry-rule"></a>Telemetri kuralı oluşturma

Telemetri kuralı oluşturmak için cihaz şablonu en az bir telemetri ölçüm tanımlanmış olmalıdır. Bu örnek, sıcaklık ve nem telemetrisini gönderen bir refrigerated satış makine cihaz kullanır. Kural, cihaz tarafından bildirilen sıcaklık izler ve 80 derecenin üzerinde olduğunda bir e-posta gönderir.

1. Kullanarak **cihaz şablonları** sayfasında, kural ekleme cihaz şablonu gidin.

1. Herhangi bir kural henüz oluşturmadıysanız, aşağıdaki ekranı görürsünüz:

    ![Henüz hiçbir kural](media/howto-create-telemetry-rules/rules_landing_page1.png)

1. Üzerinde **kuralları** sekmesinde **+ yeni kural** kuralları oluşturmak için kullanabileceğiniz türlerini görmek için.

1. Seçin **Telemetri** cihaz telemetrisi izlemek için bir kural oluşturmak için.

    ![Kural türü](media/howto-create-telemetry-rules/rule_types1.png)

1. Bu cihaz şablonu kuralında tanımlamanıza yardımcı olacak bir ad girin.

1. Bu şablon için oluşturulan tüm cihazlar için kuralın hemen etkinleştirmek için geçiş **Bu şablon için tüm cihazlar için etkinleştirme kuralı**.

   ![Kural Ayrıntıları](media/howto-create-telemetry-rules/rule_detail1.png)

    Kural cihaz şablonu altındaki tüm cihazlara otomatik olarak uygulanır.

### <a name="configure-the-rule-conditions"></a>Kural koşulları yapılandırın

Koşul, kural tarafından izlenen ölçütleri tanımlar.

1. Seçin **+** yanındaki **koşullar** yeni bir koşul ekleme.

1. İzlemek istediğiniz telemetri seçin **ölçüm** açılır.

1. Ardından, **toplama**, **işleci**ve bir **eşiği** değeri.
   - Toplama isteğe bağlıdır. Toplama, koşulu karşılayan her telemetri veri noktası için kural tetiklendiğinde. Kural neredeyse anında sıcaklık 80 ise tetikleyici sonra kural Tetikleyiciler için yapılandırılmışsa, örneğin, ne zaman cihaz sıcaklık > 80 bildirir.
   - Bir toplama işlevi, ortalama, Min, maks gibi sayısı daha sonra seçilen kullanıcı sağlamalıdır bir **toplama zaman penceresi** üzerinden hangi koşulun değerlendirilmesi gerekir. Örneğin, ayarlarsanız "5 dakika" olarak döneme ve kural arar 80 ortalama sıcaklık en az 5 dakika boyunca 80 olduğunda kural tetiklendiğinde yukarıda ortalama sıcaklık. Kuralı değerlendirme sıklığı aynıdır **toplama zaman penceresi**, yani, bu örnekte, kural her 5 dakikada bir kez değerlendirilir.

     ![Koşul](media/howto-create-telemetry-rules/aggregate_condition_filled_out1.png)

     >[!NOTE]
     >Birden fazla telemetri Ölçüm altında eklenebilir **koşul**. Birden çok koşulu belirtildiğinde, kural tetiklemek tüm koşulların karşılanması gerekir. Her koşulu, bir 'Ve' yan tümcesi tarafından örtük olarak katıldı. Her ölçü, toplam kullanırken toplanmalıdır.

### <a name="configure-actions"></a>Eylemleri Yapılandır

Bu bölümde, kural tetiklendiğinde gerçekleştirilecek eylemleri ayarlamak işlemini göstermektedir. Eylemler kuralda belirtilen tüm koşulların doğru olarak değerlendirilebilmesi çağrılan.

1. Seçin **+** yanındaki **eylemleri**. Burada, kullanılabilir eylemler listesini görürsünüz.  

    ![Eylem Ekle](media/howto-create-telemetry-rules/add_action1.png)

1. Seçin **e-posta** eylemi, bir geçerli e-posta adresi girerek **için** alan ve kural tetiklendiğinde e-postanın gövdesinde görüntülenen bir not girin.

    > [!NOTE]
    > E-postaları, yalnızca uygulamaya eklenen ve en az bir kez oturum kullanıcılara gönderilir. Daha fazla bilgi edinin [kullanıcı yönetimi](howto-administer.md) Azure IOT Central içinde.

   ![Eylem yapılandırma](media/howto-create-telemetry-rules/configure_action1.png)

1. Kuralı kaydetmek için seçin **Kaydet**. Kural, birkaç dakika içinde etkin hale gelir ve uygulamanıza gönderilen telemetri izlemeye başlar. Kuralda belirtilen koşul karşılandığında kural yapılandırılan e-posta eylemi tetikler.

Diğer Eylemler gibi Microsoft Flow ve Web kancaları kuralı ekleyebilirsiniz. Kural başına en fazla 5 eylem ekleyebilirsiniz.

- [Microsoft Flow eylem](howto-add-microsoft-flow.md) için Microsoft Flow bir iş akışında bir kuralı tetiklendiğinde kazandırın 
- [Web kancası eylemi](howto-create-webhooks.md) bir kuralı tetiklendiğinde diğer hizmetleri bildirmek için

## <a name="parameterize-the-rule"></a>Kural Parametreleştirme

Kuralları, gelen belirli değerlerinde türetilebilir **cihaz özelliklerini** parametre olarak. Parametreleri kullanarak, burada telemetri eşikleri farklı cihazlar için farklı senaryolarda yararlıdır. Kuralı oluşturduğunuzda, eşik gibi belirten bir cihaz özelliği seçin **Ideal en yüksek eşik**, 80 derece gibi mutlak bir değer sağlamak yerine. Kural yürütüldüğünde, cihaz telemetrisi cihaz özelliğinde ayarlanan değer ile eşleşir.

Parametreleri kullanarak, cihaz şablonunu yönetmek için kuralların sayısını azaltmak için etkili bir yoludur.

Eylemleri kullanarak yapılandırılabilir **cihaz özelliği** bir parametre olarak. Bir özellik olarak depolanan bir e-posta adresi sonra tanımlarken kullanılabilir **için** adresi.

## <a name="delete-a-rule"></a>Kuralı silme

Bir kural artık ihtiyacınız kalmadığında, kural açarak ve silmek **Sil**. Kural siliniyor cihaz şablonunu ve ilişkili tüm cihazlardan kaldırır.

## <a name="enable-or-disable-a-rule-for-a-device-template"></a>Etkinleştirmek veya devre dışı bir cihaz şablonu için bir kural

Cihaza gidin ve etkinleştirme veya devre dışı bırakmak istediğiniz kuralı seçin. İki durumlu **Bu şablon, tüm cihazlar için etkinleştirme kuralı** kuralda etkinleştirin veya cihaz şablonu ile ilişkili olan tüm cihazlar için kuralı devre dışı bırak düğmesi.

## <a name="enable-or-disable-a-rule-for-a-device"></a>Etkinleştirmek veya devre dışı bir cihaz için bir kural

Cihaza gidin ve etkinleştirme veya devre dışı bırakmak istediğiniz kuralı seçin. İki durumlu **bu cihaz için etkinleştirme kuralı** düğmesini etkinleştirin veya bu cihaz için kuralı devre dışı bırak.

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT Central uygulamanızda kuralları oluşturulacağını öğrendiniz, bazı İleri Adım şunlardır:

- [Microsoft Flow eylem kurallarında Ekle](howto-add-microsoft-flow.md)
- [Web kancası eylem kurallarında Ekle](howto-create-webhooks.md)
- [Bir veya daha fazla kurallardan çalıştırmak için birden fazla eylem grubu](howto-use-action-groups.md)
- [Cihazlarınızı yönetme](howto-manage-devices.md)
