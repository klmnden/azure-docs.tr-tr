---
title: Bulut Hizmetleri ile Azure tanılama (.NET) kullanmak üzere nasıl | Microsoft Docs
description: Hata ayıklama, performans, izleme, trafik analizi ve daha fazla ölçmek için Azure bulut hizmetlerinden veri toplamak üzere Azure Tanılama'yı kullanarak.
services: cloud-services
documentationcenter: .net
author: thraka
manager: timlt
editor: ''
ms.assetid: 89623a0e-4e78-4b67-a446-7d19a35a44be
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/22/2017
ms.author: adegeo
ms.openlocfilehash: a8d6b16fa363062e06d48bfc5af2ca37697d5cd8
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/22/2018
ms.locfileid: "29460908"
---
# <a name="enabling-azure-diagnostics-in-azure-cloud-services"></a>Azure Cloud Services, Azure Tanılama'yı etkinleştirme
Bkz: [Azure tanılama genel bakış](../azure-diagnostics.md) bir arka planda Azure tanılama için.

## <a name="how-to-enable-diagnostics-in-a-worker-role"></a>Çalışan rolü tanılamada etkinleştirme
Bu kılavuz, .NET EventSource sınıfı kullanarak telemetri verileri gösterdiği Azure çalışan rolüne uygulamak açıklar. Azure tanılama telemetri verilerini toplamak ve bir Azure depolama hesabında depolamak için kullanılır. Çalışan rolü oluştururken, Visual Studio tanılama 1.0 Azure SDK'ları ve önceki sürümler için .NET 2.4 çözümde bir parçası olarak otomatik olarak etkinleştirir. Aşağıdaki yönergeler tanılama 1.0 çözüm ve dağıtma tanılama 1.2 veya 1.3 çalışan rolünüzün devre dışı bırakma çalışan rolü oluşturma işlemi açıklar.

### <a name="prerequisites"></a>Önkoşullar
Bu makale bir Azure aboneliğiniz varsa ve Azure SDK ile Visual Studio'ya varsayar. Bir Azure aboneliğiniz yoksa için kaydolabilirsiniz [ücretsiz deneme][Free Trial]. Emin olun [yükleyin ve Azure PowerShell sürüm 0.8.7 yapılandırın veya daha sonra][Install and configure Azure PowerShell version 0.8.7 or later].

### <a name="step-1-create-a-worker-role"></a>1. adım: Çalışan rolü oluşturma
1. **Visual Studio**’yu başlatın.
2. Oluşturma bir **Azure bulut hizmeti** gelen proje **bulut** .NET Framework 4. 5'i hedefleyen şablonu.  "WadExample" Projeyi adlandırın ve Tamam'ı tıklatın.
3. Seçin **çalışan rolü** ve Tamam'ı tıklatın. Proje oluşturulur.
4. İçinde **Çözüm Gezgini**, çift **WorkerRole1** özellikleri dosya.
5. İçinde **yapılandırma** sekmesinde, kaldırma onay **tanılamayı etkinleştir** tanılama 1.0 (Azure SDK 2.4 ve önceki) devre dışı bırakmak için.
6. Herhangi bir hata olduğunu doğrulamak için çözümünüzü oluşturun.

### <a name="step-2-instrument-your-code"></a>2. adım: kodunuzu izleme
WorkerRole.cs içeriğini aşağıdaki kodla değiştirin. Sınıf SampleEventSourceWriter, devralınan [EventSource sınıfı][EventSource Class], dört günlük yöntemlerini uygular: **SendEnums**, **MessageMethod**, **SetOther** ve **HighFreq**. İlk parametre olarak **WriteEvent** yöntemi ilgili olay kimliği tanımlar. Run yöntemi uygulanan günlük yöntemlerin her biri çağırır sonsuz bir döngüde uygulayan **SampleEventSourceWriter** 10 saniyede sınıfı.

