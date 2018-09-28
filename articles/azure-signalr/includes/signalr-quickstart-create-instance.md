---
title: include dosyası
description: include dosyası
services: signalr
author: anthonychu
ms.service: signalr
ms.topic: include
ms.date: 09/14/2018
ms.author: antchu
ms.custom: include file
ms.openlocfilehash: 97b8ef5e260a853ecdffb3c502f8a5051dd0e143
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47006308"
---
## <a name="create-an-azure-signalr-service-instance"></a>Bir Azure SignalR Hizmeti örneği oluşturma

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
