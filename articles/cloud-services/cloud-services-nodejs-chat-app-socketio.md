---
title: Socket.io - Azure kullanarak Node.js uygulaması
description: Azure üzerinde barındırılan bir node.js uygulaması içinde Socket.io kullanmayı öğrenin.
services: cloud-services
documentationcenter: nodejs
author: jpconnock
manager: timlt
editor: ''
ms.assetid: 7f9435e0-7732-4aa1-a4df-ea0e894b847f
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2017
ms.author: jeconnoc
ms.openlocfilehash: cd0bceae770182e778410d8065d34dfeed055acc
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61433197"
---
# <a name="build-a-nodejs-chat-application-with-socketio-on-an-azure-cloud-service"></a>Bir Azure bulut hizmeti Socket.IO ile Node.js sohbet uygulaması oluşturma

Socket.IO, node.js sunucunuz ile istemcileriniz arasında gerçek zamanlı iletişim sağlar. Bu öğreticide, bir yuva barındırma aracılığıyla açıklanmaktadır. GÇ sohbet uygulaması üzerinde Azure tabanlı. Socket.IO ile ilgili daha fazla bilgi için bkz [socket.io](https://socket.io).

Tamamlanmış uygulamanın bir ekran görüntüsü aşağıda verilmiştir:

![Azure üzerinde barındırılan hizmet gösteren bir tarayıcı penceresi][completed-app]  

## <a name="prerequisites"></a>Önkoşullar
Bu makaledeki örnek başarıyla tamamlamak için aşağıdaki ürünleri ve sürümleri yüklü olduğundan emin olun:

* [Visual Studio](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx)'yu yükleme
* [Node.js](https://nodejs.org/download/)’yi yükleme
* Yükleme [Python sürüm 2.7.10](https://www.python.org/)

## <a name="create-a-cloud-service-project"></a>Bir bulut hizmeti projesi oluşturma
Aşağıdaki adımlar, Socket.IO uygulamayı barındıracak bir bulut hizmeti projesi oluşturur.

1. Gelen **Başlat menüsü** veya **Başlangıç ekranına**, arama **Windows PowerShell**. Son olarak, sağ **Windows PowerShell** seçip **yönetici olarak çalıştır**.
   
    ![Azure PowerShell simgesi][powershell-menu]
2. Adlı bir dizin oluşturun **c:\\düğüm**. 
   
        PS C:\> md node
3. Dizinleri **c:\\düğüm** dizini
   
        PS C:\> cd node
4. Adlı yeni bir çözüm oluşturmak için aşağıdaki komutları girin **chatapp** ve adlı bir çalışan rolü **WorkerRole1**:
   
        PS C:\node> New-AzureServiceProject chatapp
        PS C:\Node> Add-AzureNodeWorkerRole
   
    Şu yanıtı görürsünüz:
   
    ![Yeni-azureservice ve ekleme azurenodeworkerrolecmdlets çıktısı](./media/cloud-services-nodejs-chat-app-socketio/socketio-1.png)

## <a name="download-the-chat-example"></a>Sohbet örneği indirin
Bu proje için Sohbet örnekten kullanacağız [Socket.IO GitHub deposu]. Örneği indirmek ve daha önce oluşturduğunuz projeye eklemek için aşağıdaki adımları gerçekleştirin.

1. Kullanarak depo yerel kopyasını oluşturma **kopya** düğmesi. Ayrıca **ZIP** projeyi indirmek için düğmeye.
   
   ![Bir tarayıcı penceresinde görüntüleme https://github.com/LearnBoost/socket.io/tree/master/examples/chat, vurgulanan ZIP indirme simgesi](./media/cloud-services-nodejs-chat-app-socketio/socketio-22.png)
2. Ulaşırsınız kadar dizin yapısı, yerel depoya gidin **örnekler\\sohbet** dizin. Bu dizine içeriğini kopyalayın **C:\\düğüm\\chatapp\\WorkerRole1** daha önce oluşturduğunuz dizin.
   
   ![Örnekler içeriğini görüntüleme Gezgini\\arşivden ayıklanan sohbet dizini][chat-contents]
   
   Yukarıdaki ekran görüntüsünde vurgulanmış öğe kopyalanır dosyalarıdır **örnekler\\sohbet** dizini
3. İçinde **C:\\düğüm\\chatapp\\WorkerRole1** dizini silmek **server.js** dosya ve yeniden adlandırın **app.js** Dosya **server.js**. Bu varsayılan kaldırır **server.js** tarafından daha önce oluşturduğunuz dosya **Ekle AzureNodeWorkerRole** cmdlet'i ve sohbet örnek uygulama dosyasından ile değiştirir.

### <a name="modify-serverjs-and-install-modules"></a>Server.js değiştirmek ve modülleri yükleme
Uygulamayı Azure öykünücüsü'nde test etmeden önce biz bazı küçük değişiklikler yapmanız gerekir. Server.js dosyası için aşağıdaki adımları gerçekleştirin:

1. Açık **server.js** dosyasını Visual Studio'da veya herhangi bir metin düzenleyicisi.
2. Bul **modülü bağımlılıkları** bölümünde server.js başında ve satırını içeren değiştirme **SIO require('..//..//lib//socket.io')** için **SIO require('socket.io') =** aşağıda gösterildiği gibi:
   
       var express = require('express')
         , stylus = require('stylus')
         , nib = require('nib')
       //, sio = require('..//..//lib//socket.io'); //Original
         , sio = require('socket.io');                //Updated
         var port = process.env.PORT || 3000;         //Updated
3. Doğru bağlantı noktası üzerinde dinleyen uygulama sağlamak için Not Defteri'ni veya tercih ettiğiniz düzenleyiciyi server.js açın ve ardından değiştirerek şu satırı değiştirin **3000** ile **process.env.port** aşağıda gösterildiği gibi:
   
       //app.listen(3000, function () {            //Original
       app.listen(process.env.port, function () {  //Updated
         var addr = app.address();
         console.log('   app listening on http://' + addr.address + ':' + addr.port);
       });

Değişiklikler kaydedildikten sonra **server.js**gerekli modülleri yüklemek için aşağıdaki adımları kullanın ve ardından Azure öykünücüsünde uygulamayı test edin:

1. Kullanarak **Azure PowerShell**, dizinleri **C:\\düğüm\\chatapp\\WorkerRole1** dizin ve aşağıdaki komutu modüllerini yüklemek için Bu uygulama tarafından gerekli:
   
       PS C:\node\chatapp\WorkerRole1> npm install
   
   Bu package.json dosyasında listelenen modülleri yükler. Komut tamamlandıktan sonra aşağıdakine benzer bir çıktı görmeniz gerekir:
   
   ![Çıkış npm yükleme komutu][The-output-of-the-npm-install-command]
2. Bu örnekte, ilk olarak Socket.IO GitHub deposunda bir parçası olan ve doğrudan Socket.IO kitaplığı göreli yolu tarafından başvurulan olduğundan, biz bunu aşağıdaki komutu gönderdikten tarafından yüklemelisiniz şekilde Socket.IO package.json dosyasına başvurulmadı:
   
       PS C:\node\chatapp\WorkerRole1> npm install socket.io --save

### <a name="test-and-deploy"></a>Test etme ve dağıtma
1. Aşağıdaki komutu gönderdikten tarafından öykünücüyü başlatın:
   
       PS C:\node\chatapp\WorkerRole1> Start-AzureEmulator -Launch
   
   > [!NOTE]
   > Öykünücü, örneğin uygulamasını başlatma sorunları yaşarsanız.: Start-AzureEmulator: Beklenmeyen bir hata oluştu.  Ayrıntılar: Karşılaşılan beklenmeyen bir hata iletişim nesnesi System.ServiceModel.Channels.ServiceChannel Faulted durumunda olduğundan iletişim için kullanılamaz.
   > 
   > AzureAuthoringTools v 2.7.1 ve AzureComputeEmulator v 2.7 yeniden - bu sürümü ile eşleştiğinden emin olun.

2. Bir tarayıcı açın ve gidin **http://127.0.0.1** .
3. Tarayıcı penceresi açıldığında bir takma ad girin ve ardından ENTER tuşuna basın.
   Bu, belirli bir takma ad ileti göndermek izin verir. Birden çok kullanıcı işlevselliğini test etmek için aynı URL'yi kullanarak ek tarayıcı pencerelerini açmak ve farklı takma adlarını girin.
   
   ![İki tarayıcı pencerelerini User1 ve kullanıcı2 Sohbet iletilerini görüntüleme](./media/cloud-services-nodejs-chat-app-socketio/socketio-8.png)
4. Uygulamayı test edildikten sonra aşağıdaki komutu gönderdikten tarafından öykünücü durdurun:
   
       PS C:\node\chatapp\WorkerRole1> Stop-AzureEmulator
5. Uygulamayı azure'a dağıtmak için **Publish-AzureServiceProject** cmdlet'i. Örneğin:
   
       PS C:\node\chatapp\WorkerRole1> Publish-AzureServiceProject -ServiceName mychatapp -Location "East US" -Launch
   
   > [!IMPORTANT]
   > Benzersiz bir ad kullandığınızdan emin olun, aksi takdirde yayımlama işlemi başarısız olur. Dağıtım tamamlandıktan sonra bir tarayıcı açın ve dağıtılmış hizmette gidin.
   > 
   > Sağlanan abonelik adı içeri aktarılan yayımlama profilinde yok bildiren bir hata alırsanız, indirin ve Azure'a dağıtmadan önce aboneliğinizin yayımlama profilinin içeri aktarın. Bkz: **uygulamayı azure'a dağıtma** bölümünü [Azure bulut hizmeti için bir Node.js uygulaması derleme ve dağıtma](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/)
   > 
   > 
   
   ![Azure üzerinde barındırılan hizmet gösteren bir tarayıcı penceresi][completed-app]
   
   > [!NOTE]
   > Sağlanan abonelik adı içeri aktarılan yayımlama profilinde yok bildiren bir hata alırsanız, indirin ve Azure'a dağıtmadan önce aboneliğinizin yayımlama profilinin içeri aktarın. Bkz: **uygulamayı azure'a dağıtma** bölümünü [Azure bulut hizmeti için bir Node.js uygulaması derleme ve dağıtma](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/)
   > 
   > 

Uygulamanız artık Azure'da çalışıyor ve sohbet iletileri Socket.IO kullanarak farklı istemciler arasında geçiş.

> [!NOTE]
> Kolaylık olması için bu örnek aynı örneğine bağlanan kullanıcılar arasında Sohbeti sınırlıdır. Bu bulut hizmeti, iki çalışan rolü örnekleri oluşturursa, kullanıcılar yalnızca aynı çalışan rolü örneğine bağlı kullanıcılarla sohbet mümkün anlamına gelir. Uygulamanın birden çok rol örneği ile çalışması için ölçeklendirmek için Service Bus gibi bir teknoloji Socket.IO deposu durumu örnekleri arasında paylaşmak için kullanabilirsiniz. Service Bus kuyrukları ve konuları kullanım örnekleri örnekler için bkz [Node.js GitHub deposu için Azure SDK'sı](https://github.com/WindowsAzure/azure-sdk-for-node).
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, bir Azure bulut hizmetinde bir temel sohbet uygulaması oluşturmayı öğrendiniz. Bu uygulamayı bir Azure Web sitesinde barındırmak nasıl öğrenmek için bkz. [bir Azure Web sitesinde Socket.IO ile Node.js sohbet uygulaması oluşturma][chatwebsite].

Daha fazla bilgi için Ayrıca bkz: [Node.js Geliştirici Merkezi](https://docs.microsoft.com/javascript/azure/?view=azure-node-latest).

[chatwebsite]: https://docs.microsoft.com/azure/cloud-services/cloud-services-nodejs-develop-deploy-app

[Azure SLA]: https://www.windowsazure.com/support/sla/
[Azure SDK for Node.js GitHub repository]: https://github.com/WindowsAzure/azure-sdk-for-node
[completed-app]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-10.png
[Azure SDK for Node.js]: https://www.windowsazure.com/develop/nodejs/
[Node.js Web Application]: https://www.windowsazure.com/develop/nodejs/tutorials/getting-started/
[Socket.IO GitHub deposu]: https://github.com/LearnBoost/socket.io/tree/0.9.14
[Azure Considerations]: #windowsazureconsiderations
[Hosting the Chat Example in a Worker Role]: #hostingthechatexampleinawebrole
[Summary and Next Steps]: #summary
[powershell-menu]: ./media/cloud-services-nodejs-chat-app-socketio/azure-powershell-start.png

[chat example]: https://github.com/LearnBoost/socket.io/tree/master/examples/chat
[chat-example-view]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-22.png


[chat-contents]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-5.png
[The-output-of-the-npm-install-command]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-7.png
[The output of the Publish-AzureService command]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-9.png


