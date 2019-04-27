---
title: Verilerinizi görselleştirin ve Qlik Sense Azure Cosmos DB'ye bağlanma
description: Bu makalede Qlik Sense Azure Cosmos DB'ye bağlanma ve verilerinizi görselleştirmek için gereken adımlar açıklanmaktadır.
ms.service: cosmos-db
author: SnehaGunda
ms.author: sngun
ms.topic: conceptual
ms.date: 10/22/2018
ms.reviewer: sngun
ms.openlocfilehash: 4532962b6fd9f40fad625ab000116e5a617682e5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60765955"
---
# <a name="connect-qlik-sense-to-azure-cosmos-db-and-visualize-your-data"></a>Verilerinizi görselleştirin ve Qlik Sense Azure Cosmos DB'ye bağlanma

Qlik Sense farklı kaynaklardan gelen verileri tek bir görünümde birleştiren bir veri görselleştirme aracıdır. Verilere anında öngörü sahibi olabilir, böylece verilerinizi olası her ilişkide Qlik Sense dizinler. Azure Cosmos DB veri Qlik Sense kullanarak görselleştirebilirsiniz. Bu makalede Qlik Sense Azure Cosmos DB'ye bağlanma ve verilerinizi görselleştirmek için gereken adımlar açıklanmaktadır. 

> [!NOTE]
> Qlik Sense Azure Cosmos DB'ye bağlanmanın şu anda SQL API'sini ve Azure Cosmos DB'nin API'sini yalnızca MongoDB hesapları için desteklenir.

Bağlama Qlik Sense için Azure Cosmos DB ile yapabilecekleriniz:

* Cosmos DB SQL ODBC Bağlayıcısı'nı kullanarak API.

* Azure Cosmos DB'nin MongoDB için API (şu anda Önizleme aşamasında) Qlik algılama MongoDB bağlayıcısını kullanarak.

* MongoDB ve Qlik Sense REST API Bağlayıcısı kullanarak SQL API'si için Azure Cosmos DB API.

* Cosmos DB Mongo DB Qlik çekirdek için gRPC bağlayıcısını kullanarak API.
ODBC Bağlayıcısı'nı kullanarak Cosmos DB SQL API'sine bağlanma ayrıntıları bu makalede açıklanır.

ODBC Bağlayıcısı'nı kullanarak Cosmos DB SQL API'sine bağlanma ayrıntıları bu makalede açıklanır.

## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki yönergeleri izlemeden önce aşağıdaki kaynakları hazır olduğundan emin olun:

* İndirme [Qlik algılama Masaüstü](https://www.qlik.com/us/try-or-buy/download-qlik-sense) veya Qlik Sense ile azure'da ayarlama [Qlik Sense Market öğesi yükleme](https://azuremarketplace.microsoft.com/marketplace/apps/qlik.qlik-sense).

* İndirme [video oyun veri](https://www.kaggle.com/gregorut/videogamesales), bu örnek verileri CSV biçimindedir. Bu verileri bir Cosmos DB hesabını depolamak ve Qlik Sense içinde görselleştirin.

* İçinde açıklanan adımları kullanarak bir Azure Cosmos DB SQL API hesabı oluşturma [hesap oluşturma](create-sql-api-dotnet.md#create-account) hızlı başlangıç makalesi bölümü.

* [Bir veritabanı ve koleksiyonu oluşturma](create-sql-api-dotnet.md#create-collection-database) –, kullanım 1000 RU/sn için toplama aktarım hızı değerinde ayarlayabilirsiniz. 

* Örnek video oyun satış verileri Cosmos DB hesabınıza yükleyin. Azure Cosmos DB veri geçiş aracı kullanarak verileri içeri aktarabilirsiniz, yapabileceğiniz bir [sıralı](import-data.md#SQLSeqTarget) veya [toplu içeri aktarma](import-data.md#SQLBulkTarget) veri. Cosmos DB hesabına alınacak veri için yaklaşık 3-5 dakika sürer.

* İndirme, yükleme ve adımları kullanarak ODBC sürücüsü yapılandırma [ODBC sürücüsü ile Cosmos DB'ye bağlama](odbc-driver.md) makalesi. Video oyun veri basit bir veri kümesi ve şema düzenleme, yalnızca varsayılan koleksiyon eşleme şema kullanmak yok.

## <a name="connect-qlik-sense-to-cosmos-db"></a>Qlik Sense Cosmos DB'ye bağlanma

1. Qlik Sense açın ve seçin **yeni uygulama oluştur**. Seçin ve uygulama için bir ad sağlayın **Oluştur**.

   ![Yeni bir Qlik Sense uygulaması oluşturma](./media/visualize-qlik-sense/create-new-qlik-sense-app.png)

2. Yeni Uygulama başarıyla oluşturulduktan sonra seçin **uygulama açma** ve **dosyalarını ve diğer kaynaklardan veri ekleme**. 

3. Veri kaynaklarından seçin **ODBC** yeni bağlantı Kurulum penceresini açın. 

4. Geçiş **Kullanıcı DSN** ve daha önce oluşturduğunuz ODBC bağlantısı seçin. Bağlantısını ve ardından bir ad verin **Oluştur**. 

   ![Yeni bağlantı oluşturma](./media/visualize-qlik-sense/create-new-connection.png)

5. Bağlantıyı oluşturduktan sonra video oyun verilerin bulunduğu koleksiyon veritabanını seçin ve ardından Önizleme.

   ![Veritabanı ve koleksiyonu seçin](./media/visualize-qlik-sense/choose-database-and-collection.png) 

6. Ardından **veri ekleme** Qlik Sense verileri yüklenemedi. Qlik Sense veri yükledikten sonra Öngörüler oluşturmak ve veriler üzerinde analiz gerçekleştirin. Insights'ı kullanın veya video oyunları satış keşfetmeye kendi uygulamanızı oluşturun. Aşağıdaki resimde gösterilmektedir 

   ![Verileri görselleştirme](./media/visualize-qlik-sense/visualize-data.png)

### <a name="limitations-when-connecting-with-odbc"></a>ODBC ile bağlanırken sınırlamaları 

Cosmos DB şemasız dağıtılmış Geliştirici gereksinimlerini karşılamaya modellenmiş sürücülerle veritabanıdır. ODBC sürücüsü, sütunların veri türlerini ve diğer özellikleri çıkarsamak için şema ile bir veritabanı gerektirir. SQL API'si, ANSI SQL olmadığı için normal SQL sorgusu veya ilişkisel özelliğiyle DML sözdizimi Cosmos DB SQL API için geçerli değildir. Bu nedenle, ODBC sürücüsü ile verilen SQL deyimlerini tüm yapılar için eşdeğerleri olmayan Cosmos DB'ye özel SQL söz dizimi çevrilir. Bu çeviri sorunları önlemek için ODBC bağlantısı kurma, bir şema uygulamanız gerekir. [ODBC sürücüsü ile bağlama](odbc-driver.md) makale size öneriler ve yöntemleri şema yapılandırmanıza yardımcı olması için. Bu eşleme için Cosmos DB hesabı içindeki her veritabanı/koleksiyonu oluşturmak emin olun.

## <a name="next-steps"></a>Sonraki Adımlar

Power BI gibi bir farklı görselleştirme aracı kullanıyorsanız, bunu aşağıdaki belgedeki yönergeleri kullanarak bağlanabilirsiniz:

* [Power BI Bağlayıcısı'nı kullanarak Cosmos DB verileri Görselleştir](powerbi-visualize.md)
