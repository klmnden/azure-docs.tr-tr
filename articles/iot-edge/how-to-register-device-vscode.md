---
title: Visual Studio Code - Azure IOT Edge yeni bir CİHAZDAN kaydetme | Microsoft Docs
description: Azure IOT hub'ına yeni bir IOT Edge cihazı oluşturma ve bağlantı dizesini almak için Visual Studio Code'u kullanın
author: kgremban
manager: philmea
ms.author: v-yiso
origin.date: 01/03/2019
ms.date: 01/28/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 2b851e7f2ebdbff08fa09002765fbd7d7927deba
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62126299"
---
# <a name="register-a-new-azure-iot-edge-device-from-visual-studio-code"></a>Visual Studio code'dan yeni bir Azure IOT Edge cihazı kaydedin

IOT cihazlarınızı Azure IOT Edge ile kullanabilmeniz için önce bunları IOT hub'ınıza kaydetmeniz gerekir. Bir cihaz kaydettiğinizde, Edge iş yükleri için Cihazınızı ayarlamak için kullanılan bir bağlantı dizesi alır. 

Bu makalede, Visual Studio Code (VS Code) kullanarak yeni bir IOT Edge cihazı kaydettirmek gösterilmektedir. VS Code'da çoğu işlemi gerçekleştirmek için birden çok yolu vardır. Bu makalede Gezgini'ni kullanır, ancak komut paletini adımlarının çoğunun çalıştırmak için de kullanabilirsiniz. 

## <a name="prerequisites"></a>Önkoşullar

* Bir [IOT hub'ı](../iot-hub/iot-hub-create-through-portal.md) Azure aboneliğinizdeki
* [Visual Studio Code](https://code.visualstudio.com/)
* [Azure IOT Araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) Visual Studio Code için

## <a name="sign-in-to-access-your-iot-hub"></a>IOT hub'ınıza erişmek için oturum açın

Visual Studio Code için Azure IOT uzantıları, IOT hub'ınıza işlemleri gerçekleştirmek için kullanabilirsiniz. Bu işlemleri çalışmak Azure hesabınızda oturum açın ve üzerinde çalıştığınız IOT hub'ı seçmek gerekir.

1. Visual Studio Code'da açmak **Gezgini** görünümü.

2. Explorer alt kısmında, Genişlet **Azure IOT Hub cihazları** bölümü. 

   ![Azure IOT Hub cihazları bölümü genişletin](./media/how-to-register-device-vscode/azure-iot-hub-devices.png)

3. Tıklayarak **...**  içinde **Azure IOT Hub cihazları** bölüm başlığı. Üç nokta simgesini görmüyorsanız, tıklayın veya üst bilgisinin üzerinde gezdirin. 

4. Seçin **IOT hub'ını seçin**.

5. Azure hesabınızda oturum açmadınız, bunu yapmak için yönergeleri izleyin. 

6. Azure aboneliğinizi seçin. 

7. IOT hub'ınızı seçin. 

## <a name="create-a-device"></a>Cihaz oluşturma

1. VS Code Gezgininde genişletin **Azure IOT Hub cihazları** bölümü. 

2. Tıklayarak **...**  içinde **Azure IOT Hub cihazları** bölüm başlığı. Üç nokta simgesini görmüyorsanız, tıklayın veya üst bilgisinin üzerinde gezdirin. 

3. Seçin **IOT Edge cihazı oluşturma**. 

4. Açılan metin kutusuna, cihaz kimliği verin. 

Çıkış ekranında, komutun sonucuna ilişkin bakın. İçeren cihaz bilgileri yazdırılır **DeviceID** sağladığınız ve **connectionString** fiziksel Cihazınızı IOT hub'ınıza bağlanmak için kullanabilirsiniz. 

## <a name="view-all-devices"></a>Tüm cihazları görüntüle

IOT hub'ınıza bağlanan tüm cihazlar listelenen **Azure IOT Hub cihazları** Visual Studio kod Gezgini bölüm. Ayrılabilen farklı bir simgesi vardır ve bunlar her cihaza dağıtılan modülleri göstermek için Genişletilebilir olgu kenar-olmayan cihazlardan IOT Edge cihazları. 

   ![IOT hub'ına tüm IOT Edge cihazları görüntüle](./media/how-to-register-device-vscode/view-devices.png)

## <a name="retrieve-the-connection-string"></a>Bağlantı dizesi alma

Cihazınızı ayarlamak hazır olduğunuzda, fiziksel cihazınızın IOT hub'ında kimliği ile bağlanan bağlantı dizesine ihtiyacınız vardır.

1. Cihazınızı kimliği sağ **Azure IOT Hub cihazları** bölümü. 
2. Seçin **cihaz bağlantı dizesini kopyalayın**.

   Bağlantı dizesi panonuza kopyalanır. 

Belirleyebilirsiniz **cihaz bilgi al** tüm cihaz bilgileri görmek için sağ tıklama menüsünden bağlantı dizesi dahil olmak üzere çıktı penceresinde. 


## <a name="next-steps"></a>Sonraki adımlar

Bilgi nasıl [Visual Studio Code ile bir cihaz için modülleri dağıtma](how-to-deploy-modules-vscode.md).
