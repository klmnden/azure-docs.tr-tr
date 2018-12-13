---
title: include dosyası
description: include dosyası
author: anthonychu
ms.service: signalr
ms.topic: include
ms.date: 09/14/2018
ms.author: antchu
ms.custom: include file
ms.openlocfilehash: c95f05127d62d366e1ad78b45df23d04ced0164a
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2018
ms.locfileid: "53262800"
---
## <a name="create-an-azure-signalr-service-instance"></a>Azure SignalR Hizmeti örneği oluşturma

Uygulamanız Azure’da bir SignalR hizmeti örneğine bağlanır.

1. Azure portalın sol üst köşesinde bulunan Yeni düğmesini seçin. Yeni ekranda arama kutusuna *SignalR hizmeti* yazın ve Enter tuşuna basın.

    ![SignalR Hizmetini Arama](../media/signalr-quickstart-azure-functions-javascript/signalr-quickstart-new.png)

1. Arama sonuçlarından **SignalR Hizmeti**’ni seçtikten sonra **Oluştur**’u seçin.

1. Aşağıdaki ayarları girin.

    | Ayar      | Önerilen değer  | Açıklama                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Kaynak adı** | Genel olarak benzersiz bir ad | Yeni SignalR Hizmeti örneğinizi tanımlayan ad. Geçerli karakterler: `a-z`, `0-9`, ve `-`.  | 
    | **Abonelik** | Aboneliğiniz | Yeni SignalR Hizmeti örneğinin oluşturulacağı abonelik. | 
    | **[Kaynak Grubu](../../azure-resource-manager/resource-group-overview.md)** |  myResourceGroup | SignalR Hizmeti örneğinizin oluşturulacağı yeni kaynak grubunun adı. | 
    | **Konum** | Batı ABD | Size yakın bir [bölge](https://azure.microsoft.com/regions/) seçin. |
    | **Fiyatlandırma katmanı** | Ücretsiz | Azure SignalR Hizmetini ücretsiz deneyin. |
    | **Birim sayısı** |  Uygulanamaz | Birim sayısı, SignalR Hizmeti örneğinizin kaç bağlantı kabul edebileceğini belirtir. Bu yalnızca Standart katmanda yapılandırılabilir. |

    ![SignalR Hizmeti Oluşturma](../media/signalr-quickstart-azure-functions-javascript/signalr-quickstart-create.png)

1. SignalR Hizmeti örneğini dağıtmaya başlamak için **Oluştur**’u seçin.