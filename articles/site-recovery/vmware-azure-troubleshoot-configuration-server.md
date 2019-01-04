---
title: VMware Vm'lerini ve fiziksel sunucuları Azure Site Recovery ile azure'a olağanüstü durum kurtarma sırasında yapılandırma sunucusu sorunlarını giderme | Microsoft Docs
description: Bu makalede VMware Vm'lerinin olağanüstü durum kurtarması için yapılandırma sunucusunu ve fiziksel sunucuları Azure Site Recovery ile azure'a dağıtmak için sorun giderme bilgileri sağlar.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 12/17/2018
ms.author: ramamill
ms.openlocfilehash: f5c8241907459a06f0a6206ae6865cdf3fe9ab89
ms.sourcegitcommit: da69285e86d23c471838b5242d4bdca512e73853
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2019
ms.locfileid: "53998974"
---
# <a name="troubleshoot-configuration-server-issues"></a>Yapılandırma sunucusu sorunlarını giderme

Bu makale, dağıtma ve yönetme, sorunları gidermenize yardımcı olur. [Azure Site Recovery](site-recovery-overview.md) yapılandırma sunucusu. Yapılandırma sunucusu, yönetim sunucusu olarak görev yapar ve şirket içi VMware Vm'leri ve fiziksel sunucuları Azure Site Recovery ile olağanüstü durum kurtarma ayarlama ayarlanması için kullanılır. Aşağıdaki bölümlerde, yeni bir yapılandırma sunucusu ekleme ve yapılandırma sunucusunu yönetme görülen yaygın hataları açıklanır.

## <a name="registration-failures"></a>Kayıt hataları

Kaynak makine, mobility Aracısı yüklemesi sırasında yapılandırma sunucusu ile kaydeder. Aşağıda verilen yönergeleri izleyerek bu adımı sırasında hataları ayıklanabilir:

1. C:\ProgramData\ASR\home\svsystems\var\configurator_register_host_static_info.log dosyasına gidin. ProgramData gizli bir klasör olabilir. Bırakamıyorsanız bulunacak gösterin için klasör deneyin. Hataları nedeniyle birden çok sorunlar olabilir.
2. "Geçerli bir IP adresi bulunamadı" dizesini arayın. Dize bulunursa,
    - İstenen konak kimliği kaynak makine ile aynı olup olmadığını doğrulayın.
    - Kaynak makine Aracısı kaydı için fiziksel NIC CS ile başarılı olması için atanmış en az bir IP adresi olmalıdır.
    - Kaynak makinede komutu Çalıştır `> ipconfig /all` (için Windows işletim sistemi) ve `# ifconfig -a` (için Linux işletim sistemi) kaynak makinenin tüm IP adreslerini almak için.
    - Aracı kaydı fiziksel NIC'ye atanmış geçerli bir IP v4 adresi gerektiğini lütfen unutmayın
