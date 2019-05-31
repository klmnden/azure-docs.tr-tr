---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 05/09/2019
ms.author: crdun
ms.openlocfilehash: 63c54f8af91b6b4a76ba49d5e6fc7b3cda9f5b98
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66240339"
---
1. **Android Studio** kullanarak, **Projeyi içeri aktar (Eclipse ADT, Gradle, vb.)** kullanarak projeyi açın. JDK hatalarını önlemek için bu içeri aktarma seçimini yaptığınızdan emin olun.

2. Dosyayı açmak `ToDoActivity.java` bu klasördeki - ZUMOAPPNAME/uygulama/src/main/java/com/örnek/zumoappname. Uygulama adı `ZUMOAPPNAME`.

3. Git [Azure portalında](https://portal.azure.com/) ve oluşturduğunuz mobil uygulamaya gidin. Üzerinde `Overview` dikey penceresinde mobil uygulamanız için genel bir uç nokta URL'sini arayın. Örnek - sitename my app name "test123" için olacak https://test123.azurewebsites.net.

4. İçinde `onCreate()` yöntemi Değiştir `ZUMOAPPURL` yukarıdaki genel uç nokta parametresi.
    
    `new MobileServiceClient("ZUMOAPPURL", this).withFilter(new ProgressFilter());` 
    
    olur
    
    `new MobileServiceClient("https://test123.azurewebsites.net", this).withFilter(new ProgressFilter());`
    
5. Projeyi oluşturmak için **’Uygulamayı’ çalıştır** düğmesine basın ve uygulamayı Android benzeticisinde başlatın.

4. Uygulamada, *Öğreticiyi tamamla* gibi anlamlı bir metin yazın ve ardından ‘Ekle’ düğmesine basın. Böylece, daha önce dağıttığınız Azure arka ucuna bir POST isteği gönderilir. Arka uç, verileri istekten TodoItem SQL tablosuna ekler ve yeni depolanan öğeler hakkındaki bilgileri mobil uygulamaya döndürür. Mobil uygulama bu verileri listede görüntüler.
    ![Hızlı Android](./media/app-service-mobile-android-quickstart/mobile-quickstart-startup-android.png)