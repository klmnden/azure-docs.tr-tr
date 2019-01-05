---
title: Azure Site Recovery kullanarak VMware Vm'lerini ve fiziksel sunucuları azure'a olağanüstü durum kurtarma sırasında yapılandırma sunucusu sorunlarını giderme | Microsoft Docs
description: Bu makalede, Azure Site Recovery kullanarak VMware Vm'lerinin olağanüstü durum kurtarması için yapılandırma sunucusunu ve fiziksel sunucuları Azure'a dağıtmak için sorun giderme bilgileri sağlar.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 12/17/2018
ms.author: ramamill
ms.openlocfilehash: 597b8f59ef6991f7868d3de481e98ed9a459077b
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54050804"
---
# <a name="troubleshoot-configuration-server-issues"></a>Yapılandırma sunucusu sorunlarını giderme

Bu makalede, yardımcı dağıtmak ve yönetmek, sorunlarını [Azure Site Recovery](site-recovery-overview.md) yapılandırma sunucusu. Yapılandırma sunucusunu bir yönetim sunucusu olarak görev yapar. Yapılandırma sunucusunu Site Recovery kullanarak şirket içi VMware Vm'leri ve fiziksel sunucuları azure'a olağanüstü durum kurtarma ayarlamayı öğrenin. Aşağıdaki bölümlerde, yeni bir yapılandırma sunucusu eklediğinizde ve yapılandırma sunucusu yönetirken karşılaşabileceğiniz en yaygın hataları açıklanmaktadır.

## <a name="registration-failures"></a>Kayıt hataları

Mobility Aracısı yüklediğinizde, kaynak makinenin yapılandırma sunucusuna kaydeder. Bu yönergeleri takip ederek, bu adım sırasında hataları ayıklayabilirsiniz:

