---
title: "Azure IOT kenar SQL Modülü | Microsoft Docs"
description: "Microsoft SQL modüllerle verilerin biçimlendirilmesi için Azure işlevleri ile kenara verileri depolar."
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban, ebertrams
ms.date: 02/21/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 792e754b84f1dc03a32780ed94d274c833be68f5
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="store-data-at-the-edge-with-sql-server-databases"></a>SQL Server veritabanlarına sahip sınırda veri depolama

Azure IOT kenar cihazlar sınırında oluşturulan verileri depolamak için kullanır. Aralıklı Internet bağlantıları ile cihazları kendi veritabanlarına ve rapor değişiklikleri geri yalnızca bağlıyken buluta koruyabilirsiniz. Yalnızca kritik verileri buluta normal toplu veri geri kalanı kaydedebilirsiniz göndermek için programlanmış aygıtları yükler. Bulutta yapılandırılmış veri diğer Azure hizmetleriyle örneği için paylaşılabilir sonra makine öğrenimi modeli oluşturmak için. 

Bu makale, bir SQL Server veritabanı IOT kenar cihazına dağıtmak için yönergeler sağlar. Azure işlevleri, IOT kenar cihazda çalışan gelen veri yapıları sonra veritabanına gönderir. Bu makaledeki adımları, MySQL veya PostgreSQL gibi kapsayıcılarında iş diğer veritabanlarına da uygulanabilir. 

## <a name="prerequisites"></a>Önkoşullar 

Bu makaledeki yönergeleri başlamadan önce aşağıdaki öğreticiler tamamlamanız gerekir:
* Sanal bir cihaz üzerinde Azure IOT kenar dağıtmak [Windows](tutorial-simulate-device-windows.md) veya [Linux](tutorial-simulate-device-linux.md)
* [Azure işlevi IOT kenar modülü olarak dağıtma](tutorial-deploy-function.md)

