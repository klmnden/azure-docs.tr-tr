---
title: "Bir Azure IOT kenar modülü C# ile oluşturma | Microsoft Docs"
description: "Bu öğretici nasıl en son Azure IOT kenar NuGet paketleri, Visual Studio Code ve C# kullanarak bir bırak veri dönüştürücü modülü yazılacağını gösterir."
services: iot-hub
author: jeffreyCline
manager: timlt
keywords: "Azure IOT, öğretici, modül, nuget, vscode, csharp, sınır"
ms.service: iot-hub
ms.devlang: csharp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2017
ms.author: jcline
ms.openlocfilehash: 7175ffc8de2c043593d61143b402484d33e4a8cc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-an-azure-iot-edge-module-with-cx23"></a>Bir Azure IOT kenar modülü C & #x23 oluşturun;

Bu öğretici için bir modül oluşturma paylaşan `Azure IoT Edge` kullanarak `Visual Studio Code` ve `C#`.

Ortam kurulumu ve nasıl yazılacağını Bu öğreticide, biz yol bir [bırak](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) en son kullanılarak veri dönüştürücü Modülü `Azure IoT Edge NuGet` paketler. 

>[!NOTE]
Bu öğretici kullanarak `.NET Core SDK`, platformlar arası uyumluluk destekler. Aşağıdaki öğreticide kullanılarak yazılmış `Windows 10` işletim sistemi. Bu öğretici komutlarda bazıları bağlı olarak farklı olabilir, `development environment`. 

## <a name="prerequisites"></a>Ön koşullar

Bu bölümde, biz Kurulum ortamınız için `Azure IoT Edge` modülü geliştirme. Her ikisi de geçerli **64-bit Windows** ve **64-bit Linux (Ubuntu/Debian 8)** işletim sistemleri.

Aşağıdaki yazılımlar gereklidir:

