---
author: ramonarguelles
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 1/29/2019
ms.author: rgarcia
ms.openlocfilehash: b802c9dbd0cef65325cb03538b68b49c57b85bb3
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/26/2019
ms.locfileid: "56890958"
---
Seçin **derleme** bir iletişim kutusunu açın. Ardından, Xcode projesi dışarı aktarmak için bir klasör seçin.

Dışarı aktarma tamamlandığında, dışarı aktarılan bir Xcode projesini içeren klasör görüntülenir.

### <a name="open-the-xcode-project"></a>Xcode projesi açın

Dışarı aktarılan Xcode proje klasörü içinde proje için gerekli Cocoapods'u yüklemek için aşağıdaki komutu çalıştırın:

```bash
pod install --repo-update
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