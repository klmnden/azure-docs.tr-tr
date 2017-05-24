---
title: "Azure Cosmos DB hesabı oluşturma | Microsoft Docs"
description: "Azure Cosmos DB ile bir NoSQL veritabanı oluşturun. Azure Cosmos DB hesabı oluşturmak ve üstün hızlı, küresel ölçekli NoSQL veritabanınızı oluşturmaya başlamak için bu yönergeleri uygulayın."
keywords: "veritabanı oluşturma"
services: cosmosdb
documentationcenter: 
author: mimig1
manager: jhubbard
editor: monicar
ms.assetid: 0e7f8488-7bb7-463e-b6fd-3ae91a02c03a
ms.service: cosmosdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/17/2017
ms.author: mimig
redirect_url: https://aka.ms/acdbnetqa
ROBOTS: NOINDEX, NOFOLLOW
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 13621d942f2880f4dd1523315f43099eca2607d8
ms.contentlocale: tr-tr
ms.lasthandoff: 05/10/2017


---
# <a name="how-to-create-an-azure-cosmos-db-nosql-account-using-the-azure-portal"></a>Azure portalını kullanarak Azure Cosmos DB NoSQL hesabı oluşturma
> [!div class="op_single_selector"]
> * [Azure portal](documentdb-create-account.md)
> * [Azure CLI 1.0](documentdb-automation-resource-manager-cli-nodejs.md)
> * [Azure CLI 2.0](documentdb-automation-resource-manager-cli.md)
> * [Azure PowerShell](documentdb-manage-account-with-powershell.md)

Microsoft Azure Cosmos DB ile bir veritabanı oluşturmak için:

* Bir Azure hesabınızın olması gerekir. Hesabınız yoksa [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free) edinebilirsiniz.
* Bir Azure Cosmos DB hesabı oluşturun.  

Azure portalını, Azure Resource Manager şablonlarını veya Azure komut satırı arabiriminden (CLI) herhangi birini kullanarak bir Azure Cosmos DB hesabı oluşturabilirsiniz. Bu makalede Azure portalını kullanarak Azure Cosmos DB hesabını nasıl oluşturacağınız gösterilmektedir. Azure Resource Manager'ı veya Azure CLI'sini kullanarak bir hesap oluşturmak için bkz. [Azure Cosmos DB veritabanı hesabı oluşturmayı otomatikleştirme](documentdb-automation-resource-manager-cli.md).

