---
title: Azure Cosmos DB öykünücüsü derleme göreviyle CI/CD işlem hattı oluşturma
description: Azure DevOps'ta Cosmos DB öykünücüsü derleme görevini kullanarak derleme ve yayın iş yükü ayarlama öğreticisi
author: deborahc
ms.service: cosmos-db
ms.topic: tutorial
ms.date: 11/02/2018
ms.author: dech
ms.reviewer: sngun
ms.openlocfilehash: d6250b778cdaec47ccbe2f45d35adea0b676a20a
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58882027"
---
# <a name="set-up-a-cicd-pipeline-with-the-azure-cosmos-db-emulator-build-task-in-azure-devops"></a>Azure DevOps'ta Azure Cosmos DB öykünücüsü derleme göreviyle CI/CD işlem hattı oluşturma

Azure Cosmos DB öykünücüsü, geliştirme amaçlı olarak Azure Cosmos DB hizmetine öykünen yerel bir ortam sağlar. Öykünücü sayesinde Azure aboneliği oluşturmadan veya masraf yapmadan uygulamanızı yerel ortamda geliştirip test edebilirsiniz. 

Azure DevOps için Azure Cosmos DB öykünücüsü derleme görevi, bu işlemi bir CI ortamında da gerçekleştirmenizi sağlar. Derleme göreviyle derleme ve yayın iş yüklerinizin bir parçası olarak öykünücüyle test çalıştırabilirsiniz. Bu görev öykünücünün çalıştığı bir Docker kapsayıcı başlatır ve derleme tanımının kalanı tarafından kullanılabilecek bir uç nokta sunar. İstediğiniz sayıda öykünücü örneği oluşturup başlatabilirsiniz ve oluşturduğunuz her örnek ayrı bir kapsayıcıda çalışır. 

Bu makalede Azure DevOps'ta test çalıştırmak için Cosmos DB öykünücüsü derleme görevini kullanan bir ASP.NET uygulaması için CI işlem hattı ayarlama adımları gösterilmektedir. Benzer bir yaklaşım, Node.js veya Python uygulaması için bir CI işlem hattı ayarlamak için kullanabilirsiniz. 

## <a name="install-the-emulator-build-task"></a>Öykünücü derleme görevini yükleme

