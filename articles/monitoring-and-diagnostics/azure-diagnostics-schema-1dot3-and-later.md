---
title: "Azure tanılama uzantısını 1.3 ve daha sonra yapılandırma şeması | Microsoft Docs"
description: "Microsoft Azure SDK 2.4 ve daha sonraki bir parçası olarak şema sürümü 1.3 ve daha sonra Azure tanılama geliyordu."
services: monitoring-and-diagnostics
documentationcenter: .net
author: rboucher
manager: carmonm
editor: 
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/15/2017
ms.author: robb
ms.openlocfilehash: 2ee66e0f41868d7d5411605596a22c00b5712896
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-diagnostics-13-and-later-configuration-schema"></a>Azure tanılama 1.3 ve daha sonra yapılandırma şeması
> [!NOTE]
> Azure tanılama uzantısını performans sayaçları ve diğer istatistikleri toplamak için kullanılan bileşendir:
> - Azure Sanal Makineler 
> - Sanal Makine Ölçek Kümeleri
> - Service Fabric 
> - Cloud Services 
> - Ağ Güvenlik Grupları
> 
> Bu sayfa, yalnızca bu hizmetlerden biri kullanıyorsanız geçerlidir.

Bu sayfa sürümleri 1.3 ve daha yeni (Azure SDK 2.4 ve daha yeni) için geçerli değil. Yeni yapılandırma bölümlerinin hangi sürümünün eklendikleri göstermek için bırakılır.  

Burada açıklanan yapılandırma dosyası tanılama İzleyicisi başlatıldığında tanılama yapılandırma ayarlarını belirlemek için kullanılır.  

Uzantısı Azure monitör, Application Insights ve günlük analizi gibi diğer Microsoft tanılama ürün ile birlikte kullanılır.



Genel yapılandırma dosyası şeması tanımı aşağıdaki PowerShell komutunu yürüterek yükleyin:  

```powershell  
(Get-AzureServiceAvailableExtension -ExtensionName 'PaaSDiagnostics' -ProviderNamespace 'Microsoft.Azure.Diagnostics').PublicConfigurationSchema | Out-File –Encoding utf8 -FilePath 'C:\temp\WadConfig.xsd'  
```  

Azure tanılama kullanma hakkında daha fazla bilgi için bkz: [Azure tanılama uzantısını](azure-diagnostics.md).  

## <a name="example-of-the-diagnostics-configuration-file"></a>Tanılama yapılandırma dosyası örneği  
 Aşağıdaki örnek bir genel tanılama yapılandırma dosyası gösterir:  

