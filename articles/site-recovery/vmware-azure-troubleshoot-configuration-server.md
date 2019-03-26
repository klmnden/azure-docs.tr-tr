---
title: Azure Site Recovery kullanarak VMware Vm'lerini ve fiziksel sunucuları azure'a olağanüstü durum kurtarma sırasında yapılandırma sunucusu sorunlarını giderme | Microsoft Docs
description: Bu makalede, Azure Site Recovery kullanarak VMware Vm'lerinin olağanüstü durum kurtarması için yapılandırma sunucusunu ve fiziksel sunucuları Azure'a dağıtmak için sorun giderme bilgileri sağlar.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 02/13/2019
ms.author: ramamill
ms.openlocfilehash: 287a4104104c12e33fa2c50c398f422f9e6ea8c5
ms.sourcegitcommit: 72cc94d92928c0354d9671172979759922865615
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58418712"
---
# <a name="troubleshoot-configuration-server-issues"></a>Yapılandırma sunucusu sorunlarını giderme

Bu makalede, yardımcı dağıtmak ve yönetmek, sorunlarını [Azure Site Recovery](site-recovery-overview.md) yapılandırma sunucusu. Yapılandırma sunucusunu bir yönetim sunucusu olarak görev yapar. Yapılandırma sunucusunu Site Recovery kullanarak şirket içi VMware Vm'leri ve fiziksel sunucuları azure'a olağanüstü durum kurtarma ayarlamayı öğrenin. Aşağıdaki bölümlerde, yeni bir yapılandırma sunucusu eklediğinizde ve yapılandırma sunucusu yönetirken karşılaşabileceğiniz en yaygın hataları açıklanmaktadır.

## <a name="registration-failures"></a>Kayıt hataları

Mobility Aracısı yüklediğinizde, kaynak makinenin yapılandırma sunucusuna kaydeder. Bu yönergeleri takip ederek, bu adım sırasında hataları ayıklayabilirsiniz:

