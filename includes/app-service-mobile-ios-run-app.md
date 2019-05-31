---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.author: crdun
ms.openlocfilehash: a5bde1a56bf6a1f5fca4b775c7c8e9bb7477eb6b
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66240264"
---
1. Yüklenen istemci projeyi Xcode kullanarak açın.

2. Git [Azure portalında](https://portal.azure.com/) ve oluşturduğunuz mobil uygulamaya gidin. Üzerinde `Overview` dikey penceresinde mobil uygulamanız için genel bir uç nokta URL'sini arayın. Örnek - sitename my app name "test123" için olacak https://test123.azurewebsites.net.

3. SWIFT projesi için dosyayı açmak `ToDoTableViewController.swift` bu klasördeki - ZUMOAPPNAME/ZUMOAPPNAME/ToDoTableViewController.swift. Uygulama adı `ZUMOAPPNAME`.

4. İçinde `viewDidLoad()` yöntemi Değiştir `ZUMOAPPURL` yukarıdaki genel uç nokta parametresi.

    `let client = MSClient(applicationURLString: "ZUMOAPPURL")`

    olur
    
    `let client = MSClient(applicationURLString: "https://test123.azurewebsites.net")`
    
5. Objective-C projesi için dosyayı açmak `QSTodoService.m` bu klasördeki - ZUMOAPPNAME/ZUMOAPPNAME. Uygulama adı `ZUMOAPPNAME`.

6. İçinde `init` yöntemi Değiştir `ZUMOAPPURL` yukarıdaki genel uç nokta parametresi.

    `self.client = [MSClient clientWithApplicationURLString:@"ZUMOAPPURL"];`

    olur
    
    `self.client = [MSClient clientWithApplicationURLString:@"https://test123.azurewebsites.net"];`

7. Projeyi oluşturmak için **Çalıştır** düğmesine basın ve uygulamayı iOS benzeticisinde başlatın.

8. Uygulamada, artı işaretine tıklayın ( **+** ) simgesi gibi anlamlı bir metin yazın *öğreticiye*ve sonra Kaydet'e tıklayın düğmesi. Böylece, daha önce dağıttığınız Azure arka ucuna bir POST isteği gönderilir. Arka uç, verileri istekten TodoItem SQL tablosuna ekler ve yeni depolanan öğeler hakkındaki bilgileri mobil uygulamaya döndürür. Mobil uygulama bu verileri listede görüntüler.

   ![iOS’ta çalışan hızlı başlangıç uygulaması](./media/app-service-mobile-ios-quickstart/mobile-quickstart-startup-ios.png)

[Azure portal]: https://portal.azure.com/
