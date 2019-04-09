---
title: Linux için Azure Service Fabric düzeltme eki düzenleme uygulaması | Microsoft Docs
description: Bir Linux Service Fabric kümesinde işletim sistemi düzeltme eki uygulama otomatik hale getirmek için uygulama.
services: service-fabric
documentationcenter: .net
author: novino
manager: chackdan
editor: ''
ms.assetid: de7dacf5-4038-434a-a265-5d0de80a9b1d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 5/22/2018
ms.author: nachandr
ms.openlocfilehash: 537450dbc386a94fa5c2e0d9334435dce041a32f
ms.sourcegitcommit: b4ad15a9ffcfd07351836ffedf9692a3b5d0ac86
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2019
ms.locfileid: "59057651"
---
# <a name="patch-the-linux-operating-system-in-your-service-fabric-cluster"></a>Service Fabric kümenizi Linux işletim sistemi düzeltme eki

> [!div class="op_single_selector"]
> * [Windows](service-fabric-patch-orchestration-application.md)
> * [Linux](service-fabric-patch-orchestration-application-linux.md)
>
>

Düzeltme eki düzenleme uygulama işletim sistemi düzeltme eki uygulama kapalı kalma süresi olmadan bir Service Fabric kümesinde otomatik hale getiren bir Azure Service Fabric uygulamasıdır.

Orchestration düzeltme eki uygulama, aşağıdaki özellikleri sağlar:

- **Otomatik işletim sistemi güncelleştirme yüklemesi**. İşletim sistemi güncelleştirmeleri otomatik olarak indirilip yüklendiği. Küme düğümleri, küme kapalı kalma süresi olmadan gerektiği gibi yeniden başlatılır.

- **Küme durumunu algılayan düzeltme eki uygulama ve sistem durumu tümleştirme**. Güncelleştirmeleri uygulanırken, düzeltme eki düzenleme uygulama küme düğümlerinin izler. Küme düğümleri yükseltilmiş bir düğüm teker teker teker veya bir yükseltme etki alanı yer. Küme durumunu nedeniyle düzeltme eki uygulama işlemini kalırsa, düzeltme eki uygulama sorunu aggravating önlemek için durduruldu.

## <a name="internal-details-of-the-app"></a>İç uygulama ayrıntıları

Orchestration düzeltme eki uygulama aşağıdaki bileşenleri oluşur:

- **Düzenleyicisi hizmeti**: Bu durum bilgisi olan hizmet için sorumludur:
    - Tüm küme üzerinde işletim sistemi güncelleştirme işi koordine.
    - Tamamlanan işletim sistemi güncelleştirme işlemleri sonucu depolama.
- **Düğüm Aracısı**: Bu durum bilgisi olmayan hizmet tüm Service Fabric küme düğümleri üzerinde çalışır. Hizmet için sorumludur:
    - Linux üzerinde düğüm Aracısı daemon önyükleniyor.
    - Arka plan programı hizmeti izleme.
- **Düğüm Aracısı arka plan programı**: Bu Linux daemon hizmeti, bir üst düzey önceliği (kök) çalıştırır. Buna karşılık, düğüm Aracısı hizmeti ve düzenleyici hizmetini düşük düzeyli ayrıcalık çalıştırın. Hizmet, tüm küme düğümlerinde aşağıdaki güncelleştirme işlerini gerçekleştirmek için sorumludur:
    - Düğüm üzerinde otomatik işletim sistemi güncelleştirme devre dışı bırakılıyor.
    - Kullanıcı, işletim sistemi güncelleştirme ilkesine göre yükleyip sağlamıştır.
    - Makine sonrası işletim sistemi güncelleştirme yüklemesi yeniden başlatma gerekirse.
    - İşletim sistemi güncelleştirmelerini sonuçlarını Düzenleyici hizmete karşıya yükleniyor.
    - Bir işlem tüketme tüm yeniden deneme sonrasında başarısız olduysa raporlama sistem durumu bildirir.

