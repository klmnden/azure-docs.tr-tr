---
title: Azure tablo depolaması ve Visual Studio bağlı Hizmetleri (ASP.NET) kullanmaya başlama | Microsoft Docs
description: Visual Studio bağlantılı hizmetler kullanarak bir depolama hesabı bağlandıktan sonra Visual Studio'da ASP.NET projesinde Azure tablo depolaması ile çalışmaya başlamak nasıl
services: storage
documentationcenter: ''
author: kraigb
manager: ghogen
editor: ''
ms.assetid: af81a326-18f4-4449-bc0d-e96fba27c1f8
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/21/2016
ms.author: kraigb
ms.openlocfilehash: 646ff3a12d1b28f99376ea67af25f1b6858d675a
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="get-started-with-azure-table-storage-and-visual-studio-connected-services-aspnet"></a>Azure tablo depolaması ve Visual Studio bağlı Hizmetleri (ASP.NET) kullanmaya başlama
[!INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Genel Bakış

Azure Table storage, büyük miktarlarda yapılandırılmış verileri depolamak sağlar. Kimliği doğrulanmış çağrılarından içinden ve dışından Azure bulut kabul eden bir NoSQL veri deposu hizmetidir. Azure tabloları, yapılandırılmış ve ilişkisel olmayan verilerin depolanması için idealdir.

Bu öğretici Azure tablo depolama varlıkları kullanarak bazı genel senaryolar için ASP.NET kodunun nasıl yazılacağını gösterir. Bu senaryolar, tablo oluşturma ve ekleme, sorgulama ve tablo varlıkları silme içerir. 

## <a name="prerequisites"></a>Önkoşullar

* [Microsoft Visual Studio](https://www.visualstudio.com/downloads/)
* [Azure depolama hesabı](../storage/common/storage-create-storage-account.md#create-a-storage-account)

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/vs-storage-aspnet-getting-started-create-azure-account.md)]

[!INCLUDE [storage-development-environment-include](../../includes/vs-storage-aspnet-getting-started-setup-dev-env.md)]

### <a name="create-an-mvc-controller"></a>Bir MVC denetleyicisi oluşturun. 

1. İçinde **Çözüm Gezgini**, sağ **denetleyicileri**ve bağlam menüsünden seçin **Ekle -> denetleyicisi**.

    ![Bir ASP.NET MVC uygulamasına denetleyici ekleme](./media/vs-storage-aspnet-getting-started-tables/add-controller-menu.png)

1. Üzerinde **İskele Ekle** iletişim kutusunda **MVC 5 denetleyici - boş**seçip **Ekle**.

    ![MVC denetleyicisi türünü belirtin](./media/vs-storage-aspnet-getting-started-tables/add-controller.png)

1. Üzerinde **denetleyici Ekle** iletişim kutusunda, denetleyici adı *TablesController*seçip **Ekle**.

    ![MVC Denetleyici adı](./media/vs-storage-aspnet-getting-started-tables/add-controller-name.png)

1. Aşağıdakileri ekleyin *kullanarak* yönergeleri `TablesController.cs` dosyası:

    ```csharp
    using Microsoft.Azure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Table;
    ```

### <a name="create-a-model-class"></a>Bir model sınıfı oluşturma

Bu örneklerin çoğu makale kullanımı bir **TableEntity**-türetilmiş sınıf adlı **CustomerEntity**. Aşağıdaki adımlar bu sınıfın bir model sınıfı olarak bildirme aracılığıyla Kılavuzu:

1. İçinde **Çözüm Gezgini**, sağ **modelleri**ve bağlam menüsünden seçin **Ekle -> sınıfı**.

1. Üzerinde **Yeni Öğe Ekle** iletişim kutusunda, ad sınıfı **CustomerEntity**.

1. Açık `CustomerEntity.cs` dosya ve aşağıdakileri ekleyin **kullanarak** yönergesi:

    ```csharp
    using Microsoft.WindowsAzure.Storage.Table;
    ```

1. Tamamlandığında, sınıf olduğu gibi aşağıdaki kodu bildirilmiş böylece sınıfı değiştirin. Sınıf adı verilen bir varlık sınıfı bildirir **CustomerEntity** , müşterinin adını satır anahtarını ve Soyadı bölüm anahtarı olarak kullanır.

    ```csharp
    public class CustomerEntity : TableEntity
    {
        public CustomerEntity(string lastName, string firstName)
        {
            this.PartitionKey = lastName;
            this.RowKey = firstName;
        }

        public CustomerEntity() { }

        public string Email { get; set; }
    }
    ```

## <a name="create-a-table"></a>Bir tablo oluşturma

Aşağıdaki adımlar bir tablo oluşturulacağını gösterir:

> [!NOTE]
> 
> Bu bölümdeki adımları tamamladığınızdan varsayar [geliştirme ortamını ayarlama](#set-up-the-development-environment). 

1. `TablesController.cs` dosyasını açın.

1. Adlı bir yöntem ekleyin **CreateTable** döndüren bir **ActionResult**.

    ```csharp
    public ActionResult CreateTable()
    {
        // The code in this section goes here.

        return View();
    }
    ```

1. İçinde **CreateTable** yöntemi, get bir **CloudStorageAccount** depolama hesabı bilgilerini temsil eden nesne. Azure hizmet yapılandırmasından depolama bağlantı dizesi ve depolama hesabı bilgilerini almak için aşağıdaki kodu kullanın: (değişiklik  *&lt;depolama hesabı adı >* Azure depolama hesabı adını eriştiğiniz.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. Alma bir **CloudTableClient** nesnesi bir tablo hizmeti istemcisi temsil eder.
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. Alma bir **CloudTable** istenilen tablo adı için bir başvuru temsil eden nesne. **CloudTableClient.GetTableReference** yöntemi tablo depolama doğrulamasını yapmaz. Tablo var olup olmadığına bakılmaksızın başvuru döndürülür. 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. Çağrı **CloudTable.CreateIfNotExists** henüz yoksa, tablo oluşturmak için yöntem. **CloudTable.CreateIfNotExists** yöntemi döndürür **true** tablo mevcut değil ve başarıyla oluşturuldu. Aksi takdirde, **false** döndürülür.    

    ```csharp
    ViewBag.Success = table.CreateIfNotExists();
    ```

1. Güncelleştirme **ViewBag** tablonun adı.

    ```csharp
    ViewBag.TableName = table.Name;
    ```

1. İçinde **Çözüm Gezgini**, genişletin **görünümleri** klasörünü sağ tıklatın **tabloları**ve bağlam menüsünden seçin **Ekle -> Görünüm**.

1. Üzerinde **Görünüm Ekle** iletişim kutusunda, girin **CreateTable** Görünüm adı ' nı seçip için **Ekle**.

1. Açık `CreateTable.cshtml`ve aşağıdaki kod parçacığını gibi görünüyor şekilde değiştirin:

    ```csharp
    @{
        ViewBag.Title = "Create Table";
    }
    
    <h2>Create Table results</h2>

    Creation of @ViewBag.TableName @(ViewBag.Success == true ? "succeeded" : "failed")
    ```

1. İçinde **Çözüm Gezgini**, genişletin **görünümleri -> paylaşılan** klasörü ve açık `_Layout.cshtml`.

1. Son sonra **Html.ActionLink**, aşağıdakileri ekleyin **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Create table", "CreateTable", "Tables")</li>
    ```

1. Uygulamayı çalıştırmak ve seçmek **Create table** aşağıdaki ekran görüntüsüne benzer sonuçlar görmek için:
  
    ![Tablo oluştur](./media/vs-storage-aspnet-getting-started-tables/create-table-results.png)

    Daha önce belirtildiği gibi **CloudTable.CreateIfNotExists** yöntemi döndürür **doğru** yalnızca tablo mevcut değil ve oluşturulur. Bu nedenle, tablo mevcut olduğunda uygulama çalıştırırsanız, yöntem **false**. Birden çok kez uygulamayı çalıştırmak için uygulamayı yeniden çalıştırmadan önce tabloyu silmeniz gerekir. Aracılığıyla tablo silinirken yapılabilir **CloudTable.Delete** yöntemi. Tabloyu kullanarak silebilirsiniz [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) veya [Microsoft Azure Storage Gezgini](../vs-azure-tools-storage-manage-with-storage-explorer.md).  

## <a name="add-an-entity-to-a-table"></a>Tabloya bir varlık ekleme

*Varlıkları* C eşlemesine\# özel bir sınıf kullanarak nesneleri türetilen **TableEntity**. Tabloya bir varlık eklemek için varlığınızın özelliklerini tanımlayan bir sınıf oluşturun. Bu bölümde, bölüm anahtarı olarak soyadını ve satır anahtarı müşterinin adını kullanan bir varlık sınıfın nasıl tanımlanacağını görürsünüz. Birlikte, bir varlığın bölüm ve sıra anahtarı varlığı tabloda benzersiz şekilde tanımlar. Aynı bölüm anahtarına sahip varlıklar farklı bölüm anahtarlı varlıklara göre daha hızlı sorgulanabilir ancak farklı bölüm anahtarlarının kullanılması paralel işlemler için daha büyük ölçeklendirme sağlar. Tablo hizmetinde depolanması gereken tüm özellikler için özellik ayarı hem değerlerini alma kullanıma sunan desteklenen bir tür genel özelliği olmalıdır.
Varlık sınıfı *gerekir* genel bir parametresiz oluşturucuya bildirin.

> [!NOTE]
> 
> Bu bölümdeki adımları tamamladığınızdan varsayar [geliştirme ortamını ayarlama](#set-up-the-development-environment).

1. `TablesController.cs` dosyasını açın.

1. Aşağıdaki yönergesi eklemek için kodda `TablesController.cs` dosya erişebilir **CustomerEntity** sınıfı:

    ```csharp
    using StorageAspnet.Models;
    ```

1. Adlı bir yöntem ekleyin **AddEntity** döndüren bir **ActionResult**.

    ```csharp
    public ActionResult AddEntity()
    {
        // The code in this section goes here.

        return View();
    }
    ```

1. İçinde **AddEntity** yöntemi, get bir **CloudStorageAccount** depolama hesabı bilgilerini temsil eden nesne. Azure hizmet yapılandırmasından depolama bağlantı dizesi ve depolama hesabı bilgilerini almak için aşağıdaki kodu kullanın: (değişiklik  *&lt;depolama hesabı adı >* Azure depolama hesabı adını eriştiğiniz.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. Alma bir **CloudTableClient** nesnesi bir tablo hizmeti istemcisi temsil eder.
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. Alma bir **CloudTable** olduğu bulacağınızı yeni bir varlık eklemek için tablo referansı temsil eden nesne. 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. Örnek oluşturma ve başlatma **CustomerEntity** sınıfı.

    ```csharp
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    ```

1. Oluşturma bir **TableOperation** müşteri varlığı ekler nesne.

    ```csharp
    TableOperation insertOperation = TableOperation.Insert(customer1);
    ```

1. Ekleme işlemi çağırarak yürütün **CloudTable.Execute** yöntemi. İnceleyerek işlemin sonucunu doğrulayabilirsiniz **TableResult.HttpStatusCode** özelliği. İstemci tarafından istenen eylem başarıyla işlendi 2xx durum kodu gösterir. Örneğin, yeni varlıklar sonuçları 204 işlemi başarıyla işlendi anlamına gelir, bir HTTP durum kodunu ve sunucunun başarılı eklemeleri herhangi bir içerik döndürmedi.

    ```csharp
    TableResult result = table.Execute(insertOperation);
    ```

1. Güncelleştirme **ViewBag** tablo adı ve ekleme işleminin sonuçları.

    ```csharp
    ViewBag.TableName = table.Name;
    ViewBag.Result = result.HttpStatusCode;
    ```

1. İçinde **Çözüm Gezgini**, genişletin **görünümleri** klasörünü sağ tıklatın **tabloları**ve bağlam menüsünden seçin **Ekle -> Görünüm**.

1. Üzerinde **Görünüm Ekle** iletişim kutusunda, girin **AddEntity** Görünüm adı ' nı seçip için **Ekle**.

1. Açık `AddEntity.cshtml`ve aşağıdaki kod parçacığını gibi görünüyor şekilde değiştirin:

    ```csharp
    @{
        ViewBag.Title = "Add entity";
    }
    
    <h2>Add entity results</h2>

    Insert of entity into @ViewBag.TableName @(ViewBag.Result == 204 ? "succeeded" : "failed")
    ```
1. İçinde **Çözüm Gezgini**, genişletin **görünümleri -> paylaşılan** klasörü ve açık `_Layout.cshtml`.

1. Son sonra **Html.ActionLink**, aşağıdakileri ekleyin **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Add entity", "AddEntity", "Tables")</li>
    ```

1. Uygulamayı çalıştırmak ve seçmek **varlık ekleme** aşağıdaki ekran görüntüsüne benzer sonuçlar görmek için:
  
    ![Varlık ekleme](./media/vs-storage-aspnet-getting-started-tables/add-entity-results.png)

    Varlık bölümündeki adımları izleyerek eklendiğini doğrulayabilirsiniz [tek bir varlık alma](#get-a-single-entity). Aynı zamanda [Microsoft Azure Storage Gezgini](../vs-azure-tools-storage-manage-with-storage-explorer.md) tablolarınız için tüm varlıkları görüntülemek için.

## <a name="add-a-batch-of-entities-to-a-table"></a>Bir tabloya toplu işlem varlık ekleme

İçin çalışabilme yanı sıra [aynı anda bir tabloya bir varlık eklemek](#add-an-entity-to-a-table), varlıkları toplu işlemde de ekleyebilirsiniz. Toplu işlemde varlıklar ekleme kodunuz ve Azure tablo hizmeti arasındaki gidiş dönüş sayısını azaltır. Aşağıdaki adımlar işlemi tek bir tablo için birden çok varlık ekleme ekleme gösterilmiştir:

> [!NOTE]
> 
> Bu bölümdeki adımları tamamladığınızdan varsayar [geliştirme ortamını ayarlama](#set-up-the-development-environment).

1. `TablesController.cs` dosyasını açın.

1. Adlı bir yöntem ekleyin **AddEntities** döndüren bir **ActionResult**.

    ```csharp
    public ActionResult AddEntities()
    {
        // The code in this section goes here.

        return View();
    }
    ```

1. İçinde **AddEntities** yöntemi, get bir **CloudStorageAccount** depolama hesabı bilgilerini temsil eden nesne. Azure hizmet yapılandırmasından depolama bağlantı dizesi ve depolama hesabı bilgilerini almak için aşağıdaki kodu kullanın: (değişiklik  *&lt;depolama hesabı adı >* Azure depolama hesabı adını eriştiğiniz.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. Alma bir **CloudTableClient** nesnesi bir tablo hizmeti istemcisi temsil eder.
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. Alma bir **CloudTable** olduğu bulacağınızı yeni varlıklar eklemek için tablo referansı temsil eden nesne. 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. Temel bazı müşteri nesneleri örneği **CustomerEntity** model bölümde sunulan sınıfı [tabloya bir varlık ekleme](#add-an-entity-to-a-table).

    ```csharp
    CustomerEntity customer1 = new CustomerEntity("Smith", "Jeff");
    customer1.Email = "Jeff@contoso.com";

    CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
    customer2.Email = "Ben@contoso.com";
    ```

1. Alma bir **TableBatchOperation** nesnesi.

    ```csharp
    TableBatchOperation batchOperation = new TableBatchOperation();
    ```

1. Varlıkları toplu ekleme işlemi nesnesine ekleyin.

    ```csharp
    batchOperation.Insert(customer1);
    batchOperation.Insert(customer2);
    ```

1. Toplu ekleme işlemi çağırarak yürütün **CloudTable.ExecuteBatch** yöntemi.   

    ```csharp
    IList<TableResult> results = table.ExecuteBatch(batchOperation);
    ```

1. **CloudTable.ExecuteBatch** yöntemi listesini döndürür **TableResult** nesneleri burada her **TableResult** nesne incelenmesi başarı veya başarısızlık, tek tek her işlemin belirlemek için. Bu örnek için listeden bir görünüme iletmek ve her bir işlemin sonuçlarını görüntülemek görünüm izin verin. 
 
    ```csharp
    return View(results);
    ```

1. İçinde **Çözüm Gezgini**, genişletin **görünümleri** klasörünü sağ tıklatın **tabloları**ve bağlam menüsünden seçin **Ekle -> Görünüm**.

1. Üzerinde **Görünüm Ekle** iletişim kutusunda, girin **AddEntities** Görünüm adı ' nı seçip için **Ekle**.

1. Açık `AddEntities.cshtml`ve şu şekilde görünen şekilde değiştirin.

    ```csharp
    @model IEnumerable<Microsoft.WindowsAzure.Storage.Table.TableResult>
    @{
        ViewBag.Title = "AddEntities";
    }
    
    <h2>Add-entities results</h2>
    
    <table border="1">
        <tr>
            <th>First name</th>
            <th>Last name</th>
            <th>HTTP result</th>
        </tr>
        @foreach (var result in Model)
        {
        <tr>
            <td>@((result.Result as StorageAspnet.Models.CustomerEntity).RowKey)</td>
            <td>@((result.Result as StorageAspnet.Models.CustomerEntity).PartitionKey)</td>
            <td>@result.HttpStatusCode</td>
        </tr>
        }
    </table>
    ```

1. İçinde **Çözüm Gezgini**, genişletin **görünümleri -> paylaşılan** klasörü ve açık `_Layout.cshtml`.

1. Son sonra **Html.ActionLink**, aşağıdakileri ekleyin **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Add entities", "AddEntities", "Tables")</li>
    ```

1. Uygulamayı çalıştırmak ve seçmek **varlıkları ekleyin** aşağıdaki ekran görüntüsüne benzer sonuçlar görmek için:
  
    ![Varlık ekleme](./media/vs-storage-aspnet-getting-started-tables/add-entities-results.png)

    Varlık bölümündeki adımları izleyerek eklendiğini doğrulayabilirsiniz [tek bir varlık alma](#get-a-single-entity). Aynı zamanda [Microsoft Azure Storage Gezgini](../vs-azure-tools-storage-manage-with-storage-explorer.md) tablolarınız için tüm varlıkları görüntülemek için.

## <a name="get-a-single-entity"></a>Tek bir varlık alma

Bu bölümde, tek bir varlık varlığın satır anahtarı ve bölüm anahtarı kullanarak bir tablodan alma gösterilmektedir. 

> [!NOTE]
> 
> Bu bölümdeki adımları tamamladığınızdan varsayar [geliştirme ortamını ayarlama](#set-up-the-development-environment)ve verileri kullanan [bir tabloya toplu işlem varlık ekleme](#add-a-batch-of-entities-to-a-table). 

1. `TablesController.cs` dosyasını açın.

1. Adlı bir yöntem ekleyin **GetSingle** döndüren bir **ActionResult**.

    ```csharp
    public ActionResult GetSingle()
    {
        // The code in this section goes here.

        return View();
    }
    ```

1. İçinde **GetSingle** yöntemi, get bir **CloudStorageAccount** depolama hesabı bilgilerini temsil eden nesne. Azure hizmet yapılandırmasından depolama bağlantı dizesi ve depolama hesabı bilgilerini almak için aşağıdaki kodu kullanın: (değişiklik  *&lt;depolama hesabı adı >* Azure depolama hesabı adını eriştiğiniz.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. Alma bir **CloudTableClient** nesnesi bir tablo hizmeti istemcisi temsil eder.
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. Alma bir **CloudTable** içinden, alma varlık tablo referansı temsil eden nesne. 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. Türetilen bir varlık nesnesini alır bir alma işlemi nesnesi oluşturmak **TableEntity**. İlk parametre *partitionKey*, ve ikinci parametre *rowKey*. Kullanarak **CustomerEntity** sınıf ve bölümde sunulan veri [bir tabloya toplu işlem varlık ekleme](#add-a-batch-of-entities-to-a-table), aşağıdaki kod parçacığını tablosu için sorgular bir **CustomerEntity** varlıkla bir *partitionKey* "Smith" değerini ve *rowKey* "Ben" değeri:

    ```csharp
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");
    ```

1. Alma işlemi yürütün.   

    ```csharp
    TableResult result = table.Execute(retrieveOperation);
    ```

1. Görüntü görünümüne sonucu geçirin.

    ```csharp
    return View(result);
    ```

1. İçinde **Çözüm Gezgini**, genişletin **görünümleri** klasörünü sağ tıklatın **tabloları**ve bağlam menüsünden seçin **Ekle -> Görünüm**.

1. Üzerinde **Görünüm Ekle** iletişim kutusunda, girin **GetSingle** Görünüm adı ' nı seçip için **Ekle**.

1. Açık `GetSingle.cshtml`ve aşağıdaki kod parçacığını gibi görünüyor şekilde değiştirin:

    ```csharp
    @model Microsoft.WindowsAzure.Storage.Table.TableResult
    @{
        ViewBag.Title = "GetSingle";
    }
    
    <h2>Get Single results</h2>
    
    <table border="1">
        <tr>
            <th>HTTP result</th>
            <th>First name</th>
            <th>Last name</th>
            <th>Email</th>
        </tr>
        <tr>
            <td>@Model.HttpStatusCode</td>
            <td>@((Model.Result as StorageAspnet.Models.CustomerEntity).RowKey)</td>
            <td>@((Model.Result as StorageAspnet.Models.CustomerEntity).PartitionKey)</td>
            <td>@((Model.Result as StorageAspnet.Models.CustomerEntity).Email)</td>
        </tr>
    </table>
    ```

1. İçinde **Çözüm Gezgini**, genişletin **görünümleri -> paylaşılan** klasörü ve açık `_Layout.cshtml`.

1. Son sonra **Html.ActionLink**, aşağıdakileri ekleyin **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Get single", "GetSingle", "Tables")</li>
    ```

1. Uygulamayı çalıştırmak ve seçmek **alma tek** aşağıdaki ekran görüntüsüne benzer sonuçlar görmek için:
  
    ![Tek Al](./media/vs-storage-aspnet-getting-started-tables/get-single-results.png)

## <a name="get-all-entities-in-a-partition"></a>Tüm varlıkları bir bölüme alma

Bölümünde belirtildiği gibi [tabloya bir varlık ekleme](#add-an-entity-to-a-table), bir bölüm ve satır anahtarı birleşimi bir tablodaki bir varlık benzersiz şekilde tanımlar. Aynı bölüm anahtarına sahip varlıklar farklı bölüm anahtarlarının varlıklar daha hızlı sorgulanabilir. Bu bölümde, bir tablo belirtilen bölümünden tüm varlıklar için sorgu göstermektedir.  

> [!NOTE]
> 
> Bu bölümdeki adımları tamamladığınızdan varsayar [geliştirme ortamını ayarlama](#set-up-the-development-environment)ve verileri kullanan [bir tabloya toplu işlem varlık ekleme](#add-a-batch-of-entities-to-a-table). 

1. `TablesController.cs` dosyasını açın.

1. Adlı bir yöntem ekleyin **GetPartition** döndüren bir **ActionResult**.

    ```csharp
    public ActionResult GetPartition()
    {
        // The code in this section goes here.

        return View();
    }
    ```

1. İçinde **GetPartition** yöntemi, get bir **CloudStorageAccount** depolama hesabı bilgilerini temsil eden nesne. Azure hizmet yapılandırmasından depolama bağlantı dizesi ve depolama hesabı bilgilerini almak için aşağıdaki kodu kullanın: (değişiklik  *&lt;depolama hesabı adı >* Azure depolama hesabı adını eriştiğiniz.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. Alma bir **CloudTableClient** nesnesi bir tablo hizmeti istemcisi temsil eder.
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. Alma bir **CloudTable** içinden, alma varlıkları tablo referansı temsil eden nesne. 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. Örneği bir **TableQuery** sorguda belirten nesnesini **burada** yan tümcesi. Kullanarak **CustomerEntity** sınıf ve bölümde sunulan veri [bir tabloya toplu işlem varlık ekleme](#add-a-batch-of-entities-to-a-table), tablonun tüm varlıklar için aşağıdaki kod parçacığını sorgular nerede **PartitionKey** (müşterinin soyadı) "Smith" değerine sahip:

    ```csharp
    TableQuery<CustomerEntity> query = 
        new TableQuery<CustomerEntity>()
        .Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));
    ```

1. Bir döngü içinde çağrı **CloudTable.ExecuteQuerySegmented** önceki adımda örneği sorgu nesnesi geçirme yöntemi.  **CloudTable.ExecuteQuerySegmented** yöntemi döndürür bir **TableContinuationToken** - nesnesinin zaman **null** -almak için daha fazla varlık olduğunu gösterir. Döngü içinde başka bir döngü döndürülen varlıkları yinelemek için kullanın. Aşağıdaki kod örneğinde, döndürülen her varlık bir listesine eklenir. Görüntülenmek üzere bir görünüme listesi döngü sona erer geçirilir sonra: 

    ```csharp
    List<CustomerEntity> customers = new List<CustomerEntity>();
    TableContinuationToken token = null;
    do
    {
        TableQuerySegment<CustomerEntity> resultSegment = table.ExecuteQuerySegmented(query, token);
        token = resultSegment.ContinuationToken;

        foreach (CustomerEntity customer in resultSegment.Results)
        {
            customers.Add(customer);
        }
    } while (token != null);

    return View(customers);
    ```

1. İçinde **Çözüm Gezgini**, genişletin **görünümleri** klasörünü sağ tıklatın **tabloları**ve bağlam menüsünden seçin **Ekle -> Görünüm**.

1. Üzerinde **Görünüm Ekle** iletişim kutusunda, girin **GetPartition** Görünüm adı ' nı seçip için **Ekle**.

1. Açık `GetPartition.cshtml`ve aşağıdaki kod parçacığını gibi görünüyor şekilde değiştirin:

    ```csharp
    @model IEnumerable<StorageAspnet.Models.CustomerEntity>
    @{
        ViewBag.Title = "GetPartition";
    }
    
    <h2>Get Partition results</h2>
    
    <table border="1">
        <tr>
            <th>First name</th>
            <th>Last name</th>
            <th>Email</th>
        </tr>
        @foreach (var customer in Model)
        {
        <tr>
            <td>@(customer.RowKey)</td>
            <td>@(customer.PartitionKey)</td>
            <td>@(customer.Email)</td>
        </tr>
        }
    </table>
    ```

1. İçinde **Çözüm Gezgini**, genişletin **görünümleri -> paylaşılan** klasörü ve açık `_Layout.cshtml`.

1. Son sonra **Html.ActionLink**, aşağıdakileri ekleyin **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Get partition", "GetPartition", "Tables")</li>
    ```

1. Uygulamayı çalıştırmak ve seçmek **alma bölüm** aşağıdaki ekran görüntüsüne benzer sonuçlar görmek için:
  
    ![Bölüm alma](./media/vs-storage-aspnet-getting-started-tables/get-partition-results.png)

## <a name="delete-an-entity"></a>Bir varlığı silme

Bu bölümde bir tablodan bir varlığı silmek nasıl gösterilmektedir.

> [!NOTE]
> 
> Bu bölümdeki adımları tamamladığınızdan varsayar [geliştirme ortamını ayarlama](#set-up-the-development-environment)ve verileri kullanan [bir tabloya toplu işlem varlık ekleme](#add-a-batch-of-entities-to-a-table). 

1. `TablesController.cs` dosyasını açın.

1. Adlı bir yöntem ekleyin **DeleteEntity** döndüren bir **ActionResult**.

    ```csharp
    public ActionResult DeleteEntity()
    {
        // The code in this section goes here.

        return View();
    }
    ```

1. İçinde **DeleteEntity** yöntemi, get bir **CloudStorageAccount** depolama hesabı bilgilerini temsil eden nesne. Azure hizmet yapılandırmasından depolama bağlantı dizesi ve depolama hesabı bilgilerini almak için aşağıdaki kodu kullanın: (değişiklik  *&lt;depolama hesabı adı >* Azure depolama hesabı adını eriştiğiniz.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. Alma bir **CloudTableClient** nesnesi bir tablo hizmeti istemcisi temsil eder.
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. Alma bir **CloudTable** içinden silmekte varlık tablo referansı temsil eden nesne. 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. Türetilen bir varlık nesnesini alır bir silme işlemi nesnesi oluşturmak **TableEntity**. Bu durumda, kullandığımız **CustomerEntity** sınıf ve bölümde sunulan veri [bir tabloya toplu işlem varlık ekleme](#add-a-batch-of-entities-to-a-table). Varlığın **ETag** geçerli bir değere ayarlamanız gerekir.  

    ```csharp
    TableOperation deleteOperation = 
        TableOperation.Delete(new CustomerEntity("Smith", "Ben") { ETag = "*" } );
    ```

1. Silme işlemini yürütün.   

    ```csharp
    TableResult result = table.Execute(deleteOperation);
    ```

1. Görüntü görünümüne sonucu geçirin.

    ```csharp
    return View(result);
    ```

1. İçinde **Çözüm Gezgini**, genişletin **görünümleri** klasörünü sağ tıklatın **tabloları**ve bağlam menüsünden seçin **Ekle -> Görünüm**.

1. Üzerinde **Görünüm Ekle** iletişim kutusunda, girin **DeleteEntity** Görünüm adı ' nı seçip için **Ekle**.

1. Açık `DeleteEntity.cshtml`ve aşağıdaki kod parçacığını gibi görünüyor şekilde değiştirin:

    ```csharp
    @model Microsoft.WindowsAzure.Storage.Table.TableResult
    @{
        ViewBag.Title = "DeleteEntity";
    }
    
    <h2>Delete Entity results</h2>
    
    <table border="1">
        <tr>
            <th>First name</th>
            <th>Last name</th>
            <th>HTTP result</th>
        </tr>
        <tr>
            <td>@((Model.Result as StorageAspnet.Models.CustomerEntity).RowKey)</td>
            <td>@((Model.Result as StorageAspnet.Models.CustomerEntity).PartitionKey)</td>
            <td>@Model.HttpStatusCode</td>
        </tr>
    </table>

    ```

1. İçinde **Çözüm Gezgini**, genişletin **görünümleri -> paylaşılan** klasörü ve açık `_Layout.cshtml`.

1. Son sonra **Html.ActionLink**, aşağıdakileri ekleyin **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Delete entity", "DeleteEntity", "Tables")</li>
    ```

1. Uygulamayı çalıştırmak ve seçmek **silmek varlık** aşağıdaki ekran görüntüsüne benzer sonuçlar görmek için:
  
    ![Tek Al](./media/vs-storage-aspnet-getting-started-tables/delete-entity-results.png)

## <a name="next-steps"></a>Sonraki adımlar
Azure’da veri depolama ile ilgili ek seçenekler hakkında daha fazla bilgi edinmek için daha fazla özellik kılavuzu görüntüleyin.

  * [Azure blob depolama ve Visual Studio bağlı Hizmetleri (ASP.NET) kullanmaya başlama](../storage/vs-storage-aspnet-getting-started-blobs.md)
  * [Azure kuyruk depolama ve Visual Studio bağlı Hizmetleri (ASP.NET) kullanmaya başlama](../storage/vs-storage-aspnet-getting-started-queues.md)
