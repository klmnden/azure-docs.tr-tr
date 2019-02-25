---
author: ramonarguelles
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 1/29/2019
ms.author: rgarcia
ms.openlocfilehash: 36d509bdcd1fda61cb85fae7fa38ed126697f888
ms.sourcegitcommit: e88188bc015525d5bead239ed562067d3fae9822
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/24/2019
ms.locfileid: "56752302"
---
Seçin **derleme** bir iletişim kutusunu açın. Ardından, Xcode projesi dışarı aktarmak için bir klasör seçin.

Dışarı aktarma tamamlandığında, dışarı aktarılan bir Xcode projesini içeren klasör görüntülenir.

### <a name="open-the-xcode-project"></a>Xcode projesi açın

Dışarı aktarılan Xcode proje klasörü içinde proje için gerekli Cocoapods'u yüklemek için aşağıdaki komutu çalıştırın:

```bash
pod install
```

Şimdi kullanabilirsiniz açın `Unity-iPhone.xcworkspace` Xcode'da projeyi açmak için:

```bash
open ./Unity-iPhone.xcworkspace
```

> [!NOTE]
> Görürseniz bir `library not found for -lPods-Unity-iPhone` hata büyük olasılıkla açtığınız `.xcodeproj` yerine dosya `.xcworkspace`. Açık `.xcworkspace` ve yeniden deneyin.

Kök seçin **Unity iPhone** proje ayarlarını görüntülemek ve seçmek için düğüm **genel** sekmesi.

Altında **imzalama**seçin **otomatik olarak imzalanmasını yönetmek**. Seçin **etkinleştirmek otomatik** sıfırlamak için görüntülenen iletişim kutusunda derleme ayarları.

Altında **dağıtım bilgisi**, emin **dağıtım hedefi** ayarlanır `11.0`.

### <a name="deploy-the-app-to-your-ios-device"></a>İOS cihazınıza uygulaması dağıtma

İOS cihazı ayarlama ve Mac bağlayın **etkin düzeni** iOS cihazınıza.

![Cihazı seçin](./media/spatial-anchors-unity/select-device.png)

Seçin **oluşturun ve ardından geçerli düzenini çalıştırın**.

![Dağıtma ve çalıştırma](./media/spatial-anchors-unity/deploy-run.png)