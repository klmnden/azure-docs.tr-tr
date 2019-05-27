---
title: Azure Site Recovery işlem sunucusu sorunlarını giderme
description: Bu makalede Azure Site Recovery işlem sunucusu ile ilgili sorunları giderme
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: troubleshooting
ms.date: 04/29/2019
ms.author: raynew
ms.openlocfilehash: 6e31308800f72d60381f1e4ecd540482ba263851
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65969360"
---
# <a name="troubleshoot-the-process-server"></a>İşlem Sunucusu sorunlarını giderme

[Site Recovery](site-recovery-overview.md) azure'a olağanüstü durum kurtarmaya şirket içi VMware Vm'leri ve fiziksel sunucular için ayarladığınızda, işlem sunucusu kullanılır. Bu makalede, çoğaltma ve bağlantı sorunları da dahil olmak üzere, bir işlem sunucusu ile ilgili sorunları giderme açıklar.

[Daha fazla bilgi edinin](vmware-physical-azure-config-process-server-overview.md) işlem sunucusu hakkında.

## <a name="before-you-start"></a>Başlamadan önce

Sorunlarını gidermeye başlamadan önce:

1. Anladığınızdan emin olmak için nasıl [izleme işlem sunucularının](vmware-physical-azure-monitor-process-server.md).
2. Aşağıdaki en iyi uygulamaları gözden geçirin.
3. İzlediğinizden emin olun [kapasiteyle alakalı durumlar](site-recovery-plan-capacity-vmware.md#capacity-considerations)ve boyutlandırma kılavuzluğu için [yapılandırma sunucusu](site-recovery-plan-capacity-vmware.md#size-recommendations-for-the-configuration-server-and-inbuilt-process-server) veya [tek başına işlem sunucularının](site-recovery-plan-capacity-vmware.md#size-recommendations-for-the-process-server).

## <a name="best-practices-for-process-server-deployment"></a>İşlem sunucusu dağıtımı için en iyi uygulamalar

İşlem sunucusu en iyi performans için biz birkaç genel en iyi özetlenmiş.

**En iyi uygulama** | **Ayrıntılar**
--- |---
**Kullanım** | Yapılandırma sunucusu/tek başına işlem sunucusu yalnızca hedeflenen amaç için kullanılan emin olun. Başka bir şey makine üzerinde çalışmıyor.
**IP adresi** | İşlem sunucusu statik bir IPv4 adresi vardır ve NAT yapılandırılmış yok emin olun.
**Bellek/CPU kullanımını denetleme** |CPU ve bellek kullanımı % 70'in altında tutun.
**Boş alan emin olun** | İşlem sunucusu önbellek disk alanı boş alan gösterir. Azure'a yüklenmiş önce çoğaltma verilerinin önbellekte depolanır.<br/><br/> Boş alan % 25 üzerinde tutun. %20 altına aşması durumunda, çoğaltma işlem sunucusu ile ilişkili olan çoğaltılan makineler için kısıtlanır.

## <a name="check-process-server-health"></a>İşlem sunucusunun sistem durumunu denetleyin

Sorun giderme ilk adımı, durumunu ve işlem sunucusu durumunu denetlemektir. Bunu yapmak için tüm uyarıları gözden geçirin, gerekli Hizmetleri çalıştıran ve işlem sunucusundan sinyal olduğundan emin olun. Aşağıdaki grafikte, şu adımları özetlenmiştir adımları yardımcı olacak yordamlar tarafından izlenen.

![İşlem sunucusu durumu sorunlarını giderme](./media/vmware-physical-azure-troubleshoot-process-server/troubleshoot-process-server-health.png)

## <a name="step-1-troubleshoot-process-server-health-alerts"></a>1. Adım: Sorun giderme işlemi sunucu sistem durumu uyarıları

İşlem sunucusu, bir dizi sistem durumu uyarıları oluşturur. Bu uyarıları ve önerilen eylemleri aşağıdaki tabloda özetlenmiştir.

**Uyarı türü** | **Hata:** | **Sorun giderme**
--- | --- | --- 
![İyi Durumda][green] | None  | İşlem sunucusu bağlı ve iyi durumda.
![Uyarı][yellow] | Belirtilen Hizmetleri çalıştırmıyor. | 1. Hizmetlerinin çalıştığını kontrol edin.<br/> 2. Hizmetleri beklendiği şekilde çalışıyor, aşağıdaki yönergeleri izleyin. [bağlantı ve çoğaltma sorunlarını giderme](#check-connectivity-and-replication).
![Uyarı][yellow]  | CPU kullanımı > % 80'in son 15 dakika. | 1. Yeni makineleri eklemeyin.<br/>2. İşlem sunucusunu kullanan VM sayısına hizalar onay [sınırları tanımlanmış](site-recovery-plan-capacity-vmware.md#capacity-considerations)ve kurmayı göz önünde bulundurun bir [ek işlem sunucusu](vmware-azure-set-up-process-server-scale.md).<br/>3. Aşağıdaki yönergeleri [bağlantı ve çoğaltma sorunlarını giderme](#check-connectivity-and-replication).
![Kritik][red] |  Son 15 dakika için CPU kullanımı > %95. | 1. Yeni makineleri eklemeyin.<br/>2. İşlem sunucusunu kullanan VM sayısına hizalar onay [sınırları tanımlanmış](site-recovery-plan-capacity-vmware.md#capacity-considerations)ve kurmayı göz önünde bulundurun bir [ek işlem sunucusu](vmware-azure-set-up-process-server-scale.md).<br/>3. Aşağıdaki yönergeleri [bağlantı ve çoğaltma sorunlarını giderme](#check-connectivity-and-replication).<br/> 4. Sorun devam ederse çalıştırma [dağıtım Planlayıcısı](https://aka.ms/asr-v2a-deployment-planner) VMware/fiziksel sunucu çoğaltma için.
![Uyarı][yellow] | Bellek kullanımı > % 80'in son 15 dakika. |  1. Yeni makineleri eklemeyin.<br/>2. İşlem sunucusunu kullanan VM sayısına hizalar onay [sınırları tanımlanmış](site-recovery-plan-capacity-vmware.md#capacity-considerations)ve kurmayı göz önünde bulundurun bir [ek işlem sunucusu](vmware-azure-set-up-process-server-scale.md).<br/>3. Uyarıyla ilişkili tüm yönergeleri izleyin.<br/> 4. Sorun devam ederse, için aşağıdaki yönergeleri izleyerek [bağlantı ve çoğaltma sorunlarını giderme](#check-connectivity-and-replication).
![Kritik][red] | Bellek kullanımı > %95 son 15 dakika içinde. | 1. Yeni makineleri ekleme ve ayarlama dikkate bir [ek işlem sunucusu](vmware-azure-set-up-process-server-scale.md).<br/> 2. Uyarıyla ilişkili tüm yönergeleri izleyin.<br/> 3. 4. Sorun devam ederse, için aşağıdaki yönergeleri izleyerek [bağlantı ve çoğaltma sorunlarını giderme](#check-connectivity-and-replication).<br/> 4. Sorun devam ederse çalıştırma [dağıtım Planlayıcısı](https://aka.ms/asr-v2a-deployment-planner) VMware/fiziksel sunucu çoğaltma sorunları için.
![Uyarı][yellow] | Önbellek klasörü boş alan < % 30 son 15 dakika içinde. | 1. Yoksa yeni makineleri ekleyin ve kurmayı göz önünde bulundurun bir [ek işlem sunucusu](vmware-azure-set-up-process-server-scale.md).<br/>2. İşlem sunucusunu kullanan VM sayısına hizalar onay [yönergeleri](site-recovery-plan-capacity-vmware.md#capacity-considerations).<br/> 3. Aşağıdaki yönergeleri [bağlantı ve çoğaltma sorunlarını giderme](#check-connectivity-and-replication).
![Kritik][red] |  Son 15 dakika boyunca % boş alan < 25 | 1. Bu sorun için bir uyarı ile ilgili yönergeleri izleyin.<br/> 2. 3. Aşağıdaki yönergeleri [bağlantı ve çoğaltma sorunlarını giderme](#check-connectivity-and-replication).<br/> 3. Sorun devam ederse çalıştırma [dağıtım Planlayıcısı](https://aka.ms/asr-v2a-deployment-planner) VMware/fiziksel sunucu çoğaltma için.
![Kritik][red] | 15 dakika veya daha fazla işlem sunucusundan sinyal alınmadı. Tmansvs hizmet yapılandırma sunucusuyla iletişim kurmuyor. | (1) işlem sunucusu çalışmaya olup olmadığını denetleyin.<br/> 2. Tmassvc işlem sunucusunda çalıştığından emin olun.<br/> 3. Aşağıdaki yönergeleri [bağlantı ve çoğaltma sorunlarını giderme](#check-connectivity-and-replication).


![Tablo anahtarı](./media/vmware-physical-azure-troubleshoot-process-server/table-key.png)


## <a name="step-2-check-process-server-services"></a>2. Adım: İşlem sunucusu Hizmetleri denetleme

İşlem sunucusu üzerinde çalışan hizmetler aşağıdaki tabloda özetlenmiştir. İşlem sunucusunu nasıl dağıtıldığını bağlı Hizmetleri, küçük farklılıklar vardır. 

StartType ayarlamak Microsoft Azure kurtarma Hizmetleri Aracısı'nı (obengine) dışındaki tüm hizmetler için denetleme **otomatik** veya **otomatik (Gecikmeli Başlatma)**.
 
**Dağıtım** | **Çalışan hizmetler**
--- | ---
**İşlem sunucusu olarak yapılandırma sunucusuna** | Dosya; ProcessServerMonitor; cxprocessserver; Inmage Pushınstall; Günlük karşıya yükleme hizmeti (LogUpload); Inmage Scout uygulama hizmeti; Microsoft Azure kurtarma Hizmetleri Aracısı (obengine); Inmage Scout VX Aracısı-Sentinel/Outpost (svagents); tmansvc; World Wide Web Yayımlama Hizmeti (W3SVC); MySQL; Microsoft Azure Site Recovery hizmeti (dra)
**İşlem sunucusu tek başına sunucu olarak çalıştırma** | Dosya; ProcessServerMonitor; cxprocessserver; Inmage Pushınstall; Günlük karşıya yükleme hizmeti (LogUpload); Inmage Scout uygulama hizmeti; Microsoft Azure kurtarma Hizmetleri Aracısı (obengine); Inmage Scout VX Aracısı-Sentinel/Outpost (svagents); tmansvc.
**Azure'da yeniden çalışma için Dağıtılmış işlem sunucusu** | Dosya; ProcessServerMonitor; cxprocessserver; Inmage Pushınstall; Günlük karşıya yükleme hizmeti (LogUpload)


## <a name="step-3-check-the-process-server-heartbeat"></a>3. adım: İşlem sunucusu sinyal denetleyin

(Hata kodu 806) işlem sunucusundan sinyal yok ise, aşağıdakileri yapın:

1. İşlem sunucusu sanal makine ve çalışıyor olduğundan emin olun.
2. Bu günlükler hataları denetleyin.

    C:\ProgramData\ASR\home\svsystems\eventmanager *.log C\ProgramData\ASR\home\svsystems\monitor_protection*.log

## <a name="check-connectivity-and-replication"></a>Bağlantısını denetleyin ve çoğaltma

 İlk ve devam eden çoğaltma hatalarını genellikle işlem sunucusu ile Azure arasında veya kaynak makine ve işlem sunucusu arasında bağlantı sorunları nedeniyle oluşup. Aşağıdaki grafikte, şu adımları özetlenmiştir adımları yardımcı olacak yordamlar tarafından izlenen.

![Bağlantı ve çoğaltma sorunlarını giderme](./media/vmware-physical-azure-troubleshoot-process-server/troubleshoot-connectivity-replication.png)


## <a name="step-4-verify-time-sync-on-source-machine"></a>4. Adım: Kaynak makine üzerinde zaman eşitleme doğrulayın

Çoğaltılan makinelerin için sistem tarihi/saatinin eşitlenmiş olduğundan emin olun. [Daha fazla bilgi edinin](https://docs.microsoft.com/windows-server/networking/windows-time-service/accurate-time)

## <a name="step-5-check-anti-virus-software-on-source-machine"></a>5. Adım: Kaynak makinedeki virüsten koruma yazılımı denetleyin

Site Recovery çoğaltılan makinelerin hiçbir virüsten koruma yazılımı engellemediğinden emin denetleyin. Site Recovery virüsten koruma programlarından hariç tutmak gerekirse gözden [bu makalede](vmware-azure-set-up-source.md#azure-site-recovery-folder-exclusions-from-antivirus-program).

## <a name="step-6-check-connectivity-from-source-machine"></a>6. Adım: Kaynak makineden bağlantısını kontrol edin


1. Yükleme [Telnet istemcisi](https://technet.microsoft.com/library/cc771275(v=WS.10).aspx) gerekirse kaynak makinede. Ping kullanmayın.
2. Kaynak makineden işlem sunucusu ile Telnet HTTPS bağlantı noktasına ping atın. Varsayılan olarak 9443, çoğaltma trafiği için HTTPS bağlantı noktasıdır.

    `telnet <process server IP address> <port>`

3. Bağlantı başarılı olup olmadığını doğrulayın.


**Bağlantı** | **Ayrıntılar** | **Eylem**
--- | --- | ---
**Başarılı** | İşlem sunucusu erişilebilir olduğundan ve boş bir ekran Telnet gösterir. | Başka bir işlem yapılması gerekmez.
**Başarısız** | Bağlantı kurulamıyor | İşlem sunucusunda gelen bağlantı noktası 9443'e izin verildiğinden emin olun. Örneğin, bir çevre ağına veya denetlenen bir alt ağdan varsa. Bağlantıyı yeniden denetleyin.
**Kısmen başarılı** | Bağlayabilirsiniz, ancak kaynak makinenin işlem sunucusuna ulaşılamıyor bildirir. | Sorun giderme sonraki yordamla devam edin.

## <a name="step-7-troubleshoot-an-unreachable-process-server"></a>7. Adım: Erişilemeyen işlem sunucusu sorunlarını giderme

İşlem Sunucusu Kaynak makinede ulaşılabilir değilse, hata 78186 görüntülenir. Gönderilmeyen durumunda bu sorun hem de uygulamayla tutarlı olmasına neden olur ve beklendiği gibi oluşturulmasını değil kilitlenmeyle tutarlı kurtarma noktası.

Kaynak makine, işlem sunucusunun IP adresi ulaşmak ve uçtan uca bağlantıyı denetlemek için kaynak makinede cxpsclient aracı olup olmadığını kontrol ederek sorunlarını giderin.


### <a name="check-the-ip-connection-on-the-process-server"></a>İşlem sunucusu üzerindeki IP bağlantısını denetleyin

Telnet başarılı olur, ancak kaynak makinenin işlem sunucusuna ulaşılamıyor raporları, işlem sunucusunun IP adresini ulaşıp ulaşamadığını denetleyin.

1. IP adresi https://<PS_IP>:<PS_Data_Port>/ ulaşmak bir web tarayıcısında deneyin.
2. Bu denetimi bir HTTPS sertifikası hatası gösterirse, bu durum normaldir. Hatayı yoksayın, bir 400 - bozuk istek görmeniz gerekir. Bu sunucunun bir tarayıcı isteğini sunamaz ve standart HTTPS bağlantı uygundur anlamına gelir.
3. Ardından bu denetimi işe yaramazsa, tarayıcı hata iletisi unutmayın. Örneğin, bir 407 hatası Ara sunucu kimlik doğrulaması ile ilgili bir sorun gösterecektir.

### <a name="check-the-connection-with-cxpsclient"></a>Onay cxpsclient bağlantısı

Ayrıca, uçtan uca bağlantıyı denetlemek için cxpsclient aracı çalıştırabilirsiniz.

1. Aracı aşağıdaki gibi çalıştırın:

    ```
    <install folder>\cxpsclient.exe -i <PS_IP> -l <PS_Data_Port> -y <timeout_in_secs:recommended 300>
    ```

2. İşlem sunucusu üzerindeki bu klasörlerdeki oluşturulan günlüklere bakın:

    C:\ProgramData\ASR\home\svsystems\transport\log\cxps.err  C:\ProgramData\ASR\home\svsystems\transport\log\cxps.xfer



### <a name="check-source-vm-logs-for-upload-failures-error-78028"></a>Kaynak VM günlüklerini karşıya yükleme hataları (hata 78028) denetleyin

İşlem hizmeti için kaynak makinelerden engellenen veri karşıya yüklemesi ile ilgili sorun değil oluşturulan iki kilitlenmeyle tutarlı ve uygulamayla tutarlı kurtarma noktalarına neden olabilir. 

1. Ağ yükleme sorunlarını gidermek için bu günlükteki hataları arayabilirsiniz:

    C:\Program dosyaları (x86) \Microsoft Azure Site Recovery\agent\svagents*.log 

2. Yordamlar bu makalenin geri kalanını kullanımı verileri karşıya yükleme sorunlarını çözmenize yardımcı olabilir.



## <a name="step-8-check-whether-the-process-server-is-pushing-data"></a>8. adım: Veri işlem sunucusunu göndermeye olup olmadığını denetleyin

İşlem sunucusu etkin bir şekilde veri Azure'a gönderme olup olmadığını denetleyin.

  1. İşlem sunucusu, Görev Yöneticisi'ni (Ctrl + Shift + Esc tuşlarına basın) açın.
  2. Seçin **performans** sekmesi > **açık Kaynak İzleyicisi**.
  3. İçinde **Kaynak İzleyicisi** sayfasında **ağ** sekmesi. Altında **ağ etkinliği işlemlerle**, cbengine.exe verilerinin büyük bir vNotolume etkin bir şekilde gönderip göndermediğini denetleyin.

       ![Ağ etkinliği ile işlem birimlerini](./media/vmware-physical-azure-troubleshoot-process-server/cbengine.png)

  Büyük bir veri hacmi cbengine.exe gönderme değil, aşağıdaki bölümlerde yer alan adımları tamamlayın.

## <a name="step-9-check-the-process-server-connection-to-azure-blob-storage"></a>9. adım: Azure blob depolama alanına işlem sunucusu bağlantısını denetleyin

1. Kaynak İzleyicisi'nde seçin **cbengine.exe**.
2. Altında **TCP bağlantılarını**, Azure depolama işlem sunucusundan bağlantı olup olmadığını denetleyin.

  ![Azure Blob Depolama URL'si cbengine.exe arasındaki bağlantı](./media/vmware-physical-azure-troubleshoot-process-server/rmonitor.png)

### <a name="check-services"></a>Onay Hizmetleri

Azure blob depolama URL'si işlem sunucusundan bağlantı varsa, hizmetlerinin çalıştığını kontrol edin.

1. Denetim Masası'nda seçin **Hizmetleri**.
2. Şu hizmetlerin çalıştığını doğrulayın:

    - cxprocessserver
    - Inmage Scout VX Aracısı-Sentinel/Outpost
    - Microsoft Azure Kurtarma Hizmetleri Aracısı
    - Microsoft Azure Site Recovery Hizmeti
    - tmansvc

3. Veya çalışmadığından herhangi bir hizmeti yeniden başlatın.
4. İşlem sunucusu bağlı ve erişilebilir olduğunu doğrulayın. 

## <a name="step-10-check-the-process-server-connection-to-azure-public-ip-address"></a>10. adım: Azure genel IP adresi işlem sunucusu bağlantısını denetleyin.

1. İşlem sunucusu üzerindeki içinde **%programfiles%\Microsoft Azure kurtarma Hizmetleri Agent\Temp**, en son CBEngineCurr.errlog dosyasını açın.
2. Dosyada arayın **443**, veya dizesi için **başarısız bağlantı denemesi**.

  ![Temp klasörü hata günlükleri](./media/vmware-physical-azure-troubleshoot-process-server/logdetails1.png)

3. Bir sorun yaşarsanız Azure genel IP adresi, bağlantı noktası 443 kullanılarak CBEngineCurr.currLog dosyada bulunan:

  `telnet <your Azure Public IP address as seen in CBEngineCurr.errlog>  443`

5. İşlem sunucusu üzerindeki komut satırında, Azure genel IP adresine ping atmayı Telnet kullanın.
6. Bağlanamıyorsanız, sonraki yordamı izleyin.

## <a name="step-11-check-process-server-firewall-settings"></a>11. adım: İşlem sunucusu güvenlik duvarı ayarlarını kontrol edin. 

IP adresi tabanlı güvenlik duvarı işlem sunucusu üzerindeki erişim engelleyip engellemediğini denetleyin.

1. IP adresi tabanlı güvenlik duvarı kuralları için:

    bir) tam listesini indirin [Microsoft Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653).

    b) Güvenlik Duvarı'nın azure'a (ve varsayılan HTTPS bağlantı noktası, 443) iletişimine izin verdiğinden emin olmak için güvenlik duvarı yapılandırması için IP adresi aralıklarını ekleyin.

    c) izin IP adresi aralıklarını (erişim denetimi ve kimlik yönetimi için kullanılır) Azure Batı ABD bölgesinde yanı sıra, aboneliğinizin Azure bölgesi.

2. URL tabanlı güvenlik duvarları için güvenlik duvarı yapılandırması için aşağıdaki tabloda listelenen URL'leri ekleyin.

    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]  


## <a name="step-12-verify-process-server-proxy-settings"></a>12. adım: İşlem sunucusunun proxy ayarlarını doğrulayın 

1. Bir ara sunucu kullanıyorsanız, proxy sunucusu adı DNS sunucusu tarafından çözümlenir emin olun. Yapılandırma sunucusu kayıt defteri anahtarında ayarladığınızda sağladığınız değeri denetleyin **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Azure Site Recovery\ProxySettings**.
2. Veri göndermek için aynı ayarları Azure Site Recovery aracısı tarafından kullanıldığından emin olun.

    (a) arama **Microsoft Azure yedekleme**.

    (b) açık **Microsoft Azure Backup**seçip **eylem** > **özelliklerini değiştirme**.

    c) üzerinde **Proxy Yapılandırması** sekmesinde proxy adresi olmalıdır kayıt defteri ayarlarında gösterilen proxy adresi aynı. Aksi durumda, aynı adrese değiştirin.

## <a name="step-13-check-bandwidth"></a>13. adım: Bant genişliğini denetleme

İşlem sunucusu ile Azure arasında bant genişliğini artırın ve ardından Sorun oluşmaya devam edip etmediğini denetleyin.


## <a name="next-steps"></a>Sonraki adımlar

Daha fazla yardıma ihtiyacınız varsa, sorunuzu gönderin [Azure Site Recovery Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr). 

[green]: ./media/vmware-physical-azure-troubleshoot-process-server/green.png
[yellow]: ./media/vmware-physical-azure-troubleshoot-process-server/yellow.png
[red]: ./media/vmware-physical-azure-troubleshoot-process-server/red.png