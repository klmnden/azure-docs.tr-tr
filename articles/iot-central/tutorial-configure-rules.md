---
title: Azure IoT Central’da kural ve eylem yapılandırma | Microsoft Docs
description: Bu öğreticide, bir oluşturucu olarak Azure IoT Central uygulamanızda telemetri tabanlı kural ve eylemleri nasıl yapılandıracağınız gösterilir.
author: ankitscribbles
ms.author: ankitgup
ms.date: 01/28/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: 2bd22dec13e2dba63c512276a0b36d4fc50e05f8
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59681574"
---
# <a name="tutorial-configure-rules-and-actions-for-your-device-in-azure-iot-central"></a>Öğretici: Kurallar ve Eylemler için Cihazınızı Azure IOT Central yapılandırma

*Bu makale, işleçler, oluşturucular ve yöneticiler için geçerlidir.*

Bu öğreticide, bağlı bir klima cihazı 90&deg; F sıcaklığı aştığında e-posta gönderen bir kural oluşturacaksınız.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Telemetri tabanlı kural oluşturma
> * Eylem ekleme

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce, [Uygulamanızda yeni bir cihaz türü tanımlama](tutorial-define-device-type.md) öğreticisini tamamlayarak birlikte çalışacağınız **Bağlı Klima** cihaz şablonunu oluşturmanız gerekir.

## <a name="create-a-telemetry-based-rule"></a>Telemetri tabanlı kural oluşturma

1. Sol gezinti menüsünde, uygulamanıza, telemetri tabanlı yeni bir kural eklemek için seçin **cihaz şablonları**:

    ![Cihaz şablonlarını](media/tutorial-configure-rules/templatespage1.png)

    Gördüğünüz **bağlı klima (1.0.0)** önceki öğreticide oluşturduğunuz cihaz şablonu.

2. Cihaz şablonunuzu özelleştirme için seçin **bağlı bir klima** önceki öğreticide oluşturduğunuz şablonu.

3. Telemetri tabanlı bir kural eklemek için **kuralları** görüntülenecek **kuralları**seçin **+ yeni kural**ve ardından **Telemetri**:

    ![Kurallar görünümü](media/tutorial-configure-rules/newrule.png)

5. Kuralınızı tanımlamak için aşağıdaki tabloda yer alan bilgileri kullanın:

    | Ayar                                      | Değer                             |
    | -------------------------------------------- | ------------------------------    |
    | Ad                                         | Klima sıcaklığı uyarısı |
    | Bu şablonun tüm cihazları için kuralı etkinleştir | Açık                                |
    | Koşul                                    | Sıcaklık 90’dan fazla    |
    | Toplama                                  | None                              |

    ![Sıcaklık kural koşulu](media/tutorial-configure-rules/temperaturerule.png)

    Daha sonra **Kaydet**’e tıklayın.

## <a name="add-an-action"></a>Eylem ekleme

Bir kural tanımladığınızda, kural koşulları yerine getirildiği zaman çalıştırılacak bir eylem de tanımlarsınız. Bu öğreticide, bir e-posta bildirimi gönderen bir eylem ile bir kural oluşturun.

1. Eklemek için bir **eylem**, ilk **Kaydet** kuralı ve sonra aşağı üzerinde **Telemetri kuralını yapılandırın** paneli. Seçin **+** yanındaki **eylemleri**ve ardından **e-posta**:

    ![Sıcaklık kuralı eylemi](media/tutorial-configure-rules/addaction.png)

2. Eyleminizi tanımlamak için aşağıdaki tabloda yer alan bilgileri kullanın:

    | Ayar   | Değer                          |
    | --------- | ------------------------------ |
    | Alıcı        | E-posta adresiniz             |
    | Notlar     | Klima sıcaklığı eşiği aştı. |

    > [!NOTE]
    > Bir e-posta bildirimi almak için e-posta adresinin [uygulamadaki bir kullanıcı kimliği](howto-administer.md) olması ve bu kullanıcının uygulamada en az bir kez oturum açmış olması gerekir.

    ![Sıcaklık eylemi](media/tutorial-configure-rules/temperatureaction.png)

3. **Kaydet**’i seçin. Kural listelenir **kuralları** sayfası.

## <a name="test-the-rule"></a>Kuralı test etme

Kural kaydedildikten kısa bir süre sonra dinamik olur. Kuralda tanımlanan koşullar karşılandığında, uygulamanız eylemde belirtilen e-posta adresine bir ileti gönderir.

> [!NOTE]
> Test tamamlandıktan sonra gelen kutunuzda uyarıları almayı durdurmak için kural kapatın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

<!-- Repeat task list from intro -->
> [!div class="nextstepaction"]
> * Telemetri tabanlı kural oluşturma
> * Eylem ekleme

Eşik tabanlı bir kural tanımladığınıza göre önerilen sonraki adım olarak [işlecin görünümlerini özelleştirme](tutorial-customize-operator.md).

Azure IoT Central’daki farklı kural türleri ve kural tanımının nasıl parametre haline getirileceği hakkında daha fazla bilgi için bkz.:
* [Bir telemetri kuralı oluşturun ve bildirimleri ayarlayın](howto-create-telemetry-rules.md).
* [Bir olay kuralı oluşturun ve bildirimleri ayarlayın](howto-create-event-rules.md).

<!-- Next tutorials in the sequence -->