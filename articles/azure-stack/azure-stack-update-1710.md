---
title: Azure yığın 1710 güncelleştirme (yapı 20171020.1) | Microsoft Docs
description: Azure yığın 1710 güncelleştirmesi nedir hakkında bilgi edinin tümleşik sistemleri, bilinen sorunlar ve güncelleştirme karşıdan yükleme konumu.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 135314fd-7add-4c8c-b02a-b03de93ee196
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: mabrigg
ms.openlocfilehash: 8c7c39ecdc332c994e5c00f8415462f208e7d20b
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
ms.locfileid: "30311933"
---
# <a name="azure-stack-1710-update-build-201710201"></a>Azure yığın 1710 güncelleştirme (yapı 20171020.1)

*Uygulandığı öğe: Azure yığın tümleşik sistemleri*

Bu makalede yenilikleri ve bilinen sorunlar bu sürüm ve güncelleştirmeyi yüklemek nereye Bu güncelleştirme paketi giderir. Bilinen sorunlar bilinen sorunlar bölünen doğrudan güncelleştirme işlemi ile ilgili ve bilinen sorunlar yapı (yükleme sonrası) sahip.

> [!IMPORTANT]
> Bu güncelleştirme paketi, yalnızca Azure tümleşik yığını sistemleri içindir. Bu güncelleştirme paketi Azure yığın Geliştirme Seti için geçerli değildir.

## <a name="improvements-and-fixes"></a>Geliştirmeleri ve düzeltmeler

Bu güncelleştirme aşağıdaki kalite ve düzeltmeler için Azure yığınına içerir.
 
### <a name="windows-server-2016-improvements-and-fixes"></a>Windows Server 2016 geliştirmeleri ve düzeltmeler