```xml  
<?xml version="1.0" encoding="utf-8"?>  
<DiagnosticsConfiguration  xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">   
  <PublicConfig>  
    <WadCfg>  
      <DiagnosticMonitorConfiguration overallQuotaInMB="10000">  

        <PerformanceCounters scheduledTransferPeriod="PT1M">  
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT1M" unit="percent" />  
        </PerformanceCounters>  

        <Directories scheduledTransferPeriod="PT5M">  
          <IISLogs containerName="iislogs" />  
          <FailedRequestLogs containerName="iisfailed" />  

          <DataSources>  
            <DirectoryConfiguration containerName="mynewprocess">  
              <Absolute path="C:\MyNewProcess" expandEnvironment="false" />  
            </DirectoryConfiguration>  
            <DirectoryConfiguration containerName="badapp">  
              <Absolute path="%SYSTEMDRIVE%\BadApp" expandEnvironment="true" />  
            </DirectoryConfiguration>  
            <DirectoryConfiguration containerName="goodapp">  
              <LocalResource name="Skippy" relativePath="..\PeanutButter"/>  
            </DirectoryConfiguration>  
          </DataSources>  

        </Directories>  

        <EtwProviders>  
          <EtwEventSourceProviderConfiguration   
                       provider="MyProviderClass"   
                       scheduledTransferPeriod="PT5M">  
            <Event id="0"/>  
            <Event id="1" eventDestination="errorTable"/>  
            <DefaultEvents />  
          </EtwEventSourceProviderConfiguration>  
          <EtwManifestProviderConfiguration provider="5974b00b-84c2-44bc-9e58-3a2451b4e3ad" scheduledTransferLogLevelFilter="Information" scheduledTransferPeriod="PT2M">  
            <Event id="0"/>  
            <DefaultEvents eventDestination="defaultTable"/>  
          </EtwManifestProviderConfiguration>  
        </EtwProviders>  

        <WindowsEventLog scheduledTransferPeriod="PT5M">  
          <DataSource name="System!*[System[Provider[@Name='Microsoft Antimalware']]]"/>  
          <DataSource name="System!*[System[Provider[@Name='NTFS'] and (EventID=55)]]" />  
          <DataSource name="System!*[System[Provider[@Name='disk'] and (EventID=7 or EventID=52 or EventID=55)]]" />  
        </WindowsEventLog>  

        <Logs  bufferQuotaInMB="1024"   
             scheduledTransferPeriod="PT1M"   
             scheduledTransferLogLevelFilter="Verbose"   
             sinks="ApplicationInsights.AppLogs"/>  <!-- sinks attribute added in 1.5 -->  

        <CrashDumps containerName="wad-crashdumps" directoryQuotaPercentage="30" dumpType="Mini">  
          <CrashDumpConfiguration processName="mynewprocess.exe" />  
          <CrashDumpConfiguration processName="badapp.exe"/>  
        </CrashDumps>  

        <DockerSources> <!-- Added in 1.9 --> 
          <Stats enabled="true" sampleRate="PT1M" scheduledTransferPeriod="PT1M" />
        </DockerSources>

      </DiagnosticMonitorConfiguration>  

      <SinksConfig>   <!-- Added in 1.5 -->  
        <Sink name="ApplicationInsights">   
          <ApplicationInsights>{Insert InstrumentationKey}</ApplicationInsights>   
          <Channels>   
            <Channel logLevel="Error" name="Errors"  />   
            <Channel logLevel="Verbose" name="AppLogs"  />   
          </Channels>   
        </Sink>   
        <Sink name="EventHub"> <!-- Added in 1.7 -->
          <EventHub Url="https://myeventhub-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" usePublisherId="false" />
        </Sink>
        <Sink name="secondaryEventHub"> <!-- Added in 1.7 -->
          <EventHub Url="https://myeventhub-ns.servicebus.windows.net/secondarydiageventhub" SharedAccessKeyName="SendRule" usePublisherId="false" />
        </Sink>
        <Sink name="secondaryStorageAccount"> <!-- Added in 1.7 -->
          <StorageAccount name="secondarydiagstorageaccount" endpoint="https://core.windows.net" />
        </Sink>
   </SinksConfig>

  </WadCfg>  

  <StorageAccount>diagstorageaccount</StorageAccount>
  <StorageType>TableAndBlob</StorageType> <!-- Added in 1.8 -->  
  </PublicConfig>  

  <PrivateConfig>  <!-- Added in 1.3 -->  
    <StorageAccount name="" key="" endpoint="" sasToken="{sas token}"  />  <!-- sasToken in Private config added in 1.8.1 -->  
    <EventHub Url="https://myeventhub-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" SharedAccessKey="{base64 encoded key}" />
   
    <SecondaryStorageAccounts>
       <StorageAccount name="secondarydiagstorageaccount" key="{base64 encoded key}" endpoint="https://core.windows.net" sasToken="{sas token}" />
    </SecondaryStorageAccounts>
   
    <SecondaryEventHubs>
       <EventHub Url="https://myeventhub-ns.servicebus.windows.net/secondarydiageventhub" SharedAccessKeyName="SendRule" SharedAccessKey="{base64 encoded key}" />
    </SecondaryEventHubs>

  </PrivateConfig>  
  <IsEnabled>true</IsEnabled>  
</DiagnosticsConfiguration>  

```  

Önceki XML yapılandırma dosyası denk JSON. 

Json kullanımı genellikle farklı değişkenleri olarak geçirilen çünkü PublicConfig ve PrivateConfig ayrılır. Bu durumlar: Resource Manager şablonları, PowerShell ve Visual Studio sanal makine ölçek kümesi. 