Derleme görevini kullanmak için öncelikle Azure DevOps kuruluşunuza yüklemeniz gerekir. [Market](https://marketplace.visualstudio.com/items?itemName=azure-cosmosdb.emulator-public-preview) sayfasında **Azure Cosmos DB Emulator** uzantısını bulun ve **Ücretsiz edinin**'e tıklayın.

![Azure DevOps Market'te Azure Cosmos DB Emulator derleme görevini bulun ve yükleyin](./media/tutorial-setup-ci-cd/addExtension_1.png)

Ardından uzantının yükleneceği kuruluşu seçin. 

> [!NOTE]
> Azure DevOps kuruluş için bir uzantı yüklemeniz için bir hesap sahibi veya proje koleksiyonu yöneticisi olmanız gerekir. Gerekli izinlere sahip değilseniz ancak hesap üyesiyseniz uzantı isteyebilirsiniz. [Daha fazla bilgi edinin.](https://docs.microsoft.com/azure/devops/marketplace/faq-extensions?view=vsts#install-request-assign-and-access-extensions)

![Azure DevOps kuruluştaki bir uzantı yüklemek için seçin](./media/tutorial-setup-ci-cd/addExtension_2.png)

## <a name="create-a-build-definition"></a>Derleme tanımı oluşturma

Artık uzantı yüklendiğine göre Azure DevOps hesabınızda oturum açın ve projeler panosundan projenizi bulun. Projenize bir [derleme işlem hattı](https://docs.microsoft.com/azure/devops/pipelines/get-started-designer?view=vsts&tabs=new-nav) ekleyebilir veya var olan derleme işlem hattını değiştirebilirsiniz. Bir derleme işlem hattınız varsa [Derleme tanımına Öykünücü derlemesi ekleme](#addEmulatorBuildTaskToBuildDefinition) bölümüne geçebilirsiniz.

1. Yeni bir derleme tanımı oluşturmak için Azure DevOps uygulamasının **Derlemeler** sekmesine gidin. **+Yeni**'yi seçin. \> **Yeni işlem hattı oluşturma**

   ![Yeni derleme işlem hattı oluşturma](./media/tutorial-setup-ci-cd/CreateNewBuildDef_1.png)

2. İstenen **kaynak**, **Takım projesi**, **Depo** ve **El ile ve zamanlanan derlemeler için varsayılan dal** değerlerini seçin. Gerekli seçenekleri belirttikten sonra **Devam**’ı seçin

   ![Derleme işlem hattı için takım projesini, depoyu ve dalı seçme](./media/tutorial-setup-ci-cd/CreateNewBuildDef_2.png)

3. Son olarak derleme işlem hattı için kullanmak istediğiniz şablonu belirleyin. Bu öğreticide **ASP.NET** şablonunu seçeceğiz. 

Artık Azure Cosmos DB öykünücüsü derleme görevini kullanacak şekilde ayarlayabileceğiniz bir derleme işlem hattınız var. 

## <a name="addEmulatorBuildTaskToBuildDefinition"></a>Görevi derleme işlem hattına ekleme

1. Derleme işlem hattına görev eklemeden önce aracı işi eklemelisiniz. Derleme işlem hattınıza gidin **...** öğesini ve **Aracı işi ekle**’yi seçin.

1. Ardından aracı işinin yanındaki **+** sembolünü seçerek öykünücü derleme görevini ekleyin. Arama kutusundan **cosmos** araması yapın, **Azure Cosmos DB Öykünücüsünü** seçin ve aracı işine ekleyin. Derleme görevi, üzerinde Cosmos DB öykünücüsünün bir örneğinin çalıştığı bir kapsayıcı başlatır. Azure Cosmos DB Öykünücüsü görevi, öykünücünün çalışır durumda olmasını gerektiren diğer görevlerden önce yerleştirilmelidir.

   ![Öykünücü derleme görevini derleme tanımına ekleme](./media/tutorial-setup-ci-cd/addExtension_3.png)

Bu öğreticide, testlerimiz çalıştırmadan önce öykünücünün kullanılabildiğinden emin olmak için görevi en başa ekleyeceksiniz.

## <a name="configure-tests-to-use-the-emulator"></a>Testleri öykünücüyü kullanacak şekilde yapılandırma

Şimdi testlerimizi öykünücüyü kullanacak şekilde yapılandıracağız. Öykünücü derleme görevi, derleme işlem hattındaki diğer görevlerin istek düzenleyebileceği "CosmosDbEmulator.Endpoint" ortam değişkenini dışarı aktarır. 

Bu öğreticide [Visual Studio Test görevini](https://github.com/Microsoft/azure-pipelines-tasks/blob/master/Tasks/VsTestV2/README.md) kullanarak **.runsettings** dosyasıyla yapılandırılmış birim testlerini çalıştıracağız. Birim testi kurulumu hakkında daha fazla bilgi edinmek için [belgeleri](https://docs.microsoft.com/visualstudio/test/configure-unit-tests-by-using-a-dot-runsettings-file?view=vs-2017) inceleyin. Bu belgede kullandığınız tam Todo uygulaması kod örneği kullanılabilir [Github](https://github.com/Azure-Samples/documentdb-dotnet-todo-app)

Uygulamanın birim testlerine iletilecek parametreleri tanımlayan örnek **.runsettings** dosyasını aşağıda görebilirsiniz. Kullanılan `authKey` değişkeninin öykünücü için [iyi bilinen anahtar](https://docs.microsoft.com/azure/cosmos-db/local-emulator#authenticating-requests) olduğuna dikkat edin. Bu `authKey`, öykünücü derleme görevi tarafından beklenen anahtardır ve **.runsettings** dosyanızda tanımlanmalıdır.

```csharp
<RunSettings>
    <TestRunParameters>
    <Parameter name="endpoint" value="https://localhost:8081" />
    <Parameter name="authKey" value="C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==" />
    <Parameter name="database" value="ToDoListTest" />
    <Parameter name="collection" value="ItemsTest" />
  </TestRunParameters>
</RunSettings>
```

MongoDB için Azure Cosmos DB'nin API'sini kullanan bir uygulama için bir CI/CD işlem hattı ayarlıyorsanız, varsayılan olarak bağlantı dizesini 10255 olarak bağlantı noktası numarasını içerir. Ancak, bu bağlantı noktası açık değil alternatif olarak, bağlantı kurmak için bağlantı noktası 10250 kullanmanız gerekir. MongoDB bağlantı dizesi için Azure Cosmos DB API, desteklenen bağlantı noktası numarası yerine 10255 olarak 10250 haricinde aynı kalır.

`TestRunParameters` parametrelerine uygulamanın test projesindeki bir `TestContext` özelliği aracılığıyla başvurulur. Burada Cosmos DB ile çalışan örnek bir testi görebilirsiniz.

```csharp
namespace todo.Tests
{
    [TestClass]
    public class TodoUnitTests
    {
        public TestContext TestContext { get; set; }

        [TestInitialize()]
        public void Initialize()
        {
            string endpoint = TestContext.Properties["endpoint"].ToString();
            string authKey = TestContext.Properties["authKey"].ToString();
            Console.WriteLine("Using endpoint: ", endpoint);
            DocumentDBRepository<Item>.Initialize(endpoint, authKey);
        }
        [TestMethod]
        public async Task TestCreateItemsAsync()
        {
            var item = new Item
            {
                Id = "1",
                Name = "testName",
                Description = "testDescription",
                Completed = false,
                Category = "testCategory"
            };

            // Create the item
            await DocumentDBRepository<Item>.CreateItemAsync(item);
            // Query for the item
            var returnedItem = await DocumentDBRepository<Item>.GetItemAsync(item.Id, item.Category);
            // Verify the item returned is correct.
            Assert.AreEqual(item.Id, returnedItem.Id);
            Assert.AreEqual(item.Category, returnedItem.Category);
        }

        [TestCleanup()]
        public void Cleanup()
        {
            DocumentDBRepository<Item>.Teardown();
        }
    }
}
```

Visual Studio Test görevindeki Execution Options (Yürütme Seçenekleri) bölümüne gidin. **Settings file** (Ayarlar dosyası) seçeneğinde testlerin **.runsettings** dosyasıyla yapılandırıldığını belirtin. **Override test run parameters** (Test çalıştırması parametrelerini geçersiz kıl) seçeneğine `-endpoint $(CosmosDbEmulator.Endpoint)` girişini ekleyin. Bunu yaptığınızda Test görevi **.runsettings** dosyasında tanımlanan uç noktanın yerine öykünücü derleme görevinin uç noktasına başvurur.  

![Uç noktası değişkenini öykünücü derleme görevi uç noktasıyla geçersiz kılma](./media/tutorial-setup-ci-cd/addExtension_5.png)

## <a name="run-the-build"></a>Derlemeyi çalıştırma

Şimdi derlemeyi **kaydedip kuyruğa alabilirsiniz**. 

![Derlemeyi kaydetme ve çalıştırma](./media/tutorial-setup-ci-cd/runBuild_1.png)

Derleme başlatıldıktan sonra Cosmos DB öykünücüsü görevinin öykünücünün yüklü olduğu Docker görüntüsünü çekmeye başladığından emin olun. 

![Derlemeyi kaydetme ve çalıştırma](./media/tutorial-setup-ci-cd/runBuild_4.png)

Derleme tamamlandıktan sonra testlerinizin iletildiğinden ve tümünün derleme görevindeki Cosmos DB öykünücüsüyle çalıştığından emin olun!

![Derlemeyi kaydetme ve çalıştırma](./media/tutorial-setup-ci-cd/buildComplete_1.png)

## <a name="next-steps"></a>Sonraki adımlar

Yerel geliştirme ve test için öykünücü kullanımı hakkında daha fazla bilgi edinmek için bkz. [Yerel geliştirme ve test için Azure Cosmos DB Öykünücüsünü kullanma](https://docs.microsoft.com/azure/cosmos-db/local-emulator).

Öykünücü SSL sertifikalarını dışarı aktarmak için bkz. [Azure Cosmos DB Öykünücü sertifikalarını Java, Python ve Node.js ile kullanmak için dışarı aktarma](https://docs.microsoft.com/azure/cosmos-db/local-emulator-export-ssl-certificates)
