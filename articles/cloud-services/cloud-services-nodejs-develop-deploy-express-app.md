---
title: "Web uygulaması Express (Node.js) ile | Microsoft Docs"
description: "Bulut hizmeti öğretici oluşturur ve Express modülü kullanmak nasıl gösteren bir öğretici."
services: cloud-services
documentationcenter: nodejs
author: TomArcher
manager: routlaw
editor: 
ms.assetid: 24f8e7ef-e90d-4554-9b1e-a9b31d5824e5
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2017
ms.author: tarcher
ms.openlocfilehash: 54b715695e24786ec4e8dfcabefc648d76179c8b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="build-a-nodejs-web-application-using-express-on-an-azure-cloud-service"></a>Bir Azure bulut hizmeti Express kullanarak bir Node.js web uygulaması oluşturma
Node.js işlevselliği en az bir dizi çekirdek çalışma zamanı'nda içerir.
Geliştiriciler 3 taraf modülleri bir Node.js uygulaması geliştirilirken ek işlevsellik sağlamak için çoğunlukla kullanır. Bu öğreticide kullanarak yeni bir uygulama oluşturacaksınız [Express] [ Express] modül Node.js web uygulamaları oluşturmak için bir MVC çerçevesi sağlar.

Tamamlanmış uygulamanın bir ekran görüntüsü aşağıda verilmiştir:

