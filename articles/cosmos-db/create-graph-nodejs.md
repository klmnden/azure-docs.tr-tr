---
title: "Grafik API&quot;sini kullanarak Azure Cosmos DB Node.js uygulaması oluşturma | Microsoft Docs"
description: "Azure Cosmos DB&quot;ye bağlanmak ve veritabanını sorgulamak için kullanabileceğiniz bir Node.js kod örneği sunar"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: daacbabf-1bb5-497f-92db-079910703046
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 05/21/2017
ms.author: arramac
ms.translationtype: Human Translation
ms.sourcegitcommit: 80be19618bd02895d953f80e5236d1a69d0811af
ms.openlocfilehash: b9e8c46ba2f029f8dae2b357f05a806d769d0920
ms.contentlocale: tr-tr
ms.lasthandoff: 06/07/2017


---
<a id="azure-cosmos-db-build-a-nodejs-application-by-using-graph-api" class="xliff"></a>

# Azure Cosmos DB: Grafik API'sini kullanarak bir Node.js uygulaması oluşturma

Azure Cosmos DB, Microsoft'un genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Bu hizmetle belge, anahtar/değer ve grafik veritabanlarını kolayca oluşturup sorgulayabilir ve tüm bunları yaparken Azure Cosmos DB'nin genel dağıtım ve yatay ölçeklendirme özelliklerinden faydalanabilirsiniz. 

Bu hızlı başlangıç makalesinde, Azure portalı kullanılarak Grafik API'si (önizleme), veritabanı ve grafik için Azure Cosmos DB hesabının nasıl oluşturulacağı gösterilmiştir. Bu adımların ardından açık kaynaklı [Gremlin Node.js](https://www.npmjs.com/package/gremlin-secure) sürücüsünü kullanarak bir konsol uygulaması oluşturabilir ve çalıştırabilirsiniz.  

