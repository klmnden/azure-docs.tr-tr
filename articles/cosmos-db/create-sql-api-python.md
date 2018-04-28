---
title: "Azure Cosmos DB: Python ve SQL API'si ile bir uygulama derleme | Microsoft Docs"
description: Azure Cosmos DB SQL API'sine bağlanmak ve sorgu göndermek için kullanabileceğiniz bir Python kodu örneği sunar
services: cosmos-db
documentationcenter: ''
author: SnehaGunda
manager: kfile
ms.assetid: 51c11be2-af6d-425f-a86a-39cbfe61da29
ms.service: cosmos-db
ms.custom: quick start connect, mvc, devcenter
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: quickstart
ms.date: 04/13/2018
ms.author: sngun
ms.openlocfilehash: 2e439b260ae2964aeab33c100db3f62e0bd06f33
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="azure-cosmos-db-build-a-sql-api-app-with-python-and-the-azure-portal"></a>Azure Cosmos DB: Python ve Azure portalı ile bir SQL API'si uygulaması derleme

Azure Cosmos DB, Microsoft'un genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Bu hizmetle belge, anahtar/değer ve grafik veritabanlarını kolayca oluşturup sorgulayabilir ve tüm bunları yaparken Azure Cosmos DB'nin genel dağıtım ve yatay ölçeklendirme özelliklerinden faydalanabilirsiniz. 

