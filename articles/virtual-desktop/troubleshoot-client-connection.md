---
title: Uzak Masaüstü İstemcisi Windows sanal masaüstü - Azure bağlantıları
description: Windows sanal masaüstü müşterili bir ortamda istemci bağlantıları ayarladığınızda, sorunları gidermek nasıl.
services: virtual-desktop
author: ChJenk
ms.service: virtual-desktop
ms.topic: troubleshoot
ms.date: 04/08/2019
ms.author: v-chjenk
ms.openlocfilehash: 99295fd4581cd81751f7d64b694c853efe51a106
ms.sourcegitcommit: bb85a238f7dbe1ef2b1acf1b6d368d2abdc89f10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2019
ms.locfileid: "65522937"
---
# <a name="remote-desktop-client-connections"></a>Uzak Masaüstü istemcisi bağlantıları

Windows sanal masaüstü istemci bağlantıları ile ilgili sorunları gidermek için bu makaleyi kullanın.

## <a name="provide-feedback"></a>Geri bildirim gönder

Windows sanal masaüstü Önizleme aşamasındayken biz şu anda destek alma değildir. Ziyaret [Windows sanal masaüstü teknoloji topluluğuna](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop) etkin topluluk üyeleri ve ürün ekibine Windows sanal masaüstü hizmetiyle tartışmak için.

## <a name="you-cant-open-a-web-client"></a>Bir web istemcisi açılamıyor

