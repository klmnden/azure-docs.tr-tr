---
title: include dosyası
description: include dosyası
services: iot-edge
author: kgremban
ms.service: iot-edge
ms.topic: include
ms.date: 01/22/2019
ms.author: kgremban
ms.custom: include file
ms.openlocfilehash: 8693c48905155ed757bb727e42f4180f36c015f1
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188474"
---
## <a name="associate-an-azure-storage-account-to-iot-hub"></a>IOT hub'ı bir Azure depolama hesabına ilişkilendirin

Sanal cihaz uygulaması, bir dosyayı bloba yükler. çünkü olmalıdır bir [Azure depolama](../articles/storage/common/storage-quickstart-create-account.md) , IOT hub ile ilişkili hesap. Bir Azure depolama hesabı bir IOT hub ile ilişkilendirdiğinizde, IOT hub'ı SAS URI'sini oluşturur. Bir cihaz bu SAS URI'sini güvenli bir şekilde bir blob kapsayıcısına bir dosyayı karşıya yüklemek için kullanabilirsiniz. IOT Hub hizmeti ve cihaz SDK'ları SAS URI'sini oluşturur ve bir dosyayı karşıya yüklemek için kullanılacak bir cihaz için kullanılabilir hale getirir işlemi koordine edin.

Bölümündeki yönergeleri [yapılandırma dosyasını karşıya yükler Azure portalını kullanarak](../articles/iot-hub/iot-hub-configure-file-upload.md). Bir blob kapsayıcısı, IOT hub'ı ile ilişkili olduğunu ve dosya bildirimlerini etkinleştirildiğinden emin olun.

![Portalda dosya bildirimlerini etkinleştirme](./media/iot-hub-associate-storage/enable-file-notifications.png)