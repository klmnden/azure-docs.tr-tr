<properties
    pageTitle="DocumentDB hesabı oluşturma| Microsoft Azure"
    description="Azure DocumentDB ile bir NoSQL veritabanı oluşturun. Bir DocumentDB hesabı oluşturmak ve üstün hızlı, küresel ölçekli NoSQL veritabanınızı oluşturmaya başlamak için bu yönergeleri izleyin." 
    keywords="bir veritabanı derleme"
    services="documentdb"
    documentationCenter=""
    authors="mimig1"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/25/2016"
    ms.author="mimig"/>

# Azure portalını kullanarak DocumentDB hesabı oluşturma

> [AZURE.SELECTOR]
- [Azure Portalı](documentdb-create-account.md)
- [Azure CLI ve ARM](documentdb-automation-resource-manager-cli.md)

Microsoft Azure DocumentDB ile bir veritabanı oluşturmak için şunları yapmanız gerekir:

- Bir Azure hesabınız olmalıdır. Zaten bir hesabınız yoksa [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free) edinebilirsiniz. 
- Bir DocumentDB hesabı oluşturmanız gerekir.  

Azure portal, Azure Resource Manager şablonları veya Azure komut satırı arabiriminden (CLI) herhangi birini kullanarak bir DocumentDB hesabı oluşturabilirsiniz. Bu makale Azure portalını kullanarak bir DocumentDB hesabının nasıl oluşturulacağını göstermektedir. Azure Resource Manager veya Azure CLI'yı kullanarak bir hesap oluşturmak için bkz. [DocumentDB veritabanı hesabı oluşturmayı otomatikleştirme](documentdb-automation-resource-manager-cli.md).

DocumentDB'de yeni misiniz? Çevrimiçi portalda en yaygın görevlerin nasıl tamamlanacağını görmek için Scott Hanselman'ın [bu](https://azure.microsoft.com/documentation/videos/create-documentdb-on-azure/) dört dakikalık videosunu izleyin.

[AZURE.INCLUDE [documentdb-create-dbaccount](../../includes/documentdb-create-dbaccount.md)]

## Sonraki adımlar

Artık bir DocumentDB hesabına sahip olduğunuza göre, sonraki adım bir DocumentDB veritabanı oluşturmaktır. Aşağıdakilerden birini kullanarak bir veritabanı oluşturabilirsiniz:

- Azure portalı, açıklama için bkz. [Azure portalını kullanarak bir DocumentDB veritabanı oluşturma](documentdb-create-database.md).
- GitHub'daki [azure-documentdb-net](https://github.com/Azure/azure-documentdb-net/tree/master/samples/code-samples) deposunun [DatabaseManagement](https://github.com/Azure/azure-documentdb-net/tree/master/samples/code-samples/DatabaseManagement) projesindeki C#.NET örnekleri.
- Her şeyi içeren öğreticiler: [.NET](documentdb-get-started.md), [.NET MVC](documentdb-dotnet-application.md), [Java](documentdb-java-application.md), [Node.js](documentdb-nodejs-application.md) veya [Python](documentdb-python-application.md).
- [DocumentDB SDK'ları](documentdb-sdk-dotnet.md). DocumentDB .NET, Java, Python, Node.js ve JavaScript API SDK'larına sahiptir.


Veritabanınızı oluşturduktan sonra, veritabanına [bir veya daha fazla koleksiyon eklemeniz](documentdb-create-collection.md), ardından koleksiyonlara [belgeler eklemeniz](documentdb-view-json-document-explorer.md) gerekir.

Bir koleksiyonda belgeleriniz olduktan sonra, [DocumentDB SQL](documentdb-sql-query.md) kullanarak belgelerinizde [sorgular yürütmek](documentdb-sql-query.md#executing-queries) için portaldaki [Sorgu Gezgini](documentdb-query-collections-query-explorer.md), [REST API'si](https://msdn.microsoft.com/library/azure/dn781481.aspx) veya [SDK'lardan](documentdb-sdk-dotnet.md) birini kullanabilirsiniz.

DocumentDB hakkında daha fazla bilgi edinmek için şu kaynakları araştırın:

-   [DocumentDB için öğrenme yolu](https://azure.microsoft.com/documentation/learning-paths/documentdb/)
-   [DocumentDB kaynak modeli ve kavramları](documentdb-resources.md)



<!--HONumber=ago16_HO5-->


