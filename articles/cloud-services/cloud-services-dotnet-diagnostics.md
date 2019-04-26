---
title: Azure tanılama (.NET) bulut Hizmetleri ile kullanma | Microsoft Docs
description: Hata ayıklama, performans, izleme, trafik çözümlemesi ve daha fazla ölçüm için Azure bulut hizmetlerinden veri toplamak için Azure Tanılama'yı kullanma.
services: cloud-services
documentationcenter: .net
author: jpconnock
manager: timlt
editor: ''
ms.assetid: 89623a0e-4e78-4b67-a446-7d19a35a44be
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/22/2017
ms.author: jeconnoc
ms.openlocfilehash: ba69a5aaffb39c26731ffd209587a8c8223b032a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60337400"
---
# <a name="enabling-azure-diagnostics-in-azure-cloud-services"></a>Azure bulut hizmetlerinde Azure tanılamayı etkinleştirme
Bkz: [Azure tanılama genel bakış](../azure-diagnostics.md) arka plan Azure tanılama.

## <a name="how-to-enable-diagnostics-in-a-worker-role"></a>Bir çalışan rolünde tanılamayı etkinleştirme
Bu kılavuzda, .NET EventSource sınıfı kullanarak telemetri verilerini yayan bir Azure çalışan rolünde uygulamak açıklar. Azure Tanılama'yı telemetri verilerini toplamak ve bir Azure depolama hesabında depolamak için kullanılır. Bir çalışan rolü oluştururken, Visual Studio tanılama 1.0 ve önceki sürümleri .NET 2.4 için Azure SDK'çözümün bir parçası olarak otomatik olarak etkinleştirir. Aşağıdaki yönergeler, tanılama 1.0, çalışan rolü için devre dışı, çözüm ve dağıtma tanılama 1.2 veya 1.3 çalışan rolü, oluşturma işlemi açıklanmaktadır.

### <a name="prerequisites"></a>Önkoşullar
Bu makale, bir Azure aboneliğiniz varsa ve Visual Studio Azure SDK'sı ile kullandığınızı varsayar. Azure aboneliğiniz yoksa için kaydolabilirsiniz [ücretsiz deneme][Free Trial]. Emin olun [yüklemek ve Azure PowerShell sürümü 0.8.7 yapılandırma veya üzeri][Install and configure Azure PowerShell version 0.8.7 or later].

### <a name="step-1-create-a-worker-role"></a>1. Adım: Bir çalışan rolü oluşturma
1. **Visual Studio**’yu başlatın.
2. Oluşturma bir **Azure bulut hizmeti** gelen proje **bulut** .NET Framework 4.5 hedefleyen şablonu.  "WadExample" Projeyi adlandırın ve Tamam'a tıklayın.
3. Seçin **çalışan rolü** ve Tamam'a tıklayın. Proje oluşturulur.
4. İçinde **Çözüm Gezgini**, çift **WorkerRole1** özellikler dosyası.
5. İçinde **yapılandırma** sekmesi işaretini kaldırın **tanılamayı etkinleştir** tanılama 1.0 (Azure SDK 2.4 ve önceki) devre dışı bırakmak için.
6. Herhangi bir hata olduğunu doğrulamak için çözümünüzü derleyin.

