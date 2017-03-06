---
title: "VMware’den Azure’a Azure Site Recovery Dağıtım Planlayıcısı| Microsoft Belgeleri"
description: "Bu belge Azure Site Recovery Dağıtım Planlayıcısı kullanıcı kılavuzudur."
services: site-recovery
documentationcenter: 
author: nsoneji
manager: garavd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 2/21/2017
ms.author: nisoneji
translationtype: Human Translation
ms.sourcegitcommit: f438db1d0129dfb0e2eaa00146147084cd8c11b6
ms.openlocfilehash: 179e8cf928d83a3a66ed3489173e4c28a2bc9d4f
ms.lasthandoff: 02/28/2017


---
#<a name="azure-site-recovery-deployment-planner"></a>Azure Site Recovery Dağıtım Planlayıcısı
Bu belge, VMware’den Azure’a üretim dağıtımları için Azure Site Recovery Dağıtım Planlayıcısı kullanım kılavuzudur.


##<a name="overview"></a>Genel Bakış

Azure Site Recovery kullanarak herhangi bir VMware sanal makinesini korumadan önce, istenen RPO’yu karşılayacak günlük veri değişikliği hızınıza göre yeterli bant genişliğini ayırmanız gerekir. Şirket içinde doğru sayıda Yapılandırma Sunucusu ve İşlem Sunucusu dağıtmanız gerekir. Ayrıca, zaman içinde artan kullanım nedeniyle kaynak üretim sunucularınızdaki büyümeyi hesaba katacak şekilde doğru tür ve sayıda hedef Azure Depolama hesabı (standart veya premium) oluşturmanız gerekir. Bir sanal makine için depolama türüne, iş yükü özellikleri (R/W IOPS, veri değişim sıklığı) ve Azure Site Recovery limitleri temel alınarak karar verilir.  

Azure Site Recovery Dağıtım Planlayıcısı Genel Önizlemesi, şu anda yalnızca VMware’den Azure’a senaryoları için kullanılabilen bir komut satırı aracıdır. Başarılı çoğaltma ve Yük Devretme Testine yönelik bant genişliği ve Azure depolama gereksinimlerini anlamak için, bu aracı kullanarak VMware sanal makinelerinizin profilini uzaktan oluşturabilirsiniz (herhangi bir üretim etkisi olmadan).  Şirket içinde herhangi bir Azure Site Recovery bileşeni yüklemeden aracı çalıştırabilirsiniz, ancak aldığınız aktarım hızı sonuçlarının doğru olması için Planlayıcıyı, üretim dağıtımının ilk adımlarında dağıtmanız gereken Azure Site Recovery Configuration Server’a yönelik en düşük gereksinimleri karşılayan bir Windows Server üzerinde çalıştırmanız önerilir.

Araç aşağıdaki bilgileri sağlar:

**Uyumluluk değerlendirmesi**<br>
* Disk sayısı, disk boyutu, IOPS ve değişim sıklığına göre sanal makine uygunluk değerlendirmesi

**Ağ bant genişliği gereksinimine karşılık RPO değerlendirmesi**<br>
* Delta çoğaltma için gereken tahmini ağ bant genişliği<br>
* Azure Site Recovery’nin şirket içinden Azure’a alabileceği aktarım hızı<br>
* Belirli bir süre içinde ilk çoğaltmayı tamamlamak için tahmin edilen bant genişliğine göre toplu hale getirilecek sanal makine sayısı<br>

**Microsoft Azure altyapı gereksinimleri**<br>
* Her sanal makine için depolama türü (standart veya premium depolama) gereksinimi<br>
* Çoğaltma için sağlanacak toplam standart ve premium depolama hesabı sayısı<br>
* Azure Depolama kılavuzuna göre depolama hesabı adlandırma önerileri<br>
* Her sanal makinenin depolama hesabı yerleşimi<br>
* Abonelik üzerinde yük devretme testi/yük devretme öncesinde sağlanacak Microsoft Azure çekirdek sayısı<br>
* Her bir şirket içi sanal makine için önerilen Microsoft Azure sanal makine boyutu<br>

**Şirket içi altyapı gereksinimleri**<br>
* Şirket içinde dağıtılması gereken Yapılandırma Sunucusu ve İşlem Sunucusu sayısı<br>

