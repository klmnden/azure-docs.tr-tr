---
title: include dosyası
description: include dosyası
services: functions
author: ggailey777
ms.service: functions
ms.topic: include
ms.date: 04/06/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: b6cafcfe6c892cd43f056458fe3586da834c2fd1
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188134"
---
İşlevleri kolaylaştırır bir işlev uygulaması için Application Insights tümleştirmesi ekleme [Azure Portal].

1. İçinde [portalı][azure portal]seçin **tüm hizmetler > işlev uygulamaları**, işlev uygulamanızı seçin ve ardından **ApplicationInsights** pencerenin üst kısmındaki başlık

    ![Application Insights portalından etkinleştirme](media/functions-connect-new-app-insights/enable-application-insights.png)

1. Görüntünün altındaki tabloda belirtilen ayarları kullanarak bir Application Insights kaynağı oluşturun:

   ![Application Insights kaynağı oluşturma](media/functions-connect-new-app-insights/ai-general.png)

    | Ayar      | Önerilen değer  | Açıklama                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Ad** | Benzersiz uygulama adı | Aboneliğinizde benzersiz işlev uygulamanızın olarak aynı adı kullanmak daha kolaydır. | 
    | **Location** | Batı Avrupa | Mümkünse, aynı kullanın [bölge](https://azure.microsoft.com/regions/) işlev uygulamanızı veya onu yakın. |

1. **Tamam**’ı seçin. Aynı kaynak grubunda ve abonelikte işlev uygulamanızı Application Insights kaynağı oluşturulur. Oluşturma işlemi tamamlandıktan sonra Application Insights penceresini kapatın.

1. Geri işlev uygulamanızda, seçin **uygulama ayarları**, ekranı aşağı kaydırarak **uygulama ayarları**. Adlı bir ayar gördüğünüzde `APPINSIGHTS_INSTRUMENTATIONKEY`, Azure'da çalışan işlev uygulamanız için Application Insights tümleştirmesi etkin olduğunu gösterir.

[Azure Portal]: https://portal.azure.com