### <a name="step-2-instrument-your-code"></a>2. Adım: Kodunuzu izleme
WorkerRole.cs içeriğini aşağıdaki kodla değiştirin. ' % S'sınıfı, SampleEventSourceWriter öğesinden devralınan [EventSource sınıfı][EventSource Class], dört günlük yöntemlerini uygular: **SendEnums**, **MessageMethod**, **SetOther** ve **HighFreq**. İlk parametre olarak **WriteEvent** yöntemi için ilgili olay kimliği tanımlar. Run yöntemi çağıran uygulanan günlük yöntemlerin her biri bir sonsuz döngüye uygulayan **SampleEventSourceWriter** 10 saniyede sınıfı.

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
            // see the MSDN topic at https://go.microsoft.com/fwlink/?LinkId=166357.

            return base.OnStart();
        }
    }
}
```


### <a name="step-3-deploy-your-worker-role"></a>3. Adım: Çalışan rolünüzün dağıtma

[!INCLUDE [cloud-services-wad-warning](../../includes/cloud-services-wad-warning.md)]

1. Seçerek, çalışan rolü Visual Studio'dan azure'a dağıtma **WadExample** sonra Çözüm Gezgini'nde proje **Yayımla** gelen **derleme** menüsü.
2. Aboneliğinizi seçin.
3. İçinde **Microsoft Azure yayımlama ayarları** iletişim kutusunda **yeni oluştur...** .
4. İçinde **bulut hizmeti oluşturma ve depolama hesabı** iletişim kutusunda girin bir **adı** (örneğin, "WadExample") ve bir bölge veya benzeşim grubu seçin.
5. Ayarlama **ortam** için **hazırlama**.
6. Diğer değiştirme **ayarları** olarak uygun ve tıklayın **Yayımla**.
7. Dağıtım tamamlandıktan sonra bulut hizmeti olan Azure portalında doğrulamak bir **çalıştıran** durumu.

### <a name="step-4-create-your-diagnostics-configuration-file-and-install-the-extension"></a>4. Adım: Tanılama yapılandırma dosyanızı oluşturun ve uzantıyı yükleme
1. Genel yapılandırma dosyası şeması tanımı aşağıdaki PowerShell komutunu çalıştırarak yükleyin:

    ```powershell
    (Get-AzureServiceAvailableExtension -ExtensionName 'PaaSDiagnostics' -ProviderNamespace 'Microsoft.Azure.Diagnostics').PublicConfigurationSchema | Out-File -Encoding utf8 -FilePath 'WadConfig.xsd'
    ```
2. Bir XML dosyasına ekleyin, **WorkerRole1** sağ tıklayarak proje **WorkerRole1** seçin ve proje **Ekle** -> **yeni öğe...** -> **Visual C# öğeleri** -> **veri** -> **XML dosyası**. "WadExample.xml" dosya adı.

   ![CloudServices_diag_add_xml](./media/cloud-services-dotnet-diagnostics/AddXmlFile.png)
3. WadConfig.xsd yapılandırma dosyası ile ilişkilendirin. Etkin pencereyi WadExample.xml Düzenleyicisi penceresi olduğundan emin olun. Tuşuna **F4** açmak için **özellikleri** penceresi. Tıklayın **şemaları** özelliğinde **özellikleri** penceresi. Tıklayın **...** içinde **şemaları** özelliği. **Ekle…** düğmesine düğme, XSD dosyasını kaydettiğiniz konuma gidin ve WadConfig.xsd dosyasını seçin. **Tamam** düğmesine tıklayın.

4. Aşağıdaki XML WadExample.xml yapılandırma dosyasının içeriğini değiştirin ve dosyayı kaydedin. Birkaç performans sayaçları toplamak için bu yapılandırma dosyasını tanımlar: CPU kullanımı ve bellek kullanımı için. Daha sonra yapılandırmayı SampleEventSourceWriter sınıftaki yöntemlerin karşılık gelen dört olayları tanımlar.

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

### <a name="step-5-install-diagnostics-on-your-worker-role"></a>5. Adım: Tanılama, çalışan rolünde yükleyin
Bir web veya çalışan rolü tanılama yönetmek için PowerShell cmdlet'leri şunlardır: Set-AzureServiceDiagnosticsExtension, Get-AzureServiceDiagnosticsExtension ve Remove-AzureServiceDiagnosticsExtension.

1. Azure PowerShell'i açın.
2. Çalışan rolünde tanılama yüklemek için bu betiği yürütün (Değiştir *StorageAccountKey* wadexample depolama hesabınız için depolama hesabınızın anahtarıyla ve *config_path* yoluyla *WadExample.xml* dosyası):

```powershell
$storage_name = "wadexample"
$key = "<StorageAccountKey>"
$config_path="c:\users\<user>\documents\visual studio 2013\Projects\WadExample\WorkerRole1\WadExample.xml"
$service_name="wadexample"
$storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key
Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Staging -Role WorkerRole1
```

### <a name="step-6-look-at-your-telemetry-data"></a>6. Adım: Telemetri verileriniz arayın
Visual Studio **Sunucu Gezgini**, wadexample depolama hesabına gidin. Bulut hizmeti yaklaşık beş (5) dakikalık çalıştırıldıktan sonra tabloları görmelisiniz **WADEnumsTable**, **WADHighFreqTable**, **WADMessageTable**, **WADPerformanceCountersTable** ve **WADSetOtherTable**. Tabloların toplanan telemetri verilerini görüntülemek için çift tıklayın.

![CloudServices_diag_tables](./media/cloud-services-dotnet-diagnostics/WadExampleTables.png)

## <a name="configuration-file-schema"></a>Yapılandırma dosyası şeması
Tanılama yapılandırma dosyası, tanılama aracısını başladığında tanılama yapılandırma ayarları başlatmak üzere kullanılan değerleri tanımlar. Bkz: [en son şema başvurusu](/azure/azure-monitor/platform/diagnostics-extension-schema) için geçerli değerler ve örnekler.

## <a name="troubleshooting"></a>Sorun giderme
Sorun varsa, bkz: [Azure tanılama sorunlarını giderme](../azure-diagnostics-troubleshooting.md) ortak sorunları ile ilgili Yardım için.

## <a name="next-steps"></a>Sonraki Adımlar
[İlgili Azure sanal makinesi tanılama makaleler listesini](../azure-monitor/platform/diagnostics-extension-overview.md#cloud-services-using-azure-diagnostics) toplama verileri değiştirmek için ilgili sorunları giderme veya genel Tanılama hakkında daha fazla bilgi edinin.

[EventSource Class]: https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource(v=vs.110).aspx

[Debugging an Azure Application]: https://msdn.microsoft.com/library/windowsazure/ee405479.aspx   
[Collect Logging Data by Using Azure Diagnostics]: https://msdn.microsoft.com/library/windowsazure/gg433048.aspx
[Free Trial]: https://azure.microsoft.com/pricing/free-trial/
[Install and configure Azure PowerShell version 0.8.7 or later]: https://azure.microsoft.com/documentation/articles/install-configure-powershell/
