---
title: Visual Studio’yu kullanarak Azure’da ilk işlevinizi oluşturma
description: Visual Studio kullanarak HTTP tarafından tetiklenen bir Azure İşlevi oluşturup yayımlayın.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
keywords: azure işlevleri, işlevler, olay işleme, işlem, sunucusuz mimari
ms.assetid: 82db1177-2295-4e39-bd42-763f6082e796
ms.service: azure-functions
ms.devlang: multiple
ms.topic: quickstart
ms.date: 10/17/2018
ms.author: glenga
ms.custom: mvc, devcenter, vs-azure, 23113853-34f2-4f
ms.openlocfilehash: 51f6fb8c0dc1450a39173fafb0d71efc9a59a536
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60431299"
---
# <a name="create-your-first-function-using-visual-studio"></a>Visual Studio kullanarak ilk işlevinizi oluşturma

Azure İşlevleri, öncelikle bir VM oluşturmak veya bir web uygulaması yayımlamak zorunda kalmadan kodunuzu [sunucusuz](https://azure.microsoft.com/solutions/serverless/) bir ortamda yürütmenize olanak tanır.

Bu makalede, yerel olarak bir “merhaba dünya” işlevi oluşturmak ve test etmek amacıyla Azure İşlevleri için Visual Studio 2017 araçlarını nasıl kullanacağınızı öğreneceksiniz. Ardından işlev kodunu Azure’da yayımlayacaksınız. Bu araçlar, Visual Studio 2017’de Azure geliştirme iş yükünün parçası olarak kullanılabilir.

![Tarayıcıdaki işlev localhost yanıtı](./media/functions-create-your-first-function-visual-studio/functions-create-your-first-function-visual-studio-browser-local-final.png)

Bu konuda bazı temel adımları anlatan [bir video](#watch-the-video) yer alır.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için:

* [Visual Studio 2017](https://azure.microsoft.com/downloads/)’yi yükleyin ve **Azure geliştirme** iş yükünün de yüklendiğinden emin olun.

* [En son Azure İşlevleri araçlarınızın](functions-develop-vs.md#check-your-tools-version) olduğundan emin olun.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-function-app-project"></a>İşlev uygulaması projesi oluşturma

[!INCLUDE [Create a project using the Azure Functions template](../../includes/functions-vstools-create.md)]

Visual Studio bir proje oluşturur ve bu projenin içinde seçili işlev türü için ortak kod içeren bir sınıf bulunur. Metottaki **FunctionName** özniteliği işlevin adını ayarlar. **HttpTrigger** özniteliği, işlevin bir HTTP isteği tarafından tetiklenip tetiklenmediğini belirtir. Ortak kod, istek gövdesi veya sorgu dizesinde yer alan bir değeri içeren bir HTTP yanıtı gönderir. Metoda uygun öznitelikleri uygulayarak, bir işleve giriş ve çıkış bağlamaları ekleyebilirsiniz. Daha fazla bilgi için, [Azure İşlevleri C# geliştirici başvurusu](functions-dotnet-class-library.md) konusunun [Tetikleyiciler ve bağlayıcılar](functions-dotnet-class-library.md#triggers-and-bindings) bölümüne göz atın.

İşlev projenizi ve HTTP ile tetiklenen işlev oluşturduğunuza göre, artık bunu yerel bilgisayarınızda test edebilirsiniz.

## <a name="test-the-function-locally"></a>İşlevi yerel olarak test etme

Azure İşlevleri Temel Araçları, Azure İşlevleri projenizi yerel geliştirme bilgisayarınızda çalıştırmanıza olanak sağlar. Visual Studio'da ilk kez bir işlev başlattığınızda bu araçları yüklemeniz istenir.

1. İşlevinizi test etmek için F5’e basın. İstenirse Visual Studio'dan gelen Azure İşlevleri Temel (CLI) araçlarını indirme ve yükleme isteğini kabul edin. Aracın HTTP isteklerini işleyebilmesi için bir güvenlik duvarı özel durumu etkinleştirmeniz de gerekebilir.

2. Azure İşlevleri çalışma zamanı çıktısından işlevinizin URL'sini kopyalayın.

    ![Azure yerel çalışma zamanı](./media/functions-create-your-first-function-visual-studio/functions-create-your-first-function-visual-studio-debugging.png)

3. HTTP isteğinin URL’sini tarayıcınızın adres çubuğuna yapıştırın. `?name=<YOUR_NAME>` sorgu dizesini bu URL’ye ekleyip isteği yürütün. İşlevin döndürdüğü yerel GET isteğine tarayıcıda verilen yanıt aşağıda gösterilmiştir: 

    ![Tarayıcıdaki işlev localhost yanıtı](./media/functions-create-your-first-function-visual-studio/functions-create-your-first-function-visual-studio-browser-local.png)

4. Hata ayıklamayı durdurmak için **Shift + F5** tuşuna basın.

İşlevin yerel bilgisayarınızda düzgün çalıştığını doğruladıktan sonra, projeyi Azure'da yayımlamanın zamanı gelmiştir.

## <a name="publish-the-project-to-azure"></a>Projeyi Azure'da yayımlama

Projenizi yayımlayabilmeniz için önce Azure aboneliğinizde bir işlev uygulamanızın olması gerekir. Visual Studio'dan bir işlev uygulaması oluşturabilirsiniz.

[!INCLUDE [Publish the project to Azure](../../includes/functions-vstools-publish.md)]

## <a name="test-your-function-in-azure"></a>Azure'da işlevinizi test etme

1. Yayımlama profili sayfasından işlev uygulamasının temel URL'sini kopyalayın. İşlevi yerel olarak test ederken kullandığınız URL’nin `localhost:port` kısmını, yeni temel URL ile değiştirin. Daha önce olduğu gibi, `?name=<YOUR_NAME>` sorgu dizesini bu URL’ye ekleyip isteği yürütmeyi unutmayın.

    HTTP tarafından tetiklenen işlevinizi çağıran URL aşağıdaki biçimde olmalıdır:

        http://<APP_NAME>.azurewebsites.net/api/<FUNCTION_NAME>?name=<YOUR_NAME> 

2. HTTP isteğinin yeni URL’sini tarayıcınızın adres çubuğuna yapıştırın. İşlevin döndürdüğü uzak GET isteğine tarayıcıda verilen yanıt aşağıda gösterilmiştir:

    ![Tarayıcıdaki işlev yanıtı](./media/functions-create-your-first-function-visual-studio/functions-create-your-first-function-visual-studio-browser-azure.png)

## <a name="watch-the-video"></a>Videoyu izleme

> [!VIDEO https://www.youtube-nocookie.com/embed/DrhG-Rdm80k]

## <a name="next-steps"></a>Sonraki adımlar

HTTP ile tetiklenen basit bir işlevi kullanarak C# işlev uygulaması oluşturup yayımlamak için Visual Studio’yu kullandınız.

* [Diğer hizmetlerle tümleşen giriş ve çıkış bağlamalarının nasıl ekleneceğini öğrenin.](functions-develop-vs.md#add-bindings)
* [.NET sınıfı kitaplıkları olarak işlev geliştirme hakkında daha fazla bilgi edinin](functions-dotnet-class-library.md).
