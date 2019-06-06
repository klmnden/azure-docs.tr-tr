---
title: Visual Studio Code - Azure IOT Edge yeni bir CİHAZDAN kaydetme | Microsoft Docs
description: Azure IOT hub'ına yeni bir IOT Edge cihazı oluşturma ve bağlantı dizesini almak için Visual Studio Code'u kullanın
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/03/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: c8fce104d48acc3a562599c65eb15cb0a66657b7
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66495304"
---
# <a name="register-a-new-azure-iot-edge-device-from-visual-studio-code"></a>Visual Studio code'dan yeni bir Azure IOT Edge cihazı kaydedin

IOT cihazlarınızı Azure IOT Edge ile kullanabilmeniz için önce bunları IOT hub'ınıza kaydetmeniz gerekir. Bir cihaz kaydettiğinizde, IOT Edge iş yükleri için Cihazınızı ayarlamak için kullanılan bir bağlantı dizesi alır.

Bu makalede, Visual Studio Code (VS Code) kullanarak yeni bir IOT Edge cihazı kaydettirmek gösterilmektedir. VS Code'da çoğu işlemi gerçekleştirmek için birden çok yolu vardır. Bu makalede Gezgini'ni kullanır, ancak komut paletini adımlarını çalıştırmak için de kullanabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

* Bir [IOT hub'ı](../iot-hub/iot-hub-create-through-portal.md) Azure aboneliğinizdeki
* [Visual Studio Code](https://code.visualstudio.com/)
* [Azure IOT Araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) Visual Studio Code için

## <a name="sign-in-to-access-your-iot-hub"></a>IOT hub'ınıza erişmek için oturum açın

Visual Studio Code için Azure IOT uzantıları, IOT hub'ınıza işlemleri gerçekleştirmek için kullanabilirsiniz. Bu işlemler çalışmak Azure hesabınızda oturum açın ve IOT hub'ınızı seçin gerekir.

1. Visual Studio Code'da açmak **Gezgini** görünümü.

1. Explorer alt kısmında, Genişlet **Azure IOT hub'ı** bölümü.

   ![Azure IOT Hub cihazları bölümü genişletin](./media/how-to-register-device-vscode/azure-iot-hub-devices.png)

1. Tıklayarak **...**  içinde **Azure IOT hub'ı** bölüm başlığı. Üç nokta simgesini görmüyorsanız, tıklayın veya üst bilgisinin üzerinde gezdirin.

1. Seçin **IOT hub'ını seçin**.

1. Azure hesabınızda oturum açmadıysanız, bunu yapmak için yönergeleri izleyin.

1. Azure aboneliğinizi seçin.

1. IOT hub'ınızı seçin.

## <a name="create-a-device"></a>Cihaz oluşturma

1. VS Code Gezgininde genişletin **Azure IOT Hub cihazları** bölümü.

1. Tıklayarak **...**  içinde **Azure IOT Hub cihazları** bölüm başlığı. Üç nokta simgesini görmüyorsanız, tıklayın veya üst bilgisinin üzerinde gezdirin.

1. Seçin **IOT Edge cihazı oluşturma**.

1. Açılan metin kutusuna, cihaz kimliği verin.

Çıkış ekranında, komutun sonucuna ilişkin bakın. İçeren cihaz bilgileri yazdırılır **DeviceID** sağladığınız ve **connectionString** fiziksel Cihazınızı IOT hub'ınıza bağlanmak için kullanabilirsiniz.

## <a name="view-all-devices"></a>Tüm cihazları görüntüle

IOT hub'ınıza bağlanan tüm cihazlar listelenen **Azure IOT hub'ı** Visual Studio kod Gezgini bölüm. IOT Edge cihazları farklı bir simgesi vardır ve olgu kenar-olmayan cihazlardan ayrılabilen, **$edgeAgent** ve **$edgeHub** modülleri her IOT Edge cihazı dağıtılır.

   ![IOT hub'ına tüm IOT Edge cihazları görüntüle](./media/how-to-register-device-vscode/view-devices.png)

## <a name="retrieve-the-connection-string"></a>Bağlantı dizesi alma

Cihazınızı ayarlamak hazır olduğunuzda, fiziksel cihazınızın IOT hub'ında kimliği ile bağlanan bağlantı dizesine ihtiyacınız vardır.

1. Cihazınızı kimliği sağ **Azure IOT hub'ı** bölümü.

1. Seçin **cihaz bağlantı dizesini kopyalayın**.

   Bağlantı dizesi panonuza kopyalanır.

Belirleyebilirsiniz **cihaz bilgi al** tüm cihaz bilgileri görmek için sağ tıklama menüsünden bağlantı dizesi dahil olmak üzere çıktı penceresinde.

## <a name="next-steps"></a>Sonraki adımlar

Bilgi nasıl [Visual Studio Code ile bir cihaz için modülleri dağıtma](how-to-deploy-modules-vscode.md).
