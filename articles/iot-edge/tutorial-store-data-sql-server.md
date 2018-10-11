---
title: Azure IoT Edge SQL modülüyle veri depolama | Microsoft Docs
description: Bir SQL Server modülüyle IoT Edge cihazınızda yerel veri depolamayı öğrenin
services: iot-edge
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 09/21/2018
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: f10b8e34c653add6f93b0234a9efff43d78036f3
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2018
ms.locfileid: "48237221"
---
# <a name="tutorial-store-data-at-the-edge-with-sql-server-databases"></a>Öğretici: SQL Server veritabanları ile uç cihazlarda veri depolama

Uç cihazlarda veri depolamak ve sorgulamak için Azure IoT Edge ve SQL Server işlevlerini kullanın. Azure IoT Edge, cihazın çevrimdışı duruma geçmesi halinde iletileri önbelleğe alan ve bağlantı yeniden kurulduğunda ileten yerleşik temel depolama özelliklerine sahiptir. Ancak sorgu verilerini yerel ortamda sorgulayabilme gibi daha gelişmiş depolama özelliklerine ihtiyaç duyabilirsiniz. IoT Edge cihazlarınız yerel veritabanlarını kullanarak sabit IoT Hub bağlantısı olmadan da karmaşık işlemler gerçekleştirebilir. Örneğin bir saha teknisyeni, bir makinedeki son birkaç günlük sensör verilerini yerel ortamda görselleştirebilir ancak veriler, makine öğrenimi modelinin geliştirilmesine yardımcı olmak için yalnızca ayda bir buluta yükleniyor olabilir.

Bu makalede bir IoT Edge cihazına SQL Server veritabanı dağıtma yönergeleri yer almaktadır. IoT Edge cihazında çalışan Azure İşlevleri, gelen verileri yapılandırıp veritabanına gönderir. Bu makaledeki adımlar MySQL veya PostgreSQL gibi kapsayıcılarda çalışan diğer veritabanlarına da uygulanabilir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz: 

> [!div class="checklist"]
> * Visual Studio Code kullanarak Azure İşlevi oluşturma
> * IoT Edge cihazınıza bir SQL veritabanı dağıtma
> * Visual Studio Code uygulamasını kullanarak modülleri derleme ve IoT Edge cihazınıza dağıtma
> * Oluşturulan verileri görüntüleme

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

Bir Azure IoT Edge cihazı:

* [Linux](quickstart-linux.md) veya [Windows cihazları](quickstart.md) için hızlı başlangıç adımlarını izleyerek dağıtım makinenizi veya sanal makinenizi bir Edge cihazı olarak kullanabilirsiniz.

Bulut kaynakları:

* Azure'da ücretsiz katman bir [IoT Hub'ı](../iot-hub/iot-hub-create-through-portal.md). 

Geliştirme kaynakları:

* [Visual Studio Code](https://code.visualstudio.com/). 
* Visual Studio Code uygulamasında [Visual Studio Code için C# (OmniSharp tarafından desteklenen) uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp). 
* Visual Studio Code için [Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) uzantısı. 
* [.NET Core 2.1 SDK'sı](https://www.microsoft.com/net/download). 
* [Docker CE](https://docs.docker.com/install/). 

## <a name="create-a-container-registry"></a>Kapsayıcı kayıt defteri oluşturma
Bu öğreticide modül hazırlamak ve dosyalardan bir **kapsayıcı görüntüsü** oluşturmak için VS Code için Azure IoT Edge uzantısını kullanırsınız. Ardından bu görüntüyü, görüntülerinizin depolandığı ve yönetildiği **kayıt defterine** gönderirsiniz. Son olarak, görüntünüzü IoT Edge cihazınızda çalıştırmak üzere kayıt defterinizden dağıtırsınız.  

Bu öğretici için Docker ile uyumlu herhangi bir kayıt defteri kullanabilirsiniz. Bulutta sağlanan iki popüler Docker kayıt defteri hizmeti [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) ve [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)'dır. Bu öğreticide Azure Container Registry kullanılır. 

1. [Azure portalında](https://portal.azure.com), **Kaynak oluştur** > **Kapsayıcılar** > **Container Registry**'yi seçin.

    ![Kapsayıcı kayıt defteri oluşturma](./media/tutorial-deploy-function/create-container-registry.png)

2. Kayıt defterinize bir ad verin ve bir abonelik seçin.
3. Kaynak grubu için IoT Hub'ınızın bulunduğu kaynak grubu adıyla aynı adı kullanmanız önerilir. Tüm kaynakları aynı grupta tutarak birlikte yönetebilirsiniz. Örneğin, test için kullanılan kaynak grubunu sildiğinizde o grupta bulunan tüm test kaynakları da silinir. 
4. SKU'yu **Temel** olarak ayarlayın ve **Yönetici kullanıcı** ayarını **Etkinleştir** olarak değiştirin. 
5. **Oluştur**’a tıklayın.
6. Kapsayıcı kayıt defteriniz oluşturulduktan sonra, bu kayıt defterine gidin ve **Erişim anahtarları**'nı seçin. 
7. **Oturum açma sunucusu**, **Kullanıcı adı** ve **Parola** değerlerini kopyalayın. Öğreticinin sonraki bölümlerinde bu değerleri kullanacaksınız. 

## <a name="create-a-function-project"></a>İşlev projesi oluşturma

Bir veritabanına veri göndermek için verileri doğru şekilde yapılandırıp tabloda kaydeden bir modüle ihtiyacınız vardır. 

Aşağıdaki adımlarda, Visual Studio Code'u ve Azure IoT Edge uzantısını kullanarak IoT Edge işlevinin nasıl oluşturulduğu gösterilir.

1. Visual Studio Code'u açın.
2. **Görünüm** > **Terminal**'i seçerek VS Code tümleşik terminalini açın.
3. **View (Görünüm)** > **Command palette (Komut paleti)** öğesini seçerek VS Code komut paletini açın.
4. Komut paletinde **Azure: Sign in** komutunu yazıp çalıştırdıktan sonra yönergeleri izleyerek Azure hesabınızda oturum açın. Oturumu önceden açtıysanız bu adımı atlayabilirsiniz.
3. Komut paletinde **Azure IoT Edge: New IoT Edge solution** komutunu yazıp çalıştırın. Komut paletinde çözümünüzü oluşturmak için aşağıdaki bilgileri girin: 
   1. Çözümü oluşturmak istediğiniz klasörü seçin. 
   2. Çözümünüz için bir ad girin veya varsayılan **EdgeSolution** adını kabul edin.
   3. Modül şablonu olarak **Azure İşlevleri - C#** seçin. 
   4. Modülünüze **sqlFunction** adını verin. 
   5. İlk modülünüz için görüntü deposu olarak önceki bölümde oluşturduğunuz Azure Container Registry bileşenini belirtin. **localhost:5000** yerine kopyaladığınız oturum açma sunucusu değerini yazın. Dizenin son hali **\<kayıt adı\>.azurecr.io/sqlFunction** ifadesine benzer olmalıdır.

4. VS Code penceresi IoT Edge çözümü çalışma alanınızı yükler. **modules** klasörü, **.vscode** klasörü ve dağıtım bildirimi şablonu dosyası bulunur. **modules** > **sqlFunction** > **EdgeHubTrigger-Csharp** > **run.csx** dosyasını açın.

5. Dosyanın içeriğini aşağıdaki kodla değiştirin:

   ```csharp
   #r "Microsoft.Azure.Devices.Client"
   #r "Newtonsoft.Json"
   #r "System.Data.SqlClient"

   using System.IO;
   using Microsoft.Azure.Devices.Client;
   using Newtonsoft.Json;
   using Sql = System.Data.SqlClient;
   using System.Threading.Tasks;

   // Filter messages based on the temperature value in the body of the message and the temperature threshold value.
   public static async Task Run(Message messageReceived, IAsyncCollector<Message> output, TraceWriter log)
   {
       const int temperatureThreshold = 25;
       byte[] messageBytes = messageReceived.GetBytes();
       var messageString = System.Text.Encoding.UTF8.GetString(messageBytes);

       if (!string.IsNullOrEmpty(messageString))
       {
           // Get the body of the message and deserialize it
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
               log.Info($"{rows} rows were updated");
               }
           }

           if (messageBody != null && messageBody.machine.temperature > temperatureThreshold)
           {
               // Send the message to the output as the temperature value is greater than the threshold
               var filteredMessage = new Message(messageBytes);
               // Copy the properties of the original message into the new Message object
               foreach (KeyValuePair<string, string> prop in messageReceived.Properties)
               {
                   filteredMessage.Properties.Add(prop.Key, prop.Value);
               }
               // Add a new property to the message to indicate it is an alert
               filteredMessage.Properties.Add("MessageType", "Alert");
               // Send the message        
               await output.AddAsync(filteredMessage);
               log.Info("Received and transferred a message with temperature above the threshold");
           }
       }
   }

   //Define the expected schema for the body of incoming messages
   class MessageBody
   {
       public Machine machine {get;set;}
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
   ```

6. 24. satırda **\<sql connection string\>** yerine aşağıdaki dizeyi yazın. **Veri Kaynağı** özelliği, bir sonraki bölümde **SQL** adıyla oluşturacağınız SQL Server kapsayıcısına başvurmaktadır. 

   ```C#
   Data Source=tcp:sql,1433;Initial Catalog=MeasurementsDB;User Id=SA;Password=Strong!Passw0rd;TrustServerCertificate=False;Connection Timeout=30;
   ```

7. **run.csx** dosyasını kaydedin. 

## <a name="add-a-sql-server-container"></a>SQL Server kapsayıcısı ekleme

[Dağıtım bildirimi](module-composition.md), IoT Edge çalışma zamanının hangi modüllerinin IoT Edge cihazınıza yükleneceğini bildirir. Önceki bölümde özelleştirilmiş bir İşlev modülü oluşturmak için kod eklediniz ancak SQL Server modülü önceden oluşturulmuştur. Tek yapmanız gereken IoT Edge çalışma zamanına bunu dahil etmek ve cihazınızda yapılandırmaktır. 

1. Visual Studio Code gezgininde **deployment.template.json** dosyasını açın. 
2. **moduleContent.$edgeAgent.properties.desired.modules** bölümünü bulun. Listede iki modül bulunmalıdır: Simülasyon verilerini oluşturan **tempSensor** ve **sqlFunction** modülünüz.
3. Windows kapsayıcılarını kullanıyorsanız, **sqlFunction.settings.image** bölümünü değiştirin.
    ```json
    "image": "${MODULES.sqlFunction.windows-amd64}"
    ```
4. Üçüncü bir modül bildirmek için aşağıdaki kodu ekleyin. sqlFunction bölümünden sonra bir virgül ekleyip aşağıdakileri ekleyin:

   ```json
   "sql": {
       "version": "1.0",
       "type": "docker",
       "status": "running",
       "restartPolicy": "always",
       "settings": {
           "image": "",
           "createOptions": ""
       }
   }
   ```

   Burada bir JSON öğesi ile eklemeyle ilgili karışıklıklara karşı bir örnek verilmiştir. ![SQL Server kapsayıcısı ekleme](./media/tutorial-store-data-sql-server/view_json_sql.png)

5. IoT Edge cihazınızdaki Docker kapsayıcılarının türüne bağlı olarak **sql.settings** parametrelerini aşağıdaki kodla güncelleştirin:

   * Windows kapsayıcıları:

      ```json
      "image": "microsoft/mssql-server-windows-developer",
      "createOptions": "{\"Env\": [\"ACCEPT_EULA=Y\",\"SA_PASSWORD=Strong!Passw0rd\"],\"HostConfig\": {\"Mounts\": [{\"Target\": \"C:\\\\mssql\",\"Source\": \"sqlVolume\",\"Type\": \"volume\"}],\"PortBindings\": {\"1433/tcp\": [{\"HostPort\": \"1401\"}]}}}"
      ```

   * Linux kapsayıcıları:

      ```json
      "image": "mcr.microsoft.com/mssql/server:latest",
      "createOptions": "{\"Env\": [\"ACCEPT_EULA=Y\",\"MSSQL_SA_PASSWORD=Strong!Passw0rd\"],\"HostConfig\": {\"Mounts\": [{\"Target\": \"/var/opt/mssql\",\"Source\": \"sqlVolume\",\"Type\": \"volume\"}],\"PortBindings\": {\"1433/tcp\": [{\"HostPort\": \"1401\"}]}}}"
      ```

   >[!Tip]
   >Üretim ortamında bir SQL Server kapsayıcısı oluşturduğunuzda [varsayılan sistem yöneticisi parolasını değiştirmeniz gerekir](https://docs.microsoft.com/sql/linux/quickstart-install-connect-docker#change-the-sa-password).

6. **deployment.template.json** dosyasını kaydedin.

## <a name="build-your-iot-edge-solution"></a>IoT Edge çözümünüzü derleyin

Önceki bölümlerde tek modüle sahip bir çözüm oluşturdunuz ve dağıtım bildirimi şablonuna başka bir modül daha eklediniz. Şimdi çözümü derlemeniz, modüller için kapsayıcı görüntülerini oluşturmanız ve görüntüleri kapsayıcı kayıt defterinize göndermeniz gerekir. 

1. .env dosyasında modül görüntülerinize erişebilmesi için IoT Edge çalışma zamanına kayıt defteri kimlik bilgilerinizi girin. **CONTAINER_REGISTRY_USERNAME** ve **CONTAINER_REGISTRY_PASSWORD** bölümlerini bulun ve eşittir işaretinden sonra kimlik bilgilerinizi ekleyin: 

   ```env
   CONTAINER_REGISTRY_USERNAME_yourContainerReg=<username>
   CONTAINER_REGISTRY_PASSWORD_yourContainerReg=<password>
   ```

2. .env dosyasını kaydedin.
3. Görüntüleri kayıt defterinize gönderebilmeniz için kapsayıcı kayıt defterinizi Visual Studio Code'a ekleyin. .env dosyasına eklediğiniz kimlik bilgilerini kullanın. Tümleşik terminale aşağıdaki komutu girin:

    ```csh/sh
    docker login -u <ACR username> <ACR login server>
    ```
    Parola istenir. Parolanızı isteme yapıştırın ve **Enter** tuşuna basın.

    ```csh/sh
    Password: <paste in the ACR password and press enter>
    Login Succeeded
    ```

4. VS Code gezgininde **deployment.template.json** dosyasına sağ tıklayıp **Build and Push IoT Edge solution** (IoT Edge Çözümü Oluştur ve Gönder) öğesini seçin. 

## <a name="deploy-the-solution-to-a-device"></a>Çözümü bir cihaza dağıtma

IoT Hub üzerinden bir cihazda modül ayarlayabilirsiniz ancak IoT Hub ve cihazlara Visual Studio Code aracılığıyla da erişebilirsiniz. Bu bölümde IoT Hub'ınıza erişim ayarlarını yapacak ve ardından çözümünüzü IoT Edge cihazınıza dağıtmak için VS Code uygulamasını kullanacaksınız. 

1. VS Code komut paletinde **Azure IoT Hub: Select IoT Hub** komutunu seçin.
2. Azure hesabınızda oturum açmak için yönergeleri izleyin. 
3. Komut paletinde Azure aboneliğinizi ve ardından IoT Hub'ınızı seçin. 
4. VS Code gezgininde **Azure IoT Hub Devices** (Azure IoT Hub Cihazları) bölümünü seçin. 
5. Dağıtımınızla hedeflemek istediğiniz cihaza sağ tıklayıp **Create deployment for single device** (Tek cihaz için dağıtım oluştur) öğesini seçin. 
6. Dosya gezgininde çözümünüzün içindeki **config** klasörüne gidip **deployment.json** dosyasını seçin. **Select Edge deployment manifest** (Edge dağıtım bildirimini seç) öğesine tıklayın. 

Dağıtım başarılı olursa VS Code çıkışında onay iletisi yazdırılır. Tüm modüllerin cihazınızda çalışıp çalışmadığını da kontrol edebilirsiniz. 

IoT Edge cihazında modüllerin durumunu görmek için aşağıdaki komutu çalıştırın. Bu işlem birkaç dakika sürebilir.

   ```PowerShell
   iotedge list
   ```

## <a name="create-the-sql-database"></a>SQL veritabanını oluşturma

Dağıtım bildirimini cihazınıza uyguladığınızda üç modül çalışmaya başlar. tempSensor modülü ortam verisi simülasyonunu oluşturur. sqlFunction modülü verileri alır ve veritabanı için biçimlendirir. 

Bu bölüm, sıcaklık verilerini depolamak için kullanılacak SQL veritabanını ayarlama sürecinde size yardımcı olacaktır. 

1. Komut satırında veritabanınıza bağlanın. 
   * Windows kapsayıcısı:
   
      ```cmd
      docker exec -it sql cmd
      ```
    
   * Linux kapsayıcısı: 

      ```bash
      docker exec -it sql bash
      ```

2. SQL komut aracını açın.
   * Windows kapsayıcısı:

      ```cmd
      sqlcmd -S localhost -U SA -P "Strong!Passw0rd"
      ```

   * Linux kapsayıcısı: 

      ```bash
      /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P 'Strong!Passw0rd'
      ```

3. Veritabanınızı oluşturun: 

   * Windows kapsayıcısı
      ```sql
      CREATE DATABASE MeasurementsDB
      ON
      (NAME = MeasurementsDB, FILENAME = 'C:\mssql\measurementsdb.mdf')
      GO
      ```

   * Linux kapsayıcısı
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

   ![Yerel verileri görüntüleme](./media/tutorial-store-data-sql-server/view-data.png)



## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir sonraki önerilen makaleye geçmeyi planlıyorsanız, oluşturduğunuz kaynaklarla yapılandırmaları tutabilir ve yeniden kullanabilirsiniz. Aynı IoT Edge cihazını test cihazı olarak kullanmaya devam edebilirsiniz. 

Geçmeyecekseniz ücret kesilmesini önlemek için yerel yapılandırmalarınızı ve bu makalede oluşturulan Azure kaynaklarını silebilirsiniz. 

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

[!INCLUDE [iot-edge-clean-up-local-resources](../../includes/iot-edge-clean-up-local-resources.md)]



## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, IoT Edge cihazınız tarafından oluşturulan ham verileri filtreleme kodunu içeren bir Azure İşlevleri modülü oluşturdunuz. Kendi modüllerinizi oluşturmaya hazır olduğunuzda [Visual Studio Code için Azure IoT Edge ile Azure İşlevleri Geliştirme](how-to-develop-csharp-function.md) hakkında daha fazla bilgi edinebilirsiniz. 

Azure IoT Edge'in verileri iş içgörüsüne çevirmenize yardımcı olabilecek diğer yollar hakkında bilgi edinmek için bir sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Azure Stream Analytics'te kayan pencere kullanarak ortalamaları bulma](tutorial-deploy-stream-analytics.md)
