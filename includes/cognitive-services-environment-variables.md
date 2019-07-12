---
author: aahill
ms.service: cognitive-services
ms.topic: include
ms.date: 06/24/2019
ms.author: aahi
ms.openlocfilehash: 6d6451d50a00569eb1da8f5b0a0dc10d3c6b1115
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67841598"
---
## <a name="configure-an-environment-variable-for-authentication"></a>Ortam değişkeni kimlik doğrulaması için yapılandırma

Bilişsel hizmetler kullandıkları kimlik doğrulaması yapmak uygulamaları gerekir. Kimlik doğrulaması yapmak için Azure kaynaklarınız için anahtarları depolamak için bir ortam değişkeni oluşturmanızı öneririz. 

Anahtarınızı oluşturduktan sonra uygulamayı çalıştıran yerel makine üzerinde yeni bir ortam değişkenine yazın. Ortam değişkenini ayarlamak için bir konsol penceresi açın ve işletim sisteminizin yönergelerini izleyin. Değiştirin `your-key` kaynağınızın anahtarlarından birini ile.

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
