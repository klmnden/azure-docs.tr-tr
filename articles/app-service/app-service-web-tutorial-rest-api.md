---
title: "Azure Uygulama Hizmeti’ndeki Node.js API uygulaması | Microsoft Belgeleri"
description: "Node.js RESTful API’si oluşturma ve Azure App Service’deki bir API uygulamasına dağıtma hakkında bilgi edinin."
services: app-service\api
documentationcenter: node
author: bradygaster
manager: erikre
editor: 
ms.assetid: a820e400-06af-4852-8627-12b3db4a8e70
ms.service: app-service-api
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 06/13/2017
ms.author: rachelap
ms.custom: mvc, devcenter
ms.openlocfilehash: 81d08e047a3689d110195f2325b52c6c0457e644
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2018
---
# <a name="build-a-nodejs-restful-api-and-deploy-it-to-an-api-app-in-azure"></a>Node.js RESTful API’si derleme ve Azure’daki bir API uygulamasına dağıtma

Bu hızlı başlangıç kılavuzunda, [Swagger](http://swagger.io/) tanımı kullanarak Node.js [Express](http://expressjs.com/) ile yazılmış bir REST API’nin nasıl oluşturulacağı ve bunun Azure’da nasıl dağıtılacağı gösterilmiştir. Komut satırı araçlarını kullanarak uygulamayı oluşturur, [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) ile kaynakları yapılandırır ve Git kullanarak uygulamayı dağıtırsınız.  İşiniz bittiğinde, Azure’da çalışan bir REST API örneğiniz olur.

## <a name="prerequisites"></a>Önkoşullar

* [Git](https://git-scm.com/)
* [Node.js ve NPM](https://nodejs.org/)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu konu, Azure CLI 2.0 veya sonraki bir sürümünü kullanmanızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

## <a name="prepare-your-environment"></a>Ortamınızı hazırlama

1. Bir terminal penceresinde, aşağıdaki komutu çalıştırarak örneği yerel makinenize kopyalayın.

    ```bash
    git clone https://github.com/Azure-Samples/app-service-api-node-contact-list
    ```

2. Örnek kodu içeren dizine geçin.

    ```bash
    cd app-service-api-node-contact-list
    ```

3. Yerel makinenize [Swaggerize](https://www.npmjs.com/package/swaggerize-express)’ı yükleyin. Swaggerize, Swagger tanımından REST API’niz için Node.js kodu oluşturan bir araçtır.

    ```bash
    npm install -g yo
    npm install -g generator-swaggerize
    ```

## <a name="generate-nodejs-code"></a>Node.js kodu oluşturma 

Öğreticinin bu bölümü, içinde ilk olarak Swagger meta verilerini oluşturduğunuz ve bunları API sunucu kodunun iskelesini kurmak (otomatik oluşturmak) için kullandığınız bir API geliştirme iş akışını modeller. 

Dizini *başlat* klasörü olarak değiştirin, ardından `yo swaggerize` öğesini çalıştırın. Swaggerize, *api.json*’da Swagger tanımından API’niz için bir Node.js projesi oluşturur.

```bash
cd start
yo swaggerize --apiPath api.json --framework express
```

Swaggerize bir proje adı sorduğunda *ContactList* adını kullanın.
   
   ```bash
   Swaggerize Generator
   Tell us a bit about your application
   ? What would you like to call this project: ContactList
   ? Your name: Francis Totten
   ? Your github user name: fabfrank
   ? Your email: frank@fabrikam.net
   ```
   
## <a name="customize-the-project-code"></a>Proje kodunu özelleştirme

1. *lib* klasörünü, `yo swaggerize` tarafından oluşturulan *ContactList* klasörüne kopyalayın, ardından dizini *ContactList* ile değiştirin.

    ```bash
    cp -r lib ContactList/
    cd ContactList
    ```

2. `jsonpath` ve `swaggerize-ui` NPM modüllerini yükleyin. 

    ```bash
    npm install --save jsonpath swaggerize-ui
    ```

3. *handlers/contacts.js*’deki kodu aşağıdaki kodla değiştirin: 
    ```javascript
    'use strict';

    var repository = require('../lib/contactRepository');

    module.exports = {
        get: function contacts_get(req, res) {
            res.json(repository.all())
        }
    };
    ```
    Bu kod, *lib/contactRepository.js* tarafından sunulan *lib/contacts.json*’da depolanmış JSON verilerini kullanır. Yeni *contacts.js* kodu, depodaki tüm kişileri JSON yükü olarak döndürür. 

4. **handlers/contacts/{id}.js** dosyasındaki kodu, aşağıdaki kodla değiştirin:

    ```javascript
    'use strict';

    var repository = require('../../lib/contactRepository');

    module.exports = {
        get: function contacts_get(req, res) {
            res.json(repository.get(req.params['id']));
        }    
    };
    ```

    Bu kod, yalnızca belirtilen kimliğe sahip kişiyi döndürmek için bir yol değişkeni kullanmanıza imkan tanır.

5. **server.js**’deki kodu, aşağıdaki kodla değiştirin:

    ```javascript
    'use strict';

    var port = process.env.PORT || 8000; 

    var http = require('http');
    var express = require('express');
    var bodyParser = require('body-parser');
    var swaggerize = require('swaggerize-express');
    var swaggerUi = require('swaggerize-ui'); 
    var path = require('path');
    var fs = require("fs");
    
    fs.existsSync = fs.existsSync || require('path').existsSync;

    var app = express();

    var server = http.createServer(app);

    app.use(bodyParser.json());

    app.use(swaggerize({
        api: path.resolve('./config/swagger.json'),
        handlers: path.resolve('./handlers'),
        docspath: '/swagger' 
    }));

    // change four
    app.use('/docs', swaggerUi({
        docs: '/swagger'  
    }));

    server.listen(port, function () { 
    });
    ```   

    Bu kod, Azure App Service ile çalışmasını sağlamak için birkaç küçük değişiklik yapar ve API’niz için etkileşimli bir web arabirimini kullanıma sunar.

### <a name="test-the-api-locally"></a>API’yi yerel olarak test etme

1. Node.js uygulamasını başlatma
    ```bash
    npm start
    ```
    
2. Kişi listesinin tamamı için JSON’u görüntülemek üzere http://localhost:8000/contacts’a göz atın.
   
   ```json
    {
        "id": 1,
        "name": "Barney Poland",
        "email": "barney@contoso.com"
    },
    {
        "id": 2,
        "name": "Lacy Barrera",
        "email": "lacy@contoso.com"
    },
    {
        "id": 3,
        "name": "Lora Riggs",
        "email": "lora@contoso.com"
    }
   ```

3. Kişiyi iki `id` arasından biriyle görüntülemek için http://localhost:8000/contacts/2’ye göz atın.
   
    ```json
    { 
        "id": 2,
        "name": "Lacy Barrera",
        "email": "lacy@contoso.com"
    }
    ```

4. http://localhost:8000/docs’ta Swagger web arabirimini kullanarak API’yi test edin.
   
    ![Swagger web arabirimi](media/app-service-web-tutorial-rest-api/swagger-ui.png)

## <a id="createapiapp"></a> API Uygulaması oluşturma

Bu bölümde, API’yi Azure App Service'te barındırmak için gereken kaynakları oluşturmak amacıyla Azure CLI 2.0’ı kullanırsınız. 

1.  [az login](/cli/azure/?view=azure-cli-latest#az_login) komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

    ```azurecli-interactive
    az login
    ```

2. Birden çok Azure aboneliğiniz varsa varsayılan aboneliği istediğiniz abonelikle değiştirin.

    ````azurecli-interactive
    az account set --subscription <name or id>
    ````

3. [!INCLUDE [Create resource group](../../includes/app-service-api-create-resource-group.md)] 

4. [!INCLUDE [Create app service plan](../../includes/app-service-api-create-app-service-plan.md)]

5. [!INCLUDE [Create API app](../../includes/app-service-api-create-api-app.md)] 


## <a name="deploy-the-api-with-git"></a>API’yi Git ile dağıtma

İşlemeleri yerel Git deponuzdan Azure App Service’e ileterek kodunuzu API uygulamasına dağıtırsınız.

1. [!INCLUDE [Configure your deployment credentials](../../includes/configure-deployment-user-no-h.md)] 

2. *ContactList* dizininde yeni bir depo başlatın. 

    ```bash
    git init .
    ```

3. Git’teki öğreticinin önceki bir adımında npm tarafından oluşturulan *node_modules* dizinini hariç tutun. Geçerli dizinde yeni bir `.gitignore` dosyası oluşturun ve dosyanın herhangi bir yerindeki yeni bir satıra aşağıdaki metni ekleyin.

    ```
    node_modules/
    ```
    `node_modules` klasörünün `git status` ile yoksayıldığını onaylayın.
    
4. İçine aşağıdaki satırları ekleyin `package.json`. Swaggerize tarafından oluşturulan kodu Node.js altyapısı için bir sürüm belirtmiyor. Sürüm belirtimi Azure varsayılan sürümü kullanan `0.10.18`, hangi oluşturulan kodu ile uyumlu değil.

    ```javascript
    "engines": {
        "node": "~0.10.22"
    },
    ```

5. Değişiklikleri depoya uygulayın.
    ```bash
    git add .
    git commit -m "initial version"
    ```

6. [!INCLUDE [Push to Azure](../../includes/app-service-api-git-push-to-azure.md)]  
 
## <a name="test-the-api--in-azure"></a>Azure'da API’yi test etme

1. Tarayıcıda http://app_name.azurewebsites.net/contacts adresini açın. Öğreticide daha önce yerel olarak istek yaptığınızda gördüğünüz JSON’un döndürüldüğünü göreceksiniz.

   ```json
   {
       "id": 1,
       "name": "Barney Poland",
       "email": "barney@contoso.com"
   },
   {
       "id": 2,
       "name": "Lacy Barrera",
       "email": "lacy@contoso.com"
   },
   {
       "id": 3,
       "name": "Lora Riggs",
       "email": "lora@contoso.com"
   }
   ```

2. Bir tarayıcıda, `http://app_name.azurewebsites.net/docs` uç noktasına giderek Azure'da çalışan Swagger kullanıcı arabirimini deneyin.

    ![Swagger Ii](media/app-service-web-tutorial-rest-api/swagger-azure-ui.png)

    Artık işlemleri Azure Git deposuna ileterek örnek API güncelleştirmelerini Azure’a dağıtabilirsiniz.

## <a name="clean-up"></a>Temizleme

Bu hızlı başlangıçta oluşturulan kaynakları kaldırmak için şu Azure CLI komutunu çalıştırın:

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-step"></a>Sonraki adım 
> [!div class="nextstepaction"]
> [Mevcut bir özel DNS adını Azure Web Apps ile eşleme](app-service-web-tutorial-custom-domain.md)

