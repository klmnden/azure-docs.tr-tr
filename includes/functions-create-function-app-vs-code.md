---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 11/27/2018
ms.author: glenga
ms.openlocfilehash: f9b853f0e86fc298ab35421928492ba813d85827
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67444609"
---
## <a name="create-an-azure-functions-project"></a>İşlevler projeniz olan bir işlev oluşturma 

Visual Studio Code'daki Azure İşlevleri proje şablonu, Azure'daki bir işlev uygulamasında yayımlanabilen bir proje oluşturur. İşlev uygulaması, kaynakların yönetilmesi, dağıtılması ve paylaşılması için işlevleri bir mantıksal birim olarak gruplandırmanıza olanak tanır.

1. Visual Studio Code'da komut paletini açın için F1 tuşuna basın. Arayın ve seçin komut Paleti'nde `Azure Functions: Create new project...`.

1. Proje çalışma alanınız için bir dizin konumunu seçin ve **seçin**.

    > [!NOTE]
    > Bu adımları bir çalışma alanı dışında tamamlanması için tasarlanmıştır. Bu örnekte, bir çalışma alanının parçası olan bir proje klasörünü seçmeyin.

1. Komut istemlerini, aşağıdaki bilgileri sağlayın:

    | İstem | Değer | Açıklama |
    | ------ | ----- | ----------- |
    | İşlev uygulaması projenizi için bir dil seçin | C# veya JavaScript | Bu makalede destekler C# ve JavaScript. Python için bkz: [Python makalede](https://code.visualstudio.com/docs/python/tutorial-azure-functions)ve PowerShell için bkz: [PowerShell makalede](../articles/azure-functions/functions-create-first-function-powershell.md).  |
    | Projenizin ilk işlevi için bir şablon seçin | HTTP tetikleyicisi | Yeni işlev uygulamasına bir HTTP ile tetiklenen işlevi oluşturun. |
    | Bir işlev adı girin | HttpTrigger | Varsayılan adı kullanacak şekilde Enter tuşuna basın. |
    | Bir ad sağlayın | My.Functions | (C# yalnızca) C# sınıf kitaplıkları, bir ad alanı olmalıdır.  |
    | Yetkilendirme düzeyi | İşlev | Gerektiren bir [işlev anahtarı](../articles/azure-functions/functions-bindings-http-webhook.md#authorization-keys) işlevin HTTP uç noktasını çağırmak için. |
    | Projenizi nasıl istediğinizi seçin | çalışma alanına ekleme | İşlev uygulaması, geçerli çalışma alanında oluşturur. |

Visual Studio Code işlev uygulaması projesini yeni bir çalışma alanında oluşturur. Bu proje [host.json](../articles/azure-functions/functions-host-json.md) ve [local.settings.json](../articles/azure-functions/functions-run-local.md#local-settings-file) yapılandırma dosyaları ve tüm dile özgü proje dosyalarını içerir. 

Yeni bir HTTP ile tetiklenen işlev da işlev uygulaması projesi HttpTrigger klasöründe oluşturulur.