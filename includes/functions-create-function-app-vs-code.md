---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 11/27/2018
ms.author: glenga
ms.openlocfilehash: 79dbee33928fbc7560d0ea27be3af25cc510e996
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66132269"
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

1. İstendiğinde **Çalışma alanına ekle**’yi seçin.

Visual Studio Code işlev uygulaması projesini yeni bir çalışma alanında oluşturur. Bu proje [host.json](../articles/azure-functions/functions-host-json.md) ve [local.settings.json](../articles/azure-functions/functions-run-local.md#local-settings-file) yapılandırma dosyaları ve tüm dile özgü proje dosyalarını içerir. Ayrıca proje dosyasında yeni bir Git deposu edinirsiniz.