---
title: "Socket.IO kullanarak Node.js uygulaması | Microsoft Docs"
description: "Azure üzerinde barındırılan bir node.js uygulamasında Socket.IO kullanmayı öğrenin."
services: cloud-services
documentationcenter: nodejs
author: TomArcher
manager: routlaw
editor: 
ms.assetid: 7f9435e0-7732-4aa1-a4df-ea0e894b847f
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2017
ms.author: tarcher
ms.openlocfilehash: a85d4348a13b79b5b7542362de9956aa3398375a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="build-a-nodejs-chat-application-with-socketio-on-an-azure-cloud-service"></a>Bir Azure bulut hizmeti Socket.IO ile bir Node.js sohbet uygulaması oluşturma
Socket.IO node.js sunucunuz ve istemcileriniz arasında arasında gerçek zamanlı iletişim sağlar. Bu öğretici bir yuva barındırma aracılığıyla yol gösterir. G/ç Azure sohbet uygulamaya dayalı. Socket.IO ile ilgili daha fazla bilgi için bkz: <http://socket.io/>.

Tamamlanmış uygulamanın bir ekran görüntüsü aşağıda verilmiştir:

![Azure üzerinde barındırılan hizmet gösteren bir tarayıcı penceresi][completed-app]  

## <a name="prerequisites"></a>Ön koşullar
Aşağıdaki ürünleri ve sürümleri bu makaledeki örnek başarılı bir şekilde yüklendiğinden emin olun:

