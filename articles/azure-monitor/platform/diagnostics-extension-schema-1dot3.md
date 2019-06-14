---
title: Azure tanılama uzantısı 1.3 ve üzeri yapılandırma şeması
description: Microsoft Azure SDK 2.4 ve daha sonra bir parçası olarak, şema sürümü 1.3 ve üzeri Azure tanılama birlikte gelir.
services: azure-monitor
author: rboucher
ms.service: azure-monitor
ms.devlang: dotnet
ms.topic: reference
ms.date: 09/20/2018
ms.author: robb
ms.subservice: diagnostic-extension
ms.openlocfilehash: fa03017c35c76d986139eeee00eea8a9b4a00e62
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60238065"
---
# <a name="azure-diagnostics-13-and-later-configuration-schema"></a>Azure tanılama 1.3 ve üzeri yapılandırma şeması
> [!NOTE]
> Azure tanılama uzantısını performans sayaçları ve diğer istatistikleri toplamak için kullanılan bileşendir:
> - Azure sanal makineleri
> - Sanal Makine Ölçek Kümeleri
> - Service Fabric
> - Cloud Services
> - Ağ Güvenlik Grupları
>
> Bu sayfada, yalnızca bu hizmetlerden biri kullanıyorsanız geçerlidir.

Bu sayfa, sürüm 1.3 ve daha yeni (Azure SDK 2.4 ve daha yeni) için geçerlidir. Hangi sürüm eklendikleri göstermek için yeni yapılandırma bölümlerini bırakılır.  

Burada açıklanan yapılandırma dosyası, Tanılama izleme başladığında tanılama yapılandırma ayarları ayarlamak için kullanılır.  

Uzantı, Application Insights ve Log Analytics içeren Azure İzleyici gibi diğer Microsoft tanılama ürünleri ile birlikte kullanılır.



Genel yapılandırma dosyası şeması tanımı aşağıdaki PowerShell komutunu çalıştırarak yükleyin:  

```powershell  
(Get-AzureServiceAvailableExtension -ExtensionName 'PaaSDiagnostics' -ProviderNamespace 'Microsoft.Azure.Diagnostics').PublicConfigurationSchema | Out-File –Encoding utf8 -FilePath 'C:\temp\WadConfig.xsd'  
```  

Azure tanılama kullanma hakkında daha fazla bilgi için bkz. [Azure tanılama uzantısını](diagnostics-extension-overview.md).  

## <a name="example-of-the-diagnostics-configuration-file"></a>Tanılama yapılandırma dosyası örneği  
 Aşağıdaki örnek, bir normal tanılama yapılandırma dosyası gösterir:  