>[!IMPORTANT]
>
>Araçtaki tüm bu hesaplamalar, zaman içinde artan kullanım nedeniyle iş yükü özelliklerinizde %30’luk büyüme faktörü varsayılarak ve tüm profil oluşturma ölçümlerinin (R/W IOPS, değişim sıklığı, vb.) yüzde 95’lik dilimi alınarak yapılır. Büyüme faktörü ve yüzdelik dilim hesaplaması parametrelerinin her ikisi de yapılandırılabilir özelliktedir. Hesaplama için kullanılan [büyüme faktörü](site-recovery-deployment-planner.md#growth-factor) ve [yüzdelik dilim değeri hakkında daha fazla bilgi alın](site-recovery-deployment-planner.md#percentile-value-used-for-the-calculation).
>


## <a name="requirements"></a>Gereksinimler
Araçta başlıca iki aşama vardır: profil oluşturma ve rapor oluşturma. Yalnızca aktarım hızını hesaplamaya yönelik üçüncü bir seçenek de mevcuttur. Profil oluşturma / aktarım hızı ölçümünün başlatıldığı sunucuya yönelik gereksinimler aşağıda verilmiştir.

| Gereksinim | Açıklama|
|---|---|
|Profil oluşturma ve aktarım hızı ölçümü| <br>İşletim Sistemi: Microsoft Windows Server 2012 R2 <br>En azından aşağıdaki Yapılandırma Sunucusu [boyutu](https://aka.ms/asr-v2a-on-prem-components) ile eşleşmesi idealdir<br>Makine Yapılandırması: 8 vCPus, 16 GB RAM, 300 GB HDD<br [Microsoft .NET Framework 4.5](https://aka.ms/dotnet-framework-45)<br>[VMware vSphere PowerCLI 6.0 R3](https://developercenter.vmware.com/tool/vsphere_powercli/6.0)<br>[Visual Studio 2012 için Microsoft Visual C++ Yeniden Dağıtılabilir](https://aka.ms/vcplusplus-redistributable)<br> Bu sunucudan Microsoft Azure’a İnternet erişimi<br> Microsoft Azure depolama hesabı<Br>Sunucu üzerinde yönetici erişimi<br>100 GB en düşük boş disk alanı (30 gün için her birinin profilinin oluşturulduğu varsayılan ortalama 3 diske sahip 1000 sanal makine varsayıldığında)|
| Rapor Oluşturma| Microsoft Excel 2013 ve üzerinin yüklü olduğu herhangi bir Windows PC/Windows Server |
| Kullanıcı İzinleri | Profil oluşturma sırasında VMware vCenter/vSphere sunucusuna erişmek için kullanılan kullanıcı hesabına yönelik salt okunur izinler|


> [!NOTE]
>
> Araç, sanal makinelerin profilini yalnızca VMDK ve RDM diskleri ile oluşturabilir. Sanal makine profilini iSCSI veya NFS diskleri ile oluşturamaz. Azure Site Recovery, VMware sunucuları için iSCSI ve NFS disklerini desteklese de, dağıtım planlayıcısının konuk içinde bulunmaması ve vCenter performans sayaçlarını kullanarak profil oluşturmaması nedeniyle araç bu disk türlerini görüntüleyemez.
>


##<a name="download"></a>İndirme
Azure Site Recovery Dağıtım Planlayıcısı Genel Önizlemesinin en son sürümünü [indirin](https://aka.ms/asr-deployment-planner).  Araç zip biçiminde paketlenmiştir.  Aracın geçerli sürümü yalnızca VMware’den Azure’a senaryosunu destekler.

Zip dosyasını, aracı çalıştırmak istediğiniz Windows Server’a kopyalayın. Aracı, profili oluşturulacak sanal makineleri tutan VMware vCenter sunucusuna veya VMware vSphere ESXi konağına bağlanmak için ağ erişimi olan herhangi bir Windows Server 2012 R2’den çalıştırabilmenize rağmen, donanım yapılandırması [Yapılandırma Sunucusu boyutlandırma yönergelerine](https://aka.ms/asr-v2a-on-prem-components) uygun olarak yapılmış bir sunucuda aracın çalıştırılması önerilir.  Azure Site Recovery bileşenlerini şirket içinde zaten dağıttıysanız, aracı Yapılandırma Sunucusu’ndan çalıştırmanız gerekir. Aracı çalıştırdığınız sunucudaki donanım yapılandırmasının Yapılandırma Sunucusu (yerleşik bir İşlem Sunucusuna sahiptir) ile aynı olması, aracın bildirdiği elde edilen aktarım hızının, çoğaltma sırasında Azure Site Recovery tarafından elde edilebilen gerçek aktarım hızı ile eşleşmesi açısından önerilir; aktarım hızı hesaplaması, sunucu üzerindeki kullanılabilir ağ bant genişliğine ve sunucunun donanım yapılandırmasına (CPU, depolama, vb.) bağlıdır. Aracı başka bir sunucudan çalıştırırsanız, o sunucudan Microsoft Azure’a aktarım hızı hesaplanır, ayrıca sunucunun donanım yapılandırması Yapılandırma Sunucusundan farklı olabilir ve aracın bildirdiği elde edilen aktarım hızı doğru olmaz.

Zip klasörünü ayıklayın. Birden fazla dosya ve alt klasör görebilirsiniz. Yürütülebilir dosya, üst klasördeki ASRDeploymentPlanner.exe dosyasıdır.

Örnek: .zip dosyasını E:\ sürücüsüne kopyalayıp ayıklayın.
E:\ASR Deployment Planner-Preview_v1.0.zip

E:\ASR Deployment Planner-Preview_v1.0\ ASR Deployment Planner-Preview_v1.0\ ASRDeploymentPlanner.exe

##<a name="capabilities"></a>Özellikler
Komut satırı aracı (ASRDeploymentPlanner.exe) aşağıdaki üç modun herhangi birinde çalıştırılabilir:

1.    Profil oluşturma  
2.    Rapor oluşturma
3.    Aktarım hızı alma

İlk olarak aracı profil oluşturma modunda çalıştırarak sanal makine veri değişim sıklığı ve IOPS bilgilerini toplamanız gerekir.  Ardından, ağ bant genişliği ve depolama gereksinimlerini bulmak üzere raporu oluşturmak için aracı çalıştırın.

##<a name="profiling"></a>Profil oluşturma
Profil oluşturma modunda Dağıtım Planlayıcısı aracı, sanal makineye ilişkin performans verilerini toplamak için vCenter Server veya vSphere ESXi konaklarına bağlanır.

* Profil oluşturma, üretim sanal makineleri ile doğrudan bir bağlantı kurulmadığı için üretim sanal makinelerinin performansını etkilemez. Tüm performans verileri vCenter Server/ vSphere ESXi konağından toplanır.
* Profil oluşturma nedeniyle sunucu üzerindeki etkinin önemsiz olduğundan emin olmak için vCenter sunucusu/vSphere EXSi konağı 15 dakikada bir sorgulanır. Ancak, araç her dakikaya ait performans sayacı verilerini depoladığı için bunun yapılması profil oluşturma doğruluğunu tehlikeye atmaz.

####<a name="create-a-list-of-virtual-machines-to-profile"></a>Profili oluşturulacak sanal makinelerin listesini oluşturma
İlk olarak, profilini oluşturmak istediğiniz sanal makinelerin bir listesine sahip olmanız gerekir. Aşağıdaki VMware vSphere PowerCLI komutlarını kullanarak bir VMware vCenter veya VMware vSphere ESXi konağındaki tüm sanal makinelerin adlarını alabilirsiniz. Alternatif olarak, bir dosyada profilini oluşturmak istediğiniz sanal makinelerin sadece kolay adlarını / IP adreslerini listeleyebilirsiniz.

1.    VMware vSphere PowerCLI’nin yüklü olduğu sanal makinede oturum açın
2.    VMware vSphere PowerCLI konsolunu açın
3.    Betik için yürütme ilkesinin devre dışı bırakılmadığından emin olun. Devre dışı bırakılmışsa, VMware vSphere PowerCLI konsolunu yönetici modunda başlatın ve aşağıdaki komutu çalıştırarak etkinleştirin:

            Set-ExecutionPolicy –ExecutionPolicy AllSigned

4.    Bir VMware vCenter veya VMware vSphere ESXi üzerindeki tüm sanal makinelerin adlarını alıp bir .txt dosyasına depolamak için aşağıdaki iki komutu çalıştırın.
&lsaquo;Sunucu adı&rsaquo;, &lsaquo;kullanıcı adı&rsaquo;, &lsaquo;parola&rsaquo;, &lsaquo;outputfile.txt&rsaquo; değerlerini girdilerinizle değiştirin.

            Connect-VIServer -Server <server name> -User <user name> -Password <password>

            Get-VM |  Select Name | Sort-Object -Property Name >  <outputfile.txt>


5.    Çıktı dosyasını Not Defteri'nde açın. Profilini oluşturmak istediğiniz tüm sanal makineleri, her satırda bir sanal makine adı olacak şekilde başka bir dosyaya (örneğin ProfileVMList.txt) kopyalayın. Bu dosya, komut satırı aracının -VMListFile parametresinin girdisi olarak kullanılır

    ![Dağıtım Planlayıcısı](./media/site-recovery-deployment-planner/profile-vm-list.png)


####<a name="start-profiling"></a>Profil oluşturmaya başlama
Profili oluşturulacak sanal makinelerin listesini oluşturduktan sonra, aracı profil oluşturma modunda çalıştırabilirsiniz. Aracın profil oluşturma modunda çalıştırılmasına yönelik zorunlu ve isteğe bağlı parametreler aşağıda verilmiştir. [] içindeki parametreler isteğe bağlıdır.

ASRDeploymentPlanner.exe -Operation StartProfiling /?

| Parametre adı | Açıklama |
|---|---|
| -Operation |      StartProfiling |
| -Server | Sanal makineleri için profil oluşturulacak vCenter sunucusunun/ESXi konağının tam etki alanı adı.|
| -User | vCenter sunucusuna/ESXi konağına bağlanmak için kullanıcı adı. Kullanıcının en azından salt okunur erişimi olmalıdır.|
| -VMListFile |    Profili oluşturulacak sanal makinelerin listesini içeren dosya. Dosya yolu mutlak veya göreli olabilir. Bu dosya her satırda bir sanal makine adı/IP adresi içermelidir. Dosyada belirtilen sanal makine adı, vCenter sunucusu veya ESXi konağındaki VM adıyla aynı olmalıdır. <br> Örnek: “VMList.txt” dosyası aşağıdaki sanal makineleri içerir:<br>virtual_machine_A <br>10.150.29.110<br>virtual_machine_B |
| -NoOfDaysToProfile | Profil oluşturmanın çalıştırılacağı gün sayısı. Ortamınızda belirtilen süre içindeki iş yükü deseninin doğru bir öneri sağlayacak şekilde gözlemlenip kullanılması için profil oluşturma işleminin 15 günden uzun süre çalıştırılmaması önerilir |
| [-Directory] |    Profil oluşturma sırasında oluşturulan profil oluşturma verilerini depolamak için UNC veya yerel dizin yolu. Belirtilmemişse, geçerli yolun altındaki ‘ProfiledData’ adlı dizin varsayılan dizin olarak kullanılır. |
| [-Password ] | vCenter sunucusuna/ESXi konağına bağlanmak için parola. Şu anda belirtilmezse, komut yürütülürken sorulacaktır.|
|  [-StorageAccountName]  | Şirket içinden Azure’a veri çoğaltma için ulaşılabilir aktarım hızını bulmak için Azure Depolama hesabı adı. Araç, aktarım hızını hesaplamak için test verilerini bu depolama hesabına yükler.|
| [-StorageAccountKey] | Depolama hesabına erişmek için kullanılan Azure Depolama hesabı anahtarı. Azure portal > Depolama hesapları > [Depolama hesabı adı] > Ayarlar > Erişim Anahtarları > Anahtar1 (veya klasik depolama hesabı için birincil erişim anahtarı) öğesine gidin. |

Sanal makinelerinizin profilini en az 15 ile 30 gün süreyle oluşturmanız önerilir. Profil oluşturma süresi boyunca ASRDeploymentPlanner.exe çalışmaya devam eder. Araç, profil oluşturma süre girdisini gün cinsinden alır. Aracın hızlı bir testi için birkaç saat veya dakika boyunca profil oluşturmak isterseniz, Genel Önizleme sürümünde saati karşılık gelen gün ölçüsüne dönüştürmeniz gerekir.  Örneğin, 30 dakika boyunca profil oluşturmak için girdinin 30 / (60*24) = 0,021 gün olması gerekir.  İzin verilen en kısa profil oluşturma süresi 30 dakikadır.

Profil oluşturma sırasında, Azure Site Recovery’nin çoğaltma sırasında Yapılandırma Sunucusu / İşlem Sunucusundan Azure’a elde edilebileceği aktarım hızını bulmak için, isteğe bağlı olarak bir Azure Depolama hesabı adı ve anahtarı geçirebilirsiniz. Profil oluşturma sırasında Azure Depolama hesabı adı ve anahtarı geçirilmezse, araç ulaşılabilir aktarım hızını hesaplamaz.


Farklı sanal makine kümeleri için aracın birden çok örneğini çalıştırabilirsiniz. Sanal makine adlarının, profil kümelerinin hiçbirinde yinelenmediğinden emin olun. Örneğin, on sanal makine (VM1 - VM10) profili oluşturdunuz ve birkaç gün sonra beş sanal makine (VM11 - VM15) profili daha oluşturmak istiyorsunuz; bu durumda, ikinci sanal makine kümesi (VM11 - VM15) için başka bir komut satırı konsolundan aracı çalıştırabilirsiniz. Ancak, ikinci sanal makine kümesinde birinci profil oluşturma örneğinden herhangi bir sanal makine adı olmadığından veya ikinci çalıştırma için farklı bir çıktı dizini kullandığınızdan emin olun. Aracın iki örneği aynı sanal makinelerin profilini oluşturmak için kullanılır ve aynı çıktı dizinini kullanırsa, oluşturulan rapor hatalı olacaktır.

Sanal makine yapılandırması, profil oluşturma işleminin başında bir kez yakalanır ve VMDetailList.xml adlı bir dosyada depolanır. Bu bilgiler, rapor oluşturma sırasında kullanılır. Profil oluşturmanın başlamasıyla sona ermesi arasında VM yapılandırmasında meydana gelen hiçbir değişiklik (örneğin çekirdek, disk, NIC sayısının artması vb.) yakalanmaz. Profili oluşturulmuş herhangi bir sanal makine yapılandırmasının profil oluşturma sırasında değiştiği bir durumla karşılaşırsanız, Genel Önizleme sürümünde rapor oluştururken en son sanal makine bilgilerini almaya yönelik geçici çözüm aşağıda verilmiştir.   

* 'VMdetailList.xml' dosyasını yedekleyip, dosyayı geçerli konumundan silin.
* -User ve -Password bağımsız değişkenlerini rapor oluşturma sırasında geçirin.

Profil oluşturma komutu, profil oluşturma dizininde birkaç dosya oluşturur; lütfen bu dosyaların hiçbirini silmeyin, aksi takdirde rapor oluşturma işlemi etkilenir.

#####<a name="example-1-to-profile-virtual-machines-for-30-days-and-find-the-throughput-from-on-premises-to-azure"></a>Örnek 1: 30 günlük sanal makine profili oluşturmak ve şirket içinden Azure’a aktarım hızını bulma
ASRDeploymentPlanner.exe **-Operation** StartProfiling -Directory “E:\vCenter1_ProfiledData” **-Server** vCenter1.contoso.com **-VMListFile** “E:\vCenter1_ProfiledData\ProfileVMList1.txt”  **-NoOfDaysToProfile**  30  **-User** vCenterUser1 **-StorageAccountName**  asrspfarm1 **-StorageAccountKey** Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==

#####<a name="example-2-to-profile-virtual-machines-for-15-days"></a>Örnek 2: 15 günlük sanal makine profili oluşturma
ASRDeploymentPlanner.exe **-Operation** StartProfiling **-Directory** “E:\vCenter1_ProfiledData” **-Server** vCenter1.contoso.com **-VMListFile** “E:\vCenter1_ProfiledData\ProfileVMList1.txt”  **-NoOfDaysToProfile**  15  -User vCenterUser1

#####<a name="example-3-to-profile-virtual-machines-for-1-hour-for-a-quick-test-of-the-tool"></a>Örnek 3: Aracın hızlı bir testine yönelik 1 saatlik sanal makine profili oluşturma
ASRDeploymentPlanner.exe **-Operation** StartProfiling **-Directory** “E:\vCenter1_ProfiledData” **-Server** vCenter1.contoso.com **-VMListFile** “E:\vCenter1_ProfiledData\ProfileVMList1.txt”  **-NoOfDaysToProfile**  0.04  **-User** vCenterUser1


>[!NOTE]
>
> * Aracın çalıştığı sunucu yeniden başlatılırsa veya kilitlenmişse ya da Ctrl+C tuşları ile araçtan çıkış yaparsanız, profili oluşturulan veriler korunur. Bu nedenle, profili oluşturulan verilen son 15 dakikası kaybedilebilir. Sunucu yedeklemeyi başlattıktan sonra aracı profil oluşturma modunda yeniden çalıştırmanız gerekir.
>
> * Azure Depolama hesabı adı ve anahtarı geçirildiğinde, araç profil oluşturma işleminin son adımında aktarım hızını ölçer. Profil oluşturma düzgün biçimde tamamlanmadan önce araç sonlandırılırsa, aktarım hızı hesaplanmaz. Raporu oluşturmadan önce aktarım hızını bulmak için GetThroughput işlemini her zaman komut satırı konsolundan çalıştırabilirsiniz; aksi takdirde, oluşturulan raporda ulaşılan aktarım hızı bilgileri bulunmaz.
>

##<a name="generate-report"></a>Rapor oluşturma
Tüm dağıtım önerilerini özetleyen rapor çıktısı olarak araç bir XLSM (makro özellikli Microsoft Excel dosyası) oluşturur; rapor DeploymentPlannerReport_<Unique Numeric Identifier>.xlsm olarak adlandırılır ve belirtilen dizine yerleştirilir.

Profil oluşturma tamamlandıktan sonra, aracı rapor oluşturma modunda çalıştırabilirsiniz. Aracın rapor oluşturma modunda çalıştırılmasına yönelik zorunlu ve isteğe bağlı parametreler aşağıda verilmiştir. [] içindeki parametreler isteğe bağlıdır.

ASRDeploymentPlanner.exe -Operation GenerateReport /?

|Parametre adı | Açıklama |
|-|-|
| -Operation | GenerateReport |
| -Server |  Raporu oluşturulacak profili oluşturulmuş sanal makinelerin bulunduğu vCenter/vSphere Sunucusu tam etki alanı adı veya IP adresi (profil oluşturma sırasında kullandığınız adın veya IP adresinin aynısını kullanın). Profil oluşturma sırasında bir vCenter Sunucusu kullandıysanız, rapor oluşturma için vSphere Sunucusu kullanamazsınız.|
| -VMListFile | Rapor oluşturulacak profili oluşturulmuş sanal makinelerin listesini içeren dosya. Dosya yolu mutlak veya göreli olabilir. Bu dosya her satırda bir sanal makine adı/IP adresi içermelidir. Dosyada belirtilen sanal makine adları, vCenter sunucusu ya da ESXi konağındakilerle aynı olmalı ve profil oluşturma sırasında kullanılanla eşleşmelidir.|
| [-Directory] | Profili oluşturulan verilerin (profil oluşturma sırasında oluşturulan dosyalar) depolandığı UNC veya yerel dizin yolu. Bu veriler, rapor oluşturmak için gereklidir. Belirtilmezse, 'ProfiledData' dizini kullanılır. |
| [-GoalToCompleteIR] |    Profili oluşturulan sanal makinelerin ilk çoğaltmasının tamamlanması gereken saat sayısı. Oluşturulan rapor, ilk çoğaltması belirtilen süre içinde tamamlanması gereken sanal makine sayısını sağlar. Varsayılan değer 72 saattir. |
| [-User] | vCenter/vSphere sunucusuna bağlanmak için kullanıcı adı. Bu parametre, sanal makinelerin disk sayısı, çekirdek sayısı, NIC sayısı gibi raporda kullanılacak en son yapılandırma bilgilerini getirmek için kullanılır. Bu seçenek belirtilmezse, profil oluşturma işleminin başında toplanan yapılandırma bilgileri kullanılır. |
| [-Password] | vCenter sunucusuna/ESXi konağına bağlanmak için parola. Parametre olarak belirtilmezse, daha sonra komut yürütülürken sorulacaktır. |
| [-DesiredRPO] | Dakika cinsinden istenen Kurtarma Noktası Hedefi (RPO). Varsayılan değer 15 dakikadır.|
| [-Bandwidth] | MB/sn cinsinden bant genişliği. Bu değer, belirtilen bant genişliği için ulaşılabilecek RPO’yu hesaplamak için kullanılır. |
| [-StartDate]  | AA-GG-YYYY:SS:DD (24 saat biçiminde) cinsinden başlangıç tarihi ve saati. ‘StartDate’ değeri ‘EndDate’ ile birlikte belirtilmelidir. Belirtildiğinde, StartDate ile EndDate arasında toplanan profili oluşturulmuş veriler için rapor oluşturulur. |
| [-EndDate] | AA-GG-YYYY:SS:DD (24 saat biçiminde) cinsinden bitiş tarihi ve saati. ‘EndDate’ değeri ‘StartDate’ ile birlikte belirtilmelidir. Belirtildiğinde, StartDate ile EndDate arasında toplanan profili oluşturulmuş veriler için rapor oluşturulur. |
| [-GrowthFactor] |Yüzde cinsinden büyüme faktörü. Varsayılan değer %30'dur.  |


##### <a name="example-1-to-generate-report-with-default-values-when-profiled-data-is-on-the-local-drive"></a>Örnek 1: Profili oluşturulan veriler yerel sürücüde olduğunda raporu varsayılan değerlerle oluşturma
ASRDeploymentPlanner.exe **-Operation** GenerateReport **-Server** vCenter1.contoso.com **-Directory** “E:\vCenter1_ProfiledData” **-VMListFile** “E:\vCenter1_ProfiledData\ProfileVMList1.txt”


##### <a name="example-2-to-generate-report-when-profiled-data-is-on-a-remote-server-user-should-have-readwrite-access-on-the-remote-directory"></a>Örnek 2: Profili oluşturulan veriler uzak bir sunucuda olduğunda rapor oluşturma. Kullanıcının uzak dizin üzerinde okuma/yazma erişimi olmalıdır.
ASRDeploymentPlanner.exe **-Operation** GenerateReport **-Server** vCenter1.contoso.com **-Directory** “\\PS1-W2K12R2\vCenter1_ProfiledData” **-VMListFile** “\\PS1-W2K12R2\vCenter1_ProfiledData\ProfileVMList1.txt”

##### <a name="example-3-generate-report-with-specific-bandwidth-and-goal-to-complete-ir-within-specified-time"></a>Örnek 3: Belirli bir bant genişliği ve belirtilen süre içinde IR tamamlama hedefi ile rapor oluşturma
ASRDeploymentPlanner.exe **-Operation** GenerateReport **-Server** vCenter1.contoso.com **-Directory** “E:\vCenter1_ProfiledData” **-VMListFile** “E:\vCenter1_ProfiledData\ProfileVMList1.txt” **-Bandwidth** 100 **-GoalToCompleteIR** 24

##### <a name="example-4-generate-report-with-5-growth-factor-instead-of-the-default-30"></a>Örnek 4: %30’luk varsayılan değer yerine %5 büyüme faktörü ile rapor oluşturma
ASRDeploymentPlanner.exe **-Operation** GenerateReport **-Server** vCenter1.contoso.com **-Directory** “E:\vCenter1_ProfiledData” **-VMListFile** “E:\vCenter1_ProfiledData\ProfileVMList1.txt” **-GrowthFactor** 5

##### <a name="example-5-generate-report-with-a-subset-of-profiled-data-say-you-have-30-days-of-profiled-data-and-want-to-generate-the-report-for-only-20-days"></a>Örnek 5: Profili oluşturulan verilerin bir alt kümesi ile rapor oluşturma. 30 günlük profili oluşturulmuş verilerinizin olduğunu ve raporu yalnızca 20 gün için oluşturduğunuzu varsayalım.
ASRDeploymentPlanner.exe **-Operation** GenerateReport **-Server** vCenter1.contoso.com **-Directory** “E:\vCenter1_ProfiledData” **-VMListFile** “E:\vCenter1_ProfiledData\ProfileVMList1.txt” **-StartDate**  01-10-2017:12:30 -**EndDate** 01-19-2017:12:30

##### <a name="example-6-generate-report-for-5-minutes-rpo"></a>Örnek 6: 5 dakikalık RPO için rapor oluşturma.
ASRDeploymentPlanner.exe **-Operation** GenerateReport **-Server** vCenter1.contoso.com **-Directory** “E:\vCenter1_ProfiledData” **-VMListFile** “E:\vCenter1_ProfiledData\ProfileVMList1.txt”  **-DesiredRPO** 5

### <a name="percentile-value-used-for-the-calculation"></a>Hesaplama için kullanılan yüzdelik değer
**Rapor oluşturulurken, profil oluşturma sırasında toplanan performans ölçümlerinin hangi varsayılan yüzdelik dilim değeri kullanılır?**

Aracın, tüm sanal makinelerin profili oluşturulurken toplanan okuma/yazma IOPS, yazma IOPS ve veri değişim sıklığı için varsayılan değeri yüzde 95’lik dilimdir. Bu değer, örneğin günde bir kez çalışan bir yedekleme işi, düzenli bir veritabanı dizini oluşturma veya analiz raporu oluşturma etkinliği ya da profil oluşturma döneminde gerçekleşen kısa süreli benzer bir olay gibi geçici olaylar nedeniyle sanal makinelerinizin karşılaşabileceği yüzde 100’lük dilimin, hedef Azure Depolama ve kaynak bant genişliği gereksinimlerini belirlemek üzere kullanılmamasını sağlar. Yüzde 95’lik dilim değeri, gerçek iş yükü özelliklerinin gerçek bir resmini verir ve bu iş yükleri Microsoft Azure üzerinde çalışırken en iyi performansı sağlar. Bu sayıyı sık değiştirmeniz beklenmez, ancak daha da düşürmeyi seçerseniz (örn. yüzde 90’lık dilim), bu ‘ASRDeploymentPlanner.exe.config’ yapılandırma dosyasını varsayılan klasörde güncelleştirebilir ve kaydederek var olan profili oluşturulmuş verilere ilişkin yeni bir rapor oluşturabilirsiniz.

        &lsaquo;add key="WriteIOPSPercentile" value="95" /&rsaquo;>      
        &lsaquo;add key="ReadWriteIOPSPercentile" value="95" /&rsaquo;>      
        &lsaquo;add key="DataChurnPercentile" value="95" /&rsaquo;

### <a name="growth-factor"></a>Büyüme faktörü
**Dağıtım planlaması sırasında büyüme faktörü neden göz önünde bulundurulmalıdır?**

Zaman içindeki kullanımın olası artışı varsayılarak, iş yükü özelliklerinizde büyümenin hesaba katılması önemlidir. Bunun nedeni, korunduktan sonra iş yükü özellikleriniz değişirse, korumayı devre dışı bırakıp yeniden etkinleştirmeden koruma için şu anda farklı bir Azure Depolama hesabına geçmenin yolu olmamasıdır. Örneğin günümüzde bir sanal makine standart bir depolama çoğaltma hesabına uyuyorsa (diyelim ki üç aylık sürede), sanal makine üzerinde çalışan uygulamanın kullanıcı sayısının artması nedeniyle, örneğin VM üzerindeki değişim sıklığı artar ve Azure Site Recovery çoğaltmasının daha yüksek olan yeni değişim sıklığına ayak uydurabilmesi için premium depolamaya geçmeyi gerektirirse, premium depolama hesabı için korumayı devre dışı bırakıp yeniden etkinleştirmeniz gerekir. Bu nedenle, dağıtım planlaması sırasında büyümenin planlanması önemle tavsiye edilir ve varsayılan değer %30’dur. Uygulamalarınızın kullanım desenini ve büyüme tahminlerini en iyi siz bilirsiniz ve rapor oluştururken bu sayıyı uygun şekilde siz değiştirebilirsiniz. Aslında, profili oluşturulmuş aynı verilerle farklı büyüme faktörleri içeren birden fazla rapor oluşturabilir ve en çok işinize yarayacak hedef Azure Depolama ile kaynak bant genişliği önerilerini görebilirsiniz.

Oluşturulan Microsoft Excel raporu aşağıdaki sayfaları içerir

* [Girdi](site-recovery-deployment-planner.md#input)
* [Öneriler](site-recovery-deployment-planner.md#recommendations-with-desired-rpo-as-input)
* [Öneriler-Bant Genişliği Girdisi](site-recovery-deployment-planner.md#recommendations-with-available-bandwidth-as-input)
* [VM<->Depolama Yerleşimi](site-recovery-deployment-planner.md#vm-storage-placement)
* [Uyumlu VM’ler](site-recovery-deployment-planner.md#compatible-vms)
* [Uyumsuz VM’ler](site-recovery-deployment-planner.md#incompatible-vms)

![Dağıtım Planlayıcısı](./media/site-recovery-deployment-planner/dp-report.png)


##<a name="get-throughput"></a>Aktarım hızı alma
Azure Site Recovery’nin çoğaltma sırasında şirket içinden Azure’a elde edebildiği aktarım hızını tahmin etmek için aracı GetThroughput modunda çalıştırın. Araç, aracın çalıştığı sunucudan aktarım hızını hesaplar (Yapılandırma Sunucusu boyutlandırma kılavuzunu temel alan bir sunucu olması idealdir).  Azure Site Recovery altyapı bileşenlerini şirket içinde zaten dağıttıysanız, aracı Yapılandırma Sunucusu’nda çalıştırın.

Bir komut satırı konsolu açın ve ASR dağıtım planlama aracının klasörüne gidin.  ASRDeploymentPlanner.exe dosyasını aşağıdaki parametrelerle çalıştırın. [] içindeki parametreler isteğe bağlıdır.

ASRDeploymentPlanner.exe -Operation GetThroughput /?

|Parametre adı | Açıklama |
|-|-|
| -operation | GetThroughput |
| [-Directory] | Profili oluşturulan verilerin (profil oluşturma sırasında oluşturulan dosyalar) depolandığı UNC veya yerel dizin yolu. Bu veriler, rapor oluşturmak için gereklidir. Belirtilmezse, 'ProfiledData' dizini kullanılır.  |
| -StorageAccountName | Şirket içinden Azure’a veri çoğaltma için kullanılan bant genişliğini bulmak için Azure Depolama hesabı adı. Araç, kullanılan bant genişliğini bulmak için test verilerini bu depolama hesabına yükler. |
| -StorageAccountKey | Depolama hesabına erişmek için kullanılan Azure Depolama Hesabı Anahtarı. Azure portal > Depolama hesapları > [Depolama hesabı adı] > Ayarlar > Erişim Anahtarları > Anahtar1 (veya klasik depolama hesabı için birincil erişim anahtarı) öğesine gidin. |
| -VMListFile | Kullanılan bant genişliğini hesaplamak için profili oluşturulacak sanal makinelerin listesini içeren dosya. Dosya yolu mutlak veya göreli olabilir. Bu dosya her satırda bir sanal makine adı/IP adresi içermelidir. Dosyada belirtilen sanal makine adları, vCenter sunucusu ya da ESXi konağındakilerle aynı olmalıdır.<br>Örneğin “VMList.txt” dosyası aşağıdaki sanal makineleri içerir:<br>virtual machine_A <br>10.150.29.110<br>virtual machine_B|

Araç belirtilen dizinde 64 MB’lik birkaç ‘asrvhdfile<#>.vhd’ (# sayıdır) dosyası oluşturur.  Aktarım hızını bulmak için bu dosyaları Azure Depolama hesabına yükler. Aktarım hızı ölçüldükten sonra tüm bu dosyaları Azure Depolama hesabından ve yerel sunucudan siler. Araç aktarım hızını hesapladığı sırada herhangi bir nedenle sonlandırılırsa, dosyalar Azure Depolama veya yerel sunucudan silinmez ve sizin el ile silmeniz gerekir.

Aktarım hızı belirli bir zaman noktasında ölçülür ve diğer tüm faktörlerin aynı kalması koşuluyla Azure Site Recovery’nin çoğaltma sırasında ulaşabileceği en yüksek aktarım hızıdır. Örneğin, herhangi bir uygulama aynı ağ üzerinde daha fazla bant genişliği tüketmeye başlarsa, çoğaltma sırasında gerçek aktarım hızı farklılık gösterir. Bir Yapılandırma Sunucusundan GetThroughput komutunu çalıştırıyorsanız, araç korunan sanal makineleri ve devam eden çoğaltmayı fark etmez. GetThroughput işlemi korunan sanal makinelerin yüksek veri değişim sıklığına sahip olduğu zamanlarda çalıştırıldığında, düşük veri değişim sıklığına sahip olduğu zamanlarda çalıştırılmasına göre ölçülen aktarım hızının sonucu farklı olur.  Çeşitli zamanlarda hangi aktarım hızına ulaşılabileceğini anlamak için, profil oluşturma sırasında farklı zaman noktalarında aracın çalıştırılması önerilir. Raporda, araç son ölçülen aktarım hızını gösterir.


##### <a name="example"></a>Örnek
ASRDeploymentPlanner.exe **-Operation** GetThroughput **-Directory**  E:\vCenter1_ProfiledData **-VMListFile** E:\vCenter1_ProfiledData\ProfileVMList1.txt  **-StorageAccountName**  asrspfarm1 **-StorageAccountKey** by8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==

>[!NOTE]
>
> * Aracı, Yapılandırma Sunucusu ile aynı depolama ve CPU özelliklerine sahip bir sunucuda çalıştırın
>
> * Çoğaltma için sürenin %100 RPO’sunu karşılamak üzere önerilen bant genişliğini sağlayın. Doğru bant genişliğini sağladıktan sonra bile, araç tarafından ulaşıldığı bildirilen aktarım hızında herhangi bir artış görmezseniz aşağıdakileri denetleyin:
>
> a. Azure Site Recovery aktarım hızını sınırlayan herhangi bir ağ Hizmet Kalitesi (QoS) olup olmadığını denetleyin
>
> b. Ağ gecikme süresini en aza indirmek için, Azure Site Recovery kasanızın desteklenen en yakın fiziksel Microsoft Azure bölgesinde olup olmadığını denetleyin
>
> c. Yerel depolama özelliklerinizi denetleyin ve donanımı iyileştirmeye çalışın (örn. HDD’den SSD’ye)
>
> d. [Çoğaltma için kullanılan ağ bant genişliği miktarını artırmak](./site-recovery-plan-capacity-vmware.md#control-network-bandwidth) üzere işlem sunucusundaki Azure Site Recovery ayarlarını değiştirin.
>

##<a name="recommendations-with-desired-rpo-as-input"></a>Girdi olarak istenen RPO ile öneriler

###<a name="profiled-data"></a>Profili oluşturulan veriler

![Dağıtım Planlayıcısı](./media/site-recovery-deployment-planner/profiled-data-period.png)

**Profili oluşturulmuş veri dönemi**, profil oluşturma işleminin gerçekleştirildiği süredir. Varsayılan olarak, rapor oluşturma sırasında StartDate ve EndDate seçenekleri kullanılarak rapor belirli bir süre için oluşturulmadıkça araç, hesaplama için profili oluşturulmuş tüm verileri alır.

**Sunucu Adı**, sanal makinelerinin raporu oluşturulan VMware vCenter veya ESXi konağının adı ya da IP adresidir.

**İstenen RPO**, dağıtımınıza yönelik Kurtarma Noktası Hedefidir (RPO). Varsayılan olarak, gerekli ağ bant genişliği 15, 30 ve 60 dakikalık RPO değerleri için hesaplanır. Seçim temel alınarak, etkilenen değerler sayfada güncelleştirilir. Raporu oluştururken DesiredRPOinMin parametresini kullandıysanız, değer bu İstenen RPO açılır listesinde gösterilir.

###<a name="profiling-overview"></a>Profil Oluşturmaya Genel Bakış

![Dağıtım Planlayıcısı](./media/site-recovery-deployment-planner/profiling-overview.png)

**Profili Oluşturulan Toplam Sanal Makine Sayısı**, profili oluşturulan verileri bulunan sanal makinelerin toplam sayısıdır. VMListFile dosyasında profili oluşturulmamış sanal makineler varsa, bu sanal makineler rapor oluşturma işleminde göz önünde bulundurulmaz ve profili oluşturulan toplam sanal makine sayısının dışında tutulur.

**Uyumlu Sanal Makineler**, Azure Site Recovery kullanılarak Azure’da korunabilen sanal makine sayısıdır. Bu sayı, gerekli ağ bant genişliği, Azure Depolama hesabı sayısı, Microsoft Azure çekirdek sayısı ve Yapılandırma Sunucusu ile ek İşlem Sunucularının sayısının hesaplandığı uyumlu sanal makinelerin toplamıdır. Her uyumlu sanal makinenin ayrıntıları, raporun Uyumlu VM'ler tablosunda bulunur.

**Uyumsuz Sanal Makineler**, Azure Site Recovery ile koruma için uygun olmayan, profili oluşturulmuş sanal makine sayısıdır. Uyumsuzluğun nedenleri, aşağıdaki Uyumsuz VM’ler bölümünde belirtilmiştir. VMListFile dosyasında profili oluşturulmamış sanal makineler varsa, bu sanal makineler uyumsuz sanal makine sayısının dışında tutulur. Bu sanal makineler, Uyumsuz VM’ler tablosunun sonunda 'Veri bulunamadı' olarak listelenir.

**İstenen RPO**, dakika cinsinden istediğiniz RPO’dur. Rapor, varsayılan değer 15 dakika olmak üzere 15, 30 ve 60 dakikalık üç RPO değeri için oluşturulur. Rapordaki bant genişliği önerisi, tablonun sağ üst köşesinde bulunan İstenen RPO açılır menüsündeki seçiminize göre değişir. Raporu özel bir değer ile “-DesiredRPO” parametresini kullanarak oluşturduysanız, bu özel değer İstenen RPO açılır listesinde varsayılan olarak gösterilir.

###<a name="required-network-bandwidth-mbps"></a>Gerekli Ağ Bant Genişliği (Mb/sn)

![Dağıtım Planlayıcısı](./media/site-recovery-deployment-planner/required-network-bandwidth.png)

**%100 RPO süresini karşılamak için:** İstediğiniz %100 RPO süresini karşılamak için Mb/sn cinsinden ayrılacak önerilen bant genişliğidir. Bu bant genişliği miktarı, herhangi bir RPO ihlalini önlemek üzere tüm uyumlu sanal makinelerinizin kararlı durum delta çoğaltması için ayrılmalıdır.

**%90 RPO süresini karşılamak için**: Geniş bant fiyatlandırması veya istediğiniz %100 RPO süresini karşılamak için gereken bant genişliğini sağlayamamanız durumunda başka bir nedenle, istediğiniz %90 RPO süresini karşılayabilen daha düşük bir bant genişliği sağlamayı seçebilirsiniz. Daha düşük olan bu bant genişliğini sağlamanın etkilerini anlamak için, raporda beklenen RPO ihlallerinin sayısı ve süresine ilişkin bir ne yapmalı analizi sağlar.

**Arşivlenen Aktarım Hızı:** Azure Depolama hesabının bulunduğu Microsoft Azure bölgesine GetThroughput komutunu gönderdiğiniz sunucudan aktarım hızıdır. Yapılandırma Sunucusu / İşlem Sunucusu depolama ve ağ özelliklerinin, aracı çalıştırdığınız sunucudakilerle aynı kalması şartıyla, Azure Site Recovery kullanarak uyumlu sanal makineleri koruduğunuzda ulaşılabilecek yaklaşık aktarım hızını gösterir. Arşivlenen Aktarım Hızı, Azure Depolama hesabının bulunduğu Microsoft Azure bölgesine GetThroughput komutunu gönderdiğiniz sunucudan aktarım hızıdır. Yapılandırma Sunucusu / İşlem Sunucusu depolama ve ağ özelliklerinin, aracı çalıştırdığınız sunucudakilerle aynı kalması şartıyla, Azure Site Recovery kullanarak uyumlu sanal makineleri koruduğunuzda ulaşılabilecek yaklaşık aktarım hızını gösterir.

Çoğaltma için sürenin %100 RPO’sunu karşılamak üzere önerilen bant genişliğini sağlamanız gerekir. Doğru bant genişliğini sağladıktan sonra bile, araç tarafından ulaşıldığı bildirilen aktarım hızında herhangi bir artış görmezseniz aşağıdakileri denetleyin:

a.    Azure Site Recovery aktarım hızını sınırlayan herhangi bir ağ Hizmet Kalitesi (QoS) olup olmadığını denetleyin

b.    Ağ gecikme süresini en aza indirmek için, Azure Site Recovery kasanızın desteklenen en yakın fiziksel Microsoft Azure bölgesinde olup olmadığını denetleyin

c.    Yerel depolama özelliklerinizi denetleyin ve donanımı iyileştirmeye çalışın (örn. HDD’den SSD’ye)

d. [Çoğaltma için kullanılan ağ bant genişliği miktarını artırmak](./site-recovery-plan-capacity-vmware.md#control-network-bandwidth) üzere işlem sunucusundaki Azure Site Recovery ayarlarını değiştirin.

Aracı zaten korunan sanal makinelere sahip bir Yapılandırma Sunucusu / İşlem Sunucusu üzerinde çalıştırdığınız durumlarda, ulaşılan aktarım hızı sayısı ilgili zaman noktasında işlenmekte olan değişim sıklığı miktarına bağlı olarak değişeceği için, aracı birkaç kez çalıştırın.

Tüm kurumsal Azure Site Recovery dağıtımları için [ExpressRoute](https://aka.ms/expressroute) kullanılması önerilir.

###<a name="required-azure-storage-accounts"></a>Gerekli Azure Depolama Hesapları
Bu grafikte tüm uyumlu sanal makineleri korumak için gereken Azure Depolama hesaplarının (standart ve premium) toplam sayısı gösterilmektedir.  Her bir sanal makine için kullanılması gereken depolama hesabını öğrenmek üzere [Önerilen VM yerleşim planı](site-recovery-deployment-planner.md#vm-storage-placement)’na tıklayın.  

![Dağıtım Planlayıcısı](./media/site-recovery-deployment-planner/required-azure-storage-accounts.png)

###<a name="required-number-of-azure-cores"></a>Gerekli Azure Çekirdek Sayısı
Tüm uyumlu sanal makinelerin yük devretme işlemi ya da yük devretme testi öncesinde sağlanması gereken toplam çekirdek sayısıdır. Abonelikte yeterli çekirdek yoksa, yük devretme testi veya yük devretme sırasında Azure Site Recovery, sanal makineleri oluşturamaz.

![Dağıtım Planlayıcısı](./media/site-recovery-deployment-planner/required-number-of-azure-cores.png)

###<a name="required-on-premises-infrastructure"></a>Gerekli Şirket İçi Altyapısı
Tüm uyumlu sanal makineleri korumak için yapılandırılması gereken Yapılandırma Sunucusu ve ek İşlem Sunucularının toplam sayısıdır. En büyük yapılandırmaya ilişkin desteklenen [limitlere](https://aka.ms/asr-v2a-on-prem-components) göre: günlük değişim sıklığı veya en fazla korunan sanal makine sayısından (sanal makine başına ortalama üç disk olduğu varsayılarak), Yapılandırma Sunucusu veya ek İşlem Sunucusunda ilk gerçekleşen olaya göre, araç ek sunucular önerir. Günlük toplam değişim sıklığı ve toplam korunan disk sayısına ilişkin ayrıntılar [Girdi](site-recovery-deployment-planner.md#input) tablosunda bulunabilir.

![Dağıtım Planlayıcısı](./media/site-recovery-deployment-planner/required-on-premises-infrastructure.png)

###<a name="what-if-analysis"></a>Ne Yapmalı Analizi
Bu analiz, sürenin yalnızca %90’ını karşılamasını istediğiniz RPO için daha düşük bir bant genişliği sağladığınızda profil oluşturma sırasında kaç tane ihlal oluşabileceğini özetler. Belirli bir günde bir veya daha fazla RPO ihlali oluşabilir; grafikte günün yoğun RPO zamanı gösterilmektedir.
Bu analizi temel alarak, belirtilen düşük bant genişliği ile tüm günlerdeki RPO ihlali sayısının ve bir gündeki en yoğun RPO gerçekleşme zamanının kabul edilebilir olup olmadığına karar verebilirsiniz. Değer kabul edilebilir düzeydeyse, çoğaltma için düşük bant genişliği ayırabilirsiniz; aksi takdirde, istenilen %100 RPO süresini karşılamak üzere önerilen yüksek bant genişliğini ayırmanız gerekir.

![Dağıtım Planlayıcısı](./media/site-recovery-deployment-planner/what-if-analysis.png)

###<a name="recommended-vm-batch-size-for-initial-replication"></a>İlk çoğaltma için önerilen VM toplu iş boyutu
Bu bölümde, istenilen %100 RPO süresini karşılamak üzere önerilen bant genişliği sağlanarak 72 saat içinde (yapılandırılabilir değer; bunu değiştirmek için rapor oluşturma sırasında GoalToCompleteIR parametresini kullanın) ilk çoğaltmayı tamamlamak amacıyla paralel olarak korunabilecek sanal makine sayısı önerilmektedir.  Grafikte, tüm uyumlu makinelerde algılanan ortalama sanal makine boyutuna göre 72 saat içinde ilk çoğaltmayı tamamlamak için bir bant genişliği değer aralığı ve hesaplanmış sanal makine toplu iş boyutu sayısı gösterilmektedir.  

Genel Önizleme sürümünde rapor, bir toplu işe hangi sanal makinelerin dahil edilmesi gerektiğini belirtmez. Her bir sanal makinenin boyutunu bulmak ve bir toplu işe yönelik sanal makinelerinizi seçmek ya da bilinen iş yükü özelliklerine göre seçim yapmak için, Uyumlu VM’ler tablosunda gösterilen disk boyutunu kullanabilirsiniz.  İlk çoğaltma tamamlama süresi, gerçek sanal makine disk boyutu, kullanılan disk alanı ve kullanılabilir ağ aktarım hızı ile doğru orantılı bir şekilde değişir.

![Dağıtım Planlayıcısı](./media/site-recovery-deployment-planner/recommended-vm-batch-size.png)

###<a name="growth-factor-and-percentile-values-used"></a>Kullanılan büyüme faktörü ve yüzdelik değerler
Tablonun alt kısmındaki bu bölümde, profili oluşturulan sanal makinelerin tüm performans sayaçları için kullanılan yüzdelik dilim değeri (varsayılan değer yüzde 95’lik dilim) ve tüm hesaplamalarda kullanılan % cinsinden büyüme faktörü (varsayılan değer %30) gösterilmektedir.

![Dağıtım Planlayıcısı](./media/site-recovery-deployment-planner/max-iops-and-data-churn-setting.png)

##<a name="recommendations-with-available-bandwidth-as-input"></a>Girdi olarak kullanılabilir bant genişliği ile ilgili öneriler

![Dağıtım Planlayıcısı](./media/site-recovery-deployment-planner/profiling-overview-bandwidth-input.png)

Azure Site Recovery çoğaltması için x MB/sn’den fazla bant genişliği sağlayamayacağınızı bildiğiniz bir durumla karşılaşabilirsiniz. Araç, kullanılabilir bant genişliğini girmenize (rapor oluşturma sırasında -Bandwidth parametresini kullanarak) ve dakika cinsinden ulaşılabilir RPO değerini almanıza olanak tanır. Bu ulaşılabilir RPO değeriyle ek bant genişliği sağlamanızın gerekli olup olmadığına veya bu RPO ile bir olağanüstü durum kurtarma çözümüne sahip olmak isteyip istemediğinize karar verebilirsiniz.

![Dağıtım Planlayıcısı](./media/site-recovery-deployment-planner/achievable-rpos.png)

##<a name="input"></a>Girdi
Girdi sayfası, profili oluşturulmuş VMware ortamına genel bir bakış sağlar.

![Dağıtım Planlayıcısı](./media/site-recovery-deployment-planner/Input.png)

**Başlangıç Tarihi ve Bitiş Tarihi**, rapor oluşturma için göz önünde bulundurulan profil oluşturma verilerinin başlangıç ve bitiş tarihleridir. Varsayılan olarak, başlangıç tarihi profil oluşturmanın başladığı, bitiş tarihi ise profil oluşturmanın durdurulduğu tarihtir.  Rapor bu parametrelerle oluşturulduysa bunlar ‘StartDate’ ve ‘EndDate’ değerleri olabilir. Başlangıç Tarihi ve Bitiş Tarihi: Rapor oluşturma için göz önünde bulundurulan profil oluşturma verilerinin başlangıç ve bitiş tarihleridir. Varsayılan olarak, başlangıç tarihi profil oluşturmanın başladığı, bitiş tarihi ise profil oluşturmanın durdurulduğu tarihtir.  Rapor bu parametrelerle oluşturulduysa bunlar ‘StartDate’ ve ‘EndDate’ değerleri olabilir.

**Profil oluşturulan toplam gün sayısı**, raporun oluşturulduğu başlangıç ile bitiş tarihleri arasında profil oluşturulan toplam gün sayısıdır. Profil oluşturulan toplam gün sayısı, raporun oluşturulduğu başlangıç ile bitiş tarihleri arasında profil oluşturulan toplam gün sayısıdır.

**Uyumlu sanal makine sayısı**, gerekli ağ bant genişliği, gerekli Azure Depolama hesabı sayısı, Microsoft Azure çekirdek sayısı ve Yapılandırma Sunucusu ile ek İşlem Sunucularının sayısının hesaplandığı uyumlu sanal makinelerin toplamıdır.
Tüm uyumlu sanal makinelerdeki toplam disk sayısı, tüm uyumlu sanal makinelerdeki toplam disk sayısıdır. Bu sayı, dağıtımda kullanılacak Yapılandırma Sunucusu ve ek İşlem Sunucusu sayısına karar vermeye yönelik girdilerden biri olarak kullanılır.

**Bir uyumlu sanal makinedeki ortalama disk sayısı**, tüm uyumlu sanal makinelerde hesaplanan ortalama disk sayısıdır.

**Ortalama disk boyutu (GB)**, tüm uyumlu sanal makinelerde hesaplanan ortalama disk boyutudur.

**İstenen RPO (dakika)**, varsayılan RPO’dur veya rapor oluşturma sırasında gerekli bant genişliğini tahmin etmek üzere ‘DesiredRPO’ parametresi için geçirilen değerdir.

**İstenen bant genişliği (Mb/sn)**, rapor oluşturma sırasında ulaşılabilir RPO’yu tahmin etmek üzere ‘Bandwidth’ parametresi için geçirdiğiniz değerdir.

**Bir günde gözlemlenen tipik veri değişim sıklığı (GB)**, profil oluşturulan tüm günlerde gözlemlenen ortalama veri değişim sıklığıdır. Bu sayı, dağıtımda kullanılacak Yapılandırma Sunucusu ve ek İşlem Sunucusu sayısına karar vermeye yönelik girdilerden biri olarak kullanılır.


##<a name="vm-storage-placement"></a>VM-Depolama yerleşimi

![Dağıtım Planlayıcısı](./media/site-recovery-deployment-planner/vm-storage-placement.png)

**Disk Depolama Türü**, ‘Yerleştirilecek VM’ler’ sütununda bahsedilen tüm ilgili sanal makineleri çoğaltmak için kullanılan ‘Standart’ veya ‘Premium’ Azure Depolama hesabıdır.

**Önerilen Ön Ek**, Azure depolama hesabını adlandırmak için kullanılabilecek, üç karakterli bir önerilen ön ektir. Her zaman kendi ön ekinizi kullanabilirsiniz, ancak aracın önerdiği ön ek, [Azure Depolama hesaplarının bölüm adlandırma kuralına](https://aka.ms/storage-performance-checklist) uygundur.

**Önerilen Hesap Adı**, önerilen ön ek dahil edildikten sonra Azure Depolama hesabı adının nasıl görünmesi gerektiğini gösterir. < > içindeki adı özel girdinizle değiştirin.

**Kayıt Depolama Hesabı:** Tüm çoğaltma kayıtları standart bir Azure Depolama hesabında depolanır. Premium Azure Depolama hesabına çoğaltılan sanal makineler için, kayıt depolamaya yönelik ek bir standart Azure Depolama hesabı sağlanmalıdır. Tek bir standart kayıt depolama hesabı, birden fazla premium çoğaltma depolama hesabı tarafından kullanılabilir. Standart depolama hesaplarına çoğaltılan sanal makineler, kayıtlarla aynı depolama hesabını kullanır.

**Önerilen Kayıt Hesabı Adı**, önerilen ön ek dahil edildikten sonra kayıt Azure Depolama hesabı adının nasıl görünmesi gerektiğini gösterir. < > içindeki adı özel girdinizle değiştirin.

**Yerleştirme Özeti**, çoğaltma ve yük devretme testi / yük devretme sırasında Azure Depolama hesabındaki toplam sanal makine yükünün özetini sağlar. Buna Azure Depolama hesabıyla eşlenmiş toplam sanal makine sayısı, bu Azure Depolama hesabına yerleştirilen tüm sanal makinelerdeki toplam okuma/yazma IOPS sayısı, toplam yazma (çoğaltma) IOPS sayısı, tüm disklerde sağlanan toplam boyut ve toplam disk sayısı dahildir.

**Yerleştirilecek Sanal Makineler**, en iyi performans ve kullanım için belirli bir Azure Depolama hesabına yerleştirilmesi gereken tüm sanal makineleri listeler.

##<a name="compatible-vms"></a>Uyumlu VM’ler
![Dağıtım Planlayıcısı](./media/site-recovery-deployment-planner/compatible-vms.png)

**VM Adı**, rapor oluşturma sırasında VMListFile içinde kullanılan sanal makine adı veya IP adresidir. Bu sütunda ayrıca sanal makinelere bağlanan diskler (VMDK) listelenir.

**VM Uyumluluğu** iki değere sahiptir – Evet / Evet*. Evet*, sanal makinenin P20 veya P30 kategorisinde profili oluşturulmuş yüksek değişim sıklığı /IOPS disk uyumu ile [premium Azure Depolama](https://aka.ms/premium-storage-workload)’ya uygun olduğu, ancak disk boyutunun P10 veya P20 ile eşlenmeye neden olduğu durumlar için geçerlidir. Azure Depolama, bir diskin hangi premium depolama disk türüne eşleneceğine, boyutuna göre karar verir – örn. < 128 GB P10, 128 ila 512 GB P20, 512 GB ila 1023 GB ise P30’dur. Bu nedenle, bir diskin iş yükü özellikleri onu P20 veya P30’a yerleştirir, ancak boyutu onu daha düşük bir premium depolama disk türü ile eşlerse, araç bu sanal makineyi Evet* olarak işaretler ve kaynak disk boyutunu önerilen doğru premium depolama disk türüne uyacak şekilde değiştirmenizi ya da yük devretme sonrasındaki hedef disk türünü değiştirmenizi önerir.
Depolama Türü standart veya premiumdur.

**Önerilen Ön Ek**, üç karakterli Azure Depolama hesabı ön ekidir

**Depolama Hesabı**, önerilen ön eki kullanan addır

**Okuma/Yazma IOPS (Büyüme Faktörü ile)**, disk üzerinde gelecekteki büyüme faktörünü de (varsayılan değer %30) içeren en yoğun iş yükü IOPS değeridir (varsayılan değer yüzde 95’lik dilim). Sanal makinenin en yoğun Okuma/Yazma IOPS değeri, profil oluşturma döneminin her dakikasında içindeki ayrı disklerin Okuma/Yazma IOPS değeri toplamının tepe noktası olduğundan, sanal makinenin toplam Okuma/Yazma IOPS değeri her zaman sanal makinedeki ayrı disklerin Okuma/Yazma IOPS değerinin toplamı olmayacaktır.

**Mb/sn cinsinden Veri Değişim Sıklığı (Büyüme Faktörü ile)**, disk üzerinde gelecekteki büyüme faktörünü de (varsayılan değer %30) içeren en yüksek erime oranıdır (varsayılan değer yüzde 95’lik dilim). Sanal makinenin en yoğun veri değişim sıklığı, profil oluşturma döneminin her dakikasında içindeki ayrı disklerin veri değişim sıklığı toplamının tepe noktası olduğundan, sanal makinenin toplam veri değişim sıklığı her zaman sanal makinedeki ayrı disklerin veri değişim sıklığının toplamı olmayacaktır.

**Azure VM Boyutu** bu şirket içi sanal makine için eşlenen ideal Azure İşlem sanal makine boyutudur. Bu eşleme, şirket içi sanal makine belleğine, disk/çekirdek/NIC sayısına ve Okuma/Yazma IOPS değerine göre yapılır; her zaman tüm bu şirket içi sanal makine özellikleri ile eşleşen en düşük Azure sanal makine boyutunun kullanılması önerilir.

**Disk Sayısı**, sanal makine üzerindeki toplam disk sayısıdır (VMDK)

**Disk boyutu (GB)**, sanal makinenin tüm diskleri için sağlanan toplam boyuttur. Araç ayrıca sanal makinedeki ayrı diskler için disk boyutunu gösterir.

**Çekirdek**, sanal makine üzerindeki CPU çekirdeği sayısıdır.

**Bellek (MB)**, sanal makinedeki RAM kapasitesidir.

**NIC**, sanal makine üzerindeki NIC sayısıdır.

##<a name="incompatible-vms"></a>Uyumsuz VM’ler

![Dağıtım Planlayıcısı](./media/site-recovery-deployment-planner/incompatible-vms.png)

**VM Adı**, rapor oluşturma sırasında VMListFile içinde kullanılan sanal makine adı veya IP adresidir. Bu sütunda ayrıca sanal makinelere bağlanan diskler (VMDK) listelenir.

**VM Uyumluluğu**, belirli bir sanal makinenin Azure Site recovery ile kullanıma neden uyumlu olmadığını gösterir. Nedenler sanal makinenin her uyumsuz diski için ayrıca gösterilir ve yayımlanan Azure Depolama [limitlerine](https://aka.ms/azure-storage-scalbility-performance) göre aşağıdakilerden biri olabilir.

* Disk boyutu > 1023 GB – Azure Depolama şu anda 1 TB’den büyük disk boyutlarını desteklememektedir
* Toplam VM boyutu (çoğaltma + TFO) desteklenen Azure Depolama hesabı boyut limitini (35 TB) aşıyor – Bu durum genellikle sanal makinede sanal makineyi premium depolama bölgesine iten ve standart depolama için desteklenen Microsoft Azure / Azure Site Recovery üst limitlerini aşan bazı performans özelliklerine sahip tek bir disk olduğunda gerçekleşir. Ancak, bir premium Azure Depolama hesabı için desteklenen en büyük boyut 35 TB’dir ve tek bir korunan sanal makine birden fazla depolama hesabında korunamaz. Ayrıca, korunan bir sanal makinede TFO (yük devretme testi) yürütüldüğünde, çoğaltmanın devam ettiği depolama hesabında çalıştığı unutulmamalıdır; bu nedenle, disk çoğaltmanın devam etmesi ve paralel olarak yük devretme testinin başarılı olması için disk boyutunun 2 katı sağlanmalıdır.
* Kaynak IOPS, Azure Depolama IOPS için disk başına desteklenen 5000 limitini aşıyor
* Kaynak IOPS, Azure Depolama IOPS için VM başına desteklenen 80.000 limitini aşıyor
* Ortalama veri değişim sıklığı, disk için ortalama G/Ç’ye yönelik 10 Mb/sn’lik Azure Site Recovery veri değişim sıklığı limitini aşıyor
* Sanal makine üzerindeki tüm disklerde bulunan toplam veri değişim sıklığı, VM başına 54 MB/sn’lik desteklenen Azure Site Recovery veri değişim sıklığı sınırını aşıyor
* Algılanan ortalama yazma IOPS değeri, disk için 840 olan desteklenen Azure Site Recovery IOPS limitini aşıyor
* Hesaplanan anlık görüntü depolama alanı, 10 TB’lik desteklenen anlık görüntü depolama limitini aşıyor

**Okuma/Yazma IOPS (Büyüme Faktörü ile)**, disk üzerinde gelecekteki büyüme faktörünü de (varsayılan değer %30) içeren en yoğun iş yükü IOPS değeridir (varsayılan değer yüzde 95’lik dilim). Sanal makinenin en yoğun Okuma/Yazma IOPS değeri, profil oluşturma döneminin her dakikasında içindeki ayrı disklerin Okuma/Yazma IOPS değeri toplamının tepe noktası olduğundan, sanal makinenin toplam Okuma/Yazma IOPS değeri her zaman sanal makinedeki ayrı disklerin Okuma/Yazma IOPS değerinin toplamı olmayacaktır.

**Mb/sn cinsinden Veri Değişim Sıklığı (Büyüme Faktörü ile)**, disk üzerinde gelecekteki büyüme faktörünü de (varsayılan değer %30) içeren en yüksek erime oranıdır (varsayılan değer yüzde 95’lik dilim). Sanal makinenin en yoğun veri değişim sıklığı, profil oluşturma döneminin her dakikasında içindeki ayrı disklerin veri değişim sıklığı toplamının tepe noktası olduğundan, sanal makinenin toplam veri değişim sıklığı her zaman sanal makinedeki ayrı disklerin veri değişim sıklığının toplamı olmayacaktır.

**Disk Sayısı**, sanal makine üzerindeki toplam disk sayısıdır (VMDK)

**Disk boyutu (GB)**, sanal makinenin tüm diskleri için sağlanan toplam boyuttur. Araç ayrıca sanal makinedeki ayrı diskler için disk boyutunu gösterir.

**Çekirdek**, sanal makine üzerindeki CPU çekirdeği sayısıdır.

**Bellek (MB)**, sanal makinedeki RAM kapasitesidir.

**NIC**, sanal makine üzerindeki NIC sayısıdır.


##<a name="azure-site-recovery-limits"></a>Azure Site Recovery limitleri

**Çoğaltma Depolama Hedefi** | **Ortalama Kaynak Disk G/Ç Boyutu** |**Ortalama Kaynak Disk Veri Değişim Sıklığı** | **Günlük Toplam Kaynak Disk Veri Değişim Sıklığı**
---|---|---|---
Standart depolama | 8 KB    | 2 MB/sn | Disk başına&168; GB
Premium P10 disk | 8 KB    | 2 MB/sn | Disk başına&168; GB
Premium P10 disk | 16 KB | 4 MB/sn |    Disk başına&336; GB
Premium P10 disk | 32 KB veya daha yüksek | 8 MB/sn | Disk başına&672; GB
Premium P20/P30 disk | 8 KB    | 5 MB/sn | Disk başına&421; GB
Premium P20/P30 disk | 16 KB veya daha yüksek |10 MB/sn    | Disk başına&842; GB


Bunlar %30 G/Ç çakışmasını varsayan ortalama sayılardır. Azure Site Recovery; çakışma oranı, büyük yazma boyutları ve gerçek iş yükü G/Ç davranışına göre daha yüksek aktarım hızını işleyebilir. Yukarıdaki sayılar yaklaşık 5 dakikalık tipik bir kapsam olduğunu varsayar; örn. veriler yüklendikten sonra işlenir ve 5 dakika içinde bir kurtarma noktası oluşturulur.

Yukarıdaki yayımlanmış limitler yaptığımız testleri temel alsa da mümkün olan tüm uygulama G/Ç birleşimlerini kapsamamaktadır. Gerçek sonuçlar, uygulamanızın G/Ç karışımına göre değişir. En iyi sonuçlar için, gerçek performans görüntüsünü elde etmek üzere, dağıtım planlamasından sonra bile yük devretme testi kullanılarak her zaman kapsamlı uygulama testleri gerçekleştirilmesi önerilir.

##<a name="release-notes"></a>Sürüm notları
Azure Site Recovery Dağıtım Planlayıcısı Genel Önizleme 1.0 sürümünde, gelecek güncelleştirmelerde giderilecek aşağıdaki bilinen sorunlar bulunmaktadır.

* Araç yalnızca VMware’den Azure’a senaryosu için çalışır, Hyper-V’den Azure’a dağıtımlar için çalışmaz. Hyper-V’den Azure’a dağıtım senaryosu için [Hyper-V kapasite planlayıcısı aracını](./site-recovery-capacity-planning-for-hyper-v-replication.md) kullanın.
* GetThroughput işlemi ABD Hükümeti ve Çin Microsoft Azure bölgelerinde desteklenmez.
* vCenter farklı ESXi konakları üzerinde aynı ad/IP adresine sahip iki veya daha fazla sanal makineye sahipse araç, sanal makinelerin profilini oluşturamaz. Bu sürümde, araç VMListFile dosyasındaki yinelenen sanal makine adları/IP adresleri için profil oluşturmayı atlar. Bunun geçici çözümü, sanal makine profillerinin vCenter sunucusu yerine ESXi konağı ile oluşturulmasını içerir. Her ESXi konağı için bir örnek çalıştırmanız gerekir.

