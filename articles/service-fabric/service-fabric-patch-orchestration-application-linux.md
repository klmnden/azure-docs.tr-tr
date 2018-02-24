---
title: "Linux için Azure Service Fabric düzeltme eki orchestration uygulama | Microsoft Docs"
description: "Bir Linux Service Fabric kümesi işletim sistemi düzeltme eki uygulama otomatikleştirmek için uygulama."
services: service-fabric
documentationcenter: .net
author: novino
manager: timlt
editor: 
ms.assetid: de7dacf5-4038-434a-a265-5d0de80a9b1d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 1/22/2018
ms.author: nachandr
ms.openlocfilehash: dac8068705e284b04d84d128eb1ce62c459d44ff
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="patch-the-linux-operating-system-in-your-service-fabric-cluster"></a>Service Fabric kümesi Linux işletim sistemi düzeltme eki

> [!div class="op_single_selector"]
> * [Windows](service-fabric-patch-orchestration-application.md)
> * [Linux](service-fabric-patch-orchestration-application-linux.md)
>
>

Düzeltme eki orchestration uygulama kapalı kalma süresi olmadan bir Service Fabric kümesindeki düzeltme eki uygulama işletim sistemi otomatikleştiren bir Azure Service Fabric uygulamasıdır.

Düzeltme eki orchestration uygulama aşağıdaki özellikleri sağlar:

- **Otomatik işletim sistemi güncelleştirme yüklemesini**. İşletim sistemi güncelleştirmeleri otomatik olarak karşıdan yüklenir ve. Küme düğümleri küme kapalı kalma süresi olmadan gerektiği gibi yeniden başlatılır.

- **Küme durumunu algılayan düzeltme eki uygulama ve sistem durumu tümleştirme**. Güncelleştirmeler uygulanırken düzeltme eki orchestration uygulama küme düğümlerinin sistem durumunu izler. Küme düğümü yükseltilmiş bir düğümde aynı anda bir saat ya da bir yükseltme etki alanı olan. Küme durumunu düzeltme eki uygulama işlemi nedeniyle kullanılamaz hale gelirse, düzeltme eki uygulama sorun aggravating önlemek için durduruldu.

## <a name="internal-details-of-the-app"></a>Uygulamanın iç ayrıntıları

Düzeltme eki orchestration uygulama aşağıdaki bileşenleri oluşur:

- **Düzenleyicisi hizmeti**: Bu durum bilgisi olan hizmet sorumludur:
    - İşletim sistemi güncelleştirme işi tüm küme üzerinde Eşgüdümleme.
    - Tamamlanan işletim sistemi güncelleştirme işlemleri sonucunu depolamak.
- **Düğüm Aracısı hizmeti**: Bu durum bilgisiz hizmet tüm Service Fabric küme düğümleri üzerinde çalışır. Hizmet için sorumludur:
    - Linux düğümü aracı arka plan önyükleme.
    - Arka plan programı hizmeti izleme.
- **Düğüm Aracısı arka plan programı**: Bu Linux arka plan programı hizmeti, üst düzey bir ayrıcalık (kök) çalışır. Buna karşılık, düğüm Aracısı hizmeti ve Coordinator hizmeti düşük düzeyli ayrıcalık çalıştırın. Hizmet, tüm küme düğümlerinde aşağıdaki güncelleştirme işlerini gerçekleştirmek için sorumludur:
    - Düğümde otomatik işletim sistemi güncelleştirme devre dışı bırakılıyor.
    - Karşıdan yükleme ve işletim sistemi güncelleştirme ilkesine göre kullanıcı sağlamıştır.
    - Makine post işletim sistemi güncelleştirme yüklemesini yeniden başlatma gerekirse.
    - İşletim sistemi güncelleştirmelerini sonuçlarını Coordinator hizmeti yükleniyor.
    - Tüm yeniden denemeler tükenmesinden sonra bir işlem başarısız oldu durumda raporlama durumu raporları.

