---
title: Oluşturma ve Azure IOT merkezi uygulamanızda olay kuralları yönetme | Microsoft Docs
description: Azure IOT Orta olay kuralları aygıtlarınızı yakın gerçek zamanlı izleme ve otomatik olarak kural harekete geçirdiğinde, bir e-posta gönderme gibi eylemleri çağırmak için etkinleştirin.
services: iot-central
author: ankitgupta
ms.author: ankitgup
ms.date: 04/29/2018
ms.topic: article
ms.prod: microsoft-iot-central
manager: timlt
ms.openlocfilehash: 932b1906b767ee7676f46ffd7242ad3d478d41c2
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="create-an-event-rule-and-set-up-notifications-in-your-azure-iot-central-application"></a>Bir olay kuralı oluşturma ve Azure IOT merkezi uygulamanızda bildirimlerini ayarlama

Microsoft Azure IOT merkezi uzaktan bağlı aygıtları izlemek için kullanabilirsiniz. Azure IOT Orta kuralları aygıtlarınızı yakın gerçek zamanlı izleme ve otomatik olarak kural harekete geçirdiğinde, bir e-posta gönderme gibi eylemleri çağırmak için etkinleştirin. Yalnızca birkaç tıklama ile cihaz verilerinizi izlemek ve çağrılacak eylem yapılandırmak için koşul tanımlayabilirsiniz. Bu makalede izleme kuralı ayrıntılı olay açıklanmaktadır.

Azure IOT Orta kullanan [olay ölçüm](howto-set-up-template.md) aygıt verilerini yakalamak için. Her ölçü ölçüm tanımlayan anahtar öznitelikleri türü. Her cihaz ölçüm türü izlemek ve kural harekete geçirdiğinde uyarıları oluşturmak için kurallar oluşturabilirsiniz. Seçilen aygıt olay aygıt tarafından bildirildiğinde bir olay kuralı tetikler.

## <a name="create-an-event-rule"></a>Bir olay kuralı oluşturma

Bu bölümde bir olay kuralının nasıl oluşturulacağını gösterir. Bu örnek, bu raporları fan motor hata olayı refrigerated satış makine aygıt kullanır. Kural aygıt tarafından rapor edilen olay izler ve olay bildirilen her bir e-posta gönderir.

1. Kural için eklemekte olduğunuz cihaz için cihaz ayrıntıları sayfasına gidebilirsiniz.

1. Herhangi bir kuralın henüz oluşturmadıysanız, aşağıdaki ekranı görürsünüz:

    ![Henüz hiçbir kural](media\howto-create-event-rules\image1.png)

1. Üzerinde **kuralları** sekmesinde, seçin **+ yeni kural** oluşturabileceğiniz kural türlerini görmek için.

    ![Kural türleri](media\howto-create-event-rules\image2.png)

1. Tıklayın **olay** kuralı oluşturmak için formunu açmak üzere.

    ![Olay kuralı](media\howto-create-event-rules\image3.png)

1. Bu cihaz şablonu kuralında tanımlamanıza yardımcı olacak bir ad seçin.

1. Hemen bu şablondan oluşturulan tüm aygıtlar için kuralını etkinleştirmek için geçiş **etkinleştir kural**.

### <a name="configure-the-rule-condition"></a>Kural koşulu yapılandırın

Bu bölümde Fan motor hata olayı ölçüm izlemek için bir koşul eklemek nasıl gösterir.

1. Seçin **+** yanına **koşul**.

1. Olay ölçüm izlemek istediğiniz aşağı açılır listeden seçin. Bu örnekte **Fan Motor hata** olay seçildi.

1. İsteğe bağlı olarak belirli bir aygıt tarafından rapor edilen olay değerini izlemek isteyebileceğiniz de bir değer sağlayabilir. Cihaz kuralın koşuluyla bir değer olarak hata kodu sağlayan farklı hata kodlarıyla aynı olay bildirirse yalnızca cihaz olay yükü olarak belirli değeri gönderdiğinde örneği için kural Tetikleyicileri ilişkilendirilmesini sağlar. Bu boş bırakılırsa, olayın olay değeri bakılmaksızın cihaz gönderir her kural tetikleyecek anlamına gelir.

    ![Olay koşulu ekleyin](media\howto-create-event-rules\image4.png)

    > [!NOTE]
    > Bir olay Kural koşulu tanımlarken, en az bir olay ölçüm seçmeniz gerekir.

### <a name="configure-the-action"></a>Eylemi yapılandırın

Bu bölümde koşul bir eylem ekleyerek eşleştiğinde kuralın ne yaptığını belirtme gösterir.

1. Seçin **+** yanına **Eylemler**. Burada listesi kullanılabilir eylemler için bkz. Genel Önizleme sırasında **e-posta** yalnızca desteklenen eylemdir.

    ![Eylem Ekle](media\howto-create-event-rules\image5.png)

1. Seçin **e-posta** eylem, bir geçerli e-posta adresi girmeniz **için** alan ve kural harekete geçirdiğinde e-posta gövdesinde yer için not girin.

    > [!NOTE]
    > E-postalar yalnızca uygulamaya eklenen ve en az bir kez günlüğe kullanıcılara gönderilir. Daha fazla bilgi edinmek [kullanıcı yönetimi](howto-administer.md) Azure IOT merkezi olarak.

   ![Eylemi yapılandırın](media\howto-create-event-rules\image6.png)

1. Kuralı kaydetmek üzere seçim yapın **kaydetmek**. Kural birkaç dakika içinde dinamik gider ve uygulamanıza gönderilen olayları izleme başlatır. Kural içinde belirtilen koşulun eşleştiğinde kural yapılandırılan e-posta eylemi tetikler.

## <a name="parameterize-the-rule"></a>Kural Parametreleştirme

Eylemler de kullanılarak yapılandırılabilir **cihaz özelliği** bir parametre olarak. Bir e-posta adresi bir cihaz özelliği depolanan sonra tanımlarken kullanılabilir **için** adresi.

## <a name="delete-a-rule"></a>Kural silme

Bir kural artık ihtiyacınız varsa, kural açarak ve seçme silmek **silmek**. Kural silme aygıt şablonunu ve ilişkili tüm cihazları kaldırır.

## <a name="enable-or-disable-a-rule-for-a-device-template"></a>Etkinleştirmek veya bir aygıt şablonu için bir kural devre dışı bırakma

Cihaza gidin ve etkinleştirmek veya devre dışı bırakmak istediğiniz kuralı seçin. Geçiş **bu şablonu tüm cihazlar için etkinleştirme kuralı** kural düğmesini etkinleştirir veya aygıt şablonuyla ilişkili tüm aygıtlar için kural devre dışı bırakır.

## <a name="enable-or-disable-a-rule-for-a-device"></a>Etkinleştirmek veya bir aygıt için bir kural devre dışı bırakma

Cihaza gidin ve etkinleştirmek veya devre dışı bırakmak istediğiniz kuralı seçin. İki durumlu **bu cihaz için etkinleştirme kuralı** düğmesini etkinleştirin veya bu cihaz için kural devre dışı bırakmak için.

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT merkezi uygulamanızda kuralları oluşturmak öğrendiniz, önerilen sonraki adım aşağıda verilmiştir:

> [!div class="nextstepaction"]
> [Cihazlarınızı yönetmek nasıl](howto-manage-devices.md).