```xml  
<?xml version="1.0" encoding="utf-8"?>  
<DiagnosticsConfiguration  xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">   
  <PublicConfig>  
    <WadCfg>  
      <DiagnosticMonitorConfiguration overallQuotaInMB="10000">  

        <PerformanceCounters scheduledTransferPeriod="PT1M", sinks="AzureMonitorSink">  
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
        <Sink name="AzureMonitorSink">
            <AzureMonitor> <!-- Added in 1.11 -->
                <resourceId>{insert resourceId}</ResourceId> <!-- Parameter only needed for classic VMs and Classic Cloud Services, exclude VMSS and Resource Manager VMs-->
                <Region>{insert Azure region of resource}</Region> <!-- Parameter only needed for classic VMs and Classic Cloud Services, exclude VMSS and Resource Manager VMs -->
            </AzureMonitor>
        </Sink>
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

    <AzureMonitorAccount>
        <ServicePrincipalMeta> <!-- Added in 1.11; only needed for classic VMs and Classic cloud services -->
            <PrincipalId>{Insert service principal clientId}</PrincipalId>
            <Secret>{Insert service principal client secret}</Secret>
        </ServicePrincipalMeta>
    </AzureMonitorAccount>

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
> [!NOTE]
> Genel yapılandırma Azure İzleyici havuz tanımı, iki özellik, ResourceId ve bölge vardır. Yalnızca bunlar Klasik Vm'leri ve klasik bulut Hizmetleri için gereklidir. Bu özellikleri için Resource Manager sanal makinelerinde kullanılmamalıdır veya sanal makine ölçek kümeleri.
> Bir sorumlu kimliği ve parolası geçen Azure Monitor havuzu için ek bir özel yapılandırma öğesi yoktur. Bu, yalnızca klasik Vm'leri ve klasik bulut Hizmetleri için gereklidir. Resource Manager Vm'leri ve Azure İzleyici VMSS için özel bir yapılandırma öğesi tanımı tutulabilir.
>

Önceki XML yapılandırma dosyasını denk JSON.

Çoğu json kullanım durumda farklı bir değişken geçirilen çünkü PublicConfig ve PrivateConfig ayrılır. Bu durumlar: Resource Manager şablonları, PowerShell ve Visual Studio sanal makine ölçek kümesi.

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
                "sinks": "AzureMonitorSink",
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
                    "name": "AzureMonitorSink",
                    "AzureMonitor":
                    {
                        "ResourceId": "{insert resourceId if a classic VM or cloud service, else property not needed}",
                        "Region": "{insert Azure region of resource if a classic VM or cloud service, else property not needed}"
                    }
                },
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

> [!NOTE]
> Genel yapılandırma Azure İzleyici havuz tanımı, iki özellik, ResourceId ve bölge vardır. Yalnızca bunlar Klasik Vm'leri ve klasik bulut Hizmetleri için gereklidir.
> Bu özellikleri için Resource Manager sanal makinelerinde kullanılmamalıdır veya sanal makine ölçek kümeleri.
>

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
    "AzureMonitorAccount": {
        "ServicePrincipalMeta": {
            "PrincipalId": "{Insert service principal client Id}",
            "Secret": "{Insert service principal client secret}"
        }
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

> [!NOTE]
> Bir sorumlu kimliği ve parolası geçen Azure Monitor havuzu için ek bir özel yapılandırma öğesi yoktur. Bu, yalnızca klasik Vm'leri ve klasik bulut Hizmetleri için gereklidir. Resource Manager Vm'leri ve Azure İzleyici VMSS için özel bir yapılandırma öğesi tanımı tutulabilir.
>


## <a name="reading-this-page"></a>Bu sayfa okuma  
 Aşağıdaki etiketler kabaca önceki örnekte gösterilen sırada aşağıdaki gibidir.  Tam açıklamasını burada beklediğiniz görmüyorsanız, sayfanın öğesi veya özniteliği için'ı arayın.  

## <a name="common-attribute-types"></a>Genel öznitelik türleri  
 **scheduledTransferPeriod** özniteliği çeşitli öğeleri görünür. En yakın dakikaya yuvarlanır depolama zamanlanmış aktarmalarıyla arasında aralığıdır. Değer bir [XML "Süresi veri türü."](https://www.w3schools.com/xml/schema_dtypes_date.asp)


## <a name="diagnosticsconfiguration-element"></a>DiagnosticsConfiguration öğesi  
 *Ağaç: Kök - DiagnosticsConfiguration*

Sürüm 1.3 eklendi.  

En üst düzey öğesi tanılama yapılandırma dosyası.  

**Öznitelik** xmlns - tanılama yapılandırma dosyası için XML ad alanıdır:  
http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration  


|Alt öğeleri|Açıklama|  
|--------------------|-----------------|  
|**PublicConfig**|Gereklidir. Açıklama, başka bir yerde şu sayfada görürsünüz.|  
|**PrivateConfig**|İsteğe bağlı. Açıklama, başka bir yerde şu sayfada görürsünüz.|  
|**IsEnabled**|Boole değeri. Açıklama, başka bir yerde şu sayfada görürsünüz.|  

## <a name="publicconfig-element"></a>PublicConfig öğesi  
 *Ağaç: Kök - DiagnosticsConfiguration - PublicConfig*

 Genel Tanılama yapılandırmasını açıklar.  

|Alt öğeleri|Açıklama|  
|--------------------|-----------------|  
|**WadCfg**|Gereklidir. Açıklama, başka bir yerde şu sayfada görürsünüz.|  
|**Depolama hesabı**|Verileri depolamak için Azure depolama hesabı adı. Ayrıca kümesi AzureServiceDiagnosticsExtension cmdlet'ini çalıştırırken bir parametre olarak belirtilebilir.|  
|**StorageType**|Olabilir *tablo*, *Blob*, veya *TableAndBlob*. Tablo varsayılandır. TableAndBlob seçildiğinde, Tanılama verileri iki kez--her tür için bir kez yazılır.|  
|**LocalResourceDirectory**|Monitoring Agent olay verilerini depoladığı sanal makinesinde dizin. Aksi takdirde, küme, varsayılan dizin kullanılır:<br /><br /> Çalışan/web rolü için: `C:\Resources\<guid>\directory\<guid>.<RoleName.DiagnosticStore\`<br /><br /> Bir sanal makine için: `C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<WADVersion>\WAD<WADVersion>`<br /><br /> Gerekli öznitelik şunlardır:<br /><br /> - **yol** -Azure tanılama tarafından kullanılmak üzere sistemde dizini.<br /><br /> - **expandEnvironment** -ortam değişkenlerini yol adına genişletilir olup olmadığını denetler.|  

## <a name="wadcfg-element"></a>WadCFG Element  
 *Ağaç: Kök - DiagnosticsConfiguration - PublicConfig - WadCFG*

 Tanımlar ve telemetri verilerini toplanacak yapılandırır.  


## <a name="diagnosticmonitorconfiguration-element"></a>DiagnosticMonitorConfiguration öğesi
 *Ağaç: Kök - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration*

 Gerekli

|Öznitelikler|Açıklama|  
|----------------|-----------------|  
| **overallQuotaInMB** | En çok çeşitli Azure tanılama tarafından toplanan Tanılama verileri tarafından tüketilebilir yerel disk alanı miktarı. 4096 MB varsayılan ayardır.<br />
|**useProxyServer** | IE ayarlarında belirlenen Ara sunucu ayarlarını kullanmak için Azure Tanılama'yı yapılandırın.|
|**havuzlar** | 1\.5 eklendi. İsteğe bağlı. Ayrıca havuzlarını destekleyen tüm alt öğeleri için Tanılama verileri göndermek için bir havuz konuma işaret eder. Application Insights veya olay hub'ları havuz örnek verilebilir.|  


<br /> <br />

|Alt öğeleri|Açıklama|  
|--------------------|-----------------|  
|**CrashDumps**|Açıklama, başka bir yerde şu sayfada görürsünüz.|  
|**DiagnosticInfrastructureLogs**|Azure tanılama tarafından oluşturulan günlüklerin toplanmasını etkinleştirin. Tanılama Altyapısı günlükleri, tanılama sistem gidermek için kullanışlıdır. İsteğe bağlı öznitelikleri şunlardır:<br /><br /> - **scheduledTransferLogLevelFilter** -toplanan günlüklerde en düşük önem derecesi yapılandırır.<br /><br /> - **scheduledTransferPeriod** -zamanlanmış aktarmalarıyla depolama arasındaki aralığı ve en yakın dakikaya yuvarlanır. Değer bir [XML "Süresi veri türü."](https://www.w3schools.com/xml/schema_dtypes_date.asp) |  
|**Dizinler**|Açıklama, başka bir yerde şu sayfada görürsünüz.|  
|**EtwProviders**|Açıklama, başka bir yerde şu sayfada görürsünüz.|  
|**Ölçümler**|Açıklama, başka bir yerde şu sayfada görürsünüz.|  
|**PerformanceCounters**|Açıklama, başka bir yerde şu sayfada görürsünüz.|  
|**WindowsEventLog**|Açıklama, başka bir yerde şu sayfada görürsünüz.|
|**DockerSources**|Açıklama, başka bir yerde şu sayfada görürsünüz. |



## <a name="crashdumps-element"></a>CrashDumps öğesi  
 *Ağaç: -DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - CrashDumps kök*

 Kilitlenme bilgi dökümleri koleksiyonunu etkinleştirin.  

|Öznitelikler|Açıklama|  
|----------------|-----------------|  
|**kapsayıcı adı**|İsteğe bağlı. Kilitlenme bilgi dökümleri depolamak için kullanılacak Azure depolama hesabınızdaki blob kapsayıcısının adı.|  
|**crashDumpType**|İsteğe bağlı.  Mini veya tam kilitlenme dökümleri toplamak için Azure Tanılama'yı yapılandırır.|  
|**directoryQuotaPercentage**|İsteğe bağlı.  Yüzdesini yapılandırır **overallQuotaInMB** VM'de kilitlenme dökümleri için rezerve edilecek.|  

|Alt öğeleri|Açıklama|  
|--------------------|-----------------|  
|**CrashDumpConfiguration**|Gereklidir. Her işlem için yapılandırma değerlerini tanımlar.<br /><br /> Aşağıdaki özniteliği de gereklidir:<br /><br /> **processName** -adı işlemi için kilitlenme bilgi dökümü toplamak için Azure tanılama istiyor.|  

## <a name="directories-element"></a>Dizinleri öğesi
 *Ağaç: -DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - dizinleri kök*

 Bir dizin, IIS başarısız istek günlüklerine erişme ve/veya IIS günlükler içeriğini toplanmasını etkinleştirir.  

 İsteğe bağlı **scheduledTransferPeriod** özniteliği. Önceki Açıklama konusuna bakın.  

|Alt öğeleri|Açıklama|  
|--------------------|-----------------|  
|**IISLogs**|Bu öğe yapılandırmada dahil olmak üzere IIS günlükler koleksiyonunu sağlar:<br /><br /> **containerName** -Azure depolama hesabınızda IIS günlüklerini depolamak için kullanılacak blob kapsayıcısının adı.|   
|**FailedRequestLogs**|Bu öğe yapılandırmada dahil olmak üzere bir IIS sitesi veya uygulama başarısız istekler hakkında günlüklerin toplanmasını sağlar. İzleme seçenekleri altında da etkinleştirmeniz gerekir **sistem. Web sunucusu** içinde **Web.config**.|  
|**Veri kaynakları**|İzlenecek dizinler bir listesi.|




## <a name="datasources-element"></a>Veri kaynakları öğesi  
 *Ağaç: Kök - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - dizinleri - veri kaynakları*

 İzlenecek dizinler bir listesi.  

|Alt öğeleri|Açıklama|  
|--------------------|-----------------|  
|**DirectoryConfiguration**|Gereklidir. Gerekli öznitelik:<br /><br /> **containerName** -günlük dosyalarını depolamak için kullanılacak hesap, Azure Depolama'daki bir blob kapsayıcısının adı.|  





## <a name="directoryconfiguration-element"></a>DirectoryConfiguration öğesi  
 *Ağaç: Kök - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - dizinleri - veri kaynakları - DirectoryConfiguration*

 Ya da içerebilir **mutlak** veya **LocalResource** öğe ancak ikisine birden değil.  

|Alt öğeleri|Açıklama|  
|--------------------|-----------------|  
|**Mutlak**|İzlemek için dizinin mutlak yolu. Aşağıdaki öznitelikler gereklidir:<br /><br /> - **Yol** -izlemek için dizinin mutlak yolu.<br /><br /> - **expandEnvironment** -yolunda ortam değişkenleri genişletilmiş paylaşamayacağını yapılandırır.|  
|**LocalResource**|İzlemek için yerel kaynak göreli yol. Gerekli öznitelik şunlardır:<br /><br /> - **Ad** -izlemek için dizin içeren yerel kaynağı<br /><br /> - **relativePath** -izlemek için dizin içeren adı göreli yol|  



## <a name="etwproviders-element"></a>EtwProviders Element  
 *Ağaç: -DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - EtwProviders kök*

 EventSource ETW olayları toplamayı yapılandırır ve/veya ETW bildirim alarak sağlayıcıları.  

|Alt öğeleri|Açıklama|  
|--------------------|-----------------|  
|**EtwEventSourceProviderConfiguration**|Oluşturulan olayları toplamayı yapılandırır [EventSource sınıfı](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource\(v=vs.110\).aspx). Gerekli öznitelik:<br /><br /> **Sağlayıcı** -EventSource olay sınıfı adı.<br /><br /> İsteğe bağlı öznitelikleri şunlardır:<br /><br /> - **scheduledTransferLogLevelFilter** -depolama hesabınıza aktarmak için en düşük önem düzeyi.<br /><br /> - **scheduledTransferPeriod** -zamanlanmış aktarmalarıyla depolama arasındaki aralığı ve en yakın dakikaya yuvarlanır. Değer bir [XML "Süresi veri türü."](https://www.w3schools.com/xml/schema_dtypes_date.asp) |  
|**EtwManifestProviderConfiguration**|Gerekli öznitelik:<br /><br /> **Sağlayıcı** -Olay sağlayıcısı GUID<br /><br /> İsteğe bağlı öznitelikleri şunlardır:<br /><br /> - **scheduledTransferLogLevelFilter** -depolama hesabınıza aktarmak için en düşük önem düzeyi.<br /><br /> - **scheduledTransferPeriod** -zamanlanmış aktarmalarıyla depolama arasındaki aralığı ve en yakın dakikaya yuvarlanır. Değer bir [XML "Süresi veri türü."](https://www.w3schools.com/xml/schema_dtypes_date.asp) |  



## <a name="etweventsourceproviderconfiguration-element"></a>EtwEventSourceProviderConfiguration Element  
 *Ağaç: -DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - EtwEventSourceProviderConfiguration EtwProviders - kök*

 Oluşturulan olayları toplamayı yapılandırır [EventSource sınıfı](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource\(v=vs.110\).aspx).  

|Alt öğeleri|Açıklama|  
|--------------------|-----------------|  
|**DefaultEvents**|İsteğe bağlı öznitelik:<br/><br/> **eventDestination** -olayları depolamak için bir tablonun adı|  
|**Olay**|Gerekli öznitelik:<br /><br /> **Kimliği** -etkinliğin kimliği.<br /><br /> İsteğe bağlı öznitelik:<br /><br /> **eventDestination** -olayları depolamak için bir tablonun adı|  



## <a name="etwmanifestproviderconfiguration-element"></a>EtwManifestProviderConfiguration Element  
 *Ağaç: Kök - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - EtwProviders - EtwManifestProviderConfiguration*

|Alt öğeleri|Açıklama|  
|--------------------|-----------------|  
|**DefaultEvents**|İsteğe bağlı öznitelik:<br /><br /> **eventDestination** -olayları depolamak için bir tablonun adı|  
|**Olay**|Gerekli öznitelik:<br /><br /> **Kimliği** -etkinliğin kimliği.<br /><br /> İsteğe bağlı öznitelik:<br /><br /> **eventDestination** -olayları depolamak için bir tablonun adı|  



## <a name="metrics-element"></a>Ölçümleri öğesi  
 *Ağaç: Kök - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - ölçümler*

 Hızlı sorgular için optimize edilmiş performans sayacı tablo oluşturmanıza olanak sağlar. Tanımlanan her performans sayacı **PerformanceCounters** öğesi performans sayacı tablonun yanı sıra ölçüm tablosunda depolanır.  

 **ResourceId** özniteliği gereklidir.  Azure tanılama dağıttığınız sanal makine veya sanal makine ölçek kümesi kaynak kimliği. Alma **ResourceId** gelen [Azure portalında](https://portal.azure.com). Seçin **Gözat** -> **kaynak grupları** ->  **< adı\>** . Tıklayın **özellikleri** kutucuğuna ve değeri Şuradan Kopyala: **kimliği** alan.  

|Alt öğeleri|Açıklama|  
|--------------------|-----------------|  
|**MetricAggregation**|Gerekli öznitelik:<br /><br /> **scheduledTransferPeriod** -zamanlanmış aktarmalarıyla depolama arasındaki aralığı ve en yakın dakikaya yuvarlanır. Değer bir [XML "Süresi veri türü."](https://www.w3schools.com/xml/schema_dtypes_date.asp) |  



## <a name="performancecounters-element"></a>PerformanceCounters öğesi  
 *Ağaç: -DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - PerformanceCounters kök*

 Performans sayaçlarını toplamayı etkinleştirir.  

 İsteğe bağlı öznitelik:  

 İsteğe bağlı **scheduledTransferPeriod** özniteliği. Önceki Açıklama konusuna bakın.

|Alt öğe|Açıklama|  
|-------------------|-----------------|  
|**PerformanceCounterConfiguration**|Aşağıdaki öznitelikler gereklidir:<br /><br /> - **counterSpecifier** -performans sayacının adı. Örneğin, `\Processor(_Total)\% Processor Time`. Konağınız üzerinde performans sayaçları listesini almak için komutu çalıştırmak `typeperf`.<br /><br /> - **sampleRate** -sayaç ne sıklıkta örneklenir.<br /><br /> İsteğe bağlı öznitelik:<br /><br /> **Birim** -sayacın ölçü birimi.|
|**havuzlar** | 1\.5 eklendi. İsteğe bağlı. Bir havuzu de Tanılama verileri gönder konumuna işaret eder. Örneğin, Azure İzleyici ya da olay hub'ları.|    




## <a name="windowseventlog-element"></a>WindowsEventLog öğesi
 *Ağaç: -DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - WindowsEventLog kök*

 Windows olay günlükleri, koleksiyonunu etkinleştirir.  

 İsteğe bağlı **scheduledTransferPeriod** özniteliği. Önceki Açıklama konusuna bakın.  

|Alt öğe|Açıklama|  
|-------------------|-----------------|  
|**Veri kaynağı**|Windows olay günlüklerini toplamak üzere. Gerekli öznitelik:<br /><br /> **ad** - Tahsil edilecek windows olayları tanımlayan XPath sorgusu. Örneğin:<br /><br /> `Application!*[System[(Level <=3)]], System!*[System[(Level <=3)]], System!*[System[Provider[@Name='Microsoft Antimalware']]], Security!*[System[(Level <= 3)]`<br /><br /> Tüm olaylarını toplamak için belirtin "*"|  




## <a name="logs-element"></a>Günlükleri öğesi  
 *Ağaç: Kök - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - günlükleri*

 Sürüm 1.0 ve 1.1 sunar. 1\.2 ile eksik. 1\.3 geri eklendi.  

 Temel Azure günlükleri arabellek yapılandırmasını tanımlar.  

|Öznitelik|Tür|Açıklama|  
|---------------|----------|-----------------|  
|**bufferQuotaInMB**|**unsignedInt**|İsteğe bağlı. Belirtilen veriler için kullanılabilen dosya sistemi depolama miktarını belirtir.<br /><br /> Varsayılan değer 0'dır.|  
|**scheduledTransferLogLevelFilter**|**dize**|İsteğe bağlı. Aktarılan günlük girişlerini için en düşük önem derecesini belirtir. Varsayılan değer **tanımlanmamış**, tüm günlükleri aktarır. Diğer olası değerler (en az bilgi sırasına göre) **ayrıntılı**, **bilgi**, **uyarı**, **hata**ve **Kritik**.|  
|**scheduledTransferPeriod**|**Süresi**|İsteğe bağlı. Zamanlanmış aktardığı veriler, en yakın dakikaya yuvarlanır arasındaki aralığı belirtir.<br /><br /> PT0S varsayılandır.|  
|**havuzlar** |**dize**| 1\.5 eklendi. İsteğe bağlı. Bir havuzu de Tanılama verileri gönder konumuna işaret eder. Örneğin, Application Insights veya olay hub'ları.|  

## <a name="dockersources"></a>DockerSources
 *Ağaç: -DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - DockerSources kök*

 İçinde 1.9 eklendi.

|Öğe adı|Açıklama|  
|------------------|-----------------|  
|**İstatistikleri**|Docker kapsayıcıları istatistikleri toplamak için sistemi bildirir|  

## <a name="sinksconfig-element"></a>SinksConfig öğesi  
 *Ağaç: Kök - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig*

 Tanılama verilerini ve bu konumları ile ilişkili yapılandırma göndermek için konumların listesi.  

|Öğe adı|Açıklama|  
|------------------|-----------------|  
|**Havuz**|Açıklama, başka bir yerde şu sayfada görürsünüz.|  

## <a name="sink-element"></a>Öğe havuz
 *Ağaç: Kök - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig - havuz*

 Sürüm 1.5 eklendi.  

 Tanılama verileri göndermesini konumlarını tanımlar. Örneğin, Application Insights hizmeti.  

|Öznitelik|Tür|Açıklama|  
|---------------|----------|-----------------|  
|**name**|string|Sinkname tanımlayan bir dize.|  

|Öğe|Tür|Açıklama|  
|-------------|----------|-----------------|  
|**Application Insights**|string|Yalnızca veri Application Insights'a gönderirken kullanılan. Erişiminiz olan etkin bir Application Insights hesabı için izleme anahtarı bulunur.|  
|**Kanallar**|string|Her ek, akış filtrelemesi için|  

## <a name="channels-element"></a>Kanalları öğesi  
 *Ağaç: Kök - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig - havuz - kanallar*

 Sürüm 1.5 eklendi.  

 Bir havuz arasında geçen günlük veri akışları için filtreleri tanımlar.  

|Öğe|Tür|Açıklama|  
|-------------|----------|-----------------|  
|**Kanal**|string|Açıklama, başka bir yerde şu sayfada görürsünüz.|  

## <a name="channel-element"></a>Kanal öğesi
 *Ağaç: Kök - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig - havuz - kanal - kanal*

 Sürüm 1.5 eklendi.  

 Tanılama verileri göndermesini konumlarını tanımlar. Örneğin, Application Insights hizmeti.  

|Öznitelikler|Tür|Açıklama|  
|----------------|----------|-----------------|  
|**logLevel**|**dize**|Aktarılan günlük girişlerini için en düşük önem derecesini belirtir. Varsayılan değer **tanımlanmamış**, tüm günlükleri aktarır. Diğer olası değerler (en az bilgi sırasına göre) **ayrıntılı**, **bilgi**, **uyarı**, **hata**ve **Kritik**.|  
|**name**|**dize**|Kanalın başvurmak için benzersiz bir ad|  


## <a name="privateconfig-element"></a>PrivateConfig öğesi
 *Ağaç: Kök - DiagnosticsConfiguration - PrivateConfig*

 Sürüm 1.3 eklendi.  

 İsteğe bağlı  

 Özel (adını, anahtarını ve uç nokta) depolama hesabı ayrıntılarını depolar. Bu bilgiler, sanal makineye gönderilir ancak ondan alınamıyor.  

|Alt öğeleri|Açıklama|  
|--------------------|-----------------|  
|**Depolama hesabı**|Kullanılacak depolama hesabı. Aşağıdaki öznitelikler gereklidir<br /><br /> - **ad** -depolama hesabının adı.<br /><br /> - **anahtar** -depolama hesabı anahtarı.<br /><br /> - **uç nokta** -depolama hesabına erişmek için uç nokta. <br /><br /> -**sasToken** (özel yapılandırmada bir depolama hesabı anahtarı yerine SAS belirteci belirtebilirsiniz 1.8.1)-eklenir. Sağlanırsa, depolama hesabı anahtarını göz ardı edilir. <br />SAS belirteci gereksinimleri: <br />-Yalnızca hesap SAS belirtecini destekler <br />- *b*, *t* hizmet türü gereklidir. <br /> - *bir*, *c*, *u*, *w* izinleri gereklidir. <br /> - *c*, *o* kaynak türleri gereklidir. <br /> -Yalnızca HTTPS protokolünü destekler. <br /> -Başlatmak ve sona erme saati geçerli olmalıdır.|  


## <a name="isenabled-element"></a>IsEnabled öğesi  
 *Ağaç: Kök - DiagnosticsConfiguration - IsEnabled*

 Boole değeri. Kullanım `true` tanılamayı etkinleştirmeyi veya `false` tanılama devre dışı bırakmak için.

