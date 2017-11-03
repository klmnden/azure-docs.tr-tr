---
title: "Azure tanılama 1.0 yapılandırma şeması | Microsoft Docs"
description: "YALNIZCA ilgili Azure SDK 2.4 kullanıyorsanız ve aşağıda Azure sanal makineler, sanal makine ölçek kümeleri, Service Fabric veya Bulut Hizmetleri ile."
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
ms.openlocfilehash: a8fdfb52d5091d3fc9779657737c7430fcfada51
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-diagnostics-10-configuration-schema"></a>Azure tanılama 1.0 yapılandırma şeması
> [!NOTE]
> Azure tanılama Azure sanal makineler, sanal makine ölçek kümeleri, Service Fabric ve bulut Hizmetleri performans sayaçları ve diğer istatistikleri toplamak için kullanılan bileşendir.  Bu sayfa, yalnızca bu hizmetlerden biri kullanıyorsanız geçerlidir.
>

Azure tanılama Azure monitör, Application Insights ve günlük analizi gibi diğer Microsoft tanılama ürünlerle kullanılır.

Azure tanılama yapılandırma dosyası tanılama İzleyicisi'ni başlatmak için kullanılan değerleri tanımlar. Bu dosya, tanılama İzleyicisi başlatıldığında tanılama yapılandırma ayarlarını başlatmak için kullanılır.  

 Azure tanılama yapılandırması şema dosyası varsayılan olarak, yüklü `C:\Program Files\Microsoft SDKs\Azure\.NET SDK\<version>\schemas` dizini. Değiştir `<version>` yüklü olan sürümü ile [Azure SDK'sı](http://www.windowsazure.com/develop/downloads/).  

> [!NOTE]
>  Tanılama yapılandırma dosyası, genellikle önceki başlatma işleminde toplanacak tanılama veri gerektiren başlangıç görevleri ile kullanılır. Azure tanılama kullanma hakkında daha fazla bilgi için bkz: [kullanarak Azure tanılama tarafından günlüğü verilerini toplamak](assetId:///83a91c23-5ca2-4fc9-8df3-62036c37a3d7).  

## <a name="example-of-the-diagnostics-configuration-file"></a>Tanılama yapılandırma dosyası örneği  
 Aşağıdaki örnek bir genel tanılama yapılandırma dosyası gösterir:  

```xml  
<?xml version="1.0" encoding="utf-8"?>
<DiagnosticMonitorConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration"  
      configurationChangePollInterval="PT1M"  
      overallQuotaInMB="4096">  
   <DiagnosticInfrastructureLogs bufferQuotaInMB="1024"  
      scheduledTransferLogLevelFilter="Verbose"  
      scheduledTransferPeriod="PT1M" />  
   <Logs bufferQuotaInMB="1024"  
      scheduledTransferLogLevelFilter="Verbose"  
      scheduledTransferPeriod="PT1M" />  

   <Directories bufferQuotaInMB="1024"   
      scheduledTransferPeriod="PT1M">  

      <!-- These three elements specify the special directories   
           that are set up for the log types -->  
      <CrashDumps container="wad-crash-dumps" directoryQuotaInMB="256" />  
      <FailedRequestLogs container="wad-frq" directoryQuotaInMB="256" />  
      <IISLogs container="wad-iis" directoryQuotaInMB="256" />  

      <!-- For regular directories the DataSources element is used -->  
      <DataSources>  
         <DirectoryConfiguration container="wad-panther" directoryQuotaInMB="128">  
            <!-- Absolute specifies an absolute path with optional environment expansion -->  
            <Absolute expandEnvironment="true" path="%SystemRoot%\system32\sysprep\Panther" />  
         </DirectoryConfiguration>  
         <DirectoryConfiguration container="wad-custom" directoryQuotaInMB="128">  
            <!-- LocalResource specifies a path relative to a local   
                 resource defined in the service definition -->  
            <LocalResource name="MyLoggingLocalResource" relativePath="logs" />  
         </DirectoryConfiguration>  
      </DataSources>  
   </Directories>  

   <PerformanceCounters bufferQuotaInMB="512" scheduledTransferPeriod="PT1M">  
      <!-- The counter specifier is in the same format as the imperative   
           diagnostics configuration API -->  
      <PerformanceCounterConfiguration   
         counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT5S" />  
   </PerformanceCounters>  

   <WindowsEventLog bufferQuotaInMB="512"  
      scheduledTransferLogLevelFilter="Verbose"  
      scheduledTransferPeriod="PT1M">  
      <!-- The event log name is in the same format as the imperative   
           diagnostics configuration API -->  
      <DataSource name="System!*" />  
   </WindowsEventLog>  
</DiagnosticMonitorConfiguration>  
```  

## <a name="diagnosticsconfiguration-namespace"></a>DiagnosticsConfiguration Namespace  
 Tanılama yapılandırma dosyası için XML ad alanı şöyledir:  

```  
http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration  
```  

## <a name="schema-elements"></a>Şema öğeleri  
 Tanılama yapılandırma dosyası, aşağıdaki öğeleri içerir.


## <a name="diagnosticmonitorconfiguration-element"></a>DiagnosticMonitorConfiguration öğesi  
Tanılama yapılandırma dosyasının en üst düzey öğesi.  

Öznitelikleri:

|Öznitelik  |Tür   |Gerekli| Varsayılan | Açıklama|  
|-----------|-------|--------|---------|------------|  
|**configurationChangePollInterval**|Süre|İsteğe bağlı | PT1M| Tanılama yapılandırma değişiklikleri için tanı İzleyicisi yoklama aralığını belirtir.|  
|**overallQuotaInMB**|unsignedInt|İsteğe bağlı| 4000 MB. Bir değer belirtirseniz, bu miktar aşmamalıdır |Tüm günlük arabellekleri için ayrılan dosya sistemi depolaması toplam miktarı.|  

## <a name="diagnosticinfrastructurelogs-element"></a>DiagnosticInfrastructureLogs öğesi  
Temel alınan Tanılama Altyapısı tarafından oluşturulan günlükleri için arabellek yapılandırmasını tanımlar.

Üst öğe: [DiagnosticMonitorConfiguration öğesi](#DiagnosticMonitorConfiguration).  

Öznitelikleri:

|Öznitelik|Tür|Açıklama|  
|---------|----|-----------------|  
|**bufferQuotaInMB**|unsignedInt|İsteğe bağlı. Belirtilen verileri için kullanılabilir olan dosya sistemi depolaması en miktarını belirtir.<br /><br /> Varsayılan değer 0'dır.|  
|**scheduledTransferLogLevelFilter**|Dize|İsteğe bağlı. Aktarılır günlük girişlerini en düşük önem derecesini belirtir. Varsayılan değer **tanımlanmamış**. Diğer olası değerler şunlardır: **ayrıntılı**, **bilgi**, **uyarı**, **hata**, ve **kritik**.|  
|**scheduledTransferPeriod**|Süre|İsteğe bağlı. Zamanlanmış aktardığı veriler, en yakın dakika yuvarlanan arasındaki aralığı belirtir.<br /><br /> PT0S varsayılandır.|  

## <a name="logs-element"></a>Günlükleri öğesi  
 Temel Azure günlükleri için arabellek yapılandırmasını tanımlar.

 Üst öğenin: [DiagnosticMonitorConfiguration öğesi](#DiagnosticMonitorConfiguration).  

Öznitelikleri:  

|Öznitelik|Tür|Açıklama|  
|---------------|----------|-----------------|  
|**bufferQuotaInMB**|unsignedInt|İsteğe bağlı. Belirtilen verileri için kullanılabilir olan dosya sistemi depolaması en miktarını belirtir.<br /><br /> Varsayılan değer 0'dır.|  
|**scheduledTransferLogLevelFilter**|Dize|İsteğe bağlı. Aktarılır günlük girişlerini en düşük önem derecesini belirtir. Varsayılan değer **tanımlanmamış**. Diğer olası değerler şunlardır: **ayrıntılı**, **bilgi**, **uyarı**, **hata**, ve **kritik**.|  
|**scheduledTransferPeriod**|Süre|İsteğe bağlı. Zamanlanmış aktardığı veriler, en yakın dakika yuvarlanan arasındaki aralığı belirtir.<br /><br /> PT0S varsayılandır.|  

## <a name="directories-element"></a>Dizinleri öğesi  
Arabellek yapılandırmasını tanımlamak dosya tabanlı günlükleri için tanımlar.

Üst öğenin: [DiagnosticMonitorConfiguration öğesi](#DiagnosticMonitorConfiguration).  


Öznitelikleri:  

|Öznitelik|Tür|Açıklama|  
|---------------|----------|-----------------|  
|**bufferQuotaInMB**|unsignedInt|İsteğe bağlı. Belirtilen verileri için kullanılabilir olan dosya sistemi depolaması en miktarını belirtir.<br /><br /> Varsayılan değer 0'dır.|  
|**scheduledTransferPeriod**|Süre|İsteğe bağlı. Zamanlanmış aktardığı veriler, en yakın dakika yuvarlanan arasındaki aralığı belirtir.<br /><br /> PT0S varsayılandır.|  

## <a name="crashdumps-element"></a>CrashDumps öğesi  
 Kilitlenme dökümleri directory tanımlar.

 Üst öğe: [dizinleri öğesi](#Directories).  

Öznitelikleri:  

|Öznitelik|Tür|Açıklama|  
|---------------|----------|-----------------|  
|**kapsayıcı**|Dize|Dizinin içeriğini olduğu aktarılacak kapsayıcısının adı.|  
|**directoryQuotaInMB**|unsignedInt|İsteğe bağlı. Dizin en büyük boyutunu megabayt cinsinden belirtir.<br /><br /> Varsayılan değer 0'dır.|  

## <a name="failedrequestlogs-element"></a>FailedRequestLogs öğesi  
 Başarısız istek günlük dizini tanımlar.

 Üst öğesi [dizinleri öğesi](#Directories).  

Öznitelikleri:  

|Öznitelik|Tür|Açıklama|  
|---------------|----------|-----------------|  
|**kapsayıcı**|Dize|Dizinin içeriğini olduğu aktarılacak kapsayıcısının adı.|  
|**directoryQuotaInMB**|unsignedInt|İsteğe bağlı. Dizin en büyük boyutunu megabayt cinsinden belirtir.<br /><br /> Varsayılan değer 0'dır.|  

##  <a name="iislogs-element"></a>IISLogs öğesi  
 IIS Günlük dizini tanımlar.

 Üst öğesi [dizinleri öğesi](#Directories).  

Öznitelikleri:  

|Öznitelik|Tür|Açıklama|  
|---------------|----------|-----------------|  
|**kapsayıcı**|Dize|Dizinin içeriğini olduğu aktarılacak kapsayıcısının adı.|  
|**directoryQuotaInMB**|unsignedInt|İsteğe bağlı. Dizin en büyük boyutunu megabayt cinsinden belirtir.<br /><br /> Varsayılan değer 0'dır.|  

## <a name="datasources-element"></a>Veri kaynakları öğesi  
 Sıfır veya daha fazla ek günlük dizinleri tanımlar.

 Üst öğe: [dizinleri öğesi](#Directories).

## <a name="directoryconfiguration-element"></a>DirectoryConfiguration öğesi  
 Günlük dosyalarını izlemek için dizin tanımlar.

 Üst öğe: [veri kaynakları öğesi](#DataSources).

Öznitelikleri:

|Öznitelik|Tür|Açıklama|  
|---------------|----------|-----------------|  
|**kapsayıcı**|Dize|Dizinin içeriğini olduğu aktarılacak kapsayıcısının adı.|  
|**directoryQuotaInMB**|unsignedInt|İsteğe bağlı. Dizin en büyük boyutunu megabayt cinsinden belirtir.<br /><br /> Varsayılan değer 0'dır.|  

## <a name="absolute-element"></a>Mutlak öğesi  
 İsteğe bağlı ortam genişletmeyle izlemek için dizininin mutlak bir yol tanımlar.

 Üst öğe: [DirectoryConfiguration öğesi](#DirectoryConfiguration).  

Öznitelikleri:  

|Öznitelik|Tür|Açıklama|  
|---------------|----------|-----------------|  
|**yol**|Dize|Gereklidir. İzlemek için dizinine mutlak yolu.|  
|**expandEnvironment**|Boole değeri|Gereklidir. Varsa kümesine **doğru**, ortam değişkenleri yolun genişletilmiş.|  

## <a name="localresource-element"></a>LocalResource öğesi  
 Hizmet tanımında tanımlanan bir yerel kaynağı göreli bir yol tanımlar.

 Üst öğe: [DirectoryConfiguration öğesi](#DirectoryConfiguration).  

Öznitelikleri:  

|Öznitelik|Tür|Açıklama|  
|---------------|----------|-----------------|  
|**adı**|Dize|Gereklidir. İzlemek için dizinini içeren yerel kaynak adı.|  
|**relativePath**|Dize|Gereklidir. İzlemek için yerel kaynak göreli yolu.|  

## <a name="performancecounters-element"></a>PerformanceCounters öğesi  
 Yolun toplamak için performans sayacı tanımlar.

 Üst öğe: [DiagnosticMonitorConfiguration öğesi](#DiagnosticMonitorConfiguration).


 Öznitelikleri:  

|Öznitelik|Tür|Açıklama|  
|---------------|----------|-----------------|  
|**bufferQuotaInMB**|unsignedInt|İsteğe bağlı. Belirtilen verileri için kullanılabilir olan dosya sistemi depolaması en miktarını belirtir.<br /><br /> Varsayılan değer 0'dır.|  
|**scheduledTransferPeriod**|Süre|İsteğe bağlı. Zamanlanmış aktardığı veriler, en yakın dakika yuvarlanan arasındaki aralığı belirtir.<br /><br /> PT0S varsayılandır.|  

## <a name="performancecounterconfiguration-element"></a>PerformanceCounterConfiguration öğesi  
 Toplamak için performans sayacı tanımlar.

 Üst öğe: [PerformanceCounters öğesi](#PerformanceCounters).  

 Öznitelikleri:  

|Öznitelik|Tür|Açıklama|  
|---------------|----------|-----------------|  
|**counterSpecifier**|Dize|Gereklidir. Toplamak için performans sayacı yolu.|  
|**sampleRate**|Süre|Gereklidir. Performans sayacı toplanması gereken oranı.|  

## <a name="windowseventlog-element"></a>WindowsEventLog öğesi  
 İzlemek için olay günlüklerine tanımlar.

 Üst öğe: [DiagnosticMonitorConfiguration öğesi](#DiagnosticMonitorConfiguration).

  Öznitelikleri:

|Öznitelik|Tür|Açıklama|  
|---------------|----------|-----------------|  
|**bufferQuotaInMB**|unsignedInt|İsteğe bağlı. Belirtilen verileri için kullanılabilir olan dosya sistemi depolaması en miktarını belirtir.<br /><br /> Varsayılan değer 0'dır.|  
|**scheduledTransferLogLevelFilter**|Dize|İsteğe bağlı. Aktarılır günlük girişlerini en düşük önem derecesini belirtir. Varsayılan değer **tanımlanmamış**. Diğer olası değerler şunlardır: **ayrıntılı**, **bilgi**, **uyarı**, **hata**, ve **kritik**.|  
|**scheduledTransferPeriod**|Süre|İsteğe bağlı. Zamanlanmış aktardığı veriler, en yakın dakika yuvarlanan arasındaki aralığı belirtir.<br /><br /> PT0S varsayılandır.|  

## <a name="datasource-element"></a>Veri kaynağı öğesi  
 İzlemek için olay günlüğüne tanımlar.

 Üst öğe: [WindowsEventLog öğesi](#windowsEventLog).  

 Öznitelikleri:

|Öznitelik|Tür|Açıklama|  
|---------------|----------|-----------------|  
|**adı**|Dize|Gereklidir. Toplanacak günlük belirten bir XPath ifadesi.|  
