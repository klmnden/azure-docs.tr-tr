---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 11/27/2018
ms.author: glenga
ms.openlocfilehash: 894ca0e78dfb75dffc124d3d25aa7a8e72adf627
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67065576"
---
## <a name="create-an-azure-functions-project"></a>Azure İşlevleri projesi oluşturma

Visual Studio Code'daki Azure İşlevleri proje şablonu, Azure'daki bir işlev uygulamasında yayımlanabilen bir proje oluşturur. İşlev uygulaması, kaynakların yönetilmesi, dağıtılması ve paylaşılması için işlevleri bir mantıksal birim olarak gruplandırmanıza olanak tanır.

1. Visual Studio Code'da Azure logosu görüntülemek için seçin **Azure: İşlevleri** alan ve yeni proje Oluştur simgesini seçin.

    ![İşlev uygulaması projesi oluşturma](./media/functions-create-function-app-vs-code/create-function-app-project.png)

1. Proje çalışma alanınız için bir konum seçin ve **Seç** seçeneğinin belirleyin.

    > [!NOTE]
    > Bu makale, bir çalışma alanının dışında tamamlanacak şekilde tasarlanmıştır. Bu örnekte, bir çalışma alanının parçası olan bir proje klasörünü seçmeyin.

1. İşlev uygulaması projenizin dilini seçin. Bu makalede JavaScript kullanılmıştır.
    ![Proje dilini seçme](./media/functions-create-function-app-vs-code/create-function-app-project-language.png)

1. Projeniz için ilk işlev için bir şablon seçin. İşleviniz için bir ad sağlayın.
    ![İlk işlevinizi seçin](./media/functions-create-function-app-vs-code/create-function-app-project-first-function.png)

1. İstendiğinde **Çalışma alanına ekle**’yi seçin.

Visual Studio Code işlev uygulaması projesini yeni bir çalışma alanında oluşturur. Bu proje [host.json](../articles/azure-functions/functions-host-json.md) ve [local.settings.json](../articles/azure-functions/functions-run-local.md#local-settings-file) yapılandırma dosyaları ve tüm dile özgü proje dosyalarını içerir. Ayrıca proje dosyasında yeni bir Git deposu edinirsiniz.