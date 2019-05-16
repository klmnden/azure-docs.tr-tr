---
title: "Hızlı Başlangıç: Tablo API'si ile Java - Azure Cosmos DB"
description: Bu hızlı başlangıçta Azure portalı ve Java ile uygulama oluşturmak için Azure Cosmos DB Tablo API’sinin nasıl kullanılacağı gösterilmektedir
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.devlang: java
ms.topic: quickstart
ms.date: 04/10/2018
ms.author: sngun
ms.openlocfilehash: 22d5aea119d71553afc02ee923fab5cf5ead5ce0
ms.sourcegitcommit: 17411cbf03c3fa3602e624e641099196769d718b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2019
ms.locfileid: "65519042"
---
# <a name="quickstart-build-a-table-api-app-with-java-and-azure-cosmos-db"></a>Hızlı Başlangıç: API uygulamasını Java ve Azure Cosmos DB ile tablo oluşturma

> [!div class="op_single_selector"]
> * [.NET](create-table-dotnet.md)
> * [Java](create-table-java.md)
> * [Node.js](create-table-nodejs.md)
> * [Python](create-table-python.md)
> 

Bu hızlı başlangıçta GitHub’dan bir örneği kopyalayarak bir uygulama oluşturmak için Java ve Azure Cosmos DB [Tablo API’sini](table-introduction.md) nasıl kullanacağınız gösterilmektedir. Bu hızlı başlangıçta ayrıca Azure Cosmos DB hesabı oluşturma ve web tabanlı Azure portalında tablo ve varlıklar oluşturmak için Veri Gezgini’ni kullanma da gösterilmektedir.

Azure Cosmos DB, Microsoft'un genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Bu hizmetle belge, anahtar/değer ve grafik veritabanlarını kolayca oluşturup sorgulayabilir ve tüm bunları yaparken Azure Cosmos DB'nin genel dağıtım ve yatay ölçeklendirme özelliklerinden faydalanabilirsiniz. 

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

Buna ek olarak: 

* [Java Geliştirme Seti (JDK) 8](https://aka.ms/azure-jdks)
    * JAVA_HOME ortam değişkenini JDK’nin yüklü olduğu klasöre işaret edecek şekilde ayarladığınızdan emin olun.
* Bir [Maven](https://maven.apache.org/) ikili arşivi [indirin](https://maven.apache.org/download.cgi) ve [yükleyin](https://maven.apache.org/install.html)
    * Ubuntu’da Maven’i yüklemek için `apt-get install maven` komutunu çalıştırabilirsiniz.
* [Git](https://www.git-scm.com/)
    * Ubuntu’da Git’i yüklemek için `sudo apt-get install git` komutunu çalıştırabilirsiniz.

## <a name="create-a-database-account"></a>Veritabanı hesabı oluşturma

> [!IMPORTANT] 
> Genel olarak kullanılabilir Tablo API’si SDK’ları ile çalışmak için yeni bir Tablo API’si hesabı oluşturmanız gerekir. Önizleme sırasında oluşturulan Tablo API’si hesapları genel olarak kullanılabilir SDK’lar tarafından desteklenmez.
>

[!INCLUDE [cosmos-db-create-dbaccount-table](../../includes/cosmos-db-create-dbaccount-table.md)]

## <a name="add-a-table"></a>Tablo ekleme

[!INCLUDE [cosmos-db-create-table](../../includes/cosmos-db-create-table.md)]

## <a name="add-sample-data"></a>Örnek verileri ekleme

[!INCLUDE [cosmos-db-create-table-add-sample-data](../../includes/cosmos-db-create-table-add-sample-data.md)]

## <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Şimdi GitHub'dan bir Tablo uygulaması kopyalayalım, bağlantı dizesini ayarlayalım ve uygulamayı çalıştıralım. Verilerle program aracılığıyla çalışmanın ne kadar kolay olduğunu göreceksiniz. 

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
    git clone https://github.com/Azure-Samples/storage-table-java-getting-started.git 
    ```

## <a name="update-your-connection-string"></a>Bağlantı dizenizi güncelleştirme

Bu adımda Azure portalına dönerek bağlantı dizesi bilgilerinizi kopyalayıp uygulamaya ekleyin. Bu, uygulamanızın barındırılan veritabanıyla iletişim kurmasına olanak tanır. 

1. [Azure portalda](https://portal.azure.com/) **Bağlantı Dizesi**’ne tıklayın. 

   ![Bağlantı Dizesi bölmesinde gerekli bağlantı dizesi bilgilerini görüntüleme ve kopyalama](./media/create-table-java/connection-string.png)

2. Sağ taraftaki kopyala düğmesini kullanarak PRIMARY CONNECTION STRING’i kopyalayın.

3. C:\git-samples\storage-table-java-getting-started\src\main\resources klasöründen config.properties dosyasını açın. 

5. Birinci satırı açıklama satırı yapın ve ikinci satırın açıklamasını kaldırın. İlk iki satır şöyle görünmelidir.

    ```
    #StorageConnectionString = UseDevelopmentStorage=true
    StorageConnectionString = DefaultEndpointsProtocol=https;AccountName=[ACCOUNTNAME];AccountKey=[ACCOUNTKEY]
    ```

6. Portaldan PRIMARY CONNECTION STRING değerini 2. satırda StorageConnectionString değerine yapıştırın. 

    > [!IMPORTANT]
    > Uç noktanız documents.azure.com kullanıyorsa, bir önizleme hesabınız var demektir ve genel olarak kullanılabilir Tablo API’si SDK’ları ile çalışmak için [yeni bir Tablo API’si hesabı](#create-a-database-account) oluşturmanız gerekir.
    >

7. Config.properties dosyasını kaydedin.

Bu adımlarla uygulamanıza Azure Cosmos DB ile iletişim kurması için gereken tüm bilgileri eklemiş oldunuz. 

## <a name="run-the-app"></a>Uygulamayı çalıştırma

1. Git terminal penceresinde `cd` komutuyla storage-table-java-getting-started klasörüne ulaşın.

    ```git
    cd "C:\git-samples\storage-table-java-getting-started"
    ```

2. Git terminal penceresinde, Java uygulamasını başlatmak için aşağıdaki komutları çalıştırın.

    ```git
    mvn compile exec:java 
    ```

    Konsol penceresi, Azure Cosmos DB içinde yeni tablo veritabanına eklenen tablo verilerini görüntüler.

    Şimdi Veri Gezgini'ne dönüp bu yeni verileri görebilir, sorgulayabilir, değiştirebilir ve onlarla çalışabilirsiniz. 

## <a name="review-slas-in-the-azure-portal"></a>Azure portalında SLA'ları gözden geçirme

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta Azure Cosmos DB hesabı oluşturmayı, Veri Gezgini'ni kullanarak tablo oluşturmayı ve bir uygulamayı çalıştırmayı öğrendiniz.  Şimdi Tablo API'sini kullanarak verilerinizi sorgulayabilirsiniz.  

> [!div class="nextstepaction"]
> [Tablo verilerini Tablo API’sine aktarma](table-import.md)
