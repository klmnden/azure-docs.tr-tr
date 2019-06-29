---
author: aahill
ms.service: cognitive-services
ms.topic: include
ms.date: 06/24/2019
ms.author: aahi
ms.openlocfilehash: 734fce2c01614d5dca73f171fb59f25f39d13705
ms.sourcegitcommit: aa66898338a8f8c2eb7c952a8629e6d5c99d1468
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67461514"
---
## <a name="configure-an-environment-variable-for-authentication"></a>Ortam değişkeni kimlik doğrulaması için yapılandırma

Bilişsel hizmetler kullandıkları kimlik doğrulaması yapmak uygulamaları gerekir. Kimlik doğrulaması için bir anahtar, Azure kaynakları depolamak için bir ortam değişkeni oluşturmanızı öneririz. 

Anahtarınızı oluşturduktan sonra uygulamayı çalıştıran yerel makine üzerinde yeni bir ortam değişkenine yazın. Ortam değişkenini ayarlamak için bir konsol penceresi açın ve işletim sisteminizin yönergelerini izleyin. Değiştirin `your-key` Anomali algılayıcısı erişim anahtarınızı:

* Windows

    ```console
    setx COGNITIVE_SERVICE_KEY "your-key"
    ```

    Ortam değişkenini ekledikten sonra, konsol penceresi de dahil olmak üzere ortam değişkenini okumak için gereken tüm çalışan programları yeniden başlatmanız gerekebilir. Örneğin, düzenleyici olarak Visual Studio kullanıyorsanız, Visual Studio örneği çalıştırmadan önce yeniden başlatın.

* Linux
    
    ```bash
    export COGNITIVE_SERVICE_KEY=your-key
    ```
    
    Ortam değişkenini ekledikten sonra değişiklikleri uygulamak için konsol pencerenizden `source ~/.bashrc` çalıştırın.
    
* Mac OS
    
    .bash_profile dosyanızı düzenleyin ve ortam değişkenini ekleyin:
    
    ```bash
    export COGNITIVE_SERVICE_KEY=your-key
    ```
    
    Ortam değişkenini ekledikten sonra değişiklikleri uygulamak için konsol pencerenizden `source .bash_profile` çalıştırın.
