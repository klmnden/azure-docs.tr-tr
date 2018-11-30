---
title: Azure'da ilk Linux işlevinizi oluşturma
description: Azure Functions Core Tools ve Azure CLI'yi kullanarak Azure'da Linux üzerinde barındırılan ilk işlevinizi oluşturmayı öğrenin.
services: functions
keywords: ''
author: ggailey777
ms.author: glenga
ms.date: 11/28/2018
ms.topic: quickstart
ms.service: azure-functions
ms.custom: mvc
ms.devlang: multiple
manager: jeconnoc
ms.openlocfilehash: 49845cb3b07076c566ea046fca49a72812857fbf
ms.sourcegitcommit: eba6841a8b8c3cb78c94afe703d4f83bf0dcab13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2018
ms.locfileid: "52619988"
---
# <a name="create-your-first-function-hosted-on-linux-using-core-tools-and-the-azure-cli-preview"></a>Core Tools ve Azure CLI'yi kullanarak Linux’ta barındırılan ilk işlevinizi oluşturma (önizleme)

Azure işlevleri, öncelikle bir VM oluşturmak veya bir web uygulaması yayımlamak zorunda kalmadan kodunuzu bir Linux ortamı yürütmesine olanak sağlar. Linux barındırma şu anda Önizleme aşamasında olan gerektirir [işlevler 2.0 çalışma zamanını](functions-versions.md)ve yalnızca çalışan bir [App Service planı](functions-scale.md#app-service-plan).

Bu hızlı başlangıç makalesi, Azure CLI ile Linux üzerinde çalışan ilk işlev uygulamanızı oluşturma adımlarını göstermektedir. İşlev kodu yerel ortamda oluşturulur ve ardından [Azure Functions Core Tools](functions-run-local.md) ile Azure'a dağıtılır.

Aşağıdaki adımlar Mac, Windows veya Linux bilgisayarlarda desteklenir. Bu makalede JavaScript veya C# ile işlev oluşturma adımları gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

Bu örneği çalıştırmadan önce aşağıdakilere sahip olmanız gerekir:

+ [Azure Core Tools sürüm 2.x](functions-run-local.md#v2) yükleme.

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
+ `node`: bir JavaScript projesi oluşturur.

```output
Writing .gitignore
Writing host.json
Writing local.settings.json
Initialized empty Git repository in C:/functions/MyFunctionProj/.git/
```

[!INCLUDE [functions-create-function-core-tools](../../includes/functions-create-function-core-tools.md)]

[!INCLUDE [functions-update-function-code](../../includes/functions-update-function-code.md)]

[!INCLUDE [functions-run-function-test-local](../../includes/functions-run-function-test-local.md)]

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-linux-app-service-plan"></a>Bir Linux App Service planı oluşturma

[!INCLUDE [app-service-plan-no-h](../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

## <a name="create-a-linux-function-app-in-azure"></a>Azure'da bir Linux işlev uygulaması oluşturma

Linux’ta işlevlerinizin yürütülmesini barındıran bir işlev uygulamasına sahip olmanız gerekir. İşlev uygulaması, işlev kodunuzun yürütülmesi için sunucusuz bir ortam sağlar. Kaynakların daha kolay yönetilmesi, dağıtılması ve paylaşılması için işlevleri bir mantıksal birim olarak gruplandırmanıza olanak tanır. [az functionapp create](/cli/azure/functionapp#az_functionapp_create) komutunu kullanarak Linux üzerinde çalışan bir işlev uygulaması oluşturun.

Aşağıdaki komutta benzersiz bir işlev uygulamasının adını `<app_name>` yer tutucusunun ve `<storage_name>` depolama hesabı adının yerine ekleyin. `<app_name>` aynı zamanda işlev uygulamasının varsayılan DNS etki alanıdır. Bu ad Azure'daki tüm uygulamalar arasında benzersiz olmalıdır. Ayrıca ayarlamalısınız `<language>` işlev uygulamanız için çalışma zamanı gelen `dotnet` (C#) veya `node` (JavaScript).

```azurecli-interactive
az functionapp create --resource-group myResourceGroup --plan myAppServicePlan \
--name <app_name> --storage-account  <storage_name> --runtime <language>
```

> [!NOTE]
> Linux dışında App Service uygulamalarının bulunduğu `myResourceGroup` adlı bir kaynak grubunuz varsa farklı bir kaynak grubu kullanmanız gerekir. Windows ve Linux uygulamalarını aynı kaynak grubunda barındıramazsınız.  

İşlev uygulaması oluşturulduktan sonra şu ileti görüntülenir:

```output
Your serverless Linux function app 'myfunctionapp' has been successfully created.
To active this function app, publish your app content using Azure Functions Core Tools or the Azure portal.
```

Artık projenizi Azure'daki yeni işlev uygulamasında yayımlayabilirsiniz.

[!INCLUDE [functions-publish-project](../../includes/functions-publish-project.md)]

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede varsayılan Azure App Service kapsayıcısında işlev uygulaması barındırma adımları gösterildi. İşlevlerinizi Linux’ta kendi özel kapsayıcınızda da barındırabilirsiniz.

> [!div class="nextstepaction"] 
> [Linux üzerinde özel görüntü kullanarak bir işlev oluşturma](functions-create-function-linux-custom-image.md)