Azure Cosmos DB’de yeni misiniz? Çevrimiçi portalda en yaygın görevleri nasıl tamamlayacağınızı görmek için Scott Hanselman tarafından hazırlanan [bu](https://azure.microsoft.com/documentation/videos/create-documentdb-on-azure/) dört dakikalık videoyu izleyin.

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Sol gezinti bölmesinden **Yeni**, **Veritabanları** ve ardından **Azure Cosmos DB** öğesine tıklayın.

   ![Diğer Hizmetler ve NoSQL (Azure Cosmos DB) seçeneklerinin vurgulandığı Azure portalı ekran görüntüsü](./media/documentdb-create-account/create-nosql-db-databases-json-tutorial-1.png)  
3. **Yeni hesap** dikey penceresinde, Azure Cosmos DB hesabı için istenen yapılandırmayı belirtin.

    ![Yeni Azure Cosmos DB dikey penceresinin ekran görüntüsü](./media/documentdb-create-account/create-nosql-db-databases-json-tutorial-2.png)

   * **Kimlik** kutusuna, Azure Cosmos DB hesabını tanımlayacak bir ad girin.  **Kimlik** doğrulandığında, **Kimlik** kutusunda yeşil bir onay işareti görünür. **Kimlik** değeri, URI içinde konak adı olarak kullanılır. **Kimlik**, yalnızca küçük harf, rakam ve "-" karakteri içerebilir; 3 ila 50 karakter uzunluğunda olmalıdır. *documents.azure.com*'un seçtiğiniz uç nokta adına eklendiğine ve bunun, Azure Cosmos DB hesabınızın uç noktası olarak kullanıldığına dikkat edin.
   * **NoSQL API** kutusunda, kullanılacak programlama modelini seçin:

     * **DocumentDB**: DocumentDB API'si; HTTP [REST](https://msdn.microsoft.com/library/azure/dn781481.aspx)'in yanı sıra .NET, Java, Node.js, Python ve JavaScript [SDK'ları](documentdb-sdk-dotnet.md) aracılığıyla kullanılabilir ve tüm DocumentDB işlevlerine programlama erişimi sunar.
     * **MongoDB**: DocumentDB, **MongoDB** API'leri için de [protokol düzeyinde destek](documentdb-protocol-mongodb.md) sunar. MongoDB API'si seçeneğini belirlediğinizde, DocumentDB ile iletişim kurmak için var olan MongoDB SDK'larını ve [araçlarını](documentdb-mongodb-mongochef.md) kullanabilirsiniz. DocumentDB'yi [hiçbir kod değişikliğine ihtiyaç duymadan](documentdb-connect-mongodb-account.md) kullanmak için, var olan MongoDB uygulamalarınızı [taşıyabilir](documentdb-import-data.md) ve sınırsız ölçek, küresel çoğaltma ve diğer özelliklere sahip bir hizmet olarak tamamı yönetilen veritabanınızdan faydalanabilirsiniz.
   * **Abonelik** için, Azure Cosmos DB hesabınızda kullanmak istediğiniz Azure aboneliğini seçin. Hesabınızda yalnızca bir abonelik varsa bu hesap varsayılan olarak seçilidir.
   * **Kaynak Grubu**'nda, Azure Cosmos DB hesabınız için bir kaynak grubu seçin veya oluşturun.  Varsayılan olarak yeni bir kaynak grubu oluşturulur. Daha fazla bilgi edinmek için bkz. [Azure portalını kullanarak Azure kaynaklarınızı yönetme](../azure-portal/resource-group-portal.md).
   * Azure Cosmos DB hesabınızın barındırılacağı coğrafi konumu belirtmek için **Konum**'u kullanın.
4. Yeni Azure Cosmos DB hesabı seçenekleri yapılandırıldıktan sonra **Oluştur**'a tıklayın. Dağıtım durumunu denetlemek için, Bildirimler hub'ına bakın.  

   ![Hızlı bir şekilde veritabanı oluşturma - Azure Cosmos DB hesabının oluşturulduğunu gösteren Bildirimler hub'ı ekran görüntüsü](./media/documentdb-create-account/create-nosql-db-databases-json-tutorial-4.png)  

   ![Azure Cosmos DB hesabının başarılı bir şekilde oluşturulduğunu ve bir kaynak grubuna dağıtıldığını gösteren Bildirimler hub'ı ekran görüntüsü - Çevrimiçi veritabanı oluşturucu bildirimi](./media/documentdb-create-account/create-nosql-db-databases-json-tutorial-5.png)
5. Azure Cosmos DB hesabı oluşturulduktan sonra, varsayılan ayarlarla birlikte kullanıma hazırdır. Azure Cosmos DB hesabının varsayılan tutarlılığı **Oturum** olarak ayarlandı.  Kaynak menüsünde **Varsayılan Tutarlılık** seçeneğine tıklayarak varsayılan tutarlılık ayarını belirleyebilirsiniz. Azure Cosmos DB tarafından sunulan tutarlılık düzeyleri hakkında daha fazla bilgi edinmek için bkz. [Azure Cosmos DB'deki tutarlılık düzeyleri](documentdb-consistency-levels.md).

   ![Kaynak Grubu dikey penceresinin ekran görüntüsü - uygulama geliştirmesini başlatma](./media/documentdb-create-account/create-nosql-db-databases-json-tutorial-6.png)  

   ![Tutarlılık Düzeyi dikey penceresinin ekran görüntüsü - Oturum Tutarlılığı](./media/documentdb-create-account/create-nosql-db-databases-json-tutorial-7.png)  

[How to: Create an Azure Cosmos DB account]: #Howto
[Next steps]: #NextSteps


## <a name="next-steps"></a>Sonraki adımlar
Bir Azure Cosmos DB hesabı oluşturduğunuza göre, sıradaki adımda bir Azure Cosmos DB koleksiyonu ve veritabanı oluşturacaksınız.

Şunlardan birini kullanarak yeni bir koleksiyon ve veritabanı oluşturabilirsiniz:

* Azure portalı (bkz. [Azure portalını kullanarak Azure Cosmos DB koleksiyonu oluşturma](documentdb-create-collection.md)).
* [.NET](documentdb-get-started.md), [.NET MVC](documentdb-dotnet-application.md), [Java](documentdb-java-application.md), [Node.js](documentdb-nodejs-application.md) veya [Python](documentdb-python-application.md) örnek verilerini içeren geniş kapsamlı öğreticiler.
* GitHub'da bulunan [.NET](documentdb-dotnet-samples.md#database-examples), [Node.js](documentdb-nodejs-samples.md#database-examples) veya [Python](documentdb-python-samples.md#database-examples) örnek kodu.
* [.NET](documentdb-sdk-dotnet.md), [.NET Core](documentdb-sdk-dotnet-core.md), [Node.js](documentdb-sdk-node.md), [Java](documentdb-sdk-java.md), [Python](documentdb-sdk-python.md) ve [REST](https://msdn.microsoft.com/library/azure/mt489072.aspx) SDK'ları.

Veritabanınızı ve koleksiyonunuzu oluşturduktan sonra, koleksiyonlara [belge eklemeniz](documentdb-view-json-document-explorer.md) gerekir.

Koleksiyonda belge yükledikten sonra, [DocumentDB SQL](documentdb-sql-query.md)'i kullanarak belgelerinizde [sorgu yürütebilirsiniz](documentdb-sql-query.md#ExecutingSqlQueries). Portaldaki [Sorgu Gezgini](documentdb-query-collections-query-explorer.md)'ni, [REST API](https://msdn.microsoft.com/library/azure/dn781481.aspx)'yi veya [SDK](documentdb-sdk-dotnet.md)'lardan birini kullanarak sorgu yürütebilirsiniz.

### <a name="learn-more"></a>Daha fazla bilgi edinin
Azure Cosmos DB hakkında daha fazla bilgi için şu kaynakları araştırın:

* [Azure Cosmos DB’ye Giriş](../cosmos-db/introduction.md)

