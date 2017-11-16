---
title: "Hızlı Başlangıç: Cassandra API'si Node.js - Azure Cosmos DB ile | Microsoft Docs"
description: "Bu hızlı başlangıç Azure Cosmos DB Cassandra API Node.js ile bir profil uygulaması oluşturmak için nasıl kullanılacağını gösterir"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 4732e57d-32ed-40e2-b148-a8df4ff2630d
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: quickstart
ms.date: 11/15/2017
ms.author: govindk
ms.openlocfilehash: 76850d6403fc4e87e95f5842b87b258d652c2c35
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2017
---
# <a name="quickstart-build-a-cassandra-app-with-nodejs-and-azure-cosmos-db"></a>Hızlı Başlangıç: Node.js ve Azure Cosmos DB ile Cassandra uygulaması oluşturma

Bu hızlı başlangıç Node.js ve Azure Cosmos DB nasıl kullanılacağını gösterir [Cassandra API](cassandra-introduction.md) örneği github'dan kopyalanarak profili uygulamanızı oluşturmak için. Bu hızlı başlangıç Ayrıca, bir Azure Cosmos DB hesap oluşturulmasını web tabanlı Azure portalını kullanarak açıklanmaktadır.

Azure Cosmos DB Microsoft'un Genel dağıtılmış birden çok model veritabanı hizmetidir. Hızlı bir şekilde oluşturmak ve belge, tablo, anahtar-değer ve grafik veritabanları, her biri genel dağıtım ve yatay ölçek yetenekleri Azure Cosmos DB en yararlı sorgulayabilirsiniz. 

## <a name="prerequisites"></a>Ön koşullar

