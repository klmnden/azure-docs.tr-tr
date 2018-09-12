---
title: Azure Cosmos DB öykünücüsü derleme göreviyle CI/CD işlem hattı oluşturma
description: Visual Studio Team Services (VSTS) uygulamasında Cosmos DB öykünücüsü derleme görevini kullanarak derleme ve yayın iş yükü ayarlama öğreticisi
services: cosmos-db
keywords: Azure Cosmos DB Öykünücüsü
author: deborahc
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: tutorial
ms.date: 8/28/2018
ms.author: dech
ms.openlocfilehash: 37bb43435c34f14145b3642aa12c5cb0f16d780c
ms.sourcegitcommit: e2348a7a40dc352677ae0d7e4096540b47704374
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/05/2018
ms.locfileid: "43783883"
---
# <a name="set-up-a-cicd-pipeline-with-the-azure-cosmos-db-emulator-build-task-in-visual-studio-team-services"></a>Visual Studio Team Services uygulamasında Azure Cosmos DB öykünücüsü derleme göreviyle CI/CD işlem hattı oluşturma

Azure Cosmos DB öykünücüsü, geliştirme amaçlı olarak Azure Cosmos DB hizmetine öykünen yerel bir ortam sağlar. Öykünücü sayesinde Azure aboneliği oluşturmadan veya masraf yapmadan uygulamanızı yerel ortamda geliştirip test edebilirsiniz. 

Visual Studio Team Services (VSTS) için Azure Cosmos DB öykünücüsü derleme görevi, bu işlemi bir CI ortamında da gerçekleştirmenizi sağlar. Derleme göreviyle derleme ve yayın iş yüklerinizin bir parçası olarak öykünücüyle test çalıştırabilirsiniz. Bu görev öykünücünün çalıştığı bir Docker kapsayıcı başlatır ve derleme tanımının kalanı tarafından kullanılabilecek bir uç nokta sunar. İstediğiniz sayıda öykünücü örneği oluşturup başlatabilirsiniz ve oluşturduğunuz her örnek ayrı bir kapsayıcıda çalışır. 

Bu makalede VSTS uygulamasında test çalıştırmak için Cosmos DB öykünücüsü derleme görevini kullanan bir ASP.NET uygulaması için CI işlem hattı ayarlama adımları gösterilmektedir. 

## <a name="install-the-emulator-build-task"></a>Öykünücü derleme görevini yükleme

