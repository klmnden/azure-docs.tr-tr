---
title: include dosyası
description: include dosyası
services: functions
author: jeffhollan
ms.service: azure-functions
ms.topic: include
ms.date: 04/01/2019
ms.author: jehollan, glenga
ms.custom: include file
ms.openlocfilehash: 0f3303e7bc87ca0bd29f367405372568ed6da7a7
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188526"
---
1. [Azure Portal](https://portal.azure.com) gidin.

1. Seçin **+ kaynak Oluştur** seçin sol tarafta **işlev uygulaması**.

1. İçin **barındırma planı**, seçin **App Service planı**, ardından **App Service planı/konumu**.

    ![İşlev uygulaması oluşturma](./media/functions-premium-create/create-function-app-resource.png)

1. Seçin **Yeni Oluştur**, türü bir **App Service planı** ad öğesini bir **konumu** içinde bir [bölge](https://azure.microsoft.com/regions/) yakınınızdaki veya diğer yakın işlevlerinizi Hizmetleri erişim ve ardından **fiyatlandırma katmanı**.

    ![App Service planı oluşturma](./media/functions-premium-create/new-app-service-plan.png)

1. Seçin **EP1** (esnek Premium) planlayın ve ardından **Uygula**.

    ![Premium planı seçin](./media/functions-premium-create/hosting-plan.png) 

1. Seçin **Tamam** görüntünün altındaki tabloda belirtilen planı oluşturmak ve ardından olarak kalan işlev uygulaması ayarlarını kullanın. 

    ![Tamamlanmış uygulama hizmeti planı](./media/functions-premium-create/create-function-app.png)  

    | Ayar      | Önerilen değer  | Açıklama                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Uygulama adı** | Genel olarak benzersiz bir ad | Yeni işlev uygulamanızı tanımlayan ad. Geçerli karakterler: `a-z`, `0-9`, ve `-`.  | 
    | **Abonelik** | Aboneliğiniz | Bu yeni işlev uygulamasının oluşturulduğu abonelik. |
    | **[Kaynak Grubu](../articles/azure-resource-manager/resource-group-overview.md)** |  myResourceGroup | İşlev uygulamanızın oluşturulacağı yeni kaynak grubunun adı. Önerilen değer de kullanabilirsiniz. |
    | **OS** | Windows | Linux üzerinde Premium planı şu anda desteklenmiyor. |
    | **Çalışma zamanı yığını** | Tercih edilen dil | Tercih ettiğiniz işlev programlama dilini destekleyen bir çalışma zamanı seçin. C# ve F# için **.NET** işlevlerini seçin. Yalnızca seçtiğiniz üzerinde desteklenen dilleri **işletim sistemi** görüntülenir. |
    | **[Depolama](../articles/storage/common/storage-quickstart-create-account.md)** |  Genel olarak benzersiz bir ad |  İşlev uygulamanız tarafından kullanılan bir depolama hesabı oluşturun. Depolama hesabı adları 3 ile 24 karakter arasında olmalı ve yalnızca sayıyla küçük harf içermelidir. Dilerseniz [depolama hesabı gereksinimlerini](../articles/azure-functions/functions-scale.md#storage-account-requirements) karşılayan mevcut bir hesap da kullanabilirsiniz. |
    | **[Application Insights](../articles/azure-functions/functions-monitoring.md)** | Varsayılan | Bir Application Insights kaynağı aynı oluşturur *uygulama adı* , desteklenen en yakın bölgesinde. Bu ayar genişleterek değiştirebilirsiniz **yeni kaynak adı** veya farklı bir seçim **konumu** içinde bir [her Azure coğrafyası](https://azure.microsoft.com/global-infrastructure/geographies/) istediğiniz verileri depolamak. |

1. Ayarlarınızı doğrulandıktan sonra Seç **Oluştur**.

1. Portalın sağ üst köşesindeki Bildirim simgesini seçin ve **Dağıtım başarılı** iletisini bekleyin.

    ![Yeni işlev uygulaması ayarlarını tanımlama](./media/functions-premium-create/function-app-create-notification.png)

1. Yeni işlev uygulamanızı görüntülemek için **Kaynağa git**’i seçin. Belirleyebilirsiniz **panoya Sabitle**. Sabitleme, bu işlev uygulaması kaynağa panonuzdan döndürülecek kolaylaştırır.