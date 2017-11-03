---
title: "Azure yığın 1710 güncelleştirme (yapı 20171020.1) | Microsoft Docs"
description: "Azure yığın 1710 güncelleştirmesi nedir hakkında bilgi edinin tümleşik sistemleri, bilinen sorunlar ve güncelleştirme karşıdan yükleme konumu."
services: azure-stack
documentationcenter: 
author: twooley
manager: byronr
editor: 
ms.assetid: 135314fd-7add-4c8c-b02a-b03de93ee196
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/26/2017
ms.author: twooley
ms.openlocfilehash: d91a23ae4eb5aee14d3d2fef74467e7f33c458cc
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="azure-stack-1710-update-build-201710201"></a>Azure yığın 1710 güncelleştirme (yapı 20171020.1)

*Uygulandığı öğe: Azure yığın tümleşik sistemleri*

Bu makalede yenilikleri ve bilinen sorunlar bu sürüm ve güncelleştirmeyi yüklemek nereye Bu güncelleştirme paketi giderir. Bilinen sorunlar bilinen sorunlar bölünen doğrudan güncelleştirme işlemi ile ilgili ve bilinen sorunlar yapı (yükleme sonrası) sahip.

> [!IMPORTANT]
> Bu güncelleştirme paketi, yalnızca Azure tümleşik yığını sistemleri içindir. Bu güncelleştirme paketi Azure yığın Geliştirme Seti için geçerli değildir.

## <a name="improvements-and-fixes"></a>Geliştirmeleri ve düzeltmeler

Bu güncelleştirme aşağıdaki kalite ve düzeltmeler için Azure yığınına içerir.
 
### <a name="windows-server-2016-improvements-and-fixes"></a>Windows Server 2016 geliştirmeleri ve düzeltmeler

