---
title: include dosyası
description: include dosyası
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 03/25/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 710bb8cba7fbbe4bc9b9fdc52b0767c96f97fe72
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188990"
---
## <a name="create-base-resources"></a>Temel kaynaklar oluşturma

İleti yönlendirme yapılandırmadan önce bir IOT hub'ı, bir depolama hesabı ve Service Bus kuyruğuna oluşturmanız gerekir. Bu kaynaklar, kullanılabilir dört makalelerden birine Bu öğreticinin için 1 kullanılarak oluşturulabilir: Azure CLI, Azure PowerShell, Azure portalında veya Azure Resource Manager şablonu.

Tüm kaynaklar için aynı kaynak grubunu ve konumunu kullanın. Ardından sonunda, tüm kaynakları tek bir adımda kaynak grubunu silerek kaldırabilirsiniz.

Aşağıdaki bölümlerde, gerçekleştirilmesi gereken adımlar açıklanmaktadır.

1. Bir [kaynak grubu](../articles/azure-resource-manager/resource-group-overview.md) oluşturun.

2. S1 katmanında IoT hub'ı oluşturun. IOT hub'ınızı bir tüketici grubu ekleyin. Tüketici grubu Azure Stream Analytics tarafından veriler alınırken kullanılır.

   > [!NOTE]
   > Bu öğreticiyi tamamlamak için ücretli bir katmanda bir IOT hub'ı kullanmanız gerekir. Ücretsiz katman yalnızca bir uç noktasını ayarlamanıza olanak tanır ve Bu öğretici için birden fazla uç noktası gereklidir.
   > 

3. Standard_LRS çoğaltmasıyla standart bir V1 depolama hesabı oluşturun.

4. Service Bus ad alanı ve kuyruğu oluşturun.

5. Hub'ınıza iletiler gönderen simülasyon cihazı için cihaz kimliği oluşturun. Test aşaması için anahtarı kaydedin. (Resource Manager şablonu oluşturuyorsanız, bu şablonu dağıttıktan sonra gerçekleştirilir.)