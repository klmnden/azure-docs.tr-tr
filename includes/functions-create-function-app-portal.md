---
title: include dosyası
description: include dosyası
services: functions
author: ggailey777
ms.service: functions
ms.topic: include
ms.date: 07/17/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: a0f6ca72e6d3e487e85d16a6860e0d84003a9aa8
ms.sourcegitcommit: 8ebcecb837bbfb989728e4667d74e42f7a3a9352
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2018
ms.locfileid: "40260800"
---
1. Azure portalının sol üst köşesinde bulunan **Yeni** düğmesine tıklayın, ardından **İşlem** > **İşlev Uygulaması** seçeneğini belirleyin. 

    ![Azure portalında işlev uygulaması oluşturma](./media/functions-create-function-app-portal/function-app-create-flow.png)

2. Görüntünün altındaki tabloda belirtilen işlev uygulaması ayarlarını kullanın.

    ![Yeni işlev uygulaması ayarlarını tanımlama](./media/functions-create-function-app-portal/function-app-create-flow2.png)

    | Ayar      | Önerilen değer  | Açıklama                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Uygulama adı** | Genel olarak benzersiz bir ad | Yeni işlev uygulamanızı tanımlayan ad. Geçerli karakterler: `a-z`, `0-9`, ve `-`.  | 
    | **Abonelik** | Aboneliğiniz | Bu yeni işlev uygulamasının oluşturulduğu abonelik. | 
    | **[Kaynak Grubu](../articles/azure-resource-manager/resource-group-overview.md)** |  myResourceGroup | İşlev uygulamanızın oluşturulacağı yeni kaynak grubunun adı. | 
    | **OS** | Windows | Sunucusuz barındırma şu anda yalnızca Windows’ta çalıştırıldığında kullanılabilir. Linux barındırması için bkz. [Azure CLI kullanarak Linux’ta çalışan ilk işlevinizi oluşturma](../articles/azure-functions/functions-create-first-azure-function-azure-cli-linux.md). |
    | **[Barındırma planı](../articles/azure-functions/functions-scale.md)** | Tüketim planı | Kaynakların işlev uygulamanıza nasıl ayrılacağını tanımlayan barındırma planı. Varsayılan **Tüketim Planı**'nda kaynaklar işlevlerin taleplerine göre dinamik olarak eklenir. Bu [sunucusuz](https://azure.microsoft.com/overview/serverless-computing/) barındırmada, yalnızca işlevlerinizin çalıştığı süre için ödeme yaparsınız. Bir App Service planı içinde çalıştırdığınızda [işlev uygulamanızın ölçeklendirmesini](../articles/azure-functions/functions-scale.md) yönetmeniz gerekir.  |
    | **Konum** | Batı Avrupa | Kendinize veya işlevinizin erişeceği diğer hizmetlere yakın bir [bölge](https://azure.microsoft.com/regions/) seçin. |
    | **[Depolama hesabı](../articles/storage/common/storage-quickstart-create-account.md)** |  Genel olarak benzersiz bir ad |  İşlev uygulamanız tarafından kullanılan yeni depolama hesabının adı. Depolama hesabı adları 3 ile 24 karakter arasında olmalı ve yalnızca sayıyla küçük harf içermelidir. Dilerseniz [depolama hesabı gereksinimlerini](../articles/azure-functions/functions-scale.md#storage-account-requirements) karşılayan mevcut bir hesap da kullanabilirsiniz. |

3. İşlev uygulamasını sağlamak ve dağıtmak için **Oluştur**'u seçin.

4. Portalın sağ üst köşesindeki Bildirim simgesini seçin ve **Dağıtım başarılı** iletisini bekleyin. 

    ![Yeni işlev uygulaması ayarlarını tanımlama](./media/functions-create-function-app-portal/function-app-create-notification.png)

5. Yeni işlev uygulamanızı görüntülemek için **Kaynağa git**’i seçin.

> [!TIP]
> Portalda işlev uygulamalarınızı bulma konusunda sorun yaşıyorsanız, [Azure portalında İşlev Uygulamalarını sık kullanılanlarınıza eklemeyi](../articles/azure-functions/functions-how-to-use-azure-function-app-settings.md#favorite) deneyin.   