```csharp
using Microsoft.WindowsAzure.ServiceRuntime;
using System;
using System.Diagnostics;
using System.Diagnostics.Tracing;
using System.Net;
using System.Threading;

namespace WorkerRole1
{
    sealed class SampleEventSourceWriter : EventSource
    {
        public static SampleEventSourceWriter Log = new SampleEventSourceWriter();
        public void SendEnums(MyColor color, MyFlags flags) { if (IsEnabled())  WriteEvent(1, (int)color, (int)flags); }// Cast enums to int for efficient logging.
        public void MessageMethod(string Message) { if (IsEnabled())  WriteEvent(2, Message); }
        public void SetOther(bool flag, int myInt) { if (IsEnabled())  WriteEvent(3, flag, myInt); }
        public void HighFreq(int value) { if (IsEnabled()) WriteEvent(4, value); }

    }

    enum MyColor
    {
        Red,
        Blue,
        Green
    }

    [Flags]
    enum MyFlags
    {
        Flag1 = 1,
        Flag2 = 2,
        Flag3 = 4
    }

    public class WorkerRole : RoleEntryPoint
    {
        public override void Run()
        {
            // This is a sample worker implementation. Replace with your logic.
            Trace.TraceInformation("WorkerRole1 entry point called");

            int value = 0;

            while (true)
            {
                Thread.Sleep(10000);
                Trace.TraceInformation("Working");

                // Emit several events every time we go through the loop
                for (int i = 0; i < 6; i++)
                {
                    SampleEventSourceWriter.Log.SendEnums(MyColor.Blue, MyFlags.Flag2 | MyFlags.Flag3);
                }

                for (int i = 0; i < 3; i++)
                {
                    SampleEventSourceWriter.Log.MessageMethod("This is a message.");
                    SampleEventSourceWriter.Log.SetOther(true, 123456789);
                }

                if (value == int.MaxValue) value = 0;
                SampleEventSourceWriter.Log.HighFreq(value++);
            }
        }

        public override bool OnStart()
        {
            // Set the maximum number of concurrent connections
            ServicePointManager.DefaultConnectionLimit = 12;

            // For information on handling configuration changes
            // see the MSDN topic at http://go.microsoft.com/fwlink/?LinkId=166357.

            return base.OnStart();
        }
    }
}
```


### <a name="step-3-deploy-your-worker-role"></a>Adım 3: çalışan rolünüzün dağıtma

[!INCLUDE [cloud-services-wad-warning](../../includes/cloud-services-wad-warning.md)]

1. Seçerek, çalışan rolü Azure'dan Visual Studio içinde dağıtmak **WadExample** sonra Çözüm Gezgini'nde projeye **Yayımla** gelen **yapı** menüsü.
2. Aboneliğinizi seçin.
3. İçinde **Microsoft Azure yayımlama ayarları** iletişim kutusunda **yeni oluştur...** .
4. İçinde **bulut hizmeti oluşturma ve depolama hesabı** iletişim kutusunda, girin bir **adı** (örneğin, "WadExample") ve bir bölge veya benzeşim grubu seçin.
5. Ayarlama **ortam** için **hazırlama**.
6. Diğer değiştirme **ayarları** olarak uygun ve tıklatın **Yayımla**.
7. Dağıtım tamamlandıktan sonra bulut hizmeti olan Azure portalında doğrulayın bir **çalıştıran** durumu.

### <a name="step-4-create-your-diagnostics-configuration-file-and-install-the-extension"></a>4. adım: Tanılama yapılandırma dosyanızı oluşturun ve uzantısını yükle
1. Genel yapılandırma dosyası şeması tanımı aşağıdaki PowerShell komutunu yürüterek yükleyin:

    ```powershell
    (Get-AzureServiceAvailableExtension -ExtensionName 'PaaSDiagnostics' -ProviderNamespace 'Microsoft.Azure.Diagnostics').PublicConfigurationSchema | Out-File -Encoding utf8 -FilePath 'WadConfig.xsd'
    ```
2. Bir XML dosyasına eklemek, **WorkerRole1** sağ tıklayarak projeyi **WorkerRole1** proje ve seçin **Ekle** -> **yeni öğe...** -> **Visual C# öğeleri** -> **veri** -> **XML dosyası**. "WadExample.xml" dosya adı.

   ![CloudServices_diag_add_xml](./media/cloud-services-dotnet-diagnostics/AddXmlFile.png)
3. WadConfig.xsd yapılandırma dosyası ile ilişkilendirin. Etkin pencereyi WadExample.xml Düzenleyicisi penceresini olduğundan emin olun. Tuşuna **F4** açmak için **özellikleri** penceresi. Tıklatın **şemaları** özelliğinde **özellikleri** penceresi. Tıklatın **...** içinde **şemaları** özelliği. **Ekle…** düğmesine düğmesini ve XSD dosyasını kaydettiğiniz konuma gidin ve WadConfig.xsd dosyasını seçin. **Tamam**’a tıklayın.

