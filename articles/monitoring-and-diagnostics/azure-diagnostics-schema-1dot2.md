---
title: "Azure tanılama 1.2 yapılandırma şeması | Microsoft Docs"
description: "YALNIZCA Azure sanal makineler, sanal makine ölçek kümeleri, Service Fabric veya Bulut Hizmetleri ile Azure SDK 2.5 kullanıyorsanız ilgilidir."
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
ms.openlocfilehash: 1e9cc6d0950945df8c4fba74d8e1f6196be224f0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-diagnostics-12-configuration-schema"></a>Azure tanılama 1.2 yapılandırma şeması
> [!NOTE]
> Azure tanılama Azure sanal makineler, sanal makine ölçek kümeleri, Service Fabric ve bulut Hizmetleri performans sayaçları ve diğer istatistikleri toplamak için kullanılan bileşendir.  Bu sayfa, yalnızca bu hizmetlerden biri kullanıyorsanız geçerlidir.
>

Azure tanılama Azure monitör, Application Insights ve günlük analizi gibi diğer Microsoft tanılama ürünlerle kullanılır.

Bu şemayı tanılama İzleyicisi başlatıldığında tanılama yapılandırma ayarlarını başlatmak için kullanabileceğiniz olası değerler tanımlar.  


 Genel yapılandırma dosyası şeması tanımı aşağıdaki PowerShell komutunu yürüterek yükleyin:  