- [Git istemci](https://git-scm.com/downloads)
- [.NET Core SDK](https://www.microsoft.com/net/core#windowscmd)
- [Visual Studio Code](https://code.visualstudio.com/)

Bu öğreticide ele alınan tüm örnek kodları bulunur ancak aşağıdaki depoya Bu örnek, depoyu kopyalama gerekmez:

- `git clone https://github.com/Azure-Samples/iot-edge-samples.git`.
- `cd iot-edge-samples/dotnetcore/simulated_ble`

## <a name="getting-started"></a>Başlarken

1. Yükleme `.NET Core SDK`.
2. Yükleme `Visual Studio Code` ve `C# extension` Visual Studio kod Marketi'nden.

Bu görünümü, [hızlı videosu](https://channel9.msdn.com/Blogs/dotnet/Get-started-VSCode-Csharp-NET-Core-Windows) kullanmaya başlamak nasıl hakkında `Visual Studio Code` ve `.NET Core SDK`.

## <a name="creating-the-azure-iot-edge-converter-module"></a>Azure IOT kenar dönüştürücü modül oluşturma

1. Yeni bir başlatma `.NET Core` sınıf kitaplığı C# projesi:
    - Bir komut istemi açın (`Windows + R` -> `cmd` -> `enter`).
    - Burada istediğinizi oluşturmak klasöre gidin `C#` projesi.
    - Tür **dotnet yeni classlib -o IoTEdgeConverterModule -f netstandard1.3**. 
    - Bu komut adlı boş bir sınıf oluşturur `Class1.cs` projeleri dizininizde.
2. Burada yeni oluşturduğumuz sınıf kitaplığı proje yazarak klasöre gidin **cd IoTEdgeConverterModule**.
3. Projeyi `Visual Studio Code` yazarak **kodu**.
4. Proje içinde açıldıktan sonra `Visual Studio Code`, tıklayın **IoTEdgeConverterModule.csproj** aşağıdaki görüntüde gösterildiği gibi dosyayı açmak için:

    ![Visual Studio Code düzenleme penceresi](media/iot-hub-iot-edge-create-module/vscode-edit-csproj.png)

5. INSERT `XML` kapatma arasındaki aşağıdaki kod parçacığında gösterildiği blob `PropertyGroup` etiketi ve kapanış `Project` etiketi; altı önceki görüntüde satır ve tuşlarına basarak dosyayı kaydedin `Ctrl`  +  `S`.

   ```xml
     <ItemGroup>
       <PackageReference Include="Microsoft.Azure.Devices.Gateway.Module.NetStandard" Version="1.0.5" />
       <PackageReference Include="System.Threading.Thread" Version="4.3.0" />
       <PackageReference Include="Newtonsoft.Json" Version="10.0.2" />
     </ItemGroup> 
   ```

6. Kaydettikten sonra `.csproj` dosyası `Visual Studio Code` sizinle ister bir `unresolved dependencies` aşağıdaki resimde görüldüğü gibi iletişim: 

    ![Visual Studio Code geri yükleme bağımlılıkları iletişim kutusu](media/iot-hub-iot-edge-create-module/vscode-restore.png)

    bir) tıklatın `Restore` tüm projelerdeki başvuruları geri yüklemek için `.csproj` dosyası dahil `PackageReferences` ekledik. 

    b) `Visual Studio Code` otomatik olarak oluşturur `project.assets.json` projelerinizi dosyasında `obj` klasör. Bu dosya sonraki geri yüklemeler daha hızlı hale getirmek için projenizin bağımlılıkları hakkında bilgi içerir.
 
    >[!NOTE]
    `.NET Core Tools`MSBuild tabanlı sunulmuştur. Anlamına gelen bir `.csproj` proje dosyası oluşturuldu. yerine bir `project.json`.

    - Varsa `Visual Studio Code` Tamam, sizden istemez biz el ile yapabilirsiniz. Açık `Visual Studio Code` basarak tümleşik terminal penceresi `Ctrl`  +  `backtick` anahtarları veya menüleri kullanarak `View`  ->  `Integrated Terminal`.
    - İçinde `Integrated Terminal` pencere türü **dotnet geri yükleme**.
    
7. Yeniden Adlandır `Class1.cs` dosyasını `BleConverterModule.cs`. 

    bir) yeniden adlandırmak için dosyanın ilk tıklatın dosyada tuşuna basarak `F2` anahtarı.
    
    (b) yeni adı yazın **BleConverterModule**, aşağıdaki resimde görüldüğü gibi:

    ![Visual Studio Code bir sınıfı yeniden adlandırma](media/iot-hub-iot-edge-create-module/vscode-rename.png)

8. Varolan kodla `BleConverterModule.cs` içine aşağıdaki kod parçacığını yapıştırarak dosya, `BleConverterModule.cs` dosya.

   ```csharp
   using System;
   using System.Collections.Generic;
   using System.Globalization;
   using System.Linq;
   using System.Text;
   using System.Threading;
   using System.Threading.Tasks;
   using Microsoft.Azure.Devices.Gateway;
   using Newtonsoft.Json;
   
   namespace IoTEdgeConverterModule
   {
       public class BleConverterModule : IGatewayModule, IGatewayModuleStart
       {
           private Broker broker;
           private string configuration;
           private int messageCount;

           public void Create(Broker broker, byte[] configuration)
           {
               this.broker = broker;
               this.configuration = System.Text.Encoding.UTF8.GetString(configuration);
           }
   
           public void Start()
           {
           }

           public void Destroy()
           {
           }

           public void Receive(Message received_message)
           {
               string recMsg = Encoding.UTF8.GetString(received_message.Content, 0, received_message.Content.Length);
               BleData receivedData = JsonConvert.DeserializeObject<BleData>(recMsg);

               float temperature = float.Parse(receivedData.Temperature, CultureInfo.InvariantCulture.NumberFormat); 
               Dictionary<string, string> receivedProperties = received_message.Properties;
            
               Dictionary<string, string> properties = new Dictionary<string, string>();
               properties.Add("source", receivedProperties["source"]);
               properties.Add("macAddress", receivedProperties["macAddress"]);
               properties.Add("temperatureAlert", temperature > 30 ? "true" : "false");
   
               String content = String.Format("{0} \"deviceId\": \"Intel NUC Gateway\", \"messageId\": {1}, \"temperature\": {2} {3}", "{", ++this.messageCount, temperature, "}");
               Message messageToPublish = new Message(content, properties);
   
               this.broker.Publish(messageToPublish);
           }
       }
   }
   ```

9. Tuşlarına basarak dosyayı kaydedin `Ctrl`  +  `S`.

10. Adlı yeni bir dosya oluşturun `Untitled-1` basarak `Ctrl`  +  `N` anahtarları aşağıdaki resimde görüldüğü gibi:

    ![Visual Studio Code yeni dosya](media/iot-hub-iot-edge-create-module/vscode-new-file.png)

11. Seri durumdan çıkarılacak `JSON` benzetimli aldığımız nesne `BLE` aygıt, aşağıdaki kodu kopyalayın `Untitled-1` dosya kod düzenleyici penceresini. 

   ```csharp
   using System;
   using Newtonsoft.Json;

   namespace IoTEdgeConverterModule
   {
       public class BleData
       {
           [JsonProperty(PropertyName = "temperature")]
           public string Temperature { get; set; }
       }
   }
   ```

12. Dosyayı Farklı Kaydet `BleData.cs` basarak `Ctrl`  +  `Shift`  +  `S` anahtarları.
    - Farklı Kaydet iletişim kutusu, şirket içinde `Save as Type` açılır menüsünde, select `C# (*.cs;*.csx)` aşağıdaki resimde görüldüğü gibi:

    ![Visual Studio Code iletişim kutusu olarak Kaydet](media/iot-hub-iot-edge-create-module/vscode-save-as.png)

13. Adlı yeni bir dosya oluşturun `Untitled-1` basarak `Ctrl`  +  `N` anahtarları.

14. İçine aşağıdaki kod parçacığını kopyalayıp `Untitled-1` dosya. Bu sınıf, bir `Azure IoT Edge` alınan veri çıkışı için kullanırız modülü bizim `BleConverterModule`.

   ```csharp
   using System;
   using System.Collections.Generic;
   using System.Linq;
   using System.Text;
   using System.Threading.Tasks;
   using Microsoft.Azure.Devices.Gateway;
   using Newtonsoft.Json;
   
   namespace PrinterModule
   {
       public class DotNetPrinterModule : IGatewayModule
       {
           private string configuration;
           public void Create(Broker broker, byte[] configuration)
           {
               this.configuration = System.Text.Encoding.UTF8.GetString(configuration);
           }
   
           public void Destroy()
           {
           }
   
           public void Receive(Message received_message)
           {
               string recMsg = System.Text.Encoding.UTF8.GetString(received_message.Content, 0, received_message.Content.Length);
               Dictionary<string, string> receivedProperties = received_message.Properties;
               
               BleConverterData receivedData = JsonConvert.DeserializeObject<BleConverterData>(recMsg);
   
               Console.WriteLine();
               Console.WriteLine("Module           : [DotNetPrinterModule]");
               Console.WriteLine("received_message : {0}", recMsg);
   
               if(received_message.Properties["source"] == "bleTelemetry")
               {
                   Console.WriteLine("Source           : {0}", receivedProperties["source"]);
                   Console.WriteLine("MAC address      : {0}", receivedProperties["macAddress"]);
                   Console.WriteLine("Temperature Alert: {0}", receivedProperties["temperatureAlert"]);
                   Console.WriteLine("Deserialized Obj : [BleConverterData]");
                   Console.WriteLine("  DeviceId       : {0}", receivedData.DeviceId);
                   Console.WriteLine("  MessageId      : {0}", receivedData.MessageId);
                   Console.WriteLine("  Temperature    : {0}", receivedData.Temperature);
               }
   
               Console.WriteLine();
           }
       }
   }
   ```

15. Dosyayı Farklı Kaydet `DotNetPrinterModule.cs` basarak `Ctrl`  +  `Shift`  +  `S`.
    - Farklı Kaydet iletişim kutusu, şirket içinde `Save as Type` açılır menüsünde, select `C# (*.cs;*.csx)`.

16. Adlı yeni bir dosya oluşturun `Untitled-1` basarak `Ctrl`  +  `N` anahtarları.

17. Seri durumdan çıkarılacak `JSON` gelen aldığımız nesne `BleConverterModule`, Kopyala ve Yapıştır aşağıdaki kod parçacığını içine `Untitled-1` dosya. 

   ```csharp
   using System;
   using Newtonsoft.Json;

   namespace PrinterModule
   {
       public class BleConverterData
       {
           [JsonProperty(PropertyName = "deviceId")]
           public string DeviceId { get; set; }
   
           [JsonProperty(PropertyName = "messageId")]
           public string MessageId { get; set; }
   
           [JsonProperty(PropertyName = "temperature")]
           public string Temperature { get; set; }
       }
   }
   ```

18. Dosyayı Farklı Kaydet `BleConverterData.cs` basarak `Ctrl`  +  `Shift`  +  `S`.
    - Farklı Kaydet iletişim kutusu, şirket içinde `Save as Type` açılır menüsünde, select `C# (*.cs;*.csx)`.

19. Adlı yeni bir dosya oluşturun `Untitled-1` basarak `Ctrl`  +  `N` anahtarları.

20. İçine aşağıdaki kod parçacığını kopyalayıp `Untitled-1` dosya.

   ```json
   {
       "loaders": [
           {
               "type": "dotnetcore",
               "name": "dotnetcore",
               "configuration": {
                   "binding.path": "dotnetcore.dll",
                   "binding.coreclrpath": "C:\\Program Files\\dotnet\\shared\\Microsoft.NETCore.App\\1.1.1\\coreclr.dll",
                   "binding.trustedplatformassemblieslocation": "C:\\Program Files\\dotnet\\shared\\Microsoft.NETCore.App\\1.1.1\\"
               }
           }
       ],
          "modules": [
           {
               "name": "simulated_device",
               "loader": {
                   "name": "native",
                   "entrypoint": {
                       "module.path": "simulated_device.dll"
                   }
               },
               "args": {
                   "macAddress": "01:02:03:03:02:01",
                   "messagePeriod": 500
               }
           },
           {
               "name": "ble_converter_module",
               "loader": {
                   "name": "dotnetcore",
                   "entrypoint": {
                       "assembly.name": "IoTEdgeConverterModule",
                       "entry.type": "IoTEdgeConverterModule.BleConverterModule"
                   }
               },
               "args": ""
           },
           {
               "name": "printer_module",
               "loader": {
                   "name": "dotnetcore",
                   "entrypoint": {
                       "assembly.name": "IoTEdgeConverterModule",
                       "entry.type": "PrinterModule.DotNetPrinterModule"
                   }
               },
               "args": ""
           }
       ],
       "links": [
           {
               "source": "simulated_device", "sink": "ble_converter_module"
           },
           {
               "source": "ble_converter_module", "sink": "printer_module"
           }
   ]
   }
   ```

21. Dosyayı Farklı Kaydet `gw-config.json` basarak `Ctrl`  +  `Shift`  +  `S`.
    - Farklı Kaydet iletişim kutusu, şirket içinde `Save as Type` açılır menüsünde, select `JSON (*.json;*.bowerrc;*.jshintrc;*.jscsrc;*.eslintrc;*.babelrc;*webmanifest)`.

22. Yapılandırma dosyası çıktı dizinine kopyalama etkinleştirmek için güncelleştirme `IoTEdgeConverterModule.csproj` aşağıdaki XML blob ile:

   ```xml
     <ItemGroup>
       <None Update="gw-config.json" CopyToOutputDirectory="PreserveNewest" />
     </ItemGroup>
   ```
    
   - Güncelleştirilmiş `IoTEdgeConverterModule.csproj` aşağıdaki görüntü gibi görünmelidir:

    ![Visual Studio Code güncelleştirilmiş .csproj dosyası](media/iot-hub-iot-edge-create-module/vscode-update-csproj.png)

23. Adlı yeni bir dosya oluşturun `Untitled-1` basarak `Ctrl`  +  `N` anahtarları.

24. İçine aşağıdaki kod parçacığını kopyalayıp `Untitled-1` dosya.

   ```powershell
   Copy-Item -Path $env:userprofile\.nuget\packages\microsoft.azure.devices.gateway.native.windows.x64\1.1.3\runtimes\win-x64\native\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.runtime.serialization.formatters\4.3.0\lib\netstandard1.4\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.runtime.serialization.primitives\4.3.0\lib\netstandard1.3\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\newtonsoft.json\10.0.2\lib\netstandard1.3\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.componentmodel.typeconverter\4.3.0\lib\netstandard1.5\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.collections.nongeneric\4.3.0\lib\netstandard1.3\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.collections.specialized\4.3.0\lib\netstandard1.3\* -Destination .\bin\Debug\netstandard1.3
   ```

25. Dosyayı Farklı Kaydet `binplace.ps1` basarak `Ctrl`  +  `Shift`  +  `S`.
    - Farklı Kaydet iletişim kutusu, şirket içinde `Save as Type` açılır menüsünde, select `PowerShell (*.ps1;*.psm1;*.psd1;*.pssc;*.psrc)`.

26. Tuşuna basarak projeyi oluşturun `Ctrl`  +  `Shift`  +  `B` anahtarları. İlk olarak, projeyi derlerken `Visual Studio Code` ile ister `No build task defined.` aşağıdaki resimde görüldüğü gibi iletişim:

    ![Visual Studio Code yapı görev iletişim kutusu](media/iot-hub-iot-edge-create-module/vscode-build-task.png)

    bir) tıklatın `Configure Build Task` düğmesi.

    b) içinde `Select a Task Runner` iletişim kutusu açılır menüsünde. Seçin `.NET Core` aşağıdaki resimde görüldüğü gibi: 

    ![Visual Studio Code görev iletişim kutusu seç](media/iot-hub-iot-edge-create-module/vscode-build-task-runner.png)

    c) tıkladığınızda `.NET Core` öğesi oluşturur `tasks.json` dosyasını, `.vscode` dizin ve dosya açılır `code editor` penceresi. Bu dosyayı değiştirmek için sekmeyi kapatmak gerek yoktur.

