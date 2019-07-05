---
title: Azure Application Insights Snapshot Debugger ile ilgili sorunları giderme | Microsoft Docs
description: Bu makalede, sorun giderme adımları ve etkinleştirme veya Application Insights Snapshot Debugger'ı kullanarak sorun geliştiriciler yardımcı olacak bilgiler sunulmaktadır.
services: application-insights
documentationcenter: ''
author: brahmnes
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.reviewer: mbullwin
ms.date: 03/07/2019
ms.author: mbullwin
ms.openlocfilehash: 25ccf20fc78a9ec00d4dfe23a60e824e96d12945
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67444546"
---
# <a id="troubleshooting"></a> Application Insights Snapshot Debugger'ı etkinleştirme veya anlık görüntüleri görüntüleme sorunlarını giderme
Uygulamanız için Application Insights Snapshot Debugger etkin, ancak özel durumlar için anlık görüntüleri görememek, sorunlarını gidermek için bu yönergeleri kullanabilirsiniz. Neden anlık görüntüleri oluşturulmaz birçok farklı neden olabilir. Bazı olası ortak nedenleri belirlemek için anlık görüntü sistem durumu denetimi çalıştırabilirsiniz.

## <a name="use-the-snapshot-health-check"></a>Anlık görüntü sistem durumu denetimini kullanma
Bazı yaygın sorunlar Aç hata ayıklama gösterilmiyor anlık sonuçlanır. Bir tarihi geçmiş anlık görüntü toplayıcının, örneğin kullanma; Günlük karşıya yükleme sınırına ulaşması; veya belki de anlık görüntü yalnızca karşıya yüklemek için bir uzun sürüyor. Sık karşılaşılan sorunları gidermek için anlık görüntü sistem durumu denetimi kullanın.

Uçtan uca izleme görünümünün anlık görüntü sistem durumu denetimi alan özel durum bölmesinde bir bağlantı yoktur.

![Anlık görüntü durum denetimi girin](./media/snapshot-debugger/enter-snapshot-health-check.png)

Etkileşimli, sohbet benzeri arabirimi, sık karşılaşılan sorun için arar ve bunları düzeltmek için size yol gösterir.

![Sistem durumu denetimi](./media/snapshot-debugger/healthcheck.png)

Ardından, sorunu çözmezse, sorun giderme adımları aşağıdaki kılavuzuna başvurun.

## <a name="verify-the-instrumentation-key"></a>İzleme anahtarını doğrulayın

Yayımlanan uygulamanızı doğru izleme anahtarını kullandığınızdan emin olun. Genellikle, izleme anahtarı Applicationınsights.config dosyasından okunur. Değer portalında gördüğünüz Application Insights kaynağı için izleme anahtarı ile aynı olduğunu doğrulayın.

