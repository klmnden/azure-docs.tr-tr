---
title: Azure Service Fabric düzeltme eki düzenleme uygulaması | Microsoft Docs
description: Bir Service Fabric kümesinde işletim sistemi düzeltme eki uygulama otomatik hale getirmek için uygulama.
services: service-fabric
documentationcenter: .net
author: khandelwalbrijeshiitr
manager: chackdan
editor: ''
ms.assetid: de7dacf5-4038-434a-a265-5d0de80a9b1d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/01/2019
ms.author: brkhande
ms.openlocfilehash: ccc0399b6ac886ec8d9ef7d207c3539f1d078070
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65951937"
---
# <a name="patch-the-windows-operating-system-in-your-service-fabric-cluster"></a>Service Fabric kümenizi Windows işletim sistemi düzeltme eki

> 
> [!IMPORTANT]
> Uygulama sürümü 1.2. * desteği 30 Nisan 2019 tarihinde geçiyor. Lütfen en son sürüme yükseltin.


[Azure sanal makine ölçek kümesinin otomatik işletim sistemi görüntüsü yükseltmeleri](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-automatic-upgrade) tutma, işletim sistemleri, Azure'da düzeltme eki için en iyi yöntem ve düzeltme eki düzenleme uygulaması (POA) hizmeti dokularını RepairManager sistemler hizmeti çevresinde bir sarmalayıcı Bu, yapılandırma temel işletim sistemi düzeltme eki Azure dışı barındırılan kümeler için zamanlama sağlar. POA Azure dışı barındırılan kümeleri için gerekli değildir, ancak yükseltme etki alanları, düzeltme eki yüklemesiyle zamanlama Service Fabric kümeleri konakları kapalı kalma süresi olmadan düzeltme için gereklidir.

POA işletim sistemi düzeltme eki uygulama kapalı kalma süresi olmadan bir Service Fabric kümesinde otomatik hale getiren bir Azure Service Fabric uygulamasıdır.

Orchestration düzeltme eki uygulama, aşağıdaki özellikleri sağlar:

- **Otomatik işletim sistemi güncelleştirme yüklemesi**. İşletim sistemi güncelleştirmeleri otomatik olarak indirilip yüklendiği. Küme düğümleri, küme kapalı kalma süresi olmadan gerektiği gibi yeniden başlatılır.

- **Küme durumunu algılayan düzeltme eki uygulama ve sistem durumu tümleştirme**. Güncelleştirmeleri uygulanırken, düzeltme eki düzenleme uygulama küme düğümlerinin izler. Küme düğümleri yükseltilmiş bir düğüm teker teker teker veya bir yükseltme etki alanı yer. Küme durumunu nedeniyle düzeltme eki uygulama işlemini kalırsa, düzeltme eki uygulama sorunu aggravating önlemek için durduruldu.

## <a name="internal-details-of-the-app"></a>İç uygulama ayrıntıları

Orchestration düzeltme eki uygulama aşağıdaki bileşenleri oluşur:

- **Düzenleyicisi hizmeti**: Bu durum bilgisi olan hizmet için sorumludur:
    - Tüm küme üzerinde Windows güncelleştirme işi koordine.
    - Tamamlanan Windows güncelleştirme işlemleri sonucu depolama.
- **Düğüm Aracısı**: Bu durum bilgisi olmayan hizmet tüm Service Fabric küme düğümleri üzerinde çalışır. Hizmet için sorumludur:
    - Düğüm Aracısı NTService önyükleniyor.
    - Düğüm Aracısı NTService izleme.
- **Düğüm Aracısı NTService**: Bu Windows NT hizmeti, bir üst düzey önceliği (Sistem) çalışır. Buna karşılık, düğüm Aracısı hizmeti ve düzenleyici hizmetini bir alt düzey önceliği (ağ hizmeti) çalıştırın. Hizmet, tüm küme düğümlerinde aşağıdaki Windows güncelleştirme işlerini gerçekleştirmek için sorumludur:
    - Düğüm Windows Update'te otomatik devre dışı bırakılıyor.
    - Windows güncelleştirme ilkesine göre yükleyip kullanıcı sağlamıştır.
    - Makine post Windows Güncelleştirme yüklemesini yeniden başlatılıyor.
    - Windows güncelleştirmeleri sonuçlarını Düzenleyici hizmete karşıya yükleniyor.
    - Bir işlem tüketme tüm yeniden deneme sonrasında başarısız olduysa raporlama sistem durumu bildirir.

> [!NOTE]
> Düzeltme eki düzenleme uygulama devre dışı bırakın veya düğüm etkinleştirmek ve sistem durumu denetimleri gerçekleştirmek için Service Fabric onarım Yöneticisi sistem hizmeti kullanır. Onarım görevi düzeltme eki düzenleme uygulama tarafından oluşturulan her düğüm için Windows güncelleştirme ilerleme durumunu izler.

## <a name="prerequisites"></a>Önkoşullar

> [!NOTE]
> Gerekli en düşük .NET framework sürümünü 4.6 ' dir.

### <a name="enable-the-repair-manager-service-if-its-not-running-already"></a>(Bunu zaten çalışmıyorsa) onarım Yöneticisi hizmetini etkinleştirme

Düzeltme eki düzenleme uygulama küme üzerinde etkinleştirilmesini onarım Yöneticisi sistem hizmeti gerekiyor.

#### <a name="azure-clusters"></a>Azure kümeleri