4. Aşağıdaki XML WadExample.xml yapılandırma dosyasının içeriğini değiştirin ve dosyayı kaydedin. Bu yapılandırma dosyasını toplamak için birkaç performans sayaçlarını tanımlar: CPU kullanımı, diğeri bellek kullanımı için. Ardından yapılandırmasını SampleEventSourceWriter sınıftaki yöntemlerin karşılık gelen dört olayları tanımlar.

```xml
<?xml version="1.0" encoding="utf-8"?>
<PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  <WadCfg>
    <DiagnosticMonitorConfiguration overallQuotaInMB="25000">
      <PerformanceCounters scheduledTransferPeriod="PT1M">
        <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT1M" unit="percent" />
        <PerformanceCounterConfiguration counterSpecifier="\Memory\Committed Bytes" sampleRate="PT1M" unit="bytes"/>
      </PerformanceCounters>
      <EtwProviders>
        <EtwEventSourceProviderConfiguration provider="SampleEventSourceWriter" scheduledTransferPeriod="PT5M">
          <Event id="1" eventDestination="EnumsTable"/>
          <Event id="2" eventDestination="MessageTable"/>
          <Event id="3" eventDestination="SetOtherTable"/>
          <Event id="4" eventDestination="HighFreqTable"/>
          <DefaultEvents eventDestination="DefaultTable" />
        </EtwEventSourceProviderConfiguration>
      </EtwProviders>
    </DiagnosticMonitorConfiguration>
  </WadCfg>
</PublicConfig>
```

### <a name="step-5-install-diagnostics-on-your-worker-role"></a>5. adım: Tanılama, çalışan rolü yükleyin.
Bir web veya çalışan rolü tanılama yönetmek için PowerShell cmdlet'leri şunlardır: Set-AzureServiceDiagnosticsExtension, Get-AzureServiceDiagnosticsExtension ve Kaldır-AzureServiceDiagnosticsExtension.

1. Open Azure PowerShell.
2. Tanılama, çalışan rolü yüklemek için komut dosyası yürütme (Değiştir *StorageAccountKey* wadexample depolama hesabınız için depolama hesabınızın anahtarıyla ve *config_path* yoluyla *WadExample.xml* dosyası):

```powershell
$storage_name = "wadexample"
$key = "<StorageAccountKey>"
$config_path="c:\users\<user>\documents\visual studio 2013\Projects\WadExample\WorkerRole1\WadExample.xml"
$service_name="wadexample"
$storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key
Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Staging -Role WorkerRole1
```

### <a name="step-6-look-at-your-telemetry-data"></a>6. adım: Telemetri verilerinizi bakma
Visual Studio'da **Sunucu Gezgini**, wadexample depolama hesabınıza gidin. Bulut hizmeti yaklaşık beş (5) dakika yürütüldükten sonra tabloları görmelisiniz **WADEnumsTable**, **WADHighFreqTable**, **WADMessageTable**, **WADPerformanceCountersTable** ve **WADSetOtherTable**. Toplanan telemetri görüntülemek için tablolar birini çift tıklatın.

![CloudServices_diag_tables](./media/cloud-services-dotnet-diagnostics/WadExampleTables.png)

## <a name="configuration-file-schema"></a>Yapılandırma dosyası şeması
Tanılama yapılandırma dosyası Tanılama aracı başladığında tanılama yapılandırma ayarlarını başlatmak için kullanılan değerleri tanımlar. Bkz: [en son şema başvurusu](https://msdn.microsoft.com/library/azure/mt634524.aspx) geçerli değerler ve örnekler için.

## <a name="troubleshooting"></a>Sorun giderme
Konusunda sorun yaşıyorsanız, bkz: [sorun giderme Azure tanılama](../azure-diagnostics-troubleshooting.md) sık karşılaşılan sorunları ile ilgili Yardım.

## <a name="next-steps"></a>Sonraki Adımlar
[İlgili Azure sanal makinesi tanılama makaleler listesini görmek](../monitoring-and-diagnostics/azure-diagnostics.md#cloud-services-using-azure-diagnostics) toplama verileri değiştirmek için ilgili sorunları giderme veya Tanılama hakkında daha fazla genel bilgi edinin.

[EventSource Class]: http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource(v=vs.110).aspx

[Debugging an Azure Application]: http://msdn.microsoft.com/library/windowsazure/ee405479.aspx   
[Collect Logging Data by Using Azure Diagnostics]: http://msdn.microsoft.com/library/windowsazure/gg433048.aspx
[Free Trial]: http://azure.microsoft.com/pricing/free-trial/
[Install and configure Azure PowerShell version 0.8.7 or later]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/
