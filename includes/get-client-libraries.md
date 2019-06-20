---
author: spelluru
ms.service: service-bus-messaging
ms.topic: include
ms.date: 11/25/2018
ms.author: spelluru
ms.openlocfilehash: b6e0e57881154f5885e9f518363eda3c5b1169a0
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188613"
---
### <a name="install-via-composer"></a>Oluşturucu yükleme
1. Adlı bir dosya oluşturun **composer.json** projenizin kökünde ve aşağıdaki kodu ekleyin:
   
    ```json
    {
      "require": {
        "microsoft/azure-storage": "*"
      }
    }
    ```
2. İndirme **[composer.phar] [ composer-phar]** proje kökünüze içinde.
3. Bir komut istemi açın ve proje kökünde aşağıdaki komutu yürütün
   
    ```
    php composer.phar install
    ```

Alternatif olarak Git [Azure Storage PHP istemci Kitaplığı] [ php-sdk-github] kaynak kodu kopyalamak için GitHub üzerindeki.

[php-sdk-github]: https://github.com/Azure/azure-storage-php
[install-git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[download-SDK-PHP]: ../articles/php-download-sdk.md
[composer-phar]: http://getcomposer.org/composer.phar
