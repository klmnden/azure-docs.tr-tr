---
title: Azure Stack 1710 Güncelleştirmesi (derleme 20171020.1) | Microsoft Docs
description: Azure Stack 1710 güncelleştirmesi nedir hakkında bilgi edinin tümleşik sistemleri, bilinen sorunlar ve nerede güncelleştirmeyi indirin.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: 135314fd-7add-4c8c-b02a-b03de93ee196
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: brenduns
ms.reviewer: justini
ms.openlocfilehash: cf870551a3dbd9b5ea0ef6f886dc6451e43b2c25
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2018
ms.locfileid: "44377199"
---
# <a name="azure-stack-1710-update-build-201710201"></a>Azure Stack 1710 Güncelleştirmesi (derleme 20171020.1)

*İçin geçerlidir: Azure Stack tümleşik sistemleri*

Bu makalede geliştirmeleri açıklar ve bilinen sorunlar için bu sürüm ve güncelleştirme karşıdan yükleme konumu bu güncelleştirme paketini giderir. Bilinen sorunlar, bilinen bir sorunla karşılaşırsanız ayrılmıştır doğrudan güncelleştirme işlemiyle ilgili ve bilinen sorunlar yapı (yükleme sonrası) sahip.

> [!IMPORTANT]
> Yalnızca Azure Stack tümleşik sistemleri için bu güncelleştirme paketidir. Bu güncelleştirme paketini Azure Stack Geliştirme Seti için geçerli değildir.

## <a name="improvements-and-fixes"></a>İyileştirmeler ve düzeltmeler

Bu güncelleştirme, Azure Stack için aşağıdaki kalitesi geliştirmeleri ve düzeltmeleri içerir.
 
### <a name="windows-server-2016-improvements-and-fixes"></a>Windows Server 2016 iyileştirmeler ve düzeltmeler

