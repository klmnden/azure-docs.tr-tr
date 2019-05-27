---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.author: crdun
ms.openlocfilehash: 46cfb27b8bde95990d13ec4bca4e96f25cfe9dc5
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66141224"
---
Sürmekte olan geliştirme nedeniyle, Android Studio'da yüklü Android SDK'sı sürüm kodu sürümünde eşleşmeyebilir. Bu öğreticide başvurulan Android SDK 26, yazma sırasında en son sürümüdür. SDK'sının daha yeni sürümleri görünür ve kullanılabilir en son sürümünü kullanmanızı öneririz, sürüm numarasını artırmanızı.

Sürüm uyuşmazlığı iki belirtileri şunlardır:

- Derleme veya projeyi yeniden derleyin, Gradle hata iletileri gibi alabilirsiniz `Gradle sync failed: Failed to find target with hash string 'android-XX'`.
- Standart Android nesneleri çözümlenmelidir kod temelinde `import` ifadeleri hata iletileri oluşturur.

Bunlardan biri görünürse, Android, Android Studio'da yüklü SDK sürümü indirilen projedeki SDK hedefi eşleşmeyebilir. Sürümü doğrulamak için aşağıdaki değişiklikleri yapın:

1. Android Studio'da **Araçları** > **Android** > **SDK Yöneticisi**. SDK platformunu en son sürümü yüklü değilse, daha sonra yüklemek için tıklayın. Sürüm numarasını not edin.

2. Üzerinde **Proje Gezgini** sekmesindeki **Gradle betiklerini**, dosyayı açma **build.gradle (modül: uygulama)** . Emin **compileSdkVersion** ve **targetSdkVersion** yüklü en son SDK sürümüne ayarlanır. `build.gradle` Şuna benzeyebilir:

    ```gradle
    android {
        compileSdkVersion 26
        defaultConfig {
            targetSdkVersion 26
        }
    }
    ```