> [!NOTE]
> Düzeltme eki orchestration app Service Fabric onarım Yöneticisi sistem hizmeti devre dışı bırakın veya düğüm etkinleştirmek ve sistem durumu denetimleri gerçekleştirmek için kullanır. Düzeltme eki orchestration uygulama tarafından oluşturulan onarım görevi her düğüm için güncelleştirme ilerleme durumunu izler.

## <a name="prerequisites"></a>Önkoşullar

### <a name="ensure-that-your-azure-vms-are-running-ubuntu-1604"></a>Azure Vm'leriniz Ubuntu 16.04 çalıştığından emin olun
Bu belge, Ubuntu 16.04 yazma zamanında (`Xenial Xerus`) desteklenen tek sürümdür.

### <a name="ensure-that-the-service-fabric-linux-cluster-is-version-61x-and-above"></a>Service fabric linux kümesi sürüm olduğundan emin olun 6.1.x ve üstü

Düzeltme eki orchestration uygulama linux kullanır, yalnızca service fabric çalışma zamanı sürümünde kullanılabilen bazı özellikleri çalışma zamanının 6.1.x ve üstü.

### <a name="enable-the-repair-manager-service-if-its-not-running-already"></a>(Bu zaten çalışmıyorsa) onarım Yöneticisi hizmetini etkinleştirme

Düzeltme eki orchestration uygulama kümede etkinleştirilmesi için onarım Yöneticisi sistem hizmeti gerektirir.

#### <a name="azure-clusters"></a>Azure kümeleri

Gümüş Azure linux kümeleri ve altın dayanıklılık katmanı sahip onarım Yöneticisi hizmeti varsayılan olarak etkindir. Varsayılan olarak, Bronz dayanıklılık katmanı Azure kümelerde etkin onarım Yöneticisi hizmeti yok. Hizmet zaten etkin değilse, Service Fabric Explorer Sistem Hizmetleri bölümünde çalışmasını görebilirsiniz.