- Windows Server 2016 güncelleştirmesi: 10 Ekim 2017 — KB4041691 (işletim sistemi derleme 14393.1770. Bkz: [ https://support.microsoft.com/help/4041691 ](https://support.microsoft.com/help/4041691) daha fazla bilgi için.

### <a name="additional-quality-improvements-and-fixes"></a>Ek kalitesi geliştirmeleri ve düzeltmeleri

- Sorun giderme ve Ağ Saati Protokolü (NTP) sunucuyu güncelleştirmek için PowerShell cmdlet'leri ayrıcalıklı uç noktası eklenir.
- Ayrıcalıklı uç nokta yeterli yönetim (JEA) uç noktası modüller ve cmdlet beyaz liste güncelleştiriliyor için destek eklendi. 
- Ayrıcalıklı uç noktasında yerel dil hatasını düzeltti.
- Ağ Geçidi kimlik bilgilerini döndürme özelliği eklendi.
- CBLocalAdmin yerel yönetici hesabı kaldırıldı. 
- Sinyal uyarı şablon içeriği sabit sinyal emin olmak için iş doğru bir güncelleştirmeden sonra uyarır.
- FRU işlemleri sırasında zaman aşımı ile dağıtılacak doku kaynak sağlayıcısını düzeltildi. 
- Azure Stack'te Azure Resource Manager API profillerini kullanmak bulut geliştiricileri için özelliği eklendi.
- Dağıtım sanal makinede (DVM) Windows Update hizmeti devre dışı. 
- Düğüm güç açık/kapalı Eylemler kullanıcı arabiriminden kaldırıldı.
- Çeşitli diğer performans ve kararlılık düzeltmeleri. 
 
## <a name="known-issues-with-the-update-process"></a>Güncelleştirme işlemi ile ilgili bilinen sorunlar

Bu bölüm, 1710 güncelleştirme yüklemesi sırasında karşılaşabileceğiniz bilinen sorunları içerir.

> [!IMPORTANT]
> Güncelleştirme başarısız olursa, daha sonra çalıştığınızda kullanmalısınız güncelleştirmeye devam et `Resume-AzureStackUpdate` ayrıcalıklı uç noktasından cmdlet'i. Güncelleştirme Yönetici portalını kullanarak devam. (Bu sürümündeki bilinen bir sorundur budur.) Daha fazla bilgi için [izleme ayrıcalıklı uç noktayı kullanarak Azure stack'teki güncelleştirmeleri](azure-stack-monitor-update.md).

| Belirti  | Nedeni  | Çözüm |
|---------|---------|---------|
|Bir güncelleştirme, aşağıdakine benzer bir hata gerçekleştirdiğinizde<br>"Depolama konakları yeniden Depolama düğümü" adımı sırasında ortaya çıkabilir<br> güncelleştirme eylem planı.<br><br>**{"name": "Yeniden depolama ana bilgisayarlar", "description": "yeniden<br> depolama konakları. errorMessage","": "yazın rolünün ' Restart'<br> 'BareMetal' yükseltilmiş bir özel durum: \n\n bilgisayar<br> HostName-05 atlandı. Kendi LastBootUpTime alınamadı<br> WMI hizmeti şu hata iletisiyle aracılığıyla:<br> RPC sunucusu kullanılamıyor.<br> (HRESULT özel durum: 0x800706BA). yeniden başlatma \nat ana bilgisayar** | Bu sorunu bazı yapılandırmalarda mevcut olası hatalı sürücü kaynaklanır. | 1. Temel Kart Yönetim denetleyicisine (BMC) web arabirimine oturum açın ve hata iletisinde tanımlanan ana bilgisayarı yeniden başlatın.<br><br>2. Ayrıcalıklı uç nokta kullanarak güncelleştirme sürdürün. |
| Güncelleştirme işlemi, bir güncelleştirme gerçekleştirdiğinizde, cep görünüyor<br> ve ilerleme adımından sonra olmamasını "Adım: adım 2.4 - yükleme çalıştırma<br> güncelleştirme eylem planını güncelleştir".<br><br>Bu adımın ardından .nupkg kopyalama işlemleri bir dizi tarafından izlenir<br> İç altyapı dosyaları dosya paylaşımları. Örneğin:<br><br>**1 dosyaları için content\PerfCollector\VirtualMachines kopyalayarak <br> \VirtualMachineName-ERCS03\C$\TraceCollectorUpdate\ <br>PerfCounterConfiguration**<br><br>Veya iletisini görürsünüz:<br><br>**WarningMessage:Task: Çağrı 'LiveUpdate' arabiriminin<br> 'Cloud\Fabric\VirtualMachines başarısız oldu' rolünün:<br> 'LiveUpdate' rolünün 'yükseltilmiş VirtualMachines' türü bir<br> özel durum: diskte yeterli alan yok .**  | Sorun, bir altyapı sanal makineye yükleyip bir sorun, Windows Server genişleme dosya sunucusu (bir sonraki güncelleştirmesinde teslim SOFS) disklerini doldurma günlük dosyalarını kaynaklanır. | Yardım için lütfen Microsoft Müşteri Hizmetleri ve desteği (CSS) başvurun. | 
| Bir güncelleştirme, aşağıdakine benzer bir hata gerçekleştirdiğinizde<br> adımı sırasında ortaya çıkabilecek "Adım: güncelleştirme 2.13.2 - adım çalıştırılıyor<br> *VM_Name*"planın güncelleştirme eylemi. (Sanal makine<br> ad farklılık gösterebilir.)<br><br>**ActionPlanInstanceWarning ece/MachineName:<br> WarningMessage:Task: çağrılmasını 'LiveUpdate' arabirim<br> rol 'Cloud\Fabric\WAS başarısız oldu': 'LiveUpdate' rolü yazın<br> harekete geçirilen özel durum 'Olan': sırasında hata OLUŞTU Depolama<br> başlatma: bir API yapılmaya çalışılırken bir hata oluştu<br> Microsoft Storage hizmetine çağrı: {"Message": "bir zaman aşımı<br> Service Fabric ile iletişim kurulurken hata oluştu.<br> Özel durum türü: TimeoutException.<br> Özel durum iletisi: işlem zaman aşımına uğradı. "}**  | Sorun, sonraki bir güncelleştirmede düzeltilecektir Windows Server'daki bir g/ç zaman aşımı nedeniyle oluşur. | Yardım için lütfen Microsoft CSS başvurun.
| Bir güncelleştirme, aşağıdakine benzer bir hata gerçekleştirdiğinizde<br> adımı sırasında "21 yeniden SQL server Vm'leri adım." oluşabilir<br><br>**'LiveUpdateRestart' rolünün 'yükseltilmiş VirtualMachines' türü bir<br> özel durum: VerboseMessage: [VirtualMachines:LiveUpdateRestart]<br> sorgulama için VM makineadı-Sql01. - 13/10/2017-17:11:50 VerboseMessage: [VirtualMachines: LiveUpdateRestart]<br> VM olarak HighlyAvailable. - 13/10/2017 işaretlenmiş-17:11:50<br> VerboseMessage: [VirtualMachines:LiveUpdateRestart] konumundaki<br>MS. Konumunda Internal.ServerClusters.ExceptionHelp.Build<br>MS. Internal.ServerClusters.ClusterResource.BeginTakeOffline<br>(Boolean zorla) adresindeki Microsoft.FailoverClusters.PowerShell.<br> Konumunda StopClusterResourceCommand.BeginTimedOperation() <br>Microsoft.FailoverClusters.PowerShell.TimedCmdlet.Wrapped<br>Microsoft.FailoverClusters.PowerShell adresindeki ProcessRecord().<br> FCCmdlet.ProcessRecord() - 13/10/2017-17:11:50 uyarı<br>ileti: Görev: çağrılmasını 'LiveUpdateRestart' arabirim<br> rol 'Cloud\Fabric\VirtualMachines' başarısız oldu:** | Sanal makineyi yeniden başlatamadı durumunda bu sorun oluşabilir. | Yardım için lütfen Microsoft CSS başvurun.
| Bir güncelleştirme işlemi yaptığınızda, aşağıdakine benzer bir hata oluşabilir:<br><br>**2017-10-22T01:37:37.5369944Z rol 'SQL' türü 'Kapatma'<br> bir özel durum bildirdi: duraklatma düğümü bir hata oluştu<br> 'Sql01 s45r1004'.komut Stop-SQL, C:\ProgramData\SF\ErcsClusterNode2 <br>\Fabric\work\ Applications\ EnterpriseCloud <br>EngineApplicationType&#95;App1\ <br>EnterpriseCloudEngineServicePkg.Code.1.0.597.18\ <br> CloudDeployment\Roles\SQL\SQL.psm1:line 542 adresindeki<br> Kapatma, C:\ProgramData\SF\ErcsClusterNode2\Fabric\work\ <br>uygulamaları \EnterpriseCloudEngineApplicationType&#95;App1\ <br>EnterpriseCloudEngineServicePkg.Code.1.0.597.18\Cloud<br> Deployment\Classes\SQL\SQL.psm1: 50 satır < ScriptBlock&#62;,<br> <No file>: 18 sırasında satır < ScriptBlock&#62;, < dosya&#62;: satır 16** | Sanal makine rolleri boşaltma için askıya alınmış durumda koyarsanız bu sorun oluşabilir. | Yardım için lütfen Microsoft CSS başvurun.
| Bir güncelleme gerçekleştirmeden, aşağıdaki hatalardan birini ya da meydana gelebilir:<br><br>**"ADFS' rolünün 'Doğrula' türü bir özel durum oluşturuldu: doğrulama<br> ADFS/grafik rol hatasıyla başarısız oldu: ADFS denetlenirken hata oluştu<br> araştırma uç noktası *endpoint_URI*: özel durum arama<br> " "0" bağımsız değişken ile GetResponse yanıtına":" uzak sunucu<br> bir hata döndürdü: (503) sunucusu kullanılamıyor. ", Invoke -<br>ADFSGraphValidation**<br><br>**'ADFS' rolünün 'Doğrula' türü bir özel durum oluşturuldu: doğrulama<br> ADFS/grafik rol hatasıyla başarısız oldu: hata getirilirken<br> ADFS özellikleri: bağlanamadı <br>NET.TCP://localhost: 1500/ilke. Bağlantı denemesi görüşmelerin Süren<br> 00:00:02.0498923 bir zaman aralığı için. TCP hata kodu<br> 10061: hiçbir bağlantı kurulamadı, çünkü hedef<br> makine etkin olarak reddedildi, 127.0.0.1:1500.<br> Invoke-ADFSGraphValidation konumunda** | Güncelleştirme eylem planını, Active Directory Federasyon Hizmetleri (AD FS) sistem durumu doğrulanamıyor. | Yardım için lütfen Microsoft CSS başvurun.

## <a name="known-issues-post-installation"></a>Bilinen sorunlar (yükleme sonrası)

Bu bölümde, yükleme sonrası 20171020.1 yapı ile ilgili bilinen sorunlar bulunmaktadır.

### <a name="portal"></a>Portal

- Yönetici portalı'nda işlem ve depolama kaynaklarını görüntülemek mümkün olmayabilir. Bu güncelleştirmenin yüklenmesi sırasında bir hata oluştuğunu ve güncelleştirme yanlış başarılı olarak bildirildi olduğunu gösterir. Bu sorun devam ederse Yardım için lütfen Microsoft CSS başvurun.
- Bir boş Pano portalında görebilirsiniz. Pano kurtarmak için portalın sağ üst köşesindeki dişli simgesini seçin ve ardından **varsayılan ayarları geri**.
- Bir kaynak grubu özelliklerini görüntülediğinizde **taşıma** düğmesi devre dışıdır. Bu davranış beklenmektedir. Kaynak grupları, abonelikler arasında taşıma şu anda desteklenmiyor.
- Burada abonelik, kaynak grubu veya konum aşağı açılan listesinde seçtiğiniz herhangi bir iş akışı için bir veya daha fazla aşağıdaki sorunlarla karşılaşabilirsiniz:

   - Listenin üstündeki boş bir satır görebilirsiniz. Hala beklendiği gibi bir öğe seçebilir olmalıdır.
   - Açılan listedeki öğelerin listesini kısa ise, herhangi bir öğe adları görüntülemek mümkün olmayabilir.
   - Birden çok kullanıcı aboneliğiniz varsa, kaynak grubu aşağı açılan liste boş olabilir. 

   Son iki sorun geçici olarak çözmek için abonelik veya kaynak grubu (biliyorsanız) adını yazmanız veya bunun yerine PowerShell kullanabilirsiniz.
- Kullanıcı abonelikleri sonuçlarında yalnız bırakılmış kaynakları siliniyor. Geçici bir çözüm olarak kullanıcı kaynaklar veya kaynak grubunun tamamını silin ve sonra kullanıcı abonelikleri silin.
- Aboneliğinizi, Azure Stack portalları kullanma izni görüntülemeniz mümkün değildir. Geçici çözüm olarak, PowerShell kullanarak izinleri doğrulayabilirsiniz.
- **Hizmet durumu** dikey başarısız yüklenemedi. Hizmet durumu dikey penceresi yönetici veya Kullanıcı Portalı'nda, Azure Stack açtığınızda, bir hata görüntüler ve bilgi yüklemez. Bu beklenen bir davranıştır. Seçin ve hizmetin sistem durumunu açık olsa da, bu özellik henüz kullanılamıyor, ancak Azure Stack gelecek bir sürümünde uygulanacaktır.
 

### <a name="backup"></a>Backup

- Yedekleme altyapısı üzerinde etkinleştirmeyin **altyapı yedeklemesine** dikey penceresi.

### <a name="health-and-monitoring"></a>Sistem durumu ve izleme

- Bir altyapı rol örneği yeniden başlatma, yeniden başlatma başarısız olduğunu belirten bir ileti alabilirsiniz. Ancak, gerçekte başlatma işlemi başarılı.

### <a name="marketplace"></a>Market
- İçin Azure Stack marketini kullanarak öğeleri eklemeye çalıştığınızda **Ekle azure'dan** seçeneği, tüm öğeleri olabilir indirme için görünür.
- Kullanıcılar, abone olmadan tüm markete göz atabilir ve planlar ve teklifler gibi yönetim öğelerini görebilirsiniz. Bu kullanıcılara işlevsel olmayan öğelerdir.

### <a name="compute"></a>İşlem
- Kullanıcılar coğrafi olarak yedekli depolama ile sanal makine oluşturmak için bu seçeneği sunulur. Bu yapılandırma, sanal makine oluşturmanın başarısız olmasına neden olur.
- Yalnızca bir hata etki alanı ve bir güncelleme etki alanı ile bir sanal makine kullanılabilirlik yapılandırabilirsiniz.
- Sanal makine ölçek kümeleri oluşturmak için herhangi bir Market deneyimi vardır. Bir ölçek kümesi şablon kullanarak oluşturabilirsiniz.
- Ölçeklendirme ayarları sanal makine ölçek kümeleri için portalda kullanılabilir değildir. Geçici çözüm olarak, kullandığınız [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). PowerShell sürüm farklar nedeniyle, kullanmanız gereken `-Name` yerine parametre `-VMScaleSetName`.
 
### <a name="networking"></a>Ağ
- Portalı kullanarak bir yük dengeleyici genel IP adresi ile oluşturulamıyor. Geçici çözüm olarak, yük dengeleyici oluşturmak için PowerShell kullanabilirsiniz.
- Ağ Yükü dengeleyici oluşturduğunuzda, bir ağ adresi çevirisi (NAT) kuralı oluşturmanız gerekir. Bunu yapmazsanız, Yük Dengeleyiciyi oluşturduktan sonra bir NAT kuralı eklemeye çalıştığınızda bir hata alırsınız.
- Sonra bir VM oluşturulur ve bu IP adresi ile ilişkili bir genel IP adresi bir sanal makineden (VM) ilişkisini olamaz. İlişkisi kaldırma çalışmaya görünür, ancak daha önce atanan genel IP adresi orijinal VM ile ilişkili olarak kalır. Yeni bir VM için IP adresi yeniden atama olsa bile bu durumda (genellikle olarak adlandırılan bir *VIP takas*). Tüm gelecek bu IP adresi sonuç başlangıçta ilişkili VM ve yeni bir bağlantı üzerinden bağlanma girişiminde bulunur. Şu anda yeni VM oluşturma için yalnızca yeni ortak IP adreslerini kullanmanız gerekir.
 
### <a name="sqlmysql"></a>SQL/MySQL
- Bu yeni bir SQL veya MySQL SKU kiracılar veritabanları oluşturabilmeniz için önce bir saate kadar sürebilir. 
- Öğeleri doğrudan SQL ve MySQL kaynak sağlayıcısı tarafından gerçekleştirilmeyen sunucuları barındırma oluşturulması desteklenmez ve eşleşmeyen bir duruma neden olabilir.
 
### <a name="app-service"></a>App Service
- Bir kullanıcı, abonelikte, ilk Azure işlevinizi oluşturmadan depolama kaynak sağlayıcısını kaydetmeniz gerekir.
 
### <a name="field-replaceable-unit-fru-procedures"></a>Alan bulunduğu yerde değiştirilebilen biriminin (FRU) yordamları

- Güncelleştirme çalışması sırasında çevrimdışı görüntüleri yama yok. Bir ölçek birimi düğüm değiştirmeniz gerekiyorsa, değiştirilen düğümü en son düzeltme eki düzeyine sahip olduğundan emin olmak için OEM donanım satıcınızla çalışır.

## <a name="download-the-update"></a>Güncelleştirmeyi indirin

1710 güncelleştirme paketini indirebileceğiniz [burada](https://aka.ms/azurestackupdatedownload).

## <a name="next-steps"></a>Sonraki adımlar

- Azure Stack'te güncelleştirme yönetimi genel bakış için bkz. [Azure Stack genel bakış güncelleştirmeleri yönetme](azure-stack-updates.md).
- Uygulama güncelleştirmeleri hakkında daha fazla bilgi için bkz: [güncelleştirmelerini Azure Stack'te](azure-stack-apply-updates.md).
