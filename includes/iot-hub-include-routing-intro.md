---
title: include dosyası
description: include dosyası
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 03/05/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: d0accd01926743d64fa4911dfe56806537170c2d
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66271578"
---
[İleti yönlendirme](../articles/iot-hub/iot-hub-devguide-messages-d2c.md) blob depolama, Service Bus kuyrukları, Service Bus konu başlıklarına ve Event Hubs telemetri verilerini IOT cihazlarınızdan yerleşik Event Hub ile uyumlu uç noktalar veya özel uç noktalar gibi gönderilmesini sağlar. Özel ileti yönlendirmeyi yapılandırmak için oluşturduğunuz [yönlendirme sorguları](../articles/iot-hub/iot-hub-devguide-routing-query-syntax.md) belirli bir koşul ile eşleşen bir rota özelleştirmek için. Ayarlandıktan sonra, gelen veriler IoT Hub'ı tarafından otomatik olarak uç noktalara yönlendirilir. Bir ileti herhangi bir tanımlı yönlendirme sorgular eşleşmiyorsa, varsayılan uç noktaya yönlendirilir.

2-bir parçası olan Bu öğreticide, kurma ve IOT Hub ile bu özel yönlendirme sorguları kullanma konusunda bilgi edinin. Blob depolama ve Service Bus kuyruğuna dahil olmak üzere birden fazla uç nokta birine iletileri IOT cihazından yol. Service Bus kuyruğuna ileti bir mantıksal uygulama tarafından teslim alındı ve e-postayla gönderilir. Tanımlanan özel ileti yönlendirme olmayan iletiler varsayılan uç noktaya gönderdi sonra Azure Stream Analytics tarafından teslim alındı ve Power BI görselleştirmelerinde görüntülenen.

Tam bölümleri için 1 ve 2. Bu öğretici aşağıdaki görevleri gerçekleştirmiştir:

**Bölümü I: İleti yönlendirmeyi'kurmak, kaynakları oluşturma**
> [!div class="checklist"]
> * Kaynakları--bir IOT hub'ı, bir depolama hesabı, Service Bus kuyruğuna ve bir simülasyon cihazı oluşturun. Bu yapılabilir portalı, Azure CLI, Azure PowerShell veya Azure Resource Manager şablonu kullanarak.
> * Uç noktaları ve ileti yollarını IOT Hub'ında depolama hesabı ve Service Bus kuyruğu için yapılandırın.

**Bölüm II: Hub'ına ileti göndermek, yönlendirilmiş sonuçlarını görüntüleme**
> [!div class="checklist"]
> * Service Bus kuyruğuna ileti eklendiğinde tetiklenen ve e-posta gönderen bir Mantıksal Uygulama oluşturma.
> * Farklı yönlendirme seçenekleri için hub'a iletiler gönderen bir IoT Cihazının simülasyonu olan bir uygulama indirme ve çalıştırma.
> * Varsayılan uç noktaya gönderilen veriler için bir Power BI görselleştirmesi oluşturun.
> * Service Bus kuyruğunda ve e-postalarda ...
> * Depolama hesabında ...
> * PowerBI görselleştirmesinde ...
> * ...sonuçları görüntüleyin.

## <a name="prerequisites"></a>Önkoşullar

* Bu öğreticinin için 1:
  - Bir Azure aboneliğiniz olmalıdır. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

* Bu öğreticinin için 2:
  - Bu öğreticinin tamamlanan bölüm 1 olan ve hala kullanılabilir kaynaklara sahibiz.
  - [Visual Studio](https://www.visualstudio.com/)’yu yükleyin.
  - Varsayılan uç noktanın akış analizini yapmak için Power BI hesabı. ([Power BI'ı ücretsiz deneyin](https://app.powerbi.com/signupredirect?pbi_source=web).)
  - Bildirim e-postalarını göndermek için Office 365 hesabı.

[!INCLUDE [cloud-shell-try-it.md](cloud-shell-try-it.md)]