> [!NOTE]
> Düzeltme eki düzenleme uygulama devre dışı bırakın veya düğüm etkinleştirmek ve sistem durumu denetimleri gerçekleştirmek için Service Fabric onarım Yöneticisi sistem hizmeti kullanır. Onarım görevi düzeltme eki düzenleme uygulama tarafından oluşturulan her düğüm için güncelleştirme ilerleme durumunu izler.

## <a name="prerequisites"></a>Önkoşullar

### <a name="ensure-that-your-azure-vms-are-running-ubuntu-1604"></a>Ubuntu 16.04 Azure Vm'lerinizi çalıştırdığından emin olun
Bu belge, Ubuntu 16.04 yazma zamanında (`Xenial Xerus`) yalnızca desteklenen sürümüdür.

### <a name="ensure-that-the-service-fabric-linux-cluster-is-version-62x-and-above"></a>Service fabric linux kümesi sürüm olduğundan emin olun 6.2.x ve üzeri

Düzeltme eki düzenleme uygulama linux kullanan yalnızca service fabric çalışma zamanı sürümünde kullanılabilen bazı çalışma zamanı özellikleri 6.2.x ve üstü.

### <a name="enable-the-repair-manager-service-if-its-not-running-already"></a>(Bunu zaten çalışmıyorsa) onarım Yöneticisi hizmetini etkinleştirme

Düzeltme eki düzenleme uygulama küme üzerinde etkinleştirilmesini onarım Yöneticisi sistem hizmeti gerekiyor.

#### <a name="azure-clusters"></a>Azure kümeleri

Azure linux kümeleri silver ve gold dayanıklılık katmanı varsayılan olarak etkin onarım Yöneticisi hizmeti sahiptir. Onarım Yöneticisi hizmetinin etkinleştirilmiş Azure kümelerde varsayılan olarak, Bronz dayanıklılık katmanı yok. Hizmet zaten etkin değilse, Service Fabric Explorer'da Sistem Hizmetleri bölümündeki çalışmasını görebilirsiniz.

