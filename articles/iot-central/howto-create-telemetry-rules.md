---
title: Oluşturma ve Azure IOT merkezi uygulamanızda telemetri kurallarını yönetme | Microsoft Docs
description: Azure IOT Orta telemetri kuralları aygıtlarınızı yakın gerçek zamanlı izleme ve otomatik olarak kural harekete geçirdiğinde, bir e-posta gönderme gibi eylemleri çağırmak için etkinleştirin.
author: tbhagwat3
ms.author: tanmayb
ms.date: 04/16/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: caca4e9db898b3766995fde8c5eebd4767abd85b
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34629822"
---
# <a name="create-a-telemetry-rule-and-set-up-notifications-in-your-azure-iot-central-application"></a>Bir telemetri kuralı oluşturabilir ve Azure IOT merkezi uygulamanızda bildirimlerini ayarlama

Microsoft Azure IOT merkezi uzaktan bağlı aygıtları izlemek için kullanabilirsiniz. Azure IOT Orta kuralları aygıtlarınızı yakın gerçek zamanlı izleme ve otomatik olarak kural harekete geçirdiğinde, bir e-posta gönderme gibi eylemleri çağırmak için etkinleştirin. Yalnızca birkaç tıklama ile cihaz verilerinizi izlemek ve çağrılacak eylem yapılandırmak için koşul tanımlayabilirsiniz. Bu makalede telemetri kural ayrıntılı açıklanmaktadır.

Azure IOT Orta kullanan [telemetri ölçümleri](howto-set-up-template.md) aygıt verilerini yakalamak için. Her ölçü ölçüm tanımlayan anahtar öznitelikleri türü. Her cihaz ölçüm türü izlemek ve kural harekete geçirdiğinde uyarıları oluşturmak için kurallar oluşturabilirsiniz. Seçilen aygıt telemetri belirtilen eşiğin kestiği telemetri kural tetikler.

## <a name="create-a-telemetry-rule"></a>Telemetri kural oluşturma

Bu bölümde bir telemetri kuralının nasıl oluşturulacağını gösterir. Bu örnek sıcaklık ve nem telemetrisi gönderir bağlı bir klima aygıt kullanır. Kural cihaz tarafından raporlanan sıcaklık izler ve 80 derecenin gittiğinde bir e-posta gönderir.

1. Kural için eklemekte olduğunuz cihaz için cihaz ayrıntıları sayfasına gidebilirsiniz.

1. Herhangi bir kuralın henüz oluşturmadıysanız, aşağıdaki ekranı görürsünüz:

    ![Henüz hiçbir kural](media\howto-create-telemetry-rules\image1.png)

1. Üzerinde **kuralları** sekmesinde, seçin **+ yeni kural** oluşturabileceğiniz kural türlerini görmek için.

    ![Kural türleri](media\howto-create-telemetry-rules\image2.png)

1. Seçin **Telemetri** kuralı oluşturmak için formunu açmak için kutucuğa.

    ![Telemetri kuralı](media\howto-create-telemetry-rules\image3.png)

1. Bu cihaz şablonu kuralında tanımlamanıza yardımcı olacak bir ad seçin.

1. Hemen bu şablondan oluşturulan tüm aygıtlar için kuralını etkinleştirmek için geçiş **etkinleştir kural**.

### <a name="configure-the-rule-condition"></a>Kural koşulu yapılandırın

Bu bölümde sıcaklık telemetri izlemek için bir koşul eklemek nasıl gösterir.

1. Seçin **+** yanına **koşul**.

1. Seçin **sıcaklık** telemetri türünden açılır. Ardından işleci seçin ve bir eşik değeri sağlayın. Birden çok telemetri koşullar ekleyebilirsiniz. Birden çok koşul belirtildiğinde, kuralın tetiklemek için tüm koşulların karşılanması gerekir.

    ![Telemetri koşul Ekle](media\howto-create-telemetry-rules\image4.png)

    > [!NOTE]
    > Bir telemetri Kural koşulu tanımladığınızda en az bir telemetri ölçümü seçin.