- Windows Server 2016 için güncelleştirmeler: 10 Ekim 2017 — KB4041691 (işletim sistemi yapı 14393.1770. Bkz: [https://support.microsoft.com/help/4041691](https://support.microsoft.com/help/4041691) daha fazla bilgi için.

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
|Bir güncelleştirme gerçekleştirmek, aşağıdakine benzer bir hata güncelleştirme eylem planını "Depolama ana bilgisayarları yeniden Depolama düğümü" adımı sırasında ortaya çıkabilir.<br><br>**{"name": "Yeniden depolama Hosts", "Açıklama": "Yeniden depolama konakları.", "errorMessage": "türü 'BareMetal' rolünün ' Restart' HostName-05 atlandı bir özel durum: \n\nThe bilgisayar oluşturulur. WMI hizmeti şu hata iletisiyle üzerinden kendi LastBootUpTime alınamadı: RPC sunucusu kullanılamıyor. (HRESULT özel durumu: 0x800706BA). \nat yeniden-ana bilgisayar** | Bu sorunun olası hatalı bir sürücüden bazı yapılandırmalarda mevcut kaynaklanır. | 1. Temel kart yönetim denetleyicisi (BMC) web arabirimine oturum açmak ve hata iletisinde tanımlanan ana bilgisayarı yeniden başlatın.<br><br>2. Güncelleştirme ayrıcalıklı uç nokta kullanarak devam edin. |
| Bir güncelleştirme gerçekleştirdiğinizde, güncelleştirme işlemi kabin ve ilerleme adımından sonra olmamasını görünüyor "Adım: adımı 2.4 - Güncelleştirme çalışıyorsa" güncelleştirme eylem planı.<br><br>Bu adım sonra kopyalama işlemi .nupkg dosyaların bir dizi tarafından iç altyapı dosya paylaşımlarına izler. Örneğin:<br><br>**1 dosyaları için \VirtualMachineName-ERCS03\C$\TraceCollectorUpdate\PerfCounterConfiguration content\PerfCollector\VirtualMachines kopyalama**  | Sorunu bir altyapı sanal makine ve bir sorun, Windows Server genişleme dosya sunucusu (bir sonraki güncelleştirmesinde teslim SOFS) disklerde doldurma günlük dosyalarını kaynaklanır. | Yardım için lütfen Microsoft Müşteri Hizmetleri ve desteği (CSS) başvurun. | 
| Bir güncelleştirme gerçekleştirdiğinizde, aşağıdakine benzer bir hata adımı sırasında oluşabilecek "Adım: adım 2.13.2 - güncelleştirme çalıştıran *VM_Name*" güncelleştirme eylem planı. (Sanal makine adı farklılık gösterebilir.)<br><br>**ActionPlanInstanceWarning ece/MachineName: WarningMessage:Task: 'Başarısız Cloud\Fabric\WAS' rolünün ' LiveUpdate' arabiriminin çağırma:<br>'WAS' rolünün 'LiveUpdate' türü bir özel durum oluşturuldu:<br>depolama sırasında bir hata oluştu başlatma: Microsoft Storage hizmetine bir API çağrısı yapma çalışırken bir hata oluştu: {"İletisi": "Service Fabric ile iletişim kurulurken bir zaman aşımı oluştu. Özel durum türü: TimeoutException. Özel durum iletisi: işlem zaman aşımına uğradı. "}**  | Sorun Windows Server'daki bir sonraki güncelleştirmesinde sabit bir g/ç zaman aşımı nedeniyle oluşur. | Yardım için Microsoft CSS temasa geçin.
| Bir güncelleştirme gerçekleştirdiğinizde, aşağıdakine benzer bir hata adımı sırasında "21 yeniden SQL server Vm'lerinin adım." oluşabilir<br><br>**'VirtualMachines' rolünün 'LiveUpdateRestart' türü bir özel durum oluşturuldu:<br>VerboseMessage: VM MachineName [VirtualMachines:LiveUpdateRestart] sorgulama-Sql01. - 13/10/2017 5:11:50 PM VerboseMessage: [VirtualMachines: LiveUpdateRestart] VM HighlyAvailable işaretlenir. -10/13/2017 5:11:50 PM VerboseMessage: [VirtualMachines:LiveUpdateRestart] konumundaki MS. MS adresindeki Internal.ServerClusters.ExceptionHelp.Build. Microsoft.FailoverClusters.PowerShell.StopClusterResourceCommand.BeginTimedOperation() adresindeki Internal.ServerClusters.ClusterResource.BeginTakeOffline (Boolean zorla) Microsoft.FailoverClusters.PowerShell.FCCmdlet.ProcessRecord() - 13/10/2017 5:11:50 PM WarningMessage:Task adresindeki Microsoft.FailoverClusters.PowerShell.TimedCmdlet.WrappedProcessRecord(): çağırma arabiriminin 'LiveUpdateRestart' rolünün ' Cloud\Fabric\VirtualMachines başarısız oldu:** | Sanal makine yeniden başlatamadı Bu sorun oluşabilir. | Yardım için Microsoft CSS temasa geçin.
| Bir güncelleştirme gerçekleştirdiğinizde, aşağıdakine benzer bir hata oluşabilir:<br><br>**2017-10-22T01:37:37.5369944Z 'SQL' rolünün 'Kapatma' türü bir özel durum oluşturuldu: duraklatma düğümü 's45r1004-Sql01' sitesindeki Stop-SQL, C:\ProgramData\SF\ErcsClusterNode2\Fabric\work\Applications\EnterpriseCloudEngineApplicationType_ bir hata oluştu App1\EnterpriseCloudEngineServicePkg.Code.1.0.597.18\CloudDeployment\Roles\SQL\SQL.psm1: 542at kapatma, C:\ProgramData\SF\ErcsClusterNode2\Fabric\work\Applications\EnterpriseCloudEngineApplicationType_App1\ satır EnterpriseCloudEngineServicePkg.Code.1.0.597.18\CloudDeployment\Classes\SQL\SQL.psm1: 50at satır <ScriptBlock>, <No file>: 18at satır <ScriptBlock>, <No file>: satır 16** | Sanal makine rolleri boşaltmak için askıya alınmış bir duruma yerleştirirseniz Bu sorun ortaya çıkabilir. | Yardım için Microsoft CSS temasa geçin.
| Bir güncelleştirme gerçekleştirdiğinizde, aşağıdaki hata oluşabilir:<br><br>**'ADFS' rolünün 'Doğrula' türü bir özel durum oluşturuldu: ADFS/grafik rol için doğrulama başarısız oldu, hata: ADFS araştırma endpoint denetlenirken hata *endpoint_URI*: "0" bağımsız değişkenle "GetResponse" çağrılırken özel durum: "uzak sunucu bir hata döndürdü: (503) sunucusu kullanılamıyor. "Invoke-ADFSGraphValidation adresindeki**<br><br>**'ADFS' rolünün 'Doğrula' türü bir özel durum oluşturuldu: ADFS/grafik rol için doğrulama başarısız oldu, hata: ADFS özellikleri alınırken hata oluştu: net.tcp://localhost için bağlanılamadı: 1500/ilkesi. Bağlantı denemesi 00:00:02.0498923 için bir zaman aralığı devam. TCP hata kodu 10061: hedef makine 127.0.0.1:1500 etkin olarak reddettiğinden bağlantı kurulamadı. Invoke-ADFSGraphValidation** | Güncelleştirme eylem planını Active Directory Federasyon Hizmetleri (AD FS) sistem durumu doğrulanamıyor. | Yardım için Microsoft CSS başvurun.

## <a name="known-issues-post-installation"></a>Bilinen sorunlar (yükleme sonrası)

Bu bölümde 20171020.1 yapı ile ilgili bilinen sorunlar yükleme sonrası içerir.

### <a name="portal"></a>Portal

- Yönetici portalı'nda işlem ve depolama kaynaklarını görüntülemek mümkün olmayabilir. Bu güncelleştirme yükleme sırasında bir hata oluştu ve güncelleştirme yanlış başarılı olarak bildirildi olduğunu gösterir. Bu sorun devam ederse, lütfen Yardım için Microsoft CSS başvurun.
- Boş bir portal panosunda görebilirsiniz. Pano kurtarmak için portalın sağ üst köşesindeki dişli simgesini seçin ve ardından **varsayılan ayarları geri**.
- Kullanıcılar bir abonelik olmadan tam Market göz atabilir ve planları ve teklifleri gibi yönetim öğelerini görebilirsiniz. Bu öğeler kullanıcılara işlevsiz.
- **Taşıma** bir kaynak grubu özelliklerini görüntülediğinizde düğmesi devre dışı. Bu davranış beklenir. Kaynak grupları abonelikler arasında taşıma şu anda desteklenmiyor.
- Aboneliğiniz Azure yığın Portal kullanımı için izinleri görüntülemek mümkün değildir. Geçici bir çözüm olarak izinleri PowerShell kullanarak doğrulayabilirsiniz.
-  Burada abonelik, kaynak grubu veya konum aşağı açılan listesinde seçtiğiniz herhangi bir iş akışı için bir veya daha fazla aşağıdaki sorunlarla karşılaşabilirsiniz:

   - Boş bir satır listesi üstündeki görebilirsiniz. Hala beklendiği gibi bir öğe seçin yapabiliyor olmanız gerekir.
   - Açılan listedeki öğeleri listesi kısaysa, öğe adlarının herhangi biri görmeye olmayabilir.
   - Birden çok kullanıcı aboneliğiniz varsa, kaynak grubu aşağı açılan listesi boş olabilir. 

   Son iki sorunlarını geçici olarak çözmek için abonelik veya kaynak grubu (biliyorsanız) adını yazın veya bunun yerine PowerShell kullanabilirsiniz.
  
### <a name="backup"></a>Backup

- Altyapı yedekleme üzerinde etkinleştirmeyin **altyapı yedekleme** dikey.

### <a name="health-and-monitoring"></a>Sistem durumu ve izleme

- Bir altyapı rol örneğini yeniden başlatırsanız, yeniden başlatma başarısız olduğunu belirten bir ileti alabilirsiniz. Bununla birlikte, yeniden başlatma gerçekten başarılı oldu.

### <a name="services"></a>Hizmetler

**Market**
- Kullanarak Azure yığın Market öğeleri eklemeye çalıştığınızda **azure'dan Ekle** seçeneği, tüm öğeleri olabilir indirme için görünür.
- Sanal makine ölçek kümeleri oluşturmak için hiçbir Market deneyim yoktur. Bir şablonu kullanarak bir ölçek oluşturabilirsiniz.
- Bunlar ilk Azure işlevlerine abonelikte oluşturmadan önce bir kiracı depolama kaynak sağlayıcısı kaydetmeniz gerekir.
- Kullanıcı abonelikleri yalnız bırakılmış kaynakları sonuçlarında siliniyor. Geçici bir çözüm olarak ilk kullanıcı kaynakları veya tüm kaynak grubunu silme ve kullanıcı abonelikleri silin. 

**SQL/MySQL**
- Bu yeni bir SQL veya MySQL SKU kiracılar veritabanları oluşturabilmeniz için önce bir saate kadar sürebilir. 
- Öğeleri doğrudan SQL ve MySQL kaynak sağlayıcısı tarafından gerçekleştirilen değil sunucularda barındırma oluşturulması desteklenmiyor ve eşleşmeyen bir duruma neden olabilir.

**İşlem**
- Kullanıcılar coğrafi olarak yedekli depolama ile sanal makine oluşturmak için seçeneği sunulur. Bu yapılandırma, sanal makine oluşturma başarısız olmasına neden olur.
- Yalnızca bir hata etki alanı ve bir bir güncelleştirme etki alanı ile bir sanal makine kullanılabilirlik yapılandırabilirsiniz.
 
**Ağ**
- Portalı kullanarak bir ortak IP adresi ile bir yük dengeleyicisi oluşturulamıyor. Geçici bir çözüm olarak, yük dengeleyici oluşturmak için PowerShell'i kullanabilirsiniz.
- Ağ Yük Dengeleyici oluşturduğunuzda, bir ağ adresi çevirisi (NAT) kuralı oluşturmanız gerekir. Bunu yapmazsanız, yük dengeleyici oluşturulduktan sonra bir NAT kuralı eklemeye çalıştığınızda bir hata alırsınız.
 
### <a name="field-replaceable-unit-fru-procedures"></a>Alan değiştirilebilen biriminin (FRU) yordamları

- Çevrimdışı yansımaları güncelleştirme çalışması sırasında düzeltme eki değil. Bir ölçek birimi düğümü değiştirmeniz gerekiyorsa, değiştirilen düğümü en son düzeltme eki düzeyine sahip olduğundan emin olmak için OEM donanım satıcınızla birlikte çalışır.

## <a name="download-the-update"></a>Güncelleştirme karşıdan yükle

1710 güncelleştirme paketini karşıdan [burada](https://aka.ms/azurestackupdatedownload).

## <a name="next-steps"></a>Sonraki adımlar

- Güncelleştirme yönetimi Azure yığınında genel bakış için bkz: [yönetmek Azure yığın genel bakış güncelleştirmelerinde](azure-stack-updates.md).
- Güncelleştirmeleri uygulama hakkında daha fazla bilgi için bkz: [Azure yığınında güncelleştirmelerini](azure-stack-apply-updates.md).