* Azure Cosmos DB Cassandra API Önizleme programına erişim. Erişim için henüz yapmadıysanız uyguladıysanız [şimdi kaydolun](https://aka.ms/cosmosdb-cassandra-signup).
* [Node.js](https://nodejs.org/en/) sürüm v0.10.29 veya üzeri
* [Git](http://git-scm.com/)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]Alternatif olarak, [Azure Cosmos DB ücretsiz deneyin](https://azure.microsoft.com/try/cosmosdb/) gider ve taahhüt bir Azure aboneliği boş.


## <a name="create-a-database-account"></a>Veritabanı hesabı oluşturma

Bir belge veritabanı oluşturmadan önce Azure Cosmos DB ile Cassandra hesabı oluşturmanız gerekir.

[!INCLUDE [cosmos-db-create-dbaccount-cassandra](../../includes/cosmos-db-create-dbaccount-cassandra.md)]

## <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Artık şimdi kopya Cassandra API uygulaması, github'dan bağlantı dizesini ayarlamak ve çalıştırın. Verilerle programlı bir şekilde çalışmanın ne kadar kolay olduğunu görüyorsunuz. 

1. Git bash gibi bir git terminal penceresi açın ve kullanmak `cd` örnek uygulamayı yüklemek için bir klasör olarak değiştirmek için komutu. 

    ```bash
    cd "C:\git-samples"
    ```

2. Örnek depoyu kopyalamak için aşağıdaki komutu çalıştırın. Bu komut bilgisayarınızda örnek uygulaması bir kopyasını oluşturur.

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-cassandra-nodejs-getting-started.git
    ```

## <a name="review-the-code"></a>Kodu gözden geçirin

Bu adım isteğe bağlıdır. Veritabanı kaynakları kodda nasıl oluşturulduğunu öğrenmek isterseniz, aşağıdaki kod parçacıkları gözden geçirebilirsiniz. Kod parçacıkları tüm gelen alınır `uprofile.js` C:\git-samples\azure-cosmos-db-cassandra-nodejs-getting-started klasördeki dosya. Aksi takdirde, atlayabilirsiniz [bağlantı dizenizi güncelleştirme](#update-your-connection-string). 

* Kullanıcı adı ve parola bağlantı dizesini sayfasında Azure portalı kullanılarak ayarlanır. 'path\to\cert' bir X509 bir yolunu sağlar sertifika. 

   ```nodejs
   var ssl_option = {
        cert : fs.readFileSync("path\to\cert"),
        rejectUnauthorized : true,
        secureProtocol: 'TLSv1_2_method'
        };
   const authProviderLocalCassandra = new cassandra.auth.PlainTextAuthProvider(config.username, config.password);
   ```

* `client` ContactPoint bilgilerle başlatılır. ContactPoint Azure portalından alınır.

    ```nodejs
    const client = new cassandra.Client({contactPoints: [config.contactPoint], authProvider: authProviderLocalCassandra, sslOptions:ssl_option});
    ```

* `client` Azure Cosmos DB Cassandra API'sine bağlanır.

    ```nodejs
    client.connect(next);
    ```

* Yeni bir keyspace oluşturulur.

    ```nodejs
    function createKeyspace(next) {
        var query = "CREATE KEYSPACE IF NOT EXISTS uprofile WITH replication = {\'class\': \'NetworkTopologyStrategy\', \'datacenter1\' : \'1\' }";
        client.execute(query, next);
        console.log("created keyspace");    
  }
    ```

* Yeni bir tablo oluşturulur.

   ```nodejs
   function createTable(next) {
    var query = "CREATE TABLE IF NOT EXISTS uprofile.user (user_id int PRIMARY KEY, user_name text, user_bcity text)";
        client.execute(query, next);
        console.log("created table");
   },
   ```

* Anahtar/değer varlıklar eklenir.

    ```nodejs
    ...
       {
          query: 'INSERT INTO  uprofile.user  (user_id, user_name , user_bcity) VALUES (?,?,?)',
          params: [5, 'IvanaV', 'Belgaum', '2017-10-3136']
        }
    ];
    client.batch(queries, { prepare: true}, next);
    ```

* Almak için sorgu tüm anahtar değerlerin alın.

    ```nodejs
   var query = 'SELECT * FROM uprofile.user';
    client.execute(query, { prepare: true}, function (err, result) {
      if (err) return next(err);
      result.rows.forEach(function(row) {
        console.log('Obtained row: %d | %s | %s ',row.user_id, row.user_name, row.user_bcity);
      }, this);
      next();
    });
    ```  
    
* Bir anahtar-değer almak için sorgu.

    ```nodejs
    function selectById(next) {
        console.log("\Getting by id");
        var query = 'SELECT * FROM uprofile.user where user_id=1';
        client.execute(query, { prepare: true}, function (err, result) {
        if (err) return next(err);
            result.rows.forEach(function(row) {
            console.log('Obtained row: %d | %s | %s ',row.user_id, row.user_name, row.user_bcity);
        }, this);
        next();
        });
    }
    ```  

## <a name="update-your-connection-string"></a>Bağlantı dizenizi güncelleştirme

Bu adımda Azure portalına dönerek bağlantı dizesi bilgilerinizi kopyalayıp uygulamaya ekleyin. Bu, barındırılan veritabanıyla iletişim kurmak uygulamanızı sağlar.

1. İçinde [Azure portal](http://portal.azure.com/), tıklatın **bağlantı dizesi**. 

    Kullanın ![Kopyala düğmesini](./media/create-cassandra-nodejs/copy.png) ekranın üst değer, ilgili kişi noktası kopyalamak için sağ taraftaki düğmesi.

    ![Görüntüleme ve kişi noktası, kullanıcı adı ve parola Azure portal, bağlantı dizesi sayfasından kopyalama](./media/create-cassandra-nodejs/keys.png)

2. `config.js` dosyasını açın. 

3. Üzerinden portalı kişi noktası değerinden Yapıştır `<FillMEIN>` satırında 4.

    Satır 4 benzer görünmelidir 

    `config.contactPoint = "cosmos-db-quickstarts.documents.azure.com:10350"`

4. USERNAME değerini portaldan kopyalayıp üzerinden yapıştırın `<FillMEIN>` satırında 2.

    2. satır benzer görünmelidir 

    `config.username = 'cosmos-db-quickstart';`
    
5. PAROLA değeri Portal'dan kopyalayın ve üzerinden yapıştırın `<FillMEIN>` satırında 3.

    Satırı 3 şimdi şuna benzemelidir

    `config.password = '2Ggkr662ifxz2Mg==';`

6. Config.js. dosyasına kaydedin.
    
## <a name="use-the-x509-certificate"></a>Kullanım X509 sertifika 

1. Baltimore CyberTrust Root eklemeniz gerekiyorsa, seri numarası 02:00:00:b9 ve SHA1 parmak izi d4🇩🇪20:d0:5e:66:fc:53:fe:1a:50:88:2 c onu vardır: 78:db:28:52:ca:e4:74. Yerel bir dosya uzantısı .cer ile kaydedilen https://cacert.omniroot.com/bc2025.crt'nden indirilebilir. 

2. Uprofile.js açın ve 'yeni sertifikanızı işaret edecek şekilde path\to\cert' değiştirin. 

3. Uprofile.js kaydedin. 

## <a name="run-the-app"></a>Uygulamayı çalıştırma

1. Git terminal penceresinden çalıştırmak `npm install` gerekli npm modüllerini yüklemek için.

2. Çalıştırma `node uprofile.js` düğümü uygulamanızı başlatmak için.

3. Komut satırından beklendiği gibi sonuçlarını doğrulayın.

    ![Görüntülemek ve çıktı doğrulayın](./media/create-cassandra-nodejs/output.png)

    Programın exection durdurun ve konsol penceresini kapatmak için CTRL + C tuşlarına basın. 

    Sorgu görmek, değiştirmek ve bu yeni verilerle çalışmak için Azure portalında Veri Gezgini artık açabilirsiniz. 

    ![Verileri veri Gezgini'nde görüntüleyin](./media/create-cassandra-nodejs/data-explorer.png) 

## <a name="review-slas-in-the-azure-portal"></a>Azure portalında SLA'ları gözden geçirme

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta Azure Cosmos DB hesabı oluşturmayı, Veri Gezgini'ni kullanarak koleksiyon oluşturmayı ve bir uygulamayı çalıştırmayı öğrendiniz. Şimdi Cosmos DB hesabınıza ek veri aktarabilirsiniz. 

> [!div class="nextstepaction"]
> [Azure Cosmos Veritabanına Cassandra veri alma](cassandra-import-data.md)


