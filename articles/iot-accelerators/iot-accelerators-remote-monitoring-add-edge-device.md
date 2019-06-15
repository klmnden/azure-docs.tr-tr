---
title: Uzaktan izleme çözümünü ekleme Edge cihazı - Azure | Microsoft Docs
description: Bu makalede, bir IOT Edge cihazı için Uzaktan izleme çözüm Hızlandırıcısını eklemeyi açıklar
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 10/09/2018
ms.topic: conceptual
ms.openlocfilehash: d34ac159a216c5c77214b4c8b799a233c3671235
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61447371"
---
# <a name="add-an-iot-edge-device-to-your-remote-monitoring-solution-accelerator"></a>Uzaktan izleme çözüm Hızlandırıcısını için bir IOT Edge cihazı Ekle

Eklemek için bir [IOT Edge](../iot-edge/about-iot-edge.md) çözüm hızlandırıcınız cihaza aşağıdaki iki adımı tamamlayın:

1. Sınır cihazı eklemek **Device Explorer** Uzaktan izleme çözüm Hızlandırıcı Web kullanıcı Arabiriminde sayfası.
1. IOT Edge çalışma zamanı Edge Cihazınızda yükleyin.

## <a name="add-the-iot-edge-device"></a>IOT Edge cihazı Ekle

IOT Edge cihazı için Uzaktan izleme çözüm Hızlandırıcısını eklemek için gidin **Device Explorer** sayfasında web kullanıcı Arabiriminde ve tıklayın **+ yeni cihaz**.

İçinde **yeni cihaz** panelinde öğesini **IOT Edge cihazı**. Diğer ayarlar için varsayılan değerleri bırakabilirsiniz. Ardından **Apply** (Uygula) öğesine tıklayın:

![IOT Edge cihazı Ekle](media/iot-accelerators-remote-monitoring-add-edge-device/addedgedevice.png)

### <a name="alternative-ways-to-add-an-iot-edge-device"></a>IOT Edge cihazı eklemek için alternatif yöntemler

Çözüm hızlandırıcınız, IOT hub'ı örneği ile doğrudan IOT Edge cihazı kaydetmek mümkündür. Bu nasıl yapılır kılavuzlarından birini izlemeden önce çözüm hızlandırıcınız IOT hub'ı adını bilmeniz gerekir:

- [Azure portalında yeni bir Azure IOT Edge cihazı kaydedin](../iot-edge/how-to-register-device-portal.md)
- [Azure CLI ile yeni bir Azure IOT Edge cihazı kaydetme](../iot-edge/how-to-register-device-cli.md)
- [Visual Studio code'dan yeni bir Azure IOT Edge cihazı kaydedin](../iot-edge/how-to-register-device-vscode.md)

Uzaktan izleme çözüm Hızlandırıcısını IOT hub'ı ile doğrudan bir cihaz kaydettiğinizde, listelenmiş olup **Device Explorer** Web kullanıcı Arabiriminde sayfası.

## <a name="install-the-iot-edge-runtime"></a>IOT Edge çalışma zamanını yükleme

Edge cihazınıza modülleri dağıtmadan önce IOT Edge çalışma zamanı gerçek cihaz üzerinde yüklemeniz gerekir. Aşağıdaki nasıl yapılır kılavuzları, ortak cihaz platformlarında çalışma zamanı yükleme gösterilmektedir:

- [Azure IOT Edge çalışma zamanı (x64) Linux'ta yükleme](../iot-edge/how-to-install-iot-edge-linux.md)
- [Azure IOT Edge çalışma zamanı (ARM32v7/armhf) Linux'ta yükleme](../iot-edge/how-to-install-iot-edge-linux-arm.md)
- [Windows kapsayıcıları ile kullanmak için Windows Azure IOT Edge çalışma zamanını yükleyin](../iot-edge/how-to-install-iot-edge-windows-with-windows.md)
- [Linux kapsayıcıları ile kullanmak için Windows Azure IOT Edge çalışma zamanını yükleyin](../iot-edge/how-to-install-iot-edge-windows-with-linux.md)
- [IOT Edge çalışma zamanı üzerinde Windows IOT Core yükleme](../iot-edge/how-to-install-iot-core.md)

## <a name="next-steps"></a>Sonraki adımlar

IOT Edge Cihazınızı hazırlandıktan sonra sonraki adım, modülleri dağıtma sağlamaktır. Bkz: [bir IOT Edge paketi Uzaktan izleme çözüm Hızlandırıcısını alma](iot-accelerators-remote-monitoring-import-edge-package.md)
