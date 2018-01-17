---
title: "Hyper-V’den Azure’a Azure Site Recovery dağıtım planlayıcısı| Microsoft Docs"
description: "Bu makalede Hyper-V'den Azure dağıtım senaryosu için Azure Site Recovery dağıtım planlayıcısının çalışma modu açıklanır."
services: site-recovery
documentationcenter: 
author: nsoneji
manager: garavd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 12/02/2017
ms.author: nisoneji
ms.openlocfilehash: 5c7ff99c2f67f82f9a7d605d9960960f84e96900
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2018
---
# <a name="run-azure-site-recovery-deployment-planner-for-hyper-v-to-azure"></a>Hyper-V’den Azure’a Azure Site Recovery dağıtım planlayıcısını çalıştırma

## <a name="modes-of-running-deployment-planner"></a>Dağıtım planlayıcısını çalıştırma modları
Komut satırı aracını (ASRDeploymentPlanner.exe) aşağıdaki dört modun herhangi birinde çalıştırabilirsiniz: 
1.  [VM listesini alma](#get-vm-list-for-profiling-hyper-v-vms)
2.  [Profil oluşturma](#profile-hyper-v-vms)
3.  [Rapor oluşturma](#generate-report)
4.  [Aktarım hızı alma](#get-throughput)

Öncelikle, bir veya birden fazla Hyper-V konağından VM’lerin listesini almak için aracı çalıştırın.  Ardından VM veri değişim sıklığı ve IOPS toplamak için aracı profil oluşturma modunda çalıştırın. Ardından, ağ bant genişliği ve depolama gereksinimlerini bulmak üzere raporu oluşturmak için aracı çalıştırın.

## <a name="get-vm-list-for-profiling-hyper-v-vms"></a>Hyper-V VM’lerinin profilini oluşturmak için VM listesi alma
İlk olarak, profili oluşturulacak sanal makinelerin bir listesi gerekir. Birden çok Hyper-V konağında bulunan VM’lerin listesini tek bir komutta oluşturmak için Dağıtım planlayıcısı arasının GetVMList modunu kullanın. Tam listeyi oluşturduktan sonra, profilini oluşturmak istemediğiniz VM’leri çıkış dosyasından kaldırabilirsiniz. Ardından profil oluşturma, rapor oluşturma ve aktarım hızı alma gibi diğer tüm işlemler için bu çıkış dosyasını kullanın.

Aracı bir Hyper-V kümesine, tek başına bir Hyper-V konağına veya tümünün birleşimine yönlendirerek VM listesini oluşturabilirsiniz.

### <a name="command-line-parameters"></a>Komut satırı parametreleri
Aşağıdaki tabloda, aracın GetVMList modunda çalışması için gereken zorunlu ve isteğe bağlı parametreler listelenmiştir. 
```
ASRDeploymentPlanner.exe -Operation GetVMList /?
```
| Parametre adı | Açıklama |
|---|---|
| -Operation | GetVMList |
| -User | Hyper-V konağı veya Hyper-V kümesine bağlanmak için kullanıcı adı. Kullanıcının yönetici erişimi olmalıdır.|
|-ServerListFile | Profili oluşturulacak VM’leri içeren sunucuların listesinin bulunduğu dosya. Dosya yolu mutlak veya göreli olabilir. Bu dosya her satırda aşağıdakilerden birini içermelidir:<ul><li>Hyper-V konak adı veya IP adresi</li><li>Hyper-V küme adı veya IP adresi</li></ul><br>Örnek: “ServerList.txt” dosyası aşağıdaki sunucuları içerir:<ul><li>Host_1</li><li>10.8.59.27</li><li>Cluster_1</li><li>Host_2</li>|
| -Directory|(İsteğe bağlı) Bu işlem sırasında oluşturulan verileri depolamak için evrensel adlandırma kuralı (UNC) veya yerel dizin yolu. Belirtilmemişse, geçerli yolun altındaki “ProfiledData” adlı dizin varsayılan dizin olarak kullanılır.|
|-OutputFile| (İsteğe bağlı) Belirtilen Hyper-V sunucularından alınan VM'lerin listesinin kaydedildiği dosya. Ad belirtilmezse, ayrıntılar "VMList.txt" dosyasında depolanır.  Profili gerekmeyen VM'ler kaldırdıktan sonra profil oluşturmayı başlatmak için bu dosyayı kullanın.|
|-Password|(İsteğe bağlı) Hyper-V konağına bağlanmak için parola.   Parola parametre olarak belirtilmezse, daha sonra komut yürütülürken sorulacaktır.|

### <a name="how-does-getvmlist-discovery-work"></a>GetVMList bulma işlemi nasıl çalışır?
**Hyper-V kümesi**: Hyper-V küme adı sunucular listesi dosyasında belirtildiğinde, araç kümenin tüm Hyper-V düğümlerinin bulur ve her Hyper-V konağında mevcut olan VM'leri alır.

**Hyper-V konağı**: Hyper-V ana bilgisayar adı verildiğinde, araç öncelikle bu adın bir kümeye ait olup olmadığını denetler. Öyleyse, kümeye ait düğümleri getirir. Ardından sanal makineleri her Hyper-V ana bilgisayarından alır. 

Dilerseniz profilini el ile oluşturmak istediğiniz sanal makinelerin kolay adlarını veya IP adreslerini bir dosyada listeleyebilirsiniz.

Çıktı dosyasını Not Defteri’nde açın ve sonra profilini oluşturmak istediğiniz tüm VM’lerin adlarını, her satıra bir VM gelecek şekilde başka bir dosyaya (örneğin, ProfileVMList.txt) kopyalayın. Bu dosya, profil oluşturma, rapor oluşturma ve aktarım hızı alma gibi diğer tüm işlemler için aracın - VMListFile parametresinde girdi olarak kullanılır.

### <a name="example-1-to-store-the-list-of-vms-in-a-file"></a>Örnek 1: VM'lerin listesini bir dosyaya depolamak için
```
ASRDeploymentPlanner.exe -Operation GetVMlist -ServerListFile “E:\Hyper-V_ProfiledData\ServerList.txt" -User Hyper-VUser1 -OutputFile "E:\Hyper-V_ProfiledData\VMListFile.txt"
```

### <a name="example-2-to-store-the-list-of-vms-at-the-default-location-ie--directory-path"></a>Örnek 2: VM'lerin listesini varsayılan konumda (-Dizin yolu) depolamak için
```
ASRDeploymentPlanner.exe -Operation GetVMList -Directory "E:\Hyper-V_ProfiledData" -ServerListFile "E:\Hyper-V_ProfiledData\ServerList.txt" -User Hyper-VUser1
```

## <a name="profile-hyper-v-vms"></a>Hyper-V VM’lerinin profilini oluşturma
Profil oluşturma modunda dağıtım planlayıcısı aracı, sanal makineye ilişkin performans verilerini toplamak için Hyper-V konaklarının her birine bağlanır. 

* Profil oluşturma işlemi sırasında üretim VM’leri ile doğrudan bağlantı kurulmadığı için bu VM’lerin performansı etkilenmez. Tüm performans verileri Hyper-V konağından toplanır.
* Araç, profil doğruluğunu sağlamak için Hyper-V konağını her 15 saniyede bir kez sorgular ve her dakikanın performans sayacı verilerinin ortalamasını depolar.
* Araç, kümedeki bir düğümden başka bir düğüme VM geçişini ve bir konak içindeki depolama geçişini sorunsuz bir şekilde işler.

### <a name="get-vm-list-to-profile"></a>Profili oluşturulacak VM listesini alma
Profili oluşturulacak VM’lerin listesini oluşturmak için [GetVMList](#get-vm-list-for-profiling-hyper-v-vms) işlemine başvurun

Profili oluşturulacak sanal makinelerin listesini oluşturduktan sonra, aracı profil oluşturma modunda çalıştırabilirsiniz. 

### <a name="command-line-parameters"></a>Komut satırı parametreleri
Aşağıdaki tabloda, aracın profil oluşturma modunda çalışması için gereken zorunlu ve isteğe bağlı parametreler listelenmiştir. Bu araç, hem VMware’den Azure’a hem de Hyper-V’den Azure’a senaryoları için sıkça kullanılır. Aşağıdaki parametreler Hyper-V’de kullanılabilir. 
```
ASRDeploymentPlanner.exe -Operation StartProfiling /?
```
| Parametre adı | Açıklama |
|---|---|
| -Operation | StartProfiling |
| -User | Hyper-V konağı veya Hyper-V kümesine bağlanmak için kullanıcı adı. Kullanıcının yönetici erişimi olmalıdır.|
| -VMListFile | Profili oluşturulacak VM’lerin listesini içeren dosya. Dosya yolu mutlak veya göreli olabilir. Hyper-V için, GetVMList işleminin çıkış dosyası bu dosyadır. El ile hazırlıyorsanız, dosyada bir sunucu adı ya da IP adresi ve ardından her satırı \ ile ayrılan VM adı bulunmalıdır. Dosyada belirtilen VM adı, Hyper-V konağındaki VM adıyla aynı olmalıdır.<ul>Örnek: "VMList.txt" dosyası aşağıdaki sanal makineleri içerir:<ul><li>Host_1\VM_A</li><li>10.8.59.27\VM_B</li><li>Host_2\VM_C</li><ul>|
|-NoOfMinutesToProfile|Profil oluşturmanın çalıştırılacağı dakika sayısı. En az 30 dakika olmalıdır.|
|-NoOfHoursToProfile|Profil oluşturmanın çalıştırılacağı saat sayısı.|
|-NoOfDaysToProfile |Profil oluşturmanın çalıştırılacağı gün sayısı. Ortamınızda belirtilen süre içindeki iş yükü deseninin doğru bir öneri sağlayacak şekilde gözlemlenip kullanılması için profil oluşturma işleminin 7 günden uzun süre çalıştırılmaması önerilir.|
|-Sanallaştırma|Sanallaştırma türünü (VMware veya Hyper-V) belirtin.|
|-Directory|(İsteğe bağlı) Profil oluşturma sırasında oluşturulan profil oluşturma verilerini depolamak için evrensel adlandırma kuralı (UNC) veya yerel dizin yolu. Belirtilmemişse, geçerli yolun altındaki “ProfiledData” adlı dizin varsayılan dizin olarak kullanılır.|
|-Password|(İsteğe bağlı) Hyper-V konağına bağlanmak için parola. Şu anda belirtilmezse, daha sonra komut yürütülürken sorulacaktır.|
|-StorageAccountName|(İsteğe bağlı) Şirket içinden Azure’a veri çoğaltma için ulaşılabilir aktarım hızını bulmak için depolama hesabı adı. Araç, aktarım hızını hesaplamak için test verilerini bu depolama hesabına yükler.|
|-StorageAccountKey|(İsteğe bağlı) Depolama hesabına erişmek için kullanılan depolama hesabı anahtarı. Azure portalı > Depolama hesapları > <Storage account name> > Ayarlar > Erişim Anahtarları > Anahtar1 (veya klasik depolama hesabı için birincil erişim anahtarı) adımlarını izleyin.|
|-Ortam|(İsteğe bağlı) Bu, hedef Azure Depolama hesabı ortamınızdır. Şu üç değerden herhangi birini alabilir: AzureCloud,AzureUSGovernment, AzureChinaCloud. Varsayılan seçenek AzureCloud değeridir. Hedef Azure bölgeniz Azure US Government veya Azure China bulutları olduğunda ilgili parametreyi kullanın.|

VM’lerinizin en az 7 günlük profilinin oluşturulması önerilir. Değişim sıklığı bir ay içinde değişiklik gösteriyorsa, bu sıklığın en yüksek seviyeye ulaştığı hafta sırasında profil oluşturmanız önerilir. En iyi yöntem, daha iyi öneriler almak için 31 günlük profil oluşturmaktır. Profil oluşturma süresi boyunca ASRDeploymentPlanner.exe çalışmaya devam eder. Araç, profil oluşturma süre girdisini gün cinsinden alır. Aracı hızla sınamak veya kavram kanıtı için, birkaç saatlik veya birkaç dakikalık profil oluşturabilirsiniz. İzin verilen en kısa profil oluşturma süresi 30 dakikadır. 

Profil oluşturma sırasında, Azure Site Recovery’nin çoğaltma sırasında Hyper-V sunucusundan Azure’a elde edilebileceği aktarım hızını bulmak için, isteğe bağlı olarak bir depolama hesabı adı ve anahtarı geçirebilirsiniz. Profil oluşturma sırasında depolama hesabı adı ve anahtarı geçirilmezse, araç ulaşılabilir aktarım hızını hesaplamaz.

Çeşitli sanal makine kümeleri için aracın birden çok örneğini çalıştırabilirsiniz. Sanal makine adlarının, profil kümelerinin hiçbirinde yinelenmediğinden emin olun. Örneğin, 10 sanal makine (VM1 - VM10) profili oluşturdunuz ve birkaç gün sonra beş sanal makine (VM11 - VM15) profili daha oluşturmak istiyorsunuz; bu durumda, ikinci sanal makine kümesi (VM11 - VM15) için başka bir komut satırı konsolundan aracı çalıştırabilirsiniz. Ancak, ikinci sanal makine kümesinde birinci profil oluşturma örneğinden herhangi bir sanal makine adı olmadığından veya ikinci çalıştırma için farklı bir çıktı dizini kullandığınızdan emin olun. Aracın iki örneği aynı sanal makinelerin profilini oluşturmak için kullanılır ve aynı çıktı dizinini kullanırsa, oluşturulan rapor hatalı olacaktır. 

Varsayılan olarak araç, 1000 VM'ye kadar profil ve rapor oluşturmak üzere yapılandırılmıştır. *ASRDeploymentPlanner.exe.config* dosyasındaki MaxVMsSupported anahtar değerini değiştirerek sınırı değiştirebilirsiniz.
```
<!-- Maximum number of vms supported-->
<add key="MaxVmsSupported" value="1000"/>
```
Varsayılan ayarlarla, örneğin 1500 VM profili oluşturmak için iki VMList.txt dosyası oluşturun. Biri 1000 VM, diğeri 500 VM ile listelenir. ASR Dağıtım Planlayıcısı’nın iki örneğinden birini VMList1.txt, diğerini VMList2.txt ile çalıştırın. Her iki VMList VM’lerin profil verilerini depolamak için aynı dizin yolunu kullanabilirsiniz. 

Başta aracın raporu oluşturmak üzere çalıştırıldığı sunucunun RAM boyutu olmak üzere donanım yapılandırmasına göre, işlem yetersiz bellek nedeniyle başarısız olabilir. Donanımınız iyiyse, MaxVMsSupported değerini daha yüksek bir değerle değiştirebilirsiniz.  

Sanal makine yapılandırması, profil oluşturma işleminin başında bir kez yakalanır ve VMDetailList.xml adlı bir dosyada depolanır. Rapor oluşturulduğunda bu bilgiler kullanılır. Profil oluşturmanın başlangıcı ile bitişi arasında VM yapılandırmasında meydana gelen hiçbir değişiklik (örneğin, çekirdek, disk veya ağ arabirimi sayısının artması) yakalanmaz. Profili oluşturulmuş bir VM yapılandırması profil oluşturma sırasında değiştiyse, rapor oluştururken en son VM bilgilerini almaya yönelik geçici çözüm aşağıda verilmiştir:

* VMdetailList.xml dosyasını yedekleyip, dosyayı geçerli konumundan silin.
* -User ve -Password bağımsız değişkenlerini rapor oluşturma sırasında geçirme

Profil oluşturma komutu, profil oluşturma dizininde birkaç dosya oluşturur. Rapor oluşturmayı etkileyeceği için hiçbir dosyayı silmeyin.

### <a name="examples"></a>Örnekler
#### <a name="example-1-profile-vms-for-30-days-and-find-the-throughput-from-on-premises-to-azure"></a>Örnek 1: 30 günlük sanal makine profili oluşturma ve şirket içinden Azure’a aktarım hızını bulma
```
ASRDeploymentPlanner.exe -Operation StartProfiling -virtualization Hyper-V -Directory “E:\Hyper-V_ProfiledData” -VMListFile “E:\Hyper-V_ProfiledData\ProfileVMList1.txt”  -NoOfDaysToProfile 30 -User Contoso\HyperVUser1 -StorageAccountName  asrspfarm1 -StorageAccountKey Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==
```

#### <a name="example-2-profile-vms-for-15-days"></a>Örnek 2: 15 günlük sanal makine profili oluşturma
```
ASRDeploymentPlanner.exe -Operation StartProfiling -Virtualization Hyper-V -Directory “E:\Hyper-V_ProfiledData” -VMListFile “E:\vCenter1_ProfiledData\ProfileVMList1.txt”  -NoOfDaysToProfile  15  -User contoso\HypreVUser1
```

#### <a name="example-3-profile-vms-for-60-minutes-for-a-quick-test-of-the-tool"></a>Örnek 3: Aracın hızlı bir testine yönelik 60 dakikalık VM profili oluşturma
```
ASRDeploymentPlanner.exe -Operation StartProfiling -Virtualization Hyper-V -Directory “E:\Hyper-V_ProfiledData” -VMListFile “E:\Hyper-V_ProfiledData\ProfileVMList1.txt”  -NoOfMinutesToProfile 60 -User Contoso\HyperVUser1
```

#### <a name="example-4-profile-vms-for-2-hours-for-a-proof-of-concept"></a>Örnek 4: Kavram kanıtı için VM’lerin 2 saatlik profilini oluşturma
```
ASRDeploymentPlanner.exe -Operation StartProfiling -Virtualization Hyper-V -Directory “E:\Hyper-V_ProfiledData” -VMListFile “E:\Hyper-V_ProfiledData\ProfileVMList1.txt”  -NoOfHoursToProfile 2 -User Contoso\HyperVUser1
```

>[NOT!]
>
* Aracın çalıştığı sunucu yeniden başlatılırsa veya kilitlenmişse ya da Ctrl+C tuşlarını kullanarak aracı kapatırsanız, profili oluşturulan veriler korunur. Ancak, profili oluşturulan verilen son 15 dakikası kaybedilebilir. Böyle durumlarda, sunucu yeniden başlatıldıktan sonra aracı profil oluşturma modunda yeniden çalıştırmanız gerekir.
* Depolama hesabı adı ve anahtarı geçirildiğinde, araç profil oluşturma işleminin son adımında aktarım hızını ölçer. Profil oluşturma tamamlanmadan önce araç kapatılırsa, aktarım hızı hesaplanmaz. Raporu oluşturmadan önce aktarım hızını bulmak için, komut satırı konsolundan GetThroughput işlemini çalıştırabilirsiniz. Aksi takdirde, oluşturulan rapor aktarım hızı bilgilerini içermez.
* Azure Site Recovery, iSCSI ve geçiş disklerine sahip olan VM’leri desteklemez. Ancak araç, VM’lere eklenmiş iSCSI ve geçiş disklerini algılayamaz ve bunların profilini oluşturamaz.

## <a name="generate-report"></a>Rapor oluşturma
Araç, rapor çıktısı olarak tüm dağıtım önerilerini özetleyen makro özellikli bir Microsoft Excel dosyası (XLSM dosyası) oluşturur. Rapor, DeploymentPlannerReport_<unique numeric identifier>.xlsm olarak adlandırılıp belirtilen dizine yerleştirilir.
Profil oluşturma tamamlandıktan sonra, aracı rapor oluşturma modunda çalıştırabilirsiniz. 

### <a name="command-line-parameters"></a>Komut satırı parametreleri
Aşağıdaki tabloda, rapor oluşturma modunda çalışmaya yönelik zorunlu ve isteğe bağlı parametreler listelenmiştir. Bu araç, hem VMware’den Azure’a hem de Hyper-V’den Azure’a senaryoları için sıkça kullanılır. Aşağıdaki parametreler Hyper-V’de kullanılabilir.
```
ASRDeploymentPlanner.exe -Operation GenerateReport /?
```
| Parametre adı | Açıklama |
|---|---|
| -Operation | GenerateReport |
|-VMListFile | Raporun oluşturulacağı profili oluşturulmuş sanal makinelerin listesini içeren dosya. Dosya yolu mutlak veya göreli olabilir. Hyper-V için, GetVMList işleminin çıkış dosyası bu dosyadır. El ile hazırlıyorsanız, dosyada bir sunucu adı ya da IP adresi ve ardından her satırı \ ile ayrılan VM adı bulunmalıdır. Dosyada belirtilen VM adı, Hyper-V konağındaki VM adıyla aynı olmalıdır.<ul>Örnek: "VMList.txt" dosyası aşağıdaki sanal makineleri içerir:<ul><li>Host_1\VM_A</li><li>10.8.59.27\VM_B</li><li>Host_2\VM_C</li><ul>|
|-Sanallaştırma|Sanallaştırma türünü (VMware veya Hyper-V) belirtin.|
|-Directory|(İsteğe bağlı) Profili oluşturulan verilerin (profil oluşturma sırasında oluşturulan dosyalar) depolandığı evrensel adlandırma kuralı (UNC) veya yerel dizin yolu. Bu veriler, rapor oluşturmak için gereklidir. Bir ad belirtilmemişse, geçerli yolun altındaki “ProfiledData” adlı dizin varsayılan dizin olarak kullanılır.|
| -User | (İsteğe bağlı) Hyper-V konağı veya Hyper-V kümesine bağlanmak için kullanıcı adı. Kullanıcının yönetici erişimi olmalıdır.<br>Kullanıcı ve parola, VM’lerin disk sayısı, çekirdek sayısı, NIC sayısı gibi raporda kullanılacak en son yapılandırma bilgilerini getirmek için kullanılır. Bu seçenek belirtilmezse, profil oluşturma sırasında toplanan yapılandırma bilgileri kullanılır.|
|-Password|(İsteğe bağlı) Hyper-V konağına bağlanmak için parola. Şu anda belirtilmezse, daha sonra komut yürütülürken sorulacaktır.|
| -DesiredRPO | (İsteğe bağlı) Dakika cinsinden istenen kurtarma noktası hedefi. Varsayılan değer 15 dakikadır.|
| -Bandwidth | (İsteğe bağlı) MB/sn cinsinden bant genişliği. Belirtilen bant genişliği için ulaşılabilecek RPO’yu hesaplamak için kullanılan parametre. |
| -StartDate | (İsteğe bağlı) AA-GG-YYYY:SS:DD (24 saat biçiminde) cinsinden başlangıç tarihi ve saati. *StartDate* değeri *EndDate* ile birlikte belirtilmelidir. StartDate belirtildiğinde, StartDate ile EndDate arasında toplanan profili oluşturulmuş veriler için rapor oluşturulur. |
| -EndDate | (İsteğe bağlı) AA-GG-YYYY:SS:DD (24 saat biçiminde) cinsinden bitiş tarihi ve saati. *EndDate* değeri *StartDate* ile birlikte belirtilmelidir. EndDate belirtildiğinde, StartDate ile EndDate arasında toplanan profili oluşturulmuş veriler için rapor oluşturulur. |
| -GrowthFactor | (İsteğe bağlı) Yüzde olarak ifade edilen büyüme faktörü. Varsayılan değer yüzde 30'dur. |
| -UseManagedDisks | (Optional) UseManagedDisks - Evet/Hayır. Varsayılan değer Evet’tir. Tek bir depolama hesabında bulunabilecek sanal makine sayısı, sanal makinelerin Yük devretme işleminin/Yük devretme testinin yönetilmeyen disk yerine yönetilen disk üzerinde yapılıp yapılmadığına bağlı olarak hesaplanır. |
|-SubscriptionId |(İsteğe bağlı) Abonelik GUID’si. Aboneliğinizin son fiyatına, aboneliğinizle ilişkili teklife ve belirli hedef Azure bölgenize dayalı olarak ve belirli bir para birimi kullanılarak maliyet tahmini raporunu oluşturmak için bu parametreyi kullanın.|
|-TargetRegion|(İsteğe bağlı) Çoğaltmanın hedeflendiği Azure bölgesi. Azure maliyetleri bölgelere göre değiştiğinden, belirli bir Azure bölgesini hedef alan bir rapor oluşturmak için bu parametreyi kullanın.<br>Varsayılan olarak WestUS2 veya en son kullanılan hedef bölge kullanılır.<br>[Desteklenen hedef bölgeler](site-recovery-hyper-v-deployment-planner-cost-estimation.md#supported-target-regions) listesine başvurun.|
|-OfferId|(İsteğe bağlı) Belirtilen abonelikle ilişkili teklif. Varsayılan olarak MS-AZR-0003P (Kullandıkça Öde) kullanılır.|
|-Currency|(İsteğe bağlı) Oluşturulan raporda maliyetin gösterileceği para birimi. Varsayılan olarak ABD doları ($) veya en son kullanılan para birimi kullanılır.<br>[Desteklenen para birimleri](site-recovery-hyper-v-deployment-planner-cost-estimation.md#supported-currencies) listesine başvurun.|

Varsayılan olarak araç, 1000 VM'ye kadar profil ve rapor oluşturmak üzere yapılandırılmıştır. *ASRDeploymentPlanner.exe.config* dosyasındaki MaxVMsSupported anahtar değerini değiştirerek sınırı değiştirebilirsiniz.
```
<!-- Maximum number of vms supported-->
<add key="MaxVmsSupported" value="1000"/>
```

### <a name="examples"></a>Örnekler
#### <a name="example-1-generate-a-report-with-default-values-when-the-profiled-data-is-on-the-local-drive"></a>Örnek 1: Profili oluşturulan veriler yerel sürücüde olduğunda raporu varsayılan değerlerle oluşturma
```
ASRDeploymentPlanner.exe -Operation GenerateReport -virtualization Hyper-V -Directory “E:\Hyper-V_ProfiledData” -VMListFile “E:\Hyper-V_ProfiledData\ProfileVMList1.txt”
```

#### <a name="example-2-generate-a-report-when-the-profiled-data-is-on-a-remote-server"></a>Örnek 2: Profili oluşturulan veriler uzak bir sunucuda olduğunda rapor oluşturma
Uzak dizin üzerinde okuma/yazma erişiminiz olmalıdır.
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Virtualization Hyper-V - -Directory “\\PS1-W2K12R2\Hyper-V_ProfiledData” -VMListFile “\\PS1-W2K12R2\vCenter1_ProfiledData\ProfileVMList1.txt”
```

#### <a name="example-3-generate-a-report-with-a-specific-bandwidth-that-you-will-be-provision-for-the-replication"></a>Örnek 3: Çoğaltma için sağlayacağınız belirli bir bant genişliğine sahip bir rapor oluşturma
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Virtualization Hyper-V -Directory “E:\Hyper-V_ProfiledData” -VMListFile “E:\Hyper-V_ProfiledData\ProfileVMList1.txt” -Bandwidth 100
```

#### <a name="example-4-generate-a-report-with-a-5-percent-growth-factor-instead-of-the-default-30-percent"></a>Örnek 4: Yüzde 30’luk varsayılan değer yerine yüzde 5 büyüme faktörü ile rapor oluşturma 
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Virtualization Hyper-V -Directory “E:\Hyper-V_ProfiledData” -VMListFile “E:\Hyper-V_ProfiledData\ProfileVMList1.txt” -GrowthFactor 5
```

#### <a name="example-5-generate-a-report-with-a-subset-of-profiled-data"></a>Örnek 5: Profili oluşturulan verilerin bir alt kümesi ile rapor oluşturma
Örneğin, 30 günlük profili oluşturulmuş verilerinizin olduğunu ve raporu yalnızca 20 gün için oluşturduğunuzu varsayalım.
```
ASRDeploymentPlanner.exe -Operation GenerateReport -virtualization Hyper-V -Directory “E:\Hyper-V_ProfiledData” -VMListFile “E:\Hyper-V_ProfiledData\ProfileVMList1.txt” -StartDate  01-10-2017:12:30 -EndDate 01-19-2017:12:30
```

#### <a name="example-6-generate-a-report-for-5-minutes-rpo"></a>Örnek 6: 5 dakikalık RPO için rapor oluşturma
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Virtualization Hyper-V -Directory “E:\Hyper-V_ProfiledData” -VMListFile “E:\Hyper-V_ProfiledData\ProfileVMList1.txt”  -DesiredRPO 5
```

#### <a name="example-7-generate-a-report-for-south-india-azure-region-with-indian-rupee-and-specific-offer-id"></a>Örnek 7: Güney Hindistan Azure bölgesi için Hindistan Rupisi ve belirli teklif kimliğini içeren rapor oluşturma
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Virtualization Hyper-V -Directory “E:\Hyper-V_ProfiledData” -VMListFile “E:\Hyper-V_ProfiledData\ProfileVMList1.txt”  -SubscriptionID 4d19f16b-3e00-4b89-a2ba-8645edf42fe5 -OfferID MS-AZR-0148P -TargetRegion southindia -Currency INR
```


## <a name="percentile-value-used-for-the-calculation"></a>Hesaplama için kullanılan yüzdelik değer
**Rapor oluşturulurken, profil oluşturma sırasında toplanan performans ölçümlerinin hangi varsayılan yüzdelik dilim değeri araç tarafından kullanılır?**

Aracın, tüm sanal makinelerin profili oluşturulurken toplanan okuma/yazma IOPS, yazma IOPS ve veri değişim sıklığı için varsayılan değeri yüzde 95’lik dilimdir. Bu ölçüm, VM’lerinizin geçici olaylar nedeniyle görebileceği %100’lük dilim artışının, hedef depolama hesabı ve kaynak bant genişliği gereksinimlerini belirlemek için kullanılmamasını sağlar. Örneğin, geçici olay günde bir kez gerçekleştirilen bir yedekleme işi, düzenli aralıklarla yapılan veritabanı dizini oluşturma veya analiz raporu oluşturma etkinliği ya da kısa süreli diğer benzer olaylar olabilir.

Yüzde 95’lik dilim değeri, gerçek iş yükü özelliklerinin gerçek bir resmini verir ve bu iş yükleri Azure üzerinde çalışırken en iyi performansı sağlar. Bu sayıyı değiştirmenizin gerekli olacağı düşünülmemektedir. Bu sayıyı değiştirirseniz (örn. yüzde 90’lık dilime), bu ASRDeploymentPlanner.exe.config yapılandırma dosyasını varsayılan klasörde güncelleştirebilir ve kaydederek var olan profili oluşturulmuş verilere ilişkin yeni bir rapor oluşturabilirsiniz.
```
<add key="WriteIOPSPercentile" value="95" />      
<add key="ReadWriteIOPSPercentile" value="95" />      
<add key="DataChurnPercentile" value="95" />
```

## <a name="growth-factor-considerations"></a>Büyüme faktörü konuları
**Dağıtımları planlarken neden büyüme faktörünü göz önünde bulundurmalıyım?**
Zaman içindeki kullanımın olası artışı varsayılarak, iş yükü özelliklerinizde büyümenin hesaba katılması önemlidir. Koruma uygulandıktan sonra iş yükü özellikleriniz değişirse, korumayı devre dışı bırakıp yeniden etkinleştirmeden farklı bir depolama hesabına geçiş yapamazsınız.

Örneğin, bugün sanal makinenizin standart bir depolama çoğaltma hesabına uygun olduğunu düşünelim. Önümüzdeki üç ay üzerinde bazı değişiklikler oluşabilir:

* VM üzerinde çalışan uygulamanın kullanıcı sayısı artar.
* VM üzerinde artan değişim hızı, Azure Site Recovery çoğaltmasının gerçekleştirilebilmesi için VM’nizin premium depolamaya geçmesini gerektirir.
* Sonuç olarak, bir premium depolama hesabında korumayı devre dışı bırakıp yeniden etkinleştirmeniz gerekir.

Dağıtım planlaması sırasında ve varsayılan değer yüzde 30 iken büyüme planlaması yapmanız önemle önerilir. Uygulama kullanma düzeniniz ve büyüme projeksiyonları konusunda en sağlıklı bilgilere sahip olan sizsiniz ve rapor oluştururken bu sayıyı duruma uygun biçimde değiştirebilirsiniz. Ayrıca, profili oluşturulmuş aynı verilerle farklı büyüme faktörleri içeren birden fazla rapor oluşturabilir ve en çok işinize yarayacak hedef depolama ile kaynak bant genişliği önerilerini belirleyebilirsiniz. 

Oluşturulan Microsoft Excel raporu aşağıdaki bilgileri içerir:

* [Şirket İçi Özeti](site-recovery-hyper-v-deployment-planner-analyze-report.md#on-premises-summary)
* [Öneriler](site-recovery-hyper-v-deployment-planner-analyze-report.md#recommendations)
* [VM<->Depolama Yerleşimi](site-recovery-hyper-v-deployment-planner-analyze-report.md#vm-storage-placement-recommendation)]
* [Uyumlu VM’ler](site-recovery-hyper-v-deployment-planner-analyze-report.md#compatible-vms)
* [Uyumsuz VM’ler](site-recovery-hyper-v-deployment-planner-analyze-report.md#incompatible-vms)
* [Şirket İçi Depolama Gereksinimleri](site-recovery-hyper-v-deployment-planner-analyze-report.md#on-premises-storage-requirement)
* [IR İşlem Grubu Oluşturma](site-recovery-hyper-v-deployment-planner-analyze-report.md#initial-replication-batching)
* [Maliyet Tahmini](site-recovery-hyper-v-deployment-planner-cost-estimation.md)

![Dağıtım planlayıcısı raporu](media/site-recovery-hyper-v-deployment-planner-run/deployment-planner-report-h2a.png)


## <a name="get-throughput"></a>Aktarım hızı alma
Azure Site Recovery’nin çoğaltma sırasında şirket içinden Azure’a elde edebildiği aktarım hızını tahmin etmek için aracı GetThroughput modunda çalıştırın. Araç, üzerinde çalıştığı sunucudan aktarım hızını hesaplar. İdeal olarak bu sunucu, korunacak olan VM'lerin bulunduğu Hyper-V sunucusudur. 

### <a name="command-line-parameters"></a>Komut satırı parametreleri 
Bir komut satırı konsolu açın ve Azure Site Recovery dağıtım planlama aracının klasörüne gidin. ASRDeploymentPlanner.exe dosyasını aşağıdaki parametrelerle çalıştırın.
```
ASRDeploymentPlanner.exe -Operation GetThroughput /?
```
 Parametre adı | Açıklama |
|---|---|
| -Operation | GetThroughtput |
|-Sanallaştırma|Sanallaştırma türünü (VMware veya Hyper-V) belirtin.|
|-Directory|(İsteğe bağlı) Profili oluşturulan verilerin (profil oluşturma sırasında oluşturulan dosyalar) depolandığı evrensel adlandırma kuralı (UNC) veya yerel dizin yolu. Bu veriler, rapor oluşturmak için gereklidir. Bir ad belirtilmemişse, geçerli yolun altındaki “ProfiledData” adlı dizin varsayılan dizin olarak kullanılır.|
| -StorageAccountName | Şirket içinden Azure’a veri çoğaltma için kullanılan bant genişliğini bulmak için depolama hesabı adı. Araç, kullanılan bant genişliğini bulmak için test verilerini bu depolama hesabına yükler. |
| -StorageAccountKey | Depolama hesabına erişmek için kullanılan depolama hesabı anahtarı. Azure portalı > Depolama hesapları > <*Depolama hesabı adı*> > Ayarlar > Erişim Anahtarları > Anahtar1 adımlarını izleyin.|
| -VMListFile | Kullanılan bant genişliğini hesaplamak için profili oluşturulacak sanal makinelerin listesini içeren dosya. Dosya yolu mutlak veya göreli olabilir. Hyper-V için, GetVMList işleminin çıkış dosyası bu dosyadır. El ile hazırlıyorsanız, dosyada bir sunucu adı ya da IP adresi ve ardından her satırı \ ile ayrılan VM adı bulunmalıdır. Dosyada belirtilen VM adı, Hyper-V konağındaki VM adıyla aynı olmalıdır.<ul>Örnek: "VMList.txt" dosyası aşağıdaki sanal makineleri içerir:<ul><li>Host_1\VM_A</li><li>10.8.59.27\VM_B</li><li>Host_2\VM_C</li><ul>|
|-Ortam|(İsteğe bağlı) Bu, hedef Azure Depolama hesabı ortamınızdır. Şu üç değerden herhangi birini alabilir: AzureCloud, AzureUSGovernment, AzureChinaCloud. Varsayılan seçenek AzureCloud değeridir. Hedef Azure bölgeniz Azure US Government veya Azure China bulutları olduğunda ilgili parametreyi kullanın|


Araç, belirtilen dizinde 64 MB’lık birkaç asrvhdfile<#>.vhd (“#” sayıdır) dosyası oluşturur. Araç, aktarım hızını bulmak için dosyaları depolama hesabına yükler. Aktarım hızı ölçüldükten sonra araç tüm dosyaları depolama hesabından ve yerel sunucudan siler. Araç aktarım hızını hesaplarken herhangi bir nedenle sonlandırılırsa, dosyaları depolama alanından veya yerel sunucudan silmez. Bunları el ile silmeniz gerekir.

Aktarım hızı belirli bir zaman noktasında ölçülür ve diğer tüm faktörlerin aynı kalması koşuluyla Azure Site Recovery’nin çoğaltma sırasında ulaşabileceği en yüksek aktarım hızıdır. Örneğin, herhangi bir uygulama aynı ağ üzerinde daha fazla bant genişliği tüketmeye başlarsa, çoğaltma sırasında gerçek aktarım hızı farklılık gösterir. Korunan VM’ler yüksek veri değişim sıklığına sahip olduğunda GetThroughput işlemi çalıştırılırsa, ölçülen aktarım hızının sonucu farklı olur. Çeşitli zamanlarda hangi aktarım hızı düzeylerine ulaşılabileceğini anlamak için, profil oluşturma sırasında farklı zaman noktalarında aracın çalıştırılması önerilir. Raporda, araç son ölçülen aktarım hızını gösterir.
    
### <a name="example"></a>Örnek
```
ASRDeploymentPlanner.exe -Operation GetThroughput -Virtualization Hyper-V -Directory E:\Hyp-erV_ProfiledData -VMListFile E:\Hyper-V_ProfiledData\ProfileVMList1.txt  -StorageAccountName  asrspfarm1 -StorageAccountKey by8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==
```

[NOT!]
>
> Aracı, Hyper-V sunucusu ile aynı depolama ve CPU özelliklerine sahip bir sunucuda çalıştırın.
>
> Çoğaltma için, önerilen bant genişliğini sürenin yüzde 100’ünü karşılayacak şekilde ayarlayın. Doğru bant genişliğini sağladıktan sonra araç tarafından ulaşıldığı bildirilen aktarım hızında herhangi bir artış görmezseniz aşağıdakileri yapın:
>
>  1. Azure Site Recovery aktarım hızını sınırlayan herhangi bir ağ Hizmet Kalitesi (QoS) olup olmadığını denetleyin.
>
>  2. Ağ gecikme süresini en aza indirmek için, Azure Site Recovery kasanızın desteklenen en yakın fiziksel Microsoft Azure bölgesinde olup olmadığını denetleyin.
>
>  3. Donanımı iyileştirip iyileştiremeyeceğinizi (örneğin, HDD’den SSD’ye) belirlemek için yerel depolama özelliklerini denetleyin.
>

## <a name="next-steps"></a>Sonraki adımlar
* [Oluşturulan raporu analiz etme](site-recovery-hyper-v-deployment-planner-analyze-report.md).