##### <a name="azure-portal"></a>Azure portal
Onarım Yöneticisi Azure Portalı'ndan kümesini ayarlama sırasında etkinleştirebilirsiniz. Seçin **onarım Yöneticisi dahil** altındaki **eklenti özellikleri** küme yapılandırmasının zaman.
![Azure portalından etkinleştirme onarım Yöneticisi'nin resmi](media/service-fabric-patch-orchestration-application/EnableRepairManager.png)

##### <a name="azure-resource-manager-deployment-model"></a>Azure Resource Manager dağıtım modeli
Alternatif olarak [Azure Resource Manager dağıtım modeli](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) yeni ve mevcut Service Fabric kümeleri üzerinde onarım Yöneticisi hizmetini etkinleştirmek için. Şablonu dağıtmak istediğiniz küme için alın. Örnek şablonları kullanabilir veya özel bir Azure Resource Manager dağıtım modeli şablon oluşturabilirsiniz. 

Onarım Yöneticisi hizmetini kullanarak etkinleştirmek için [Azure Resource Manager dağıtım modeli şablonu](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm):

1. İlk maddeyi `apiversion` ayarlanır `2017-07-01-preview` için `Microsoft.ServiceFabric/clusters` kaynak. Farklı sonra güncelleştirmeye gerek duyduğunuz `apiVersion` değerine `2017-07-01-preview` veya üzeri:

    ```json
    {
        "apiVersion": "2017-07-01-preview",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
    }
    ```

2. Şimdi aşağıdakileri ekleyerek onarım Yöneticisi hizmetini etkinleştirmek `addonFeatures` sonra bölüm `fabricSettings` bölümü:

    ```json
    "fabricSettings": [
        ...      
    ],
    "addonFeatures": [
        "RepairManager"
    ],
    ```

3. Bu değişikliklerle küme şablonunuza güncelleştirildikten sonra bunları uygulamak ve son yükseltme izin verin. Artık, kümenizde çalışan onarım Yöneticisi sistem hizmeti de görebilirsiniz. Çağrıldığı `fabric:/System/RepairManagerService` Sistem Hizmetleri bölümündeki Service Fabric Explorer. 

### <a name="standalone-on-premises-clusters"></a>Tek başına şirket içi kümeleri

Bu belge yazma sırasında tek başına Service Fabric Linux kümelerinde desteklenmez.

### <a name="disable-automatic-os-update-on-all-nodes"></a>Tüm düğümlerde otomatik işletim sistemi güncelleştirme devre dışı bırak

Otomatik işletim sistemi güncelleştirmeleri kullanılabilirlik kaybına neden ve veya çalışan uygulamaların davranışını değiştirebilirsiniz. Düzeltme eki düzenleme uygulama varsayılan olarak, bu senaryolara önlemek için her küme düğümünde otomatik işletim sistemi güncelleştirme devre dışı bırakmak çalışır.
Ubuntu için [yükseltmeleri katılımsız](https://help.ubuntu.com/community/AutomaticSecurityUpdates) düzeltme eki düzenleme uygulama tarafından devre dışı bırakılır.

## <a name="download-the-app-package"></a>Uygulama paketini indirme

Uygulamayı yükleme betikleri ile birlikte gelen indirilebilir [arşiv bağlantı](https://go.microsoft.com/fwlink/?linkid=867984).

Uygulama sfpkg biçimde nden indirilebilir [sfpkg bağlantı](https://aka.ms/POA/POA_v2.0.3.sfpkg). Bu için kullanışlı gelir [Azure Resource Manager tabanlı uygulama dağıtımı](service-fabric-application-arm-resource.md).

## <a name="configure-the-app"></a>Uygulamayı yapılandırma

Düzeltme eki düzenleme uygulamanın davranış şekli, gereksinimlerinizi karşılayacak şekilde yapılandırılabilir. Uygulama oluşturma veya güncelleştirme işlemi sırasında uygulama parametresi olarak geçirerek varsayılan değerleri geçersiz. Uygulama parametreleri belirterek sağlanabilir `ApplicationParameter` için `Start-ServiceFabricApplicationUpgrade` veya `New-ServiceFabricApplication` cmdlet'leri.

|**Parametre**        |**Type**                          | **Ayrıntılar**|
|:-|-|-|
|MaxResultsToCache    |Uzun                              | Güncelleştirme sonuçları önbelleğe alınması gereken maksimum sayısı. <br>Varsayılan değer: 3000 varsayılarak: <br> -Düğüm sayısı 20'dir. <br> -Bir düğüm / ay üzerinde gerçekleştirilecek güncelleştirme sayısı beştir. <br> -İşlem başına sonuç sayısı 10 olabilir. <br> -Son üç ay için sonuçları depolanması gerekir. |
|TaskApprovalPolicy   |Sabit listesi <br> {NodeWise, UpgradeDomainWise}                          |Service Fabric küme düğümleri arasında güncelleştirmeleri yüklemek için Düzenleyici hizmeti tarafından kullanılacak olan ilke TaskApprovalPolicy gösterir.<br>                         İzin verilen değerler şunlardır: <br>                                                           <b>NodeWise</b>. Yüklü bir düğümü aynı anda güncelleştirmelerdir. <br>                                                           <b>UpgradeDomainWise</b>. Yüklü bir yükseltme etki alanı aynı anda güncelleştirmelerdir. (En güncelleştirmesi bir yükseltme etki alanına ait olan tüm düğümleri gidebilirsiniz.)
| UpdateOperationTimeOutInMinutes | Int <br>(Varsayılan: 180)                   | (İndirme veya yükleme) herhangi bir güncelleştirme işlemi için zaman aşımını belirtir. İşlemi belirtilen süre içinde tamamlanmazsa, iptal edildi.       |
| RescheduleCount      | Int <br> (Varsayılan: 5)                  | Bir işlem kalıcı olarak başarısız olması durumunda en fazla kaç kez işletim sistemi hizmet tarih değiştirdiğinde güncelleştirin.          |
| RescheduleTimeInMinutes  | Int <br>(Varsayılan: 30) | Hata devam ederse durumunda güncelleştirme, hizmet işletim sistemi tarih değiştirdiğinde aralığı. |
| UpdateFrequency           | Virgülle ayrılmış bir dize (varsayılan: "Haftalık, Çarşamba, 7:00:00")     | Küme üzerinde işletim sistemi güncelleştirmeleri yükleme sıklığı. Biçim ve olası değerler şunlardır: <br>-Örneğin, aylık, 5, 12:22:32 aylık, DD ss. <br> -Örneğin, haftalık, Salı, 12:22:32 için haftalık, gün, ss.  <br> -Örneğin, günlük, 12:22:32 günlük, ss.  <br> -Yok, bu güncelleştirmenin yapılması olmamalıdır belirtir.  <br><br> Tüm saatler UTC biçimindedir.|
| UpdateClassification | Virgülle ayrılmış bir dize (varsayılan: "securityupdates") | Küme düğümleri üzerinde yüklenmesi gereken güncelleştirmelerin türü. Kabul edilebilir değerler securityupdates, tüm. <br> -securityupdates - yalnızca güvenlik güncelleştirmeleri yüklenir <br> -all - Tüm kullanılabilir güncelleştirmeleri apt'ndan yüklenir.|
| ApprovedPatches | Virgülle ayrılmış bir dize (varsayılan: "") | Küme düğümlerine yüklenmesi gereken onaylı güncelleştirmeler listesidir. Virgülle ayrılmış liste onaylanmış paketler ve isteğe bağlı olarak istenen hedef sürümünü içerir.<br> Örneğin: "apt-utils 1.2.10ubuntu1, jwt python3, aktarım https apt < 1.2.194, libsystemd0 = > 229 4ubuntu16 =" <br> Yukarıdaki yüklenir <br> -apt-utils apt-önbellekte varsa sürüm 1.2.10ubuntu1 ile. Ardından, belirli bir sürümü kullanılabilir durumda değilse, bir İşlemsiz hale gelir. <br> -kullanılabilir en son sürüme yükseltme python3 jwt. Ardından bir paket yoksa, bir İşlemsiz olduğu. <br> -Aktarım https apt yükseltmeleri 1.2.194'den küçük olan en yüksek sürüm. Bu sürüm yoksa, onu bir İşlemsiz olur. <br> -libsystemd0 yükseltmeleri 229 4ubuntu16 değerine eşit veya daha büyük olan en yüksek sürüm. Ardından bu tür bir sürümü mevcut değilse bir İşlemsiz hale gelir.|
| RejectedPatches | Virgülle ayrılmış bir dize (varsayılan: "") | Bu, küme düğümlerine yüklenmemelidir güncelleştirmelerin listesi <br> Örneğin: "bash, sudo" <br> Önceki bash, güncelleştirmeleri almasını sudo filtreler. |


> [!TIP]
> İşletim sistemi hemen gerçekleşmeye güncelleştirme istiyorsanız ayarlayın `UpdateFrequency` göre uygulama dağıtım süresi. Örneğin, beş düğümlü test kümesi vardır ve uygulaması yaklaşık 5: 00'da dağıtmayı planladığınız varsayalım UTC. Uygulama yükseltme veya dağıtım sırasında en fazla 30 dakika sürer olduğunu varsayarsak, UpdateFrequency "Günlük, 17:30:00" ayarlayın.

## <a name="deploy-the-app"></a>Uygulamayı dağıtma

1. Küme, tüm önkoşul adımları tamamlama göre hazırlayın.
2. Herhangi bir Service Fabric uygulaması gibi düzeltme eki düzenleme uygulaması dağıtın. PowerShell veya Azure Service Fabric CLI'yı kullanarak uygulamayı dağıtabilirsiniz. Bağlantısındaki [PowerShell kullanarak dağıtma ve Kaldır uygulamaları](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications) veya [Azure Service Fabric CLI kullanarak uygulamayı dağıtma](https://docs.microsoft.com/azure/service-fabric/scripts/cli-deploy-application)
3. Dağıtım sırasında uygulama yapılandırmak için geçirmek `ApplicationParameter` için `New-ServiceFabricApplication` cmdlet veya betik sağlanan. Kolaylık olması için (Deploy.ps1) powershell ve bash (Deploy.sh) betikleri uygulama ile birlikte sağlanır. Betiği kullanmak için:

    - Bir Service Fabric kümesine bağlanın.
    - Dağıtım betiği yürütün. İsteğe bağlı olarak uygulama parametresi betiğine geçirin. Örneğin:.\Deploy.ps1 - ApplicationParameter @{UpdateFrequency "Günlük, 11:00:00" =} OR./Deploy.sh "{\"UpdateFrequency\":\"günlük, 11:00:00\"}" 

> [!NOTE]
> Betik ve uygulama klasörü PatchOrchestrationApplication aynı dizinde tutun.

## <a name="upgrade-the-app"></a>Uygulama yükseltme

Var olan bir düzeltme eki düzenleme uygulamaya yükseltmek için adımları izleyin. [PowerShell kullanarak Service Fabric uygulaması yükseltme](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-upgrade-tutorial-powershell) veya [Azure Service Fabric CLI kullanarak Service Fabric uygulaması yükseltme](https://docs.microsoft.com/azure/service-fabric/service-fabric-sfctl-application#sfctl-application-upgrade)

## <a name="remove-the-app"></a>Uygulamayı Kaldır

Uygulamayı kaldırmak için adımları izleyin. [PowerShell kullanarak dağıtma ve Kaldır uygulamaları](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications) veya [Azure Service Fabric CLI kullanarak bir uygulamayı kaldırma](https://docs.microsoft.com/azure/service-fabric/service-fabric-sfctl-application#sfctl-application-delete)

Kolaylık olması için (Undeploy.ps1) powershell ve bash (Undeploy.sh) betikleri uygulama ile birlikte sağlanır. Betiği kullanmak için:

  - Bir Service Fabric kümesine bağlanın.
  - Betik Undeploy.ps1 veya Undeploy.sh yürütün

> [!NOTE]
> Betik ve uygulama klasörü PatchOrchestrationApplication aynı dizinde tutun.

## <a name="view-the-update-results"></a>Güncelleştirme sonuçları görüntüleyin

Düzeltme eki düzenleme uygulama kullanıcı için geçmiş sonuçlarını görüntülemek için REST API'lerini kullanıma sunar. Aşağıdaki örnek oluşur:
```testadm@bronze000001:~$ curl -X GET http://10.0.0.5:20002/PatchOrchestrationApplication/v1/GetResults```
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

JSON alanlarının aşağıda açıklanmıştır:

Alan | Değerler | Ayrıntılar
-- | -- | --
OperationResult | 0 - başarılı<br> 1 - hatalarıyla başarılı oldu<br> 2 - başarısız oldu<br> 3 - durduruldu<br> 4 - zaman aşımı ile iptal edildi | Genel işlemin (genellikle bir veya daha fazla güncelleştirmelerin yüklenmesini içeren) sonucunu gösterir.
ResultCode | OperationResult aynı | Bu alan tek güncelleştirme için yükleme işleminin sonucu gösterir.
OperationType | 1 - yükleme<br> 0 - arayın ve yükleyin.| Yükleme Sonuçları varsayılan olarak gösteriliyordu yalnızca OperationType olur.
UpdateClassification | Varsayılan değer "securityupdates" | Güncelleştirme işlemi sırasında yüklenen güncelleştirmeleri türü
UpdateFrequency | Varsayılan değer "Haftalık, Çarşamba, 7:00:00" | Bu güncelleştirme için yapılandırılan sıklık güncelleştirin.
RebootRequired | TRUE - yeniden başlatma gerekli<br> false - yeniden başlatma gerekli değil | Yeniden başlatma güncelleştirmeleri yüklemesini tamamlamak için gerekli olmadığını belirtir.
ApprovedList | Varsayılan değer "" | Bu güncelleştirme için onaylanan düzeltme eklerinin listesini
RejectedList | Varsayılan değer "" | Bu güncelleştirme için reddedilen düzeltme eklerinin listesini

Hiçbir güncelleştirme henüz zamanlanmış, JSON sonuç boş olur.

Sorgu güncelleştirme sonuçları kümeye oturum açın. Ardından Coordinator hizmetinin birincil çoğaltması adresini öğrenmek ve tarayıcı URL'den isabet: http://&lt;çoğaltma IP&gt;:&lt;ApplicationPort&gt;/PatchOrchestrationApplication/v1/GetResults .

REST uç noktasını Koordinatör hizmeti için dinamik bir bağlantı noktası var. Tam URL'yi kontrol etmek için Service Fabric Explorer'a bakın. Örneğin, sonuçları kullanılabilir `http://10.0.0.7:20000/PatchOrchestrationApplication/v1/GetResults`.

![REST uç noktası görüntüsü](media/service-fabric-patch-orchestration-application/Rest_Endpoint.png)

## <a name="diagnosticshealth-events"></a>Tanılama/sistem durumu olayları

### <a name="diagnostic-logs"></a>Tanılama günlükleri

Düzeltme eki düzenleme uygulama günlükleri, Service Fabric çalışma zamanı günlüklerini bir parçası olarak toplanır.

Tanılama Aracı/ardışık seçtiğiniz aracılığıyla günlükleri tutmak istemeniz durumunda. Düzeltme eki düzenleme uygulaması aracılığıyla olaylarını günlüğe kaydedecek şekilde sabit sağlayıcısını kimlikleri kullanır [eventsource](https://docs.microsoft.com/dotnet/api/system.diagnostics.tracing.eventsource?view=netstandard-2.0)

- e39b723c-590c-4090-abb0-11e3e6616346
- fc0028ff-bfdc-499f-80dc-ed922c52c5e9
- 24afa313-0d3b-4c7c-b485-1047fd964b60
- 05dc046c-60e9-4ef7-965e-91660adffa68

### <a name="health-reports"></a>Sistem durumu raporlarının sayısı

Orchestration düzeltme eki uygulama, aşağıdaki durumlarda karşı Coordinator hizmetini veya düğüm Aracısı sistem durumu raporlarının de yayımlar:

#### <a name="an-update-operation-failed"></a>Bir güncelleştirme işlemi başarısız oldu

Bir düğümde bir güncelleştirme işlemi başarısız olursa, sistem durumu raporu karşı düğüm Aracısı hizmeti oluşturulur. Sistem Durumu raporu ayrıntılarını sorunlu düğüm adını içerir.

Düzeltme eki uygulama sorunlu düğümde başarıyla tamamlandıktan sonra raporu otomatik olarak temizlenir.

#### <a name="the-node-agent-daemon-service-is-down"></a>Düğüm Aracısı arka plan programı hizmeti çalışmıyor

Düğüm Aracısı arka plan programı hizmeti bir düğüm üzerinde çalışmıyorsa, bir uyarı düzeyi sistem durumu raporu karşı düğüm Aracısı hizmeti oluşturulur.

#### <a name="the-repair-manager-service-is-not-enabled"></a>Onarım Yöneticisi hizmeti etkin değil

Onarım Yöneticisi hizmeti kümede bulunamazsa bir uyarı düzeyi sistem durumu rapor Düzenleyicisi hizmeti için oluşturulur.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

S. **Düzeltme eki düzenleme uygulama çalışırken hata durumunda kümem neden görüyorum?**

A. Yükleme işlemi sırasında orchestration düzeltme eki uygulamayı devre dışı bırakır veya düğümleri yeniden başlatılır. Bu işlem geçici olarak giderek kümesinin sistem sonuçlanabilir.

İlke uygulama için bağlı olarak, ya da bir düğüm bir düzeltme eki uygulama işlemi sırasında gidebilirsiniz *veya* tüm yükseltme etki alanı aynı anda Git aşağı.

Yüklemenin sonunda, düğümler yeniden iler hale getirilir yeniden başlatma gönderin.

Geçici bir hata durumu aşağıdaki örnekte, küme oluştu çünkü iki düğümü olan aşağı ve MaxPercentageUnhealthyNodes ilke ihlal. Düzeltme eki uygulama işlemi devam ediyor kadar geçici bir hatadır.

![Sağlıksız kümesinin görüntüsü](media/service-fabric-patch-orchestration-application/MaxPercentage_causing_unhealthy_cluster.png)

Sorun devam ederse, sorun giderme bölümüne bakın.

S. **Uyarı durumunda düzeltme eki düzenleme uygulama**

A. Uygulamaya karşı gönderilen bir sistem durumu raporu, kök neden olup olmadığını denetleyin. Genellikle, uyarı sorunun ayrıntılarını içerir. Geçici bir sorundur, uygulamayı bu durumdan otomatik olarak kurtarmak için bekleniyor.

S. **Kümem sağlıksız olduğunu ve Acil işletim sistemi güncelleştirme yapmanız durumunda neler yapabilirim?**

A. Küme sağlıksız durumdayken düzeltme eki düzenleme uygulama güncelleştirmelerini yüklemez. Düzeltme eki düzenleme uygulama iş akışı engelini kaldırmak için kümenizin sağlıklı bir duruma getirin.

S. **Neden kümelerinde düzeltme eki uygulama kadar çalıştırmak için sürüyor?**

A. Düzeltme eki düzenleme uygulama tarafından gereken süre, genellikle aşağıdaki etkenlere bağlıdır:

- İlke Düzenleyicisi hizmeti. 
  - Varsayılan ilkeyi `NodeWise`, aynı anda yalnızca tek bir düğüme düzeltme eki uygulama neden olur. Özellikle varsa daha büyük bir küme kullanmanızı öneririz `UpgradeDomainWise` küme genelinde daha hızlı düzeltme eki uygulama elde etmek için ilke.
- Güncelleştirme indirme ve yükleme için kullanılabilir sayısı. 
- İndirmek ve bir güncelleştirmeyi yüklemek için gereken ortalama süre, birkaç saat aşmamalıdır.
- VM ve ağ bant genişliği performansını.

S. **Güvenlik güncelleştirmeleri olan mu düzeltme eki düzenleme uygulama hangi güncelleştirmelerin nasıl karar verir.**

A. Düzeltme eki düzenleme uygulama distro özgü mantığı, hangi güncelleştirmelerin güvenlik güncelleştirmeleri kullanılabilir güncelleştirmeler arasında belirlemek için kullanır. Örneğin: Ubuntu, uygulama arşivleri $RELEASE güncelleştirmeleri arar-güvenlik, $RELEASE-güncelleştirmeleri ($RELEASE xenial = ya da linux standart temel yayın sürümü). 

 
S. **Belirli bir paket sürümünü açın nasıl kilitlemek üzere?**

A. Belirli bir sürüme paketlerinizi kilitlemek için ApprovedPatches ayarları kullanın. 


S. **Ubuntu otomatik güncelleştirmeleri ne olur?**

A. Kümenizde düzeltme eki düzenleme uygulama yüklemeden hemen sonra yükseltme, küme düğümünde katılımsız devre dışı. Tüm düzenli güncelleştirme iş akışını düzeltme eki düzenleme uygulama tarafından yönlendirilen.
Küme genelinde ortam tutarlılığını sağlamak için düzeltme düzenlemesi yalnızca uygulama aracılığıyla güncelleştirmeleri yüklemeniz önerilir. 
 
S. **Yükseltme, orchestration uygulama kullanılmayan paketlerin temizleme sonrası düzeltme?**

A. Evet, yükleme sonrası adımları bir parçası olarak temizleme gerçekleşir. 

S. **Düzeltme ekini düzenlemeyi uygulama geliştirme kümem (tek düğümlü kümenize) düzeltme eki için kullanılabilir mi?**

A. Hayır, düzeltme eki düzenleme uygulama düzeltme eki tek düğümlü küme için kullanılamaz. Bu tasarım gereği, olarak sınırlamasıdır [service fabric sistem hizmetlerinin](https://docs.microsoft.com/azure/service-fabric/service-fabric-technical-overview#system-services) veya herhangi bir müşteri uygulama kapalı kalma süresi karşılaşır ve bu nedenle düzeltme eki uygulama için herhangi bir onarım işi hiçbir zaman onarım Yöneticisi tarafından onaylanan.

## <a name="troubleshooting"></a>Sorun giderme

### <a name="a-node-is-not-coming-back-to-up-state"></a>Bir düğüm geri durumu yukarı geliyor değil

**Düğümü devre dışı bırakma bir durumda olduğundan takılmış olabilir**:

Bir güvenlik onay bekliyor. Bu durumu ortadan kaldırmak için sağlam durumda yeterli düğümleri kullanılabilir olduğundan emin olun.

**Düğümü devre dışı durumda olduğundan takılmış olabilir**:

- Düğüm el ile devre dışı bırakıldı.
- Düğüm bir devam eden Azure altyapı işi nedeniyle devre dışı bırakıldı.
- Düğüm düğüm düzeltme eki uygulama düzeltme eki düzenleme uygulama tarafından geçici olarak devre dışı bırakıldı.

**Düğümü aşağı bir durumda olduğundan takılmış olabilir**:

- Düğümü aşağı bir durumda el ile durumuna alındı.
- Düğümü yeniden başlatma (Bu düzeltme eki düzenleme uygulama tarafından tetiklenebilir) bir kayma yaşanıyor.
- Düğümü hatalı bir VM veya makine veya ağ bağlantı sorunları nedeniyle çalışmıyor.

### <a name="updates-were-skipped-on-some-nodes"></a>Bazı düğümlerinde güncelleştirmeleri atlandı

Düzeltme eki düzenleme uygulaması, yeniden zamanlama ilkesine göre bir güncelleştirme yüklemeyi dener. Hizmet, düğüm kurtarmak ve uygulama ilkesine göre güncelleştirmeyi atlama dener.

Böyle bir durumda bir uyarı düzeyi sistem durumu raporu karşı düğüm Aracısı hizmeti oluşturulur. Güncelleştirme için sonuç, hatanın olası nedeni de içerir.

### <a name="the-health-of-the-cluster-goes-to-error-while-the-update-installs"></a>Güncelleştirmeyi yüklerken küme durumunu hata durumuna geçer

Hatalı bir güncelleştirme, bir uygulama veya belirli bir düğüm veya yükseltme etki alanı küme durumunu aşağı getirebilirsiniz. Düzeltme eki düzenleme uygulamayı yeniden kümenin sağlıklı olup kadar herhangi bir sonraki güncelleştirme işlemi sona erdirir.

Bir yönetici, müdahale ve uygulama veya küme neden daha önce yüklenen bir güncelleştirme nedeniyle sağlıksız olduğunu belirleyin.

## <a name="disclaimer"></a>Bildirim

Düzeltme eki düzenleme uygulama kullanımını ve performansını izlemek için telemetri toplar. Uygulamanın telemetri (Bu varsayılan olarak etkindir) Service Fabric çalışma zamanı telemetri ayarı ayarını izler.

## <a name="release-notes"></a>Sürüm Notları

### <a name="version-010"></a>Sürüm 0.1.0
- Özel önizleme sürümü

### <a name="version-200"></a>Sürüm 2.0.0
- Genel sürüm

### <a name="version-201"></a>Sürüm 2.0.1
- Son Service Fabric SDK'sını kullanarak uygulamayı yeniden derlenen

### <a name="version-202"></a>Sürüm 2.0.2 
- Sistem durumu uyarı yeniden başlatma sırasında geride bir sorun düzeltildi.

### <a name="version-203-latest"></a>Sürüm 2.0.3 sürümünü (son sürüm)
- Düğüm Aracısı arka plan programı hizmeti için CPU kullanımını en fazla %99 işler için standart_d1_v2 VM üzerinde nereye ulaşıldı sorunu düzeltme.
- Geçerli düğüm adı alt kümesi olan bir ada sahip düğüm olasılığına, düzeltme eki uygulama yaşam döngüsü bir düğümde parametreden sorunu düzeltme. Bu düğümleri, kendi olası düzeltme eki uygulama eksik veya yeniden başlatma beklemede.
- Bozuk ayarları hizmete geçirildiğinde, nedeniyle düğüm Aracısı arka plan programı kilitlenen tutan bir hata düzeltildi.