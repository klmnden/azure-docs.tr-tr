---
title: Azure'da ilk Linux işlevinizi oluşturma
description: Azure Functions Core Tools ve Azure CLI'yi kullanarak Azure'da Linux üzerinde barındırılan ilk işlevinizi oluşturmayı öğrenin.
services: functions
keywords: ''
author: ggailey777
ms.author: glenga
ms.date: 03/12/2019
ms.topic: quickstart
ms.service: azure-functions
ms.custom: mvc, fasttrack-edit
ms.devlang: javascript
manager: jeconnoc
ms.openlocfilehash: f79fbc46104eb886a758802aff79a46feb4f5a3b
ms.sourcegitcommit: e7d4881105ef17e6f10e8e11043a31262cfcf3b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2019
ms.locfileid: "64866626"
---
# <a name="create-your-first-function-hosted-on-linux-using-core-tools-and-the-azure-cli-preview"></a>Core Tools ve Azure CLI'yi kullanarak Linux’ta barındırılan ilk işlevinizi oluşturma (önizleme)

Azure İşlevleri, öncelikle bir VM oluşturmak veya bir web uygulaması yayımlamak zorunda kalmadan kodunuzu [sunucusuz](https://azure.com/serverless) bir Linux ortamında yürütmenize olanak tanır. Linux barındırma gerektirir [işlevler 2.0 çalışma zamanını](functions-versions.md). Linux'ta sunucusuz bir işlev uygulaması çalıştırmak için destek [tüketim planı](functions-scale.md#consumption-plan) şu anda Önizleme aşamasındadır. Daha fazla bilgi için bkz. [Önizleme konuları makalede](https://aka.ms/funclinux).

Bu hızlı başlangıç makalesi, Azure CLI ile Linux üzerinde çalışan ilk işlev uygulamanızı oluşturma adımlarını göstermektedir. İşlev kodu yerel ortamda oluşturulur ve ardından [Azure Functions Core Tools](functions-run-local.md) ile Azure'a dağıtılır.

Aşağıdaki adımlar Mac, Windows veya Linux bilgisayarlarda desteklenir. Bu makalede JavaScript veya C# ile işlev oluşturma adımları gösterilmektedir. Python işlevleri oluşturmayı öğrenmek için bkz: [temel araçları ve Azure CLI (Önizleme) kullanarak ilk Python uygulamanızı oluşturma](functions-create-first-function-python.md).

## <a name="prerequisites"></a>Önkoşullar

Bu örneği çalıştırmadan önce aşağıdakilere sahip olmanız gerekir:

- Yükleme [Azure işlevleri çekirdek Araçları](./functions-run-local.md#v2) 2.6.666 sürüm veya üstü.

+ [Azure CLI]( /cli/azure/install-azure-cli)’yi yükleyin. Bu makale, Azure CLI 2.0 veya sonraki bir sürümü gerektirir. Kullandığınız sürümü bulmak için `az --version` komutunu çalıştırın. [Azure Cloud Shell](https://shell.azure.com/bash)’i de kullanabilirsiniz.

+ Etkin bir Azure aboneliği.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-the-local-function-app-project"></a>Yerel işlev uygulaması projesi oluşturma

Geçerli yerel dizinin `MyFunctionProj` klasöründe bir işlev uygulaması projesi oluşturmak için komut satırından aşağıdaki komutu çalıştırın. `MyFunctionProj`’de bir GitHub deposu da oluşturulur.

```bash
func init MyFunctionProj
```

İstendiğinde, aşağıdaki dil seçimlerinden bir worker çalışma zamanı seçmek için ok tuşlarını kullanın:

+ `dotnet`: yeni bir .NET Core sınıf kitaplığı projesi (.csproj) oluşturur.
+ `node`: bir JavaScript veya TypeScript projesi oluşturur. İstendiğinde, `JavaScript`.
+ `python`: bir Python projesi oluşturur. Python işlevleri için [Python hızlı](functions-create-first-function-python.md).

Komut yürütüldüğünde, aşağıdaki çıktıya benzer bir şey görürsünüz:

```output
Writing .gitignore
Writing host.json
Writing local.settings.json
Initialized empty Git repository in C:/functions/MyFunctionProj/.git/
```

Yeni `MyFunctionProj` proje klasörüne gitmek için aşağıdaki komutu kullanın.

```bash
cd MyFunctionProj
```

## <a name="reference-bindings"></a>Başvuru bağlamaları

[!INCLUDE [functions-extension-bundles](../../includes/functions-extension-bundles.md)]

[!INCLUDE [functions-create-function-core-tools](../../includes/functions-create-function-core-tools.md)]

[!INCLUDE [functions-run-function-test-local](../../includes/functions-run-function-test-local.md)]

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-linux-function-app-in-azure"></a>Azure'da bir Linux işlev uygulaması oluşturma

Linux’ta işlevlerinizin yürütülmesini barındıran bir işlev uygulamasına sahip olmanız gerekir. İşlev uygulaması, işlev kodunuzun yürütülmesi için sunucusuz bir ortam sağlar. Kaynakların daha kolay yönetilmesi, dağıtılması ve paylaşılması için işlevleri bir mantıksal birim olarak gruplandırmanıza olanak tanır. [az functionapp create](/cli/azure/functionapp#az-functionapp-create) komutunu kullanarak Linux üzerinde çalışan bir işlev uygulaması oluşturun.

Aşağıdaki komutta benzersiz bir işlev uygulamasının adını `<app_name>` yer tutucusunun ve `<storage_name>` depolama hesabı adının yerine ekleyin. `<app_name>` aynı zamanda işlev uygulamasının varsayılan DNS etki alanıdır. Bu ad Azure'daki tüm uygulamalar arasında benzersiz olmalıdır. Ayrıca ayarlamalısınız `<language>` işlev uygulamanız için çalışma zamanı gelen `dotnet` (C#), `node` (JavaScript/TypeScript) veya `python`.

```azurecli-interactive
az functionapp create --resource-group myResourceGroup --consumption-plan-location westus --os-type Linux \
--name <app_name> --storage-account  <storage_name> --runtime <language>
```

İşlev uygulaması oluşturulduktan sonra şu ileti görüntülenir:

```output
Your serverless Linux function app 'myfunctionapp' has been successfully created.
To active this function app, publish your app content using Azure Functions Core Tools or the Azure portal.
```

Artık projenizi Azure'daki yeni işlev uygulamasında yayımlayabilirsiniz.

[!INCLUDE [functions-publish-project](../../includes/functions-publish-project.md)]

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

[!INCLUDE [functions-quickstart-next-steps-cli](../../includes/functions-quickstart-next-steps-cli.md)]