- Windows Server 2016 için güncelleştirmeler: 10 Ekim 2017 — KB4041691 (işletim sistemi yapı 14393.1770. Bkz: [ https://support.microsoft.com/help/4041691 ](https://support.microsoft.com/help/4041691) daha fazla bilgi için.

### <a name="additional-quality-improvements-and-fixes"></a>Ek kalite ve düzeltmeler

- Sorun giderme ve güncelleştirme ağ zaman Protokolü (NTP) sunucusu yardımcı olmak için PowerShell cmdlet'leri ayrıcalıklı uç nokta eklenir.
- Ayrıcalıklı uç nokta yalnızca yetecek kadar Yönetim (JEA) uç nokta modülleri ve cmdlet beyaz liste güncelleştirmek için destek eklendi. 
- Ayrıcalıklı uç yerel dil hatasını düzeltti.
- Ağ Geçidi kimlik bilgileri döndürmek için özelliği eklenmiştir.
- CBLocalAdmin yerel yönetici hesabı kaldırıldı. 
- Sinyal uyarı şablon içeriği sabit sinyal emin olmak için iş doğru bir güncelleştirme sonrası sizi uyarır.
- FRU işlemleri sırasında zaman aşımı ile mücadele etmek için doku kaynak sağlayıcısı sabit. 
- Bulut geliştiricilerin Azure yığında Azure Resource Manager API profillerini kullanmak özelliği eklenmiştir.
- Windows Update hizmeti (DVM) dağıtım sanal makinede devre dışı. 
- Düğüm güç açık/kapalı Eylemler kullanıcı arabiriminden kaldırıldı.
- Çeşitli diğer performans ve kararlılık giderir. 
 
## <a name="known-issues-with-the-update-process"></a>Güncelleştirme işlemi ile ilgili bilinen sorunlar

Bu bölüm, 1710 güncelleştirme yüklemesi sırasında karşılaşabileceğiniz bilinen sorunları içerir.

> [!IMPORTANT]
> Güncelleştirmesi başarısız olursa, daha sonra çalıştığınızda kullanmalısınız güncelleştirmeye devam et `Resume-AzureStackUpdate` ayrıcalıklı uç noktasından cmdlet'i. Güncelleştirme Yönetici portalı'nı kullanarak devam. (Bu sürümündeki bilinen bir sorundur budur.) Daha fazla bilgi için bkz: [izlemek Azure kullanan ayrıcalıklı uç noktasını yığınında güncelleştirmeleri](azure-stack-monitor-update.md).

| Belirti  | Nedeni  | Çözüm |
|---------|---------|---------|
|Bir güncelleştirme, aşağıdakine benzer bir hata gerçekleştirdiğinizde<br>"Depolama ana bilgisayarları yeniden Depolama düğümü" adımı sırasında ortaya çıkabilir<br> güncelleştirme eylem planını.<br><br>**{"name": "Yeniden depolama Hosts", "Açıklama": "yeniden<br> depolama konakları.","errorMessage": "yazın rolünün ' Restart'<br> 'BareMetal' yükseltilmiş bir özel durum: \n\n bilgisayar<br> HostName-05 atlandı. Kendi LastBootUpTime alınamadı<br> WMI hizmeti şu hata iletisiyle üzerinden:<br> RPC sunucusu kullanılamıyor.<br> (HRESULT özel durumu: 0x800706BA). \nat yeniden-ana bilgisayar** | Bu sorunun olası hatalı bir sürücüden bazı yapılandırmalarda mevcut kaynaklanır. | 1. Temel kart yönetim denetleyicisi (BMC) web arabirimine oturum açmak ve hata iletisinde tanımlanan ana bilgisayarı yeniden başlatın.<br><br>2. Güncelleştirme ayrıcalıklı uç nokta kullanarak devam edin. |
| Bir güncelleştirme gerçekleştirdiğinizde, güncelleştirme işlemi kabin görünüyor<br> ve ilerleme adımından sonra olmamasını "Adım: adım 2.4 - yükleme çalıştırma<br> güncelleştirme eylem planını güncelleştir".<br><br>Bu adım sonra .nupkg kopyalama işlemlerinin bir dizi tarafından izlenir<br> İç altyapı dosya paylaşımlarına dosyalar. Örneğin:<br><br>**1 dosyaları için content\PerfCollector\VirtualMachines kopyalayarak <br> \VirtualMachineName-ERCS03\C$\TraceCollectorUpdate\ <br>PerfCounterConfiguration**<br><br>Veya, iletiyi görürsünüz:<br><br>**WarningMessage:Task: 'LiveUpdate' arabiriminin çağırma<br> 'başarısız Cloud\Fabric\VirtualMachines' rolünün:<br> 'LiveUpdate' rolünün 'yükseltilmiş VirtualMachines' türü bir<br> özel durum: disk üzerinde yeterli alan yok .**  | Sorunu bir altyapı sanal makine ve bir sorun, Windows Server genişleme dosya sunucusu (bir sonraki güncelleştirmesinde teslim SOFS) disklerde doldurma günlük dosyalarını kaynaklanır. | Yardım için lütfen Microsoft Müşteri Hizmetleri ve desteği (CSS) başvurun. | 
| Bir güncelleştirme, aşağıdakine benzer bir hata gerçekleştirdiğinizde<br> adım sırasında ortaya çıkabilecek "Adım: adım 2.13.2 - güncelleştirme çalıştırma<br> *VM_Name*"planının güncelleştirme eylem. (Sanal makine<br> adı değişebilir.)<br><br>**ActionPlanInstanceWarning ece/MachineName:<br> WarningMessage:Task: çağrıldı 'LiveUpdate' arabirim<br> rolü 'başarısız Cloud\Fabric\WAS': 'LiveUpdate' rolü yazın<br> '' yükseltildi bir özel durum: sırasında bir hata oluştu Depolama<br> başlatma: bir API yapmaya çalışırken bir hata oluştu<br> Microsoft Storage hizmetine çağrı: {"İletisi": "zaman aşımı<br> Service Fabric ile iletişim kurulurken oluştu.<br> Özel durum türü: TimeoutException.<br> Özel durum iletisi: işlem zaman aşımına uğradı. "}**  | Sorun Windows Server'daki bir sonraki güncelleştirmesinde sabit bir g/ç zaman aşımı nedeniyle oluşur. | Yardım için Microsoft CSS temasa geçin.
| Bir güncelleştirme, aşağıdakine benzer bir hata gerçekleştirdiğinizde<br> adımı sırasında "21 yeniden SQL server Vm'lerinin adım." oluşabilir<br><br>**'LiveUpdateRestart' rolünün 'yükseltilmiş VirtualMachines' türü bir<br> özel durum: VerboseMessage: [VirtualMachines:LiveUpdateRestart]<br> VM MachineName için sorgulama-Sql01. - 13/10/2017 5:11:50 PM VerboseMessage: [VirtualMachines: LiveUpdateRestart]<br> VM olarak HighlyAvailable. - 13/10/2017 işaretlenmiş 5:11:50 PM<br> VerboseMessage: [VirtualMachines:LiveUpdateRestart] konumundaki<br>MS. Konumundaki Internal.ServerClusters.ExceptionHelp.Build<br>MS. Internal.ServerClusters.ClusterResource.BeginTakeOffline<br>(Boolean zorla) Microsoft.FailoverClusters.PowerShell adresindeki.<br> Konumundaki StopClusterResourceCommand.BeginTimedOperation() <br>Microsoft.FailoverClusters.PowerShell.TimedCmdlet.Wrapped<br>ProcessRecord() Microsoft.FailoverClusters.PowerShell adresindeki.<br> FCCmdlet.ProcessRecord() - 13/10/2017 5:11:50 PM uyarı<br>ileti: Görev: çağrıldı 'LiveUpdateRestart' arabirim<br> rolü 'Cloud\Fabric\VirtualMachines' başarısız oldu:** | Sanal makine yeniden başlatamadı Bu sorun oluşabilir. | Yardım için Microsoft CSS temasa geçin.
| Bir güncelleştirme gerçekleştirdiğinizde, aşağıdakine benzer bir hata oluşabilir:<br><br>**2017-10-22T01:37:37.5369944Z türü 'Kapatma' rolünün 'SQL'<br> özel durum oluşturuldu: duraklatma düğüm bir hata oluştu<br> 's45r1004-Sql01' sitesindeki Stop-SQL, C:\ProgramData\SF\ErcsClusterNode2 <br>\Fabric\work\ Applications\ EnterpriseCloud <br>EngineApplicationType&#95;App1\ <br>EnterpriseCloudEngineServicePkg.Code.1.0.597.18\ <br> CloudDeployment\Roles\SQL\SQL.psm1:line 542 adresindeki<br> Kapatma, C:\ProgramData\SF\ErcsClusterNode2\Fabric\work\ <br>uygulamaları \EnterpriseCloudEngineApplicationType&#95;App1\ <br>EnterpriseCloudEngineServicePkg.Code.1.0.597.18\Cloud<br> Deployment\Classes\SQL\SQL.psm1: 50 satır < ScriptBlock&#62;,<br> <No file>: 18 konumundaki satır < ScriptBlock&#62;, < hiçbir dosya&#62;: satır 16** | Sanal makine rolleri boşaltmak için askıya alınmış bir duruma yerleştirirseniz Bu sorun ortaya çıkabilir. | Yardım için Microsoft CSS temasa geçin.
| Bir güncelleştirme gerçekleştirdiğinizde, aşağıdaki hata oluşabilir:<br><br>**"ADFS' rolünün 'Doğrula' türü bir özel durum oluşturuldu: doğrulama<br> ADFS/grafik rolü hatasıyla başarısız oldu: hata ADFS denetimini<br> endpoint araştırma *endpoint_URI*: özel durum arama<br> " "0" bağımsız değişkenli ile GetResponse":" uzak sunucu<br> bir hata döndürdü: (503) sunucusu kullanılamıyor. "konumundaki Invoke -<br>ADFSGraphValidation**<br><br>**'ADFS' rolünün 'Doğrula' türü bir özel durum oluşturuldu: doğrulama<br> ADFS/grafik rolü hatasıyla başarısız oldu: alınırken hata<br> ADFS özellikleri: bağlanılamadı <br>net.tcp://localhost: 1500/ilkesi. Bağlantı denemesi devam<br> 00:00:02.0498923, bir zaman aralığı için. TCP hata kodu<br> 10061: hiçbir bağlantı kurulamadı, çünkü hedef<br> makine etkin olarak reddetti onu 127.0.0.1:1500.<br> Invoke-ADFSGraphValidation adresindeki** | Güncelleştirme eylem planını Active Directory Federasyon Hizmetleri (AD FS) sistem durumu doğrulanamıyor. | Yardım için Microsoft CSS temasa geçin.

## <a name="known-issues-post-installation"></a>Bilinen sorunlar (yükleme sonrası)

Bu bölümde 20171020.1 yapı ile ilgili bilinen sorunlar yükleme sonrası içerir.

### <a name="portal"></a>Portal

- Yönetici portalı'nda işlem ve depolama kaynaklarını görüntülemek mümkün olmayabilir. Bu güncelleştirme yükleme sırasında bir hata oluştu ve güncelleştirme yanlış başarılı olarak bildirildi olduğunu gösterir. Bu sorun devam ederse, lütfen Yardım için Microsoft CSS başvurun.
- Boş bir portal panosunda görebilirsiniz. Pano kurtarmak için portalın sağ üst köşesindeki dişli simgesini seçin ve ardından **varsayılan ayarları geri**.
- Bir kaynak grubu özelliklerini görüntülediğinizde **taşıma** düğmesi devre dışıdır. Bu davranış beklenir. Kaynak grupları abonelikler arasında taşıma şu anda desteklenmiyor.
- Burada abonelik, kaynak grubu veya konum aşağı açılan listesinde seçtiğiniz herhangi bir iş akışı için bir veya daha fazla aşağıdaki sorunlarla karşılaşabilirsiniz:

   - Boş bir satır listesi üstündeki görebilirsiniz. Hala beklendiği gibi bir öğe seçin yapabiliyor olmanız gerekir.
   - Açılan listedeki öğeleri listesi kısaysa, öğe adlarının herhangi biri görmeye olmayabilir.
   - Birden çok kullanıcı aboneliğiniz varsa, kaynak grubu aşağı açılan listesi boş olabilir. 

   Son iki sorunlarını geçici olarak çözmek için abonelik veya kaynak grubu (biliyorsanız) adını yazın veya bunun yerine PowerShell kullanabilirsiniz.
- Kullanıcı abonelikleri yalnız bırakılmış kaynakları sonuçlarında siliniyor. Geçici bir çözüm olarak ilk kullanıcı kaynakları veya tüm kaynak grubunu silme ve kullanıcı abonelikleri silin.
- Aboneliğiniz Azure yığın Portal kullanımı için izinleri görüntülemek mümkün değildir. Geçici bir çözüm olarak izinleri PowerShell kullanarak doğrulayabilirsiniz.
- **Hizmet durumu** dikey başarısız yüklenemiyor. Yönetici veya Kullanıcı Portalı'nda Azure yığın hizmet durumu dikey penceresini açtığınızda bir hata görüntüler ve bilgi yüklemez. Bu beklenen bir davranıştır. Seçin ve hizmetin sistem durumunu açın, ancak bu özellik henüz kullanılabilir değil ancak Azure yığın gelecek bir sürümünde uygulanacaktır.
 

### <a name="backup"></a>Backup

- Altyapı yedekleme üzerinde etkinleştirmeyin **altyapı yedekleme** dikey.

### <a name="health-and-monitoring"></a>Sistem durumu ve izleme

- Bir altyapı rol örneğini yeniden başlatırsanız, yeniden başlatma başarısız olduğunu belirten bir ileti alabilirsiniz. Bununla birlikte, yeniden başlatma gerçekten başarılı oldu.

### <a name="marketplace"></a>Market
- Kullanarak Azure yığın Market öğeleri eklemeye çalıştığınızda **azure'dan Ekle** seçeneği, tüm öğeleri olabilir indirme için görünür.
- Kullanıcılar bir abonelik olmadan tam Market göz atabilir ve planları ve teklifleri gibi yönetim öğelerini görebilirsiniz. Bu öğeler kullanıcılara işlevsiz.

### <a name="compute"></a>İşlem
- Kullanıcılar coğrafi olarak yedekli depolama ile sanal makine oluşturmak için seçeneği sunulur. Bu yapılandırma, sanal makine oluşturma başarısız olmasına neden olur.
- Yalnızca bir hata etki alanı ve bir bir güncelleştirme etki alanı ile bir sanal makine kullanılabilirlik yapılandırabilirsiniz.
- Sanal makine ölçek kümeleri oluşturmak için hiçbir Market deneyim yoktur. Bir şablonu kullanarak bir ölçek oluşturabilirsiniz.
- Sanal makine ölçek kümeleri için ölçeklendirme ayarları portalda kullanılabilir değildir. Geçici bir çözüm olarak, kullandığınız [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). PowerShell sürümü farklılıkları nedeniyle kullanmalısınız `-Name` yerine parametre `-VMScaleSetName`.
 
### <a name="networking"></a>Ağ
- Portalı kullanarak bir ortak IP adresi ile bir yük dengeleyicisi oluşturulamıyor. Geçici bir çözüm olarak, yük dengeleyici oluşturmak için PowerShell'i kullanabilirsiniz.
- Ağ Yük Dengeleyici oluşturduğunuzda, bir ağ adresi çevirisi (NAT) kuralı oluşturmanız gerekir. Bunu yapmazsanız, yük dengeleyici oluşturulduktan sonra bir NAT kuralı eklemeye çalıştığınızda bir hata alırsınız.
- VM oluşturulur ve bu IP adresi ile ilişkili sonra bir sanal makineden (VM) genel bir IP adresi ilişkisini olamaz. Disassociation çalışmak için görünür, ancak daha önce atanan genel IP adresi orijinal VM ile ilişkili olarak kalır. Yeni bir VM için IP adresi yeniden atama olsa bile bu davranış oluşur (genellikle olarak adlandırılan bir *VIP takası*). Tüm gelecekte bu IP adresi sonucu başlangıçta ilişkili VM değil de yeni bir bağlantı üzerinden bağlanma girişiminde bulunur. Şu anda, yeni VM oluşturmak için yalnızca yeni ortak IP adreslerini kullanmanız gerekir.
 
### <a name="sqlmysql"></a>SQL/MySQL
- Bu yeni bir SQL veya MySQL SKU kiracılar veritabanları oluşturabilmeniz için önce bir saate kadar sürebilir. 
- Öğeleri doğrudan SQL ve MySQL kaynak sağlayıcısı tarafından gerçekleştirilen değil sunucularda barındırma oluşturulması desteklenmiyor ve eşleşmeyen bir duruma neden olabilir.
 
### <a name="app-service"></a>App Service
- Bir kullanıcı, bunlar ilk Azure işlevlerine abonelikte oluşturmadan önce depolama kaynak sağlayıcısı kaydetmeniz gerekir.
 
### <a name="field-replaceable-unit-fru-procedures"></a>Alan değiştirilebilen biriminin (FRU) yordamları

- Çevrimdışı yansımaları güncelleştirme çalışması sırasında düzeltme eki değil. Bir ölçek birimi düğümü değiştirmeniz gerekiyorsa, değiştirilen düğümü en son düzeltme eki düzeyine sahip olduğundan emin olmak için OEM donanım satıcınızla birlikte çalışır.

## <a name="download-the-update"></a>Güncelleştirme karşıdan yükle

1710 güncelleştirme paketini karşıdan [burada](https://aka.ms/azurestackupdatedownload).

## <a name="next-steps"></a>Sonraki adımlar

- Güncelleştirme yönetimi Azure yığınında genel bakış için bkz: [yönetmek Azure yığın genel bakış güncelleştirmelerinde](azure-stack-updates.md).
- Güncelleştirmeleri uygulama hakkında daha fazla bilgi için bkz: [Azure yığınında güncelleştirmelerini](azure-stack-apply-updates.md).
