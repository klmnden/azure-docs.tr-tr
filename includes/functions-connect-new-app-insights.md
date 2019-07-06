---
title: include dosyası
description: include dosyası
services: functions
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 04/06/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 9c519fc2db020b8df22275c6b276c6ec23d10b1c
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67608401"
---
İşlevleri kolaylaştırır bir işlev uygulaması için Application Insights tümleştirmesi ekleme [Azure Portal].

1. İçinde [portalı][azure portal]seçin **tüm hizmetler > işlev uygulamaları**, işlev uygulamanızı seçin ve ardından **ApplicationInsights** pencerenin üst kısmındaki başlık

    ![Application Insights portalından etkinleştirme](media/functions-connect-new-app-insights/enable-application-insights.png)

1. Görüntünün altındaki tabloda belirtilen ayarları kullanarak bir Application Insights kaynağı oluşturun:

   ![Application Insights kaynağı oluşturma](media/functions-connect-new-app-insights/ai-general.png)

    | Ayar      | Önerilen değer  | Açıklama                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Name** | Benzersiz uygulama adı | Aboneliğinizde benzersiz işlev uygulamanızın olarak aynı adı kullanmak daha kolaydır. | 
    | **Location** | Batı Avrupa | Mümkünse, aynı kullanın [bölge](https://azure.microsoft.com/regions/) işlev uygulamanızı veya onu yakın. |

1. **Tamam**’ı seçin. Aynı kaynak grubunda ve abonelikte işlev uygulamanızı Application Insights kaynağı oluşturulur. Oluşturma işlemi tamamlandıktan sonra Application Insights penceresini kapatın.

1. Geri işlev uygulamanızda, seçin **uygulama ayarları**, ekranı aşağı kaydırarak **uygulama ayarları**. Adlı bir ayar gördüğünüzde `APPINSIGHTS_INSTRUMENTATIONKEY`, Azure'da çalışan işlev uygulamanız için Application Insights tümleştirmesi etkin olduğunu gösterir.

[Azure Portal]: https://portal.azure.com
