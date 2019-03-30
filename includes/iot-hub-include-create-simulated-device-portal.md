---
title: include dosyası
description: include dosyası
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 02/26/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 8d8f8021925ac9c9e427dd6571ecaa1a30c85497
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58670991"
---
<!-- This is the instructions for creating a simulated device you can use for testing routing.-->

Sonra bir cihaz kimliği oluşturun ve anahtarını daha sonra kullanmak üzere kaydedin. Bu cihaz kimliği simülasyon uygulaması tarafından IoT hub'ına iletileri göndermek için kullanılır. Bu özellik, bir Azure Resource Manager şablonu kullanarak, PowerShell veya kullanılabilir değil. Aşağıdaki adımları kullanarak sanal cihaz oluşturma size [Azure portalında](https://portal.azure.com).

1. [Azure portalını](https://portal.azure.com) açın ve Azure hesabınızla oturum açın.

2. Seçin **kaynak grupları** kaynak grubunuzu seçin. Bu öğreticide **ContosoResources** kullanılır.

3. Kaynak listesinde, IOT hub'ınızı seçin. Bu öğreticide **ContosoTestHub** kullanılır. Hub bölmesinden **IoT Cihazları**'nı seçin.

4. **+ Ekle** öğesini seçin. Cihaz Ekle bölmesinde cihaz kimliğini girin. Bu öğreticide **Contoso-Test-Device** kullanılır. Anahtarları boş bırakın ve **Anahtarları Otomatik Olarak Oluştur**'a tıklayın. **Cihazı IoT Hub'ına bağla** seçeneğinin etkinleştirildiğinden emin olun. **Kaydet**’i seçin.

   ![Cihaz Ekle ekranı](./media/iot-hub-include-create-simulated-device-portal/add-device.png)

5. Oluşturulduktan sonra oluşturulan tuşlarını görmek için cihazı seçin. Birincil anahtar Kopyala simgesini seçin ve onu bir yere not defteri gibi bu öğreticinin sınama aşaması için kaydedin.

   ![Anahtarları dahil olmak üzere cihaz ayrıntıları](./media/iot-hub-include-create-simulated-device-portal/device-details.png)