##### <a name="azure-portal"></a>Azure portalına
Kümenin kurma sırasında onarım Yöneticisi Azure portalından etkinleştirebilirsiniz. Seçin **dahil onarım Yöneticisi** altında seçeneği **eklenti özellikleri** küme yapılandırması zaman.
![Azure portalından etkinleştirme onarım Yöneticisi'nin resmi](media/service-fabric-patch-orchestration-application/EnableRepairManager.png)

##### <a name="azure-resource-manager-deployment-model"></a>Azure Resource Manager dağıtım modeli
Alternatif olarak kullanabileceğiniz [Azure Resource Manager dağıtım modeli](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) yeni ve mevcut Service Fabric kümeleri üzerinde onarım Yöneticisi hizmeti etkinleştirmek için. Şablonu dağıtmak istediğiniz kümenin alın. Örnek şablonları kullanabilir veya özel bir Azure Resource Manager dağıtım modeli şablon oluşturabilirsiniz. 

Onarım Yöneticisi hizmetini kullanarak etkinleştirmek için [Azure Resource Manager dağıtım modeli şablonu](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm):

1. İlk denetleyin `apiversion` ayarlanır `2017-07-01-preview` için `Microsoft.ServiceFabric/clusters` kaynak. Farklı sonra güncelleştirmek gereken `apiVersion` değerine `2017-07-01-preview` veya üstü:

    ```json
    {
        "apiVersion": "2017-07-01-preview",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
    }
    ```

2. Aşağıdakileri ekleyerek onarım Yöneticisi hizmeti şimdi etkinleştirmek `addonFeatures` sonra bölümünde `fabricSettings` bölümü:

    ```json
    "fabricSettings": [
        ...      
    ],
    "addonFeatures": [
        "RepairManager"
    ],
    ```

3. Bu değişikliklerle küme şablonunuzu güncelleştirildikten sonra bunları uygulamak ve son yükseltme sağlayabilirsiniz. Kümenizde çalışan onarım Yöneticisi sistem hizmeti şimdi görebilirsiniz. Çağrılır `fabric:/System/RepairManagerService` Service Fabric Explorer Sistem Hizmetleri bölümünde. 

### <a name="standalone-on-premises-clusters"></a>Tek başına şirket içi kümeleri

Tek başına Service Fabric Linux kümeleri, bu belgenin yazıldığı sırada desteklenmez.

### <a name="disable-automatic-os-update-on-all-nodes"></a>Tüm düğümlerde otomatik işletim sistemi güncelleştirme devre dışı bırak

Otomatik işletim sistemi güncelleştirmeleri kullanılabilirlik kaybına neden ve veya çalışan uygulamaların davranışı değiştirebilirsiniz. Düzeltme eki orchestration uygulama varsayılan olarak, bu tür senaryoları önlemek için her küme düğümünde otomatik işletim sistemi güncelleştirme devre dışı bırakmak çalışır.
Ubuntu için [katılımsız yükseltmeler](https://help.ubuntu.com/community/AutomaticSecurityUpdates) düzeltme eki orchestration uygulama tarafından devre dışı bırakılır.

## <a name="download-the-app-package"></a>Uygulama paketi yükle

Uygulamayı karşıdan [bağlantı karşıdan](https://go.microsoft.com/fwlink/?linkid=867984).

## <a name="configure-the-app"></a>Uygulamayı yapılandırma

Düzeltme eki orchestration uygulamanın davranışı gereksinimlerinizi karşılayacak şekilde yapılandırılabilir. Uygulama oluşturma veya güncelleştirme işlemi sırasında uygulama parametresini geçirerek varsayılan değerleri geçersiz. Uygulama parametreleri belirterek sağlanabilir `ApplicationParameter` için `Start-ServiceFabricApplicationUpgrade` veya `New-ServiceFabricApplication` cmdlet'leri.

|Parametre        |**Tür**                          | **Ayrıntılar**|
|:-|-|-|
|MaxResultsToCache    |Uzun                              | Önbelleğe alınması gereken güncelleştirme sonuçlarının maksimum sayısı. <br>Varsayılan değer 3000 varsayılır: <br> -Düğüm sayısı 20'dir. <br> -Ayda bir düğümde gerçekleştiği güncelleştirme sayısı beştir. <br> -İşlemi başına sonuç sayısı 10 olabilir. <br> -Son üç ay için sonuçları depolanması gerekir. |
|TaskApprovalPolicy   |Enum <br> {NodeWise, UpgradeDomainWise}                          |Service Fabric küme düğümleri arasında güncelleştirmeleri yüklemek için Koordinatör hizmeti tarafından kullanılacak ilkeyi TaskApprovalPolicy gösterir.<br>                         İzin verilen değerler: <br>                                                           <b>NodeWise</b>. Yüklü bir düğümün aynı anda güncelleştirmelerdir. <br>                                                           <b>UpgradeDomainWise</b>. Yüklü bir yükseltme etki alanı aynı anda güncelleştirmelerdir. (En güncelleştirme için bir yükseltme etki alanına ait tüm düğümlerde gidebilirsiniz.)
| UpdateOperationTimeOutInMinutes | Int <br>(Varsayılan: 180)                   | (İndirme veya yükleme) herhangi bir güncelleştirme işlemi için zaman aşımını belirtir. İşlemi belirtilen zaman aşımı süresi içinde tamamlanmazsa durdurulur.       |
| RescheduleCount      | Int <br> (Varsayılan: 5).                  | Bir işlem kalıcı olarak başarısız olursa en fazla kaç kez işletim sistemi hizmet reschedules güncelleştirin.          |
| RescheduleTimeInMinutes  | Int <br>(Varsayılan: 30). | Hata devam ederse durumunda hangi hizmet Arabelleksiz yeniden zamanlama sayısı işletim sistemi güncelleştirme aralığı. |
| UpdateFrequency           | Virgülle ayrılmış dize (varsayılan: "Haftalık, Çarşamba, 7:00:00")     | İşletim sistemi yükleme sıklığını kümede güncelleştirir. Biçim ve olası değerler şunlardır: <br>-Örneğin, aylık, 5, 12:22:32 aylık, gg ss. <br> -Örneğin, haftalık, Salı, 12:22:32 için haftalık, gün, ss.  <br> -Örneğin, günlük, 12:22:32 günlük, ss.  <br> -Yok, bu güncelleştirme yapılması döndürmemelidir gösterir.  <br><br> Tüm saatler UTC biçimindedir.|
| UpdateClassification | Virgülle ayrılmış dize (varsayılan: "securityupdates") | Küme düğümlerinde yüklü güncelleştirmeleri türü. Kabul edilebilir değerler securityupdates, tüm. <br> -securityupdates - yalnızca güvenlik güncelleştirmelerini yükler <br> -Tümü - tüm kullanılabilir güncelleştirmeleri gelen apt yüklenir.|
| ApprovedPatches | Virgülle ayrılmış dize (varsayılan: "") | Küme düğümlerinde yüklü onaylanmış güncelleştirmeleri listesidir. Virgülle ayrılmış liste onaylanmış paketler ve isteğe bağlı olarak istenen hedef sürümünü içerir.<br> Örneğin: "yardımcı programları apt = 1.2.10ubuntu1, python3 jwt, aktarım https apt < 1.2.194, libsystemd0 > 229 4ubuntu16 =" <br> Yukarıdaki yüklenir <br> -apt-yardımcı programları apt-önbellekte kullanılabilir durumdaysa sürüm 1.2.10ubuntu1 ile. Ardından bu belirli sürümü kullanılabilir durumda değilse, Hayır op hale gelir. <br> -en son sürüme yükseltme python3 jwt. Sonra paketi yoksa, Hayır op değil. <br> -değerinden 1.2.194 yüksek sürümüne aktarım https apt yükseltme. Ardından bu sürümü mevcut değilse, Hayır op olduğu. <br> -büyük 229 4ubuntu16 eşit olan en yüksek sürüm libsystemd0 yükseltme. Ardından bu tür bir sürümü mevcut değil, Hayır op olur.|
| RejectedPatches | Virgülle ayrılmış dize (varsayılan: "") | Küme düğümlerine yüklenmemelidir güncelleştirmelerinin listesidir <br> Örneğin: "bash, sudo" <br> Yukarıdaki bash, tüm güncelleştirmeleri almasını sudo çıkışı filtreler. |


> [!TIP]
> İşletim sistemi hemen gerçekleşecek şekilde güncelleştirme istiyorsanız ayarlayın `UpdateFrequency` uygulama dağıtım süresini göre. Örneğin, beş düğümlü test kümesi olduğunu ve yaklaşık 5: 00'da uygulama dağıtmayı planladığınız varsayalım UTC. Uygulama yükseltme veya dağıtım en 30 dakika sürer olduğunu varsayarsak, UpdateFrequency "Günlük, 17:30:00." ayarlayın.

## <a name="deploy-the-app"></a>Uygulamayı dağıtma

1. Küme, tüm önkoşul adımları tamamlama tarafından hazırlayın.
2. Düzeltme eki orchestration uygulamayı başka bir Service Fabric uygulaması gibi dağıtın. PowerShell veya Azure Service Fabric CLI kullanarak uygulama dağıtabilirsiniz. Adımları [PowerShell kullanarak uygulamaları dağıtma ve Kaldır](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications) veya [Azure Service Fabric CLI kullanarak uygulamayı dağıtın](https://docs.microsoft.com/azure/service-fabric/scripts/cli-deploy-application)
3. Geçişi dağıtım zamanında uygulama yapılandırmak için `ApplicationParamater` için `New-ServiceFabricApplication` sağlanan cmdlet'ini veya komut dosyaları. Size kolaylık olması için powershell (Deploy.ps1) ve (Deploy.sh) bash betiklerini uygulamayı ile birlikte sağlanır. Betik kullanmak için:

    - Service Fabric kümeye bağlanın.
    - Dağıtım betiğini yürütün. İsteğe bağlı olarak uygulama parametresi komut dosyasına geçirin. Örneğin:.\Deploy.ps1 - ApplicationParameter @{UpdateFrequency "Günlük, 11:00:00" =} veya./Deploy.sh "{\"UpdateFrequency\":\"günlük, 11:00:00\"}" 

> [!NOTE]
> Komut dosyası ve uygulama klasörü PatchOrchestrationApplication aynı dizinde tutun.

## <a name="upgrade-the-app"></a>Uygulama yükseltme

Var olan bir düzeltme eki orchestration uygulamaya yükseltmek için adımları [PowerShell kullanarak Service Fabric uygulama yükseltme](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-upgrade-tutorial-powershell) veya [Azure Service Fabric CLI kullanarak Service Fabric uygulama yükseltme](https://docs.microsoft.com/azure/service-fabric/service-fabric-sfctl-application#sfctl-application-upgrade)

## <a name="remove-the-app"></a>Uygulamayı kaldırma

Uygulamayı kaldırmak için adımları [PowerShell kullanarak uygulamaları dağıtma ve Kaldır](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications) veya [Azure Service Fabric CLI kullanarak bir uygulamayı kaldırma](https://docs.microsoft.com/azure/service-fabric/service-fabric-sfctl-application#sfctl-application-delete)

Size kolaylık olması için powershell (Undeploy.ps1) ve (Undeploy.sh) bash betiklerini uygulamayı ile birlikte sağlanır. Betik kullanmak için:

  - Service Fabric kümeye bağlanın.
  - Komut dosyası Undeploy.ps1 veya Undeploy.sh yürütme

> [!NOTE]
> Komut dosyası ve uygulama klasörü PatchOrchestrationApplication aynı dizinde tutun.

## <a name="view-the-update-results"></a>Güncelleştirme sonuçları görüntüleme

Düzeltme eki orchestration uygulama kullanıcıya geçmiş sonuçlarını görüntülemek için REST API'lerini kullanıma sunar. Aşağıdaki örnek oluşur: ```testadm@bronze000001:~$ curl -X GET http://10.0.0.5:20002/PatchOrchestrationApplication/v1/GetResults```
```json
[ 
  { 
    "NodeName": "_bronze_0", 
    "UpdateOperationResults": [ 
      { 
        "OperationResult": "succeeded", 
        "NodeName": "_bronze_0", 
        "OperationTime": "2017-11-21T12:39:29.0435917Z", 
        "UpdateDetails": [ 
          { 
            "UpdateId": "linux-cloud-tools-azure:amd64=4.11.0.1015.15", 
            "ResultCode": "succeeded" 
          }, 
          { 
            "UpdateId": "linux-headers-azure:amd64=4.11.0.1015.15", 
            "ResultCode": "succeeded" 
          }, 
          { 
            "UpdateId": "linux-image-azure:amd64=4.11.0.1015.15", 
            "ResultCode": "succeeded" 
          }, 
          { 
            "UpdateId": "linux-tools-azure:amd64=4.11.0.1015.15", 
            "ResultCode": "succeeded" 
          }, 
          { 
            "UpdateId": "python3-apport:amd64=2.20.1-0ubuntu2.13", 
            "ResultCode": "succeeded" 
          }, 
        ], 
        "OperationType": "installation", 
        "UpdateClassification": "securityupdates", 
        "UpdateFrequency": "Daily, 7:00:00", 
        "RebootRequired": true, 
        "ApprovedList": "", 
        "RejectedList": "" 
      } 
    ] 
  } 
] 
```

JSON alanları aşağıda açıklanmıştır:

Alan | Değerler | Ayrıntılar
-- | -- | --
OperationResult | 0 - başarılı<br> 1 - hatalarıyla başarılı oldu<br> 2 - başarısız oldu<br> 3 - durduruldu<br> 4 - zaman aşımı ile iptal edildi | Genel işlemin (genellikle bir veya daha fazla güncelleştirme yüklemesini içeren) sonucunu gösterir.
ResultCode | OperationResult aynı | Bu alan tek güncelleştirme için yükleme işleminin sonucu gösterir.
OperationType | 1 - yükleme<br> 0 - arayın ve yükleyin.| Varsayılan olarak sonuçlarda gösterilen yalnızca OperationType bir yüklemedir.
UpdateClassification | Varsayılan değer "securityupdates" | Güncelleştirme işlemi sırasında yüklenen güncelleştirmeleri türü
UpdateFrequency | Varsayılan değer "Haftalık, Çarşamba, 7:00:00" | Bu güncelleştirme için yapılandırılan sıklık güncelleştirin.
RebootRequired | TRUE - yeniden başlatma gerekli<br> false - yeniden başlatma gerekli değildi | Yeniden başlatma güncelleştirmeleri yüklemesini tamamlamak için gerekli gösterir.
ApprovedList | Varsayılan değer "" | Bu güncelleştirme için onaylanan düzeltme eklerinin listesini
RejectedList | Varsayılan değer "" | Bu güncelleştirme için reddedilen düzeltme eklerinin listesini

Hiçbir güncelleştirme henüz zamanladıysanız JSON sonuç boştur.

Sorgu güncelleştirme sonuçları kümeye oturum açın. Ardından Coordinator hizmeti birincil çoğaltma adresi bulmak ve tarayıcı URL'den isabet: http://&lt;çoğaltma IP&gt;:&lt;ApplicationPort&gt;/PatchOrchestrationApplication/v1/GetResults .

REST uç noktası Coordinator hizmeti için dinamik bir bağlantı noktası var. Tam URL'yi denetlemek için Service Fabric Explorer bakın. Örneğin, sonuçların kullanılabilir `http://10.0.0.7:20000/PatchOrchestrationApplication/v1/GetResults`.

![REST uç noktasını görüntüsü](media/service-fabric-patch-orchestration-application/Rest_Endpoint.png)

## <a name="diagnosticshealth-events"></a>Tanılama/sistem durumu olayları

### <a name="diagnostic-logs"></a>Tanılama günlükleri

Düzeltme eki orchestration uygulama günlükleri Service Fabric çalışma zamanı günlükleri bir parçası olarak toplanır.

Tanılama Aracı/ardışık düzen tercih ettiğiniz aracılığıyla günlükleri yakalamak istediğiniz durumda. Düzeltme eki orchestration uygulama aracılığıyla olaylarını günlüğe kaydedecek şekilde sabit sağlayıcısını kimlikleri kullanır [eventsource](https://docs.microsoft.com/dotnet/api/system.diagnostics.tracing.eventsource?view=netstandard-2.0)

- e39b723c-590c-4090-abb0-11e3e6616346
- fc0028ff-bfdc-499f-80dc-ed922c52c5e9
- 24afa313-0d3b-4c7c-b485-1047fd964b60
- 05dc046c-60e9-4ef7-965e-91660adffa68

### <a name="health-reports"></a>Sistem durumu raporları

Düzeltme eki orchestration uygulama sistem durumu raporlarının Düzenleyicisi hizmeti veya düğüm Aracısı hizmeti karşı aşağıdaki durumlarda da yayımlar:

#### <a name="an-update-operation-failed"></a>Bir güncelleştirme işlemi başarısız oldu

Bir düğümde güncelleştirme işlemi başarısız olursa, bir sistem durumu raporu karşı düğüm Aracısı hizmeti oluşturulur. Sistem Durumu raporu ayrıntılarını sorunlu bir düğüm adı içeriyor.

Düzeltme eki uygulama sorunlu bir düğüm üzerinde başarıyla tamamlandıktan sonra raporu otomatik olarak temizlenir.

#### <a name="the-node-agent-daemon-service-is-down"></a>Düğüm Aracısı arka plan programı hizmeti çalışmıyor

Düğüm Aracısı arka plan programı hizmeti kapalı bir düğümde ise, bir uyarı düzeyi sistem durumu raporu karşı düğüm Aracısı hizmeti oluşturulur.

#### <a name="the-repair-manager-service-is-not-enabled"></a>Onarım Yöneticisi hizmeti etkin değil

Onarım Yöneticisi hizmeti kümede bulunmazsa uyarı düzeyi sistem durumu raporu Düzenleyicisi hizmeti için oluşturulur.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

Q. **Düzeltme eki orchestration uygulama çalışırken neden bir hata durumuna my küme görüyor?**

A. Yükleme işlemi sırasında düzeltme eki orchestration uygulama devre dışı bırakır veya düğümleri yeniden başlatılır. Bu işlem, büyük bir geçici olarak giderek küme durumunu neden olabilir.

İlke uygulama için bağlı olarak, herhangi bir düğümün bir düzeltme eki uygulama işlemi sırasında gidebilirsiniz *veya* tüm yükseltme etki alanı aynı anda Git aşağı.

Yükleme sonuna düğümlerin yeniden iler hale getirilir yeniden başlatma gönderin.

Bir hata durumu kümeyi aşağıdaki örnekte, geçici olarak oluştu çünkü iki düğüm olan aşağı ve MaxPercentageUnhealthyNodes ilke ihlal. Düzeltme eki uygulama işlemi devam eden olana kadar geçici bir hatadır.

![Sağlıksız küme görüntüsü](media/service-fabric-patch-orchestration-application/MaxPercentage_causing_unhealthy_cluster.png)

Sorun devam ederse, sorun giderme bölümüne bakın.

Q. **Düzeltme eki orchestration uygulama uyarı durumunda**

A. Uygulamaya karşı gönderilen bir sistem durumu raporu kök neden olup olmadığını denetleyin. Genellikle, uyarı sorunun ayrıntılarını içerir. Sorun geçici ise, uygulama otomatik kurtarma bu durumdan olması beklenir.

Q. **My küme sağlıksız ise ve Acil işletim sistemi güncelleştirmesi yapmanız ne yapabilirim?**

A. Küme sağlıksız durumdayken düzeltme eki orchestration uygulama güncelleştirmelerini yüklemez. Düzeltme eki orchestration uygulama iş akışı engelini kaldırmak için küme sağlıklı bir duruma getirin.

Q. **Neden kümelerinde düzeltme eki uygulama kadar çalıştırmak için sürer?**

A. Düzeltme eki orchestration uygulama tarafından gereken süre genellikle aşağıdaki etkenlere bağlıdır:

- Düzenleyici hizmet ilkesi. 
  - Varsayılan ilke `NodeWise`, sonuçlarını aynı anda yalnızca tek bir düğüme düzeltme eki uygulama içinde. Özellikle varsa daha büyük bir küme, kullanmanız önerilir `UpgradeDomainWise` küme arasında daha hızlı düzeltme eki uygulama elde etmek için ilke.
- İndirme ve yükleme için kullanılabilir güncelleştirmeleri sayısı. 
- Karşıdan yüklemek ve bir güncelleştirmeyi yüklemek için gereken ortalama süre, birkaç saat aşamaz.
- VM ve ağ bant genişliği performans.

Q. **Güvenlik güncelleştirmeleri olan mu düzeltme eki orchestration uygulama hangi güncelleştirmelerin nasıl karar verir.**

A. Düzeltme eki orchestration uygulama distro özgü mantığı hangi güncelleştirmelerin güvenlik güncelleştirmeleri kullanılabilir güncelleştirmeleri arasında belirlemek için kullanır. Örneğin: uygulama arar arşivler $RELEASE güncelleştirmeleri ubuntu içinde-güvenlik, $RELEASE-güncelleştirmeleri ($RELEASE xenial = ya da linux standart temel yayın sürümü). 

 
Q. **Belirli bir paket sürümünü açın nasıl kilitlemek üzere?**

A. Belirli bir sürüme paketlerinizi kilitlemek için ApprovedPatches ayarları kullanın. 


Q. **Otomatik Güncelleştirmeler Ubuntu etkin ne olur?**

A. Kümenizde düzeltme eki orchestration uygulama yükleme hemen yükseltmeler katılımsız küme düğümünde devre dışı. Tüm düzenli güncelleştirme iş akışını düzeltme eki orchestration uygulama tarafından yönlendirilen.
Küme genelinde ortamı tutarlılığını sağlamak için düzeltme düzenlemesi yalnızca uygulama güncelleştirmelerine yüklemenizi öneririz. 
 
Q. **Yükseltme orchestration uygulama kullanılmayan paketleri temizleme sonrası düzeltme?**

A. Evet, yükleme sonrası adımlar bir parçası olarak temizleme gerçekleşir. 

## <a name="troubleshooting"></a>Sorun giderme

### <a name="a-node-is-not-coming-back-to-up-state"></a>Bir düğüm geri durumu yukarı geliyor değil

**Düğümü devre dışı bırakma durumunda olduğundan takılmış olabilir**:

Güvenlik onay bekliyor. Bu durumu düzeltmek için sağlam bir durumda yeterli düğüm kullanılabilir olduğundan emin olun.

**Düğümü devre dışı durumda olduğundan takılmış olabilir**:

- Düğüm el ile devre dışı bırakıldı.
- Düğüm, devam eden Azure altyapı iş nedeniyle devre dışı bırakıldı.
- Düğüm düğüm düzeltme eki için düzeltme eki orchestration uygulama tarafından geçici olarak devre dışı bırakıldı.

**Düğüm aşağı bir durumda olduğundan takılmış olabilir**:

- Düğüm aşağı durumda el ile askıya alınmış.
- Düğüm (hangi düzeltme eki orchestration uygulama tarafından tetiklenen) yeniden yapılıyor.
- Düğümü hatalı VM veya makine ya da ağ bağlantısı sorunları nedeniyle çalışmıyor.

### <a name="updates-were-skipped-on-some-nodes"></a>Güncelleştirmeleri bazı düğümler üzerinde atlandı

Yeniden zamanlama ilkesine göre bir güncelleştirmeyi yüklemek düzeltme eki orchestration uygulama çalışır. Düğüm kurtarmak ve uygulama ilkesi göre güncelleştirme atlamak hizmeti çalışır.

Böyle bir durumda, bir uyarı düzeyi sistem durumu raporu karşı düğüm Aracısı hizmeti oluşturulur. Güncelleştirme için sonucu hatanın olası nedenini de içerir.

### <a name="the-health-of-the-cluster-goes-to-error-while-the-update-installs"></a>Güncelleştirmeyi yüklerken küme durumunu hata durumuna geçer

Hatalı bir güncelleştirme, bir uygulama veya belirli düğüme veya yükseltme etki alanı küme durumunu aşağı kullanıma sunabilirsiniz. Düzeltme eki orchestration uygulama kümeye yeniden sağlıklı duruma gelene kadar sonraki güncelleştirme işlemleri sona erdirir.

Bir yönetici, müdahale ve uygulama ya da küme neden önceden yüklenmiş bir güncelleştirmeyi nedeniyle sağlıksız olduğunu belirler.

## <a name="disclaimer"></a>Bildirim

Düzeltme eki orchestration uygulama kullanımını ve performansını izlemek için telemetri toplar. Uygulamanın telemetri (varsayılan olarak etkindir) Service Fabric çalışma zamanı telemetri ayarını ayarı izler.

## <a name="release-notes"></a>Sürüm Notları

### <a name="version-010"></a>Sürüm 0.1.0
- Özel önizleme sürümü

### <a name="version-200-latest"></a>Sürüm 2.0.0 (en yeni)
- Ortak sürüm
