---
title: Öğretici deposu veri SQL modülü - Azure IOT Edge ile | Microsoft Docs
description: Bir SQL Server modülüyle IoT Edge cihazınızda yerel veri depolamayı öğrenin
services: iot-edge
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 03/28/2019
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc, seodec18
ms.openlocfilehash: a83b8a56a8108f86d868e3420d8368c74fba308a
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2019
ms.locfileid: "58578207"
---
# <a name="tutorial-store-data-at-the-edge-with-sql-server-databases"></a>Öğretici: SQL Server veritabanları ile uçta veri Store

Uç cihazlarda veri depolamak ve sorgulamak için Azure IoT Edge ve SQL Server işlevlerini kullanın. Azure IOT Edge cihaz çevrimdışı olduğunda iletileri önbelleğe alınması ve bağlantı yeniden kurulduğunda bunları iletmek için temel depolama özellikleri vardır. Ancak sorgu verilerini yerel ortamda sorgulayabilme gibi daha gelişmiş depolama özelliklerine ihtiyaç duyabilirsiniz. IOT Edge cihazlarınıza yerel veritabanlarını, IOT hub'ı bir bağlantı sürdürmenizi gerek kalmadan daha karmaşık bilgi işlem gerçekleştirmek için kullanabilirsiniz. 

Bu makalede bir IoT Edge cihazına SQL Server veritabanı dağıtma yönergeleri yer almaktadır. IoT Edge cihazında çalışan Azure İşlevleri, gelen verileri yapılandırıp veritabanına gönderir. Bu makaledeki adımlar MySQL veya PostgreSQL gibi kapsayıcılarda çalışan diğer veritabanlarına da uygulanabilir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz: 

> [!div class="checklist"]
> * Visual Studio Code kullanarak Azure İşlevi oluşturma
> * IoT Edge cihazınıza bir SQL veritabanı dağıtma
> * Visual Studio Code uygulamasını kullanarak modülleri derleme ve IoT Edge cihazınıza dağıtma
> * Oluşturulan verileri görüntüleme

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Bir Azure IoT Edge cihazı:

* Bir Azure sanal makinesi için hızlı başlangıç adımları izleyerek bir IOT Edge cihazı kullanabilirsiniz [Linux](quickstart-linux.md).
* SQL Server, yalnızca Linux kapsayıcıları destekler. Bu öğretici, IOT Edge cihazınız olarak bir Windows cihazı kullanarak test etmek isterseniz, Linux kapsayıcıları kullanmasını sağlayacak şekilde yapılandırmalısınız. Bkz: [Windows yükleme Azure IOT Edge çalışma zamanına](how-to-install-iot-edge-windows.md) Önkoşullar ve yükleme adımları Windows üzerinde Linux kapsayıcıları için IOT Edge çalışma zamanı yapılandırma.

Bulut kaynakları:

* Azure'da ücretsiz veya standart katman [IoT Hub'ı](../iot-hub/iot-hub-create-through-portal.md). 

Geliştirme kaynakları:

