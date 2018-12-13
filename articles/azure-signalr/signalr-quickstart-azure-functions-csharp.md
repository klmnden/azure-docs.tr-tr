---
title: Azure SignalR hizmeti sunucusuz hızlı başlangıç-C#
description: Azure SignalR Hizmetini ve Azure İşlevlerini kullanarak bir sohbet odası oluşturmaya yönelik hızlı başlangıç.
author: sffamily
ms.service: signalr
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 09/23/2018
ms.author: zhshang
ms.openlocfilehash: db54282563acc6afac4dada7a45f6dff0716879a
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2018
ms.locfileid: "53255066"
---
# <a name="quickstart-create-a-chat-room-with-azure-functions-and-signalr-service-using-c"></a>Hızlı Başlangıç: SignalR C kullanarak Service ve Azure işlevleri ile sohbet odası oluşturamadı.\#

Azure SignalR hizmeti uygulamanıza kolayca gerçek zamanlı işlevsellik eklemenizi sağlar. Azure İşlevleri, herhangi bir altyapı yönetimine gerek kalmadan kodunuzu çalıştırmanıza olanak tanıyan sunucusuz bir platformdur. Bu hızlı başlangıçta, SignalR Hizmeti ve İşlevlerini sunucusuz ve gerçek zamanlı bir sohbet uygulaması oluşturmak için kullanmayı öğrenin.

## <a name="prerequisites"></a>Önkoşullar

Henüz Visual Studio 2017’yi yüklemediyseniz, **ücretsiz** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/)’ı indirip kullanabilirsiniz. Visual Studio kurulumu sırasında **Azure dağıtımını** etkinleştirdiğinizden emin olun.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="log-in-to-azure"></a>Azure'da oturum açma

Azure hesabınızla Azure portalında <https://portal.azure.com/> sayfasında oturum açın.

[!INCLUDE [Create instance](includes/signalr-quickstart-create-instance.md)]

[!INCLUDE [Clone application](includes/signalr-quickstart-clone-application.md)]

## <a name="configure-and-run-the-azure-function-app"></a>Azure İşlev Uygulamasını yapılandırıp çalıştırma

1. Visual Studio'yu başlatın ve kopyalanmış deponun *chat\src\csharp* klasöründeki çözümü açın.

1. Azure portalın açık olduğu tarayıcıda portalın üst kısmındaki arama kutusundan adını arayarak önceden dağıttığınız SignalR Hizmeti örneğinin başarılı bir şekilde oluşturulduğundan emin olun. Açmak için örneği seçin.

    ![SignalR Hizmeti örneğini arayın](media/signalr-quickstart-azure-functions-csharp/signalr-quickstart-search-instance.png)

1. SignalR Hizmeti örneğinin bağlantı dizelerini görüntülemek için **Anahtarlar**’ı seçin.

1. Birincil bağlantı dizesini seçerek kopyalayın.

1. Visual Studio’ya dönerek Çözüm Gezgininde *local.settings.sample.json* dosyasının adını *local.settings.json* olarak değiştirin.

1. **local.settings.json** dosyasının içinde, bağlantı dizesini **AzureSignalRConnectionString** ayarının değerine yapıştırın. Dosyayı kaydedin.

1. **Functions.cs**’yi açın. Bu işlev uygulamasında iki adet HTTP ile tetiklenen işlev vardır:

    - **GetSignalRInfo** - Geçerli bağlantı bilgileri döndürmek için *SignalRConnectionInfo* giriş bağlamasını kullanır.
    - **SendMessage** - İstek gövdesinde bir sohbet iletisi alır ve iletiyi bağlı olan tüm istemci uygulamalara yaymak için *SignalR* çıkış bağlamasını kullanır.

1. Uygulamayı çalıştırmak için **Hata Ayıklama** menüsünde **Hata Ayıklamayı Başlat**’ı seçin.

    ![Uygulamada hata ayıklama](media/signalr-quickstart-azure-functions-csharp/signalr-quickstart-debug-vs.png)

[!INCLUDE [Run web application](includes/signalr-quickstart-run-web-application.md)]

[!INCLUDE [Cleanup](includes/signalr-quickstart-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, VS Code'da gerçek zamanlı ve sunucusuz bir uygulama oluşturup çalıştırdınız. Bir sonraki adımda Azure İşlevlerini nasıl VS Code’dan dağıtacağınızı öğrenin.

> [!div class="nextstepaction"]
> [VS Code ile Azure İşlevlerini dağıtma](https://code.visualstudio.com/tutorials/functions-extension/getting-started)