```PowerShell  
(Get-AzureServiceAvailableExtension -ExtensionName 'PaaSDiagnostics' -ProviderNamespace 'Microsoft.Azure.Diagnostics').PublicConfigurationSchema | Out-File –Encoding utf8 -FilePath 'C:\temp\WadConfig.xsd'  
```  

 Azure tanılama kullanma hakkında daha fazla bilgi için bkz: [Azure Cloud Services etkinleştirme tanılamada](http://azure.microsoft.com/documentation/articles/cloud-services-dotnet-diagnostics/).  

## <a name="example-of-the-diagnostics-configuration-file"></a>Tanılama yapılandırma dosyası örneği  
 Aşağıdaki örnek bir genel tanılama yapılandırma dosyası gösterir:  

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
 Tanılama yapılandırma dosyası için XML ad alanı şöyledir:  

```  
http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration  
```  

## <a name="publicconfig-element"></a>PublicConfig öğesi  
 Tanılama yapılandırma dosyasının en üst düzey öğesi. Aşağıdaki tabloda yapılandırma dosyasının öğelerini açıklar.  

|Öğe adı|Açıklama|  
|------------------|-----------------|  
|**WadCfg**|Gereklidir. Toplanacak telemetri verileri için yapılandırma ayarları.|  
|**StorageAccount**|Verileri depolamak için Azure Storage hesabının adı. Bu da bir parametre olarak Set-AzureServiceDiagnosticsExtension cmdlet'ini çalıştırırken belirtilebilir.|  
|**LocalResourceDirectory**|Olay verilerini depolamak için izleme aracısı tarafından kullanılacak sanal makinede dizin. Aksi halde kümesi, varsayılan dizini kullanılır:<br /><br /> Worker/web rolü için:`C:\Resources\<guid>\directory\<guid>.<RoleName.DiagnosticStore\`<br /><br /> Bir sanal makine için:`C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<WADVersion>\WAD<WADVersion>`<br /><br /> Gerekli öznitelikler şunlardır:<br /><br /> -                      **yol** -Azure tanılama tarafından kullanılmak üzere sistemde dizin.<br /><br /> -                      **expandEnvironment** -ortam değişkenleri yol adındaki genişletilmiş olup olmadığını denetler.|  

## <a name="wadcfg-element"></a>WadCFG öğesi  
Toplanacak telemetri verilerini yapılandırma ayarlarını tanımlar. Aşağıdaki tabloda, alt öğeleri açıklanmıştır:  

|Öğe adı|Açıklama|  
|------------------|-----------------|  
|**DiagnosticMonitorConfiguration**|Gereklidir. İsteğe bağlı öznitelikleri şunlardır:<br /><br /> -                     **overallQuotaInMB** -tanılama verilerini çeşitli türleri tarafından tüketilen yerel disk alanı miktarını Azure tanılama tarafından toplanır. Varsayılan ayar 5120 MB'tır.<br /><br /> -                     **useProxyServer** -IE ayarlarının kümesinde olarak ara sunucu ayarlarını kullanmak için Azure Tanılama'yı yapılandırın.|  
|**CrashDumps**|Kilitlenme bilgi dökümleri koleksiyonunu etkinleştirin. İsteğe bağlı öznitelikleri şunlardır:<br /><br /> -                     **kapsayıcı adı** -kilitlenme bilgi dökümleri depolamak için kullanılacak Azure depolama hesabınızdaki blob kapsayıcısının adı.<br /><br /> -                     **crashDumpType** -Mini ya da tam kilitlenme toplamak için Azure Tanılama'yı yapılandırır dökümünü yapar.<br /><br /> -                     **directoryQuotaPercentage**-yüzdesini yapılandırır **overallQuotaInMB** VM kilitlenme dökümleri için ayrılmış olmalıdır.|  
|**DiagnosticInfrastructureLogs**|Azure tanılama tarafından oluşturulan günlükleri koleksiyonunu etkinleştirin. Tanılama Altyapısı günlükleri, tanılama sistem sorun giderme için yararlıdır. İsteğe bağlı öznitelikleri şunlardır:<br /><br /> -                     **scheduledTransferLogLevelFilter** -toplanan günlüklerini en düşük önem derecesi yapılandırır.<br /><br /> -                     **scheduledTransferPeriod** -depolama zamanlanmış aktarımları arasındaki aralığı yakın dakika yuvarlanan. Değer bir [XML "Süre veri türü."](http://www.w3schools.com/schema/schema_dtypes_date.asp)|  
|**Dizinleri**|Bir dizin, IIS başarısız erişim isteği günlükleri ve/veya IIS günlüklerini içeriğini koleksiyonunu sağlar. İsteğe bağlı öznitelik:<br /><br /> **scheduledTransferPeriod** -depolama zamanlanmış aktarımları arasındaki aralığı yakın dakika yuvarlanan. Değer bir [XML "Süre veri türü."](http://www.w3schools.com/schema/schema_dtypes_date.asp)|  
|**EtwProviders**|EventSource ETW olayları koleksiyonu yapılandırır ve/veya ETW bildirim dayalı sağlayıcıları.|  
|**Ölçümler**|Bu öğe hızlı sorguları için en iyi hale getirilmiş bir performans sayacı tablo oluşturmanıza olanak sağlar. Tanımlanan her performans sayacı **performans sayaçları** öğesi performans sayacı tablo yanı sıra ölçüm tablosunda depolanır. Gerekli öznitelik:<br /><br /> **ResourceId** -bu Azure tanılama dağıttığınız sanal makine kaynak kimliğidir. Alma **ResourceId** gelen [Azure portal](https://portal.azure.com). Seçin **Gözat** -> **kaynak grupları** -> **< adı\>**. Tıklatın **özellikleri** döşeme ve değerini kopyalayın **kimliği** alan.|  
|**Performans sayaçları**|Performans sayaçları koleksiyonunu sağlar. İsteğe bağlı öznitelik:<br /><br /> **scheduledTransferPeriod** -depolama zamanlanmış aktarımları arasındaki aralığı yakın dakika yuvarlanan. Değer bir [XML "Süre veri türü".](http://www.w3schools.com/schema/schema_dtypes_date.asp)|  
|**WindowsEventLog**|Windows olay günlüklerini toplama sağlar. İsteğe bağlı öznitelik:<br /><br /> **scheduledTransferPeriod** -depolama zamanlanmış aktarımları arasındaki aralığı yakın dakika yuvarlanan. Değer bir [XML "Süre veri türü".](http://www.w3schools.com/schema/schema_dtypes_date.asp)|  

## <a name="crashdumps-element"></a>CrashDumps öğesi  
 Kilitlenme bilgi dökümleri koleksiyonunu sağlar. Aşağıdaki tabloda, alt öğeleri açıklanmıştır:  

|Öğe adı|Açıklama|  
|------------------|-----------------|  
|**CrashDumpConfiguration**|Gereklidir. Gerekli öznitelik:<br /><br /> **işlemadı** -adı işlemi için kilitlenme bilgilerini toplamak için Azure tanılama istiyor.|  
|**crashDumpType**|Mini ya da tam kilitlenme dökümleri toplamak için Azure tanılama yapılandırır.|  
|**directoryQuotaPercentage**|Yüzdesini yapılandırır **overallQuotaInMB** VM kilitlenme dökümleri için ayrılmış olmalıdır.|  

## <a name="directories-element"></a>Dizinleri öğesi  
 Bir dizin, IIS başarısız erişim isteği günlükleri ve/veya IIS günlüklerini içeriğini koleksiyonunu sağlar. Aşağıdaki tabloda, alt öğeleri açıklanmıştır:  

|Öğe adı|Açıklama|  
|------------------|-----------------|  
|**Veri kaynakları**|İzlemek için dizinlerin listesi.|  
|**FailedRequestLogs**|Bu öğe yapılandırmada dahil olmak üzere bir IIS site veya uygulama başarısız istekler hakkında günlükleri koleksiyonunu sağlar. İzleme seçenekleri altında da etkinleştirmeniz gerekir **sistem. Web sunucusu** içinde **Web.config**.|  
|**IISLogs**|Bu öğe yapılandırmada dahil olmak üzere, IIS günlüklerini koleksiyonunu sağlar:<br /><br /> **kapsayıcı adı** -IIS günlükleri depolamak için kullanılacak Azure depolama hesabınızdaki blob kapsayıcısının adı.|  

## <a name="datasources-element"></a>Veri kaynakları öğesi  
 İzlemek için dizinlerin listesi. Aşağıdaki tabloda, alt öğeleri açıklanmıştır:  

|Öğe adı|Açıklama|  
|------------------|-----------------|  
|**DirectoryConfiguration**|Gereklidir. Gerekli öznitelik:<br /><br /> **kapsayıcı adı** -günlük dosyalarını depolamak için kullanılacak Azure depolama hesabınızdaki blob kapsayıcısının adı.|  

## <a name="directoryconfiguration-element"></a>DirectoryConfiguration öğesi  
 **DirectoryConfiguration** ya da içerebilir **mutlak** veya **LocalResource** öğesi ikisini birden belirtmeyin. Aşağıdaki tabloda, alt öğeleri açıklanmıştır:  

|Öğe adı|Açıklama|  
|------------------|-----------------|  
|**Mutlak**|İzlemek için dizinine mutlak yolu. Aşağıdaki öznitelikler gereklidir:<br /><br /> -                     **Yol** -izlemek için dizinine mutlak yolu.<br /><br /> -                      **expandEnvironment** -Path ortam değişkenleri genişletilmiş olup olmadığını yapılandırır.|  
|**LocalResource**|İzlemek için bir yerel kaynağı göreli yolu. Gerekli öznitelikler şunlardır:<br /><br /> -                     **Ad** -izlemek için dizinini içeren yerel kaynağı<br /><br /> -                     **relativePath** -izlemek için dizin içeren adı göreli yolu|  

## <a name="etwproviders-element"></a>EtwProviders öğesi  
 EventSource ETW olayları koleksiyonu yapılandırır ve/veya ETW bildirim dayalı sağlayıcıları. Aşağıdaki tabloda, alt öğeleri açıklanmıştır:  

|Öğe adı|Açıklama|  
|------------------|-----------------|  
|**EtwEventSourceProviderConfiguration**|Koleksiyon üretilen olayların yapılandırır [EventSource sınıfı](http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource\(v=vs.110\).aspx). Gerekli öznitelik:<br /><br /> **Sağlayıcı** -EventSource olay sınıfı adı.<br /><br /> İsteğe bağlı öznitelikleri şunlardır:<br /><br /> -                     **scheduledTransferLogLevelFilter** -depolama hesabınıza aktarmak için en düşük önem düzeyi.<br /><br /> -                     **scheduledTransferPeriod** -depolama zamanlanmış aktarımları arasındaki aralığı yakın dakika yuvarlanan. Değer bir [XML süresi veri türü](http://www.w3schools.com/schema/schema_dtypes_date.asp).|  
|**EtwManifestProviderConfiguration**|Gerekli öznitelik:<br /><br /> **Sağlayıcı** -GUID Olay sağlayıcısı<br /><br /> İsteğe bağlı öznitelikleri şunlardır:<br /><br /> - **scheduledTransferLogLevelFilter** -depolama hesabınıza aktarmak için en düşük önem düzeyi.<br /><br /> -                     **scheduledTransferPeriod** -depolama zamanlanmış aktarımları arasındaki aralığı yakın dakika yuvarlanan. Değer bir [XML süresi veri türü](http://www.w3schools.com/schema/schema_dtypes_date.asp).|  

## <a name="etweventsourceproviderconfiguration-element"></a>EtwEventSourceProviderConfiguration öğesi  
 Koleksiyon üretilen olayların yapılandırır [EventSource sınıfı](http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource\(v=vs.110\).aspx). Aşağıdaki tabloda, alt öğeleri açıklanmıştır:  

|Öğe adı|Açıklama|  
|------------------|-----------------|  
|**DefaultEvents**|İsteğe bağlı öznitelik:<br /><br /> **eventDestination** -olayları depolamak için tablonun adı|  
|**Olay**|Gerekli öznitelik:<br /><br /> **Kimliği** -olay kimliği.<br /><br /> İsteğe bağlı öznitelik:<br /><br /> **eventDestination** -olayları depolamak için tablonun adı|  

## <a name="etwmanifestproviderconfiguration-element"></a>EtwManifestProviderConfiguration öğesi  
 Aşağıdaki tabloda, alt öğeleri açıklanmıştır:  

|Öğe adı|Açıklama|  
|------------------|-----------------|  
|**DefaultEvents**|İsteğe bağlı öznitelik:<br /><br /> **eventDestination** -olayları depolamak için tablonun adı|  
|**Olay**|Gerekli öznitelik:<br /><br /> **Kimliği** -olay kimliği.<br /><br /> İsteğe bağlı öznitelik:<br /><br /> **eventDestination** -olayları depolamak için tablonun adı|  

## <a name="metrics-element"></a>Ölçümleri öğesi  
 Hızlı sorguları için en iyi hale getirilmiş bir performans sayacı tablo oluşturmanıza olanak sağlar. Aşağıdaki tabloda, alt öğeleri açıklanmıştır:  

|Öğe adı|Açıklama|  
|------------------|-----------------|  
|**MetricAggregation**|Gerekli öznitelik:<br /><br /> **scheduledTransferPeriod** -depolama zamanlanmış aktarımları arasındaki aralığı yakın dakika yuvarlanan. Değer bir [XML süresi veri türü](http://www.w3schools.com/schema/schema_dtypes_date.asp).|  

## <a name="performancecounters-element"></a>PerformanceCounters öğesi  
 Performans sayaçları koleksiyonunu sağlar. Aşağıdaki tabloda, alt öğeleri açıklanmıştır:  

|Öğe adı|Açıklama|  
|------------------|-----------------|  
|**PerformanceCounterConfiguration**|Aşağıdaki öznitelikler gereklidir:<br /><br /> -                     **counterSpecifier** -performans sayacının adı. Örneğin, `\Processor(_Total)\% Processor Time`. Performans listesini almak için ana bilgisayarınızda sayaçları komutu çalıştırın `typeperf`.<br /><br /> -                     **sampleRate** -sayaç ne sıklıkta örneklenen.<br /><br /> İsteğe bağlı öznitelik:<br /><br /> **Birim** -sayaç ölçü birimi.|  

## <a name="performancecounterconfiguration-element"></a>PerformanceCounterConfiguration öğesi  
 Aşağıdaki tabloda, alt öğeleri açıklanmıştır:  

|Öğe adı|Açıklama|  
|------------------|-----------------|  
|**ek açıklama**|Gerekli öznitelik:<br /><br /> **displayName** -sayaç görünen adı<br /><br /> İsteğe bağlı öznitelik:<br /><br /> **yerel ayar** -sayaç adı görüntülerken kullanılacak yerel ayar|  

## <a name="windowseventlog-element"></a>WindowsEventLog öğesi  
 Aşağıdaki tabloda, alt öğeleri açıklanmıştır:  

|Öğe adı|Açıklama|  
|------------------|-----------------|  
|**Veri kaynağı**|Windows olay günlüklerini toplamak üzere. Gerekli öznitelik:<br /><br /> **ad** - toplanacak windows olayları tanımlayan XPath sorgusu. Örneğin:<br /><br /> `Application!*[System[(Level >= 3)]], System!*[System[(Level <=3)]], System!*[System[Provider[@Name='Microsoft Antimalware']]], Security!*[System[(Level >= 3]]`<br /><br /> Tüm olaylarını toplamak için belirtin "*".|