> [!NOTE]
> `gremlin-secure` npm modülü, `gremlin` modülünün Azure Cosmos DB ile bağlantı kurmak için gereken SSL ve SASL desteğine sahip değiştirilmiş bir sürümüdür. Kaynak kodu [GitHub](https://github.com/CosmosDB/gremlin-javascript)’dan edinilebilir.
>

<a id="prerequisites" class="xliff"></a>

## Ön koşullar

Bu örneği çalıştırmadan önce aşağıdaki önkoşullara sahip olmanız gerekir:
* [Node.js](https://nodejs.org/en/) v0.10.29 sürümü veya sonraki bir sürüm
* [Git](http://git-scm.com/)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

<a id="create-a-database-account" class="xliff"></a>

## Veritabanı hesabı oluşturma

[!INCLUDE [cosmos-db-create-dbaccount-graph](../../includes/cosmos-db-create-dbaccount-graph.md)]

<a id="add-a-graph" class="xliff"></a>

## Grafik ekleme

[!INCLUDE [cosmos-db-create-graph](../../includes/cosmos-db-create-graph.md)]

<a id="clone-the-sample-application" class="xliff"></a>

## Örnek uygulamayı kopyalama

Şimdi GitHub'dan bir Grafik API'si uygulaması kopyalayalım, bağlantı dizesini ayarlayalım ve uygulamayı çalıştıralım. Verilerle programlı bir şekilde çalışmanın ne kadar kolay olduğunu göreceksiniz. 

1. Git Bash gibi bir Git terminal penceresi açın ve bir çalışma diziniyle değiştirin (`cd` komutuyla).  

2. Örnek depoyu kopyalamak için aşağıdaki komutu çalıştırın. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-graph-nodejs-getting-started.git
    ```

3. Çözüm dosyasını Visual Studio'da açın. 

<a id="review-the-code" class="xliff"></a>

## Kodu gözden geçirin

Uygulamada gerçekleşen işlemleri hızlıca gözden geçirelim. `app.js` dosyasını açtığınızda aşağıdaki kod satırlarıyla karşılaşacaksınız. 

* Gremlin istemcisi oluşturulur.

    ```nodejs
    const client = Gremlin.createClient(
        443, 
        config.endpoint, 
        { 
            "session": false, 
            "ssl": true, 
            "user": `/dbs/${config.database}/colls/${config.collection}`,
            "password": config.primaryKey
        });
    ```

  Tüm yapılandırmalar, aşağıdaki bölümde düzenlediğimiz `config.js` öğesinde yer alır.

* `client.execute` yöntemiyle bir dizi Gremlin adımı yürütülür.

    ```nodejs
    console.log('Running Count'); 
    client.execute("g.V().count()", { }, (err, results) => {
        if (err) return console.error(err);
        console.log(JSON.stringify(results));
        console.log();
    });
    ```

<a id="update-your-connection-string" class="xliff"></a>

## Bağlantı dizenizi güncelleştirme

Bu adımda, Azure portalına dönerek bağlantı dizesi bilgilerinizi alıp uygulamaya kopyalayın.

1. [Azure portalı](http://portal.azure.com/)’nda, Azure Cosmos DB hesabınızın sol taraftaki gezinti menüsünde **Anahtarlar**'a ve ardından **Okuma/Yazma Anahtarları**'na tıklayın. Sağ taraftaki kopyalama düğmelerini kullanarak URI ve birincil anahtarı, sonraki adımdaki `app.js` dosyasına kopyalarsınız.

    ![Azure portalı Anahtarlar dikey penceresi](./media/create-graph-nodejs/keys.png)

2. Portaldaki Gremlin URI değerinizi kopyalayın (kopyalama düğmesini kullanarak) ve config.js dosyasında `config.endpoint` anahtarının değeri yapın. Gremlin uç noktası, `mygraphdb.graphs.azure.com` (`https://mygraphdb.graphs.azure.com` veya `mygraphdb.graphs.azure.com:433` değil) gibi protokol/bağlantı noktası numarası olmayan tek ana bilgisayar adı olmalıdır.

    `config.endpoint = "GRAPHENDPOINT";`

3. Portaldaki birincil anahtar değerinizi kopyalayın ve config.js dosyasında config.primaryKey’in değeri yapın. Bu adımlarla uygulamanıza Azure Cosmos DB ile iletişim kurması için gereken tüm bilgileri eklemiş oldunuz. 

    `config.primaryKey = "PRIMARYKEY";`

4. Veritabanı adını ve config.database ve config.collection değerinin grafik (kapsayıcı) adını girin. 

Aşağıda, tamamlanan config.js dosyanızın nasıl görüneceğine ilişkin bir örnek bulabilirsiniz:

```nodejs
var config = {}

// Note that this must not have HTTPS or the port number
config.endpoint = "mygraphdb.graphs.azure.com";
config.primaryKey = "OjlhK6tjxfSXyKtrmCiM9O6gQQgu5DmgAoauzD1PdPIq1LZJmILTarHvrolyUYOB0whGQ4j21rdAFwoYep7Kkw==";
config.database = "graphdb"
config.collection = "Persons"

module.exports = config;
```

<a id="run-the-console-app" class="xliff"></a>

## Konsol uygulamasını çalıştırma

1. Terminal penceresi açın ve projeye dahil olan package.json dosyası için yükleme diziniyle değiştirin (`cd` komutuyla).  

2. `gremlin-secure` dahil gerekli npm modüllerini yüklemek için `npm install` öğesini çalıştırın.

3. Node.js uygulamanızı başlatmak için bir terminalde `node app.js` komutunu çalıştırın.

<a id="browse-with-data-explorer" class="xliff"></a>

## Veri Gezgini ile Göz Ama

Artık Azure portalındaki Veri Gezgini'ne dönerek yeni grafik verilerinizi görüntüleyebilir, sorgulayabilir, değiştirebilir ve bu verilerle çalışabilirsiniz.

Yeni veritabanı, Veri Gezgini'nin **Koleksiyonlar** bölmesinde görünür. **graphdb**, **graphcoll** öğelerini genişletip **Grafik** öğesine tıklayın.

Örnek uygulama tarafından oluşturulan veriler **Grafikler** bölmesinde görüntülenir.

<a id="review-slas-in-the-azure-portal" class="xliff"></a>

## Azure portalında SLA'ları gözden geçirme

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

<a id="clean-up-your-resources" class="xliff"></a>

## Kaynaklarınızı temizleme

Bu uygulamayı kullanmaya devam etmeyi düşünmüyorsanız aşağıdakileri yaparak bu makalede oluşturduğunuz tüm kaynakları silin: 

1. Azure portalında sol taraftaki menüden **Kaynak grupları**'na ve ardından oluşturduğunuz kaynağın adına tıklayın. 
2. Kaynak grubu sayfanızda **Sil**'e tıklayın, silinecek kaynağın adını yazın ve ardından **Sil**'e tıklayın.

<a id="next-steps" class="xliff"></a>

## Sonraki adımlar

Bu makalede, Azure Cosmos DB hesabı oluşturmayı, Veri Gezgini'ni kullanarak grafik oluşturmayı ve bir uygulamayı çalıştırmayı öğrendiniz. Artık daha karmaşık sorgular oluşturabilir ve Gremlin kullanarak güçlü grafik geçişi mantığını kullanabilirsiniz. 

> [!div class="nextstepaction"]
> [Gremlin kullanarak sorgulama](tutorial-query-graph.md)

