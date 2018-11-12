---
title: include dosyası
description: IOT edge
author: kgremban
manager: timlt
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 09/18/2018
ms.author: kgremban
ms.openlocfilehash: be4d82577584e83e29f2511d51256fda0970e917
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51264400"
---
## <a name="enabling-extended-offline-operation-preview"></a>Genişletilmiş çevrimdışı işlemi (Önizleme) etkinleştirme
İle [v1.0.4 yayın](https://github.com/Azure/azure-iotedge/releases/tag/1.0.4) Edge çalışma zamanını, kendisine bağlanmak için aşağı akış cihazlarının ve Edge cihazı için genişletilmiş çevrimdışı işlemi yapılandırılabilir. 

Bu özellik sayesinde yerel modülleri veya aşağı akış cihazları Edge cihazı ile gerektiği gibi kimliğinizi yeniden doğrulayın ve iletileri ve IOT Hub'ından değilken bile yöntemleri kullanarak birbirlerinin ile iletişim. Bkz. Bu [blog gönderisi](https://aka.ms/iot-edge-offline) ve [kavramı makale](../articles/iot-edge/offline-capabilities.md) daha fazla ayrıntı ve bu özellik kapsamı için.

Çevrimdışı bir ağ geçidi senaryosunda genişletilmiş etkinleştirmek için edge cihazı ve buna bağlanmak aşağı akış cihazlar arasında bir üst-alt ilişkisi oluşturun.

1. IOT hub'ı portalda Edge cihaz ayrıntıları dikey penceresinden tıklayın **alt cihazları yönetme (Önizleme)** üst komut çubuğunda düğme.

1. Tıklayın **+ Ekle** düğmesi.

1. Cihazlar listesinde, alt cihazları seçin ve alt öğeleri olarak eklemek için olanları seçmek için sağ ok tuşunu kullanın.

1. Tıklayın **Tamam** onaylamak için.

Sınır cihazı ve onun alt cihazlar artık genişletilmiş çevrimdışı işlemi için etkinleştirilir.  