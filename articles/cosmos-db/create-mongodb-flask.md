---
title: MongoDB API'si için Python ve Azure Cosmos DB ile bir Flask web uygulaması oluşturma
description: Bağlanmak ve MongoDB API'si için Azure Cosmos DB buradan veri sorgulamak için kullanabileceğiniz bir Python Flask kodu örneği sunar.
services: cosmos-db
author: slyons
ms.author: sclyon
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.custom: quick start connect, mvc, seodec18
ms.devlang: python
ms.topic: quickstart
ms.date: 10/02/2017
ms.openlocfilehash: 4bd62507ae3cae25d4bca10a906a305550d0b61a
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2018
ms.locfileid: "53715840"
---
# <a name="build-a-flask-app-with-the-azure-cosmos-db-for-mongodb-api"></a>MongoDB API'si için Azure Cosmos DB ile bir Flask uygulaması derleme

> [!div class="op_single_selector"]
> * [.NET](create-mongodb-dotnet.md)
> * [Java](create-mongodb-java.md)
> * [Node.js](create-mongodb-nodejs.md)
> * [Python](create-mongodb-flask.md)
> * [Xamarin](create-mongodb-xamarin.md)
> * [Golang](create-mongodb-golang.md)
>  

Azure Cosmos DB, Microsoft'un genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Bu hizmetle belge, anahtar/değer ve grafik veritabanlarını kolayca oluşturup sorgulayabilir ve tüm bunları yaparken Azure Cosmos DB'nin genel dağıtım ve yatay ölçeklendirme özelliklerinden faydalanabilirsiniz.

