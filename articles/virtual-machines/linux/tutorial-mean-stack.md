---
title: "Bir ORTALAMASI oluşturma azure'da bir Linux VM yığında | Microsoft Docs"
description: "Azure'da bir Linux VM üzerinde MongoDB, Express, AngularJS ve Node.js (ortalama) yığın oluşturmayı öğrenin."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/08/2017
ms.author: davidmu
ms.custom: mvc
ms.openlocfilehash: 1d74ead08dfb63276afb08bdcb7f4e3e3db5bfd3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-mongodb-express-angularjs-and-nodejs-mean-stack-on-a-linux-vm-in-azure"></a>MongoDB, Express, AngularJS ve Node.js (ortalama) yığın azure'da bir Linux VM oluşturma

Bu öğretici azure'da bir Linux VM üzerinde MongoDB, Express, AngularJS ve Node.js (ortalama) yığın uygulamak nasıl gösterir. Oluşturduğunuz ortalama yığın ekleme, silme ve bir veritabanında books listeleme sağlar. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Linux VM oluşturma
> * Node.js yükleme
> * MongoDB yükleme ve sunucu ayarlama
> * Hızlı yükleme ve sunucu yolları ayarlama
> * AngularJS yollar erişim
> * Uygulamayı çalıştırma

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Yüklemek ve CLI yerel olarak kullanmak seçerseniz, Bu öğretici, Azure CLI Sürüm 2.0.4 çalıştırmasını gerektirir veya sonraki bir sürümü. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli).


## <a name="create-a-linux-vm"></a>Linux VM oluşturma

