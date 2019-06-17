---
author: dlepow
ms.service: container-registry
ms.topic: include
ms.date: 05/02/2019
ms.author: danlep
ms.openlocfilehash: 40cc1856a5e943ca5596e7d11712febadd30e3ec
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67133698"
---
## <a name="prerequisites"></a>Önkoşullar

### <a name="get-sample-code"></a>Örnek kodu alma

Bu öğreticide, [önceki öğreticide](../articles/container-registry/container-registry-tutorial-quick-task.md) yer alan adımları zaten tamamladığınız ve örnek deponun çatalını ve kopyasını oluşturduğunuz varsayılır. Henüz yapmadıysanız, devam etmeden önce önceki öğreticinin [Önkoşullar](../articles/container-registry/container-registry-tutorial-quick-task.md#prerequisites) bölümündeki adımları tamamlayın.

### <a name="container-registry"></a>Kapsayıcı kayıt defteri

Bu öğreticiyi tamamlamak için Azure aboneliğinizde bir Azure kapsayıcı kayıt defteri olması gerekir. Bir kayıt defteri gerekirse bkz [önceki öğreticide](../articles/container-registry/container-registry-tutorial-quick-task.md), veya [hızlı başlangıç: Azure CLI kullanarak bir kapsayıcı kayıt defteri oluşturma](../articles/container-registry/container-registry-get-started-azure-cli.md).

## <a name="create-a-github-personal-access-token"></a>GitHub kişisel erişim belirteci oluşturma

Bir Git deposuna bir işleme görevde tetiklemek için kişisel erişim belirteci (PAT) depoya erişmek için ACR görevleri gerekir. Bir PAT zaten yoksa, bir GitHub oluşturmak için aşağıdaki adımları izleyin:

1. GitHub üzerinde https://github.com/settings/tokens/new adresindeki PAT oluşturma sayfasında gidin
1. Belirteç için kısa bir **açıklama** girin; örneğin, "ACR Görevleri Tanıtımı"
1. ACR depoya erişmek kapsamları seçin. Bu öğreticide, olduğu gibi genel deponun altında erişmeye **depo**, etkinleştirme **depo: durum** ve **public_repo**

   ![GitHub'da Kişisel Erişim Belirteci oluşturma sayfasının ekran görüntüsü][build-task-01-new-token]

   > [!NOTE]
   > Erişmek için bir PAT oluşturmak için bir *özel* depo, tam kapsam seçin **depo** denetimi.

1. **Belirteç Oluştur** düğmesini seçin (parolanızı onaylamanız istenebilir)
1. Oluşturulan belirteci kopyalayın ve **güvenli bir konuma** kaydedin (bu belirteci sonraki bölümde bir görev tanımlarken kullanacaksınız)

   ![GitHub'da oluşturulan Kişisel Erişim Belirtecinin ekran görüntüsü][build-task-02-generated-token]

<!-- Images -->
[build-task-01-new-token]: ./media/container-registry-task-tutorial-prereq/build-task-01-new-token.png
[build-task-02-generated-token]: ./media/container-registry-task-tutorial-prereq/build-task-02-generated-token.png