```json
"PublicConfig" {
    "WadCfg": {
        "DiagnosticMonitorConfiguration": {
            "overallQuotaInMB": 10000,
            "DiagnosticInfrastructureLogs": {
                "scheduledTransferLogLevelFilter": "Error"
            },
            "PerformanceCounters": {
                "scheduledTransferPeriod": "PT1M",
                "PerformanceCounterConfiguration": [
                    {
                        "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
                        "sampleRate": "PT1M",
                        "unit": "percent"
                    }
                ]
            },
            "Directories": {
                "scheduledTransferPeriod": "PT5M",
                "IISLogs": {
                    "containerName": "iislogs"
                },
                "FailedRequestLogs": {
                    "containerName": "iisfailed"
                },
                "DataSources": [
                    {
                        "containerName": "mynewprocess",
                        "Absolute": {
                            "path": "C:\\MyNewProcess",
                            "expandEnvironment": false
                        }
                    },
                    {
                        "containerName": "badapp",
                        "Absolute": {
                            "path": "%SYSTEMDRIVE%\\BadApp",
                            "expandEnvironment": true
                        }
                    },
                    {
                        "containerName": "goodapp",
                        "LocalResource": {
                            "relativePath": "..\\PeanutButter",
                            "name": "Skippy"
                        }
                    }
                ]
            },
            "EtwProviders": {
                "sinks": "",
                "EtwEventSourceProviderConfiguration": [
                    {
                        "scheduledTransferPeriod": "PT5M",
                        "provider": "MyProviderClass",
                        "Event": [
                            {
                                "id": 0
                            },
                            {
                                "id": 1,
                                "eventDestination": "errorTable"
                            }
                        ],
                        "DefaultEvents": {
                        }
                    }
                ],
                "EtwManifestProviderConfiguration": [
                    {
                        "scheduledTransferPeriod": "PT2M",
                        "scheduledTransferLogLevelFilter": "Information",
                        "provider": "5974b00b-84c2-44bc-9e58-3a2451b4e3ad",
                        "Event": [
                            {
                                "id": 0
                            }
                        ],
                        "DefaultEvents": {
                        }
                    }
                ]
            },
            "WindowsEventLog": {
                "scheduledTransferPeriod": "PT5M",
                "DataSource": [
                    {
                        "name": "System!*[System[Provider[@Name='Microsoft Antimalware']]]"
                    },
                    {
                        "name": "System!*[System[Provider[@Name='NTFS'] and (EventID=55)]]"
                    },
                    {
                        "name": "System!*[System[Provider[@Name='disk'] and (EventID=7 or EventID=52 or EventID=55)]]"
                    }
                ]
            },
            "Logs": {
                "scheduledTransferPeriod": "PT1M",
                "scheduledTransferLogLevelFilter": "Verbose",
                "sinks": "ApplicationInsights.AppLogs"
            },
            "CrashDumps": {
                "directoryQuotaPercentage": 30,
                "dumpType": "Mini",
                "containerName": "wad-crashdumps",
                "CrashDumpConfiguration": [
                    {
                        "processName": "mynewprocess.exe"
                    },
                    {
                        "processName": "badapp.exe"
                    }
                ]
            }
        },
        "SinksConfig": {
            "Sink": [
                {
                    "name": "ApplicationInsights",
                    "ApplicationInsights": "{Insert InstrumentationKey}",
                    "Channels": {
                        "Channel": [
                            {
                                "logLevel": "Error",
                                "name": "Errors"
                            },
                            {
                                "logLevel": "Verbose",
                                "name": "AppLogs"
                            }
                        ]
                    }
                },
                {
                    "name": "EventHub",
                    "EventHub": {
                        "Url": "https://myeventhub-ns.servicebus.windows.net/diageventhub",
                        "SharedAccessKeyName": "SendRule",
                        "usePublisherId": false
                    }
                },
                {
                    "name": "secondaryEventHub",
                    "EventHub": {
                        "Url": "https://myeventhub-ns.servicebus.windows.net/secondarydiageventhub",
                        "SharedAccessKeyName": "SendRule",
                        "usePublisherId": false
                    }
                },
                {
                    "name": "secondaryStorageAccount",
                    "StorageAccount": {
                        "name": "secondarydiagstorageaccount",
                        "endpoint": "https://core.windows.net"
                    }
                }
            ]
        }
    },
    "StorageAccount": "diagstorageaccount",
    "StorageType": "TableAndBlob"
}
```

```json
"PrivateConfig" {
    "storageAccountName": "diagstorageaccount",
    "storageAccountKey": "{base64 encoded key}",
    "storageAccountEndPoint": "https://core.windows.net",
    "storageAccountSasToken": "{sas token}",
    "EventHub": {
        "Url": "https://myeventhub-ns.servicebus.windows.net/diageventhub",
        "SharedAccessKeyName": "SendRule",
        "SharedAccessKey": "{base64 encoded key}"
    },
    "SecondaryStorageAccounts": {
        "StorageAccount": [
            {
                "name": "secondarydiagstorageaccount",
                "key": "{base64 encoded key}",
                "endpoint": "https://core.windows.net",
                "sasToken": "{sas token}"
            }
        ]
    },
    "SecondaryEventHubs": {
        "EventHub": [
            {
                "Url": "https://myeventhub-ns.servicebus.windows.net/secondarydiageventhub",
                "SharedAccessKeyName": "SendRule",
                "SharedAccessKey": "{base64 encoded key}"
            }
        ]
    }
}

```

