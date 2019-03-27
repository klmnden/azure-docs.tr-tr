---
title: Azure tanılama uzantısını 1.2 yapılandırma şeması
description: YALNIZCA Azure sanal makineler, sanal makine ölçek kümeleri, Service Fabric ve Cloud Services ile Azure SDK 2.5 kullanıyorsanız ilgilidir.
services: azure-monitor
author: rboucher
ms.service: azure-monitor
ms.devlang: dotnet
ms.topic: reference
ms.date: 05/15/2017
ms.author: robb
ms.subservice: diagnostic-extension
ms.openlocfilehash: 1ffeab91933bfcba9f3ffa0b557e849a1e6890f5
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58486164"
---
# <a name="azure-diagnostics-12-configuration-schema"></a>Azure tanılama 1.2 yapılandırma şeması
> [!NOTE]
> Azure Tanılama, Azure sanal makineler, sanal makine ölçek kümeleri, Service Fabric ve Cloud Services performans sayaçları ve diğer istatistikleri toplamak için kullanılan bileşendir.  Bu sayfada, yalnızca bu hizmetlerden biri kullanıyorsanız geçerlidir.
>

Azure Tanılama, Azure İzleyici, Application Insights ve Log Analytics gibi diğer Microsoft tanılama ürünleriyle kullanılır.

Bu şema Tanılama izleme başladığında tanılama yapılandırma ayarları başlatmak için kullanabilirsiniz olası değerleri tanımlar.  


 Genel yapılandırma dosyası şeması tanımı aşağıdaki PowerShell komutunu çalıştırarak yükleyin:  

