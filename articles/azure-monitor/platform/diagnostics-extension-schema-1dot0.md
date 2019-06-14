---
title: Azure tanılama 1.0 yapılandırma şeması
description: YALNIZCA ilgili Azure SDK 2.4 kullanıyorsanız ve aşağıdaki Azure sanal makineler, sanal makine ölçek kümeleri, Service Fabric ve Cloud Services ile.
services: azure-monitor
author: rboucher
ms.service: azure-monitor
ms.devlang: dotnet
ms.topic: reference
ms.date: 05/15/2017
ms.author: robb
ms.subservice: diagnostic-extension
ms.openlocfilehash: ac2b79d670b803573a359dfc9f8738f972f2d9b5
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60237850"
---
# <a name="azure-diagnostics-10-configuration-schema"></a>Azure tanılama 1.0 yapılandırma şeması
> [!NOTE]
> Azure Tanılama, Azure sanal makineler, sanal makine ölçek kümeleri, Service Fabric ve Cloud Services performans sayaçları ve diğer istatistikleri toplamak için kullanılan bileşendir.  Bu sayfada, yalnızca bu hizmetlerden biri kullanıyorsanız geçerlidir.
>

Azure Tanılama, Application Insights ve Log Analytics içeren Azure İzleyici gibi diğer Microsoft tanılama ürünleriyle kullanılır.