* [Visual Studio Code](https://code.visualstudio.com/). 
* [C#Visual Studio Code (OmniSharp tarafından desteklenen) uzantısı için Visual Studio Code için](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp). 
* [Visual Studio Code için Azure IOT Araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge). 
* [.NET Core 2.1 SDK'sı](https://www.microsoft.com/net/download). 
* [Docker CE](https://docs.docker.com/install/). 
  * Bir Windows makinesinde geliştiriyorsanız, Docker olduğundan emin olun [Linux kapsayıcıları kullanacak şekilde yapılandırılmış](https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers). 

## <a name="create-a-container-registry"></a>Kapsayıcı kayıt defteri oluşturma

Bu öğreticide, bir modül olarak derlemek ve oluşturmak için Visual Studio Code için Azure IOT araçları kullanmak bir **kapsayıcı görüntüsü** dosyalarından. Ardından bu görüntüyü, görüntülerinizin depolandığı ve yönetildiği **kayıt defterine** gönderirsiniz. Son olarak, görüntünüzü IoT Edge cihazınızda çalıştırmak üzere kayıt defterinizden dağıtırsınız.  

Herhangi bir Docker ile uyumlu kayıt defteri, kapsayıcı görüntülerinizi tutmak için kullanabilirsiniz. İki popüler Docker kayıt defteri Hizmetleri [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) ve [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). Bu öğreticide Azure Container Registry kullanılır. 

Kapsayıcı kayıt defteri zaten yoksa, azure'da yeni bir tane oluşturmak için aşağıdaki adımları izleyin:

1. [Azure portalında](https://portal.azure.com), **Kaynak oluştur** > **Kapsayıcılar** > **Container Registry**'yi seçin.

2. Kapsayıcı kayıt defterinizi oluşturmak için aşağıdaki değerleri girin:

   | Alan | Değer | 
   | ----- | ----- |
   | Kayıt defteri adı | Benzersiz bir ad girin. |
   | Abonelik | Açılan listeden bir abonelik seçin. |
   | Kaynak grubu | IoT Edge hızlı başlangıçlarında ve öğreticilerinde oluşturduğunuz tüm test kaynakları için aynı kaynak grubunu kullanmanızı öneririz. Örneğin, **IoTEdgeResources**. |
   | Konum | Size yakın bir konum seçin. |
   | Yönetici kullanıcı | **Etkinleştir**'i seçin. |
   | SKU | **Temel**'i seçin. | 

5. **Oluştur**’u seçin.

6. Kapsayıcı kayıt defteriniz oluşturulduktan sonra, bu kayıt defterine gidin ve **Erişim anahtarları**'nı seçin. 

7. **Oturum açma sunucusu**, **Kullanıcı adı** ve **Parola** değerlerini kopyalayın. Kapsayıcı kayıt defterine erişim sağlamak için öğreticinin ilerleyen bölümlerinde bu değerleri kullanırsınız.  

## <a name="create-a-function-project"></a>İşlev projesi oluşturma

Bir veritabanına veri göndermek için verileri doğru şekilde yapılandırıp tabloda kaydeden bir modüle ihtiyacınız vardır. 

Aşağıdaki adımlar Visual Studio Code ve Azure IOT araçları kullanarak bir IOT Edge işlevinin nasıl oluşturulacağı gösterir.

1. Visual Studio Code'u açın.

2. **View (Görünüm)** > **Command palette (Komut paleti)** öğesini seçerek VS Code komut paletini açın.

3. Yazın ve şu komutu çalıştırın komut Paleti'nde **Azure IOT Edge: Yeni bir IOT Edge çözüm**. Komut paletinde çözümünüzü oluşturmak için aşağıdaki bilgileri girin: 

   | Alan | Değer |
   | ----- | ----- |
   | Klasör seçin | Geliştirme makinenizde VS Code'un çözüm dosyalarını oluşturmak için kullanacağı konumu seçin. |
   | Çözüm adı sağlayın | Gibi çözümünüz için açıklayıcı bir ad girin **SqlSolution**, veya varsayılan değeri kabul edin. |
   | Modül şablonunu seçin | Seçin **Azure işlevleri - C#** . |
   | Modül adı sağlayın | Modülünüze **sqlFunction** adını verin. |
   | Modül için Docker görüntü deposunu sağlama | Görüntü deposu, kapsayıcı kayıt defterinizin adını ve kapsayıcı görüntünüzün adını içerir. Kapsayıcı görüntünüz bir önceki adımdaki değerle önceden doldurulur. **localhost:5000** yerine Azure kapsayıcı kayıt defterinizden alacağınız oturum açma sunucusu değerini yazın. Oturum açma sunucusunu Azure portalda kapsayıcı kayıt defterinizin Genel bakış sayfasından alabilirsiniz. <br><br>Son dize şuna benzer \<kayıt defteri adı\>.azurecr.io/sqlFunction. |

   VS Code penceresi IoT Edge çözümü çalışma alanınızı yükler. 
   
4. IOT Edge çözümünüzü açın \.env dosyası. 

   Yeni bir IOT Edge çözüm oluşturduğunuzda, VS Code kayıt defteri kimlik bilgilerinizi girmenizi ister \.env dosyası. Bu dosya, git yoksayıldı ve IOT Edge uzantısını daha sonra IOT Edge Cihazınızı kayıt defteri erişim sağlamak için kullanır. 

   Önceki adımda kapsayıcı kayıt defterinizin sağlamadı, ancak varsayılan localhost:5000 kabul olmaz bir \.env dosyası.

5. .env dosyasında modül görüntülerinize erişebilmesi için IoT Edge çalışma zamanına kayıt defteri kimlik bilgilerinizi girin. **CONTAINER_REGISTRY_USERNAME** ve **CONTAINER_REGISTRY_PASSWORD** bölümlerini bulun ve eşittir işaretinden sonra kimlik bilgilerinizi ekleyin: 

   ```env
   CONTAINER_REGISTRY_USERNAME_yourregistry=<username>
   CONTAINER_REGISTRY_PASSWORD_yourregistry=<password>
   ```

6. .env dosyasını kaydedin.

7. VS Code Gezgininde açın **modülleri** > **sqlFunction** > **sqlFunction.cs**.

8. Dosyanın tüm içeriğini aşağıdaki kodla değiştirin:

   ```csharp
   using System;
   using System.Collections.Generic;
   using System.IO;
   using System.Text;
   using System.Threading.Tasks;
   using Microsoft.Azure.Devices.Client;
   using Microsoft.Azure.WebJobs;
   using Microsoft.Azure.WebJobs.Extensions.EdgeHub;
   using Microsoft.Azure.WebJobs.Host;
   using Microsoft.Extensions.Logging;
   using Newtonsoft.Json;
   using Sql = System.Data.SqlClient;

   namespace Functions.Samples
   {
       public static class sqlFunction
       {
           [FunctionName("sqlFunction")]
           public static async Task FilterMessageAndSendMessage(
               [EdgeHubTrigger("input1")] Message messageReceived,
               [EdgeHub(OutputName = "output1")] IAsyncCollector<Message> output,
               ILogger logger)
           {
               const int temperatureThreshold = 20;
               byte[] messageBytes = messageReceived.GetBytes();
               var messageString = System.Text.Encoding.UTF8.GetString(messageBytes);

               if (!string.IsNullOrEmpty(messageString))
               {
                   logger.LogInformation("Info: Received one non-empty message");
                   // Get the body of the message and deserialize it.
                   var messageBody = JsonConvert.DeserializeObject<MessageBody>(messageString);

                   //Store the data in SQL db
                   const string str = "<sql connection string>";
                   using (Sql.SqlConnection conn = new Sql.SqlConnection(str))
                   {
                       conn.Open();
                       var insertMachineTemperature = "INSERT INTO MeasurementsDB.dbo.TemperatureMeasurements VALUES (CONVERT(DATETIME2,'" + messageBody.timeCreated + "', 127), 'machine', " + messageBody.machine.temperature + ");";
                       var insertAmbientTemperature = "INSERT INTO MeasurementsDB.dbo.TemperatureMeasurements VALUES (CONVERT(DATETIME2,'" + messageBody.timeCreated + "', 127), 'ambient', " + messageBody.ambient.temperature + ");"; 
                       using (Sql.SqlCommand cmd = new Sql.SqlCommand(insertMachineTemperature + "\n" + insertAmbientTemperature, conn))
                       {
                           //Execute the command and log the # rows affected.
                           var rows = await cmd.ExecuteNonQueryAsync();
                           logger.LogInformation($"{rows} rows were updated");
                       }
                   }

                   if (messageBody != null && messageBody.machine.temperature > temperatureThreshold)
                   {
                       // Send the message to the output as the temperature value is greater than the threashold.
                       var filteredMessage = new Message(messageBytes);
                       // Copy the properties of the original message into the new Message object.
                       foreach (KeyValuePair<string, string> prop in messageReceived.Properties)
                       {filteredMessage.Properties.Add(prop.Key, prop.Value);}
                       // Add a new property to the message to indicate it is an alert.
                       filteredMessage.Properties.Add("MessageType", "Alert");
                       // Send the message.       
                       await output.AddAsync(filteredMessage);
                       logger.LogInformation("Info: Received and transferred a message with temperature above the threshold");
                   }
               }
           }
       }
       //Define the expected schema for the body of incoming messages.
       class MessageBody
       {
           public Machine machine {get; set;}
           public Ambient ambient {get; set;}
           public string timeCreated {get; set;}
       }
       class Machine
       {
           public double temperature {get; set;}
           public double pressure {get; set;}         
       }
       class Ambient
       {
           public double temperature {get; set;}
           public int humidity {get; set;}         
       }
   }
   ```

6. Dize 35 satırında değiştirin **\<sql bağlantı dizesi\>** aşağıdaki dizeyi. **Veri kaynağı** özelliği henüz mevcut olmayan SQL Server kapsayıcı başvuruyor ancak adıyla oluşturacağınız **SQL** sonraki bölümde. 

   ```csharp
   Data Source=tcp:sql,1433;Initial Catalog=MeasurementsDB;User Id=SA;Password=Strong!Passw0rd;TrustServerCertificate=False;Connection Timeout=30;
   ```

7. Kaydet **sqlFunction.cs** dosya. 

8. Açık **sqlFunction.csproj** dosya.

9. Paket başvuruları grubunu bulun ve SqlClient dahil etmek için yeni bir tane ekleyin. 

   ```csproj
   <PackageReference Include="System.Data.SqlClient" Version="4.5.1"/>
   ```

10. Kaydet **sqlFunction.csproj** dosya.

## <a name="add-the-sql-server-container"></a>SQL Server kapsayıcı ekleme

[Dağıtım bildirimi](module-composition.md), IoT Edge çalışma zamanının hangi modüllerinin IoT Edge cihazınıza yükleneceğini bildirir. Önceki bölümde özelleştirilmiş bir işlev modülü için gereken kodu sağlanan, ancak SQL Server modülü zaten oluşturulmuş ve Azure Marketi'nde kullanılabilir. Tek yapmanız gereken IoT Edge çalışma zamanına bunu dahil etmek ve cihazınızda yapılandırmaktır. 

1. Visual Studio Code'da seçerek komut paletini açın **görünümü** > **komut paleti**.

2. Yazın ve şu komutu çalıştırın komut Paleti'nde **Azure IOT Edge: IOT Edge Modülü Ekle**. Komut paletini içinde yeni bir modül eklemek için aşağıdaki bilgileri sağlayın: 

   | Alan | Değer | 
   | ----- | ----- |
   | Dağıtım şablonu dosyasını seçin | Komut paletini deployment.template.json dosyanın geçerli çözüm klasörünüzde vurgular. Bu dosyayı seçin.  |
   | Modül şablonunu seçin | Seçin **modülü Azure Market'ten**. |

3. Azure IOT Edge modülü Market'te arayın ve seçin **SQL Server Modülü**. 

4. Modül adı için değiştirme **sql**, tamamen küçük harflerden. Bu ad, kapsayıcı adı bağlantı dizesinde sqlFunction.cs dosyasında bildirilen eşleşir. 

5. Seçin **alma** modülü çözümünüze eklemek için. 

6. Çözüm klasörünüzü açın **deployment.template.json** dosya. 

7. Bulma **modülleri** bölümü. Üç modül görmeniz gerekir. Modül *tempSensor* varsayılan olarak yeni çözümler dahil edilmiştir ve diğer modüller ile kullanmak için test verilerini sağlar. Modül *sqlFunction* başlangıçta oluşturduğunuz ve yeni kod iler güncelleştirilmiş modülüdür. Son olarak, modül *sql* Azure Marketi'nden içeri aktarıldı. 

   >[!Tip]
   >SQL Server modülü, varsayılan parola dağıtım bildiriminin ortam değişkenleri kümesi ile birlikte gelir. Üretim ortamında bir SQL Server kapsayıcısı oluşturduğunuzda [varsayılan sistem yöneticisi parolasını değiştirmeniz gerekir](https://docs.microsoft.com/sql/linux/quickstart-install-connect-docker).

8. Kapat **deployment.template.json** dosya.

## <a name="build-your-iot-edge-solution"></a>IoT Edge çözümünüzü derleyin

Önceki bölümlerde tek modüle sahip bir çözüm oluşturdunuz ve dağıtım bildirimi şablonuna başka bir modül daha eklediniz. SQL Server modülü genel olarak Microsoft tarafından barındırılan ancak işlevler modüldeki kodun kapsayıcılı hale getirme gerekir. Bu bölümde, çözümü derleyin, sqlFunction modülü için kapsayıcı görüntüleri oluşturma ve görüntüyü kapsayıcı kayıt defterinize gönderin. 

1. Visual Studio Code'da **Görünüm** > **Terminal**'i seçerek tümleşik terminali açın.  

1. Görüntüleri kayıt defterinize gönderebilmeniz için kapsayıcı kayıt defterinizi Visual Studio Code'a ekleyin. .Env dosyasına eklediğiniz Azure Container Registry (ACR) kimlik bilgilerini kullanın. Tümleşik terminale aşağıdaki komutu girin:

    ```csh/sh
    docker login -u <ACR username> -p <ACR password> <ACR login server>
    ```
    
    Kullanımını öneren bir güvenlik uyarısı görebilirsiniz--parola stdin parametre. Bunun kullanımı bu makalenin kapsamında olmasa da bu en iyi yöntemin izlenmesi önerilir. Daha fazla bilgi için [docker oturum açma](https://docs.docker.com/engine/reference/commandline/login/#provide-a-password-using-stdin) komut başvurusu. 

2. VS Code gezgininde **deployment.template.json** dosyasına sağ tıklayıp **Build and Push IoT Edge solution** (IoT Edge Çözümü Oluştur ve Gönder) öğesini seçin. 

Visual Studio Code uygulamasına çözümünüzü derleme komutu verdiğinizde dağıtım şablonundaki bilgileri alır ve **config** adlı yeni bir klasörde deployment.json dosyası oluşturur. Ardından tümleşik terminalde `docker build` ve `docker push` komutlarını çalıştırır. Bu iki komut, kodunuzu derlemek, modül kapsayıcılı hale getirme ve ardından kod çözümü hazırlarken belirttiğiniz kapsayıcı kayıt defterine gönderin. 

SqlFunction modülü, kapsayıcı kayıt defterinizde başarıyla gönderildi doğrulayabilirsiniz. Azure portalında kapsayıcı kayıt defterinize gidin. Seçin **depoları** araması **sqlFunction**. Zaten Microsoft kayıtlarla kendi depolarına işaret çünkü diğer iki modülleri tempSensor ve sql, kapsayıcı kayıt defterinizde itilecek olmaz.

## <a name="deploy-the-solution-to-a-device"></a>Çözümü bir cihaza dağıtma

IoT Hub üzerinden bir cihazda modül ayarlayabilirsiniz ancak IoT Hub ve cihazlara Visual Studio Code aracılığıyla da erişebilirsiniz. Bu bölümde IoT Hub'ınıza erişim ayarlarını yapacak ve ardından çözümünüzü IoT Edge cihazınıza dağıtmak için VS Code uygulamasını kullanacaksınız. 

1. VS Code komut paletinde seçin **Azure IOT Hub: IOT hub'ını seçin**.

2. Azure hesabınızda oturum açmak için yönergeleri izleyin. 

3. Komut paletinde Azure aboneliğinizi ve ardından IoT Hub'ınızı seçin. 

4. VS Code gezgininde **Azure IoT Hub Devices** (Azure IoT Hub Cihazları) bölümünü seçin. 

5. Dağıtımınızla hedeflemek istediğiniz cihaza sağ tıklayıp **Create deployment for single device** (Tek cihaz için dağıtım oluştur) öğesini seçin. 

   ![Tek bir cihaz için dağıtım oluşturma](./media/tutorial-store-data-sql-server/create-deployment.png)

6. Dosya Gezgini'nde gidin **config** klasörün içinde çözümünüzün ve **deployment.amd64**. **Select Edge deployment manifest** (Edge dağıtım bildirimini seç) öğesine tıklayın. 

   Deployment.template.json dosyasını bir dağıtım bildirimi kullanmayın.

Dağıtım başarılı olursa VS Code çıkışında onay iletisi yazdırılır. 

Cihazınızı Azure IOT Hub cihazları bölümünde VS Code durumunu yenileyin. Yeni modüller listelenir ve birkaç dakika içinde çalışan kapsayıcıları yüklü ve başlatılmış olarak rapor başlar. Tüm modüllerin cihazınızda çalışıp çalışmadığını da kontrol edebilirsiniz. IoT Edge cihazında modüllerin durumunu görmek için aşağıdaki komutu çalıştırın. 

   ```cmd/sh
   iotedge list
   ```

## <a name="create-the-sql-database"></a>SQL veritabanını oluşturma

Dağıtım bildirimini cihazınıza uyguladığınızda üç modül çalışmaya başlar. tempSensor modülü ortam verisi simülasyonunu oluşturur. sqlFunction modülü verileri alır ve veritabanı için biçimlendirir. Bu bölüm, sıcaklık verilerini depolamak için kullanılacak SQL veritabanını ayarlama sürecinde size yardımcı olacaktır. 

IOT Edge Cihazınızda aşağıdaki komutları çalıştırın. Bu komutlar bağlanma **sql** modülü, cihaz üzerinde çalışan ve bir veritabanı ve kendisine gönderilen sıcaklık verileri tutmak için tablo oluşturun. 

1. IOT Edge Cihazınızda bir komut satırı aracı veritabanınıza bağlanın. 
      ```bash
      sudo docker exec -it sql bash
      ```

2. SQL komut aracını açın.
      ```bash
      /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P 'Strong!Passw0rd'
      ```

3. Veritabanınızı oluşturun: 
      ```sql
      CREATE DATABASE MeasurementsDB
      ON
      (NAME = MeasurementsDB, FILENAME = '/var/opt/mssql/measurementsdb.mdf')
      GO
      ```

4. Tablonuzu tanımlayın.

   ```sql
   CREATE TABLE MeasurementsDB.dbo.TemperatureMeasurements (measurementTime DATETIME2, location NVARCHAR(50), temperature FLOAT)
   GO
   ```

SQL Server Docker dosyanızı özelleştirerek SQL Server örneğinizin birden fazla IoT Edge cihazına otomatik olarak dağıtılmasını sağlayabilirsiniz. Daha fazla bilgi için bkz. [Microsoft SQL Server kapsayıcısı demo proje](https://github.com/twright-msft/mssql-node-docker-demo-app). 

## <a name="view-the-local-data"></a>Yerel verileri görüntüleme

Tablonuz oluşturulduktan sonra sqlFunction modülü verileri IoT Edge cihazınızdaki yerel bir SQL Server 2017 veritabanında depolamaya başlar. 

SQL komut aracında biçimlendirilmiş tablo verilerinizi görüntülemek için aşağıdaki komutu çalıştırın: 

   ```sql
   SELECT * FROM MeasurementsDB.dbo.TemperatureMeasurements
   GO
   ```

   ![Yerel veritabanı içeriğini görüntüleyin](./media/tutorial-store-data-sql-server/view-data.png)



## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir sonraki önerilen makaleye geçmeyi planlıyorsanız, oluşturduğunuz kaynaklarla yapılandırmaları tutabilir ve yeniden kullanabilirsiniz. Aynı IoT Edge cihazını test cihazı olarak kullanmaya devam edebilirsiniz. 

Geçmeyecekseniz ücret kesilmesini önlemek için yerel yapılandırmalarınızı ve bu makalede oluşturulan Azure kaynaklarını silebilirsiniz. 

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, IoT Edge cihazınız tarafından oluşturulan ham verileri filtreleme kodunu içeren bir Azure İşlevleri modülü oluşturdunuz. Kendi modüllerinizi oluşturmaya hazır olduğunuzda [Visual Studio Code için Azure IoT Edge ile Azure İşlevleri Geliştirme](how-to-develop-csharp-function.md) hakkında daha fazla bilgi edinebilirsiniz. 

Azure IoT Edge'in verileri iş içgörüsüne çevirmenize yardımcı olabilecek diğer yollar hakkında bilgi edinmek için bir sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [C# kodunu kullanarak sensör verilerini filtreleme](tutorial-csharp-module.md)