27.  Açık `Visual Studio Code` basarak tümleşik terminal penceresi `Ctrl`  +  `backtick` anahtarları veya menüleri kullanarak `View`  ->  `Integrated Terminal` ve türü **.\binplace.ps1** içine `PowerShell` komut istemi. Bu komut, tüm bağımlılıkları çıktı dizinine kopyalar.

28. Proje çıktı dizinine gidin `Integrated Terminal` yazarak penceresi **cd.\bin\Debug\netstandard1.3**.

29. Örnek Proje yazarak çalıştırırsınız **. \gw.exe gw-config.json** içine `Integrated Terminal` penceresi istemi. 
    - Bu öğreticide yakından adımları izlediğinizden, şimdi çalıştırması `Azure IoT Edge BLE Data Converter Module` aşağıdaki resimde görüldüğü gibi örnek proje:
    
        ![Visual Studio kodda çalıştıran sanal cihaz örneği](media/iot-hub-iot-edge-create-module/vscode-run.png)
    
    - Uygulamayı istiyorsanız, basın `<Enter>` anahtarı.

>[!IMPORTANT]
Kullanılacak önerilmez `Ctrl`  +  `C` sonlandırmak için `IoT Edge` ağ geçidi uygulaması (diğer bir deyişle, **gw.exe**). Bu eylem aşırı ciddiyeti işlemin neden olarak.

