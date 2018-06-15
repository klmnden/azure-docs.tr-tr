---
title: 'Hızlı Başlangıç: Python ile Tablo API’si - Azure Cosmos DB | Microsoft Docs'
description: Bu hızlı başlangıçta Azure portalı ve Python ile uygulama oluşturmak için Azure Cosmos DB Tablo API’sinin nasıl kullanılacağı gösterilmektedir
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-table
ms.devlang: python
ms.topic: quickstart
ms.date: 04/10/2018
ms.author: sngun
ms.custom: mvc
ms.openlocfilehash: f23d7441a2abc6b66e95e226ef0d17d29ed25c9d
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34796007"
---
# <a name="quickstart-build-a-table-api-app-with-python-and-azure-cosmos-db"></a>Hızlı Başlangıç: Python ve Azure Cosmos DB ile Tablo API’si uygulaması oluşturma

Bu hızlı başlangıçta GitHub’dan bir örneği kopyalayarak bir uygulama oluşturmak için Python ve Azure Cosmos DB [Tablo API’sini](table-introduction.md) nasıl kullanacağınız gösterilmektedir. Bu hızlı başlangıçta ayrıca Azure Cosmos DB hesabı oluşturma ve web tabanlı Azure portalında tablo ve varlıklar oluşturmak için Veri Gezgini’ni kullanma da gösterilmektedir.

Azure Cosmos DB, Microsoft'un genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Bu hizmetle belge, anahtar/değer,geniş sütun ve grafik veritabanlarını kolayca oluşturup sorgulayabilir ve tüm bunları yaparken Azure Cosmos DB'nin genel dağıtım ve yatay ölçeklendirme özelliklerinden faydalanabilirsiniz. 

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

Buna ek olarak:

* Henüz Visual Studio 2017’yi yüklemediyseniz, **ücretsiz** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/)’ı indirip kullanabilirsiniz. Visual Studio kurulumu sırasında **Azure dağıtımını** etkinleştirdiğinizden emin olun.
* [GitHub](http://microsoft.github.io/PTVS/)'dan Visual Studio için Python Araçları. Bu öğreticide, VS 2015 için Python Araçları kullanır.
* [python.org](https://www.python.org/downloads/release/python-2712/) adresinden Python 2.7

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
    git clone https://github.com/Azure-Samples/storage-python-getting-started.git
    ```

3. Ardından çözüm dosyasını Visual Studio'da açın. 

## <a name="update-your-connection-string"></a>Bağlantı dizenizi güncelleştirme

Bu adımda Azure portalına dönerek bağlantı dizesi bilgilerinizi kopyalayıp uygulamaya ekleyin. Bu, uygulamanızın barındırılan veritabanıyla iletişim kurmasına olanak tanır. 

1. [Azure portalında](http://portal.azure.com/) **Bağlantı Dizesi**’ne tıklayın. 

    ![Bağlantı Dizesi bölmesindeki CONNECTION STRING’i kopyalama ve görüntüleme](./media/create-table-python/connection-string.png)

2. Sağ taraftaki düğmeyi kullanarak ACCOUNT NAME’i kopyalayın.

3. Config.py dosyasını açın ve portaldan ACCOUNT NAME değerini 19. satırdaki STORAGE_ACCOUNT_NAME değerine yapıştırın.

4. Portala geri dönüp PRIMARY KEY’i kopyalayın.

5. Portaldan PRIMARY KEY değerini 20. satırdaki STORAGE_ACCOUNT_KEY değerine yapıştırın.

3. Config.py dosyasını kaydedin.

## <a name="run-the-app"></a>Uygulamayı çalıştırma

1. Visual Studio'nun **Çözüm Gezgini** bölümünde projeye sağ tıklayın, geçerli Python ortamını seçin ve sağ tıklayın.

2. Python Paketini Yükle'yi seçip **azure-storage-table** yazın

3. Uygulamayı çalıştırmak için F5'e basın. Uygulamanız tarayıcınızda görüntülenir. 

Şimdi Veri Gezgini'ne dönüp bu yeni verileri görebilir, sorgulayabilir, değiştirebilir ve onlarla çalışabilirsiniz. 

## <a name="review-slas-in-the-azure-portal"></a>Azure portalında SLA'ları gözden geçirme

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta Azure Cosmos DB hesabı oluşturmayı, Veri Gezgini'ni kullanarak tablo oluşturmayı ve bir uygulamayı çalıştırmayı öğrendiniz.  Şimdi Tablo API'sini kullanarak verilerinizi sorgulayabilirsiniz.  

> [!div class="nextstepaction"]
> [Tablo verilerini Tablo API’sine aktarma](table-import.md)
