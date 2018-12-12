---
title: Azure Portal - Azure IOT Edge yeni bir cihaz kaydetme | Microsoft Docs
description: Yeni bir IOT Edge cihazı kaydedin ve bağlantı dizesini almak için Azure portalını kullanma
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/05/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: da93541339ac1c199d3ba0a220fbfff6bbb8bf9c
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53082135"
---
# <a name="register-a-new-azure-iot-edge-device-from-the-azure-portal"></a>Azure portalında yeni bir Azure IOT Edge cihazı kaydedin

IOT cihazlarınızı Azure IOT Edge ile kullanabilmeniz için önce bunları IOT hub'ınıza kaydetmeniz gerekir. Bir cihaz kaydettiğinizde, Edge iş yükleri için Cihazınızı ayarlamak için kullanılan bir bağlantı dizesi alır. 

Bu makalede, Azure portalını kullanarak yeni bir IOT Edge cihazı kaydettirmek gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

* Bir [IOT hub'ı](../iot-hub/iot-hub-create-through-portal.md) Azure aboneliğinizdeki. 

## <a name="create-a-device"></a>Cihaz oluşturma

Azure portalında, IOT Edge cihazları oluşturulur ve IOT hub'ınıza bağlanmak, ancak edge etkin olmayan cihazlardan ayrı olarak yönetilir. 

1. Oturum [Azure portalında](https://portal.azure.com) ve IOT hub'ınıza gidin. 
2. Seçin **IOT Edge** menüsünde.
3. Seçin **IOT Edge cihazı Ekle**. 
4. Açıklayıcı bir cihaz kimliği sağlayın. 
5. **Kaydet**’i seçin. 

## <a name="view-all-devices"></a>Tüm cihazları görüntüle

IOT hub'ınıza bağlanan tüm edge özellikli cihazlar listelenir **IOT Edge** sayfası. 

## <a name="retrieve-the-connection-string"></a>Bağlantı dizesi alma

Cihazınızı ayarlamak hazır olduğunuzda, fiziksel cihazınızın IOT hub'ında kimliği ile bağlanan bağlantı dizesine ihtiyacınız vardır.

1. Gelen **IOT Edge** sayfasında Portalı'nda, uç cihazlarında listesinden bir cihaz kimliği tıklayın. 
2. Ya da değeri kopyalayın **bağlantı dizesi (birincil anahtar)** veya **bağlantı dizesi (ikincil anahtarı)**. 

## <a name="next-steps"></a>Sonraki adımlar

Bilgi edinmek için nasıl [modülleri Azure portalıyla bir cihaza dağıtın](how-to-deploy-modules-portal.md)