1. Open the C:\ProgramData\ASR\home\svsystems\var\configurator_register_host_static_info.log file. (ProgramData klasörü gizli bir klasör olabilir. ProgramData klasörü dosya Gezgini'nde, üzerinde görmezseniz **görünümü** sekmesinde **Göster/Gizle** bölümünden **öğelerin gizli** onay kutusunu.) Hataları, birden çok sorunlarından kaynaklanıyor olabilir.

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

7. Linux üzerinde platform değerini < INSTALLATION_DIR\>/etc/drscout.conf bozuk, kayıt işlemi başarısız olur. Bu sorunu tanımlamak için /var/log/ua_install.log dosyasını açın. Arama dizesi için **VM_PLATFORM değeri null ya da olduğu gibi yapılandırma durduruluyor VmWare/Azure değil**. Platform ayarlanması **VmWare** veya **Azure**. Drscout.conf dosyası bozuk, öneririz, [mobility aracısını kaldırın](vmware-physical-manage-mobility-service.md#uninstall-mobility-service) ve mobility Aracısı'nı yeniden yükleyin. Kaldırma başarısız olursa, aşağıdaki adımları tamamlayın: bir. Installation_Directory/uninstall.sh dosyasını açıp çağrı yorum **StopServices** işlevi.
    b. Installation_Directory/Vx/bin/uninstall.sh dosyasını açıp çağrı yorum **stop_services** işlevi.
    c. Fx hizmetini durdurmak için çalışan tüm bölüm yorum ve Installation_Directory/Fx/uninstall.sh dosyasını açın.
    d. [Kaldırma](vmware-physical-manage-mobility-service.md#uninstall-mobility-service) mobility Aracısı. Başarılı kaldırma sonra sistemi yeniden başlatın ve ardından mobility aracısını yeniden yüklemeyi deneyin.

## <a name="installation-failure-failed-to-load-accounts"></a>Yükleme hatası: Hesaplar yüklenemedi

Mobility Aracısı'nı yükleme ve yapılandırma sunucusu ile kayıt hizmet taşıma bağlantısından veriler okunamıyor olamaz bu hata oluşur. Sorunu çözmek için TLS 1.0, kaynak makinede etkinleştirildiğinden emin olun.

## <a name="vcenter-discovery-failures"></a>vCenter bulma hatası

VCenter bulma hataları çözmek için atlama listesi proxy ayarları için vCenter sunucusu ekleyin. 

- PsExec aracını indirin [burada](https://aka.ms/PsExec) sistem kullanıcı içeriğe erişmek için.
- Internet Explorer açın sistem kullanıcı içeriği aşağıdaki komut satırı psexec -s çalıştırarak -i "%ProgramFiles%\Internet Explorer\iexplore.exe"
- Proxy ayarlarını IE'de ekleyin ve tmanssvc hizmetini yeniden başlatın.
- DRA proxy ayarlarını yapılandırmak için cd C:\Program Files\Microsoft Azure Site Recovery Sağlayıcısı'nı çalıştırın
- Ardından, DRCONFIGURATOR yürütün. EXE / /AddBypassUrls configure [Server sağlanan sırasında bir vCenter'ın IP adresini/FQDN'yi **vCenter sunucusu/vSphere ESXi sunucusunu yapılandır** adımında [yapılandırma sunucusu dağıtımı](vmware-azure-deploy-configuration-server.md#configure-settings)]

## <a name="change-the-ip-address-of-the-configuration-server"></a>Yapılandırma sunucusunun IP adresini değiştirme

Yapılandırma sunucusunun IP adresini değiştirmemenizi öneririz. Yapılandırma sunucusuna atadığınız tüm IP adreslerini statik IP adresleri olduğundan emin olun. DHCP IP adresini kullanmayın.

## <a name="acs50008-saml-token-is-invalid"></a>ACS50008: SAML belirteci geçersiz.

Bu hatayı önlemek için bu, sistem saati 15 dakikadan fazla kaynağından yerel saat farklı değil emin olun. Kayıt işlemini tamamlamak için yükleyiciyi yeniden çalıştırın.

## <a name="failed-to-create-a-certificate"></a>Bir sertifika oluşturulamadı

Site Recovery kimlik doğrulaması için gereken sertifika oluşturulamıyor. Kurulum yerel bir yönetici olarak çalıştırıyorsanız emin olduktan sonra kurulumu yeniden çalıştırın.

## <a name="failure-to-activate-windows-license-from-server-standard-evaluation-to-server-standard"></a>Windows Server Standard değerlendirme lisanstan Server standart etkinleştirmeyi başaramamak

1. OVF ile Configuration server dağıtımının bir parçası olarak, 180 gün boyunca geçerli olduğu bir deneme lisansı kullanılır. Bu lisans bu süresi önce etkinleştirmeniz gerekir. Aksi takdirde, bu yapılandırma sunucusunun sık kapatma neden ve bu nedenle çoğaltma etkinliklere performans sorunu neden.
2. Windows Lisansı Etkinleştirme bulamıyorsanız, ulaşın [Windows Destek ekibine](https://aka.ms/Windows_Support) sorunu çözmek için.

## <a name="register-source-machine-with-configuration-server"></a>Kaynak makinenin yapılandırma sunucusuna kaydedin

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

## <a name="unable-to-configure-the-configuration-server"></a>Yapılandırma sunucusu yapılandırılamıyor.

Sanal makinede yapılandırma sunucusu dışındaki uygulamalar yüklerseniz, ana hedef yapılandıramıyor.%n olabilir. 

Yapılandırma sunucusu, tek amaçlı bir sunucu ve bir paylaşılan sunucuyu desteklenmeyen olduğundan kullanılarak olması gerekir. 

Daha fazla bilgi için bkz: ' % s'yapılandırması SSS içinde [yapılandırma sunucusunu dağıtma](vmware-azure-deploy-configuration-server.md#faq). 

## <a name="remove-the-stale-entries-for-protected-items-from-the-configuration-server-database"></a>Korumalı öğeler için eski girişler yapılandırma sunucusu veritabanı bağlantısını Kaldır 

Yapılandırma sunucusundaki eski korunan makinenin kaldırmak için aşağıdaki adımları kullanın. 
 
1. Kaynak makine ve eski bir girişe IP adresini belirlemek için: 

    1. MYSQL komut satırı, Yönetici modunda açın. 
    2. Aşağıdaki komutları yürütün. 
   
        ```
        mysql> use svsdb1;
        mysql> select id as hostid, name, ipaddress, ostype as operatingsystem, from_unixtime(lasthostupdatetime) as heartbeat from hosts where name!='InMageProfiler'\G;
        ```

        Bu IP adresleri ve son sinyal birlikte kayıtlı makinelerin listesini döndürür. Eski çoğaltma çiftlerinin olan konak bulun.

2. Yükseltilmiş bir komut istemi açın ve C:\ProgramData\ASR\home\svsystems\bin için gidin. 
4. Yapılandırma sunucusunun kayıtlı konakları ayrıntıları ve eski giriş bilgilerini kaldırmak için kaynak makine ve eski bir girişe IP adresini kullanarak şu komutu çalıştırın. 
   
    `Syntax: Unregister-ASRComponent.pl -IPAddress <IP_ADDRESS_OF_MACHINE_TO_UNREGISTER> -Component <Source/ PS / MT>`
 
    Bir IP-adresi, 10.0.0.4 "VM01 OnPrem" bir kaynak sunucu girişi varsa ardından aşağıdaki komutu kullanın.
 
    `perl Unregister-ASRComponent.pl -IPAddress 10.0.0.4 -Component Source`
 
5. Yapılandırma sunucusu ile yeniden kaydettirmek için kaynak makinedeki aşağıdaki hizmetleri yeniden başlatın. 
 
    - Inmage Scout uygulama hizmeti
    - Inmage Scout VX Aracısı - Sentinel/Outpost

## <a name="upgrade-fails-when-the-services-fail-to-stop"></a>Hizmetleri durdurma başarısız olduğunda, yükseltme başarısız oluyor

Bazı hizmetler değil durdurduğunuzda yapılandırma sunucusu yükseltme başarısız olur. 

Sorunu tanımlamak için yapılandırma sunucusu için C:\ProgramData\ASRSetupLogs\CX_TP_InstallLogFile gidin. Hatalar görürseniz, sorunu çözmek için aşağıdaki adımları kullanın: 

    2018-06-28 14:28:12.943   Successfully copied php.ini to C:\Temp from C:\thirdparty\php5nts
    2018-06-28 14:28:12.943   svagents service status - SERVICE_RUNNING
    2018-06-28 14:28:12.944   Stopping svagents service.
    2018-06-28 14:31:32.949   Unable to stop svagents service.
    2018-06-28 14:31:32.949   Stopping svagents service.
    2018-06-28 14:34:52.960   Unable to stop svagents service.
    2018-06-28 14:34:52.960   Stopping svagents service.
    2018-06-28 14:38:12.971   Unable to stop svagents service.
    2018-06-28 14:38:12.971   Rolling back the install changes.
    2018-06-28 14:38:12.971   Upgrade has failed.

Bu sorunu çözmek için:

Aşağıdaki hizmetler el ile durdurun:

- cxprocessserver
- Inmage Scout VX Aracısı-Sentinel/Outpost, 
- Microsoft Azure kurtarma Hizmetleri Aracısı, 
- Microsoft Azure Site Recovery hizmeti 
- tmansvc
  
Yapılandırma sunucusunu güncelleştirmek için çalıştırın [birleşik Kurulumu](service-updates-how-to.md#links-to-currently-supported-update-rollups) yeniden.

## <a name="azure-active-directory-application-creation-failure"></a>Azure Active Directory Uygulama oluşturma hatası

Azure Active Directory (AAD) kullanarak bir uygulama oluşturmak için yeterli izinlere sahip [açık sanallaştırma uygulama (OVA)](vmware-azure-deploy-configuration-server.md#deployment-of-configuration-server-through-ova-template
) şablonu.

Bu sorunu çözmek için Azure portalında oturum açın ve aşağıdaki işlemlerden birini yapın:

- Aad'de uygulama geliştiricisi rol isteyin. Uygulama geliştiricisi rolü hakkında daha fazla bilgi için bkz. [Azure Active Directory'de Yönetici rolü izinleri](../active-directory/users-groups-roles/directory-assign-admin-roles.md).
- Doğrulayın **kullanıcı, uygulama oluşturabilir** bayrağı ayarlandığında *true* aad'de. Daha fazla bilgi için [nasıl yapılır: Azure AD'yi kaynaklara erişebilen uygulaması ve hizmet sorumlusu oluşturmak için portalı kullanma](../active-directory/develop/howto-create-service-principal-portal.md#required-permissions).

## <a name="process-servermaster-target-are-unable-to-communicate-with-the-configuration-server"></a>İşlem sunucusu/ana hedef yapılandırma sunucusu ile iletişim kuramıyor 

İşlem Sunucusu (PS) ve ana hedef (MT) modülleri (CS) yapılandırma sunucusu ile iletişim kuramadı ve durumlarını üzerinde bağlı Azure portalı olarak gösterilir.

Genellikle bu bağlantı noktası 443 ile bir hata nedeniyle oluşur. Bağlantı noktasının engelini kaldırmak ve CS ile iletişimi yeniden etkinleştirmek için aşağıdaki adımları kullanın.

**MARS Aracısı ana hedef aracı tarafından çağrılan doğrulayın**

Ana hedef aracısı için yapılandırma sunucusu IP'si TCP oturumu oluşturabilirsiniz doğrulamak için ana hedef aracı günlüklerinde aşağıdakine benzer bir izleme bakın:

TCP <Replace IP with CS IP here>: 52739 <Replace IP with CS IP here>: 443 SYN_SENT 

TCP 192.168.1.40:52739 192.168.1.40:443 SYN_SENT / / CS IP'sini buraya IP değiştirin

İzlemeleri MT aracı günlüklerinde aşağıdakine benzer fark ederseniz, MT Aracısı bağlantı noktası 443 üzerinden hata bildiriyor:

    #~> (11-20-2018 20:31:51):   ERROR  2508 8408 313 FAILED : PostToSVServer with error [at curlwrapper.cpp:CurlWrapper::processCurlResponse:212]   failed to post request: (7) - Couldn't connect to server
    #~> (11-20-2018 20:31:54):   ERROR  2508 8408 314 FAILED : PostToSVServer with error [at curlwrapper.cpp:CurlWrapper::processCurlResponse:212]   failed to post request: (7) - Couldn't connect to server
 
Diğer uygulamalara da bağlantı noktası 443 veya bağlantı noktası engelleyen bir güvenlik duvarı ayarı nedeniyle kullandığınızda bu hata ile.

Bu sorunu çözmek için:

- 443 numaralı bağlantı noktası güvenlik duvarı tarafından engellenmediğinden emin olun.
- Bağlantı noktası Bu bağlantı noktasını kullanarak başka bir uygulama nedeniyle ulaşılamaz durumdaysa durdurun ve uygulamayı kaldırın.
  - Uygulamayı durdurma yapmak uygun değilse, yeni bir temiz CS ayarlayın.
- Yapılandırma sunucusunu yeniden başlatın.
- IIS hizmetini yeniden başlatın.

### <a name="configuration-server-is-not-connected-due-to-incorrect-uuid-entries"></a>Yanlış UUID girişler nedeniyle yapılandırma sunucusu bağlı değil

Veritabanında birden çok yapılandırma sunucusu (CS) örneği UUID girişi olduğunda bu hata oluşabilir. VM yapılandırma Sunucusu'na kopyaladığınızda, sorun genellikle oluşur.

Bu sorunu çözmek için:

1. Eski/old CS VM, Vcenter'dan kaldırın. Daha fazla bilgi için [kaldırmak, sunucuları ve korumayı devre dışı](site-recovery-manage-registration-and-protection.md).
2. VM yapılandırma sunucusuna oturum açın ve MySQL svsdb1 veritabanına bağlanın. 
3. Aşağıdaki sorguyu yürütün:

    > [!IMPORTANT]
    >
    > UUID Ayrıntılar kopyalanan yapılandırma sunucusunun veya eski bir girişe artık sanal makineleri korumak için kullanılan yapılandırma sunucusunun girip girmediğinizi denetleyin. Bir yanlış UUID girerek tüm mevcut korumalı öğeler için bilgi kesilmesine neden olur.
   
    ```
        MySQL> use svsdb1;
        MySQL> delete from infrastructurevms where infrastructurevmid='<Stale CS VM UUID>';
        MySQL> commit; 
    ```
4. Portal sayfayı yenileyin.

## <a name="an-infinite-sign-in-loop-occurs-when-entering-your-credentials"></a>Kimlik bilgilerinizi girdikten sonsuz bir döngü oturum gerçekleşir

Azure'da oturum açın doğru kullanıcı adını ve parolayı yapılandırma sunucusunda OVF girdikten sonra doğru kimlik bilgilerini soracak şekilde devam eder.

Sistem saatini yanlış olduğunda bu sorun oluşabilir.

Bu sorunu çözmek için:

Bilgisayarda doğru saatini ayarlayın ve oturum açma yeniden deneyin. 
 