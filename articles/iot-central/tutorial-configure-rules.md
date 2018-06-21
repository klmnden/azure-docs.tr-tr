---
title: Azure IoT Central’da kural ve eylem yapılandırma | Microsoft Docs
description: Bu öğreticide, bir oluşturucu olarak Azure IoT Central uygulamanızda telemetri tabanlı kural ve eylemleri nasıl yapılandıracağınız gösterilir.
author: ankitgupta
ms.author: ankitgup
ms.date: 04/16/2018
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: af2aa8d7b01d973da400808fd3e97d0739693cd2
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35236340"
---
# <a name="tutorial-configure-rules-and-actions-for-your-device-in-azure-iot-central"></a>Öğretici: Azure IoT Central’da cihazınız için kurallar ve eylemler yapılandırma

Bu öğreticide, bir oluşturucu olarak Microsoft Azure IoT Central uygulamanızda telemetri tabanlı kural ve eylemleri nasıl yapılandıracağınız gösterilir.

Bu öğreticide, bağlı bir klima cihazı 90&deg; F sıcaklığı aştığında e-posta gönderen bir kural oluşturacaksınız.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Telemetri tabanlı kural oluşturma
> * Eylem ekleme

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce, [Uygulamanızda yeni bir cihaz türü tanımlama](tutorial-define-device-type.md) öğreticisini tamamlayarak birlikte çalışacağınız **Bağlı Klima** cihaz şablonunu oluşturmanız gerekir.

## <a name="create-a-telemetry-based-rule"></a>Telemetri tabanlı kural oluşturma

1. Uygulamanıza yeni bir telemetri tabanlı kural eklemek için, sol gezinti menüsünde **Device Explorer**’ı seçin:

    ![Device Explorer sayfası](media/tutorial-configure-rules/explorerpage.png)

    **Bağlı Klima (1.0.0)** cihaz şablonunu ve önceki öğreticide oluşturduğunuz **Bağlı Klima-1** cihazını görürsünüz.

2. Bağlı klima cihazınızı özelleştirmeye başlamak için, önceki öğreticide oluşturduğunuz cihazı seçin:

    ![Bağlı klima sayfası](media/tutorial-configure-rules/builderdevicelist.png)

3. **Kurallar** görünümünde bir kural eklemeye başlamak için **Kurallar**’ı seçin:

    ![Kurallar görünümü](media/tutorial-configure-rules/builderrulesview.png)

4. Eşik tabanlı telemetri kuralı oluşturmaya başlamak için **Yeni Kural** ve ardından **Telemetri**’yi seçin.

5. Kuralınızı tanımlamak için aşağıdaki tabloda yer alan bilgileri kullanın:

    | Ayar     | Değer                          |
    | ----------- | ------------------------------ |
    | Adı        | Klima sıcaklığı    |
    | Etkinleştirme kuralı | Açık                             |
    | Koşul   | Sıcaklık 90’dan fazla |

    ![Sıcaklık kural koşulu](media/tutorial-configure-rules/buildertemperaturerule.png)

## <a name="add-an-action"></a>Eylem ekleme

Bir kural tanımladığınızda, kural koşulları yerine getirildiği zaman çalıştırılacak bir eylem de tanımlarsınız. Bu öğreticide, kuralın tetiklediği bir bildirim olarak e-posta gönderme eylemi eklersiniz.

1. Bir **Eylem** eklemek için **Telemetri Kuralı Yapılandırma** panelini kaydırın ve **Eylemler**’in yanındaki **+** öğesini, ardından **E-posta**’yı seçin:

    ![Sıcaklık kuralı eylemi](media/tutorial-configure-rules/builderaddaction.png)

2. Eyleminizi tanımlamak için aşağıdaki tabloda yer alan bilgileri kullanın:

    | Ayar   | Değer                          |
    | --------- | ------------------------------ |
    | Alıcı        | E-posta adresiniz             |
    | Notlar     | Klimada sıcaklığın eşiği aşması. |

    > [!NOTE]
    > Bir e-posta bildirimi almak için e-posta adresinin [uygulamadaki bir kullanıcı kimliği](howto-administer.md) olması ve bu kullanıcının uygulamada en az bir kez oturum açmış olması gerekir.

    ![Uygulama Oluşturucu Sıcaklık eylemi](media/tutorial-configure-rules/buildertemperatureaction.png)

3. **Kaydet**'i seçin. Kuralınız **Kurallar** sayfasında listelenir:

    ![Uygulama Oluşturucu kuralları](media/tutorial-configure-rules/builderrules.png)

## <a name="test-the-rule"></a>Kuralı test etme

Kural kaydedildikten kısa bir süre sonra dinamik olur. Kuralda tanımlanan koşullar karşılandığında, uygulamanız eylemde belirtilen e-posta adresine bir ileti gönderir.

![E-posta eylemi](media/tutorial-configure-rules/email.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

<!-- Repeat task list from intro -->
> [!div class="nextstepaction"]
> * Telemetri tabanlı kural oluşturma
> * Eylem ekleme

Eşik tabanlı bir kural tanımladıktan sonra önerilen adım, [Operatör görünümlerini özelleştirme](tutorial-customize-operator.md) adımıdır.

Azure IoT Central’daki farklı kural türleri ve kural tanımının nasıl parametre haline getirileceği hakkında daha fazla bilgi için bkz.:
* [Bir telemetri kuralı oluşturun ve bildirimleri ayarlayın](howto-create-telemetry-rules.md).
* [Bir olay kuralı oluşturun ve bildirimleri ayarlayın](howto-create-event-rules.md).

<!-- Next tutorials in the sequence -->
