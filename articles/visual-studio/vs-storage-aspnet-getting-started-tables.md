---
title: Azure tablo depolama ve Visual Studio bağlı Hizmetleri (ASP.NET) ile çalışmaya başlama | Microsoft Docs
description: Nasıl Visual Studio bağlı Hizmetler'i kullanarak bir depolama hesabına bağlandıktan sonra Visual Studio'da ASP.NET projesinde Azure tablo depolama kullanmaya başlama
services: storage
author: ghogen
manager: douge
ms.assetid: af81a326-18f4-4449-bc0d-e96fba27c1f8
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 12/21/2016
ms.author: ghogen
ms.openlocfilehash: ea50506df53bfd586656d0030be4536d9d3b907d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62122999"
---
# <a name="get-started-with-azure-table-storage-and-visual-studio-connected-services-aspnet"></a>Azure tablo depolama ve Visual Studio bağlı Hizmetleri (ASP.NET) kullanmaya başlama
[!INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Genel Bakış

Azure tablo depolama, büyük miktarlarda yapısal veriyi depolamanızı olanaklı kılar. Kimliği doğrulanmış çağrılarından içindeki ve Azure Bulutu dışındaki kabul eden bir NoSQL veri deposu hizmetidir. Azure tabloları, yapılandırılmış ve ilişkisel olmayan verilerin depolanması için idealdir.

Bu öğretici, Azure tablo depolama varlıkları kullanarak bazı genel senaryolar için ASP.NET kodunun nasıl yazılacağını gösterir. Bu senaryolar, tablo oluşturma ve ekleme, sorgulama ve tablo varlıklarını silme içerir. 

## <a name="prerequisites"></a>Önkoşullar

* [Microsoft Visual Studio](https://www.visualstudio.com/downloads/)
* [Azure depolama hesabı](../storage/common/storage-quickstart-create-account.md)

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/vs-storage-aspnet-getting-started-create-azure-account.md)]

[!INCLUDE [storage-development-environment-include](../../includes/vs-storage-aspnet-getting-started-setup-dev-env.md)]

### <a name="create-an-mvc-controller"></a>MVC denetleyicisi oluşturma 

1. İçinde **Çözüm Gezgini**, sağ **denetleyicileri**, bağlam menüsünden seçin **Ekle -> denetleyicisi**.

    ![ASP.NET MVC uygulaması için denetleyici ekleme](./media/vs-storage-aspnet-getting-started-tables/add-controller-menu.png)

1. Üzerinde **İskele Ekle** iletişim kutusunda **MVC 5 denetleyici - boş**seçip **Ekle**.

    ![MVC denetleyici türü belirtin](./media/vs-storage-aspnet-getting-started-tables/add-controller.png)

1. Üzerinde **denetleyici Ekle** iletişim kutusunda, denetleyici adı *TablesController*seçip **Ekle**.

    ![MVC Denetleyici adı](./media/vs-storage-aspnet-getting-started-tables/add-controller-name.png)

1. Aşağıdaki *kullanarak* yönergelerini `TablesController.cs` dosyası:

    ```csharp
    using Microsoft.Azure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Table;
    ```

### <a name="create-a-model-class"></a>Bir model sınıfı oluşturma

Bu örneklerin çoğu makale kullanımı bir **TableEntity**-türetilmiş sınıf adlı **CustomerEntity**. Aşağıdaki adımlar bu sınıfı bir model sınıfı olarak bildirme aracılığıyla Kılavuzu:

1. İçinde **Çözüm Gezgini**, sağ **modelleri**, bağlam menüsünden seçin **Ekle -> sınıf**.

1. Üzerinde **Yeni Öğe Ekle** iletişim kutusunda, sınıf adı **CustomerEntity**.

1. Açık `CustomerEntity.cs` dosyasını bulun ve aşağıdakileri ekleyin **kullanarak** yönergesi:

    ```csharp
    using Microsoft.WindowsAzure.Storage.Table;
    ```