### <a name="configure-the-action"></a>Eylemi yapılandırın

Bu bölümde koşul bir eylem ekleyerek eşleştiğinde kuralın ne yaptığını belirtme gösterir.

1. Seçin **+** yanına **Eylemler**. Burada listesi kullanılabilir eylemler için bkz. Genel Önizleme sırasında **e-posta** yalnızca desteklenen eylemdir.

    ![Eylem Ekle](media\howto-create-telemetry-rules\image5.png)

1. Seçin **e-posta** eylem, bir geçerli e-posta adresi girmeniz **için** alan ve kural harekete geçirdiğinde e-posta gövdesinde yer için not girin.

    > [!NOTE]
    > E-postalar yalnızca uygulamaya eklenen ve en az bir kez günlüğe kullanıcılara gönderilir. Daha fazla bilgi edinmek [kullanıcı yönetimi](howto-administer.md) Azure IOT merkezi olarak.

   ![Eylemi yapılandırın](media\howto-create-telemetry-rules\image6.png)

1. Kuralı kaydetmek üzere seçim yapın **kaydetmek**. Kural birkaç dakika içinde dinamik gider ve uygulamanıza gönderilen telemetri izleme başlatır. Kural içinde belirtilen koşulun eşleştiğinde kural yapılandırılan e-posta eylemi tetikler.

## <a name="parameterize-the-rule"></a>Kural Parametreleştirme

Kuralları belirli değerlerinde öğesinden türetilen **cihaz özellikleri** parametre olarak. Parametreleri kullanarak, burada telemetri eşikleri farklı aygıtlar için farklılık senaryolarında yararlıdır. Kuralı oluşturduğunuzda, eşik gibi belirten bir cihaz özelliği seçin **maksimum Ideal eşik**, 80 derece gibi mutlak bir değer sağlama yerine. Kural yürütüldüğünde, cihaz telemetrisi cihaz özelliğinde sağlanan değer ile eşleşir.

Parametrelerini kullanarak cihaz şablonu yönetmek için kuralların sayısını azaltmak için etkili bir yoldur.

Eylemler de kullanılarak yapılandırılabilir **cihaz özelliği** bir parametre olarak. Bir e-posta adresi bir cihaz özelliği depolanan sonra tanımlarken kullanılabilir **için** adresi.

## <a name="delete-a-rule"></a>Kural silme

Bir kural artık ihtiyacınız varsa, kural açarak ve seçme silmek **silmek**. Kural silme aygıt şablonunu ve ilişkili tüm cihazları kaldırır.

## <a name="enable-or-disable-a-rule-for-a-device-template"></a>Etkinleştirmek veya bir aygıt şablonu için bir kural devre dışı bırakma

Cihaza gidin ve etkinleştirmek veya devre dışı bırakmak istediğiniz kuralı seçin. Geçiş **bu şablonu tüm cihazlar için etkinleştirme kuralı** kural düğmesini etkinleştirir veya aygıt şablonuyla ilişkili tüm aygıtlar için kural devre dışı bırakır.

## <a name="enable-or-disable-a-rule-for-a-device"></a>Etkinleştirmek veya bir aygıt için bir kural devre dışı bırakma

Cihaza gidin ve etkinleştirmek veya devre dışı bırakmak istediğiniz kuralı seçin. İki durumlu **bu cihaz için etkinleştirme kuralı** düğmesini etkinleştirin veya bu cihaz için kural devre dışı bırakmak için.

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT merkezi uygulamanızda kuralları düzenlemek öğrendiniz, önerilen sonraki adım aşağıda verilmiştir:

> [!div class="nextstepaction"]
> [Cihazlarınızı yönetmek nasıl](howto-manage-devices.md).