## <a name="preview-versions-of-net-core"></a>.NET Core Önizleme sürümleri
Uygulamanın bir .NET Core önizleme sürümünü kullanıyorsa ve anlık görüntü hata ayıklayıcısı ile etkinleştirildi [Application Insights bölmesinde](snapshot-debugger-appservice.md?toc=/azure/azure-monitor/toc.json) portalda sonra anlık görüntü hata ayıklayıcısı başlatılamayabilir. Konumundaki yönergeleri [diğer ortamlar için Snapshot Debugger'ı etkinleştirme](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json) ilk içerecek şekilde [Microsoft.applicationınsights.snapshotcollectya da](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet paketini uygulamayla***ayrıca*** üzerinden etkinleştirmek için [Application Insights bölmesinde](snapshot-debugger-appservice.md?toc=/azure/azure-monitor/toc.json).


## <a name="upgrade-to-the-latest-version-of-the-nuget-package"></a>NuGet paketinin en son sürüme yükseltme

Anlık görüntü hata ayıklayıcısı ile etkinleştirilmişse [portalındaki Application Insights bölmesinde](snapshot-debugger-appservice.md?toc=/azure/azure-monitor/toc.json), sonra da uygulamanızın en son NuGet paketini çalışır durumda olması. Anlık görüntü hata ayıklayıcısı ekleyerek etkinleştirilirse [Microsoft.applicationınsights.snapshotcollectya da](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet paketi, en son sürümünü kullandığınızdan emin olmak için kullanım Visual Studio'nun NuGet Paket Yöneticisi Microsoft.applicationınsights.snapshotcollectya da. Sürüm Notları şu yolda bulunabilir: https://github.com/Microsoft/ApplicationInsights-Home/issues/167

## <a name="check-the-uploader-logs"></a>Yükleyici günlükleri denetleyin

Bir mini döküm dosyası (.dmp), bir anlık görüntü oluşturulduktan sonra disk üzerinde oluşturulur. Ayrı yükleyici işlemi bu mini döküm dosyası oluşturur ve bunu, Application Insights Snapshot Debugger depolama ilişkili tüm pdb birlikte yükler. Mini döküm dosyası başarıyla karşıya yüklendikten sonra bu diskinden silinir. Yükleyici işlem için günlük dosyaları diskte tutulur. Bir App Service ortamında, bu günlüklerde bulabilirsiniz `D:\Home\LogFiles`. Bu günlük dosyaları bulmak için App Service Kudu yönetim sitesi kullanın.

1. App Service uygulamanızı Azure portalında açın.
2. Tıklayın **Gelişmiş Araçlar**, veya arama **Kudu**.
3. Tıklayın **Git**.
4. İçinde **hata ayıklama konsoluna** aşağı açılan liste kutusunda **CMD**.
5. Tıklayın **LogFiles**.

İle başlayan bir ada sahip en az bir dosya görmeniz gerekir `Uploader_` veya `SnapshotUploader_` ve `.log` uzantısı. Tüm günlük dosyalarını indirin veya bunları bir tarayıcıda açmak için uygun simgeye tıklayın.
App Service örneğine tanımlayan benzersiz bir son eke dosya adını içerir. App Service örneğinizin birden fazla makine üzerinde barındırılıyorsa, her makine için ayrı günlük dosyası vardır. Karşıya yükleyen yeni bir mini döküm dosyası algıladığında, bu günlük dosyasına kaydedilir. Başarılı bir anlık görüntü ve karşıya yükleme örneği aşağıda verilmiştir:

```
SnapshotUploader.exe Information: 0 : Received Fork request ID 139e411a23934dc0b9ea08a626db16c5 from process 6368 (Low pri)
    DateTime=2018-03-09T01:42:41.8571711Z
SnapshotUploader.exe Information: 0 : Creating minidump from Fork request ID 139e411a23934dc0b9ea08a626db16c5 from process 6368 (Low pri)
    DateTime=2018-03-09T01:42:41.8571711Z
SnapshotUploader.exe Information: 0 : Dump placeholder file created: 139e411a23934dc0b9ea08a626db16c5.dm_
    DateTime=2018-03-09T01:42:41.8728496Z
SnapshotUploader.exe Information: 0 : Dump available 139e411a23934dc0b9ea08a626db16c5.dmp
    DateTime=2018-03-09T01:42:45.7525022Z
SnapshotUploader.exe Information: 0 : Successfully wrote minidump to D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp
    DateTime=2018-03-09T01:42:45.7681360Z
SnapshotUploader.exe Information: 0 : Uploading D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp, 214.42 MB (uncompressed)
    DateTime=2018-03-09T01:42:45.7681360Z
SnapshotUploader.exe Information: 0 : Upload successful. Compressed size 86.56 MB
    DateTime=2018-03-09T01:42:59.6184651Z
SnapshotUploader.exe Information: 0 : Extracting PDB info from D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp.
    DateTime=2018-03-09T01:42:59.6184651Z
SnapshotUploader.exe Information: 0 : Matched 2 PDB(s) with local files.
    DateTime=2018-03-09T01:42:59.6809606Z
SnapshotUploader.exe Information: 0 : Stamp does not want any of our matched PDBs.
    DateTime=2018-03-09T01:42:59.8059929Z
SnapshotUploader.exe Information: 0 : Deleted D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp
    DateTime=2018-03-09T01:42:59.8530649Z
```

> [!NOTE]
> Yukarıdaki örnekte Microsoft.applicationınsights.snapshotcollectya da NuGet Paketi 1.2.0 sürümüne bulunur. Önceki sürümlerde, yükleyici işlemi olarak adlandırılır `MinidumpUploader.exe` ve günlük daha az ayrıntılı olarak verilmiştir.

Önceki örnekte, izleme anahtarını olan `c12a605e73c44346a984e00000000000`. Bu değer, uygulamanızın izleme anahtarını eşleşmesi gerekir.
Mini döküm dosyasında bir anlık görüntü kimliği ile ilişkili olduğu `139e411a23934dc0b9ea08a626db16c5`. Application Insights Analytics ilişkili özel durum telemetrisi bulmak için bu kimliği daha sonra kullanabilirsiniz.

Karşıya yükleyen her 15 dakikada hakkında yeni pdb tarar. Bir örneği aşağıda verilmiştir:

```
SnapshotUploader.exe Information: 0 : PDB rescan requested.
    DateTime=2018-03-09T01:47:19.4457768Z
SnapshotUploader.exe Information: 0 : Scanning D:\home\site\wwwroot for local PDBs.
    DateTime=2018-03-09T01:47:19.4457768Z
SnapshotUploader.exe Information: 0 : Local PDB scan complete. Found 2 PDB(s).
    DateTime=2018-03-09T01:47:19.4614027Z
SnapshotUploader.exe Information: 0 : Deleted PDB scan marker : D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\6368.pdbscan
    DateTime=2018-03-09T01:47:19.4614027Z
```

Uygulamalar için _olmayan_ App Service'te barındırılan, mini döküm dosyalarında aynı klasörde yükleyici günlükleri şunlardır: `%TEMP%\Dumps\<ikey>` (burada `<ikey>` izleme anahtarınız).

## <a name="troubleshooting-cloud-services"></a>Bulut Hizmetleri sorunlarını giderme
Bulut hizmetlerindeki roller için varsayılan geçici klasör kayıp anlık görüntüleri için önde gelen mini döküm dosyaları tutmak için çok küçük olabilir.
İhtiyaç duyulan alanı uygulamanızı eşzamanlı anlık görüntü sayısı ve toplam çalışma kümesine bağlıdır.
Çalışma kümesi 32-bit ASP.NET web rolü, genellikle 200 MB ile en fazla 500 MB arasındadır.
En az iki eş zamanlı anlık görüntüler için izin verin.
Örneğin, uygulamanız çalışma kümesi toplam 1 GB kullanıyorsa, en az 2 anlık görüntülerini depolamak için GB disk alanı olduğundan emin olmalısınız.
Anlık görüntüler için ayrılmış bir yerel kaynak ile bulut hizmeti rolünü yapılandırmak için aşağıdaki adımları izleyin.

1. Yeni bir yerel kaynak, bulut hizmet tanımı (.csdef) dosyasını düzenleyerek bulut hizmetinize ekleyin. Aşağıdaki örnek adlı bir kaynak tanımlar `SnapshotStore` boyutu 5 GB.
   ```xml
   <LocalResources>
     <LocalStorage name="SnapshotStore" cleanOnRoleRecycle="false" sizeInMB="5120" />
   </LocalResources>
   ```

2. İşaret eden bir ortam değişkeni eklemek için rolün başlatma kodunu değiştirmek `SnapshotStore` yerel kaynak. Çalışan rolleri için kodu, rolün eklenmelidir `OnStart` yöntemi:
   ```csharp
   public override bool OnStart()
   {
       Environment.SetEnvironmentVariable("SNAPSHOTSTORE", RoleEnvironment.GetLocalResource("SnapshotStore").RootPath);
       return base.OnStart();
   }
   ```
   Web rolleri (ASP.NET), web uygulamanızın için kod eklenmelidir `Application_Start` yöntemi:
   ```csharp
   using Microsoft.WindowsAzure.ServiceRuntime;
   using System;

   namespace MyWebRoleApp
   {
       public class MyMvcApplication : System.Web.HttpApplication
       {
          protected void Application_Start()
          {
             Environment.SetEnvironmentVariable("SNAPSHOTSTORE", RoleEnvironment.GetLocalResource("SnapshotStore").RootPath);
             // TODO: The rest of your application startup code
          }
       }
   }
   ```

3. Tarafından kullanılan geçici klasör konumu geçersiz kılmak için rolün Applicationınsights.config dosyasını güncelleştirme `SnapshotCollector`
   ```xml
   <TelemetryProcessors>
    <Add Type="Microsoft.ApplicationInsights.SnapshotCollector.SnapshotCollectorTelemetryProcessor, Microsoft.ApplicationInsights.SnapshotCollector">
      <!-- Use the SnapshotStore local resource for snapshots -->
      <TempFolder>%SNAPSHOTSTORE%</TempFolder>
      <!-- Other SnapshotCollector configuration options -->
    </Add>
   </TelemetryProcessors>
   ```

## <a name="overriding-the-shadow-copy-folder"></a>Gölge kopya klasörü geçersiz kılma

Anlık görüntü toplayıcının başlatıldığında anlık görüntü yükleyici işlemi çalıştırmak için uygun diskte bir klasörü bulmayı dener. Seçtiğiniz klasör gölge kopya klasör olarak bilinir.

Anlık görüntü toplayıcının, bazı iyi bilinen konumları, anlık görüntü Uploader ikili dosyalarını kopyalamak için izinlere sahip sağlamaktan denetler. Aşağıdaki ortam değişkenlerini kullanılır:
- Fabric_Folder_App_Temp
- LOCALAPPDATA
- APPDATA
- TEMP

Uygun bir klasör bulunamazsa, anlık görüntü toplayıcının hata bildiren raporları _"uygun gölge kopya klasörü bulunamadı."_

Kopyalama başarısız olursa, anlık görüntü toplayıcısı raporları bir `ShadowCopyFailed` hata.

Yükleyici başlatılamıyor ise, anlık görüntü toplayıcısı raporları bir `UploaderCannotStartFromShadowCopy` hata. Genellikle ileti gövdesini içeren `System.UnauthorizedAccessException`. Bu hata genellikle uygulama sınırlı izinlere sahip bir hesap altında çalıştığı için oluşur. Hesabın gölge kopya klasörüne yazmak için yeterli izne sahip, ancak kod yürütmek için izinlere sahip değil.

Bu hatalar genellikle başlatma sırasında meydana olduğundan, bunlar genellikle tarafından izlenmesi bir `ExceptionDuringConnect` hata bildiren _"Yükleyicisi başlatılamadı."_

Bu hataların geçici olarak çözmek için gölge kopya klasörü kullanarak el ile belirtebilirsiniz `ShadowCopyFolder` yapılandırma seçeneği. Örneğin, Applicationınsights.config kullanma:

   ```xml
   <TelemetryProcessors>
    <Add Type="Microsoft.ApplicationInsights.SnapshotCollector.SnapshotCollectorTelemetryProcessor, Microsoft.ApplicationInsights.SnapshotCollector">
      <!-- Override the default shadow copy folder. -->
      <ShadowCopyFolder>D:\SnapshotUploader</ShadowCopyFolder>
      <!-- Other SnapshotCollector configuration options -->
    </Add>
   </TelemetryProcessors>
   ```

Veya, bir .NET Core uygulaması ile appsettings.json kullanıyorsanız:

   ```json
   {
     "ApplicationInsights": {
       "InstrumentationKey": "<your instrumentation key>"
     },
     "SnapshotCollectorConfiguration": {
       "ShadowCopyFolder": "D:\\SnapshotUploader"
     }
   }
   ```

## <a name="use-application-insights-search-to-find-exceptions-with-snapshots"></a>Application ınsights'ı özel durumların anlık görüntülerle bulmak için arama yapın

Anlık görüntü oluşturulduğunda, özel durum oluşturmaya bir anlık görüntü kimliği ile etiketlenir. Özel durum telemetrisi Application Insights'a bildirildiğinde bu anlık görüntü kimliği bir özel özellik olarak dahil edilir. Kullanarak **arama** Application Insights ile tüm telemetri bulabilirsiniz `ai.snapshot.id` özel özellik.

1. Azure portalında Application Insights kaynağınıza göz atın.
2. **Ara**'ya tıklayın.
3. Tür `ai.snapshot.id` arama metin kutusu ve Enter tuşuna basın.

![Portalda bir anlık görüntü kimliği ile telemetri arayın](./media/snapshot-debugger/search-snapshot-portal.png)

Bu arama sonuç vermedi, anlık görüntü yok seçili zaman aralığındaki uygulamanız için Application ınsights'ı bildirildi.

Yükleyici günlükleri belirli bir anlık görüntüye Kimliğinden aramak için bu kimliği arama kutusuna yazın. Karşıya yüklenen bildiğiniz bir anlık görüntü için telemetri bulamazsanız, aşağıdaki adımları izleyin:

1. İzleme anahtarını doğrulayarak doğru Application Insights kaynağı arıyorsanız denetleyin.

2. Yükleyici günlük itibaren zaman damgası kullanarak, o zaman aralığınızı kapsayacak şekilde arama zaman aralığı filtresini ayarlayın.

Bu anlık görüntü Kimliğe sahip bir özel durum yine de görmüyorsanız, özel durum telemetrisi Application ınsights'ı bildirilen değildi. Bu durum, anlık görüntü sürdüğünü sonra uygulamanızı kilitlendi, ancak özel durum telemetrisi bildirilen önce gerçekleşebilir. Bu durumda, App Service günlükleri altında denetleyin `Diagnose and solve problems` beklenmeyen yeniden başlatmaları olup olmadığını görmek için veya işlenmemiş özel durumlar.

## <a name="edit-network-proxy-or-firewall-rules"></a>Ağ proxy veya güvenlik duvarı kurallarını Düzenle

Uygulamanız İnternet'e bir ara sunucu veya bir güvenlik duvarı üzerinden bağlanıyorsa, Snapshot Debugger hizmetiyle iletişim kurmak için uygulamanıza izin vermek için kuralları düzenlemek gerekebilir. İşte [IP adresleri ve anlık görüntü hata ayıklayıcısı tarafından kullanılan bağlantı noktaları listesini](../../azure-monitor/app/ip-addresses.md#snapshot-debugger).