Aşağıdaki makaleler Bu öğreticiyi başarıyla tamamlamak için gerekli değildir, ancak yararlı bağlam sağlayabilir:
* [SQL Server 2017 kapsayıcı görüntü Docker ile Çalıştır](https://docs.microsoft.com/sql/linux/quickstart-install-connect-docker)
* [Visual Studio Code geliştirmek ve Azure IOT kenarına Azure işlevleri dağıtmak için kullanın](how-to-vscode-develop-azure-function.md)

Gerekli öğreticileri tamamladıktan sonra tüm gerekli Önkoşullar hazır makinenizde sahip olmanız gerekir: 
* Etkin bir Azure IOT hub.
* En az 2 GB RAM ve 2 GB disk sürücüsü olan bir IOT sınır cihazı.
* [Visual Studio Code](https://code.visualstudio.com/). 
* [Visual Studio Code için Azure IOT kenar uzantısı](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge). 
* [C# Visual Studio Code (OmniSharp tarafından desteklenen) uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp). 
* [Docker](https://docs.docker.com/engine/installation/)
* [.NET 2.0 SDK çekirdek](https://www.microsoft.com/net/core#windowscmd). 
* [Python 2.7](https://www.python.org/downloads/)
* [IOT kenar denetim komut dosyası](https://pypi.python.org/pypi/azure-iot-edge-runtime-ctl)
* AzureIoTEdgeFunction şablonu (`dotnet new -i Microsoft.Azure.IoT.Edge.Function`)
* Etkin bir IOT hub ile en az bir IOT sınır cihazı.

Bu öğretici için işlemci mimarileri iş x64 hem Windows hem de Linux kapsayıcılarında. SQL Server ARM işlemcileri desteklemez.

## <a name="deploy-a-sql-server-container"></a>Bir SQL Server kapsayıcı dağıtma

Bu bölümde, sanal IOT kenar cihazınız bir MS SQL veritabanını ekleyin. SQL Server 2017 docker kapsayıcısı görüntüsü, kullanılabilir olarak kullanarak bir [Windows](https://hub.docker.com/r/microsoft/mssql-server-windows-developer/) kapsayıcısı ve bir [Linux](https://hub.docker.com/r/microsoft/mssql-server-linux/) kapsayıcı. 

### <a name="deploy-sql-server-2017"></a>SQL Server 2017 dağıtma

Varsayılan olarak, bu bölümdeki kod SQL Server 2017 ücretsiz Geliştirici sürümü ile bir kapsayıcı oluşturur. Bunun yerine üretim sürümleri çalıştırmak istiyorsanız, bkz: [kapsayıcı görüntüsünü üretim çalıştırın](https://docs.microsoft.com/sql/linux/sql-server-linux-configure-docker#production) ayrıntılı bilgi için. 

Adım 3'te, ortam değişkenleri ve persistant depolama kurmak için önemli olan seçenekleri SQL Server kapsayıcısı oluşturun, ekleyin. Yapılandırılmış [ortam değişkenleri](https://docs.microsoft.com/sql/linux/sql-server-linux-configure-environment-variables) son kullanıcı lisans sözleşmesinin kabul edin ve bir parola tanımlayın. [Persistant depolama](https://docs.microsoft.com/sql/linux/sql-server-linux-configure-docker#persist) kullanılarak yapılandırılmış [bağlar](https://docs.docker.com/storage/volumes/). Başlatmalar ile SQL Server 2017 kapsayıcı oluşturmak bir *sqlvolume* kapsayıcı silinse bile verilerinizi devam ederse ekli birim kapsayıcısı. 

1. Açık `deployment.json` Visual Studio Code dosyasında. 
2. Değiştir **modülleri** aşağıdaki kodla dosyasının bölümü: 

   ```json
   "modules": {
          "filterFunction": {
            "version": "1.0",
            "type": "docker",
            "status": "running",
            "restartPolicy": "always",
            "settings": {
              "image": "<docker registry address>/filterfunction:latest",
              "createOptions": "{}"
            }
          },
          "tempSensor": {
            "version": "1.0",
            "type": "docker",
            "status": "running",
            "restartPolicy": "always",
            "settings": {
              "image": "microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview",
              "createOptions": "{}"
            }
          },
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
        }
   ```

3. Değiştir `<docker registry address>` tamamlanmış öğreticiye doldurulmuş adresiyle [Azure işlevi dağıtmak bir IOT kenar modül - Önizleme](https://docs.microsoft.com/en-us/azure/iot-edge/tutorial-deploy-function)

   >[!NOTE]
   >Kapsayıcı kayıt defteri adresi kayıt defterinden kopyaladığınız oturum açma sunucusu ile aynıdır. Biçiminde olmalıdır `<your container registry name>.azurecr.io`

4. Çalıştırdığınız işletim sistemi'ne bağlı olarak SQL Modülü aşağıdaki kod ile güncelleştirin: 

   * Windows:

      ```json
      "image": "microsoft/mssql-server-windows-developer",
      "createOptions": "{\"Env\": [\"ACCEPT_EULA=Y\",\"MSSQL_SA_PASSWORD=Strong!Passw0rd\"],\"HostConfig\": {\"Mounts\": [{\"Target\": \"C:\\\\mssql\",\"Source\": \"sqlVolume\",\"Type\": \"volume\"}],\"PortBindings\": {\"1433/tcp\": [{\"HostPort\": \"1401\"}]}}"
      ```

   * Linux:

      ```json
      "image": "microsoft/mssql-server-linux:2017-latest",
      "createOptions": "{\"Env\": [\"ACCEPT_EULA=Y\",\"MSSQL_SA_PASSWORD=Strong!Passw0rd\"],\"HostConfig\": {\"Mounts\": [{\"Target\": \"/var/opt/mssql\",\"Source\": \"sqlVolume\",\"Type\": \"volume\"}],\"PortBindings\": {\"1433/tcp\": [{\"HostPort\": \"1401\"}]}}}"
      ```

5. Dosyayı kaydedin. 
6. VS Code komutu Palette seçin **kenar: sınır cihazı için dağıtımı oluşturma**. 
7. IOT kenar cihaz kimliği seçin.
8. Seçin `deployment.json` , güncelleştirilmiş dosya. Çıktı penceresinde dağıtımınız için karşılık gelen çıkışları görebilirsiniz. 
9. Edge çalışma zamanı başlatmak için **kenar: Başlangıç kenar** komutu paletindeki.

>[!TIP]
>Bir üretim ortamında, bir SQL Server kapsayıcı oluşturmak istediğiniz zaman, gereken [varsayılan sistem yöneticisi parolasını değiştirme](https://docs.microsoft.com/sql/linux/quickstart-install-connect-docker#change-the-sa-password).

## <a name="create-the-sql-database"></a>SQL veritabanı oluşturma

Bu bölümde IOT kenar cihaza bağlı algılayıcılar alınan sıcaklık verileri depolamak için SQL veritabanı ayarlanıyor aracılığıyla sırasında size kılavuzluk eder. Bir sanal cihaz kullanıyorsanız, bu verilerin geldiği *tempSensor* kapsayıcı. 

Bir komut satırı aracı, veritabanına bağlan: 

* Windows kapsayıcısı
   ```cmd
   docker exec -it sql cmd
   ```

* Linux kapsayıcısı
   ```bash
   docker exec -it sql bash
   ```

SQL komut aracını açın: 

* Windows kapsayıcısı
   ```cmd
   sqlcmd -S localhost -U SA -P 'Strong!Passw0rd'
   ```

* Linux kapsayıcısı
   ```bash
   /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P 'Strong!Passw0rd'
   ```

Veritabanınızı oluşturun: 

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

Tablonuz tanımlayın: 

   ```sql
   CREATE TABLE MeasurementsDB.dbo.TemperatureMeasurements (measurementTime DATETIME2, location NVARCHAR(50), temperature FLOAT)
   GO
   ```

Otomatik olarak birden çok IOT kenar cihazlarda dağıtılması için SQL Server'ı ayarlamak için SQL Server docker dosyanızı özelleştirebilirsiniz. Daha fazla bilgi için bkz: [Microsoft SQL Server kapsayıcı demo proje](https://github.com/twright-msft/mssql-node-docker-demo-app).

## <a name="understand-the-sql-connection"></a>SQL Bağlantısı anlama

Diğer eğitimlerine kapsayıcıları birbirinden yalıtılmış kalan sırasında iletişim kurmasına izin vermek için yollar kullanın. Ancak, SQL Server veritabanı ile çalışırken daha yakın bir ilişkisi gereklidir. 

Başlatıldığında IOT kenar Köprüsü (Linux) ya da NAT (Windows) ağ otomatik olarak oluşturur. Ağ olarak adlandırılır **azure IOT kenar**. Bu bağlantı hatalarını ayıklamak gerekiyorsa, komut satırı özelliklerinde arayabilirsiniz:

* Windows

   ```cmd
   docker network inspect azure-iot-edge
   ```

* Linux

   ```bash
   sudo docker network inspect azure-iot-edge
   ```

SQL Server veritabanınız IP adresini bakın gerek kalmaması IOT kenar docker, yoluyla bir kapsayıcı adı DNS de çözebilirsiniz. 

Örnek olarak, sonraki bölümde kullanırız bağlantı dizesi şöyledir: 

   ```csharp
   Data Source=tcp:sql,1433;Initial Catalog=MeasurementsDB;User Id=SA;Password=Strong!Passw0rd;TrustServerCertificate=False;Connection Timeout=30;
   ```

Bağlantı dizesi sunucu adına göre kapsayıcı başvuran görebilirsiniz **sql**. Başka bir şey olarak modül adı değişirse, bu bağlantı dizesini de güncelleştirin. Aksi halde, sonraki bölümde açın devam edin. 

## <a name="update-your-azure-function"></a>Azure işlevinizi güncelleştir
Veritabanınıza veri göndermek için önceki öğreticide yapılan FilterFunction Azure işlevi güncelleştirin. Bu dosya, algılayıcılar tarafından alınan veri yapıları sonra bir SQL tablosuna depolar şekilde değiştirin. 

1. Visual Studio Code FilterFunction klasörünüzü açın. 
2. Run.csx dosya önceki bölümdeki SQL bağlantı dizesi içeren aşağıdaki kod ile değiştirin: 

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
           const string str = "Data Source=tcp:sql,1433;Initial Catalog=MeasurementsDB;User Id=SA;Password=Strong!Passw0rd;TrustServerCertificate=False;Connection Timeout=30;";
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

## <a name="update-your-container-image"></a>Kapsayıcı Görüntünüzü güncelleştirmek

Yaptığınız değişiklikleri uygulamak için kapsayıcı Görüntünüzü güncelleştirmek, yayımlayın ve IOT kenar yeniden başlatın.

1. Visual Studio Code genişletin **Docker** klasör. 
2. Kullanmakta olduğunuz platformu temel alarak, genişletin **windows nano** veya **linux x64** klasör. 
3. Sağ **Dockerfile** dosya ve seçin **yapı IOT kenar modülü Docker görüntü**.
4. Gidin **FilterFunction** proje klasörünü ve tıklatın **EXE_DIR olarak klasörü seçin**.
5. VS Code pencerenin üstündeki açılır metin kutusuna görüntü adı girin. Örneğin, `<your container registry address>/filterfunction:latest`. Yerel bir kayıt defterine dağıtıyorsanız, ad olmalıdır `<localhost:5000/filterfunction:latest>`.
6. VS Code komutu palette seçin **kenar: anında IOT kenar modülü Docker görüntü**. 
7. Açılır metin kutusuna aynı görüntü adı girin. 
8. VS Code komutu palette seçin **kenar: kenar yeniden**.

## <a name="view-the-local-data"></a>Yerel veri görüntüleme

Kapsayıcılarınızı yeniden başlattığınızda sıcaklık algılayıcıları alınan verileri IOT kenar aygıtınızda yerel bir SQL Server 2017 veritabanında depolanır. 

Bir komut satırı aracı, veritabanına bağlan: 

* Windows kapsayıcısı
   ```cmd
   docker exec -it sql cmd
   ```

* Linux kapsayıcısı
   ```bash
   docker exec -it sql bash
   ```

SQL komut aracını açın: 

* Windows kapsayıcısı
   ```cmd
   sqlcmd -S localhost -U SA -P 'Strong!Passw0rd'
   ```

* Linux kapsayıcısı
   ```bash
   /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P 'Strong!Passw0rd'
   ```

Verilerinizi görüntüleyin: 

   ```sql
   SELECT * FROM MeasurementsDB.dbo.TemperatureMeasurements
   GO
   ```

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi edinmek için nasıl [Docker üzerinde SQL Server 2017 kapsayıcı görüntüleri yapılandırma](https://docs.microsoft.com/sql/linux/sql-server-linux-configure-docker).

* Ziyaret [mssql docker GitHub deposunu](https://github.com/Microsoft/mssql-docker) kaynakları, geri bildirim ve bilinen sorunlar için.