Başka bir web sitesini açarak internet bağlantısı olduğunu doğrulayın; Örneğin, [www.Bing.com](https://www.bing.com).

Kullanım **nslookup** DNS FQDN giderebilir onaylamak için:

    ```cmd
    nslookup rdweb.wvd.microsoft.com
    ```

Windows 7 veya Windows 10 ve onay web istemcisi açarsanız görmek uzak masaüstü istemcisini gibi başka bir istemci ile bağlanmayı deneyin.

### <a name="error-opening-another-site-fails"></a>Hata: Başka bir site başarısız açma

**Neden:** Ağ sorunları ve/veya kesintiler.

**Düzeltme:** Ağ desteğine başvurun.

### <a name="error-nslookup-cannot-resolve-the-name"></a>Hata: Nslookup adı çözümlenemiyor

**Neden:** Ağ sorunları ve/veya kesintiler.

**Düzeltme:** Ağ desteğe başvurun

### <a name="error-you-cant-connect-but-other-clients-can-connect"></a>Hata: Bağlantı kurulamıyor ancak diğer istemciler bağlanabilir

**Neden:** Tarayıcı olarak beklenen ve durdurulmuş çalışma davranmayan.

**Düzeltme:** Tarayıcı sorunlarını gidermek için bu yönergeleri izleyin.

1. Tarayıcı yeniden başlatın.
2. Açık tarayıcı tanımlama bilgileri. Bkz: [Internet Explorer'da tanımlama bilgisi dosyalarını nasıl sileceğinizi](https://support.microsoft.com/help/278835/how-to-delete-cookie-files-in-internet-explorer).
3. Açık tarayıcı önbelleğini. Bkz: [tarayıcınızın tarayıcı önbelleğini Temizle](https://binged.it/2RKyfdU).
4. Açık tarayıcı özel modda.

## <a name="web-client-stops-responding-or-disconnects"></a>Web istemci yanıt vermiyor veya bağlantısını keser

Başka bir tarayıcıyı veya istemci kullanarak bağlanmayı deneyin.

### <a name="error-other-browsers-and-clients-also-malfunction-or-fail-to-open"></a>Hata: Diğer tarayıcılar ve istemciler, ayrıca çalışma hatası veya açılamayabiliyor

**Neden:** Ağ ve/veya işlem sistem sorunları veya kesintiler

**Düzeltme:** Ekipler desteğe başvurun.

## <a name="web-client-keeps-prompting-for-credentials"></a>Web istemcisi için kimlik bilgilerini isteyen tutar

Web istemcisi için kimlik bilgilerini isteyen tutar, bu yönergeleri izleyin.

1. Web istemci URL'nin doğru olduğunu onaylayın.
2. Kimlik bilgilerinin Windows sanal masaüstü ortamı URL'sine bağlı olduğunu onaylayın.
3. Açık tarayıcı tanımlama bilgileri. Bkz: [Internet Explorer'da tanımlama bilgisi dosyalarını nasıl sileceğinizi](https://support.microsoft.com/help/278835/how-to-delete-cookie-files-in-internet-explorer).
4. Açık tarayıcı önbelleğini. Bkz: [tarayıcınızın tarayıcı önbelleğini Temizle](https://binged.it/2RKyfdU).
5. Açık tarayıcı özel modda.

## <a name="remote-desktop-client-for-windows-7-or-windows-10-stops-responding-or-cannot-be-opened"></a>Windows 7 veya Windows 10 için Uzak Masaüstü İstemcisi yanıt vermiyor veya açılamıyor

Bant dışı (OOB) istemci kayıt defterleri temizlemek için aşağıdaki PowerShell cmdlet'lerini kullanın.

```PowerShell
Remove-ItemProperty 'HKCU:\Software\Microsoft\Terminal Server Client\Default' - Name FeedURLs

#Remove RdClientRadc registry key
Remove-Item 'HKCU:\Software\Microsoft\RdClientRadc' -Recurse

#Remove all files under %appdata%\RdClientRadc
Remove-Item C:\Users\pavithir\AppData\Roaming\RdClientRadc\* -Recurse
```

Gidin **%AppData%\RdClientRadc** ve tüm içeriğini silin.

Kaldırın ve Windows 7 ve Windows 10 için uzak masaüstü istemcisini yükleyin.

## <a name="troubleshooting-end-user-connectivity"></a>Son kullanıcı bağlantısı sorunlarını giderme

Bazen kullanıcılar kendi akış ve yerel kaynaklara erişim, ancak yine de yapılandırma, kullanılabilirlik veya uzak kaynaklara erişmesini engelleyen performans sorunlarını gerekir. Bu durumlarda, kullanıcı iletileri bunlara benzer alır:

![Uzak Masaüstü bağlantısı hata iletisi.](media/eb76b666808bddb611448dfb621152ce.png)

![Ağ geçidi hata iletisi bağlanamıyor.](media/a8fbb9910d4672147335550affe58481.png)

Bu istemci bağlantısı hata kodları için genel sorun giderme yönergeleri izleyin.

1. Kullanıcı adı ve saat sorun ne zaman karşılaşılmıştır onaylayın.
2. Açık **PowerShell** ve bağlantı sorunu olduğu bildirildi Windows sanal masaüstü kiracıya oluşturmaktır.
3. Doğru Kiracı için bağlantıyı doğrulamanız **Get-RdsTenant.**
4. Kullanarak **Get-RdsHostPool** ve **Get-RdsSessionHost** cmdlet'leri, sorun giderme doğru konak havuzunda yapıldığını onaylayın.
5. Belirtilen zaman penceresi için bağlantı türü tüm başarısız etkinlikler listesini almak için aşağıdaki komutu yürütün:

    ```cmd
     Get-RdsDiagnosticActivities -TenantName <TenantName> -username <UPN> -StartTime
     "11/21/2018 1:07:03 PM" -EndTime "11/21/2018 1:27:03 PM" -Outcome Failure -ActivityType Connection
    ```

6. Kullanarak **ActivityID** önceki cmdlet'inin çıktısı, aşağıdaki komutu çalıştırın:

    ```
    (Get-RdsDiagnosticActivities -TenantName $tenant -ActivityId <ActivityId> -Detailed).Errors
    ```

7. Komutu, aşağıda gösterilen çıktıya benzer bir çıktı üretir. Kullanım **ErrorCodeSymbolic** ve **ErrorMessage** kökenini giderilir.

    ```
    ErrorSource       : <Source>
    ErrorOperation    : <Operation>
    ErrorCode         : <Error code>
    ErrorCodeSymbolic : <Error code string>
    ErrorMessage      : <Error code message>
    ErrorInternal     : <Internal for the OS>
    ReportedBy        : <Reported by component>
    Time              : <Timestampt>
    ```

### <a name="error-oaddusertogroupfailed--failed-to-add-user--username-to-group--remote-desktop-users-reason-win32errornosuchmember"></a>Hata: O_ADD_USER_TO_GROUP_FAILED / kullanıcı ekleme başarısız ≤username≥ grubuna = Remote Desktop Users =. Neden: Win32.ERROR_NO_SUCH_MEMBER

**Neden:** VM kullanıcı nesnesi olduğu etki alanına değil.

**Düzeltme:** VM doğru etki alanına ekleyin. Bkz: [bir Windows Server sanal makinesi için yönetilen etki alanına Katıl](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-admin-guide-join-windows-vm-portal).

### <a name="error-nslookup-cannot-resolve-the-name"></a>Hata: Nslookup adı çözümlenemiyor

**Neden:** Ağ sorunları veya kesintiler.

**Düzeltme:** Ağ desteğe başvurun

### <a name="error-connectionfailedclientprotocolerror"></a>Hata: ConnectionFailedClientProtocolError

**Neden:** Vm'leri kullanıcının bağlanmaya çalışıyor etki alanına katılmış değildir.

**Düzeltme:** Bir etki alanı denetleyicisinin ana havuzuna parçası olan tüm sanal makineler katılın.

## <a name="user-connects-but-nothing-is-displayed-no-feed"></a>Kullanıcı bağlanır ancak hiçbir şey görüntülenmez (akış)

Bir kullanıcı, Uzak Masaüstü istemcilerini başlayabilir ve kimlik doğrulaması için kullanıcı akışı web bulma tüm simgeleri ancak görmez.

Bu komut satırını kullanarak sorunları raporlama kullanıcı uygulama gruplarına atanmış onaylayın:

```cmd
Get-RdsAppGroupUser <tenantname> <hostpoolname> <appgroupname>
```

Kullanıcı doğru kimlik bilgileriyle oturum olduğunu onaylayın.

Web istemcisi kullanılıyorsa, önbelleğe alınmış kimlik bilgilerini sorun olduğundan emin olun.

## <a name="next-steps"></a>Sonraki adımlar

- Windows sanal masaüstü ve yükseltme parçaları sorun giderme hakkında genel bir bakış için bkz: [genel bakış, geri bildirim ve destek sorunlarını giderme](troubleshoot-set-up-overview.md).
- Bir Windows sanal masaüstü ortamında bir kiracı ve konak havuzu oluşturulurken sorunlarını gidermek için bkz: [Kiracı ve konak havuz oluşturma](troubleshoot-set-up-issues.md).
- Bir sanal makine (VM) Windows sanal masaüstü yapılandırılırken sorunlarını gidermek için bkz: [oturumu ana bilgisayar Sanal Makine Yapılandırması](troubleshoot-vm-configuration.md).
- PowerShell ile Windows sanal masaüstü kullanırken sorunlarını gidermek için bkz: [Windows sanal masaüstü PowerShell](troubleshoot-powershell.md).
- Önizleme hizmeti hakkında daha fazla bilgi için bkz: [Windows Desktop Önizleme ortamı](https://docs.microsoft.com/azure/virtual-desktop/environment-setup?).
- Bir sorun giderme öğreticiyi incelemek için bkz: [Öğreticisi: Resource Manager şablonu dağıtımlardaki sorunları çözmek](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-tutorial-troubleshoot).
- Eylemler denetleme hakkında bilgi edinmek için [Resource Manager denetim işlemleri](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-audit).
- Dağıtım sırasında hataları belirlemek için Eylemler hakkında bilgi edinmek için bkz. [dağıtım işlemlerini görüntüleme](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-operations).