Azure tanılama yapılandırma dosyası, tanılama İzleyicisi'ni başlatmak için kullanılan değerleri tanımlar. Bu dosya, Tanılama izleme başladığında tanılama yapılandırma ayarları başlatmak için kullanılır.  

 Azure tanılama yapılandırma şema dosyası varsayılan olarak yüklü `C:\Program Files\Microsoft SDKs\Azure\.NET SDK\<version>\schemas` dizin. Değiştirin `<version>` 'ın yüklü sürümü ile [Azure SDK'sı](https://www.windowsazure.com/develop/downloads/).  

> [!NOTE]
>  Tanılama yapılandırma dosyası, genellikle başlangıç işlemde daha önce toplanacak tanılama verilerini gerektiren başlangıç görevleri ile kullanılır. Azure tanılama kullanma hakkında daha fazla bilgi için bkz. [kullanarak Azure Tanılama ile günlük verileri topla](assetId:///83a91c23-5ca2-4fc9-8df3-62036c37a3d7).  

## <a name="example-of-the-diagnostics-configuration-file"></a>Tanılama yapılandırma dosyası örneği  
 Aşağıdaki örnek, bir normal tanılama yapılandırma dosyası gösterir:  

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
 Tanılama yapılandırma dosyası için XML ad alanı verilmiştir:  

```  
http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration  
```  

## <a name="schema-elements"></a>Şema öğeleri  
 Tanılama yapılandırma dosyası, aşağıdaki öğeleri içerir.


## <a name="diagnosticmonitorconfiguration-element"></a>DiagnosticMonitorConfiguration öğesi  
En üst düzey öğesi tanılama yapılandırma dosyası.  

Öznitelikleri:

|Öznitelik  |Tür   |Gerekli| Varsayılan | Açıklama|  
|-----------|-------|--------|---------|------------|  
|**configurationChangePollInterval**|Süresi|İsteğe bağlı | PT1M| Tanı İzleyicisi yoklama aralığını tanılama yapılandırma değişikliklerini belirtir.|  
|**overallQuotaInMB**|unsignedInt|İsteğe bağlı| 4000 MB. Bir değer belirtirseniz, bu miktar aşmamalıdır |Tüm günlük arabellekleri için ayrılan dosya sistemi depolama miktarı.|  

## <a name="diagnosticinfrastructurelogs-element"></a>DiagnosticInfrastructureLogs öğesi  
Temel alınan Tanılama Altyapısı tarafından oluşturulan günlükler için arabellek yapılandırmasını tanımlar.

Üst öğe: DiagnosticMonitorConfiguration öğesi.  

Öznitelikleri:

|Öznitelik|Tür|Açıklama|  
|---------|----|-----------------|  
|**bufferQuotaInMB**|unsignedInt|İsteğe bağlı. Belirtilen veriler için kullanılabilen dosya sistemi depolama miktarını belirtir.<br /><br /> Varsayılan değer 0'dır.|  
|**scheduledTransferLogLevelFilter**|string|İsteğe bağlı. Aktarılan günlük girişlerini için en düşük önem derecesini belirtir. Varsayılan değer **tanımlanmamış**. Diğer olası değerler **ayrıntılı**, **bilgi**, **uyarı**, **hata**, ve **kritik**.|  
|**scheduledTransferPeriod**|Süresi|İsteğe bağlı. Zamanlanmış aktardığı veriler, en yakın dakikaya yuvarlanır arasındaki aralığı belirtir.<br /><br /> PT0S varsayılandır.|  

## <a name="logs-element"></a>Günlükleri öğesi  
 Temel Azure günlükleri arabellek yapılandırmasını tanımlar.

 Üst öğe: DiagnosticMonitorConfiguration öğesi.  

Öznitelikleri:  

|Öznitelik|Tür|Açıklama|  
|---------------|----------|-----------------|  
|**bufferQuotaInMB**|unsignedInt|İsteğe bağlı. Belirtilen veriler için kullanılabilen dosya sistemi depolama miktarını belirtir.<br /><br /> Varsayılan değer 0'dır.|  
|**scheduledTransferLogLevelFilter**|string|İsteğe bağlı. Aktarılan günlük girişlerini için en düşük önem derecesini belirtir. Varsayılan değer **tanımlanmamış**. Diğer olası değerler **ayrıntılı**, **bilgi**, **uyarı**, **hata**, ve **kritik**.|  
|**scheduledTransferPeriod**|Süresi|İsteğe bağlı. Zamanlanmış aktardığı veriler, en yakın dakikaya yuvarlanır arasındaki aralığı belirtir.<br /><br /> PT0S varsayılandır.|  

## <a name="directories-element"></a>Dizinleri öğesi  
Tanımlayabileceğiniz dosya tabanlı günlükler için arabellek yapılandırmasını tanımlar.

Üst öğe: DiagnosticMonitorConfiguration öğesi.  


Öznitelikleri:  

|Öznitelik|Tür|Açıklama|  
|---------------|----------|-----------------|  
|**bufferQuotaInMB**|unsignedInt|İsteğe bağlı. Belirtilen veriler için kullanılabilen dosya sistemi depolama miktarını belirtir.<br /><br /> Varsayılan değer 0'dır.|  
|**scheduledTransferPeriod**|Süresi|İsteğe bağlı. Zamanlanmış aktardığı veriler, en yakın dakikaya yuvarlanır arasındaki aralığı belirtir.<br /><br /> PT0S varsayılandır.|  

## <a name="crashdumps-element"></a>CrashDumps öğesi  
 Kilitlenme bilgi dökümleri directory tanımlar.

 Üst öğe: Dizinleri öğesi.  

Öznitelikleri:  

|Öznitelik|Tür|Açıklama|  
|---------------|----------|-----------------|  
|**Kapsayıcı**|string|Dizinin içeriklerini olduğu aktarılacak kapsayıcısının adı.|  
|**directoryQuotaInMB**|unsignedInt|İsteğe bağlı. Dizinin en büyük boyutunu megabayt cinsinden belirtir.<br /><br /> Varsayılan değer 0'dır.|  

## <a name="failedrequestlogs-element"></a>FailedRequestLogs öğesi  
 Başarısız istek günlük dizini tanımlar.

 Üst öğe dizinleri öğesi.  

Öznitelikleri:  

|Öznitelik|Tür|Açıklama|  
|---------------|----------|-----------------|  
|**Kapsayıcı**|string|Dizinin içeriklerini olduğu aktarılacak kapsayıcısının adı.|  
|**directoryQuotaInMB**|unsignedInt|İsteğe bağlı. Dizinin en büyük boyutunu megabayt cinsinden belirtir.<br /><br /> Varsayılan değer 0'dır.|  

##  <a name="iislogs-element"></a>IISLogs öğesi  
 IIS Günlük dizini tanımlar.

 Üst öğe dizinleri öğesi.  

Öznitelikleri:  

|Öznitelik|Tür|Açıklama|  
|---------------|----------|-----------------|  
|**Kapsayıcı**|string|Dizinin içeriklerini olduğu aktarılacak kapsayıcısının adı.|  
|**directoryQuotaInMB**|unsignedInt|İsteğe bağlı. Dizinin en büyük boyutunu megabayt cinsinden belirtir.<br /><br /> Varsayılan değer 0'dır.|  

## <a name="datasources-element"></a>Veri kaynakları öğesi  
 Sıfır veya daha fazla ek günlük dizinleri tanımlar.

 Üst öğe: Dizinleri öğesi.

## <a name="directoryconfiguration-element"></a>DirectoryConfiguration öğesi  
 İzleme günlük dosyalarının dizin tanımlar.

 Üst öğe: Veri kaynağı öğesi.

Öznitelikleri:

|Öznitelik|Tür|Açıklama|  
|---------------|----------|-----------------|  
|**Kapsayıcı**|string|Dizinin içeriklerini olduğu aktarılacak kapsayıcısının adı.|  
|**directoryQuotaInMB**|unsignedInt|İsteğe bağlı. Dizinin en büyük boyutunu megabayt cinsinden belirtir.<br /><br /> Varsayılan değer 0'dır.|  

## <a name="absolute-element"></a>Mutlak öğe  
 İsteğe bağlı ortam genişletmeyle izlemek için dizinin mutlak bir yol tanımlar.

 Üst öğe: DirectoryConfiguration öğesi.  

Öznitelikleri:  

|Öznitelik|Tür|Açıklama|  
|---------------|----------|-----------------|  
|**Yolu**|string|Gereklidir. İzlemek için dizinin mutlak yolu.|  
|**expandEnvironment**|boole|Gereklidir. Varsa kümesine **true**, ortam değişkenleri yolun genişletilmiş.|  

## <a name="localresource-element"></a>LocalResource öğesi  
 Hizmet tanımında yerel bir kaynak göreli bir yol tanımlar.

 Üst öğe: DirectoryConfiguration öğesi.  

Öznitelikleri:  

|Öznitelik|Tür|Açıklama|  
|---------------|----------|-----------------|  
|**name**|string|Gereklidir. İzlenecek dizin içeren yerel kaynak adı.|  
|**RelativePath**|string|Gereklidir. İzlemek için yerel kaynak göreli yol.|  

## <a name="performancecounters-element"></a>PerformanceCounters öğesi  
 Toplanacak performans sayaç yolunu tanımlar.

 Üst öğe: DiagnosticMonitorConfiguration öğesi.


 Öznitelikleri:  

|Öznitelik|Tür|Açıklama|  
|---------------|----------|-----------------|  
|**bufferQuotaInMB**|unsignedInt|İsteğe bağlı. Belirtilen veriler için kullanılabilen dosya sistemi depolama miktarını belirtir.<br /><br /> Varsayılan değer 0'dır.|  
|**scheduledTransferPeriod**|Süresi|İsteğe bağlı. Zamanlanmış aktardığı veriler, en yakın dakikaya yuvarlanır arasındaki aralığı belirtir.<br /><br /> PT0S varsayılandır.|  

## <a name="performancecounterconfiguration-element"></a>PerformanceCounterConfiguration öğesi  
 Toplanacak performans sayacını tanımlar.

 Üst öğe: PerformanceCounters öğesi.  

 Öznitelikleri:  

|Öznitelik|Tür|Açıklama|  
|---------------|----------|-----------------|  
|**counterSpecifier**|string|Gereklidir. Toplanacak performans sayacı yolu.|  
|**sampleRate**|Süresi|Gereklidir. Performans sayacının toplanacağını oranı.|  

## <a name="windowseventlog-element"></a>WindowsEventLog öğesi  
 İzlemek için olay günlüklerini tanımlar.

 Üst öğe: DiagnosticMonitorConfiguration öğesi.

  Öznitelikleri:

|Öznitelik|Tür|Açıklama|  
|---------------|----------|-----------------|  
|**bufferQuotaInMB**|unsignedInt|İsteğe bağlı. Belirtilen veriler için kullanılabilen dosya sistemi depolama miktarını belirtir.<br /><br /> Varsayılan değer 0'dır.|  
|**scheduledTransferLogLevelFilter**|string|İsteğe bağlı. Aktarılan günlük girişlerini için en düşük önem derecesini belirtir. Varsayılan değer **tanımlanmamış**. Diğer olası değerler **ayrıntılı**, **bilgi**, **uyarı**, **hata**, ve **kritik**.|  
|**scheduledTransferPeriod**|Süresi|İsteğe bağlı. Zamanlanmış aktardığı veriler, en yakın dakikaya yuvarlanır arasındaki aralığı belirtir.<br /><br /> PT0S varsayılandır.|  

## <a name="datasource-element"></a>Veri kaynağı öğesi  
 İzlenecek olay günlüğünün tanımlar.

 Üst öğe: WindowsEventLog Element.  

 Öznitelikleri:

|Öznitelik|Tür|Açıklama|  
|---------------|----------|-----------------|  
|**name**|string|Gereklidir. Toplanacak günlük belirten bir XPath ifadesi.|  

