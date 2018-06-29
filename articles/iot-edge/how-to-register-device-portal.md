---
title: Yeni bir Azure IOT kenar cihaz (portal) kaydetme | Microsoft Docs
description: Yeni bir IOT sınır cihazı kaydetmek için Azure portalını kullanın
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/05/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: b61594469df33e11c23c9cbe0b9542da374fefa3
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37036307"
---
# <a name="register-a-new-azure-iot-edge-device-from-the-azure-portal"></a>Azure portalından yeni bir Azure IOT kenar cihaz kaydetme

IOT cihazlarınızı Azure IOT Edge kullanmadan önce bunları IOT hub'ınıza kaydetmeniz gerekir. Bir cihaz kaydettiğinizde, kenar iş yükleri için Cihazınızı ayarlamak için kullanılan bir bağlantı dizesi alırsınız. 

Bu makalede, Azure Portalı'nı kullanarak yeni bir IOT sınır cihazı kaydetmek gösterilmiştir.

## <a name="prerequisites"></a>Önkoşullar

* Bir [IOT hub'ı](../iot-hub/iot-hub-create-through-portal.md) Azure aboneliğinizde. 

## <a name="create-a-device"></a>Bir cihaz oluşturma

Azure portalında IOT sınır cihazları oluşturulur ve IOT hub'ınıza bağlanın ancak kenar etkin olmayan cihazlardan ayrı olarak yönetilir. 

1. Oturum [Azure portal](https://portal.azure.com) ve IOT hub'ına gidin. 
2. Seçin **IOT kenar** menüsünde.
3. Seçin **IOT sınır cihazı eklemek**. 
4. Açıklayıcı cihaz kimliğini sağlayın 
5. **Kaydet**’i seçin. 

## <a name="view-all-devices"></a>Tüm cihazları görüntüleme

IOT hub'ınıza bağlanan tüm kenar özellikli aygıtları listelendiğini **IOT kenar** sayfası. 

## <a name="retrieve-the-connection-string"></a>Bağlantı dizesi alma

Cihazınızı ayarlamak hazır olduğunuzda, fiziksel kimliğini IOT hub'ında cihazınızla bağlanan bağlantı dizesi gerekiyor.

1. Gelen **IOT kenar** sayfasında Portalı'nda, sınır cihazları listesinden cihaz Kimliği'i tıklatın. 
2. Ya da değerini kopyalayın **bağlantı dizesi — birincil anahtar** veya **bağlantı dizesi — ikincil anahtar**. 

## <a name="next-steps"></a>Sonraki adımlar

Bilgi edinmek için nasıl [modülleri Azure portal ile bir aygıta dağıtmak](how-to-deploy-modules-portal.md)