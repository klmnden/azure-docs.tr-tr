---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 05/06/2019
ms.author: crdun
ms.openlocfilehash: 8d7731480b6239c572d39f52b6a0217d2ac48d25
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66240253"
---
1. Çözüm dosyasını (.sln) istemci projesindeki gidin ve Visual Studio'yu kullanarak açın.

2. Visual Studio'da başlatma okunun yanındaki açılır menüden çözüm platformunu (Android, iOS veya Windows) seçin. Yeşil ok üzerindeki açılır menüye tıklayarak belirli bir dağıtım cihazı veya öykünücüyü seçin. Varsayılan Android platformunu ve Ripple öykünücüsünü kullanabilirsiniz. Daha gelişmiş öğreticilerde (anında iletme bildirimleri gibi) desteklenen bir cihazı ve öykünücüyü seçmeniz istenecek.

3. Dosyayı açmak `ToDoActivity.java` bu klasördeki - ZUMOAPPNAME/uygulama/src/main/java/com/örnek/zumoappname. Uygulama adı `ZUMOAPPNAME`.

4. Git [Azure portalında](https://portal.azure.com/) ve oluşturduğunuz mobil uygulamaya gidin. Üzerinde `Overview` dikey penceresinde mobil uygulamanız için genel bir uç nokta URL'sini arayın. Örnek - sitename my app name "test123" için olacak https://test123.azurewebsites.net.

5. Git `index.js` ZUMOAPPNAME/www/js/index.js hem de dosya `onDeviceReady()` yöntemi Değiştir `ZUMOAPPURL` genel uç nokta yukarıdaki parametresiyle.

    `client = new WindowsAzure.MobileServiceClient('ZUMOAPPURL');`
    
    olur
    
    `client = new WindowsAzure.MobileServiceClient('https://test123.azurewebsites.net');`
    
6. Cordova uygulamanızı derlemek ve çalıştırmak için F5'e basın veya yeşil oka tıklayın. Öykünücüde ağa erişim isteyen bir güvenlik iletişim kutusu görürseniz erişim isteğini kabul edin.

7. Üzerinde cihazda veya öykünücüde uygulama başlatıldıktan sonra anlamlı bir metin yazın **yeni metin gir**, gibi *öğreticiye* ve ardından **Ekle** düğmesi.

Arka uç, istekten alınan verileri SQL Veritabanı'ndaki TodoItem tablosuna ekler ve yeni depolanan öğeler hakkındaki bilgileri de mobil uygulamaya geri döndürür. Mobil uygulama bu verileri listede görüntüler.

Diğer platformlar için 3 ile 5 arasındaki adımları tekrarlayabilirsiniz.