Bu hızlı başlangıç belgesinde Azure portalını kullanarak bir Azure Cosmos DB [SQL API](sql-api-introduction.md) hesabını, belge veritabanını ve koleksiyonunu nasıl oluşturacağınız anlatılmıştır. Bu adımların ardından [SQL Python API'si](sql-api-sdk-python.md) kullanarak bir konsol uygulaması derleyebilir ve çalıştırabilirsiniz.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)] [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

## <a name="prerequisites"></a>Ön koşullar

* \<install location\>\Python36 ve \<install location>\Python36\Scripts ile [Python 3.6](https://www.python.org/downloads/) YOLUNUZA eklendi. 
* [Visual Studio Code](https://code.visualstudio.com/)
* [Visual Studio Code için Python uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-python.python#overview)

## <a name="create-a-database-account"></a>Veritabanı hesabı oluşturma

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-collection"></a>Koleksiyon ekleme

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

## <a name="add-sample-data"></a>Örnek verileri ekleme

[!INCLUDE [cosmos-db-create-sql-api-add-sample-data](../../includes/cosmos-db-create-sql-api-add-sample-data.md)]

## <a name="query-your-data"></a>Verilerinizi sorgulayın

[!INCLUDE [cosmos-db-create-sql-api-query-data](../../includes/cosmos-db-create-sql-api-query-data.md)]

## <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Şimdi GitHub'dan bir SQL API'si uygulaması kopyalayalım, bağlantı dizesini ayarlayalım ve uygulamayı çalıştıralım. Verilerle programlı bir şekilde çalışmanın ne kadar kolay olduğunu görüyorsunuz. 

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
    git clone https://github.com/Azure-Samples/azure-cosmos-db-documentdb-python-getting-started.git
    ```  
    
## <a name="review-the-code"></a>Kodu gözden geçirin

Bu adım isteğe bağlıdır. Veritabanı kaynaklarının kodda nasıl oluşturulduğunu öğrenmekle ilgileniyorsanız aşağıdaki kod parçacıklarını gözden geçirebilirsiniz. Aksi durumda, [Bağlantı dizenizi güncelleştirme](#update-your-connection-string) bölümüne atlayabilirsiniz. 

Aşağıdaki kod parçacıklarının tamamı, DocumentDBGetStarted.py dosyasından alınır.

* DocumentClient başlatılır.

    ```python
    # Initialize the Python client
    client = document_client.DocumentClient(config['ENDPOINT'], {'masterKey': config['MASTERKEY']})
    ```

* Yeni bir veritabanı oluşturulur.

    ```python
    # Create a database
    db = client.CreateDatabase({ 'id': config['SQL_DATABASE'] })
    ```

* Yeni bir koleksiyon oluşturulur.

    ```python
    # Create collection options
    options = {
        'offerEnableRUPerMinuteThroughput': True,
        'offerVersion': "V2",
        'offerThroughput': 400
    }

    # Create a collection
    collection = client.CreateCollection(db['_self'], { 'id': config['SQL_COLLECTION'] }, options)
    ```

* Birkaç belge oluşturulur.

    ```python
    # Create some documents
    document1 = client.CreateDocument(collection['_self'],
        { 
            'id': 'server1',
            'Web Site': 0,
            'Cloud Service': 0,
            'Virtual Machine': 0,
            'name': 'some' 
        })
    ```

* SQL kullanılarak bir sorgu gerçekleştirilir

    ```python
    # Query them in SQL
    query = { 'query': 'SELECT * FROM server s' }    
            
    options = {} 
    options['enableCrossPartitionQuery'] = True
    options['maxItemCount'] = 2

    result_iterable = client.QueryDocuments(collection['_self'], query, options)
    results = list(result_iterable);

    print(results)
    ```

## <a name="update-your-connection-string"></a>Bağlantı dizenizi güncelleştirme

Bu adımda Azure portalına dönerek bağlantı dizesi bilgilerinizi kopyalayıp uygulamaya ekleyin.

1. [Azure portalında](http://portal.azure.com/), Azure Cosmos DB hesabınızın sol gezinti menüsünden **Anahtarlar**'a tıklayın. Ekranın sağ tarafındaki kopyalama düğmelerini kullanarak **URI** ve **Birincil Anahtar** değerlerini kopyalayarak sonraki adımda DocumentDBGetStarted.py dosyasına yapıştırın.

    ![Azure portalında erişim anahtarı görüntüleme ve kopyalama, Anahtarlar dikey penceresi](./media/create-sql-api-dotnet/keys.png)

2. Visual Studio Code’da C:\git-samples\azure-cosmos-db-documentdb-python-getting-startedDocumentDBGetStarted.py dosyasını açın. 

3. Portaldaki **URI** değerinizi kopyalayın (kopyalama düğmesini kullanarak) ve DocumentDBGetStarted.py dosyasına **uç nokta** anahtarının değeri olarak yapıştırın. 

    `'ENDPOINT': 'https://FILLME.documents.azure.com',`

4. Ardından portaldaki **BİRİNCİL ANAHTAR** değerinizi kopyalayıp DocumentDBGetStarted.py içinde **config.MASTERKEY** öğesinin değeri yapın. Bu adımlarla uygulamanıza Azure Cosmos DB ile iletişim kurması için gereken tüm bilgileri eklemiş oldunuz. 

    `'MASTERKEY': 'FILLME',`

5. DocumentDBGetStarted.py dosyasını kaydedin.
    
## <a name="run-the-app"></a>Uygulamayı çalıştırma

1. Visual Studio Code’da, **Görünüm**>**Komut Paleti**’ni seçin. 

2. Komut isteminde, **Python: Yorumlayıcı Seçin** girip kullanılacak Python sürümün seçin.

    Visual Studio Code’daki alt bilgi, seçilen yorumlayıcıyı belirtmek için güncelleştirilir. 

3. Visual Studio SCode tümleşik terminalini açmak için **Görünüm** > **Tümleşik Terminal**'i seçin.

4. Tümleşik terminal penceresinde, azure-cosmos-db-documentdb-python-getting-started klasöründe olduğunuzdan emin olun. Değilseniz, örnek klasörü değiştirmek için aşağıdaki komutu çalıştırın. 

    ```
    cd "C:\git-samples\azure-cosmos-db-documentdb-python-getting-started"`
    ```

5. pydocumentdb paketini yüklemek için şu komutu çalıştırın. 

    ```
    pip3 install pydocumentdb
    ```

    Pydocumentdb yüklemeye çalışırken erişim engellendi hatası alıyorsanız, [VS Code’u yönetici olarak çalıştırmanız gerekir](https://stackoverflow.com/questions/37700536/visual-studio-code-terminal-how-to-run-a-command-with-administrator-rights).

6. Örneği çalıştırmak ve Azure Cosmos dB’de yeni belgeler oluşturup depolamak için aşağıdaki komutu çalıştırın.

    ```
    python DocumentDBGetStarted.py
    ```

7. Yeni belgelerin oluşturulduğunu ve kaydedildiğini onaylamak için, Azure portalında, **Veri Gezgini**’ni seçin, **coll** seçeneğini genişletin, **Belgeler** seçeneğini genişletin ve ardından **server1** belgesini seçin. Server1 belgesinin içerikleri tümleşik terminal pencerede döndürülen içerikle eşleşir. 

    ![Yeni belgeleri Azure portalında görüntüleme](./media/create-sql-api-python/azure-cosmos-db-confirm-documents.png)

## <a name="review-slas-in-the-azure-portal"></a>Azure portalında SLA'ları gözden geçirme

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta Azure Cosmos DB hesabı oluşturmayı, Veri Gezgini'ni kullanarak koleksiyon oluşturmayı ve bir uygulamayı çalıştırmayı öğrendiniz. Şimdi Cosmos DB hesabınıza ek veri aktarabilirsiniz. 

> [!div class="nextstepaction"]
> [SQL API'si için Azure Cosmos DB'ye veri aktarma](import-data.md)