1. C:\ProgramData\ASR\home\svsystems\var\configurator_register_host_static_info.log dosyasını açın. (ProgramData klasörü gizli bir klasör olabilir. ProgramData klasörü dosya Gezgini'nde, üzerinde görmezseniz **görünümü** sekmesinde **Göster/Gizle** bölümünden **öğelerin gizli** onay kutusunu.) Hataları, birden çok sorunlarından kaynaklanıyor olabilir.

2. Arama dizesi için **Hayır geçerli IP adresi bulunamadı**. Dize bulunursa:
    1. İstenen konak kimliği kaynak makinenin konak kimliği ile aynı olduğunu doğrulayın.
    2. Kaynak makinenin fiziksel NIC'ye atanmış en az bir IP adresi olduğunu doğrulayın Başarılı olması için yapılandırma sunucusu ile aracı kaydı için kaynak makinenin fiziksel NIC'ye atanmış en az bir geçerli IP v4 adresi olmalıdır
    3. Kaynak makinenin tüm IP adreslerini almak için kaynak makinede olarak aşağıdaki komutlardan birini çalıştırın:
      - Windows için: `> ipconfig /all`
      - Linux için: `# ifconfig -a`

3. Dize **Hayır geçerli IP adresi bulunamadı** bulunamadığında, arama dizesi için **nedeni = > NULL**. Kaynak makine ile yapılandırma sunucusunu kaydetmek için boş bir konak kullanıyorsa, bu hata oluşur. Dize bulunursa:
    - Sorunu çözdükten sonra faydalandığından [kaynak makinenin yapılandırma sunucusuna kaydedin](vmware-azure-troubleshoot-configuration-server.md#register-source-machine-with-configuration-server) kaydını el ile yeniden denemek için.

4. Dize **nedeni = > NULL** değil bulundu, kaynak makinede, açık C:\ProgramData\ASRSetupLogs\UploadedLogs\ASRUnifiedAgentInstaller.log dosya. (ProgramData klasörü gizli bir klasör olabilir. ProgramData klasörü dosya Gezgini'nde, üzerinde görmezseniz **görünümü** sekmesinde **Göster/Gizle** bölümünden **öğelerin gizli** onay kutusunu.) Hataları, birden çok sorunlarından kaynaklanıyor olabilir. 

5. Arama dizesi için **isteği gönderin: (7) - sunucuya bağlanılamadı**. Dize bulunursa:
    1. Yapılandırma sunucusu ile kaynak makine arasında ağ sorunları çözün. Ping, traceroute veya bir web tarayıcısı gibi ağ araçları kullanarak yapılandırma sunucusunun kaynak makineden erişilebilir olduğundan emin olun. Kaynak makinenin yapılandırma sunucusuna bağlantı noktası 443 üzerinden erişebildiğinden emin olun.
    2. Herhangi bir güvenlik duvarını yapılandırma sunucusu ile kaynak makine arasında bağlantı kaynak makine blok kuralları olup olmadığını denetleyin. Bağlantı sorunları engelini kaldırmak için ağ yöneticileri ile çalışır.
    3. Listelenen klasörler emin [Site Recovery klasör dışlamaları virüsten koruma programlarından](vmware-azure-set-up-source.md#azure-site-recovery-folder-exclusions-from-antivirus-program) virüsten koruma yazılımından hariç tutulur.
    4. Ağ sorunları çözüldükten sonra yönergeleri izleyerek kayıt yeniden deneme [kaynak makinenin yapılandırma sunucusuna kaydedin](vmware-azure-troubleshoot-configuration-server.md#register-source-machine-with-configuration-server).

6. Dize **isteği gönderin: (7) - sunucuya bağlanılamadı** , aynı günlük dosyasında, arama dizesi bulunamadığında **isteği: (60) - eş sertifika kimlik doğrulaması ile CA sertifikalarını**. Yapılandırma sunucusu sertifikasının süresi doldu veya kaynak makine, TLS 1.0 veya üzeri SSL protokolleri desteklemiyor. Bu hata oluşabilir. Bir güvenlik duvarı yapılandırma sunucusu ile kaynak makine arasında SSL iletişimi engellerse de oluşabilir. Dize bulunursa: 
    1. Çözümlemek için IP adresi yapılandırma sunucusuna kaynak makinede bir web tarayıcısı kullanarak bağlanın. URI https kullanacak:\/\/< yapılandırma sunucusunun IP adresi\>: 443 /. Kaynak makinenin yapılandırma sunucusuna bağlantı noktası 443 üzerinden erişebildiğinden emin olun.
    2. Kaynak makinede herhangi bir güvenlik duvarı kuralları veya kaynak makinenin yapılandırma sunucusuna konuşmaya kaldırılamaz gerekli olup olmadığını denetleyin. Kullanımda olabilecek bir güvenlik duvarı yazılımı çeşitli nedeniyle, size tüm gerekli güvenlik duvarı yapılandırmaları listelenemiyor. Bağlantı sorunları engelini kaldırmak için ağ yöneticileri ile çalışır.
    3. Listelenen klasörler emin [Site Recovery klasör dışlamaları virüsten koruma programlarından](vmware-azure-set-up-source.md#azure-site-recovery-folder-exclusions-from-antivirus-program) virüsten koruma yazılımından hariç tutulur.  
    4. Sorunu çözdükten sonra aşağıdaki yönergeleri tarafından kayıt yeniden [kaynak makinenin yapılandırma sunucusuna kaydedin](vmware-azure-troubleshoot-configuration-server.md#register-source-machine-with-configuration-server).

7. Linux üzerinde platform değerini < INSTALLATION_DIR\>/etc/drscout.conf bozuk, kayıt işlemi başarısız olur. Bu sorunu tanımlamak için /var/log/ua_install.log dosyasını açın. Arama dizesi için **VM_PLATFORM değeri null ya da olduğu gibi yapılandırma durduruluyor VmWare/Azure değil**. Platform ayarlanması **VmWare** veya **Azure**. Drscout.conf dosyası bozuk, öneririz, [mobility aracısını kaldırın](vmware-physical-mobility-service-overview.md#uninstall-the-mobility-service) ve mobility Aracısı'nı yeniden yükleyin. Kaldırma başarısız olursa, aşağıdaki adımları tamamlayın:
    1. Installation_Directory/uninstall.sh dosyasını açıp çağrı yorum **StopServices** işlevi.
    2. Installation_Directory/Vx/bin/uninstall.sh dosyasını açıp çağrı yorum **stop_services** işlevi.
    3. Fx hizmetini durdurmak için çalışan tüm bölüm yorum ve Installation_Directory/Fx/uninstall.sh dosyasını açın.
    4. [Kaldırma](vmware-physical-mobility-service-overview.md#uninstall-the-mobility-service) mobility Aracısı. Başarılı kaldırma sonra sistemi yeniden başlatın ve ardından mobility aracısını yeniden yüklemeyi deneyin.

## <a name="installation-failure-failed-to-load-accounts"></a>Yükleme hatası: Hesaplar yüklenemedi

Mobility Aracısı'nı yükleme ve yapılandırma sunucusu ile kayıt hizmet taşıma bağlantısından veriler okunamıyor olamaz bu hata oluşur. Sorunu çözmek için TLS 1.0, kaynak makinede etkinleştirildiğinden emin olun.

## <a name="change-the-ip-address-of-the-configuration-server"></a>Yapılandırma sunucusunun IP adresini değiştirme

Yapılandırma sunucusunun IP adresini değiştirmemenizi öneririz. Yapılandırma sunucusuna atadığınız tüm IP adreslerini statik IP adresleri olduğundan emin olun. DHCP IP adresini kullanmayın.

## <a name="acs50008-saml-token-is-invalid"></a>ACS50008: SAML belirteci geçersiz.

Bu hatayı önlemek için bu, sistem saati 15 dakikadan fazla kaynağından yerel saat farklı değil emin olun. Kayıt işlemini tamamlamak için yükleyiciyi yeniden çalıştırın.

## <a name="failed-to-create-a-certificate"></a>Bir sertifika oluşturulamadı

Site Recovery kimlik doğrulaması için gereken sertifika oluşturulamıyor. Kurulum yerel bir yönetici olarak çalıştırıyorsanız emin olduktan sonra kurulumu yeniden çalıştırın.

## <a name="register-the-source-machine-with-the-configuration-server"></a>Kaynak makinenin yapılandırma sunucusuna kaydedin

### <a name="if-the-source-machine-runs-windows"></a>Kaynak makine Windows çalıştırıyorsa

Kaynak makine üzerinde aşağıdaki komutu çalıştırın:

```
  cd C:\Program Files (x86)\Microsoft Azure Site Recovery\agent
  UnifiedAgentConfigurator.exe  /CSEndPoint <configuration server IP address> /PassphraseFilePath <passphrase file path>
  ```

Ayar | Ayrıntılar
--- | ---
Kullanım | UnifiedAgentConfigurator.exe/csendpoint < yapılandırma sunucusunun IP adresi \> /passphrasefilepath < parola dosya yolu\>
Aracı yapılandırma günlükleri | % ProgramData%\ASRSetupLogs\ASRUnifiedAgentConfigurator.log altında bulunur.
/CSEndPoint | Zorunlu parametre. Yapılandırma sunucusunun IP adresini belirtir. Herhangi bir geçerli IP adresi kullanın.
/PassphraseFilePath |  Zorunlu. Parola dosyasının konumu. Herhangi bir geçerli UNC veya yerel dosya yolu kullanın.

### <a name="if-the-source-machine-runs-linux"></a>Kaynak makine Linux çalıştırıyorsa

Kaynak makine üzerinde aşağıdaki komutu çalıştırın:

```
  /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <configuration server IP address> -P /var/passphrase.txt
  ```

Ayar | Ayrıntılar
--- | ---
Kullanım | CD /usr/local/ASR/Vx/bin<br /><br /> UnifiedAgentConfigurator.sh -i < yapılandırma sunucusunun IP adresi\> - P < parola dosya yolu\>
-i | Zorunlu parametre. Yapılandırma sunucusunun IP adresini belirtir. Herhangi bir geçerli IP adresi kullanın.
-P |  Zorunlu. Parola kaydedildiği dosyasının tam dosya yolu. Herhangi bir geçerli klasörü kullanın.