Derleme görevini kullanmak için öncelikle VSTS kuruluşunuza yüklemeniz gerekir. [Market](https://marketplace.visualstudio.com/items?itemName=azure-cosmosdb.emulator-public-preview) sayfasında **Azure Cosmos DB Emulator** uzantısını bulun ve **Ücretsiz edinin**'e tıklayın.

![VSTS Market'te Azure Cosmos DB Emulator derleme görevini bulun ve yükleyin](./media/tutorial-setup-ci-cd/addExtension_1.png)

Ardından uzantının yükleneceği kuruluşu seçin. 

> [!NOTE]
> Bir VSTS kuruluşuna uzantı yükleyebilmek için hesap sahibi veya proje koleksiyonu yöneticisi olmanız gerekir. Gerekli izinlere sahip değilseniz ancak hesap üyesiyseniz uzantı isteyebilirsiniz. [Daha fazla bilgi edinin.](https://docs.microsoft.com/vsts/marketplace/faq-extensions?view=vsts#install-request-assign-and-access-extensions) 

![Uzantının yükleneceği VSTS kuruluşunu seçme](./media/tutorial-setup-ci-cd/addExtension_2.png)

## <a name="create-a-build-definition"></a>Derleme tanımı oluşturma

Uzantıyı yüklediniz, şimdi bunu bir [derleme tanımına](https://docs.microsoft.com/vsts/pipelines/get-started-designer?view=vsts&tabs=new-nav) eklemeniz gerekiyor. Var olan derleme tanımlarından birini değiştirebilir veya yenisini oluşturabilirsiniz. Bir derleme tanımınız varsa [Öykünücü derleme görevini derleme tanımına ekleme](#addEmulatorBuildTaskToBuildDefinition) bölümüne geçebilirsiniz.

Yeni bir derleme tanımı oluşturmak için VSTS uygulamasının **Derleme ve Yayın** sekmesine gidin. **+Yeni**'yi seçin.

![Yeni bir derleme tanımı oluşturma](./media/tutorial-setup-ci-cd/CreateNewBuildDef_1.png) Derlemeyi etkinleştirmek için kullanmak istediğiniz takım projesini, depoyu ve dalı seçin. 

![Derleme tanımı için takım projesini, depoyu ve dalı seçme ](./media/tutorial-setup-ci-cd/CreateNewBuildDef_2.png)

Son olarak derleme tanımı için kullanmak istediğiniz şablonu belirleyin. Bu öğreticide **ASP.NET** şablonunu seçeceğiz. 

![Kullanmak istediğiniz derleme tanımı şablonunu seçin ](./media/tutorial-setup-ci-cd/CreateNewBuildDef_3.png)

Artık Azure Cosmos DB öykünücüsü derleme görevini kullanacak şekilde ayarlayabileceğiniz aşağıdakine benzer bir derleme tanımınız var. 

![ASP.NET derleme tanımı şablonu](./media/tutorial-setup-ci-cd/CreateNewBuildDef_4.png)

## <a name="addEmulatorBuildTaskToBuildDefinition"></a>Görevi derleme tanımına ekleme

Öykünücü derleme görevini eklemek için arama kutusuna **cosmos** yazın ve **Ekle**'yi seçin. Derleme görevi Cosmos DB öykünücüsü örneğinin çalıştığı bir kapsayıcı başlatır. Bu nedenle öykünücünün çalışır durumda olmasını bekleyen diğer görevlerden önce bu görevin yerleştirilmesi gerekir.

![Öykünücü derleme görevini derleme tanımına ekleme](./media/tutorial-setup-ci-cd/addExtension_3.png) Bu öğreticide öykünücünün testler yürütülmeden önce kullanılabilir durumda olmasını sağlamak için görevi 1. Aşamanın başına ekleyeceğiz.
Tamamlanmış derleme tanımı bu örneğe benzer olacaktır. 

![ASP.NET derleme tanımı şablonu](./media/tutorial-setup-ci-cd/CreateNewBuildDef_5.png)

## <a name="configure-tests-to-use-the-emulator"></a>Testleri öykünücüyü kullanacak şekilde yapılandırma
Şimdi testlerimizi öykünücüyü kullanacak şekilde yapılandıracağız. Öykünücü derleme görevi, derleme işlem hattındaki diğer görevlerin istek düzenleyebileceği "CosmosDbEmulator.Endpoint" ortam değişkenini dışarı aktarır. 

Bu öğreticide [Visual Studio Test görevini](https://github.com/Microsoft/vsts-tasks/blob/master/Tasks/VsTestV2/README.md) kullanarak **.runsettings** dosyasıyla yapılandırılmış birim testlerini çalıştıracağız. Birim testi kurulumu hakkında daha fazla bilgi edinmek için [belgeleri](https://docs.microsoft.com/visualstudio/test/configure-unit-tests-by-using-a-dot-runsettings-file?view=vs-2017) inceleyin.

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

Visual Studio Test görevindeki Execution Options (Yürütme Seçenekleri) bölümüne gidin. **Settings file** (Ayarlar dosyası) seçeneğinde testlerin **.runsettings** dosyasıyla yapılandırıldığını belirtin. **Override test run parameters** (Test çalıştırması parametrelerini geçersiz kıl) seçeneğine ` -endpoint $(CosmosDbEmulator.Endpoint)` girişini ekleyin. Bunu yaptığınızda Test görevi **.runsettings** dosyasında tanımlanan uç noktanın yerine öykünücü derleme görevinin uç noktasına başvurur.  

![Uç noktası değişkenini öykünücü derleme görevi uç noktasıyla geçersiz kılma](./media/tutorial-setup-ci-cd/addExtension_5.png)

## <a name="run-the-build"></a>Derlemeyi çalıştırma
Şimdi derlemeyi kaydedip kuyruğa alabilirsiniz. 

![Derlemeyi kaydetme ve çalıştırma](./media/tutorial-setup-ci-cd/runBuild_1.png)

Derleme başlatıldıktan sonra Cosmos DB öykünücüsü görevinin öykünücünün yüklü olduğu Docker görüntüsünü çekmeye başladığından emin olun. 

![Derlemeyi kaydetme ve çalıştırma](./media/tutorial-setup-ci-cd/runBuild_4.png)

Derleme tamamlandıktan sonra testlerinizin iletildiğinden ve tümünün derleme görevindeki Cosmos DB öykünücüsüyle çalıştığından emin olun!

![Derlemeyi kaydetme ve çalıştırma](./media/tutorial-setup-ci-cd/buildComplete_1.png)

## <a name="next-steps"></a>Sonraki adımlar

Yerel geliştirme ve test için öykünücü kullanımı hakkında daha fazla bilgi edinmek için bkz. [Yerel geliştirme ve test için Azure Cosmos DB Öykünücüsünü kullanma](https://docs.microsoft.com/azure/cosmos-db/local-emulator).

Öykünücü SSL sertifikalarını dışarı aktarmak için bkz. [Azure Cosmos DB Öykünücü sertifikalarını Java, Python ve Node.js ile kullanmak için dışarı aktarma](https://docs.microsoft.com/azure/cosmos-db/local-emulator-export-ssl-certificates)