3. Yukarıdaki dize bulunamazsa, dize "Neden" için arama = > "NULL". Varsa bulunan
    - Kaynak makinenin yapılandırma sunucusuna kaydedin için boş bir konak kullanırken, bu hata oluşur.
    - Sorunları giderdikten sonra verilen yönergeleri izleyerek [burada](vmware-azure-troubleshoot-configuration-server.md#register-source-machine-with-configuration-server) kaydını el ile yeniden denemek için.
4. Yukarıdaki dize bulunamazsa, kaynak makineye gidin ve C:\ProgramData\ASRSetupLogs\UploadedLogs günlüğünü kontrol edin\* ASRUnifiedAgentInstaller.log ProgramData, gizli bir klasör olabilir. Bırakamıyorsanız bulunacak gösterin için klasör deneyin. Hataları nedeniyle birden çok sorunlar olabilir. Arama dizesi için "istek gönderin: (7) - uygulanamadı sunucuya bağlanın". Varsa bulunan
    - Kaynak makine ve yapılandırma sunucusu arasında ağ sorunları çözün. Bu yapılandırma sunucusu ping traceroute, web tarayıcısı vb. gibi ağ araçlarını kullanarak kaynak makineden erişilebilir olduğundan emin olun, bu kaynak makinenin yapılandırma sunucusu bağlantı noktası 443 üzerinden ulaşabildiğinden emin olun.
    - Kaynak makinede herhangi bir güvenlik duvarı kuralları varsa onay kaynak makine ile yapılandırma sunucusu arasındaki bağlantıyı engelliyor. Çalışmak ağ yöneticiniz bağlantı sorunlarına engelini kaldırmak için.
    - Belirtilen klasörleri olun [burada](vmware-azure-set-up-source.md#azure-site-recovery-folder-exclusions-from-antivirus-program) virüsten koruma yazılımından hariç tutulur.
    - Ağ sorunları giderdikten sonra kayıt verilen aşağıdaki tarafından yeniden deneme yönergeleri [burada](vmware-azure-troubleshoot-configuration-server.md#register-source-machine-with-configuration-server).
5. Aksi durumda, aynı günlük arama dizesi için bulunan "istek: (60) - eş sertifika kimlik doğrulaması ile CA sertifikalarını. ". Varsa bulunan 
    - Yapılandırma sunucusu sertifikasının süresi doldu veya kaynak makine TLS 1.0 desteklemiyor ve SSL protokolleri, veya kaynak makine ve yapılandırma sunucusu arasında SSL iletişimi engelleyen bir güvenlik duvarı bu hata olabilir.
    - Yapılandırma sunucusunun IP adresi https:// URI'ın yardımıyla kaynak makinedeki bir web tarayıcısı kullanarak bağlanmak çözmek için<CSIPADDRESS>: 443 /. Bu kaynak makinenin yapılandırma sunucusu bağlantı noktası 443 üzerinden ulaşabildiğinden emin olun.
    - Kaynak makinede herhangi bir güvenlik duvarı kuralları varsa onay kaynak makine ile yapılandırma sunucusu arasındaki bağlantıyı engelliyor. Çalışmak ağ yöneticiniz bağlantı sorunlarına engelini kaldırmak için.
    - Belirtilen klasörleri olun [burada](vmware-azure-set-up-source.md#azure-site-recovery-folder-exclusions-from-antivirus-program) virüsten koruma yazılımından hariç tutulur.  
    - Sorunları giderdikten sonra kayıt verilen aşağıdaki tarafından yeniden deneme yönergeleri [burada](vmware-azure-troubleshoot-configuration-server.md#register-source-machine-with-configuration-server).
6. < INSTALLATION_DIR > /etc/drscout.conf platformdan değerini bozuksa, Linux ardından kayıt başarısız olur. Tanımlamak için günlük /var/log/ua_install.log gidin. "VM_PLATFORM değeri null ya da olduğu gibi yapılandırma durduruluyor VmWare/Azure değil." dizesi bulabilirsiniz. Platform, "VmWare" veya "Azure" olarak ayarlanmalıdır. Drscout.conf dosyası bozulmuş gibi tavsiye edilir [kaldırma](vmware-physical-mobility-service-overview.md#uninstall-the-mobility-service) mobility Aracısı ile yeniden yükleyin. Yüklemeyi başarısız olursa izleyin aşağıdaki adımları:
    - Açık dosya Installation_Directory/uninstall.sh ve işlev çağrısı yorum *StopServices*
    - Dosya Installation_Directory/Vx/bin/uninstall açın ve işlev çağrısı açıklama satırı yapın `stop_services`
    - Dosya Installation_Directory/Fx/kaldırma ve Fx hizmetini durdurmak için çalışıyor tam bölümü yorum açın.
    - Şimdi deneyin [kaldırma](vmware-physical-mobility-service-overview.md#uninstall-the-mobility-service) mobility Aracısı. Başarılı kaldırma yüklemeden sonra sistemi yeniden başlatın ve aracısını yeniden yüklemeyi deneyin.

## <a name="installation-failure---failed-to-load-accounts"></a>Yükleme başarısız - hesaplar yüklenemedi

Hizmet mobility Aracısı yükleme ve yapılandırma sunucusu ile kayıt sırasında aktarım bağlantısından veriler okuyamıyor olduğunda bu hata oluşur. Çözümlemek için TLS 1.0 kaynak makineniz üzerinde etkin olduğundan emin olun.

## <a name="change-ip-address-of-configuration-server"></a>Yapılandırma sunucusunun IP adresini değiştirme

Yapılandırma sunucusunun IP adresini değiştirmek için değil kesinlikle önerilir. Yapılandırma sunucusuna atanan tüm IP'lere statik IP ve değil DHCP IP'ler olduğundan emin olun.

## <a name="acs50008-saml-token-is-invalid"></a>ACS50008: SAML belirteci geçersiz.

Bu hatayı önlemek için sistem saati yerel saat 15 dakikadan uzun olmadığından emin olun. Kayıt işlemini tamamlamak için yükleyiciyi yeniden çalıştırın.

## <a name="failed-to-create-certificate"></a>Sertifika oluşturulamadı

Site Recovery kimlik doğrulaması için gereken sertifika oluşturulamıyor. Kurulum yerel bir yönetici olarak çalıştırdığınızdan emin olduktan sonra kurulumu yeniden çalıştırın.

## <a name="register-source-machine-with-configuration-server"></a>Kaynak makinenin yapılandırma sunucusuna kaydedin

### <a name="if-source-machine-has-windows-os"></a>Kaynak makine Windows işletim sistemi varsa

Kaynak makine üzerinde aşağıdaki komutu çalıştırın

```
  cd C:\Program Files (x86)\Microsoft Azure Site Recovery\agent
  UnifiedAgentConfigurator.exe  /CSEndPoint <CSIP> /PassphraseFilePath <PassphraseFilePath>
  ```
**Ayar** | **Ayrıntılar**
--- | ---
Kullanım | UnifiedAgentConfigurator.exe/csendpoint  <CSIP> /passphrasefilepath <PassphraseFilePath>
Aracı yapılandırma günlükleri | % ProgramData%\ASRSetupLogs\ASRUnifiedAgentConfigurator.log altında.
/CSEndPoint | Zorunlu parametre. Yapılandırma sunucusunun IP adresini belirtir. Herhangi bir geçerli IP adresi kullanın.
/PassphraseFilePath |  Zorunlu. Parola dosyasının konumu. Herhangi bir geçerli UNC veya yerel dosya yolu kullanın.

### <a name="if-source-machine-has-linux-os"></a>Kaynak makine Linux işletim sistemi varsa

Kaynak makine üzerinde aşağıdaki komutu çalıştırın

```
  /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <CSIP> -P /var/passphrase.txt
  ```
**Ayar** | **Ayrıntılar**
--- | ---
Kullanım | CD /usr/local/ASR/Vx/bin<br/><br/> UnifiedAgentConfigurator.sh -i <CSIP> - P <PassphraseFilePath>
-i | Zorunlu parametre. Yapılandırma sunucusunun IP adresini belirtir. Herhangi bir geçerli IP adresi kullanın.
-P |  Zorunlu. Parola kaydedildiği dosyasının tam dosya yolu. Herhangi bir geçerli klasörü kullanın