* [Visual Studio](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx)'yu yükleme
* [Node.js](https://nodejs.org/download/)’yi yükleme
* Yükleme [Python sürümü 2.7.10](https://www.python.org/)

## <a name="create-a-cloud-service-project"></a>Bir bulut hizmeti projesi oluşturma
Aşağıdaki adımları Socket.IO uygulama barındıracak bulut hizmeti projesi oluşturun.

1. Gelen **Başlat menüsü** veya **Başlat ekranında**, arama **Windows PowerShell**. Son olarak, sağ **Windows PowerShell** seçip **yönetici olarak çalıştır**.
   
    ![Azure PowerShell simgesi][powershell-menu]
2. Adlı bir dizin oluşturun **c:\\düğümü**. 
   
        PS C:\> md node
3. Değiştirme dizinleri **c:\\düğümü** dizini
   
        PS C:\> cd node
4. Adlı yeni bir çözüm oluşturmak için aşağıdaki komutları girin **chatapp** ve adlı çalışan rolü **WorkerRole1**:
   
        PS C:\node> New-AzureServiceProject chatapp
        PS C:\Node> Add-AzureNodeWorkerRole
   
    Aşağıdaki yanıt görürsünüz:
   
    ![Yeni-azureservice ve ekleme azurenodeworkerrolecmdlets çıktısı](./media/cloud-services-nodejs-chat-app-socketio/socketio-1.png)

## <a name="download-the-chat-example"></a>Sohbet örnek indirme
Bu proje için Sohbet örnekten kullanacağız [Socket.IO GitHub deposunu]. Örnek indirin ve daha önce oluşturduğunuz projeye eklemek için aşağıdaki adımları gerçekleştirin.

1. Kullanarak depoyu yerel bir kopyasını oluşturma **kopya** düğmesi. De kullanabilirsiniz **ZIP** düğmesi projenizi indirin.
   
   ![Vurgulanan ZIP indirme simgesiyle https://github.com/LearnBoost/Socket.io/Tree/master/examples/Chat, görüntüleme bir tarayıcı penceresi][chat-example-view]
2. Konumundaki gelmesini kadar yerel deposu dizin yapısını gezinin **örnekler\\sohbet** dizin. Bu dizine içeriğini kopyalayın **C:\\düğümü\\chatapp\\WorkerRole1** daha önce oluşturduğunuz dizin.
   
   ![Örnekler içeriğini görüntüleme explorer\\arşivden ayıklanan sohbet dizini][chat-contents]
   
   Yukarıdaki ekran görüntüsünde vurgulanan öğeleri kopyalanan dosyalarıdır **örnekler\\sohbet** dizini
3. İçinde **C:\\düğümü\\chatapp\\WorkerRole1** dizin silme **server.js** dosyasını bulun ve sonra yeniden adlandırın **app.js** dosya **server.js**. Bu varsayılan kaldırır **server.js** tarafından daha önce oluşturduğunuz dosya **Ekle AzureNodeWorkerRole** cmdlet'i ve sohbet örnek uygulama dosyasından ile değiştirir.

### <a name="modify-serverjs-and-install-modules"></a>Server.js değiştirmek ve modülleri yükleme
Uygulama Azure öykünücüsünde test etmeden önce biz bazı küçük değişiklikler yapmanız gerekir. Server.js dosyası için aşağıdaki adımları gerçekleştirin:

1. Açık **server.js** Visual Studio veya herhangi bir metin düzenleyicisi dosyası.
2. Bul **modülü bağımlılıkları** bölümünde server.js başında ve satırını içeren değiştirme **SIO require('..//..//lib//socket.io')** için **SIO require('socket.io') =** aşağıda gösterildiği gibi:
   
       var express = require('express')
         , stylus = require('stylus')
         , nib = require('nib')
       //, sio = require('..//..//lib//socket.io'); //Original
         , sio = require('socket.io');                //Updated
         var port = process.env.PORT || 3000;         //Updated
3. Doğru bağlantı noktasını dinleyen bir uygulama emin olmak için server.js Not Defteri veya tercih ettiğiniz Düzenleyicisi açın ve ardından değiştirerek aşağıdaki satırı değiştirin **3000** ile **process.env.port** aşağıda gösterildiği gibi:
   
       //app.listen(3000, function () {            //Original
       app.listen(process.env.port, function () {  //Updated
         var addr = app.address();
         console.log('   app listening on http://' + addr.address + ':' + addr.port);
       });

Değişiklikleri kaydetmeden sonra **server.js**, gerekli modüllerini yüklemek için aşağıdaki adımları kullanın ve Azure öykünücüsünde uygulamayı test etme:

1. Kullanarak **Azure PowerShell**, dizinleri değiştirmek **C:\\düğümü\\chatapp\\WorkerRole1** şu komutu bu uygulama tarafından gerekli modüllerini yüklemek için dizin ve kullanın:
   
       PS C:\node\chatapp\WorkerRole1> npm install
   
   Bu package.json dosyasında listelenen modüllerini yükler. Komut tamamlandığında, aşağıdakine benzer bir çıktı görmeniz gerekir:
   
   ![Npm çıktısını yükleme komutu][The-output-of-the-npm-install-command]
2. Bu örnekte, ilk olarak Socket.IO GitHub deposuna bir parçası olan ve doğrudan Socket.IO kitaplığı göreli yolu tarafından başvurulan olduğundan, biz aşağıdaki komutu gönderdikten yüklemeniz gerekir böylece Socket.IO package.json dosyasında başvurulmadı:
   
       PS C:\node\chatapp\WorkerRole1> npm install socket.io --save

### <a name="test-and-deploy"></a>Test etme ve dağıtma
1. Aşağıdaki komutu gönderdikten öykünücü başlatma:
   
       PS C:\node\chatapp\WorkerRole1> Start-AzureEmulator -Launch
   
   > [!NOTE]
   > Öykünücü, örneğin başlatma sorunlar karşılaşırsanız.: Başlangıç AzureEmulator: beklenmeyen bir hata oluştu.  Ayrıntılar: Karşılaştı Faulted durumda olduğu için beklenmeyen bir hata iletişim nesnesi System.ServiceModel.Channels.ServiceChannel, iletişim için kullanılamaz.
   
      AzureAuthoringTools v 2.7.1 ve AzureComputeEmulator v 2.7 yeniden - o eşleştiğinden emin olun.
   >
   >


2. Bir tarayıcı açın ve gidin **http://127.0.0.1**.
3. Bir tarayıcı penceresi açıldığında, takma ad girin ve ardından ENTER tuşuna basın.
   Bu, belirli bir takma ad ileti göndermek olanak tanır. Çok kullanıcılı işlevselliğini sınamak için aynı URL'yi kullanarak ek tarayıcı pencerelerini açın ve farklı takma adlar girin.
   
   ![İki tarayıcı pencerelerini Kullanıcı1 ve kullanıcı2 sohbet iletileri görüntüleme](./media/cloud-services-nodejs-chat-app-socketio/socketio-8.png)
4. Uygulamayı test etme sonra aşağıdaki komutu gönderdikten öykünücü durdurun:
   
       PS C:\node\chatapp\WorkerRole1> Stop-AzureEmulator
5. Uygulamayı Azure'a dağıtmak için kullandığınız **Publish-AzureServiceProject** cmdlet'i. Örneğin:
   
       PS C:\node\chatapp\WorkerRole1> Publish-AzureServiceProject -ServiceName mychatapp -Location "East US" -Launch
   
   > [!IMPORTANT]
   > Benzersiz bir ad kullandığınızdan emin olun, aksi takdirde yayımlama işlemi başarısız olur. Dağıtım tamamlandıktan sonra tarayıcıyı açın ve dağıtılmış hizmette gidin.
   > 
   > Sağlanan abonelik adını içeri aktarılan yayımlama profilinde yok bildiren bir hata alırsanız, indirin ve Azure'a dağıtmadan önce aboneliğiniz için yayımlama profili içeri aktarmanız gerekir. Bkz: **uygulamayı Azure'a dağıtma** bölümünü [derleme ve Azure bulut hizmeti bir Node.js uygulamasını dağıtma](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/)
   > 
   > 
   
   ![Azure üzerinde barındırılan hizmet gösteren bir tarayıcı penceresi][completed-app]
   
   > [!NOTE]
   > Sağlanan abonelik adını içeri aktarılan yayımlama profilinde yok bildiren bir hata alırsanız, indirin ve Azure'a dağıtmadan önce aboneliğiniz için yayımlama profili içeri aktarmanız gerekir. Bkz: **uygulamayı Azure'a dağıtma** bölümünü [derleme ve Azure bulut hizmeti bir Node.js uygulamasını dağıtma](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/)
   > 
   > 

Uygulamanız artık Azure üzerinde çalışan ve Sohbet iletilerini Socket.IO kullanarak farklı istemciler arasında geçiş.

> [!NOTE]
> Kolaylık olması için bu örnek aynı örneğine bağlı kullanıcılar arasında sohbet sınırlıdır. Bu bulut hizmeti iki çalışan rolü örnekleri oluşturursa, kullanıcılar yalnızca aynı çalışan rolü örneğine bağlı başkalarıyla sohbet mümkün olacağı anlamına gelir. Birden çok rol örnekleri ile çalışmak için uygulama ölçeklendirmek için hizmet veri yolu gibi bir teknoloji Socket.IO deposu durumu örnekleri arasında paylaşmak için kullanabilirsiniz. Service Bus kuyrukları ve konularından kullanım örnekleri örnekler için bkz [Node.js GitHub deposunu için Azure SDK](https://github.com/WindowsAzure/azure-sdk-for-node).
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide bir Azure bulut hizmetinde barındırılan bir temel sohbet uygulaması oluşturma öğrendiniz. Bu uygulamada bir Azure Web sitesi barındırmak öğrenmek için bkz: [bir Azure Web sitesinde Socket.IO ile bir Node.js sohbet uygulaması derleme][chatwebsite].

Daha fazla bilgi için Ayrıca bkz. [Node.js Geliştirici Merkezi](/develop/nodejs/).

[chatwebsite]: /develop/nodejs/tutorials/website-using-socketio/

[Azure SLA]: http://www.windowsazure.com/support/sla/
[Azure SDK for Node.js GitHub repository]: https://github.com/WindowsAzure/azure-sdk-for-node
[completed-app]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-10.png
[Azure SDK for Node.js]: https://www.windowsazure.com/develop/nodejs/
[Node.js Web Application]: https://www.windowsazure.com/develop/nodejs/tutorials/getting-started/
[Socket.IO GitHub deposunu]: https://github.com/LearnBoost/socket.io/tree/0.9.14
[Azure Considerations]: #windowsazureconsiderations
[Hosting the Chat Example in a Worker Role]: #hostingthechatexampleinawebrole
[Summary and Next Steps]: #summary
[powershell-menu]: ./media/cloud-services-nodejs-chat-app-socketio/azure-powershell-start.png

[chat example]: https://github.com/LearnBoost/socket.io/tree/master/examples/chat
[chat-example-view]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-22.png


[chat-contents]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-5.png
[The-output-of-the-npm-install-command]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-7.png
[The output of the Publish-AzureService command]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-9.png