![Hoş Geldiniz Azure Express'te gösteren bir web tarayıcısı](./media/cloud-services-nodejs-develop-deploy-express-app/node36.png)

## <a name="create-a-cloud-service-project"></a>Bir bulut hizmeti projesi oluşturma
[!INCLUDE [install-dev-tools](../../includes/install-dev-tools.md)]

'Expressapp' adlı yeni bir bulut hizmeti projesi oluşturmak için aşağıdaki adımları gerçekleştirin:

1. Gelen **Başlat menüsü** veya **Başlat ekranında**, arama **Windows PowerShell**. Son olarak, sağ **Windows PowerShell** seçip **yönetici olarak çalıştır**.
   
    ![Azure PowerShell simgesi](./media/cloud-services-nodejs-develop-deploy-express-app/azure-powershell-start.png)
2. Değiştirme dizinleri **c:\\düğümü** dizin ve adlı yeni bir çözüm oluşturmak için aşağıdaki komutları girin **expressapp** ve adında bir web rolü **WebRole1**:
   
        PS C:\node> New-AzureServiceProject expressapp
        PS C:\Node\expressapp> Add-AzureNodeWebRole
        PS C:\Node\expressapp> Set-AzureServiceProjectRole WebRole1 Node 0.10.21
   
    > [!NOTE]
    > Varsayılan olarak, **Add-AzureNodeWebRole** Node.js daha eski bir sürümü kullanır. **Kümesi AzureServiceProjectRole** yukarıdaki ifade v0.10.21 düğümünün kullanmak için Azure bildirir.  Parametreleri büyük/küçük harfe duyarlıdır unutmayın.  Node.js doğru sürümü seçilmiştir denetleyerek doğrulayabilirsiniz **motorları** özelliğinde **WebRole1\package.json**.
    > 
    > 

## <a name="install-express"></a>Hızlı yükleme
1. Hızlı Oluşturucu, aşağıdaki komutu gönderdikten yükleyin:
   
        PS C:\node\expressapp> npm install express-generator -g
   
    Npm komutunun çıkışını aşağıdaki sonucu benzer görünmelidir. 
   
    ![Windows PowerShell npm çıktısını görüntüleme yükleme express komutu.](./media/cloud-services-nodejs-develop-deploy-express-app/express-g.png)
2. Değiştirme dizinleri **WebRole1** dizin ve yeni bir uygulama oluşturmak için hızlı komutunu kullanın:
   
        PS C:\node\expressapp\WebRole1> express
   
    Önceki uygulamanızı üzerine istenir. Girin **y** veya **Evet** devam etmek için. Express app.js dosya ve uygulamanızı oluşturmak için bir klasör yapısı oluşturur.
   
    ![Hızlı komut çıktısı](./media/cloud-services-nodejs-develop-deploy-express-app/node23.png)
3. Package.json dosyasında tanımlanan ek bağımlılıklar yüklemek için aşağıdaki komutu girin:
   
       PS C:\node\expressapp\WebRole1> npm install
   
   ![Npm çıktısını yükleme komutu](./media/cloud-services-nodejs-develop-deploy-express-app/node26.png)
4. Kopyalamak için aşağıdaki komutu kullanın **bin/www** dosya **server.js**. Bulut hizmeti, bu uygulama için giriş noktası bulabilmesi budur.
   
       PS C:\node\expressapp\WebRole1> copy bin/www server.js
   
   Bu komut tamamlandıktan sonra olmalıdır bir **server.js** WebRole1 dizindeki dosyayı.
5. Değiştirme **server.js** birini kaldırmak için '.' aşağıdaki satırı karakterlerinden.
   
       var app = require('../app');
   
   Bu değişikliği yaptıktan sonra satırı aşağıdaki gibi görünmelidir.
   
       var app = require('./app');
   
   Biz dosya taşınmış olduğundan bu değişikliği gerekli değildir (önceki adıyla **bin/www**,) için gerekli uygulama dosyası ile aynı dizinde. Bu değişikliği yaptıktan sonra kaydedin **server.js** dosya.
6. Uygulama Azure öykünücüsünde çalıştırmak için aşağıdaki komutu kullanın:
   
       PS C:\node\expressapp\WebRole1> Start-AzureEmulator -launch
   
    ![Express Hoş Geldiniz içeren bir web sayfası.](./media/cloud-services-nodejs-develop-deploy-express-app/node28.png)

## <a name="modifying-the-view"></a>Görünümü değiştirme
Şimdi "Hoş Geldiniz için Express içinde Azure" iletisini görüntülemek için görünümü değiştirin.

1. İndex.jade dosyayı açmak için aşağıdaki komutu girin:
   
       PS C:\node\expressapp\WebRole1> notepad views/index.jade
   
   ![İndex.jade dosyasının içeriği.](./media/cloud-services-nodejs-develop-deploy-express-app/getting-started-19.png)
   
   Jade Express uygulamaları tarafından kullanılan varsayılan görünüm altyapısıdır. Jade görünüm altyapısı hakkında daha fazla bilgi için bkz: [http://jade-lang.com][http://jade-lang.com].
2. Metnin son satırını ekleyerek değiştirmek **azure'da**.
   
   ![Son satırı index.jade dosyasını okur: p Hoş Geldiniz için \#{title} Azure'da](./media/cloud-services-nodejs-develop-deploy-express-app/node31.png)
3. Dosyayı kaydedin ve Not Defteri'nden çıkın.
4. Tarayıcınızı yenileyin ve değişikliklerinizi görürsünüz.
   
   ![Bir tarayıcı penceresi Sayfası'na Hoş Geldiniz Azure Express'te içerir.](./media/cloud-services-nodejs-develop-deploy-express-app/node32.png)

Uygulamayı test etme sonra kullanın **Stop-AzureEmulator** öykünücü durdurmak için cmdlet.

## <a name="publishing-the-application-to-azure"></a>Uygulamayı Azure'a yayımlama
Azure PowerShell penceresinde kullanmak **Publish-AzureServiceProject** bir bulut hizmeti uygulamayı dağıtmak için cmdlet

    PS C:\node\expressapp\WebRole1> Publish-AzureServiceProject -ServiceName myexpressapp -Location "East US" -Launch

Dağıtım işlemi tamamlandıktan sonra tarayıcınızı açın ve web sayfasını görüntüleyin.

![Express sayfasını gösteren bir web tarayıcısı. Artık Azure üzerinde barındırılan URL'yi belirtir.](./media/cloud-services-nodejs-develop-deploy-express-app/node36.png)

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için bkz. [Node.js Geliştirici Merkezi](/develop/nodejs/).

[Node.js Web Application]: http://www.windowsazure.com/develop/nodejs/tutorials/getting-started/
[Express]: http://expressjs.com/
[http://jade-lang.com]: http://jade-lang.com


