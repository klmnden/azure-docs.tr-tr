---
title: Derleme ve Azure Cloud Services için bir Node.js Express uygulaması dağıtma
description: Derleme ve Azure Cloud Services için bir Node.js, Express.js uygulaması dağıtma
services: cloud-services
documentationcenter: nodejs
author: jpconnock
manager: timlt
editor: ''
ms.assetid: 24f8e7ef-e90d-4554-9b1e-a9b31d5824e5
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2017
ms.author: jeconnoc
ms.openlocfilehash: b212eaffb977846d40270a5f2abc76192aee4c0d
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60528150"
---
# <a name="build-and-deploy-a-nodejs-web-application-using-express-on-an-azure-cloud-services"></a>Express kullanarak bir Azure bulut Hizmetleri'nde bir Node.js web uygulaması derleme ve dağıtma

Node.js core çalışma zamanı'nda en az bir dizi işlev içerir.
Geliştiriciler genellikle bir Node.js uygulaması geliştirilirken ek işlevler sağlamasına olanak 3. taraf modüllerini kullanın. Bu öğreticide kullanarak yeni bir uygulama oluşturacaksınız [Express](https://github.com/expressjs/express) modülü Node.js web uygulamaları oluşturmaya yönelik MVC çerçevesini sağlar.

Tamamlanmış uygulamanın bir ekran görüntüsü aşağıda verilmiştir:

![Azure'da Express Karşılama görüntüleyen bir web tarayıcısı](./media/cloud-services-nodejs-develop-deploy-express-app/node36.png)

## <a name="create-a-cloud-service-project"></a>Bir bulut hizmeti projesi oluşturma
[!INCLUDE [install-dev-tools](../../includes/install-dev-tools.md)]

'Expressapp' adlı yeni bir bulut hizmeti projesi oluşturmak için aşağıdaki adımları gerçekleştirin:

1. Gelen **Başlat menüsü** veya **Başlangıç ekranına**, arama **Windows PowerShell**. Son olarak, sağ **Windows PowerShell** seçip **yönetici olarak çalıştır**.
   
    ![Azure PowerShell simgesi](./media/cloud-services-nodejs-develop-deploy-express-app/azure-powershell-start.png)
2. Dizinleri **c:\\düğüm** dizin adlı yeni bir çözüm oluşturmak için aşağıdaki komutları girin **expressapp** ve adlı bir web rolü **WebRole1**:
   
        PS C:\node> New-AzureServiceProject expressapp
        PS C:\Node\expressapp> Add-AzureNodeWebRole
        PS C:\Node\expressapp> Set-AzureServiceProjectRole WebRole1 Node 0.10.21
   
    > [!NOTE]
    > Varsayılan olarak, **Add-AzureNodeWebRole** Node.js daha eski bir sürümünü kullanır. **Kümesi AzureServiceProjectRole** deyimi yukarıdaki v0.10.21 düğümünün kullanmak için Azure bildirir.  Parametreler büyük/küçük harfe duyarlıdır unutmayın.  Node.js doğru sürümünü denetleyerek seçilmiş doğrulayabilirsiniz **altyapıları** özelliğinde **WebRole1\package.json**.
    > 
    > 

## <a name="install-express"></a>Express yükle
1. Hızlı Oluşturucu, aşağıdaki komutu göndererek yükleyin:
   
        PS C:\node\expressapp> npm install express-generator -g
   
    Npm komutu çıkışını sonuç aşağıdaki gibi görünmelidir. 
   
    ![Npm çıktısını gösteren Windows PowerShell yükleme hızlı komutu.](./media/cloud-services-nodejs-develop-deploy-express-app/express-g.png)
2. Dizinleri **WebRole1** dizin ve yeni bir uygulama oluşturmak için hızlı komutunu kullanın:
   
        PS C:\node\expressapp\WebRole1> express
   
    Uygulamanızı önceki üzerine istenir. Girin **y** veya **Evet** devam etmek için. Express app.js dosyasındaki ve uygulamanızı oluşturmak için bir klasör yapısı oluşturur.
   
    ![Hızlı komut çıktısı](./media/cloud-services-nodejs-develop-deploy-express-app/node23.png)
3. Package.json dosyasında tanımlı Ek Bağımlılıklar'ı yüklemek için aşağıdaki komutu girin:
   
       PS C:\node\expressapp\WebRole1> npm install
   
   ![Çıkış npm yükleme komutu](./media/cloud-services-nodejs-develop-deploy-express-app/node26.png)
4. Kopyalamak için aşağıdaki komutu kullanın **bin/www** dosyasını **server.js**. Bulut hizmeti, bu uygulama için giriş noktası bulabilmesi için budur.
   
       PS C:\node\expressapp\WebRole1> copy bin/www server.js
   
   Bu komut tamamlandıktan sonra olmalıdır bir **server.js** WebRole1 dizindeki dosya.
5. Değiştirme **server.js** birini kaldırmak için '.' karakteri aşağıdaki satırı.
   
       var app = require('../app');
   
   Bu değişikliği yaptıktan sonra satırı aşağıdaki gibi görünmelidir.
   
       var app = require('./app');
   
   Dosya geçtiğimizi olduğundan bu değişikliği gerekli değildir (eski adıyla **bin/www**,) için gerekli uygulama dosyası ile aynı dizinde. Bu değişikliği yaptıktan sonra Kaydet **server.js** dosya.
6. Azure öykünücüsünde uygulamayı çalıştırmak için aşağıdaki komutu kullanın:
   
       PS C:\node\expressapp\WebRole1> Start-AzureEmulator -launch
   
    ![Express Hoş Geldiniz içeren bir web sayfası.](./media/cloud-services-nodejs-develop-deploy-express-app/node28.png)

## <a name="modifying-the-view"></a>Görünümü değiştirme
Artık "Hoş Geldiniz için Express içinde Azure" iletisini görüntülemek için görünümü değiştirin.

1. İndex.jade dosyayı açmak için aşağıdaki komutu girin:
   
       PS C:\node\expressapp\WebRole1> notepad views/index.jade
   
   ![İndex.jade dosyasının içeriği.](./media/cloud-services-nodejs-develop-deploy-express-app/getting-started-19.png)
   
   Jade Express uygulamaları tarafından kullanılan varsayılan görünüm altyapısıdır. Jade görünüm altyapısı hakkında daha fazla bilgi için bkz. [ http://jade-lang.com ] [ http://jade-lang.com].
2. Son metin satırının ekleyerek değiştirme **azure'da**.
   
   ![Son satırı index.jade dosyasını okur: p Hoş Geldiniz için \#{title} Azure'da](./media/cloud-services-nodejs-develop-deploy-express-app/node31.png)
3. Dosyayı kaydedin ve Not Defteri'nden çıkın.
4. Tarayıcınızı yenileyin ve değişikliklerinizi görürsünüz.
   
   ![Bir tarayıcı penceresi sayfasına Hoş Geldiniz Azure Express içerir.](./media/cloud-services-nodejs-develop-deploy-express-app/node32.png)

Uygulamayı test etmek sonra **Stop-AzureEmulator** öykünücüsü'nü durdurmak için cmdlet.

## <a name="publishing-the-application-to-azure"></a>Uygulamayı azure'a yayımlama
Azure PowerShell penceresinde **Publish-AzureServiceProject** cmdlet'i bir bulut hizmeti uygulamayı dağıtmak için

    PS C:\node\expressapp\WebRole1> Publish-AzureServiceProject -ServiceName myexpressapp -Location "East US" -Launch

Dağıtım işlemi tamamlandıktan sonra tarayıcınızı açın ve web sayfasını görüntüleyin.

![Express sayfasını gösteren bir web tarayıcısı. URL, artık Azure'da barındırıldığını gösterir.](./media/cloud-services-nodejs-develop-deploy-express-app/node36.png)

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için bkz. [Node.js Geliştirici Merkezi](https://docs.microsoft.com/javascript/azure/?view=azure-node-latest).

[Node.js Web Application]: https://www.windowsazure.com/develop/nodejs/tutorials/getting-started/
[Express]: https://expressjs.com/
[http://jade-lang.com]: http://jade-lang.com


