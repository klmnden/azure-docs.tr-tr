---
title: Tetikleyici Azure işlevleri Azure IOT Central Web kancalarını kullanma
description: Azure IOT Central içinde bir kural her tetiklenişinde çalışan bir işlev uygulaması oluşturun.
author: viv-liu
ms.author: viviali
ms.date: 03/26/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 0d92e9bdf8ec207e5ef0e3f891c162182b5a4fff
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59264854"
---
# <a name="trigger-azure-functions-using-webhooks-in-azure-iot-central"></a>Tetikleyici Azure işlevleri Azure IOT Central Web kancalarını kullanma

*Bu konu, Oluşturucular ve Yöneticiler için geçerlidir.*

Azure işlevleri, sunucusuz bir kod Web kancası çıktı IOT Central kurallarını çalıştırmak için kullanın. Bir VM sağlama veya Azure işlevleri'ni kullanmak için bir web uygulaması yayımlamak zorunda değilsiniz ancak bunun yerine bu kod sunucusuz çalıştırabilirsiniz. SQL veritabanı veya Event Grid gibi son hedefine göndermeden önce Web kancası yükü dönüştürmek için Azure işlevlerini kullanın.

## <a name="prerequisites"></a>Önkoşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="how-to-connect-azure-functions"></a>Azure işlevleri bağlama

1. [Azure portalında yeni işlev uygulaması oluşturma](https://ms.portal.azure.com/#create/Microsoft.FunctionApp).

    ![Azure portalında yeni işlev uygulaması oluşturma](media/howto-trigger-azure-functions/createfunction.png)

2. İşlev uygulamanızı genişletin ve seçin **+ düğmesini** işlevleri yanında. Bu işlev, işlev uygulamanızdaki ilk öğe, seçin **portalında** geliştirme ortamı olarak **devam**.

    ![Özel işlev işlev uygulaması seçin](media/howto-trigger-azure-functions/customfunction.png)

3. Seçin **Web kancası + API** şablonu seçip alt **Oluştur**. Bu konuda, .NET tabanlı Azure işlevini kullanır.

    ![Genel Web kancası tetikleyicisini seçin](media/howto-trigger-azure-functions/genericwebhooktrigger.png)

4. Yeni işlevinizde seçin **<> / işlev URL'sini Al**değeri kaydetmek ve kopyalayıp. Bu değeri, web kancasını yapılandırmak için kullanırsınız.

    ![İşlev URL'sini Al](media/howto-trigger-azure-functions/getfunctionurl.png)

4. IOT Central'ın içinde işlev uygulamanıza bağlamak istediğiniz kuralı gidin.

5. Bir Web kancası eylemi ekleyin. Girin bir **görünen ad** ve daha önce kopyaladığınız içine işlev URL'sini yapıştırın **geri çağırma URL'si**.

    ![Geri çağırma URL'si alanına işlev URL'sini girin](media/howto-trigger-azure-functions/configurewebhook.PNG)

6. Kural kaydedin. Artık kuralı tetiklendiğinde çalıştırılacak bir işlev uygulaması Web kancasını çağırır. İşlev uygulamanızda seçtiğiniz **İzleyici** işlev çağırma geçmişini görmek için. App Insights ya da Klasik Görünüm geçmişinize aramak için kullanabilirsiniz.

    ![İşlev çağırma geçmişini izleme](media/howto-trigger-azure-functions/monitorfunction.PNG)

Hakkında daha fazla bilgi için Azure işlevleri makaleyi ziyaret edin [Genel Web kancası tarafından tetiklenen bir işlev oluşturma](https://docs.microsoft.com/azure/azure-functions/functions-create-generic-webhook-triggered-function).

## <a name="next-steps"></a>Sonraki adımlar
Ayarlama ve Web kancalarını kullanma öğrendiniz, önerilen sonraki keşfetmek için adımdır [Microsoft Flow, iş akışları oluşturarak](howto-add-microsoft-flow.md).
