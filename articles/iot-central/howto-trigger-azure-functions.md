---
title: Tetikleyici Azure işlevleri Azure IOT Central Web kancalarını kullanma
description: Azure IOT Central içinde bir kural her tetiklenişinde çalışan bir işlev uygulaması oluşturun.
author: viv-liu
ms.author: viviali
ms.date: 09/06/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 7b14dc4eeec1ab543c407b1bb1cf8a5ce46f3ecb
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2018
ms.locfileid: "44357421"
---
# <a name="trigger-azure-functions-using-webhooks-in-azure-iot-central"></a>Tetikleyici Azure işlevleri Azure IOT Central Web kancalarını kullanma

Azure işlevleri, sunucusuz bir kod Web kancası çıktı IOT Central kurallarını çalıştırmak için kullanın. Bir VM sağlama veya Azure işlevleri'ni kullanmak için bir web uygulaması yayımlamak zorunda değildir ancak bunun yerine bu kod serverlessly çalıştırabilirsiniz. SQL veritabanı veya Event Grid gibi son hedefine göndermeden önce Web kancası yükü dönüştürmek için Azure işlevlerini kullanın. 

## <a name="prerequisites"></a>Önkoşullar
+ Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="how-to-connect-azure-functions"></a>Azure işlevleri bağlama

1. [Azure Portalı'nda yeni bir işlev uygulaması oluşturun. ](https://ms.portal.azure.com/#create/Microsoft.FunctionApp).
2. İşlev uygulamanızı genişletin ve tıklayın + işlevleri yanındaki düğmesi. Bu işlev, işlev uygulamanızdaki ilk öğe, özel işlevi seçin. Böylece işlev şablonlarının tamamı görüntülenir.
3. Arama alanına genel yazın ve ardından Genel Web kancası tetikleyici şablonunuz için istediğiniz dili seçin. Bu konuda bir C# işlevi kullanılmaktadır. 
4. Yeni işlevinizde, **</> İşlev URL’sini al**’a tıklayın, sonra da değeri kopyalayın ve kaydedin. Bu değeri, web kancasını yapılandırmak için kullanırsınız. 
4. IOT Central'ın içinde işlev uygulamanıza bağlamak istediğiniz kuralı bulun. 
5. Bir Web kancası eylemi ekleyin. Girin bir **görünen ad** işlevi daha önce kopyaladığınız URL'yi yapıştırın.
6. Kural kaydedin. Artık Web kancasını kuralı tetiklendiğinde çalıştırılacak bir işlev uygulaması çağıracaktır. İşlev uygulamanızda, bkz: her işlev tetiklenir ve günlükleri gözlemleyebilirsiniz.

Hakkında daha fazla bilgi için Azure işlevleri makaleyi ziyaret edin [Genel Web kancası tarafından tetiklenen bir işlev oluşturma](https://docs.microsoft.com/azure/azure-functions/functions-create-generic-webhook-triggered-function). 

## <a name="next-steps"></a>Sonraki adımlar
Ayarlama ve Web kancalarını kullanma öğrendiniz, önerilen sonraki keşfetmek için adımdır [Microsoft Flow, iş akışları oluşturarak](howto-add-microsoft-flow.md).
