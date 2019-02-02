---
title: Azure IoT Central’da kural ve eylem yapılandırma | Microsoft Docs
description: Bu öğreticide, bir oluşturucu olarak Azure IoT Central uygulamanızda telemetri tabanlı kural ve eylemleri nasıl yapılandıracağınız gösterilir.
author: ankitscribbles
ms.author: ankitgup
ms.date: 10/12/2018
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: d7269f61579ce1ffd9a686634effd153837a2f25
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2019
ms.locfileid: "55663001"
---
# <a name="tutorial-configure-rules-and-actions-for-your-device-in-azure-iot-central"></a>Öğretici: Kurallar ve Eylemler için Cihazınızı Azure IOT Central yapılandırma

*Bu makale, işleçler, oluşturucular ve yöneticiler için geçerlidir.*

Bu öğreticide, bağlı bir klima cihazı 90&deg; F sıcaklığı aştığında e-posta gönderen bir kural oluşturacaksınız.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Telemetri tabanlı kural oluşturma
> * Eylem ekleme

[!INCLUDE [iot-central-experimental-note](../../includes/iot-central-experimental-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce, [Uygulamanızda yeni bir cihaz türü tanımlama](tutorial-define-device-type.md) öğreticisini tamamlayarak birlikte çalışacağınız **Bağlı Klima** cihaz şablonunu oluşturmanız gerekir.

## <a name="create-a-telemetry-based-rule"></a>Telemetri tabanlı kural oluşturma

1. Uygulamanıza yeni bir telemetri tabanlı kural eklemek için, sol gezinti menüsünde **Device Explorer**’ı seçin:

    ![Device Explorer sayfası](media/tutorial-configure-rules/explorerpage1.png)

    **Bağlı Klima (1.0.0)** cihaz şablonunu ve önceki öğreticide oluşturduğunuz **Bağlı Klima-1** cihazını görürsünüz.

2. Bağlı klima cihazınızı özelleştirmeye başlamak için, önceki öğreticide oluşturduğunuz cihazı seçin:

    ![Bağlı klima sayfası](media/tutorial-configure-rules/builderdevicelist1.png)

3. **Kurallar** görünümünde bir kural eklemeye başlamak için **Kurallar**’ı seçin ve **Şablonu Düzenle**’ye tıklayın:

    ![Kurallar görünümü](media/tutorial-configure-rules/builderedittemplate.png)

4. Eşik tabanlı telemetri kuralı oluşturmak için **Yeni Kural**’a ve sonra **Telemetri**‘ye tıklayın.

    ![Şablon düzenleme](media/tutorial-configure-rules/buildernewrule.png)

5. Kuralınızı tanımlamak için aşağıdaki tabloda yer alan bilgileri kullanın:

    | Ayar                                      | Değer                             |
    | -------------------------------------------- | ------------------------------    |
    | Ad                                         | Klima sıcaklığı uyarısı |
    | Bu şablonun tüm cihazları için kuralı etkinleştir | Açık                                |
    | Bu cihazda kuralı etkinleştir                   | Açık                                |
    | Koşul                                    | Sıcaklık 90’dan fazla    |
    | Toplama                                  | None                              |

    ![Sıcaklık kural koşulu](media/tutorial-configure-rules/buildertemperaturerule1.png)

## <a name="add-an-action"></a>Eylem ekleme

Bir kural tanımladığınızda, kural koşulları yerine getirildiği zaman çalıştırılacak bir eylem de tanımlarsınız. Bu öğreticide, kuralın tetiklediği bir bildirim olarak e-posta gönderme eylemi eklersiniz.

1. Bir **Eylem** eklemek için **Kaydet**’e tıklayarak kuralı kaydedin ve sonra **Telemetri Kuralı Yapılandırma** panelini kaydırarak **Eylemler**’in yanındaki **+** seçeneğini belirleyip **E-posta**‘yı seçin:

    ![Sıcaklık kuralı eylemi](media/tutorial-configure-rules/builderaddaction1.png)

2. Eyleminizi tanımlamak için aşağıdaki tabloda yer alan bilgileri kullanın:

    | Ayar   | Değer                          |
    | --------- | ------------------------------ |
    | Alıcı        | E-posta adresiniz             |
    | Notlar     | Klima sıcaklığı eşiği aştı. |

    > [!NOTE]
    > Bir e-posta bildirimi almak için e-posta adresinin [uygulamadaki bir kullanıcı kimliği](howto-administer.md) olması ve bu kullanıcının uygulamada en az bir kez oturum açmış olması gerekir.

    ![Uygulama Oluşturucu Sıcaklık eylemi](media/tutorial-configure-rules/buildertemperatureaction.png)

3. **Kaydet**'i seçin. Kuralınız **Kurallar** sayfasında listelenir:

    ![Uygulama Oluşturucu kuralları](media/tutorial-configure-rules/builderrules1.png)

4. **Bitti**’yi seçerek **Şablonu Düzenle** modundan çıkın.
 

## <a name="test-the-rule"></a>Kuralı test etme

Kural kaydedildikten kısa bir süre sonra dinamik olur. Kuralda tanımlanan koşullar karşılandığında, uygulamanız eylemde belirtilen e-posta adresine bir ileti gönderir.

![E-posta eylemi](media/tutorial-configure-rules/email.png)

> [!NOTE]
> Test tamamlandıktan sonra gelen kutunuza uyarı gönderilmemesi için kuralı kapatın. 

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
