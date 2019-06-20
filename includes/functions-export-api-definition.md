---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: 49ac1a7585ddf2a6500c7e9382880109c3f7f431
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188142"
---
## <a name="export-an-api-definition"></a>Bir API tanımını dışarı aktarma
Gelen işleviniz için bir Openapı tanımına sahip [bir işlev için Openapı tanımı oluşturma](../articles/azure-functions/functions-openapi-definition.md). Bu işlem bir sonraki adım, PowerApps ve Microsoft Flow özel bir API kullanabilirsiniz, böylece API tanımını dışa aktarma sağlamaktır.

> [!IMPORTANT]
> Azure'a powerapps Hizmetinizine ilişkin kullanırsınız ve Microsoft Flow Kiracı kimlik bilgileri ile oturum açmanız gerekir olduğunu unutmayın. Bu özel API oluşturma ve PowerApps ve Microsoft Flow için kullanılabilmesi Azure sağlar.

1. İçinde [Azure portalında](https://portal.azure.com), işlev uygulamanızın adı tıklayın (gibi **function-demo-energy**) > **Platform özellikleri** > **API tanımı** .

    ![API tanımı](media/functions-export-api-definition/api-definition.png)

1. Tıklayın **PowerApps + Flow'a dışarı aktarma**.

    ![API tanımı kaynağı](media/functions-export-api-definition/export-api-1.png)

1. Sağ bölmede, tabloda belirtilen ayarları kullanın.

    |Ayar|Açıklama|
    |--------|------------|
    |**Dışarı aktarma modu**|Seçin **Express** özel API'yi otomatik olarak oluşturulacak. Seçme **el ile** dışarı aktarmaları API tanımı, ancak ardından gerekir almak, PowerApps ve Microsoft Flow el ile. Daha fazla bilgi için [dışarı aktarmak için PowerApps ve Microsoft Flow](../articles/azure-functions/app-service-export-api-to-powerapps-and-flow.md).|
    |**Ortam**|Özel API'yi kaydedilmesi gereken ortamı seçin. Daha fazla bilgi için [ortamlara genel bakış (PowerApps)](https://powerapps.microsoft.com/tutorials/environments-overview/) veya [ortamlara genel bakış (Microsoft Flow)](https://us.flow.microsoft.com/documentation/environments-overview-admin/).|
    |**Özel API adı**|Gibi bir ad girin `Turbine Repair`.|
    |**API anahtarı adı**|Özel API Arabiriminde uygulama ve akış oluşturucular görmelisiniz adı girin. Örnek yararlı bilgiler içerdiğini unutmayın.|
 
    ![PowerApps ve Microsoft Flow’a dışarı aktarma](media/functions-export-api-definition/export-api-2.png)

1. **Tamam** düğmesine tıklayın. Özel API'yi yerleşik ve belirttiğiniz ortama eklenir.