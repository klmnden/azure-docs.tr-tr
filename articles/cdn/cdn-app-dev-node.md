---
title: "Node.js için Azure CDN SDK'sı ile çalışmaya başlama | Microsoft Docs"
description: "Azure CDN yönetmek için Node.js uygulamalarını yazmayı öğrenin."
services: cdn
documentationcenter: nodejs
author: zhangmanling
manager: erikre
editor: 
ms.assetid: c4bb6a61-de3d-4f0c-9dca-202554c43dfa
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 46ae8cd9775432d126cbde856c1fb06ea319297e
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2017
---
# <a name="get-started-with-azure-cdn-development"></a>Azure CDN ile geliştirmeye başlama
> [!div class="op_single_selector"]
> * [Node.js](cdn-app-dev-node.md)
> * [.NET](cdn-app-dev-net.md)
> 
> 

Kullanabileceğiniz [Node.js için Azure CDN SDK](https://www.npmjs.com/package/azure-arm-cdn) oluşturulması ve CDN profili ve uç noktaları Yönetimi otomatik hale getirmek için.  Bu öğreticide birkaç kullanılabilir işlemleri gösteren basit bir Node.js konsol uygulaması oluşturma aşamasından açıklanmaktadır.  Bu öğretici Azure CDN SDK'sını tüm yönlerini Node.js için ayrıntılı bir şekilde tanımlamak üzere tasarlanmamıştır.

Bu öğreticiyi tamamlamak için zaten yüklü olmalıdır [Node.js](http://www.nodejs.org) **4.x.x** veya üzeri yüklenir ve yapılandırılır.  Node.js uygulamanızı oluşturmak istediğiniz herhangi bir metin düzenleyicisi kullanabilirsiniz.  Bu öğretici yazmak için kullandığım [Visual Studio Code](https://code.visualstudio.com).  

> [!TIP]
> [Bu öğreticinin tamamlanan projeden](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74) MSDN'de indirilebilir.
> 
> 

[!INCLUDE [cdn-app-dev-prep](../../includes/cdn-app-dev-prep.md)]

## <a name="create-your-project-and-add-npm-dependencies"></a>Projenizi oluşturmak ve NPM bağımlılıkları ekleyin.
Bizim CDN profili için bir kaynak grubu oluşturduk artık ve CDN profili ve uç noktaları grubu içindeki yönetmek için Azure AD uygulama izni verilen göre biz uygulamamızı oluşturmaya başlayabilirsiniz.

Uygulamanızı depolamak için bir klasör oluşturun.  Geçerli yolda Node.js araçları ile bir konsoldan bu yeni klasöre konumunuzu ayarlamak ve projenizin yürüterek başlatma:

    npm init

Ardından, projenizin başlatılamadı sorular bir dizi sunulur.  İçin **giriş noktası**, Bu öğretici kullanır *app.js*.  Aşağıdaki örnekte my diğer seçenekleri görebilirsiniz.

![NPM init çıktı](./media/cdn-app-dev-node/cdn-npm-init.png)

Projemizin şimdi ile başlatılmış bir *packages.json* dosya.  Projemizin NPM paketlerinde bulunan bazı Azure kitaplıkları kullanmak zordur.  (Azure arm cd) Node.js için Azure istemci çalışma zamanı ' Node.js (ms-rest-azure) için ve Azure CDN istemci kitaplığı kullanacağız.  Bu proje için bağımlılıklar olarak ekleyelim.

    npm install --save ms-rest-azure
    npm install --save azure-arm-cdn

Paketleri tamamladıktan sonra yükleme, *package.json* dosyası bu örneğe (sürüm numaralarını farklı olabilir) benzer görünmelidir:

``` json
{
  "name": "cdn_node",
  "version": "1.0.0",
  "description": "Azure CDN Node.js tutorial project",
  "main": "app.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "Cam Soper",
  "license": "MIT",
  "dependencies": {
    "azure-arm-cdn": "^0.2.1",
    "ms-rest-azure": "^1.14.4"
  }
}
```

Son olarak, Metin Düzenleyicisi'ni kullanarak bir boş metin dosyası oluşturun ve bizim proje klasörü olarak kök Kaydet *app.js*.  Biz artık kod yazmaya başlamak hazırsınız.

## <a name="requires-constants-authentication-and-structure"></a>Sabitler, kimlik doğrulama ve yapısı gerektirir
İle *app.js* bizim düzenleyicisinde açın, şimdi yazılmış programımız temel yapısını Al.

1. "" NPM paketlerimize en üstünde aşağıdakileri gerektirir ile ekleyin:
   
    ``` javascript
    var msRestAzure = require('ms-rest-azure');
    var cdnManagementClient = require('azure-arm-cdn');
    ```
2. Bizim yöntemler kullanacağı bazı sabitleri tanımlamak gerekir.  Aşağıdakileri ekleyin.  Dahil olmak üzere, yer tutucular değiştirdiğinizden emin olun  **&lt;köşeli&gt;**, gerektiği gibi kendi değerlere sahip.
   
    ``` javascript
    //Tenant app constants
    const clientId = "<YOUR CLIENT ID>";
    const clientSecret = "<YOUR CLIENT AUTHENTICATION KEY>"; //Only for service principals
    const tenantId = "<YOUR TENANT ID>";
   
    //Application constants
    const subscriptionId = "<YOUR SUBSCRIPTION ID>";
    const resourceGroupName = "CdnConsoleTutorial";
    const resourceLocation = "<YOUR PREFERRED AZURE LOCATION, SUCH AS Central US>";
    ```
3. Ardından, CDN management istemcisi örneği ve bizim kimlik bilgileri verin.
   
    ``` javascript
    var credentials = new msRestAzure.ApplicationTokenCredentials(clientId, tenantId, clientSecret);
    var cdnClient = new cdnManagementClient(credentials, subscriptionId);
    ```
   
    Tek tek kullanıcı kimlik doğrulaması kullanıyorsanız, bu iki satır biraz farklı görünecektir.
   
   > [!IMPORTANT]
   > Yalnızca bireysel kullanıcı kimlik doğrulaması, bir hizmet sorumlusu yerine seçmek, bu kod örneği kullanın.  Bireysel kullanıcı kimlik bilgileri koruması ve gizli tutmak dikkatli olun.
   > 
   > 
   
    ``` javascript
    var credentials = new msRestAzure.UserTokenCredentials(clientId, 
        tenantId, '<username>', '<password>', '<redirect URI>');
    var cdnClient = new cdnManagementClient(credentials, subscriptionId);
    ```
   
    Öğeleri değiştirdiğinizden emin olun  **&lt;köşeli&gt;**  doğru bilgileri.  İçin `<redirect URI>`, yeniden yönlendirme URI'si Azure AD'de uygulama kaydolurken girdiğiniz kullanın.
4. Bazı komut satırı parametreleri yapılacak bizim Node.js konsol uygulaması geçiyor.  Şimdi en az bir doğrulama parametresi geçirildi.
   
   ```javascript
   //Collect command-line parameters
   var parms = process.argv.slice(2);
   
   //Do we have parameters?
   if(parms == null || parms.length == 0)
   {
       console.log("Not enough parameters!");
       console.log("Valid commands are list, delete, create, and purge.");
       process.exit(1);
   }
   ```
5. Bize burada size hangi parametreler geçirilmiş bağlı diğer işlevleri dallanma programımız ana kısmına beraberinde getirir.
   
    ```javascript
    switch(parms[0].toLowerCase())
    {
        case "list":
            cdnList();
            break;
   
        case "create":
            cdnCreate();
            break;
   
        case "delete":
            cdnDelete();
            break;
   
        case "purge":
            cdnPurge();
            break;
   
        default:
            console.log("Valid commands are list, delete, create, and purge.");
            process.exit(1);
    }
    ```
6. Programımız çeşitli yerlerde biz doğru sayıda parametre olarak geçirilen ve doğru görünmüyorsa bazı Yardım görüntüleme emin olmak gerekir.  Bunu yapmak için işlevleri oluşturalım.
   
   ```javascript
   function requireParms(parmCount) {
       if(parms.length < parmCount) {
           usageHelp(parms[0].toLowerCase());
           process.exit(1);
       }
   }
   
   function usageHelp(cmd) {
       console.log("Usage for " + cmd + ":");
       switch(cmd)
       {
           case "list":
               console.log("list profiles");
               console.log("list endpoints <profile name>");
               break;
   
           case "create":
               console.log("create profile <profile name>");
               console.log("create endpoint <profile name> <endpoint name> <origin hostname>");
               break;
   
           case "delete":
               console.log("delete profile <profile name>");
               console.log("delete endpoint <profile name> <endpoint name>");
               break;
   
           case "purge":
               console.log("purge <profile name> <endpoint name> <path>");
               break;
   
           default:
               console.log("Invalid command.");
       }
   }
   ```
7. Son olarak, bu nedenle geri işiniz bittiğinde çağrılacak bir yöntem gerekir biz CDN yönetim istemcisini kullanmaya başlayacağınız işlevleri zaman uyumsuz,.  CDN Yönetimi istemcisi (varsa) çıkışı görüntülemek ve program sessizce Çık olalım.
   
    ```javascript
    function callback(err, result, request, response) {
        if (err) {
            console.log(err);
            process.exit(1);
        } else {
            console.log((result == null) ? "Done!" : result);
            process.exit(0);
        }
    }
    ```

Programımız temel yapısını yazılır, biz bizim parametrelere göre çağrılan işlevler oluşturmanız gerekir.

## <a name="list-cdn-profiles-and-endpoints"></a>Liste CDN profili ve uç noktaları
Bizim varolan profil ve uç noktaları listesinde koduyla başlayalım.  My kod açıklamaları beklenen sözdizimi sağlar, bu nedenle, her bir parametreyi gider biliyoruz.

```javascript
// list profiles
// list endpoints <profile name>
function cdnList(){
    requireParms(2);
    switch(parms[1].toLowerCase())
    {
        case "profiles":
            console.log("Listing profiles...");
            cdnClient.profiles.listByResourceGroup(resourceGroupName, callback);
            break;

        case "endpoints":
            requireParms(3);
            console.log("Listing endpoints...");
            cdnClient.endpoints.listByProfile(parms[2], resourceGroupName, callback);
            break;

        default:
            console.log("Invalid parameter.");
            process.exit(1);
    }
}
```

## <a name="create-cdn-profiles-and-endpoints"></a>CDN profili ve uç noktaları oluşturma
Ardından, biz profilleri ve uç noktaları oluşturmak için işlevleri yazacaksınız.

```javascript
function cdnCreate() {
    requireParms(2);
    switch(parms[1].toLowerCase())
    {
        case "profile":
            cdnCreateProfile();
            break;

        case "endpoint":
            cdnCreateEndpoint();
            break;

        default:
            console.log("Invalid parameter.");
            process.exit(1);
    }
}

// create profile <profile name>
function cdnCreateProfile() {
    requireParms(3);
    console.log("Creating profile...");
    var standardCreateParameters = {
        location: resourceLocation,
        sku: {
            name: 'Standard_Verizon'
        }
    };

    cdnClient.profiles.create(parms[2], standardCreateParameters, resourceGroupName, callback);
}

// create endpoint <profile name> <endpoint name> <origin hostname>        
function cdnCreateEndpoint() {
    requireParms(5);
    console.log("Creating endpoint...");
    var endpointProperties = {
        location: resourceLocation,
        origins: [{
            name: parms[4],
            hostName: parms[4]
        }]
    };

    cdnClient.endpoints.create(parms[3], endpointProperties, parms[2], resourceGroupName, callback);
}
```

## <a name="purge-an-endpoint"></a>Bir uç noktası
Uç noktası oluşturuldu varsayıldığında, biz bizim programında gerçekleştirmek isteyebileceğiniz ortak görev bizim endpoint içeriğinde temizleme.

```javascript
// purge <profile name> <endpoint name> <path>
function cdnPurge() {
    requireParms(4);
    console.log("Purging endpoint...");
    var purgeContentPaths = [ parms[3] ];
    cdnClient.endpoints.purgeContent(parms[2], parms[1], resourceGroupName, purgeContentPaths, callback);
}
```

## <a name="delete-cdn-profiles-and-endpoints"></a>CDN profili ve uç noktaları Sil
Biz içerecektir son işlevi uç noktaları ve profilleri siler.

```javascript
function cdnDelete() {
    requireParms(2);
    switch(parms[1].toLowerCase())
    {
        // delete profile <profile name>
        case "profile":
            requireParms(3);
            console.log("Deleting profile...");
            cdnClient.profiles.deleteIfExists(parms[2], resourceGroupName, callback);
            break;

        // delete endpoint <profile name> <endpoint name>
        case "endpoint":
            requireParms(4);
            console.log("Deleting endpoint...");
            cdnClient.endpoints.deleteIfExists(parms[3], parms[2], resourceGroupName, callback);
            break;

        default:
            console.log("Invalid parameter.");
            process.exit(1);
    }
}
```

## <a name="running-the-program"></a>Programın çalıştırılması
Biz şimdi sık kullanılan bizim hata ayıklayıcıyı kullanma Node.js programımız yürütebilir veya konsolda.

> [!TIP]
> Visual Studio Code, hata ayıklayıcı olarak kullanıyorsanız, komut satırı parametreleri ortamınızı ayarlamanız gerekir.  Visual Studio Code yaptığı bu da **lanuch.json** dosya.  Adlı bir özellik arayın **args** ve dize değerleri, parametre için bir dizi ekleyebilirsiniz, böylece şuna benzer: `"args": ["list", "profiles"]`.
> 
> 

Bizim profilleri listeleyerek başlayalım.

![Liste profilleri](./media/cdn-app-dev-node/cdn-list-profiles.png)

Biz, boş bir dizi geri aldı.  Herhangi bir profil bizim kaynak grubunda yok olduğundan, bu beklenir.  Artık bir profili oluşturalım.

![Profil oluşturma](./media/cdn-app-dev-node/cdn-create-profile.png)

Şimdi, bir uç nokta ekleyelim.

![Uç noktası oluşturma](./media/cdn-app-dev-node/cdn-create-endpoint.png)

Son olarak, şirketinizdeki bizim profilini silin.

![Profili Sil](./media/cdn-app-dev-node/cdn-delete-profile.png)

## <a name="next-steps"></a>Sonraki Adımlar
Bu kılavuzda tamamlanmış projeden görmek için [örneği indirmek](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74).

Başvuru için Node.js için Azure CDN SDK'sı görmek için görüntüleyin [başvuru](http://azure.github.io/azure-sdk-for-node/azure-arm-cdn/latest/).

Node.js için Azure SDK'sı üzerinde ek belgeleri bulmak için görüntülemek [tam başvuru](http://azure.github.io/azure-sdk-for-node/).

CDN kaynaklarınızı yönetmek [PowerShell](cdn-manage-powershell.md).