1. İşiniz bittiğinde sınıfı aşağıdaki kodda gösterildiği gibi bildirilen böylece sınıfı değiştirin. Sınıf adlı bir varlık sınıfı bildirir **CustomerEntity** , müşterinin ad son adı ve satır anahtarı bölüm anahtarı olarak kullanır.

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

Aşağıdaki adımlar, bir tablo oluşturma işlemini göstermektedir:

> [!NOTE]
> 
> Bu bölümdeki adımları bitirdiğinizi [geliştirme ortamını ayarlama](#set-up-the-development-environment). 

1. `TablesController.cs` dosyasını açın.

1. Adlı bir yöntem ekleyin **CreateTable** döndüren bir **actionresult öğesini**.

    ```csharp
    public ActionResult CreateTable()
    {
        // The code in this section goes here.

        return View();
    }
    ```

1. İçinde **CreateTable** yöntemi almak bir **CloudStorageAccount** depolama hesap bilgilerini temsil eden nesne. Azure hizmet yapılandırmasından bir depolama bağlantı dizesi ve depolama hesabı bilgileri almak için aşağıdaki kodu kullanın: (Değişiklik  *&lt;depolama hesabı adı >* erişmek için Azure depolama hesabı adı.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. Alma bir **CloudTableClient** nesnesi bir tablo hizmeti istemcisini temsil eder.
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. Alma bir **CloudTable** başvuru istenen tablo adını temsil eden nesne. **CloudTableClient.GetTableReference** yöntemi, tablo depolama doğrulamasını yapmaz. Tablo var olup olmadığını başvurusu döndürülür. 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. Çağrı **CloudTable.CreateIfNotExists** henüz yoksa, tablo oluşturmak için yöntemi. **CloudTable.CreateIfNotExists** yöntemi döndürür **true** tablo mevcut değil ve başarıyla oluşturuldu. Aksi takdirde, **false** döndürülür.    

    ```csharp
    ViewBag.Success = table.CreateIfNotExists();
    ```

1. Güncelleştirme **ViewBag** tablonun adı.

    ```csharp
    ViewBag.TableName = table.Name;
    ```

1. İçinde **Çözüm Gezgini**, genişletin **görünümleri** klasörü sağ tıklatın **tabloları**ve bağlam menüsünden seçin **Ekle -> View**.

1. Üzerinde **Görünüm Ekle** iletişim kutusunda girin **CreateTable** görünüm adını ve seçin için **Ekle**.

1. Açık `CreateTable.cshtml`ve aşağıdaki kod parçacığı gibi görünüyor şekilde değiştirin:

    ```csharp
    @{
        ViewBag.Title = "Create Table";
    }
    
    <h2>Create Table results</h2>

    Creation of @ViewBag.TableName @(ViewBag.Success == true ? "succeeded" : "failed")
    ```

1. İçinde **Çözüm Gezgini**, genişletme **görünümleri, paylaşılan ->** klasörü ve açık `_Layout.cshtml`.

1. En son **Html.ActionLink**, aşağıdaki **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Create table", "CreateTable", "Tables")</li>
    ```

1. Uygulamayı çalıştırmak ve seçmek **Oluştur tablo** aşağıdaki ekran görüntüsüne benzer bir sonuç görmek için:
  
    ![Tablo oluşturma](./media/vs-storage-aspnet-getting-started-tables/create-table-results.png)

    Daha önce de belirtildiği **CloudTable.CreateIfNotExists** yöntemi döndürür **true** yalnızca tablo mevcut değil ve oluşturulur. Bu nedenle, tablo varsa, uygulamayı çalıştırırsanız, yöntem döndürür **false**. Uygulama birden çok kez çalıştırmak için uygulamayı yeniden çalıştırmadan önce tabloda silmeniz gerekir. Aracılığıyla tablo silme yapılabilir **CloudTable.Delete** yöntemi. Tabloyu kullanarak silebilirsiniz [Azure portalında](https://go.microsoft.com/fwlink/p/?LinkID=525040) veya [Microsoft Azure Depolama Gezgini](../vs-azure-tools-storage-manage-with-storage-explorer.md).  

## <a name="add-an-entity-to-a-table"></a>Tabloya bir varlık ekleme

*Varlıkları* C eşlemesine\# özel bir sınıf kullanarak nesneleri türetilen **TableEntity**. Tabloya bir varlık eklemek için varlığınızın özelliklerini tanımlayan bir sınıf oluşturun. Bu bölümde, bölüm anahtarı olarak soyadını ve satır anahtarı müşterinin ad kullanan bir varlık sınıfın nasıl tanımlanacağını görürsünüz. Birlikte, bir varlığın bölüm ve sıra anahtarı varlığı tabloda benzersiz şekilde tanımlar. Aynı bölüm anahtarına sahip varlıklar farklı bölüm anahtarlı varlıklara göre daha hızlı sorgulanabilir ancak farklı bölüm anahtarlarının kullanılması paralel işlemler için daha büyük ölçeklendirme sağlar. Tablo hizmetinde depolanması gereken tüm özellikler için özellik değerlerini alma hem ayarlama kullanıma sunan desteklenen türde genel bir özellik olması gerekir.
Varlık sınıfı *gerekir* genel bir parametresiz oluşturucu bildirin.

> [!NOTE]
> 
> Bu bölümdeki adımları bitirdiğinizi [geliştirme ortamını ayarlama](#set-up-the-development-environment).

1. `TablesController.cs` dosyasını açın.

1. Aşağıdaki yönerge ekleyin böylece kodda `TablesController.cs` dosya erişim **CustomerEntity** sınıfı:

    ```csharp
    using StorageAspnet.Models;
    ```

1. Adlı bir yöntem ekleyin **AddEntity** döndüren bir **actionresult öğesini**.

    ```csharp
    public ActionResult AddEntity()
    {
        // The code in this section goes here.

        return View();
    }
    ```

1. İçinde **AddEntity** yöntemi almak bir **CloudStorageAccount** depolama hesap bilgilerini temsil eden nesne. Azure hizmet yapılandırmasından bir depolama bağlantı dizesi ve depolama hesabı bilgileri almak için aşağıdaki kodu kullanın: (Değişiklik  *&lt;depolama hesabı adı >* erişmek için Azure depolama hesabı adı.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. Alma bir **CloudTableClient** nesnesi bir tablo hizmeti istemcisini temsil eder.
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. Alma bir **CloudTable** olduğu seçeceğiz yeni bir varlık eklemek için tablosuna bir başvurudur temsil eden nesne. 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. Örneği oluşturmak ve başlatmak **CustomerEntity** sınıfı.

    ```csharp
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    ```

1. Oluşturma bir **TableOperation** müşteri varlığı ekler nesne.

    ```csharp
    TableOperation insertOperation = TableOperation.Insert(customer1);
    ```

1. Ekleme işleminin çağırarak çalıştırırsınız **CloudTable.Execute** yöntemi. İnceleyerek, işlemin sonucunu doğrulayabilirsiniz **TableResult.HttpStatusCode** özelliği. İstemci tarafından istenen eylem başarıyla işlendi durum kodu 2xx gösterir. Örneğin, bir HTTP durum kodu 204, yani işlemi başarıyla işlendi ve sunucunun yeni varlıklar sonuçlarının başarılı eklemeler herhangi bir içerik döndürmedi.

    ```csharp
    TableResult result = table.Execute(insertOperation);
    ```

1. Güncelleştirme **ViewBag** tablo adını ve ekleme işleminin sonuçlarını.

    ```csharp
    ViewBag.TableName = table.Name;
    ViewBag.Result = result.HttpStatusCode;
    ```

1. İçinde **Çözüm Gezgini**, genişletin **görünümleri** klasörü sağ tıklatın **tabloları**ve bağlam menüsünden seçin **Ekle -> View**.

1. Üzerinde **Görünüm Ekle** iletişim kutusunda girin **AddEntity** görünüm adını ve seçin için **Ekle**.

1. Açık `AddEntity.cshtml`ve aşağıdaki kod parçacığı gibi görünüyor şekilde değiştirin:

    ```csharp
    @{
        ViewBag.Title = "Add entity";
    }
    
    <h2>Add entity results</h2>

    Insert of entity into @ViewBag.TableName @(ViewBag.Result == 204 ? "succeeded" : "failed")
    ```
1. İçinde **Çözüm Gezgini**, genişletme **görünümleri, paylaşılan ->** klasörü ve açık `_Layout.cshtml`.

1. En son **Html.ActionLink**, aşağıdaki **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Add entity", "AddEntity", "Tables")</li>
    ```

1. Uygulamayı çalıştırmak ve seçmek **varlık ekleme** aşağıdaki ekran görüntüsüne benzer bir sonuç görmek için:
  
    ![Varlık ekleme](./media/vs-storage-aspnet-getting-started-tables/add-entity-results.png)

    Varlık bölümündeki adımları izleyerek eklendiğini doğrulayabilirsiniz [tek bir varlık alma](#get-a-single-entity). Ayrıca [Microsoft Azure Depolama Gezgini](../vs-azure-tools-storage-manage-with-storage-explorer.md) tablolarınızı için tüm varlıkları görüntülemek için.

## <a name="add-a-batch-of-entities-to-a-table"></a>Tabloya bir varlık ekleme

İmkanına yanı sıra [teker teker tabloya bir varlık eklemek](#add-an-entity-to-a-table), varlıkları toplu işlemde de ekleyebilirsiniz. Batch'de varlıklar ekleme kodunuzu ve Azure tablo hizmeti arasındaki gidiş dönüş sayısını azaltır. Aşağıdaki adımlar, tek bir tablo için birden çok varlık Ekle işlemi ekleme göstermektedir:

> [!NOTE]
> 
> Bu bölümdeki adımları bitirdiğinizi [geliştirme ortamını ayarlama](#set-up-the-development-environment).

1. `TablesController.cs` dosyasını açın.

1. Adlı bir yöntem ekleyin **AddEntities** döndüren bir **actionresult öğesini**.

    ```csharp
    public ActionResult AddEntities()
    {
        // The code in this section goes here.

        return View();
    }
    ```

1. İçinde **AddEntities** yöntemi almak bir **CloudStorageAccount** depolama hesap bilgilerini temsil eden nesne. Azure hizmet yapılandırmasından bir depolama bağlantı dizesi ve depolama hesabı bilgileri almak için aşağıdaki kodu kullanın: (Değişiklik  *&lt;depolama hesabı adı >* erişmek için Azure depolama hesabı adı.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. Alma bir **CloudTableClient** nesnesi bir tablo hizmeti istemcisini temsil eder.
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. Alma bir **CloudTable** olduğu seçeceğiz yeni varlıklar eklemek için tablosuna bir başvurudur temsil eden nesne. 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. Bazı müşteri nesneleri göre örneği **CustomerEntity** model sınıfı bölümde sunulan [bir tabloya bir varlık ekleme](#add-an-entity-to-a-table).

    ```csharp
    CustomerEntity customer1 = new CustomerEntity("Smith", "Jeff");
    customer1.Email = "Jeff@contoso.com";

    CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
    customer2.Email = "Ben@contoso.com";
    ```

1. Alma bir **TableBatchOperation** nesne.

    ```csharp
    TableBatchOperation batchOperation = new TableBatchOperation();
    ```

1. Varlıkları toplu ekleme işlemi nesnesine ekleyin.

    ```csharp
    batchOperation.Insert(customer1);
    batchOperation.Insert(customer2);
    ```

1. Toplu ekleme işlemi çağırarak çalıştırırsınız **CloudTable.ExecuteBatch** yöntemi.   

    ```csharp
    IList<TableResult> results = table.ExecuteBatch(batchOperation);
    ```

1. **CloudTable.ExecuteBatch** yöntemi listesini döndürür **TableResult** nesneler burada her **TableResult** başarısı veya başarısızlığı belirlemek için nesne incelenebilir tek tek her işlem. Bu örnekte, listenin bir görünüme iletmek ve her işlemin sonuçları görüntüleme görünümü izin verin. 
 
    ```csharp
    return View(results);
    ```

1. İçinde **Çözüm Gezgini**, genişletin **görünümleri** klasörü sağ tıklatın **tabloları**ve bağlam menüsünden seçin **Ekle -> View**.

1. Üzerinde **Görünüm Ekle** iletişim kutusunda girin **AddEntities** görünüm adını ve seçin için **Ekle**.

1. Açık `AddEntities.cshtml`ve aşağıdakine benzer şekilde değiştirin.

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

1. İçinde **Çözüm Gezgini**, genişletme **görünümleri, paylaşılan ->** klasörü ve açık `_Layout.cshtml`.

1. En son **Html.ActionLink**, aşağıdaki **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Add entities", "AddEntities", "Tables")</li>
    ```

1. Uygulamayı çalıştırmak ve seçmek **varlık Ekle** aşağıdaki ekran görüntüsüne benzer bir sonuç görmek için:
  
    ![Varlık ekleme](./media/vs-storage-aspnet-getting-started-tables/add-entities-results.png)

    Varlık bölümündeki adımları izleyerek eklendiğini doğrulayabilirsiniz [tek bir varlık alma](#get-a-single-entity). Ayrıca [Microsoft Azure Depolama Gezgini](../vs-azure-tools-storage-manage-with-storage-explorer.md) tablolarınızı için tüm varlıkları görüntülemek için.

## <a name="get-a-single-entity"></a>Tek bir varlık alma

Bu bölümde, varlığın satır anahtarı ve bölüm anahtarı kullanarak bir tablodan tek bir varlık alma gösterir. 

> [!NOTE]
> 
> Bu bölümdeki adımları bitirdiğinizi [geliştirme ortamını ayarlama](#set-up-the-development-environment)ve verileri kullanan [tabloya bir varlık ekleme](#add-a-batch-of-entities-to-a-table). 

1. `TablesController.cs` dosyasını açın.

1. Adlı bir yöntem ekleyin **GetSingle** döndüren bir **actionresult öğesini**.

    ```csharp
    public ActionResult GetSingle()
    {
        // The code in this section goes here.

        return View();
    }
    ```

1. İçinde **GetSingle** yöntemi almak bir **CloudStorageAccount** depolama hesap bilgilerini temsil eden nesne. Azure hizmet yapılandırmasından bir depolama bağlantı dizesi ve depolama hesabı bilgileri almak için aşağıdaki kodu kullanın: (Değişiklik  *&lt;depolama hesabı adı >* erişmek için Azure depolama hesabı adı.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. Alma bir **CloudTableClient** nesnesi bir tablo hizmeti istemcisini temsil eder.
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. Alma bir **CloudTable** içinden, alınırken varlık tablosuna bir başvurudur temsil eden nesne. 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. Türetilen bir varlık nesnesini alır bir Al işlemi nesnesi oluşturma **TableEntity**. İlk parametre *partitionKey*, ve ikinci parametre *rowKey*. Kullanarak **CustomerEntity** sınıf ve bu bölümde sunulan veri [tabloya bir varlık ekleme](#add-a-batch-of-entities-to-a-table), aşağıdaki kod parçacığı için tabloyu sorgular bir **CustomerEntity**varlıkla bir *partitionKey* "Smith" değerini ve *rowKey* "Ben" değeri:

    ```csharp
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");
    ```

1. Alma işlemi yürütün.   

    ```csharp
    TableResult result = table.Execute(retrieveOperation);
    ```

1. Sonucu görüntülemek için görünüme geçirin.

    ```csharp
    return View(result);
    ```

1. İçinde **Çözüm Gezgini**, genişletin **görünümleri** klasörü sağ tıklatın **tabloları**ve bağlam menüsünden seçin **Ekle -> View**.

1. Üzerinde **Görünüm Ekle** iletişim kutusunda girin **GetSingle** görünüm adını ve seçin için **Ekle**.

1. Açık `GetSingle.cshtml`ve aşağıdaki kod parçacığı gibi görünüyor şekilde değiştirin:

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

1. İçinde **Çözüm Gezgini**, genişletme **görünümleri, paylaşılan ->** klasörü ve açık `_Layout.cshtml`.

1. En son **Html.ActionLink**, aşağıdaki **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Get single", "GetSingle", "Tables")</li>
    ```

1. Uygulamayı çalıştırmak ve seçmek **alma tek** aşağıdaki ekran görüntüsüne benzer bir sonuç görmek için:
  
    ![Tek Al](./media/vs-storage-aspnet-getting-started-tables/get-single-results.png)

## <a name="get-all-entities-in-a-partition"></a>Tüm varlıklar bir bölümde Al

Bölümünde belirtildiği gibi [bir tabloya bir varlık ekleme](#add-an-entity-to-a-table), bir bölüm ve satır anahtarı bileşimi benzersiz şekilde tanımlamak bir varlıkta bir tablo. Aynı bölüm anahtarına sahip varlıklar farklı bölüm anahtarlı varlıklara göre daha hızlı sorgulanabilir. Bu bölümde, belirtilen bir bölüm tüm varlıkların bir tabloyu sorgulamak üzere göstermektedir.  

> [!NOTE]
> 
> Bu bölümdeki adımları bitirdiğinizi [geliştirme ortamını ayarlama](#set-up-the-development-environment)ve verileri kullanan [tabloya bir varlık ekleme](#add-a-batch-of-entities-to-a-table). 

1. `TablesController.cs` dosyasını açın.

1. Adlı bir yöntem ekleyin **GetPartition** döndüren bir **actionresult öğesini**.

    ```csharp
    public ActionResult GetPartition()
    {
        // The code in this section goes here.

        return View();
    }
    ```

1. İçinde **GetPartition** yöntemi almak bir **CloudStorageAccount** depolama hesap bilgilerini temsil eden nesne. Azure hizmet yapılandırmasından bir depolama bağlantı dizesi ve depolama hesabı bilgileri almak için aşağıdaki kodu kullanın: (Değişiklik  *&lt;depolama hesabı adı >* erişmek için Azure depolama hesabı adı.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. Alma bir **CloudTableClient** nesnesi bir tablo hizmeti istemcisini temsil eder.
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. Alma bir **CloudTable** içinden, alma varlıkları tablosuna bir başvurudur temsil eden nesne. 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. Örneği bir **TableQuery** sorguda belirten nesnesini **burada** yan tümcesi. Kullanarak **CustomerEntity** sınıf ve bu bölümde sunulan veri [tabloya bir varlık ekleme](#add-a-batch-of-entities-to-a-table), aşağıdaki kod parçacığı tüm varlıklar için tabloyu sorgular nerede **PartitionKey**  (müşterinin son adı) "Smith" değerine sahip:

    ```csharp
    TableQuery<CustomerEntity> query = 
        new TableQuery<CustomerEntity>()
        .Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));
    ```

1. Bir döngü içinde çağrı **CloudTable.ExecuteQuerySegmented** önceki adımda oluşturulan sorgu nesnesi geçirerek yöntemi.  **CloudTable.ExecuteQuerySegmented** yöntemi döndürür bir **TableContinuationToken** - nesnesinin zaman **null** -almak için daha fazla varlık olduğunu gösterir. Döngü içinde başka bir döngü döndürülen varlıkları yineleme yapmak için kullanın. Aşağıdaki kod örneğinde, döndürülen her varlık, bir listesine eklenir. Görüntülenmek üzere bir görünüme sonra döngü sona erer, listenin geçirilir: 

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

1. İçinde **Çözüm Gezgini**, genişletin **görünümleri** klasörü sağ tıklatın **tabloları**ve bağlam menüsünden seçin **Ekle -> View**.

1. Üzerinde **Görünüm Ekle** iletişim kutusunda girin **GetPartition** görünüm adını ve seçin için **Ekle**.

1. Açık `GetPartition.cshtml`ve aşağıdaki kod parçacığı gibi görünüyor şekilde değiştirin:

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

1. İçinde **Çözüm Gezgini**, genişletme **görünümleri, paylaşılan ->** klasörü ve açık `_Layout.cshtml`.

1. En son **Html.ActionLink**, aşağıdaki **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Get partition", "GetPartition", "Tables")</li>
    ```

1. Uygulamayı çalıştırmak ve seçmek **alma bölüm** aşağıdaki ekran görüntüsüne benzer bir sonuç görmek için:
  
    ![Bölüm alma](./media/vs-storage-aspnet-getting-started-tables/get-partition-results.png)

## <a name="delete-an-entity"></a>Bir varlığı silme

Bu bölümde, bir tablodan bir varlığı silme gösterilmektedir.

> [!NOTE]
> 
> Bu bölümdeki adımları bitirdiğinizi [geliştirme ortamını ayarlama](#set-up-the-development-environment)ve verileri kullanan [tabloya bir varlık ekleme](#add-a-batch-of-entities-to-a-table). 

1. `TablesController.cs` dosyasını açın.

1. Adlı bir yöntem ekleyin **DeleteEntity** döndüren bir **actionresult öğesini**.

    ```csharp
    public ActionResult DeleteEntity()
    {
        // The code in this section goes here.

        return View();
    }
    ```

1. İçinde **DeleteEntity** yöntemi almak bir **CloudStorageAccount** depolama hesap bilgilerini temsil eden nesne. Azure hizmet yapılandırmasından bir depolama bağlantı dizesi ve depolama hesabı bilgileri almak için aşağıdaki kodu kullanın: (Değişiklik  *&lt;depolama hesabı adı >* erişmek için Azure depolama hesabı adı.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. Alma bir **CloudTableClient** nesnesi bir tablo hizmeti istemcisini temsil eder.
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. Alma bir **CloudTable** varlık içinden sildiğiniz tablosuna bir başvurudur temsil eden nesne. 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. Türetilen bir varlık nesnesini alır bir silme işlemi nesnesi oluşturma **TableEntity**. Bu durumda, kullandığımız **CustomerEntity** sınıf ve bu bölümde sunulan veri [tabloya bir varlık ekleme](#add-a-batch-of-entities-to-a-table). Varlığın **ETag** geçerli bir değere ayarlamanız gerekir.  

    ```csharp
    TableOperation deleteOperation = 
        TableOperation.Delete(new CustomerEntity("Smith", "Ben") { ETag = "*" } );
    ```

1. Silme işlemi yürütün.   

    ```csharp
    TableResult result = table.Execute(deleteOperation);
    ```

1. Sonucu görüntülemek için görünüme geçirin.

    ```csharp
    return View(result);
    ```

1. İçinde **Çözüm Gezgini**, genişletin **görünümleri** klasörü sağ tıklatın **tabloları**ve bağlam menüsünden seçin **Ekle -> View**.

1. Üzerinde **Görünüm Ekle** iletişim kutusunda girin **DeleteEntity** görünüm adını ve seçin için **Ekle**.

1. Açık `DeleteEntity.cshtml`ve aşağıdaki kod parçacığı gibi görünüyor şekilde değiştirin:

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

1. İçinde **Çözüm Gezgini**, genişletme **görünümleri, paylaşılan ->** klasörü ve açık `_Layout.cshtml`.

1. En son **Html.ActionLink**, aşağıdaki **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Delete entity", "DeleteEntity", "Tables")</li>
    ```

1. Uygulamayı çalıştırmak ve seçmek **varlığı Sil** aşağıdaki ekran görüntüsüne benzer bir sonuç görmek için:
  
    ![Tek Al](./media/vs-storage-aspnet-getting-started-tables/delete-entity-results.png)

## <a name="next-steps"></a>Sonraki adımlar
Azure’da veri depolama ile ilgili ek seçenekler hakkında daha fazla bilgi edinmek için daha fazla özellik kılavuzu görüntüleyin.

  * [Azure blob depolama ve Visual Studio bağlı Hizmetleri (ASP.NET) kullanmaya başlama](../storage/vs-storage-aspnet-getting-started-blobs.md)
  * [Azure kuyruk depolama ve Visual Studio bağlı Hizmetleri (ASP.NET) kullanmaya başlama](../storage/vs-storage-aspnet-getting-started-queues.md)