Bu Hızlı Başlangıç Kılavuzu aşağıdaki kullanan [Flask örnek](https://github.com/Azure-Samples/CosmosDB-Flask-Mongo-Sample) ve basit bir To-Do Flask uygulaması derlemeyi gösterir [Azure Cosmos DB öykünücüsü'nü](local-emulator.md) ve Azure Cosmos DB MongoDB API'si için.

## <a name="prerequisites"></a>Önkoşullar

- [Azure Cosmos DB Emulator](local-emulator.md)’ı indirin. Öykünücü şu anda yalnızca Windows’da desteklenmektedir. Örnek, herhangi bir platformda yapılabileceği gibi, Azure’dan bir üretim anahtarı ile örneği kullanma işlemini gösterir.

- Visual Studio Code henüz yüklü değilse, platformunuza yönelik [VS Code](https://code.visualstudio.com/Download)’u (Windows, Mac, Linux) hızlıca yükleyebilirsiniz.

- Popüler Python uzantılardan birini yükleyerek Python Dil Desteğini eklediğinizden emin olun.
    1. Bir uzantı seçin.
    2. `Ctrl+Shift+P` Komut Paletine `ext install` yazarak uzantıyı yükleyin.

    Bu belgedeki örnekler Don Jayamanne’ın popüler ve tam özellikli [Python Uzantısını](https://marketplace.visualstudio.com/items?itemName=donjayamanne.python) kullanır.

## <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Şimdi github'dan bir Flask-MongoDB uygulaması kopyalayalım, bağlantı dizesini ayarlayalım ve uygulamayı şimdi. Verilerle programlı bir şekilde çalışmanın ne kadar kolay olduğunu görüyorsunuz.

1. Bir komut istemini açın, git-samples adlı yeni bir klasör oluşturun ve komut istemini kapatın.

    ```bash
    md "C:\git-samples"
    ```

2. Git Bash gibi bir Git terminal penceresi açın ve örnek uygulamayı yüklemek üzere yeni bir klasör olarak değiştirmek için `cd` komutunu kullanın.

    ```bash
    cd "C:\git-samples"
    ```

3. Örnek depoyu kopyalamak için aşağıdaki komutu çalıştırın. Bu komut bilgisayarınızda örnek uygulamanın bir kopyasını oluşturur.

    ```bash
    git clone https://github.com/Azure-Samples/CosmosDB-Flask-Mongo-Sample.git
    ```
3. Python modüllerini yüklemek için aşağıdaki komutu çalıştırın.

    ```bash 
    pip install -r .\requirements.txt
    ```
4. Visual Studio Code’da klasörü açın.

## <a name="review-the-code"></a>Kodu gözden geçirin

Bu adım isteğe bağlıdır. Veritabanı kaynaklarının kodda nasıl oluşturulduğunu öğrenmekle ilgileniyorsanız aşağıdaki kod parçacıklarını gözden geçirebilirsiniz. Aksi takdirde, [Web uygulamasını çalıştırma](#run-the-web-app) konusuna atlayabilirsiniz. 

Aşağıdaki kod parçacıklarının tümü app.py dosyasından alınmıştır ve yerel Azure Cosmos DB Öykünücüsü’nün bağlantı dizesini kullanır. Başka türlü ayrıştırılamayan ileri eğik çizgiler için aşağıda görüldüğü gibi parolanın bölünmesi gerekir.

* MongoDB istemcisini başlatın, veritabanını alın ve kimlik doğrulaması yapın.

    ```python
    client = MongoClient("mongodb://127.0.0.1:10250/?ssl=true") #host uri
    db = client.test    #Select the database
    db.authenticate(name="localhost",password='C2y6yDjf5' + r'/R' + '+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw' + r'/Jw==')
    ```

* Koleksiyonu alın veya henüz yoksa bir tane oluşturun.

    ```python
    todos = db.todo #Select the collection
    ```

* Uygulama oluşturma

    ```Python
    app = Flask(__name__)
    title = "TODO with Flask"
    heading = "ToDo Reminder"
    ```
    
## <a name="run-the-web-app"></a>Web uygulamasını çalıştırma

1. Azure Cosmos DB Emulator’ın çalışır durumda olduğundan emin olun.

2. Bir terminal penceresi açın ve uygulamanın kayıtlı olduğu dizine `cd` uygulayın.

3. Ardından, bir Mac bilgisayar kullanıyorsanız Flask uygulaması için ortam değişkenini `set FLASK_APP=app.py` veya `export FLASK_APP=app.py` ile ayarlayın.

4. Uygulamayı `flask run` ile çalıştırın ve [http://127.0.0.1:5000/](http://127.0.0.1:5000/) adresine göz atın.

5. Görevleri ekleyip kaldırın ve koleksiyona eklenip kaldırıldığına dikkat edin.

## <a name="create-a-database-account"></a>Veritabanı hesabı oluşturma

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="update-your-connection-string"></a>Bağlantı dizenizi güncelleştirme

Kodu canlı bir Azure Cosmos DB Hesabına karşı test etmek isterseniz, Azure portalına giderek bir hesap oluşturun ve bağlantı dizesi bilgilerinizi alın. Ardından uygulamaya kopyalayın.

1. [Azure portalında](https://portal.azure.com/), Azure Cosmos DB hesabınızın sol taraftaki gezinti menüsünden **Bağlantı Dizesi**'ne ve ardından **Okuma-Yazma Anahtarları**'na tıklayın. Sonraki adımda ekranın sağ tarafındaki kopyalama düğmelerini kullanarak Kullanıcı Adı, Parola ve Ana Bilgisayar değerlerini Dal.cs dosyasına kopyalayacaksınız.

2. Kök dizinde **app.py** dosyasını açın.

3. **username** değerini portaldan kopyalayın (kopyalama düğmesini kullanarak) ve **app.py** dosyasına **name** değeri olarak yapıştırın.

4. Daha sonra portaldan **connection string** değerini kopyalayın **app.py** dosyanızda MongoClient değeri olarak yapıştırın.

5. Son olarak, **password** değerini portaldan kopyalayın ve **app.py** dosyanıza **password** değeri olarak yapıştırın.

Bu adımlarla uygulamanıza Azure Cosmos DB ile iletişim kurması için gereken tüm bilgileri eklemiş oldunuz. Öncekiyle aynı şekilde çalıştırabilirsiniz.

## <a name="deploy-to-azure"></a>Azure’a dağıtma

Bu uygulamayı dağıtmak için Azure'da yeni bir web uygulaması oluşturma ve bu GitHub deposunun çatalı ile sürekli dağıtımı etkinleştirme. İzleyin [öğretici](https://docs.microsoft.com/azure/app-service/deploy-continuous-deployment) azure'da GitHub ile sürekli dağıtımı ayarlamak için.

Azure'a dağıtırken uygulama anahtarlarınızı kaldırmanız ve aşağıdaki bölümün açıklama satırı yapılmadığından emin olmanız gerekir:

```python
    client = MongoClient(os.getenv("MONGOURL"))
    db = client.test    #Select the database
    db.authenticate(name=os.getenv("MONGO_USERNAME"),password=os.getenv("MONGO_PASSWORD"))
```

Daha sonra MONGOURL, MONGO_PASSWORD ve MONGO_USERNAME değerlerinizi uygulama ayarlarına eklemeniz gerekir. Azure Web Apps’te Uygulama Ayarları hakkında daha fazla bilgi almak için bu [öğreticiyi](https://docs.microsoft.com/azure/app-service-web/web-sites-configure#application-settings) izleyebilirsiniz.

Bu deponun bir çatalını oluşturmak istemiyorsanız, aşağıdaki Azure’a dağıt düğmesine de tıklayabilirsiniz. Daha sonra Azure’a gidip Cosmos DB hesabı bilgilerinizle uygulama ayarlarını yapmanız gerekir.

<a href="https://deploy.azure.com/?repository=https://github.com/heatherbshapiro/To-Do-List---Flask-MongoDB-Example" target="_blank">
<img src="https://azuredeploy.net/deploybutton.png" alt="Click to Deploy to Azure">
</a>

> [!NOTE]
> Kodunuzu GitHub veya diğer kaynak denetimi seçeneklerinde depolamayı planlıyorsanız, Lütfen bağlantı dizelerinizi koddan kaldırdığınızdan emin olun. Bağlantı dizeleriniz, bunun yerine web uygulamasının uygulama ayarlarıyla ayarlanabilir.

## <a name="review-slas-in-the-azure-portal"></a>Azure portalında SLA'ları gözden geçirme

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir Azure Cosmos DB hesabı oluşturmayı ve MongoDB API'si için istemci kullanarak bir Flask uygulamasını çalıştırmayı öğrendiniz. Şimdi Cosmos DB hesabınıza ek veri aktarabilirsiniz.

> [!div class="nextstepaction"]
> [MongoDB API'si veritabanı için Azure Cosmos DB'ye veri aktarma](mongodb-migrate.md)
