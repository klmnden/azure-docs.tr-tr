---
title: Tetikleyici Azure işlevleri Azure IOT Central Web kancalarını kullanma
description: Azure IOT Central içinde bir kural her tetiklenişinde çalışan bir işlev uygulaması oluşturun.
author: viv-liu
ms.author: viviali
ms.date: 09/17/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 694c259472bf7e18f0ae3d424acf5b5cfb7ea7ab
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2018
ms.locfileid: "46294263"
---
# <a name="trigger-azure-functions-using-webhooks-in-azure-iot-central"></a>Tetikleyici Azure işlevleri Azure IOT Central Web kancalarını kullanma

*Bu konu, Oluşturucular ve Yöneticiler için geçerlidir.*

Azure işlevleri, sunucusuz bir kod Web kancası çıktı IOT Central kurallarını çalıştırmak için kullanın. Bir VM sağlama veya Azure işlevleri'ni kullanmak için bir web uygulaması yayımlamak zorunda değildir ancak bunun yerine bu kod serverlessly çalıştırabilirsiniz. SQL veritabanı veya Event Grid gibi son hedefine göndermeden önce Web kancası yükü dönüştürmek için Azure işlevlerini kullanın. 

## <a name="prerequisites"></a>Önkoşullar
+ Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="how-to-connect-azure-functions"></a>Azure işlevleri bağlama

1. [Azure Portalı'nda yeni bir işlev uygulaması oluşturun. ](https://ms.portal.azure.com/#create/Microsoft.FunctionApp).

    ![Azure Portalı'nda yeni bir işlev uygulaması oluşturma](media/howto-trigger-azure-functions/createfunction.png)

2. İşlev uygulamanızı genişletin ve tıklayın **+ düğmesini** işlevleri yanında. Bu işlev, işlev uygulamanızdaki ilk işlevse **Özel işlev**'i seçin. Böylece işlev şablonlarının tamamı görüntülenir.
    
    ![Özel işlev işlev uygulaması seçin](media/howto-trigger-azure-functions/customfunction.png)

3. Arama alanına yazın **"Genel"** Genel Web kancası tetikleyici şablonunuz için istediğiniz dili seçin. Bu konuda bir C# işlevi kullanılmaktadır. 

    ![Genel Web kancası tetikleyicisini seçin](media/howto-trigger-azure-functions/genericwebhooktrigger.png)

4. Yeni işlevinizde, **</> İşlev URL’sini al**’a tıklayın, sonra da değeri kopyalayın ve kaydedin. Web kancasını yapılandırırken bu değeri kullanır. 

    ![İşlev URL'sini Al](media/howto-trigger-azure-functions/getfunctionurl.png)
4. IOT Central'ın içinde işlev uygulamanıza bağlamak istediğiniz kuralı gidin.

5. Bir Web kancası eylemi ekleyin. Girin bir **görünen ad** ve daha önce kopyaladığınız içine işlev URL'sini yapıştırın **geri çağırma URL'si**.

    ![Geri çağırma URL'si alanına işlev URL'sini girin](media/howto-trigger-azure-functions/configurewebhook.PNG)

6. Kural kaydedin. Artık Web kancasını kuralı tetiklendiğinde çalıştırılacak bir işlev uygulaması çağıracaktır. İşlev uygulamanızda tıklayabilirsiniz **İzleyici** işlev çağırma geçmişini görmek için. App Insights ya da Klasik Görünüm geçmişinize aramak için kullanabilirsiniz.

    ![İşlev çağırma geçmişini izleme](media/howto-trigger-azure-functions/monitorfunction.PNG)

Hakkında daha fazla bilgi için Azure işlevleri makaleyi ziyaret edin [Genel Web kancası tarafından tetiklenen bir işlev oluşturma](https://docs.microsoft.com/azure/azure-functions/functions-create-generic-webhook-triggered-function). 

## <a name="next-steps"></a>Sonraki adımlar
Ayarlama ve Web kancalarını kullanma öğrendiniz, önerilen sonraki keşfetmek için adımdır [Microsoft Flow, iş akışları oluşturarak](howto-add-microsoft-flow.md).