## <a name="reading-this-page"></a>Bu sayfa okuma  
 Önceki örnekte gösterilen sırada aşağıdaki etiketler kabaca bulunmaktadır.  Tam açıklama beklediğiniz burada görmüyorsanız, sayfanın öğesi veya özniteliği için arama.  

## <a name="common-attribute-types"></a>Ortak öznitelik türleri  
 **scheduledTransferPeriod** özniteliği birkaç öğelerinde görüntülenir. Zamanlanmış aktarımları için en yakın dakika yuvarlanan depolama için arasında aralığıdır. Değer bir [XML "Süre veri türü."](http://www.w3schools.com/schema/schema_dtypes_date.asp)


## <a name="diagnosticsconfiguration-element"></a>DiagnosticsConfiguration öğesi  
 *Ağaç: Kök - DiagnosticsConfiguration*

Sürüm 1.3 eklendi.  

Tanılama yapılandırma dosyasının en üst düzey öğesi.  

**Öznitelik** xmlns - tanılama yapılandırma dosyası için XML ad alanı olan:  
http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration  


|Alt öğeler|Açıklama|  
|--------------------|-----------------|  
|**PublicConfig**|Gereklidir. Açıklama, bu sayfada başka bir yerde bakın.|  
|**PrivateConfig**|İsteğe bağlı. Açıklama, bu sayfada başka bir yerde bakın.|  
|**IsEnabled**|Boole değeri. Açıklama, bu sayfada başka bir yerde bakın.|  

## <a name="publicconfig-element"></a>PublicConfig öğesi  
 *Ağaç: Kök - DiagnosticsConfiguration - PublicConfig*

 Genel tanılama yapılandırması açıklar.  

|Alt öğeler|Açıklama|  
|--------------------|-----------------|  
|**WadCfg**|Gereklidir. Açıklama, bu sayfada başka bir yerde bakın.|  
|**StorageAccount**|Verileri depolamak için Azure Storage hesabının adı. Ayrıca bir parametre olarak Set-AzureServiceDiagnosticsExtension cmdlet'ini çalıştırırken belirtilebilir.|  
|**StorageType**|Olabilir *tablo*, *Blob*, veya *TableAndBlob*. Tablo varsayılandır. TableAndBlob seçildiğinde Tanılama verileri iki kez--her türü için bir kez yazılır.|  
|**LocalResourceDirectory**|İzleme Aracısı olay verileri depoladığı sanal makinede dizin. Aksi takdirde, ayarlanırsa, varsayılan dizini kullanılır:<br /><br /> Worker/web rolü için:`C:\Resources\<guid>\directory\<guid>.<RoleName.DiagnosticStore\`<br /><br /> Bir sanal makine için:`C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<WADVersion>\WAD<WADVersion>`<br /><br /> Gerekli öznitelikler şunlardır:<br /><br /> - **yol** -Azure tanılama tarafından kullanılmak üzere sistemde dizin.<br /><br /> - **expandEnvironment** -ortam değişkenleri yol adındaki genişletilmiş olup olmadığını denetler.|  

## <a name="wadcfg-element"></a>WadCFG öğesi  
 *Ağaç: Kök - DiagnosticsConfiguration - PublicConfig - WadCFG*
 
 Tanımlar ve telemetri verilerinin toplanmasını yapılandırır.  


## <a name="diagnosticmonitorconfiguration-element"></a>DiagnosticMonitorConfiguration öğesi 
 *Ağaç: Kök - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration*

 Gerekli 

|Öznitelikler|Açıklama|  
|----------------|-----------------|  
| **overallQuotaInMB** | Maksimum Azure tanılama tarafından toplanan Tanılama verileri çeşitli türlerde tarafından tüketilebilir yerel disk alanı miktarı. Varsayılan ayar 5120 MB'tır.<br />
|**useProxyServer** | IE ayarlarının kümesinde olarak ara sunucu ayarlarını kullanmak için Azure tanılama yapılandırın.|  

<br /> <br />

|Alt öğeler|Açıklama|  
|--------------------|-----------------|  
|**CrashDumps**|Açıklama, bu sayfada başka bir yerde bakın.|  
|**DiagnosticInfrastructureLogs**|Azure tanılama tarafından oluşturulan günlükleri koleksiyonunu etkinleştirin. Tanılama Altyapısı günlükleri, tanılama sistem sorun giderme için yararlıdır. İsteğe bağlı öznitelikleri şunlardır:<br /><br /> - **scheduledTransferLogLevelFilter** -toplanan günlüklerini en düşük önem derecesi yapılandırır.<br /><br /> - **scheduledTransferPeriod** -depolama zamanlanmış aktarımları arasındaki aralığı yakın dakika yuvarlanan. Değer bir [XML "Süre veri türü."](http://www.w3schools.com/schema/schema_dtypes_date.asp) |  
|**Dizinleri**|Açıklama, bu sayfada başka bir yerde bakın.|  
|**EtwProviders**|Açıklama, bu sayfada başka bir yerde bakın.|  
|**Ölçümler**|Açıklama, bu sayfada başka bir yerde bakın.|  
|**Performans sayaçları**|Açıklama, bu sayfada başka bir yerde bakın.|  
|**WindowsEventLog**|Açıklama, bu sayfada başka bir yerde bakın.| 
|**DockerSources**|Açıklama, bu sayfada başka bir yerde bakın. | 



## <a name="crashdumps-element"></a>CrashDumps öğesi  
 *Ağaç: Kök - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - CrashDumps*
 
 Kilitlenme bilgi dökümleri koleksiyonunu etkinleştirin.  

|Öznitelikler|Açıklama|  
|----------------|-----------------|  
|**kapsayıcı adı**|İsteğe bağlı. Kilitlenme bilgi dökümleri depolamak için kullanılacak Azure depolama hesabınızdaki blob kapsayıcısının adı.|  
|**crashDumpType**|İsteğe bağlı.  Mini ya da tam kilitlenme dökümleri toplamak için Azure tanılama yapılandırır.|  
|**directoryQuotaPercentage**|İsteğe bağlı.  Yüzdesini yapılandırır **overallQuotaInMB** VM kilitlenme dökümleri için ayrılmış olmalıdır.|  

|Alt öğeler|Açıklama|  
|--------------------|-----------------|  
|**CrashDumpConfiguration**|Gereklidir. Her işlem için yapılandırma değerlerini tanımlar.<br /><br /> Aşağıdaki öznitelik de gereklidir:<br /><br /> **işlemadı** -adı işlemi için kilitlenme bilgilerini toplamak için Azure tanılama istiyor.|  

## <a name="directories-element"></a>Dizinleri öğesi 
 *Ağaç: Kök - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - dizinler*

 Bir dizin, IIS başarısız erişim isteği günlükleri ve/veya IIS günlüklerini içeriğini koleksiyonunu sağlar.  

 İsteğe bağlı **scheduledTransferPeriod** özniteliği. Daha önce açıklama konusuna bakın.  

|Alt öğeler|Açıklama|  
|--------------------|-----------------|  
|**IISLogs**|Bu öğe yapılandırmada dahil olmak üzere, IIS günlüklerini koleksiyonunu sağlar:<br /><br /> **kapsayıcı adı** -IIS günlükleri depolamak için kullanılacak Azure depolama hesabınızdaki blob kapsayıcısının adı.|   
|**FailedRequestLogs**|Bu öğe yapılandırmada dahil olmak üzere bir IIS site veya uygulama başarısız istekler hakkında günlükleri koleksiyonunu sağlar. İzleme seçenekleri altında da etkinleştirmeniz gerekir **sistem. Web sunucusu** içinde **Web.config**.|  
|**Veri kaynakları**|İzlemek için dizinlerin listesi.| 




## <a name="datasources-element"></a>Veri kaynakları öğesi  
 *Ağaç: Kök - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - dizinlere - veri kaynakları*

 İzlemek için dizinlerin listesi.  

|Alt öğeler|Açıklama|  
|--------------------|-----------------|  
|**DirectoryConfiguration**|Gereklidir. Gerekli öznitelik:<br /><br /> **kapsayıcı adı** -Azure depolama alanınızı blob kapsayıcısında adını, günlük dosyalarını depolamak için kullanılacak hesap.|  





## <a name="directoryconfiguration-element"></a>DirectoryConfiguration öğesi  
 *Ağaç: Kök - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - dizinlere - veri kaynakları - DirectoryConfiguration*

 Ya da içerebilir **mutlak** veya **LocalResource** öğesi ikisini birden belirtmeyin.  

|Alt öğeler|Açıklama|  
|--------------------|-----------------|  
|**Mutlak**|İzlemek için dizinine mutlak yolu. Aşağıdaki öznitelikler gereklidir:<br /><br /> - **Yol** -izlemek için dizinine mutlak yolu.<br /><br /> - **expandEnvironment** -Path ortam değişkenleri genişletilmiş olup olmadığını yapılandırır.|  
|**LocalResource**|İzlemek için bir yerel kaynağı göreli yolu. Gerekli öznitelikler şunlardır:<br /><br /> - **Ad** -izlemek için dizinini içeren yerel kaynağı<br /><br /> - **relativePath** -izlemek için dizin içeren adı göreli yolu|  



## <a name="etwproviders-element"></a>EtwProviders öğesi  
 *Ağaç: Kök - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - EtwProviders*

 EventSource ETW olayları koleksiyonu yapılandırır ve/veya ETW bildirim dayalı sağlayıcıları.  

|Alt öğeler|Açıklama|  
|--------------------|-----------------|  
|**EtwEventSourceProviderConfiguration**|Koleksiyon üretilen olayların yapılandırır [EventSource sınıfı](http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource\(v=vs.110\).aspx). Gerekli öznitelik:<br /><br /> **Sağlayıcı** -EventSource olay sınıfı adı.<br /><br /> İsteğe bağlı öznitelikleri şunlardır:<br /><br /> - **scheduledTransferLogLevelFilter** -depolama hesabınıza aktarmak için en düşük önem düzeyi.<br /><br /> - **scheduledTransferPeriod** -depolama zamanlanmış aktarımları arasındaki aralığı yakın dakika yuvarlanan. Değer bir [XML "Süre veri türü."](http://www.w3schools.com/schema/schema_dtypes_date.asp) |  
|**EtwManifestProviderConfiguration**|Gerekli öznitelik:<br /><br /> **Sağlayıcı** -GUID Olay sağlayıcısı<br /><br /> İsteğe bağlı öznitelikleri şunlardır:<br /><br /> - **scheduledTransferLogLevelFilter** -depolama hesabınıza aktarmak için en düşük önem düzeyi.<br /><br /> - **scheduledTransferPeriod** -depolama zamanlanmış aktarımları arasındaki aralığı yakın dakika yuvarlanan. Değer bir [XML "Süre veri türü."](http://www.w3schools.com/schema/schema_dtypes_date.asp) |  



## <a name="etweventsourceproviderconfiguration-element"></a>EtwEventSourceProviderConfiguration öğesi  
 *Ağaç: Kök - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - EtwProviders - EtwEventSourceProviderConfiguration*

 Koleksiyon üretilen olayların yapılandırır [EventSource sınıfı](http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource\(v=vs.110\).aspx).  

|Alt öğeler|Açıklama|  
|--------------------|-----------------|  
|**DefaultEvents**|İsteğe bağlı öznitelik:<br/><br/> **eventDestination** -olayları depolamak için tablonun adı|  
|**Olay**|Gerekli öznitelik:<br /><br /> **Kimliği** -olay kimliği.<br /><br /> İsteğe bağlı öznitelik:<br /><br /> **eventDestination** -olayları depolamak için tablonun adı|  



## <a name="etwmanifestproviderconfiguration-element"></a>EtwManifestProviderConfiguration öğesi  
 *Ağaç: Kök - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - EtwProviders - EtwManifestProviderConfiguration*

|Alt öğeler|Açıklama|  
|--------------------|-----------------|  
|**DefaultEvents**|İsteğe bağlı öznitelik:<br /><br /> **eventDestination** -olayları depolamak için tablonun adı|  
|**Olay**|Gerekli öznitelik:<br /><br /> **Kimliği** -olay kimliği.<br /><br /> İsteğe bağlı öznitelik:<br /><br /> **eventDestination** -olayları depolamak için tablonun adı|  



## <a name="metrics-element"></a>Ölçümleri öğesi  
 *Ağaç: Kök - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - ölçümleri*

 Hızlı sorguları için en iyi hale getirilmiş bir performans sayacı tablo oluşturmanıza olanak sağlar. Tanımlanan her performans sayacı **performans sayaçları** öğesi performans sayacı tablo yanı sıra ölçüm tablosunda depolanır.  

 **ResourceId** özniteliği gereklidir.  Sanal makine veya sanal makine ölçek kümesi için Azure tanılama dağıtıyorsanız kaynak kimliği. Alma **ResourceId** gelen [Azure portal](https://portal.azure.com). Seçin **Gözat** -> **kaynak grupları** -> **< adı\>**. Tıklatın **özellikleri** döşeme ve değerini kopyalayın **kimliği** alan.  

|Alt öğeler|Açıklama|  
|--------------------|-----------------|  
|**MetricAggregation**|Gerekli öznitelik:<br /><br /> **scheduledTransferPeriod** -depolama zamanlanmış aktarımları arasındaki aralığı yakın dakika yuvarlanan. Değer bir [XML "Süre veri türü."](http://www.w3schools.com/schema/schema_dtypes_date.asp) |  



## <a name="performancecounters-element"></a>PerformanceCounters öğesi  
 *Ağaç: Kök - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - performans sayaçları*

 Performans sayaçları koleksiyonunu sağlar.  

 İsteğe bağlı öznitelik:  

 İsteğe bağlı **scheduledTransferPeriod** özniteliği. Daha önce açıklama konusuna bakın.

|Alt öğe|Açıklama|  
|-------------------|-----------------|  
|**PerformanceCounterConfiguration**|Aşağıdaki öznitelikler gereklidir:<br /><br /> - **counterSpecifier** -performans sayacının adı. Örneğin, `\Processor(_Total)\% Processor Time`. Ana bilgisayarda performans sayaçları listesini almak için komutu çalıştırmak `typeperf`.<br /><br /> - **sampleRate** -sayaç ne sıklıkta örneklenen.<br /><br /> İsteğe bağlı öznitelik:<br /><br /> **Birim** -sayaç ölçü birimi.|  




## <a name="windowseventlog-element"></a>WindowsEventLog öğesi
 *Ağaç: Kök - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - WindowsEventLog*
 
 Windows olay günlüklerini toplama sağlar.  

 İsteğe bağlı **scheduledTransferPeriod** özniteliği. Daha önce açıklama konusuna bakın.  

|Alt öğe|Açıklama|  
|-------------------|-----------------|  
|**Veri kaynağı**|Windows olay günlüklerini toplamak üzere. Gerekli öznitelik:<br /><br /> **ad** - toplanacak windows olayları tanımlayan XPath sorgusu. Örneğin:<br /><br /> `Application!*[System[(Level <=3)]], System!*[System[(Level <=3)]], System!*[System[Provider[@Name='Microsoft Antimalware']]], Security!*[System[(Level <= 3)]`<br /><br /> Tüm olaylarını toplamak için belirtin "*"|  




## <a name="logs-element"></a>Günlükleri öğesi  
 *Ağaç: Kök - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - günlükleri*

 Sürüm 1.0 ve 1.1 sunar. 1.2 ile eksik. 1.3 geri eklendi.  

 Temel Azure günlükleri için arabellek yapılandırmasını tanımlar.  

|Öznitelik|Tür|Açıklama|  
|---------------|----------|-----------------|  
|**bufferQuotaInMB**|**unsignedInt**|İsteğe bağlı. Belirtilen verileri için kullanılabilir olan dosya sistemi depolaması en miktarını belirtir.<br /><br /> Varsayılan değer 0'dır.|  
|**scheduledTransferLogLevelFilterr**|**dize**|İsteğe bağlı. Aktarılır günlük girişlerini en düşük önem derecesini belirtir. Varsayılan değer **tanımlanmamış**, tüm günlükleri aktarır. Diğer olası değerler (en az bilgilere sırasına göre) **ayrıntılı**, **bilgi**, **uyarı**, **hata**, ve **kritik**.|  
|**scheduledTransferPeriod**|**süre**|İsteğe bağlı. Zamanlanmış aktardığı veriler, en yakın dakika yuvarlanan arasındaki aralığı belirtir.<br /><br /> PT0S varsayılandır.|  
|**İç havuzlar** 1.5 inç eklendi|**dize**|İsteğe bağlı. Ayrıca Tanılama verileri göndermek için bir havuz konuma noktaları. Örneğin, Application Insights.|  

## <a name="dockersources"></a>DockerSources
 *Ağaç: Kök - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - DockerSources*

 İçinde 1.9 eklendi.

|Öğe adı|Açıklama|  
|------------------|-----------------|  
|**İstatistikleri**|Docker kapsayıcıları istatistikleri toplamak için sisteme söyler|  

## <a name="sinksconfig-element"></a>SinksConfig öğesi  
 *Ağaç: Kök - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig*

 Tanılama verileri ve konumların ile ilişkili yapılandırma göndermek için konumları listesi.  

|Öğe adı|Açıklama|  
|------------------|-----------------|  
|**Havuz**|Açıklama, bu sayfada başka bir yerde bakın.|  

## <a name="sink-element"></a>Öğe havuzu
 *Ağaç: Kök - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig - havuz*

 Sürüm 1.5 eklendi.  

 Tanılama veri göndermesini konumlarını tanımlar. Örneğin, Application Insights hizmeti.  

|Öznitelik|Tür|Açıklama|  
|---------------|----------|-----------------|  
|**adı**|Dize|Sinkname tanımlayan bir dize.|  

|Öğesi|Tür|Açıklama|  
|-------------|----------|-----------------|  
|**Application Insights**|Dize|Yalnızca veri Application Insights'a gönderirken kullanılır. Erişiminiz etkin bir Application Insights hesabı izleme anahtarını içerir.|  
|**Kanalları**|Dize|Her ek, akış filtrelemesi için bir tane|  

## <a name="channels-element"></a>Kanallar öğesi  
 *Ağaç: Kök - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig - havuz - kanalları*

 Sürüm 1.5 eklendi.  

 Bir havuz geçirme günlük veri akışları için filtreleri tanımlar.  

|Öğesi|Tür|Açıklama|  
|-------------|----------|-----------------|  
|**Kanal**|Dize|Açıklama, bu sayfada başka bir yerde bakın.|  

## <a name="channel-element"></a>Kanal öğesi
 *Ağaç: Kök - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig - havuz - kanalları - kanal*

 Sürüm 1.5 eklendi.  

 Tanılama veri göndermesini konumlarını tanımlar. Örneğin, Application Insights hizmeti.  

|Öznitelikler|Tür|Açıklama|  
|----------------|----------|-----------------|  
|**logLevel**|**dize**|Aktarılır günlük girişlerini en düşük önem derecesini belirtir. Varsayılan değer **tanımlanmamış**, tüm günlükleri aktarır. Diğer olası değerler (en az bilgilere sırasına göre) **ayrıntılı**, **bilgi**, **uyarı**, **hata**, ve **kritik**.|  
|**adı**|**dize**|Kanal başvurmak için benzersiz bir ad|  


## <a name="privateconfig-element"></a>PrivateConfig öğesi 
 *Ağaç: Kök - DiagnosticsConfiguration - PrivateConfig*

 Sürüm 1.3 eklendi.  

 İsteğe bağlı  

 Depolama hesabı (adı, anahtar ve uç nokta) özel ayrıntılarını depolar. Bu bilgiler, sanal makineye gönderilir ancak ondan alınamıyor.  

|Alt öğeler|Açıklama|  
|--------------------|-----------------|  
|**StorageAccount**|Depolama hesabı. Aşağıdaki öznitelikler gereklidir<br /><br /> - **ad** -depolama hesabının adı.<br /><br /> - **anahtar** -depolama hesabı anahtarı.<br /><br /> - **uç nokta** -depolama hesabınıza erişmeniz için uç nokta. <br /><br /> -**sasToken** (özel yapılandırma dosyasında bir SAS belirteci bir depolama hesabı anahtarı yerine belirtebilirsiniz 1.8.1)-eklenir. Depolama hesabı anahtarı sağladıysanız, göz ardı edilir. <br />SAS belirteci gereksinimleri: <br />-Hesap SAS belirteci yalnızca destekler <br />- *b*, *t* hizmet türleri gereklidir. <br /> - *bir*, *c*, *u*, *w* izinleri gereklidir. <br /> - *c*, *o* kaynak türleri gereklidir. <br /> -Yalnızca HTTPS protokolünü destekler <br /> -Başlangıç ve sona erme saati geçerli olmalıdır.|  


## <a name="isenabled-element"></a>IsEnabled öğesi  
 *Ağaç: Kök - DiagnosticsConfiguration - IsEnabled*

 Boole değeri. Kullanım `true` Tanılama'yı etkinleştirmek için veya `false` tanılama devre dışı bırakmak için.