```powershell  
(Get-AzureServiceAvailableExtension -ExtensionName 'PaaSDiagnostics' -ProviderNamespace 'Microsoft.Azure.Diagnostics').PublicConfigurationSchema | Out-File –Encoding utf8 -FilePath 'C:\temp\WadConfig.xsd'  
```  

 Azure tanılama kullanma hakkında daha fazla bilgi için bkz. [Azure Cloud Services tanılamayı etkinleştirme](https://azure.microsoft.com/documentation/articles/cloud-services-dotnet-diagnostics/).  

## <a name="example-of-the-diagnostics-configuration-file"></a>Tanılama yapılandırma dosyası örneği  
 Aşağıdaki örnek, bir normal tanılama yapılandırma dosyası gösterir:  

```xml
<?xml version="1.0" encoding="utf-8"?>  
<PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">  
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
        <EtwEventSourceProviderConfiguration provider="MyProviderClass" scheduledTransferPeriod="PT5M">  
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
      <CrashDumps containerName="wad-crashdumps" directoryQuotaPercentage="30" dumpType="Mini">  
        <CrashDumpConfiguration processName="mynewprocess.exe" />  
        <CrashDumpConfiguration processName="badapp.exe"/>  
      </CrashDumps>  
    </DiagnosticMonitorConfiguration>  
  </WadCfg>  
</PublicConfig>  

```  

## <a name="diagnostics-configuration-namespace"></a>Tanılama yapılandırması Namespace  
 Tanılama yapılandırma dosyası için XML ad alanı verilmiştir:  

```  
http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration  
```  

## <a name="publicconfig-element"></a>PublicConfig öğesi  
 Tanılama yapılandırma dosyası üst düzey öğesi. Aşağıdaki tabloda, yapılandırma dosyasının öğeleri açıklar.  

|Öğe adı|Açıklama|  
|------------------|-----------------|  
|**WadCfg**|Gereklidir. Toplanacak telemetri verilerini için yapılandırma ayarları.|  
|**Depolama hesabı**|Verileri depolamak için Azure depolama hesabı adı. Bu da bir parametre olarak kümesi AzureServiceDiagnosticsExtension cmdlet'ini çalıştırırken belirtilebilir.|  
|**LocalResourceDirectory**|Olay verilerini depolamak için izleme aracısı tarafından kullanılacak sanal makinesinde dizin. Aksi halde, varsayılan dizin kullanılır:<br /><br /> Çalışan/web rolü için: `C:\Resources\<guid>\directory\<guid>.<RoleName.DiagnosticStore\`<br /><br /> Bir sanal makine için: `C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<WADVersion>\WAD<WADVersion>`<br /><br /> Gerekli öznitelik şunlardır:<br /><br /> -                      **yol** -Azure tanılama tarafından kullanılmak üzere sistemde dizini.<br /><br /> -                      **expandEnvironment** -ortam değişkenlerini yol adına genişletilir olup olmadığını denetler.|  

## <a name="wadcfg-element"></a>WadCFG Element  
Toplanacak telemetri verilerini için yapılandırma ayarlarını tanımlar. Aşağıdaki tabloda, alt öğeleri açıklanmıştır:  

|Öğe adı|Açıklama|  
|------------------|-----------------|  
|**DiagnosticMonitorConfiguration**|Gereklidir. İsteğe bağlı öznitelikleri şunlardır:<br /><br /> -                     **overallQuotaInMB** -tanılama verilerini çeşitli türleri tarafından kullanılan yerel disk alanı miktarını Azure tanılama tarafından toplanır. 5120 MB varsayılan ayardır.<br /><br /> -                     **useProxyServer** -IE ayarlarında belirlenen Ara sunucu ayarlarını kullanmak için Azure Tanılama'yı yapılandırın.|  
|**CrashDumps**|Kilitlenme bilgi dökümleri toplama. İsteğe bağlı öznitelikleri şunlardır:<br /><br /> -                     **containerName** -kilitlenme bilgi dökümleri depolamak için kullanılacak Azure depolama hesabınızdaki blob kapsayıcısının adı.<br /><br /> -                     **crashDumpType** -Mini ya da tam kilitlenme toplamak için Azure Tanılama'yı yapılandırır dökümünü yapar.<br /><br /> -                     **directoryQuotaPercentage**-yüzdesini yapılandırır **overallQuotaInMB** VM'de kilitlenme dökümleri için rezerve edilecek.|  
|**DiagnosticInfrastructureLogs**|Azure tanılama tarafından oluşturulan günlüklerin toplanmasını etkinleştirin. Tanılama Altyapısı günlükleri, tanılama sistem gidermek için kullanışlıdır. İsteğe bağlı öznitelikleri şunlardır:<br /><br /> -                     **scheduledTransferLogLevelFilter** -toplanan günlüklerde en düşük önem derecesi yapılandırır.<br /><br /> -                     **scheduledTransferPeriod** -zamanlanmış aktarmalarıyla depolama arasındaki aralığı ve en yakın dakikaya yuvarlanır. Değer bir [XML "Süresi veri türü."](https://www.w3schools.com/xml/schema_dtypes_date.asp)|  
|**Dizinler**|Bir dizin, IIS başarısız istek günlüklerine erişme ve/veya IIS günlükler içeriğini toplanmasını etkinleştirir. İsteğe bağlı öznitelik:<br /><br /> **scheduledTransferPeriod** -zamanlanmış aktarmalarıyla depolama arasındaki aralığı ve en yakın dakikaya yuvarlanır. Değer bir [XML "Süresi veri türü."](https://www.w3schools.com/xml/schema_dtypes_date.asp)|  
|**EtwProviders**|EventSource ETW olayları toplamayı yapılandırır ve/veya ETW bildirim alarak sağlayıcıları.|  
|**Ölçümler**|Bu öğe, hızlı sorgular için optimize edilmiş performans sayacı tablo oluşturmanıza olanak sağlar. Tanımlanan her performans sayacı **PerformanceCounters** öğesi performans sayacı tablonun yanı sıra ölçüm tablosunda depolanır. Gerekli öznitelik:<br /><br /> **ResourceId** -bu Azure tanılama dağıttığınız sanal makinenin kaynak kimliğidir. Alma **ResourceId** gelen [Azure portalında](https://portal.azure.com). Seçin **Gözat** -> **kaynak grupları** -> **< adı\>**. Tıklayın **özellikleri** kutucuğuna ve değeri Şuradan Kopyala: **kimliği** alan.|  
|**PerformanceCounters**|Performans sayaçlarını toplamayı etkinleştirir. İsteğe bağlı öznitelik:<br /><br /> **scheduledTransferPeriod** -zamanlanmış aktarmalarıyla depolama arasındaki aralığı ve en yakın dakikaya yuvarlanır. Değer bir [XML "Süresi veri türü".](https://www.w3schools.com/xml/schema_dtypes_date.asp)|  
|**WindowsEventLog**|Windows olay günlükleri, koleksiyonunu etkinleştirir. İsteğe bağlı öznitelik:<br /><br /> **scheduledTransferPeriod** -zamanlanmış aktarmalarıyla depolama arasındaki aralığı ve en yakın dakikaya yuvarlanır. Değer bir [XML "Süresi veri türü".](https://www.w3schools.com/xml/schema_dtypes_date.asp)|  

## <a name="crashdumps-element"></a>CrashDumps öğesi  
 Kilitlenme bilgi dökümleri toplanmasını etkinleştirir. Aşağıdaki tabloda, alt öğeleri açıklanmıştır:  

|Öğe adı|Açıklama|  
|------------------|-----------------|  
|**CrashDumpConfiguration**|Gereklidir. Gerekli öznitelik:<br /><br /> **processName** -adı işlemi için kilitlenme bilgi dökümü toplamak için Azure tanılama istiyor.|  
|**crashDumpType**|Mini veya tam kilitlenme dökümleri toplamak için Azure Tanılama'yı yapılandırır.|  
|**directoryQuotaPercentage**|Yüzdesini yapılandırır **overallQuotaInMB** VM'de kilitlenme dökümleri için rezerve edilecek.|  

## <a name="directories-element"></a>Dizinleri öğesi  
 Bir dizin, IIS başarısız istek günlüklerine erişme ve/veya IIS günlükler içeriğini toplanmasını etkinleştirir. Aşağıdaki tabloda, alt öğeleri açıklanmıştır:  

|Öğe adı|Açıklama|  
|------------------|-----------------|  
|**Veri kaynakları**|İzlenecek dizinler bir listesi.|  
|**FailedRequestLogs**|Bu öğe yapılandırmada dahil olmak üzere bir IIS sitesi veya uygulama başarısız istekler hakkında günlüklerin toplanmasını sağlar. İzleme seçenekleri altında da etkinleştirmeniz gerekir **sistem. Web sunucusu** içinde **Web.config**.|  
|**IISLogs**|Bu öğe yapılandırmada dahil olmak üzere IIS günlükler koleksiyonunu sağlar:<br /><br /> **containerName** -Azure depolama hesabınızda IIS günlüklerini depolamak için kullanılacak blob kapsayıcısının adı.|  

## <a name="datasources-element"></a>Veri kaynakları öğesi  
 İzlenecek dizinler bir listesi. Aşağıdaki tabloda, alt öğeleri açıklanmıştır:  

|Öğe adı|Açıklama|  
|------------------|-----------------|  
|**DirectoryConfiguration**|Gereklidir. Gerekli öznitelik:<br /><br /> **containerName** -günlük dosyalarını depolamak için kullanılacak Azure depolama hesabınızdaki blob kapsayıcısının adı.|  

## <a name="directoryconfiguration-element"></a>DirectoryConfiguration öğesi  
 **DirectoryConfiguration** ya da içerebilir **mutlak** veya **LocalResource** öğe ancak ikisine birden değil. Aşağıdaki tabloda, alt öğeleri açıklanmıştır:  

|Öğe adı|Açıklama|  
|------------------|-----------------|  
|**Mutlak**|İzlemek için dizinin mutlak yolu. Aşağıdaki öznitelikler gereklidir:<br /><br /> -                     **Yol** -izlemek için dizinin mutlak yolu.<br /><br /> -                      **expandEnvironment** -yolunda ortam değişkenleri genişletilmiş paylaşamayacağını yapılandırır.|  
|**LocalResource**|İzlemek için yerel kaynak göreli yol. Gerekli öznitelik şunlardır:<br /><br /> -                     **Ad** -izlemek için dizin içeren yerel kaynağı<br /><br /> -                     **relativePath** -izlemek için dizin içeren adı göreli yol|  

## <a name="etwproviders-element"></a>EtwProviders Element  
 EventSource ETW olayları toplamayı yapılandırır ve/veya ETW bildirim alarak sağlayıcıları. Aşağıdaki tabloda, alt öğeleri açıklanmıştır:  

|Öğe adı|Açıklama|  
|------------------|-----------------|  
|**EtwEventSourceProviderConfiguration**|Oluşturulan olayları toplamayı yapılandırır [EventSource sınıfı](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource\(v=vs.110\).aspx). Gerekli öznitelik:<br /><br /> **Sağlayıcı** -EventSource olay sınıfı adı.<br /><br /> İsteğe bağlı öznitelikleri şunlardır:<br /><br /> -                     **scheduledTransferLogLevelFilter** -depolama hesabınıza aktarmak için en düşük önem düzeyi.<br /><br /> -                     **scheduledTransferPeriod** -zamanlanmış aktarmalarıyla depolama arasındaki aralığı ve en yakın dakikaya yuvarlanır. Değer bir [XML süre veri türü](https://www.w3schools.com/xml/schema_dtypes_date.asp).|  
|**EtwManifestProviderConfiguration**|Gerekli öznitelik:<br /><br /> **Sağlayıcı** -Olay sağlayıcısı GUID<br /><br /> İsteğe bağlı öznitelikleri şunlardır:<br /><br /> - **scheduledTransferLogLevelFilter** -depolama hesabınıza aktarmak için en düşük önem düzeyi.<br /><br /> -                     **scheduledTransferPeriod** -zamanlanmış aktarmalarıyla depolama arasındaki aralığı ve en yakın dakikaya yuvarlanır. Değer bir [XML süre veri türü](https://www.w3schools.com/xml/schema_dtypes_date.asp).|  

## <a name="etweventsourceproviderconfiguration-element"></a>EtwEventSourceProviderConfiguration Element  
 Oluşturulan olayları toplamayı yapılandırır [EventSource sınıfı](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource\(v=vs.110\).aspx). Aşağıdaki tabloda, alt öğeleri açıklanmıştır:  

|Öğe adı|Açıklama|  
|------------------|-----------------|  
|**DefaultEvents**|İsteğe bağlı öznitelik:<br /><br /> **eventDestination** -olayları depolamak için bir tablonun adı|  
|**Olay**|Gerekli öznitelik:<br /><br /> **Kimliği** -etkinliğin kimliği.<br /><br /> İsteğe bağlı öznitelik:<br /><br /> **eventDestination** -olayları depolamak için bir tablonun adı|  

## <a name="etwmanifestproviderconfiguration-element"></a>EtwManifestProviderConfiguration Element  
 Aşağıdaki tabloda, alt öğeleri açıklanmıştır:  

|Öğe adı|Açıklama|  
|------------------|-----------------|  
|**DefaultEvents**|İsteğe bağlı öznitelik:<br /><br /> **eventDestination** -olayları depolamak için bir tablonun adı|  
|**Olay**|Gerekli öznitelik:<br /><br /> **Kimliği** -etkinliğin kimliği.<br /><br /> İsteğe bağlı öznitelik:<br /><br /> **eventDestination** -olayları depolamak için bir tablonun adı|  

## <a name="metrics-element"></a>Ölçümleri öğesi  
 Hızlı sorgular için optimize edilmiş performans sayacı tablo oluşturmanıza olanak sağlar. Aşağıdaki tabloda, alt öğeleri açıklanmıştır:  

|Öğe adı|Açıklama|  
|------------------|-----------------|  
|**MetricAggregation**|Gerekli öznitelik:<br /><br /> **scheduledTransferPeriod** -zamanlanmış aktarmalarıyla depolama arasındaki aralığı ve en yakın dakikaya yuvarlanır. Değer bir [XML süre veri türü](https://www.w3schools.com/xml/schema_dtypes_date.asp).|  

## <a name="performancecounters-element"></a>PerformanceCounters öğesi  
 Performans sayaçlarını toplamayı etkinleştirir. Aşağıdaki tabloda, alt öğeleri açıklanmıştır:  

|Öğe adı|Açıklama|  
|------------------|-----------------|  
|**PerformanceCounterConfiguration**|Aşağıdaki öznitelikler gereklidir:<br /><br /> -                     **counterSpecifier** -performans sayacının adı. Örneğin, `\Processor(_Total)\% Processor Time`. Ana bilgisayarınızda sayaçları performans listesini almak için komutu çalıştırın `typeperf`.<br /><br /> -                     **sampleRate** -sayaç ne sıklıkta örneklenir.<br /><br /> İsteğe bağlı öznitelik:<br /><br /> **Birim** -sayacın ölçü birimi.|  

## <a name="performancecounterconfiguration-element"></a>PerformanceCounterConfiguration öğesi  
 Aşağıdaki tabloda, alt öğeleri açıklanmıştır:  

|Öğe adı|Açıklama|  
|------------------|-----------------|  
|**Ek açıklama**|Gerekli öznitelik:<br /><br /> **displayName** -sayaç görünen adı<br /><br /> İsteğe bağlı öznitelik:<br /><br /> **yerel ayar** -sayaç adı görüntülenirken kullanılacak yerel ayar|  

## <a name="windowseventlog-element"></a>WindowsEventLog öğesi  
 Aşağıdaki tabloda, alt öğeleri açıklanmıştır:  

|Öğe adı|Açıklama|  
|------------------|-----------------|  
|**Veri kaynağı**|Windows olay günlüklerini toplamak üzere. Gerekli öznitelik:<br /><br /> **ad** - Tahsil edilecek windows olayları tanımlayan XPath sorgusu. Örneğin:<br /><br /> `Application!*[System[(Level >= 3)]], System!*[System[(Level <=3)]], System!*[System[Provider[@Name='Microsoft Antimalware']]], Security!*[System[(Level >= 3]]`<br /><br /> Tüm olaylarını toplamak için belirtin "*".|