Azure silver dayanıklılık katmanı kümelerde varsayılan olarak etkin onarım Yöneticisi hizmetiniz varsa. Azure gold dayanıklılık katmanı kümelerde olabilir veya onarım Yöneticisi hizmeti bu kümeler yaptığınızda oluşturulan bağlı olarak, etkin olmayabilir. Onarım Yöneticisi hizmetinin etkinleştirilmiş Azure kümelerde varsayılan olarak, Bronz dayanıklılık katmanı yok. Hizmet zaten etkin değilse, Service Fabric Explorer'da Sistem Hizmetleri bölümündeki çalışmasını görebilirsiniz.

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

Kullanabileceğiniz [tek başına Windows Küme için yapılandırma ayarlarını](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-manifest) yeni ve mevcut Service Fabric kümesinde onarım Yöneticisi hizmetini etkinleştirmek için.

Onarım Yöneticisi hizmetini etkinleştirmek için:

1. İlk maddeyi `apiversion` içinde [genel küme yapılandırmalarını](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-manifest#general-cluster-configurations) ayarlanır `04-2017` veya üzeri:

    ```json
    {
        "name": "SampleCluster",
        "clusterConfigurationVersion": "1.0.0",
        "apiVersion": "04-2017",
        ...
    }
    ```

2. Şimdi aşağıdakileri ekleyerek onarım Yöneticisi hizmeti etkinleştirmek `addonFeatures` sonra bölüm `fabricSettings` bölümünde aşağıda gösterildiği gibi:

    ```json
    "fabricSettings": [
        ...      
    ],
    "addonFeatures": [
        "RepairManager"
    ],
    ```

3. Küme bildiriminizi güncelleştirilmiş küme bildiriminde kullanarak bu değişikliklerle güncelleştirmek [yeni küme oluşturma](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-for-windows-server) veya [küme yapılandırmasını yükseltme](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-upgrade-windows-server). Güncelleştirilmiş küme bildirimi ile küme çalışmaya başladıktan sonra çağrılır, kümede çalışan onarım Yöneticisi sistem hizmeti artık görebilirsiniz `fabric:/System/RepairManagerService`altında Service Fabric explorer bölümünde sistem hizmetleri.

### <a name="configure-windows-updates-for-all-nodes"></a>Tüm düğümler için Windows güncelleştirmelerini yapılandırma

Aynı anda birden çok küme düğümüne yeniden başlatabilirsiniz çünkü Windows Otomatik Güncelleştirmeler kullanılabilirlik kaybına neden olabilir. Düzeltme eki düzenleme uygulama varsayılan olarak, her küme düğümünde otomatik Windows güncelleştirme devre dışı bırakmak çalışır. Ancak, ayarları Grup İlkesi veya bir yönetici tarafından yönetiliyorsa, "Bildirim önce indirin" için Windows Update ilke açık olarak ayarlama öneririz.

## <a name="download-the-app-package"></a>Uygulama paketini indirme

Uygulama paketini indirmek için Github'da yayın ziyaret [sayfa](https://github.com/microsoft/Service-Fabric-POA/releases/latest/) düzeltme eki düzenleme uygulaması.

## <a name="configure-the-app"></a>Uygulamayı yapılandırma

Düzeltme eki düzenleme uygulamanın davranış şekli, gereksinimlerinizi karşılayacak şekilde yapılandırılabilir. Uygulama oluşturma veya güncelleştirme işlemi sırasında uygulama parametresi olarak geçirerek varsayılan değerleri geçersiz. Uygulama parametreleri belirterek sağlanabilir `ApplicationParameter` için `Start-ServiceFabricApplicationUpgrade` veya `New-ServiceFabricApplication` cmdlet'leri.

|**Parametre**        |**Tür**                          | **Ayrıntılar**|
|:-|-|-|
|MaxResultsToCache    |Uzun                              | Önbelleğe alınan Windows Update sonuçlarının maksimum sayısı. <br>Varsayılan değer: 3000 varsayılarak: <br> -Düğüm sayısı 20'dir. <br> -Bir düğüm / ay üzerinde gerçekleştirilecek güncelleştirme sayısı beştir. <br> -İşlem başına sonuç sayısı 10 olabilir. <br> -Son üç ay için sonuçları depolanması gerekir. |
|TaskApprovalPolicy   |Enum <br> {NodeWise, UpgradeDomainWise}                          |Service Fabric küme düğümleri arasında Windows güncelleştirmeleri yüklemek için Düzenleyici hizmeti tarafından kullanılacak olan ilke TaskApprovalPolicy gösterir.<br>                         İzin verilen değerler şunlardır: <br>                                                           <b>NodeWise</b>. Windows güncelleştirme yüklü tek bir düğüm bir kerede olur. <br>                                                           <b>UpgradeDomainWise</b>. Windows Update, aynı anda yüklü bir yükseltme etki alanıdır. (En bir yükseltme etki alanına ait olan tüm düğümleri için Windows Update gidebilirsiniz.)<br> Başvurmak [SSS](#frequently-asked-questions) , uygun ilke kümeniz için en iyi olduğuna karar vermeye yönelik bölümü.
|LogsDiskQuotaInMB   |Uzun  <br> (Varsayılan: 1024)               |Yerel olarak düğümlerinde kalıcı MB, düzeltme eki düzenleme uygulama en büyük boyutunu kaydeder.
| WUQuery               | string<br>(Varsayılan: "IsInstalled = 0")                | Windows güncelleştirmeleri almak için sorgulayın. Daha fazla bilgi için [WuQuery.](https://msdn.microsoft.com/library/windows/desktop/aa386526(v=vs.85).aspx)
| InstallWindowsOSOnlyUpdates | Boolean <br> (varsayılan: false)                 | Hangi güncelleştirmelerin indirilmesi ve yüklenmesi denetlemek için bu bayrağı kullanın. Aşağıdaki değerlerine izin verilir. <br>TRUE - yalnızca Windows işletim sistemi güncelleştirmeleri yükler.<br>false - makinede sağlanan tüm güncelleştirmeleri yükler.          |
| WUOperationTimeOutInMinutes | Int <br>(Varsayılan: 90)                   | (Arama ya da indirme veya yükleme) herhangi bir Windows güncelleştirme işlemi için zaman aşımını belirtir. İşlemi belirtilen süre içinde tamamlanmazsa, iptal edildi.       |
| WURescheduleCount     | Int <br> (Varsayılan: 5)                  | Bir işlem kalıcı olarak başarısız olması durumunda en fazla kaç kez Windows hizmeti tarih değiştirdiğinde güncelleştirin.          |
| WURescheduleTimeInMinutes | Int <br>(Varsayılan: 30) | Hata devam ederse durumunda, hizmet Windows update tarih değiştirdiğinde aralığı. |
| WUFrequency           | Virgülle ayrılmış bir dize (varsayılan: "Haftalık, Çarşamba, 7:00:00")     | Windows güncelleştirme sıklığı. Biçim ve olası değerler şunlardır: <br>-Örneğin, aylık, 5, 12 aylık, gg ss: 22:32.<br>Alanın değerlerini izin gg (gün) olan numaraları arasındaki aralığı 1-28 "son". <br> -Örneğin, haftalık, Salı, 12:22:32 için haftalık, gün, ss.  <br> -Örneğin, günlük, 12:22:32 günlük, ss.  <br> -Hiçbiri, Windows güncelleştirme yapılması olmamalıdır belirtir.  <br><br> Saatleri UTC biçiminde olduğunu unutmayın.|
| AcceptWindowsUpdateEula | Boolean <br>(Varsayılan: true) | Bu bayrak ayarlandığında, uygulamayı Windows güncelleştirmesi için son kullanıcı lisans sözleşmesi makinenin sahibi adına kabul eder.              |

> [!TIP]
> Windows Update, hemen gerçekleşmeye istiyorsanız ayarlayın `WUFrequency` göre uygulama dağıtım süresi. Örneğin, beş düğümlü test kümesi vardır ve uygulaması yaklaşık 5: 00'da dağıtmayı planladığınız varsayalım UTC. Uygulama yükseltme veya dağıtım sırasında en fazla 30 dakika sürer olduğunu varsayarsak, WUFrequency "Günlük, 17:30:00" ayarlayın.

## <a name="deploy-the-app"></a>Uygulamayı dağıtma

1. Küme hazırlamak için tüm önkoşul adımlarını tamamlayın.
2. Herhangi bir Service Fabric uygulaması gibi düzeltme eki düzenleme uygulaması dağıtın. PowerShell kullanarak uygulama dağıtabilirsiniz. Bağlantısındaki [PowerShell kullanarak dağıtma ve Kaldır uygulamaları](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications).
3. Dağıtım sırasında uygulama yapılandırmak için geçirmek `ApplicationParameter` için `New-ServiceFabricApplication` cmdlet'i. Kolaylık olması için uygulamanın yanı sıra betik Deploy.ps1 sağladık. Betiği kullanmak için:

    - Kullanarak bir Service Fabric kümesine bağlanın `Connect-ServiceFabricCluster`.
    - Uygun Deploy.ps1 PowerShell betiğini yürütün `ApplicationParameter` değeri.

> [!NOTE]
> Betik ve uygulama klasörü PatchOrchestrationApplication aynı dizinde tutun.

## <a name="upgrade-the-app"></a>Uygulama yükseltme

PowerShell kullanarak mevcut bir düzeltme eki düzenleme uygulamaya yükseltmek için adımları izleyin. [PowerShell kullanarak Service Fabric uygulaması yükseltme](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-upgrade-tutorial-powershell).

## <a name="remove-the-app"></a>Uygulamayı Kaldır

Uygulamayı kaldırmak için adımları [PowerShell kullanarak dağıtma ve Kaldır uygulamaları](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications).

Kolaylık olması için uygulamanın yanı sıra betik Undeploy.ps1 sağladık. Betiği kullanmak için:

  - Kullanarak bir Service Fabric kümesine bağlanın ```Connect-ServiceFabricCluster```.

  - Undeploy.ps1 PowerShell betiğini yürütün.

> [!NOTE]
> Betik ve uygulama klasörü PatchOrchestrationApplication aynı dizinde tutun.

## <a name="view-the-windows-update-results"></a>Windows güncelleştirme sonuçlarını görüntüleme

Düzeltme eki düzenleme uygulama kullanıcı için geçmiş sonuçlarını görüntülemek için REST API'lerini kullanıma sunar. Sonuç JSON örneği:
```json
[
  {
    "NodeName": "_stg1vm_1",
    "WindowsUpdateOperationResults": [
      {
        "OperationResult": 0,
        "NodeName": "_stg1vm_1",
        "OperationTime": "2019-05-13T08:44:56.4836889Z",
        "OperationStartTime": "2019-05-13T08:44:33.5285601Z",
        "UpdateDetails": [
          {
            "UpdateId": "7392acaf-6a85-427c-8a8d-058c25beb0d6",
            "Title": "Cumulative Security Update for Internet Explorer 11 for Windows Server 2012 R2 (KB3185319)",
            "Description": "A security issue has been identified in a Microsoft software product that could affect your system. You can help protect your system by installing this update from Microsoft. For a complete listing of the issues that are included in this update, see the associated Microsoft Knowledge Base article. After you install this update, you may have to restart your system.",
            "ResultCode": 0,
            "HResult": 0
          }
        ],
        "OperationType": 1,
        "WindowsUpdateQuery": "IsInstalled=0",
        "WindowsUpdateFrequency": "Daily,10:00:00",
        "RebootRequired": false
      }
    ]
  },
  ...
]
```

JSON alanlarının aşağıda açıklanmıştır.

Alan | Değerler | Ayrıntılar
-- | -- | --
OperationResult | 0 - başarılı<br> 1 - hatalarıyla başarılı oldu<br> 2 - başarısız oldu<br> 3 - durduruldu<br> 4 - zaman aşımı ile iptal edildi | Genel işlemin (genellikle bir veya daha fazla güncelleştirmelerin yüklenmesini içeren) sonucunu gösterir.
ResultCode | OperationResult aynı | Bu alan tek güncelleştirme için yükleme işleminin sonucu gösterir.
OperationType | 1 - yükleme<br> 0 - arayın ve yükleyin.| Yükleme Sonuçları varsayılan olarak gösteriliyordu yalnızca OperationType olur.
WindowsUpdateQuery | Varsayılan değer "IsInstalled = 0" |Windows güncelleştirmeleri aramak için kullanılan sorguyu güncelleştirin. Daha fazla bilgi için [WuQuery.](https://msdn.microsoft.com/library/windows/desktop/aa386526(v=vs.85).aspx)
RebootRequired | TRUE - yeniden başlatma gerekli<br> false - yeniden başlatma gerekli değil | Yeniden başlatma güncelleştirmeleri yüklemesini tamamlamak için gerekli olup olmadığını gösterir.
OperationStartTime | DateTime | Başlatılan hangi operation(Download/Installation) zaman gösterir.
OperationTime | DateTime | Tamamlanan hangi operation(Download/Installation) zaman gösterir.
HResult | 0 - başarılı<br> diğer - hata| Windows Update ile UpdateID "7392acaf-6a85-427c-8a8d-058c25beb0d6" hatanın nedenini gösterir.

Hiçbir güncelleştirme henüz zamanlanmış, JSON sonuç boş olur.

Windows Update Sorgulanacak kümeye sonuçları oturum açın. Ardından Coordinator hizmetinin birincil çoğaltması adresini öğrenmek ve tarayıcı URL'den isabet: http://&lt;çoğaltma IP&gt;:&lt;ApplicationPort&gt;/PatchOrchestrationApplication/v1 / GetWindowsUpdateResults.

REST uç noktasını Koordinatör hizmeti için dinamik bir bağlantı noktası var. Tam URL'yi kontrol etmek için Service Fabric Explorer'a bakın. Örneğin, sonuçları kullanılabilir `http://10.0.0.7:20000/PatchOrchestrationApplication/v1/GetWindowsUpdateResults`.

![REST uç noktası görüntüsü](media/service-fabric-patch-orchestration-application/Rest_Endpoint.png)


Ters proxy kümede etkinleştirilirse, aynı zamanda kümedeki dışında URL'den erişebilirsiniz.
Http:// isabet gereken uç noktadır&lt;SERVERURL&gt;:&lt;REVERSEPROXYPORT&gt;/PatchOrchestrationApplication/CoordinatorService/v1/GetWindowsUpdateResults.

Ters proxy küme üzerinde etkinleştirmek için adımları [ters proxy Azure Service fabric'te](https://docs.microsoft.com/azure/service-fabric/service-fabric-reverseproxy). 

> 
> [!WARNING]
> Ters proxy yapılandırdıktan sonra bir HTTP uç noktasını açığa tüm mikro hizmetler kümedeki küme dışında'ten adreslenebilir.

## <a name="diagnosticshealth-events"></a>Tanılama/sistem durumu olayları

Aşağıdaki bölümde, Service Fabric kümelerinde düzeltme eki düzenleme uygulaması aracılığıyla düzeltme eki güncelleştirmeleri ile ilgili sorunlar hata ayıklama/tanılama konusunda konuşuyor.

> [!NOTE]
> V1.4.0 sürümünü POA birçok alabilmek için yüklü olmalıdır. aşağıda kendi kendine tanılama geliştirmeleri çağrılır.

NodeAgentNTService oluşturur [onarım görevleri](https://docs.microsoft.com/dotnet/api/system.fabric.repair.repairtask?view=azure-dotnet) düğümlerinde güncelleştirmeleri yüklemek için. Her görev, görev onay ilkesine göre ardından CoordinatorService tarafından hazırlanır. Hazırlanan görevleri son onarım küme sağlıksız durumda ise, herhangi bir görev onaylayacak değil Yöneticisi tarafından onaylanır. Sağlayan adım adım nasıl bir düğümde güncelleştirmeleri devam anlamak için gidin.

1. Her bir düğümde çalışan NodeAgentNTService, zamanlanan saatte kullanılabilir Windows güncelleştirmesi arar. Güncelleştirmeler varsa, ilerler ve bunları düğümde indirir.
2. Güncelleştirmeler İndirildikten sonra NodeAgentNTService, POS___ < unique_id > ada sahip düğüm için karşılık gelen onarım görevi oluşturur. Bir görüntüleyebilir bu onarım cmdlet'ini kullanarak görevleri [Get-ServiceFabricRepairTask](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricrepairtask?view=azureservicefabricps) veya düğüm Ayrıntılar bölümünde SFX. Onarım görevi oluşturulduktan sonra hızlı bir şekilde taşır [talep durumu](https://docs.microsoft.com/dotnet/api/system.fabric.repair.repairtaskstate?view=azure-dotnet).
3. Düzenleyicisi hizmeti düzenli aralıklarla onarım görevleri talep edilen durumunda arar ve ilerler ve üzerinde TaskApprovalPolicy dayanarak bir durumdan hazırlamaya güncelleştirir. Yalnızca varsa başka bir onarım görevi şu anda hazırlama/Onaylandı/Executing/geri yükleme durumunda TaskApprovalPolicy NodeWise, bir düğüme karşılık gelen bir onarım görevi olacak şekilde yapılandırıldıysa hazır olan. Benzer şekilde, UpgradeWise TaskApprovalPolicy herhangi bir noktada sağlamış durumda görevler vardır, aynı yükseltme etki alanına ait düğümleri için yukarıdaki durumlarda. Onarım görevi durumu hazırlamaya taşındıktan sonra karşılık gelen Service Fabric düğümüdür [devre dışı](https://docs.microsoft.com/powershell/module/servicefabric/disable-servicefabricnode?view=azureservicefabricps) amacıyla "Yeniden Başlat" olarak.

   POA(v1.4.0 and Above) özelliği ile olayları "ClusterPatchingStatus" yama düğümler görüntülenecek CoordinaterService üzerinde gönderir. Aşağıdaki resim güncelleştirmeleri gösterir _poanode_0 üzerinde yüklü:

    [![Küme durumu düzeltme eki uygulama görüntüsü](media/service-fabric-patch-orchestration-application/clusterpatchingstatus.png)](media/service-fabric-patch-orchestration-application/clusterpatchingstatus.png#lightbox)

4. Onarım görevi, düğümü devre dışı bırakıldıktan sonra yürütülme durumunda taşınır. Not, bir düğüm devre dışı bırakırken de takılı olduğundan durumuna, sonra hazırlarken takılmış bir onarım görevi, yeni onarım görevi engelleme neden ve bu nedenle küme düzeltme eki uygulama durdur.
5. Onarım görevi durumu yürütülürken olduğunda, o düğümle düzeltme eki yükleme başlar. Burada, düzeltme eki yükledikten sonra düğüm olabilir veya düzeltme eki bağlı olarak yeniden değil. Onarım görevi durumu geri düğümü yeniden sağlar ve ardından geri yüklemek üzere taşınır sonrası tamamlandı olarak işaretlenir.

   V1.4.0 ve sonraki sürümlerine uygulamanın güncelleştirme durumunu sistem durumu olaylarını NodeAgentService üzerinde özelliğiyle "WUOperationStatus-[NodeName]" bakarak bulunabilir. Aşağıdaki resimlerde vurgulanan bölümler, windows Update'te düğümü 'poanode_0' ve 'poanode_2' durumunu gösterir:

   [![Windows güncelleştirme işlemi durumu görüntüsü](media/service-fabric-patch-orchestration-application/wuoperationstatusa.png)](media/service-fabric-patch-orchestration-application/wuoperationstatusa.png#lightbox)

   [![Windows güncelleştirme işlemi durumu görüntüsü](media/service-fabric-patch-orchestration-application/wuoperationstatusb.png)](media/service-fabric-patch-orchestration-application/wuoperationstatusb.png#lightbox)

   Bir de alabilirsiniz kümeye bağlanarak ve onarım görevi kullanılarak durumunu getirilirken powershell kullanmayla ilgili ayrıntılar [Get-ServiceFabricRepairTask](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricrepairtask?view=azureservicefabricps). Örnekte gösterildiği bu "933c 4774 85 POS__poanode_2_125f2969 d 1-ebdf85e79f15" görev DownloadComplete durumda gibidir. Bu düğümde "poanode_2" Güncelleştirmeler yüklendi ve görevin yürütülme durumunda geçtiğinde yükleme denenecek anlamına gelir.

   ``` powershell
    D:\service-fabric-poa-bin\service-fabric-poa-bin\Release> $k = Get-ServiceFabricRepairTask -TaskId "POS__poanode_2_125f2969-933c-4774-85d1-ebdf85e79f15"

    D:\service-fabric-poa-bin\service-fabric-poa-bin\Release> $k.ExecutorData
    {"ExecutorSubState":2,"ExecutorTimeoutInMinutes":90,"RestartRequestedTime":"0001-01-01T00:00:00"}
    ```

   Varsa ardından bulunacak hala daha fazla belirli VM/VM'ler için Windows olay günlüklerini kullanarak sorun hakkında daha fazlasını bulmak için oturum açın. Yukarıda bahsedilen onarım görevi yalnızca bu Yürütücü alt durumlara sahip:

      ExecutorSubState | Ayrıntı
    -- | -- 
      None = 1 |  Devam eden bir işlem düğümünde olmadığı anlamına gelir. Olası durum geçişlerini.
      DownloadCompleted = 2 | İndirme işlemi başarılı bir şekilde, kısmi tamamlandığını gösterir ya da hata.
      InstallationApproved = 3 | Önceki indirme işlemi tamamlandıktan sonra ve yüklemeyi onarım Yöneticisi onaylayana anlamına gelir.
      InstallationInProgress=4 | Onarım görevi yürütme durumuna karşılık gelir.
      InstallationCompleted=5 | Yükleme başarı, Kısmi başarı veya hata ile tamamlandı anlamına gelir.
      RestartRequested = 6 | Yükleme Tamamlandı düzeltme eki uygulama ve düğüm üzerinde bir bekleyen yeniden başlatma eylemi yoktur anlamına gelir.
      RestartNotNeeded = 7 |  Düzeltme ekini yüklemesi tamamlandıktan sonra yeniden başlatma gerekli değildir anlamına gelir.
      RestartCompleted = 8 | Başarıyla tamamlandı, yeniden başlatma gerektirir.
      OperationCompleted 9 = | Windows için güncelleştirme işlemi başarıyla tamamlandı.
      OperationAborted 10 = | Windows güncelleştirme işlemi iptal edildiğini gösterir.

6. V1.4.0 içinde ve bir düğüm üzerinde güncelleştirme girişimi tamamlandığında, yukarıda uygulamasının özelliği "WUOperationStatus-[NodeName]" ile bir olay olduğunda sonraki deneyecek bildirmek için indirme ve güncelleştirme yükleme, başlatma için NodeAgentService gönderilir. Aşağıdaki resme bakın:

     [![Windows güncelleştirme işlemi durumu görüntüsü](media/service-fabric-patch-orchestration-application/wuoperationstatusc.png)](media/service-fabric-patch-orchestration-application/wuoperationstatusc.png#lightbox)

### <a name="diagnostic-logs"></a>Tanılama günlükleri

Düzeltme eki düzenleme uygulama günlükleri, Service Fabric çalışma zamanı günlüklerini bir parçası olarak toplanır.

Tanılama Aracı/ardışık seçtiğiniz aracılığıyla günlükleri tutmak istemeniz durumunda. Düzeltme eki düzenleme uygulaması aracılığıyla olaylarını günlüğe kaydedecek şekilde sabit sağlayıcısını kimlikleri kullanır [olay kaynağı](https://docs.microsoft.com/dotnet/api/system.diagnostics.tracing.eventsource?view=netframework-4.5.1)

- e39b723c-590c-4090-abb0-11e3e6616346
- fc0028ff-bfdc-499f-80dc-ed922c52c5e9
- 24afa313-0d3b-4c7c-b485-1047fd964b60
- 05dc046c-60e9-4ef7-965e-91660adffa68

### <a name="health-reports"></a>Sistem durumu raporlarının sayısı

Orchestration düzeltme eki uygulama, aşağıdaki durumlarda karşı Coordinator hizmetini veya düğüm Aracısı sistem durumu raporlarının de yayımlar:

#### <a name="the-node-agent-ntservice-is-down"></a>Düğüm Aracısı NTService çalışmıyor

Düğüm Aracısı NTService bir düğüm üzerinde çalışmıyorsa, bir uyarı düzeyi sistem durumu raporu karşı düğüm Aracısı hizmeti oluşturulur.

#### <a name="the-repair-manager-service-is-not-enabled"></a>Onarım Yöneticisi hizmeti etkin değil

Onarım Yöneticisi hizmeti kümede bulunamazsa bir uyarı düzeyi sistem durumu rapor Düzenleyicisi hizmeti için oluşturulur.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

S. **Düzeltme eki düzenleme uygulama çalışırken hata durumunda kümem neden görüyorum?**

A. Yükleme işlemi sırasında orchestration düzeltme eki uygulamayı devre dışı bırakır veya geçici olarak giderek kümesinin sistem sonuçlanabilir düğümü yeniden başlatır.

İlke uygulama için bağlı olarak, ya da bir düğüm bir düzeltme eki uygulama işlemi sırasında gidebilirsiniz *veya* tüm yükseltme etki alanı aynı anda Git aşağı.

Windows güncelleştirme yüklemesi sonunda, düğümler yeniden iler hale getirilir yeniden başlatma gönderin.

Geçici bir hata durumu aşağıdaki örnekte, küme oluştu çünkü iki düğümü olan aşağı ve MaxPercentageUnhealthyNodes ilke ihlal. Düzeltme eki uygulama işlemi devam ediyor kadar geçici bir hatadır.

![Sağlıksız kümesinin görüntüsü](media/service-fabric-patch-orchestration-application/MaxPercentage_causing_unhealthy_cluster.png)

Sorun devam ederse, sorun giderme bölümüne bakın.

S. **Uyarı durumunda düzeltme eki düzenleme uygulama**

A. Uygulamaya karşı gönderilen bir sistem durumu raporu, kök neden olup olmadığını denetleyin. Genellikle, uyarı sorunun ayrıntılarını içerir. Geçici bir sorundur, uygulamayı bu durumdan otomatik olarak kurtarmak için bekleniyor.

S. **Kümem sağlıksız olduğunu ve Acil işletim sistemi güncelleştirme yapmanız durumunda neler yapabilirim?**

A. Küme sağlıksız durumdayken düzeltme eki düzenleme uygulama güncelleştirmelerini yüklemez. Kümenizi düzenleme düzeltme eki uygulama iş akışı engelini kaldırmak için sağlıklı bir duruma getirmek deneyin.

S. **Ben TaskApprovalPolicy 'NodeWise' veya 'UpgradeDomainWise' olarak kümem için ayarlamalı mıyım?**

A. 'UpgradeDomainWise' genel küme daha hızlı bir şekilde paralel bir yükseltme etki alanına ait olan tüm düğümleri düzeltme eki uygulama düzeltme eki uygulama sağlar. Bu tüm bir yükseltme etki alanına ait düğümleri kullanılabilir olacağı anlamına gelir (içinde [devre dışı](https://docs.microsoft.com/dotnet/api/system.fabric.query.nodestatus?view=azure-dotnet#System_Fabric_Query_NodeStatus_Disabled) durumu) düzeltme eki uygulama işlemi sırasında.

Buna 'NodeWise' İlkesi aynı anda yalnızca tek bir düğüme yamaları, bu küme genel düzeltme eki uygulama uzun zaman alabileceğini anlamına gelir. Ancak, max yalnızca bir düğüm kullanılamaz durumda olurdu (içinde [devre dışı bırakılmış](https://docs.microsoft.com/dotnet/api/system.fabric.query.nodestatus?view=azure-dotnet#System_Fabric_Query_NodeStatus_Disabled) durumu) düzeltme eki uygulama işlemi sırasında.

Kümenizi yükseltme etki alanları N-1 sayısı daha sonra ilkeyi 'UpgradeDomainWise' olarak ayarlayabilir (N kümenizdeki yükseltme etki alanlarının sayısı olduğu) döngüsü, düzeltme eki uygulama sırasında çalışan toleransına sahipse, aksi takdirde 'NodeWise için' ayarlayın.

S. **Ne kadar zaman mevcut bir düğüm düzeltme eki uygulama Al?**

A. Bir düğüm düzeltme eki uygulama dakika sürebilir (örneğin: [Windows Defender tanım güncelleştirmeleri](https://www.microsoft.com/en-us/wdsi/definitions)) saat için (örneğin: [Windows toplu güncelleştirmeler](https://www.catalog.update.microsoft.com/Search.aspx?q=windows%20server%20cumulative%20update)). Bir düğüm düzeltme eki için gereken süre, çoğunlukla bağlıdır 
 - Güncelleştirmeleri boyutu
 - Düzeltme eki uygulayan bir pencere içinde uygulanacak olan güncelleştirme sayısı
 - Bu güncelleştirmeleri yüklemek, (gerekirse) düğümü yeniden başlatma ve yeniden başlatma sonrası yükleme adımlarını tamamlamak için geçen süre.
 - VM/makine ve ağ koşullarını performans.

S. **Ne kadar bir kümenin tamamını düzeltme eki sürer?**

A. Bir kümenin tamamını düzeltme eki için gereken süre aşağıdaki etkenlere bağlıdır:

- Bir düğüm düzeltme eki için gereken süre.
- İlke Düzenleyicisi hizmeti. -Varsayılan ilke `NodeWise`, sonuçları daha yavaş olacaktır bir anda yalnızca tek bir düğüme düzeltme `UpgradeDomainWise`. Örneğin: Bir düğüm yama uygulanacak yaklaşık 1 saat sürerse, 20 düzeltme eki için (düğümlerinin aynı türü) bir küme düğümünde 5 yükseltme etki alanları ile her biri 4 düğüm içeren.
    - İlke, tüm küme düzeltme eki yaklaşık 20 saat sürer `NodeWise`
    - İlke yaklaşık 5 saat sürer `UpgradeDomainWise`
- Küme yükleme - düzeltme eki uygulama işlemi her müşterinin iş yükü için kullanılabilir diğer küme düğümleri yeniden konumlandırma gerektirir. Düzeltme eki aşamasında düğüm olacak [devre dışı bırakılması](https://docs.microsoft.com/dotnet/api/system.fabric.query.nodestatus?view=azure-dotnet#System_Fabric_Query_NodeStatus_Disabling) bu süre boyunca durum. Küme yoğun yük çalışıyorsa, devre dışı bırakma işlemi uzun sürecektir. Bu nedenle genel düzeltme eki uygulama işlemini vurgulu böylesi yavaş görünebilir.
- Sistem durumu hataları düzeltme sırasında - her küme [performans düşüşü](https://docs.microsoft.com/dotnet/api/system.fabric.health.healthstate?view=azure-dotnet#System_Fabric_Health_HealthState_Error) içinde [küme durumunu](https://docs.microsoft.com/azure/service-fabric/service-fabric-health-introduction) düzeltme eki uygulama işlemini kesersiniz. Bu, tüm küme düzeltme eki için gereken toplam süreyi eklersiniz.

S. **Bazı güncelleştirmeler Windows Update sonuçlarında REST API aracılığıyla ancak makinedeki Windows güncelleştirme geçmişini altında elde neden görüyorum?**

A. Bazı ürün güncelleştirmeleri, yalnızca ilgili güncelleştirme/düzeltme eki geçmişlerini görünür. Örneğin, Windows Defender'ın güncelleştirmeleri olabilir veya Windows Server 2016, Windows Update geçmişinde görünmeyebilir.

S. **Düzeltme ekini düzenlemeyi uygulama geliştirme kümem (tek düğümlü kümenize) düzeltme eki için kullanılabilir mi?**

A. Hayır, düzeltme eki düzenleme uygulama düzeltme eki tek düğümlü küme için kullanılamaz. Bu tasarım gereği, olarak sınırlamasıdır [service fabric sistem hizmetlerinin](https://docs.microsoft.com/azure/service-fabric/service-fabric-technical-overview#system-services) veya herhangi bir müşteri uygulama kapalı kalma süresi karşılaşır ve bu nedenle düzeltme eki uygulama için herhangi bir onarım işi hiçbir zaman onarım Yöneticisi tarafından onaylanan.

S. **Linux küme düğümlerinde nasıl düzeltme eki?**

A. Bkz: [Azure sanal makine ölçek kümesinin otomatik işletim sistemi görüntüsü yükseltmeleri](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-automatic-upgrade) güncelleştirmeleri linux üzerinde tümleştirmeye.

S.**neden olduğu güncelleştirme döngüsü kadar uzun sürmesi?**

A. Sorgu sonucu json ve ardından, tüm düğümleri ve daha sonra güncelleştirme döngüsü girişi aracılığıyla go OperationStartTime ve OperationTime(OperationCompletionTime) kullanarak her bir düğümde güncelleştirme yüklemesi tarafından harcanan süre öğrenmek deneyebilirsiniz. Büyük bir zaman penceresi içinde ise hangi güncelleştirme yüklemesiyle, küme hata durumdaydı ve herhangi bir POA onarım görevi nedeniyle bu onarım Yöneticisi onaylamadı nedeni şunlardan biri olabilir. Ardından, güncelleştirme yüklemesi herhangi bir düğümde uzun sürdü, düğüm uzun süreden güncelleştirilmedi ve çok sayıda güncelleştirme sürdü yükleme olan mümkün olabilir. Ayrıca olabilir bir durumda, bir düğümde düzeltme eki uygulama engellendi düğümü düğümü devre dışı olduğundan genellikle gerçekleşen durumu devre dışı bırakılırken takılması nedeniyle çekirdek/veri kaybı durumlar için neden olabilir.

S. **Neden POA düzeltme düğümü devre dışı bırakmak için gereklidir?**

A. Düzeltme eki düzenleme uygulaması düğüm üzerinde çalışan tüm Service fabric hizmetleri durdurur/yeniden ayırır 'Başlat' hedefi olan düğüm devre dışı bırakır. Bu, bir düğüm devre dışı bırakmadan düzeltme eki uygulama önerilmez şekilde DLL'leri, eski ve yeni bir karışımını kullanarak uygulamaları bitmeyen emin olmak için gerçekleştirilir.

## <a name="disclaimers"></a>Bildirimler

- Orchestration düzeltme eki uygulama, kullanıcı adına Windows Update, son kullanıcı lisans sözleşmesi kabul eder. İsteğe bağlı olarak, ayar uygulama yapılandırmasında kapatılabilir.

- Düzeltme eki düzenleme uygulama kullanımını ve performansını izlemek için telemetri toplar. Uygulamanın telemetri (Bu varsayılan olarak etkindir) Service Fabric çalışma zamanı telemetri ayarı ayarını izler.

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

Düzeltme eki düzenleme uygulaması, yeniden zamanlama ilkesine göre bir Windows güncelleştirmesi yüklemeyi dener. Hizmet, düğüm kurtarmak ve uygulama ilkesine göre güncelleştirmeyi atlama dener.

Böyle bir durumda bir uyarı düzeyi sistem durumu raporu karşı düğüm Aracısı hizmeti oluşturulur. İçin Windows Update sonucu, hatanın olası nedeni de içerir.

### <a name="the-health-of-the-cluster-goes-to-error-while-the-update-installs"></a>Güncelleştirmeyi yüklerken küme durumunu hata durumuna geçer

Hatalı bir Windows güncelleştirmesi, bir uygulama veya belirli bir düğüm veya yükseltme etki alanı küme durumunu aşağı getirebilirsiniz. Düzeltme eki düzenleme uygulama yeniden kümenin sağlıklı olup kadar sonraki tüm Windows güncelleştirme işlemi sona erdirir.

Bir yönetici, müdahale ve uygulama veya küme neden Windows güncelleştirmesi nedeniyle sağlıksız olduğunu belirleyin.

## <a name="release-notes"></a>Sürüm Notları

>[!NOTE]
> 1\.4.0 sürümünden başlayarak, sürüm notları ve sürümleri bulunabilir GitHub sürümde [sayfa](https://github.com/microsoft/Service-Fabric-POA/releases/).

### <a name="version-110"></a>Sürüm 1.1.0
- Genel sürüm

### <a name="version-111"></a>Sürüm 1.1.1
- SetupEntryPoint, NodeAgentNTService yüklenmesini önleyen NodeAgentService içinde bir hata düzeltildi.

### <a name="version-120"></a>Sürümü 1.2.0

- Sistem geçici hata düzeltmeleri, iş akışını yeniden başlatın.
- Hata düzeltmesi hangi sistem durumu nedeniyle onarım görevlerin hazırlanması sırasında onay beklendiği gibi olduğunu değildi RM görevler oluşturma.
- Windows POANodeSvc otomatik otomatik Gecikmeli hizmetinin başlatma modu değiştirildi.

### <a name="version-121"></a>1\.2.1 sürümü

- Küme ölçek azaltma iş akışında hata düzeltmesi. Çöp toplama mantıksal var olmayan düğümlerine ait POA onarım görevler için kullanıma sunuldu.

### <a name="version-122"></a>Sürüm 1.2.2

- Çeşitli hata düzeltmeleri.
- İkili dosyaları artık imzalanmıştır.
- Uygulama için sfpkg bağlantısı eklendi.

### <a name="version-130"></a>Sürüm 1.3.0

- False olarak InstallWindowsOSOnlyUpdates ayarı artık kullanılabilir tüm güncelleştirmeleri yükler.
- Otomatik Güncelleştirmeler devre dışı bırakma mantığı değiştirildi. Bu, burada otomatik güncelleştirmeler Server 2016 ve üzeri devre dışı değil bir hatayı düzeltir.
- Her iki Gelişmiş kullanım örnekleri için POA, mikro hizmetler için parametreli yerleştirme kısıtlaması.

### <a name="version-131"></a>Sürüm 1.3.1
- Regresyon, Windows Server 2012 R2 ya da otomatik güncelleştirmeler devre dışı bırakma hatası nedeniyle daha düşük POA 1.3.0 nerede çalışmaz düzeltiliyor. 
- Hata nerede InstallWindowsOSOnlyUpdates yapılandırma her zaman True olarak çekilir düzeltiliyor.
- InstallWindowsOSOnlyUpdates varsayılan değerini False olarak değiştiriliyor.

### <a name="version-132"></a>Sürüm 1.3.2
- Geçerli düğüm adı alt kümesi olan bir ada sahip düğüm olasılığına düzeltme eki uygulama yaşam döngüsü bir düğümde parametreden bir sorunu giderme. Bu düğümleri, kendi olası düzeltme eki uygulama eksik veya yeniden başlatma beklemede. 