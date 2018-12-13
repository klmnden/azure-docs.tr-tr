---
title: Azure SignalR hizmeti sunucusuz hızlı başlangıç - JavaScript
description: Azure SignalR Hizmetini ve Azure İşlevlerini kullanarak bir sohbet odası oluşturmaya yönelik hızlı başlangıç.
author: sffamily
ms.service: signalr
ms.devlang: javascript
ms.topic: quickstart
ms.date: 09/23/2018
ms.author: zhshang
ms.openlocfilehash: f0044ca206d15762d44d8d4ea2d58c93950c5e1e
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2018
ms.locfileid: "53252465"
---
# <a name="quickstart-create-a-chat-room-with-azure-functions-and-signalr-service-using-javascript"></a>Hızlı Başlangıç: SignalR JavaScript kullanarak hizmeti ve Azure işlevleri ile sohbet odası oluşturamadı.

Azure SignalR hizmeti uygulamanıza kolayca gerçek zamanlı işlevsellik eklemenizi sağlar. Azure İşlevleri, herhangi bir altyapı yönetimine gerek kalmadan kodunuzu çalıştırmanıza olanak tanıyan sunucusuz bir platformdur. Bu hızlı başlangıçta, SignalR Hizmeti ve İşlevlerini sunucusuz ve gerçek zamanlı bir sohbet uygulaması oluşturmak için kullanmayı öğrenin.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıç; macOS, Windows veya Linux üzerinde çalıştırılabilir.

[Visual Studio Code](https://code.visualstudio.com/) gibi bir kod editörünün yüklü olduğundan emin olun.

Azure İşlevleri uygulamalarını yerel olarak çalıştırmak için [Azure İşlevleri Çekirdek Araçları (v2)](https://github.com/Azure/azure-functions-core-tools#installing) öğesini yükleyin.

Azure İşlevleri Çekirdek Araçları, uzantı yüklemek için [.NET Core SDK'sının](https://www.microsoft.com/net/download) yüklü olmasını gerektirir. Ancak JavaScript Azure İşlev uygulamaları oluşturmak için .NET bilgisi gerekmemektedir.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="log-in-to-azure"></a>Azure'da oturum açma

Azure hesabınızla Azure portalında <https://portal.azure.com/> sayfasında oturum açın.

[!INCLUDE [Create instance](includes/signalr-quickstart-create-instance.md)]

[!INCLUDE [Clone application](includes/signalr-quickstart-clone-application.md)]

## <a name="configure-and-run-the-azure-function-app"></a>Azure İşlev Uygulamasını yapılandırıp çalıştırma

1. Azure portalın açık olduğu tarayıcıda portalın üst kısmındaki arama kutusundan adını arayarak önceden dağıttığınız SignalR Hizmeti örneğinin başarılı bir şekilde oluşturulduğundan emin olun. Açmak için örneği seçin.

    ![SignalR Hizmeti örneğini arayın](media/signalr-quickstart-azure-functions-csharp/signalr-quickstart-search-instance.png)

1. SignalR Hizmeti örneğinin bağlantı dizelerini görüntülemek için **Anahtarlar**’ı seçin.

1. Birincil bağlantı dizesini seçerek kopyalayın.

    ![SignalR Hizmeti Oluşturma](media/signalr-quickstart-azure-functions-javascript/signalr-quickstart-keys.png)

1. Kod editörünüzde kopyalanan deponun *chat/src/javascript* klasörünü açın.

1. *local.settings.sample.json* dosyasını *local.settings.json* olarak yeniden adlandırın.

1. **local.settings.json** dosyasının içinde, bağlantı dizesini **AzureSignalRConnectionString** ayarının değerine yapıştırın. Dosyayı kaydedin.

1. JavaScript işlevleri klasörler halinde düzenlenir. Her klasörde iki dosya bulunur: *function.json* işlevde kullanılan bağlamaları tanımlar ve *index.js* işlevin gövdesidir. Bu işlev uygulamasında iki adet HTTP ile tetiklenen işlev vardır:

    - **negotiate** - Geçerli bağlantı bilgileri döndürmek için *SignalRConnectionInfo* giriş bağlamasını kullanır.
    - **messages** - İstek gövdesinde bir sohbet iletisi alır ve iletiyi bağlı olan tüm istemci uygulamalara yaymak için *SignalR* çıkış bağlamasını kullanır.

1. Terminalde *src/sohbet/javascript* klasöründe olduğunuzdan emin olun. Azure İşlevleri Çekirdek Araçlarını kullanarak uygulamayı çalıştırmak için gereken uzantıları yükleyin.

    ```bash
    func extensions install
    ```

1. İşlev uygulamasını çalıştırın.

    ```bash
    func start
    ```

    ![SignalR Hizmeti Oluşturma](media/signalr-quickstart-azure-functions-javascript/signalr-quickstart-run-application.png)

[!INCLUDE [Run web application](includes/signalr-quickstart-run-web-application.md)]

[!INCLUDE [Cleanup](includes/signalr-quickstart-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Visual Studio'da gerçek zamanlı ve sunucusuz bir uygulama oluşturup çalıştırdınız. Bir sonraki adımda Visual Studio ile nasıl Azure İşlevlerini geliştirip dağıtacağınızı öğrenin.

> [!div class="nextstepaction"]
> [Visual Studio ile Azure İşlevleri geliştirme](../azure-functions/functions-develop-vs.md)