---
title: Windows sanal masaüstü - Azure kiracısı ve konak havuzu oluşturma
description: Bir kiracı ve oturumu konak sanal makine (VM) bir Windows sanal masaüstü ortamında yapılandırırken, sorunları gidermek nasıl.
services: virtual-desktop
author: ChJenk
ms.service: virtual-desktop
ms.topic: troubleshooting
ms.date: 07/10/2019
ms.author: v-chjenk
ms.openlocfilehash: 96a9d8fc7495ea473b0a3250b34251afc5f30c13
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67786720"
---
# <a name="tenant-and-host-pool-creation"></a>Kiracı ve ana bilgisayar havuzu oluşturma

Windows Sanal Masaüstü Oturumu Ana bilgisayar sanal makineleri (VM'ler) yapılandırma sırasında karşılaştığınız sorunları gidermek için bu makaleyi kullanın.

## <a name="provide-feedback"></a>Geri bildirimde bulunma

Windows sanal masaüstü Önizleme aşamasındayken biz şu anda destek alma değildir. Ziyaret [Windows sanal masaüstü teknoloji topluluğuna](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop) etkin topluluk üyeleri ve ürün ekibine Windows sanal masaüstü hizmetiyle tartışmak için.

## <a name="vms-are-not-joined-to-the-domain"></a>Sanal makineleri etki alanına katılmamış

Sanal makineleri etki alanına katılma sorunları yaşıyorsanız, aşağıdaki yönergeleri izleyin.

- İşlemde kullanarak el ile VM'sine katılma [bir Windows Server sanal makinesi için yönetilen etki alanına Katıl](https://docs.microsoft.com/azure/active-directory-domain-services/Active-directory-ds-admin-guide-join-windows-vm-portal) veya bu adı kullanıyor [etki alanına katılım şablon](https://azure.microsoft.com/resources/templates/201-vm-domain-join-existing/).
- Etki alanı adı VM'deki komut satırından ping işlemi yapmayı deneyin.
- Etki alanı katılma hatası iletileri listesini gözden geçirin [etki alanı katılma hata iletileri sorunlarını giderme](https://social.technet.microsoft.com/wiki/contents/articles/1935.troubleshooting-domain-join-error-messages.aspx).

### <a name="error-incorrect-credentials"></a>Hata: Yanlış kimlik bilgileri

**Neden:** Azure Resource Manager şablonu arabirimi düzeltmeleri kimlik bilgileri girilen yapılan yazım hatası oluştu.

**Düzeltme:** Kimlik bilgilerini düzeltmek için bu yönergeleri izleyin.

1. El ile Vm'leri bir etki alanına ekleyin.
2. Kimlik bilgileri doğruladıktan sonra yeniden dağıtın. Bkz: [PowerShell ile bir konak havuz oluşturma](https://docs.microsoft.com/azure/virtual-desktop/create-host-pools-powershell).
3. VM'ler ile bir şablonu kullanarak bir etki alanına ekleme işlemini [var olan bir Windows VM AD etki alanına katılan](https://azure.microsoft.com/resources/templates/201-vm-domain-join-existing/).

### <a name="error-timeout-waiting-for-user-input"></a>Hata: Kullanıcı girişi beklenirken zaman aşımı

**Neden:** Etki alanına katılım tamamlamak için kullanılan hesap çok faktörlü kimlik doğrulaması (MFA) olabilir.

**Düzeltme:** Etki alanına katılım tamamlamak için bu yönergeleri izleyin.

1. Geçici olarak hesap için mfa'yı kaldırın.
2. Bir hizmet hesabı kullanın.

### <a name="error-the-account-used-during-provisioning-doesnt-have-permissions-to-complete-the-operation"></a>Hata: Sağlama sırasında kullanılan hesap işlemi tamamlamak için izinlere sahip değil

**Neden:** Kullanılan hesap, VM'lerin uyumluluk ve düzenlemeleri nedeniyle etki alanına katılmak üzere izinlere sahip değil.

**Düzeltme:** Bu yönergeleri izleyin.

1. Yönetici grubunun bir üyesi olan bir hesap kullanın.
2. Kullanılmakta olan hesap için gerekli izinleri verin.

### <a name="error-domain-name-doesnt-resolve"></a>Hata: Etki alanı adı sorunu çözmezse

**1. neden:** Etki alanı bulunduğu ile sanal ağa (VNET) ilişkili olmayan bir kaynak grubundaki vm'leridir.

**1 düzeltin:** Vm'leri burada sağlanan sanal ağ ile etki alanı denetleyicisi (DC) çalıştığı sanal ağ arasında eşleme sanal ağ oluşturun. Bkz: [oluşturma bir sanal ağ eşlemesi - Resource Manager, farklı Aboneliklerde](https://docs.microsoft.com/azure/virtual-network/create-peering-different-subscriptions).

**2. neden:** AadService (AADS) kullanırken, DNS girişlerini ayarlanmamış.

**2 düzeltin:** Etki Alanı Hizmetleri ayarlamak için bkz [etkinleştirme Azure Active Directory Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-getting-started-dns).

## <a name="windows-virtual-desktop-agent-and-windows-virtual-desktop-boot-loader-are-not-installed"></a>Sanal Masaüstü Aracısı Windows ve Windows sanal masaüstü önyükleyicinin yüklü değil

VM'ler sağlamak için önerilen yöntem, Azure Resource Manager kullanarak **oluşturma ve sağlama Windows sanal masaüstü havuzu konak** şablonu. Şablon, Windows sanal masaüstü aracı ve Windows sanal masaüstü aracı önyükleme yükleyicisi otomatik olarak yükler.

Bileşenler yüklü onaylamak ve hata iletilerini kontrol için bu yönergeleri izleyin.

1. İki bileşeni işaretleyerek yüklendiğini onaylayın **Denetim Masası** > **programlar** > **programlar ve Özellikler**. Varsa **Windows sanal masaüstü aracı** ve **Windows sanal masaüstü aracı önyükleme yükleyicisi** görünmez sanal makinede yüklü olmayan,.
2. Açık **dosya Gezgini** gidin **C:\Windows\Temp\scriptlogs.log**. Dosya eksikse, iki bileşeni yüklü PowerShell DSC sağlanan güvenlik bağlamında çalışacak şekilde mümkün olmadığını gösterir.
3. Varsa dosyayı **C:\Windows\Temp\scriptlogs.log** sunmak, açın ve hata iletilerini denetleyin.

### <a name="error-windows-virtual-desktop-agent-and-windows-virtual-desktop-agent-boot-loader-are-missing-cwindowstempscriptlogslog-is-also-missing"></a>Hata: Sanal Masaüstü Aracısı Windows ve Windows sanal masaüstü aracı önyükleme yükleyicisi eksik. C:\Windows\Temp\scriptlogs.log de eksik

**1. neden:** Azure Resource Manager şablonu giriş sırasında sağlanan kimlik bilgileri yanlış veya izinleri yetersiz.

**1 düzeltin:** Eksik bileşenleri kullanılarak Vm'lere el ile eklemeniz [PowerShell ile bir konak havuz oluşturma](https://docs.microsoft.com/azure/virtual-desktop/create-host-pools-powershell).

**2. neden:** PowerShell DSC başlatmak ve yürütmek kullanabilirsiniz, ancak Windows sanal masaüstü oturum açamaz ve gerekli bilgileri almak gibi tamamlamak başarısız oldu.

**2 düzeltin:** Aşağıdaki listedeki öğeleri doğrulayın.

- Hesap MFA olmadığından emin olun.
- Kiracı adı doğru olduğundan ve Windows sanal masaüstü kiracısı mevcut onaylayın.
- Hesabın sahip en az onaylayın RDS katkıda bulunan izinleri.

### <a name="error-authentication-failed-error-in-cwindowstempscriptlogslog"></a>Hata: Kimlik doğrulaması başarısız oldu, hata C:\Windows\Temp\scriptlogs.log

**Neden:** PowerShell DSC yürütebilmek için ancak sanal masaüstü Windows ile bağlantı kurulamadı.

**Düzeltme:** Aşağıdaki listedeki öğeleri doğrulayın.

- El ile Vm'leri sanal masaüstü Windows hizmeti ile kaydedin.
- Windows sanal masaüstüne bağlanmak için kullanılan hesap ana bilgisayar havuzları oluşturmak için Kiracı izinlere sahip olduğunu doğrulayın.
- Hesap MFA yok onaylayın.

## <a name="windows-virtual-desktop-agent-is-not-registering-with-the-windows-virtual-desktop-service"></a>Windows sanal masaüstü aracı Windows sanal masaüstü hizmetiyle kaydetmemektedir

Windows sanal masaüstü aracı oturum ilk yüklendiğinde VM'lerin ana bilgisayar (ya da el ile veya Azure Resource Manager şablonu ve aracılığıyla PowerShell DSC), bir kayıt belirtecinizi sağlar. Aşağıdaki bölümde, belirteç ve Windows sanal masaüstü aracı ile ilgili sorun giderme konuları kapsar.

### <a name="error-the-status-filed-in-get-rdssessionhost-cmdlet-shows-status-as-unavailable"></a>Hata: Get-RdsSessionHost cmdlet'inde Dosyalanan durumu durumu kullanılamaz gösterilir.

![Get-RdsSessionHost cmdlet'i, kullanılamaz durumunu gösterir.](media/23b8e5f525bb4e24494ab7f159fa6b62.png)

**Neden:** Aracıyı yeni bir sürüme güncelleştirme kendisini mümkün değildir.

**Düzeltme:** Aracıyı el ile güncelleştirmek için bu yönergeleri izleyin.

1. Aracı VM oturumu ana bilgisayarı üzerinde yeni bir sürümünü indirin.
2. Görev Yöneticisi'ni başlatın ve hizmet sekmede RDAgentBootLoader hizmetini durdurun.
3. Windows sanal masaüstü Aracısı'nın yeni sürümü için yükleyiciyi çalıştırın.
4. Tuşuna basın ve giriş INVALID_TOKEN yönelik kayıt belirtecini istendiğinde Kaldır sonraki (yeni bir belirteç gerekli değildir).
5. Yükleme Sihirbazı'nı tamamlayın.
6. Görev Yöneticisi'ni açın ve RDAgentBootLoader hizmetini başlatın.

## <a name="error--windows-virtual-desktop-agent-registry-entry-isregistered-shows-a-value-of-0"></a>Hata:  Sanal Masaüstü Aracısı Windows kayıt defteri girdisini IsRegistered 0 değerini gösterir.

**Neden:** Kayıt belirtecinizi süresi doldu veya zaman aşımı değeri ile 999999 oluşturuldu.

**Düzeltme:** Aracı kayıt defteri hatayı düzeltmek için bu yönergeleri izleyin.

1. Bir kayıt belirtecinizi ise, Remove-RDSRegistrationInfo ile kaldırın.
2. Rds NewRegistrationInfo ile yeni bir belirteç oluşturur.
3. -ExpriationHours parametresi için 72 ayarlandığından emin olun (99999 aralığında olan en yüksek değer).

### <a name="error-windows-virtual-desktop-agent-isnt-reporting-a-heartbeat-when-running-get-rdssessionhost"></a>Hata: Get-RdsSessionHost çalıştırırken Windows sanal masaüstü aracı sinyal raporlama değil

**1. neden:** RDAgentBootLoader hizmeti durduruldu.

**1 düzeltin:** Görev Yöneticisi'ni başlatın ve hizmet sekmesini RDAgentBootLoader hizmetinin durdurulmuş bir durum bildirirse, hizmeti başlatın.

**2. neden:** Bağlantı noktası 443 kapalı olabilir.

**2 düzeltin:** 443 numaralı bağlantı noktasını açmak için bu yönergeleri izleyin.

1. 443 numaralı bağlantı noktası açık PSPing aracından indirerek onaylayın [Sysinternal Araçları](https://docs.microsoft.com/sysinternals/downloads/psping).
2. PSPing VM aracısının çalıştığı oturum konakta yükleyin.
3. Yönetici olarak bir komut istemi açın ve aşağıdaki komutu yürütün:

    ```cmd
    psping rdbroker.wvdselfhost.microsoft.com:443
    ```

4. Alınan PSPing bilgilerin geri RDBroker onaylayın:

    ```
    PsPing v2.10 - PsPing - ping, latency, bandwidth measurement utility
    Copyright (C) 2012-2016 Mark Russinovich
    Sysinternals - www.sysinternals.com
    TCP connect to 13.77.160.237:443:
    5 iterations (warmup 1) ping test:
    Connecting to 13.77.160.237:443 (warmup): from 172.20.17.140:60649: 2.00ms
    Connecting to 13.77.160.237:443: from 172.20.17.140:60650: 3.83ms
    Connecting to 13.77.160.237:443: from 172.20.17.140:60652: 2.21ms
    Connecting to 13.77.160.237:443: from 172.20.17.140:60653: 2.14ms
    Connecting to 13.77.160.237:443: from 172.20.17.140:60654: 2.12ms
    TCP connect statistics for 13.77.160.237:443:
    Sent = 4, Received = 4, Lost = 0 (0% loss),
    Minimum = 2.12ms, Maximum = 3.83ms, Average = 2.58ms
    ```

## <a name="troubleshooting-issues-with-the-windows-virtual-desktop-side-by-side-stack"></a>Windows sanal masaüstü yan yana yığını sorunlarını giderme

Windows sanal masaüstü yan yana yığın, Windows Server 2019 ile otomatik olarak yüklenir. Yan yana yığını'nı Microsoft Windows Server 2016 veya Windows Server 2012 R2 üzerinde yükleme için Microsoft Installer (MSI) kullanın. Microsoft Windows 10 için Windows sanal masaüstü yan yana yığın ile etkin **enablesxstackrs.ps1**.

Yan yana yığın alır veya yüklü oturumu konak havuzu Vm'leri üzerinde etkin başlıca üç yolu vardır:

- Azure Resource Manager ile **oluşturma ve sağlama yeni sanal Windows Masaüstü konak havuzu** şablonu
- Dahil ve ana görüntü üzerinde etkin olarak
- Yüklü veya el ile her VM üzerindeki (veya uzantıları/PowerShell ile) etkin

Windows sanal masaüstü yan yana yığını ile ilgili sorunlar yaşıyorsanız yazın **qwinsta** yan yana yığın yüklenmemişse veya etkin olduğunu onaylamak için komut isteminden komutu.

Çıkışı **qwinsta** listeler **rdp sxs** yan yana yığınının yüklü ve etkinse çıktı.

![Yüklü veya etkin olarak rdp-sxs çıktıda listelenen qwinsta ile yan yana yığını.](media/23b8e5f525bb4e24494ab7f159fa6b62.png)

Aşağıda listelenen kayıt defteri girişlerini inceleyin ve değerlerine eşleştiğini doğrulayın. Kayıt defteri anahtarları eksik ya da değerleri eşleşmeyen'ndaki yönergeleri izleyin [PowerShell ile bir konak havuz oluşturma](https://docs.microsoft.com/azure/virtual-desktop/create-host-pools-powershell) nasıl yan yana yığını yeniden yükleyin.

```registry
    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal
    Server\WinStations\rds-sxs\"fEnableWinstation":DWORD=1

    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal
    Server\ClusterSettings\"SessionDirectoryListener":rdp-sxs
```

### <a name="error-oreverseconnectstackfailure"></a>Hata: O_REVERSE_CONNECT_STACK_FAILURE

![O_REVERSE_CONNECT_STACK_FAILURE hata kodu.](media/23b8e5f525bb4e24494ab7f159fa6b62.png)

**Neden:** Yan yana yığın oturumu ana bilgisayarı VM üzerinde yüklü değil.

**Düzeltme:** Yan yana yığın oturumu ana bilgisayarı VM üzerinde yüklemek için bu yönergeleri izleyin.

1. Uzak Masaüstü Protokolü (RDP) oturumu konağı VM yerel yönetici olarak doğrudan almak için kullanın.
2. İndirme ve içeri aktarma [Windows sanal masaüstü PowerShell Modülü](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview) henüz yapmadıysanız, PowerShell oturumunuzda kullanılacak.
3. Yan yana yığını kullanarak yükleme [PowerShell ile bir konak havuz oluşturma](https://docs.microsoft.com/azure/virtual-desktop/create-host-pools-powershell).

## <a name="how-to-fix-a-windows-virtual-desktop-side-by-side-stack-that-malfunctions"></a>Nasıl düzgün bir Windows sanal masaüstü yan yana yığını

Yan yana yığın çalışmasına neden olabilir durumlarda bilinen vardır:

- Yan yana yığın etkinleştirmek için doğru sırada adımları takip edilmiyor
- Otomatik güncelleştirme için Windows 10 Gelişmiş çok yönlü diski (EVD)
- Uzak Masaüstü oturumu ana bilgisayarı (RDSH) rolü eksik
- Birden çok kez enablesxsstackrc.ps1 çalıştırma
- Bir hesapta enablesxsstackrc.ps1 çalıştıran yerel yönetici ayrıcalıklarına sahip değil

Bu bölümdeki yönergelere Windows sanal masaüstü yan yana yığın kaldırmanıza yardımcı olabilir. "VM ile Windows sanal masaüstü konak havuzu kaydetmek için" yan yana yığın kaldırdıktan sonra Buraya [PowerShell ile bir konak havuz oluşturma](https://docs.microsoft.com/azure/virtual-desktop/create-host-pools-powershell) yan yana yığını yeniden yüklemek için.

Düzeltme çalıştırmak için kullanılan VM VM yapıyor yan yana yığın ile aynı alt ağ ve etki alanı olmalıdır.

Aynı alt ağ ve etki alanı düzeltme çalıştırmak için aşağıdaki yönergeleri izleyin:

1. Standart Uzak Masaüstü Protokolü (RDP ile), burada düzeltme uygulanacak gelen VM'ye bağlanın.
2. PsExec'nden indirin https://docs.microsoft.com/sysinternals/downloads/psexec.
3. İndirilen dosyanın sıkıştırmasını açın.
4. Yerel yönetici olarak komut istemini başlatın.
5. PsExec sıkıştırması olduğu klasöre gidin.
6. Komut isteminden aşağıdaki komutu kullanın:

    ```cmd
            psexec.exe \\<VMname> cmd
    ```

    >[!Note]
    >VMname yapıyor yan yana yığın ile sanal makine adıdır.

7. PsExec lisans sözleşmesini kabul et tıklayarak kabul edin.

    ![Yazılım Lisans Sözleşmesi ekran görüntüsü.](media/SoftwareLicenseTerms.png)

    >[!Note]
    >Bu iletişim kutusunu PsExec çalıştırmak yalnızca ilk kez gösterir.

8. VM'de yapıyor yan yana yığınına sahip bir komut istemi oturumu açıldıktan sonra qwinsta çalıştırın ve rdp sxs adlı bir giriş kullanılabilir olduğundan emin olun. Aksi halde, sorunu yan yana yığına bağlı değildir, dolayısıyla bir yan yana yığını sanal makinede mevcut değil.

    ![Yönetici komut istemi](media/AdministratorCommandPrompt.png)

9. Düzgün çalışmayan bir yan yana yığın VM'de yüklü Microsoft bileşenleri listeler aşağıdaki komutu çalıştırın.

    ```cmd
        wmic product get name
    ```

10. Aşağıdaki komutta, Yukarıdaki adımda ürün adları ile çalıştırın.

    ```cmd
        wmic product where name="<Remote Desktop Services Infrastructure Agent>" call uninstall
    ```

11. "Uzak Masaüstü ile" ile başlayan tüm ürünleri kaldırın

12. Tüm Windows sanal masaüstü bileşenleri kaldırıldıktan sonra bir işletim sistemi için yönergeleri izleyin:

13. İşletim sisteminiz Windows Server ise yapıyor yan yana yığını (veya Azure portalı ile PsExec aracından) olan bir VM'yi yeniden başlatın.

Microsoft Windows 10 işletim sisteminizin ise aşağıdaki yönergeleri ile devam edin:

14. PsExec çalıştıran VM'den dosya Gezgini'ni açın ve sistem sürücüsünün kökündeki malfunctioned yan yana yığınına sahip bir VM disablesxsstackrc.ps1 kopyalayın.

    ```cmd
        \\<VMname>\c$\
    ```

    >[!NOTE]
    >VMname yapıyor yan yana yığın ile sanal makine adıdır.

15. Önerilen işlemi: PsExec aracından PowerShell'i başlatın ve önceki adımdaki klasöre gidin ve disablesxsstackrc.ps1 çalıştırın. Alternatif olarak, aşağıdaki cmdlet'leri çalıştırın:

    ```PowerShell
    Remove-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\ClusterSettings" -Name "SessionDirectoryListener" -Force
    Remove-Item -Path "HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\rdp-sxs" -Recurse -Force
    Remove-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations" -Name "ReverseConnectionListener" -Force
    ```

16. Cmdlet tamamlanınca yapıyor yan yana yığınına sahip bir VM çalıştıran, yeniden başlatın.

## <a name="remote-licensing-model-is-not-configured"></a>Uzak bir lisanslama modeli yapılandırılmadı

Bir yönetici hesabı kullanarak Windows 10 Enterprise çok oturumuna oturum açarsanız, şunu bir bildirim alabilirsiniz "Uzak Masaüstü lisans modunu yapılandırılmamış, Uzak Masaüstü Hizmetleri, X çalışmayı durdurur gün. Bağlantı Aracısı sunucusunda, Sunucu Yöneticisi Uzak Masaüstü lisanslama modunu belirtmek için kullanın." Bu iletiyi görürseniz anlamına, lisanslama modu için el ile yapılandırmanız gereken **kullanıcı başına**.

Lisanslama modu el ile yapılandırmak için:  

1. Git, **Başlat menüsü** arama kutusuna sonra bulma ve açma **gpedit.msc** yerel Grup İlkesi Düzenleyicisi'ne erişmek için. 
2. Git **Bilgisayar Yapılandırması** > **Yönetim Şablonları** > **Windows bileşenleri**  >   **Uzak Masaüstü Hizmetleri** > **Uzak Masaüstü oturumu konağı** > **lisanslama**. 
3. Seçin **Uzak Masaüstü lisans modunu ayarla** ve değiştirmek için **kullanıcı başına**.

Biz şu anda bildirim ve yetkisiz kullanım süresi zaman aşımı sorunlarını bulmak ve gelecek bir güncelleştirmede yönelik planlayın. 

## <a name="next-steps"></a>Sonraki adımlar

- Windows sanal masaüstü ve yükseltme parçaları sorun giderme hakkında genel bir bakış için bkz: [genel bakış, geri bildirim ve destek sorunlarını giderme](troubleshoot-set-up-overview.md).
- Bir Windows sanal masaüstü ortamında bir kiracı ve konak havuzu oluşturulurken sorunlarını gidermek için bkz: [Kiracı ve konak havuz oluşturma](troubleshoot-set-up-issues.md).
- Bir sanal makine (VM) Windows sanal masaüstü yapılandırılırken sorunlarını gidermek için bkz: [oturumu ana bilgisayar Sanal Makine Yapılandırması](troubleshoot-vm-configuration.md).
- Windows sanal masaüstü istemci bağlantıları ile ilgili sorunları gidermek için bkz: [Uzak Masaüstü istemci bağlantıları](troubleshoot-client-connection.md).
- PowerShell ile Windows sanal masaüstü kullanırken sorunlarını gidermek için bkz: [Windows sanal masaüstü PowerShell](troubleshoot-powershell.md).
- Önizleme hizmeti hakkında daha fazla bilgi için bkz: [Windows Desktop Önizleme ortamı](https://docs.microsoft.com/azure/virtual-desktop/environment-setup).
- Bir sorun giderme öğreticiyi incelemek için bkz: [Öğreticisi: Resource Manager şablonu dağıtımlardaki sorunları çözmek](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-tutorial-troubleshoot).
- Eylemler denetleme hakkında bilgi edinmek için [Resource Manager denetim işlemleri](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-audit).
- Dağıtım sırasında hataları belirlemek için Eylemler hakkında bilgi edinmek için bkz. [dağıtım işlemlerini görüntüleme](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-operations).