Sahip bir kaynak grubu oluşturma [az grubu oluşturma](https://docs.microsoft.com/cli/azure/group#az_group_create) komut ve bir Linux VM oluşturma [az vm oluşturma](https://docs.microsoft.com/cli/azure/vm#az_vm_create) komutu. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

Aşağıdaki örnek, bir kaynak grubu oluşturmak için Azure CLI kullanır *myResourceGroupMEAN* içinde *eastus* konumu. Bir VM adlandırılmış oluşturulur *myVM* zaten bir varsayılan anahtar konumda yoksa, SSH anahtarları. Anahtarlarını belirli bir kümesini kullanmak için ssh-anahtar-değer seçeneği.

```azurecli-interactive
az group create --name myResourceGroupMEAN --location eastus
az vm create \
    --resource-group myResourceGroupMEAN \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --admin-password 'Azure12345678!' \
    --generate-ssh-keys
az vm open-port --port 3300 --resource-group myResourceGroupMEAN --name myVM
```

Azure CLI bilgileri aşağıdaki örneğe benzer şekilde VM oluşturduğunuz sırada gösterir: 

```azurecli-interactive
{
  "fqdns": "",
  "id": "/subscriptions/{subscription-id}/resourceGroups/myResourceGroupMEAN/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "13.72.77.9",
  "resourceGroup": "myResourceGroupMEAN"
}
```
`publicIpAddress` değerini not edin. Bu adres, VM’ye erişmek için kullanılır.

VM ile bir SSH oturumu oluşturmak için aşağıdaki komutu kullanın. Doğru ortak IP adresi kullandığınızdan emin olun. Örneğimizde yukarıda bizim IP adresi 13.72.77.9 oluştu.

```bash
ssh azureuser@13.72.77.9
```

## <a name="install-nodejs"></a>Node.js yükleme

[Node.js](https://nodejs.org/en/) Chrome'nın V8 JavaScript altyapısında oluşturulmuş bir JavaScript Çalışma Zamanı Modülü. Node.js, bu öğreticide, Express yollar ve AngularJS denetleyicisi ayarlamak için kullanılır.

SSH ile açılmış bash Kabuğu'nu kullanarak VM, Node.js yükleyin.

```bash
sudo apt-get install -y nodejs
```

## <a name="install-mongodb-and-set-up-the-server"></a>MongoDB yükleme ve sunucu ayarlama
[MongoDB](http://www.mongodb.com) esnek, JSON benzeri belgelerde verileri depolar. Bir veritabanı alanları belge başka bir belge değişebilir ve veri yapısı zaman içinde değiştirilebilir. Bizim örnek uygulama için rehberi adı, ISBN numarası, yazar ve sayfa sayısını içeren MongoDB defteri kayıtları ekliyoruz. 

1. VM, SSH ile açılmış bash Kabuğu'nu kullanarak MongoDB anahtarı ayarlayın.

    ```bash
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6
    echo "deb [ arch=amd64 ] http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list
    ```

2. Paket Yöneticisi anahtarı ile güncelleştirin.
  
    ```bash
    sudo apt-get update
    ```

3. MongoDB yükleyin.

    ```bash
    sudo apt-get install -y mongodb
    ```

4. Sunucuyu başlatır.

    ```bash
    sudo service mongodb start
    ```

5. Ayrıca yüklemek ihtiyacımız [gövde ayrıştırıcı](https://www.npmjs.com/package/body-parser-json) isteklerinde sunucuya geçirilen JSON işlem yardımcı olmak için paket.

    Npm Paket Yöneticisi'ni yükleyin.

    ```bash
    sudo apt-get install npm
    ```

    Gövde ayrıştırıcı paketini yükleyin.
    
    ```bash
    sudo npm install body-parser
    ```

6. Adlı bir klasör oluşturun *Books* ve dosya adında ekleyin *server.js* , web sunucusu için yapılandırmayı içerir.

    ```node.js
    var express = require('express');
    var bodyParser = require('body-parser');
    var app = express();
    app.use(express.static(__dirname + '/public'));
    app.use(bodyParser.json());
    require('./apps/routes')(app);
    app.set('port', 3300);
    app.listen(app.get('port'), function() {
        console.log('Server up: http://localhost:' + app.get('port'));
    });
    ```

## <a name="install-express-and-set-up-routes-to-the-server"></a>Hızlı yükleme ve sunucu yolları ayarlama

[Express](https://expressjs.com) web ve mobil uygulamaları için özellikler sağlayan bir minimal ve esnek Node.js web uygulama çerçevesidir. Express Bu öğreticide geçirmek için kullanılan kitap bilgileri bizim MongoDB veritabanı gelen ve giden. [Mongoose](http://mongoosejs.com) uygulama verilerinizi modellemek için düz İleri, şema tabanlı bir çözüm sağlar. Mongoose Bu öğreticide, veritabanı için bir kitap şema sağlamak için kullanılır.

1. Express ve Mongoose yükleyin.

    ```bash
    sudo npm install express mongoose
    ```

2. İçinde *Books* klasörünü adlı bir klasör oluşturun *uygulamaları* ve adlı bir dosya eklemek *routes.js* tanımlanan express yollar.

    ```node.js
    var Book = require('./models/book');
    module.exports = function(app) {
      app.get('/book', function(req, res) {
        Book.find({}, function(err, result) {
          if ( err ) throw err;
          res.json(result);
        });
      }); 
      app.post('/book', function(req, res) {
        var book = new Book( {
          name:req.body.name,
          isbn:req.body.isbn,
          author:req.body.author,
          pages:req.body.pages
        });
        book.save(function(err, result) {
          if ( err ) throw err;
          res.json( {
            message:"Successfully added book",
            book:result
          });
        });
      });
      app.delete("/book/:isbn", function(req, res) {
        Book.findOneAndRemove(req.query, function(err, result) {
          if ( err ) throw err;
          res.json( {
            message: "Successfully deleted the book",
            book: result
          });
        });
      });
      var path = require('path');
      app.get('*', function(req, res) {
        res.sendfile(path.join(__dirname + '/public', 'index.html'));
      });
    };
    ```

3. İçinde *uygulamaları* klasörünü adlı bir klasör oluşturun *modelleri* ve adlı bir dosya eklemek *book.js* tanımlanan kitap modeli yapılandırmasına sahip.  

    ```node.js
    var mongoose = require('mongoose');
    var dbHost = 'mongodb://localhost:27017/test';
    mongoose.connect(dbHost);
    mongoose.connection;
    mongoose.set('debug', true);
    var bookSchema = mongoose.Schema( {
      name: String,
      isbn: {type: String, index: true},
      author: String,
      pages: Number
    });
    var Book = mongoose.model('Book', bookSchema);
    module.exports = mongoose.model('Book', bookSchema); 
    ```

## <a name="access-the-routes-with-angularjs"></a>AngularJS yollar erişim

[AngularJS](https://angularjs.org) dinamik görünümleri, web uygulamaları oluşturmak için bir web çerçevesidir sağlar. Bu öğreticide, hızlı web sayfamızı bağlanmak ve kitap Veritabanımıza eylemleri gerçekleştirmek için AngularJS kullanın.

1. Dizin geri kadar değiştirin *Books* (`cd ../..`) ve ardından adlı bir klasör oluşturun *ortak* ve adlı bir dosya eklemek *script.js* denetleyici yapılandırması tanımlı.

    ```node.js
    var app = angular.module('myApp', []);
    app.controller('myCtrl', function($scope, $http) {
      $http( {
        method: 'GET',
        url: '/book'
      }).then(function successCallback(response) {
        $scope.books = response.data;
      }, function errorCallback(response) {
        console.log('Error: ' + response);
      });
      $scope.del_book = function(book) {
        $http( {
          method: 'DELETE',
          url: '/book/:isbn',
          params: {'isbn': book.isbn}
        }).then(function successCallback(response) {
          console.log(response);
        }, function errorCallback(response) {
          console.log('Error: ' + response);
        });
      };
      $scope.add_book = function() {
        var body = '{ "name": "' + $scope.Name + 
        '", "isbn": "' + $scope.Isbn +
        '", "author": "' + $scope.Author + 
        '", "pages": "' + $scope.Pages + '" }';
        $http({
          method: 'POST',
          url: '/book',
          data: body
        }).then(function successCallback(response) {
          console.log(response);
        }, function errorCallback(response) {
          console.log('Error: ' + response);
        });
      };
    });
    ```
    
2. İçinde *ortak* klasörünü adlı bir dosya oluşturun *index.html* tanımlanan web sayfası.

    ```html
    <!doctype html>
    <html ng-app="myApp" ng-controller="myCtrl">
      <head>
        <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.6.4/angular.min.js"></script>
        <script src="script.js"></script>
      </head>
      <body>
        <div>
          <table>
            <tr>
              <td>Name:</td> 
              <td><input type="text" ng-model="Name"></td>
            </tr>
            <tr>
              <td>Isbn:</td>
              <td><input type="text" ng-model="Isbn"></td>
            </tr>
            <tr>
              <td>Author:</td> 
              <td><input type="text" ng-model="Author"></td>
            </tr>
            <tr>
              <td>Pages:</td>
              <td><input type="number" ng-model="Pages"></td>
            </tr>
          </table>
          <button ng-click="add_book()">Add</button>
        </div>
        <hr>
        <div>
          <table>
            <tr>
              <th>Name</th>
              <th>Isbn</th>
              <th>Author</th>
              <th>Pages</th>
            </tr>
            <tr ng-repeat="book in books">
              <td><input type="button" value="Delete" data-ng-click="del_book(book)"></td>
              <td>{{book.name}}</td>
              <td>{{book.isbn}}</td>
              <td>{{book.author}}</td>
              <td>{{book.pages}}</td>
            </tr>
          </table>
        </div>
      </body>
    </html>
    ```

##  <a name="run-the-application"></a>Uygulamayı çalıştırma

1. Dizin geri kadar değiştirin *Books* (`cd ..`) ve şu komutu çalıştırarak sunucu başlatın:

    ```bash
    nodejs server.js
    ```

2. VM için kaydedilen adresine bir web tarayıcısı açın. Örneğin, *http://13.72.77.9:3300*. Aşağıdaki sayfayı gibi bir şey görmeniz gerekir:

    ![Kayıt defteri](media/tutorial-mean/meanstack-init.png)

3. Veri tıklatın ve metin kutuları girin **Ekle**. Örneğin:

    ![Kitap kaydı ekleme](media/tutorial-mean/meanstack-add.png)

4. Sayfa yenilendikten sonra bu sayfa şöyle görmeniz gerekir:

    ![Liste defteri kayıtları](media/tutorial-mean/meanstack-list.png)

5. Tıklattığınız **silmek** ve defter kaydı veritabanından kaldırın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir ortalama kullanarak kitap kaydı ve bir web uygulaması oluşturan bir Linux VM yığında. Şunları öğrendiniz:

> [!div class="checklist"]
> * Linux VM oluşturma
> * Node.js yükleme
> * MongoDB yükleme ve sunucu ayarlama
> * Hızlı yükleme ve sunucu yolları ayarlama
> * AngularJS yollar erişim
> * Uygulamayı çalıştırma

SSL sertifikaları web sunucularıyla güvenli öğrenmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [SSL ile güvenli web sunucusu](tutorial-secure-web-server.md)
