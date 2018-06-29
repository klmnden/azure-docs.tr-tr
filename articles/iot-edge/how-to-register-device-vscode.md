---
title: Yeni bir Azure IOT kenar cihaz (VS Code) kaydetme | Microsoft Docs
description: Yeni bir IOT sınır cihazı Azure IOT hub'ınıza oluşturmak için Visual Studio Code kullanın
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/14/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 7902461f58df1b4fe0c3ed3b577f668fe8be4cc2
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37036533"
---
# <a name="register-a-new-azure-iot-edge-device-from-visual-studio-code"></a>Visual Studio Code yeni bir Azure IOT kenar aygıttan kaydetme

IOT cihazlarınızı Azure IOT Edge kullanmadan önce bunları IOT hub'ınıza kaydetmeniz gerekir. Bir cihaz kaydettiğinizde, kenar iş yükleri için Cihazınızı ayarlamak için kullanılan bir bağlantı dizesi alırsınız. 

Bu makalede, Visual Studio Code (VS Code) kullanarak yeni bir IOT sınır cihazı kaydetmek gösterilmiştir. VS Code'da çoğu işlemi gerçekleştirmek için birden çok yolu vardır. Bu makalede Explorer kullanır, ancak adımları çoğunu çalıştırmak için komutu palet de kullanabilirsiniz. 

## <a name="prerequisites"></a>Önkoşullar

* Bir [IOT hub'ı](../iot-hub/iot-hub-create-through-portal.md) Azure aboneliğinizde
* [Visual Studio Code](https://code.visualstudio.com/) 
* [Azure IOT kenar uzantısı](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) için Visual Studio Code

## <a name="sign-in-to-access-your-iot-hub"></a>IOT hub'ınızı erişmek için oturum açın

Visual Studio Code için Azure IOT uzantıları, IOT hub'ınıza işlemlerini gerçekleştirmek için kullanabilirsiniz. Bu işlemler çalışmak Azure hesabınızda oturum açın ve üzerinde çalıştığınız IOT hub'ını seçin gerekir.

1. Visual Studio Code'da açın **Explorer** görünümü.

2. Explorer alt kısmındaki genişletin **Azure IOT Hub cihazları** bölümü. 

   ![Azure IOT Hub cihazları genişletin](./media/how-to-register-device-vscode/azure-iot-hub-devices.png)

3. Tıklayın **...**  içinde **Azure IOT Hub cihazları** bölüm başlığı. Üç nokta görmüyorsanız, üstbilgi gelin. 

4. Seçin **IOT hub'ını seçin**.

5. Azure hesabınızda açmadınız varsa, bunu yapmak için yönergeleri izleyin. 

6. Azure aboneliğinizi seçin. 

7. IOT hub'ı seçin. 

## <a name="create-a-device"></a>Bir cihaz oluşturma

1. VS Code Explorer'da genişletin **Azure IOT Hub cihazları** bölümü. 

2. Tıklayın **...**  içinde **Azure IOT Hub cihazları** bölüm başlığı. Üç nokta görmüyorsanız, üstbilgi gelin. 

3. Seçin **IOT sınır cihazı oluşturma**. 

4. Açılır metin kutusunda Cihazınızı kimliği verin. 

Çıkış ekranında komutu sonucu bakın. Aygıt bilgisi yazdırılır, içeren **DeviceID** belirttiğiniz ve **connectionString** fiziksel cihazınızın IOT hub'ınıza bağlanmak için kullanabilirsiniz. 

## <a name="view-all-devices"></a>Tüm cihazları görüntüleme

IOT hub'ınıza bağlanan tüm aygıtları listelenen **Azure IOT Hub cihazları** Visual Studio kod Gezgini bölümü. IOT sınır cihazları farklı bir simgesi vardır ve bunlar her cihaza dağıtılan modülleri göstermek için Genişletilebilir olgu kenar olmayan aygıtlardan ayrılabilen. 

   ![VS code'da aygıtlarını görüntüle](./media/how-to-register-device-vscode/view-devices.png)

## <a name="retrieve-the-connection-string"></a>Bağlantı dizesi alma

Cihazınızı ayarlamak hazır olduğunuzda, fiziksel kimliğini IOT hub'ında cihazınızla bağlanan bağlantı dizesi gerekiyor.

1. Cihazınızı Kimliğini sağ **Azure IOT Hub cihazları** bölümü. 
2. Seçin **cihaz bağlantı dizesini kopyalayın**.

   Bağlantı dizesi, panoya kopyalandı. 

Öğesini de seçebilirsiniz **cihaz bilgi al** tüm cihaz bilgilerini görmek için sağ tıklama menüsünden bağlantı dizesi çıktı penceresinde de dahil olmak üzere. 


## <a name="next-steps"></a>Sonraki adımlar

Bilgi edinmek için nasıl [modülleri Visual Studio Code ile bir aygıta dağıtmak](how-to-deploy-modules-vscode.md).
