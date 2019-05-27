---
title: Linux'ta Azure dosyaları sorunlarını giderme | Microsoft Docs
description: Linux'ta Azure dosyaları sorunlarını giderme
services: storage
author: jeffpatt24
tags: storage
ms.service: storage
ms.topic: article
ms.date: 10/16/2018
ms.author: jeffpatt
ms.subservice: files
ms.openlocfilehash: 06b3a5110bfdea2a2067979c806701011dc16f3d
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65987679"
---
# <a name="troubleshoot-azure-files-problems-in-linux"></a>Linux'ta Azure dosyaları sorunlarını giderme

Bu makalede Linux istemcilerden bağladığınızda, Azure dosyaları'na ilgili genel sorunları listeler. Ayrıca olası nedenleri ve çözümlemeleri için bu sorunları sağlar. 

Bu makalede sorun giderme adımları ek olarak, kullandığınız [AzFileDiagnostics](https://gallery.technet.microsoft.com/Troubleshooting-tool-for-02184089) Linux istemci önkoşulları doğru olduğundan emin olmak için. Bu makalede değinilen belirtileri çoğunu algılanması AzFileDiagnostics otomatikleştirir. En iyi performansı elde etmek için ortamınızı ayarlama yardımcı olur. Bu bilgiler de bulabilirsiniz [Azure dosyaları paylaşımlarını sorun giderici](https://support.microsoft.com/help/4022301/troubleshooter-for-azure-files-shares). Sorun giderici bağlanma, eşleme ve Azure dosya paylaşımlarını oluşturma sorunları yardımcı olması için adımları sağlar.

<a id="mounterror13"></a>
## <a name="mount-error13-permission-denied-when-you-mount-an-azure-file-share"></a>"Error(13) bağlayın: İzin reddedildi"Azure dosya paylaşımını bağlama zaman

### <a name="cause-1-unencrypted-communication-channel"></a>1. neden: Şifrelenmemiş iletişim kanalı

Güvenlik nedenleriyle, Azure dosya paylaşımlarını bağlantı iletişim kanalını şifreli değildir ve Azure dosya paylaşımlarını bulunduğu aynı veri merkezlerinden bağlantı girişimi yapılmadan değil engellenir. Aynı veri merkezindeki şifrelenmemiş bağlantıları da ise engellenir [güvenli aktarım gerekli](https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer) depolama hesabı seçeneği etkinleştirilmiştir. Şifreli iletişim kanalı, yalnızca kullanıcının istemci işletim sistemi SMB şifrelemesi destekliyorsa sağlanır.

Daha fazla bilgi için bkz. [Linux ve CIFS-utils paketi ile bir Azure dosya bağlama önkoşulları paylaşır](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-linux#prerequisites-for-mounting-an-azure-file-share-with-linux-and-the-cifs-utils-package). 

### <a name="solution-for-cause-1"></a>Çözüm nedeni 1 için

1. SMB Şifrelemesi'ni destekleyen bir istemciyi bağlanmak veya aynı veri merkezinde Azure dosya paylaşımı için kullanılan Azure depolama hesabı olarak bir sanal makinesinden bağlanabilirsiniz.
2. Doğrulama [güvenli aktarım gerekli](https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer) ayarı devre dışı depolama hesabında istemci SMB şifrelemesi desteklemiyorsa.

### <a name="cause-2-virtual-network-or-firewall-rules-are-enabled-on-the-storage-account"></a>2. neden: Sanal ağ veya güvenlik duvarı kuralları depolama hesabı etkinleştirilir 

Sanal ağ (VNET) ve güvenlik duvarı kuralları depolama hesabında yapılandırılmışsa, istemci IP adresi veya sanal ağ erişimine izin verilmesini sürece ağ trafiğini erişimi reddedilir.

### <a name="solution-for-cause-2"></a>Neden 2 çözümü

Sanal ağ ve güvenlik duvarı kuralları depolama hesabı düzgün şekilde yapılandırıldığından doğrulayın. Sanal ağ veya güvenlik duvarı kuralları neden sorun varsa test etmek için geçici olarak depolama hesabı için ayarı değiştirmeniz **tüm ağlardan erişime izin ver**. Daha fazla bilgi için bkz. [yapılandırma Azure depolama güvenlik duvarlarını ve sanal ağlar](https://docs.microsoft.com/azure/storage/common/storage-network-security).

<a id="permissiondenied"></a>
## <a name="permission-denied-disk-quota-exceeded-when-you-try-to-open-a-file"></a>"[izin reddedildi;] Disk kotası aşıldı" bir dosyayı açmaya çalıştığınızda

Linux'ta, aşağıdakine benzer bir hata iletisi alırsınız:

**\<Dosya adı > [izin reddedildi;] Disk kotası aşıldı**

### <a name="cause"></a>Nedeni

Bir dosya için izin verilen eşzamanlı açık tanıtıcıları üst sınırına ulaştınız.

### <a name="solution"></a>Çözüm

Bazı işler kapatarak eşzamanlı açık işleyicilerin sayısını azaltın ve sonra işlemi yeniden deneyin. Daha fazla bilgi için [Microsoft Azure depolama performansı ve ölçeklenebilirlik denetim listesi](../common/storage-performance-checklist.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

<a id="slowfilecopying"></a>
## <a name="slow-file-copying-to-and-from-azure-files-in-linux"></a>Dosya ve Linux'ta Azure dosyalarından kopyalamak yavaş

- Belirli bir en düşük g/ç boyutu gereksinimi yoksa, en iyi performans için 1 MiB g/ç boyutu kullanmanızı öneririz.
- Yazma işlemleri kullanarak genişletme bir dosyanın son boyutu bildiğiniz ve dosya çubuğunda yazılı bir kuyruk sıfır içerdiğinde yazılımınızı uyumluluk sorunları yaşıyorsanız değil, ardından önceden her bir genişletme yazma yapmak yerine dosya boyutunu ayarlayın.
- Doğru kopyalama yöntemi kullanın:
    - Kullanım [AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) iki dosya paylaşımları arasındaki tüm aktarımları için.
    - Kullanım [Robocopy](https://blogs.msdn.microsoft.com/granth/2009/12/07/multi-threaded-robocopy-for-faster-copies/) bir şirket içi bilgisayarda dosya paylaşımları arasında.

<a id="error112"></a>
## <a name="mount-error112-host-is-down-because-of-a-reconnection-time-out"></a>"Error(112) bağlayın: Yeniden bağlanma zaman aşımı nedeniyle konak kapalı"

Bir "112" bağlama hatası istemci uzun bir süredir boşta Linux istemcide gerçekleşir. Genişletilmiş bir boşta kalma süresi sonra istemci bağlantısını keser ve bağlantı zaman aşımına uğrar.  

### <a name="cause"></a>Nedeni

Bağlantıyı aşağıdaki nedenlerle boşta olabilir:

-   Varsayılan "soft" Bağlama seçeneği kullanıldığında sunucuya bir TCP bağlantı yeniden oluşturma engelleyen ağ iletişim hatası
-   Eski çekirdekler mevcut olmayan son yeniden düzeltmeleri

### <a name="solution"></a>Çözüm

Linux çekirdeğinin bu yeniden bağlanma sorunu artık aşağıdaki değişiklikler bir parçası olarak düzeltildi:

- [Düzeltme yeniden smb3 oturumu erteleme değil uzun yuva yeniden bağlandıktan sonra yeniden bağlanın](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/fs/cifs?id=4fcd1813e6404dd4420c7d12fb483f9320f0bf93)
- [Yuva hemen yeniden bağlandıktan sonra echo hizmet çağrısı](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=b8c600120fc87d53642476f48c8055b38d6e14c7)
- [CIFS: Yeniden bağlanma sırasında bir olası bellek bozulmayı düzeltmek](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/?id=53e0e11efe9289535b060a51d4cf37c25e0d0f2b)
- [CIFS: Bir olası çift mutex (çekirdek v4.9 ve üzeri) yeniden bağlanma sırasında kilitleme Düzelt](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/?id=96a988ffeb90dba33a71c3826086fe67c897a183)

Ancak, bu değişiklikler henüz tüm Linux dağıtımları için unity'nin değil. Bu düzeltme ve diğer yeniden düzeltmeleri şu popüler Linux çekirdeklerinin şunlardır: 4.4.40 4.8.16 ve 4.9.1. Bu önerilen çekirdek sürümlerinden birini yükselterek bu düzeltme elde edebilirsiniz.

### <a name="workaround"></a>Geçici Çözüm

Bir sabit bağlanması belirterek bu sorunu geçici olarak çalışabilir. Sabit bağlama bağlantı kurulana kadar veya açıkça kesilene kadar beklenecek istemci zorlar. Ağ zaman aşımı nedeniyle hataları önlemek için kullanabilirsiniz. Ancak, bu geçici çözüm belirsiz bekler neden olabilir. Bağlantıları gerektiği gibi durdurmak hazırlıklı olun.

En son çekirdek sürümlerine yükseltme yapamazsınız, her 30 saniyede bir veya daha az yazma Azure Dosya paylaşımındaki dosya tutarak bu sorunu geçici olarak çalışabilir. Bu dosya üzerinde oluşturulan veya değiştirilen tarihi yeniden yazma gibi bir yazma işlemi olması gerekir. Aksi takdirde, önbelleğe alınan sonuçları alabilirsiniz ve işleminizi yeniden tetikleyebilir değil.

<a id="error115"></a>
## <a name="mount-error115-operation-now-in-progress-when-you-mount-azure-files-by-using-smb-30"></a>"Error(115) bağlayın: İşlem Sürüyor"olduğunda, bağlama Azure dosyaları SMB 3.0 kullanarak

### <a name="cause"></a>Nedeni

Bazı Linux dağıtımlarında, henüz de SMB 3.0 şifreleme özellikleri desteklemez. Eksik bir özellik nedeniyle SMB 3.0 kullanarak bağlama Azure dosyaları'na çalıştıklarında, kullanıcıların bir "115" hata iletisi alabilirsiniz. Yalnızca Ubuntu 16.04 veya sonraki bir sürümü kullanırken, tam şifrelemesi ile SMB 3.0 desteklenir.

### <a name="solution"></a>Çözüm

Linux için SMB 3.0 şifreleme özelliği 4.11 Çekirdeği'nde kullanıma sunulmuştur. Bu özellik, şirket içinde veya farklı bir Azure bölgesinde Azure dosya paylaşımını bağlama olanağı sağlar. Yayımlama zaman bu işlev, Ubuntu 17.04 ve Ubuntu 16.10 backported olmuştur. 

Şifreleme, Linux SMB istemcisinin desteklemiyorsa, bağlama Azure dosya paylaşımı ile aynı veri merkezinde bulunan bir Azure Linux VM gelen SMB 2.1 kullanarak dosyaları. Doğrulayın [güvenli aktarım gerekli]( https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer) ayarı depolama hesabı devre dışı. 

<a id="accessdeniedportal"></a>
## <a name="error-access-denied-when-browsing-to-an-azure-file-share-in-the-portal"></a>"Erişim reddedildi" hatası portalında bir Azure dosya paylaşımına göz atarken

Portalda bir Azure dosya paylaşımına göz attığınızda aşağıdaki hata iletisini alabilirsiniz:

Erişim reddedildi  
Erişim izniniz yok  
Bu içeriğe erişime izniniz yok gibi görünüyor. Erişim almak için lütfen sahibiyle iletişime geçin.  

### <a name="cause-1-your-user-account-does-not-have-access-to-the-storage-account"></a>1. neden: Kullanıcı hesabınızın, depolama hesabına erişimi yok

### <a name="solution-for-cause-1"></a>Çözüm nedeni 1 için

Azure dosya paylaşımının bulunduğu depolama hesabına Gözat'a tıklayın **erişim denetimi (IAM)** ve kullanıcı hesabınızın, depolama hesabına erişimi olduğunu doğrulayın. Daha fazla bilgi için bkz. [rol tabanlı erişim denetimi (RBAC) ile depolama hesabınızın güvenliğini sağlamak nasıl](https://docs.microsoft.com/azure/storage/common/storage-security-guide#how-to-secure-your-storage-account-with-role-based-access-control-rbac).

### <a name="cause-2-virtual-network-or-firewall-rules-are-enabled-on-the-storage-account"></a>2. neden: Sanal ağ veya güvenlik duvarı kuralları depolama hesabı etkinleştirilir

### <a name="solution-for-cause-2"></a>Neden 2 çözümü

Sanal ağ ve güvenlik duvarı kuralları depolama hesabı düzgün şekilde yapılandırıldığından doğrulayın. Sanal ağ veya güvenlik duvarı kuralları neden sorun varsa test etmek için geçici olarak depolama hesabı için ayarı değiştirmeniz **tüm ağlardan erişime izin ver**. Daha fazla bilgi için bkz. [yapılandırma Azure depolama güvenlik duvarlarını ve sanal ağlar](https://docs.microsoft.com/azure/storage/common/storage-network-security).

<a id="slowperformance"></a>
## <a name="slow-performance-on-an-azure-file-share-mounted-on-a-linux-vm"></a>Azure dosya paylaşımını yavaş performans Linux sanal makinesine bağlı

### <a name="cause"></a>Nedeni

Yavaş performans olası bir nedeni, önbelleğe alma devre dışı bırakıldı.

### <a name="solution"></a>Çözüm

Önbelleğe alma devre dışı olup olmadığını denetlemek için Aranan **önbellek =** girişi. 

**Önbellek = none** önbelleğe alma devre dışı olduğunu belirtir. Varsayılan bağlama komutunu kullanarak veya açıkça ekleyerek paylaşımı yeniden bağlamak **önbellek strict =** seçeneği varsayılan önbelleğe alma sağlamak için bağlama komutu ya da "strict" önbelleğe alma modu etkin.

Bazı senaryolarda **serverino** bağlama seçeneği açabilir **ls** karşı her dizin girdisi stat çalıştırmak için komutu. Bu davranış, büyük bir dizin listelerken içinde performans düşüşü sonuçlanır. Bağlama seçeneklerini kontrol edebilirsiniz, **/etc/fstab** girişi:

`//azureuser.file.core.windows.net/cifs /cifs cifs vers=2.1,serverino,username=xxx,password=xxx,dir_mode=0777,file_mode=0777`

Ayrıca doğru seçenekleri çalıştırarak kullanılıp kullanılmadığını kontrol edebilirsiniz **sudo mount | grep CIFS** komut ve çıktısını denetleniyor. Örnek çıktı aşağıda verilmiştir:

```
//azureuser.file.core.windows.net/cifs on /cifs type cifs (rw,relatime,vers=2.1,sec=ntlmssp,cache=strict,username=xxx,domain=X,uid=0,noforceuid,gid=0,noforcegid,addr=192.168.10.1,file_mode=0777, dir_mode=0777,persistenthandles,nounix,serverino,mapposix,rsize=1048576,wsize=1048576,actimeo=1)
```

Varsa **önbellek strict =** veya **serverino** seçenektir değil sunmak, çıkarın ve yeniden bağlama komutunu çalıştırarak Azure dosyaları bağlama [belgeleri](../storage-how-to-use-files-linux.md). Ardından, yeniden denetle **/etc/fstab** giriş doğru seçeneği vardır.

<a id="timestampslost"></a>
## <a name="time-stamps-were-lost-in-copying-files-from-windows-to-linux"></a>Zaman damgaları dosyaları Windows için Linux kopyalama kaybolduğu

Linux/Unix platformlarında, **cp -p** farklı kullanıcılar dosya 1 ve 2 dosyası sahipseniz komutu başarısız olur.

### <a name="cause"></a>Nedeni

Force bayrağını **f** COPYFILE içinde sonuçları yürütülürken **cp -p -f** UNIX üzerinde. Bu komut, size ait olmayan dosya zaman damgasını korumak de başarısız olur.

### <a name="workaround"></a>Geçici Çözüm

Depolama hesabı kullanıcı dosyalarını kopyalamak için kullanın:

- `Useadd : [storage account name]`
- `Passwd [storage account name]`
- `Su [storage account name]`
- `Cp -p filename.txt /share`

## <a name="cannot-connect-to-or-mount-an-azure-file-share"></a>Bağlanamadığını ya da bir Azure dosya paylaşımını bağlama

### <a name="cause"></a>Nedeni

Bu sorunun sık karşılaşılan nedenleri şunlardır:


- Uyumsuz bir Linux dağıtımı istemcisi kullanıyorsunuz. Bir Azure dosya paylaşımına bağlanmak için aşağıdaki Linux dağıtımlarını kullanmanızı öneririz:

    |   | SMB 2.1 <br>(Aynı Azure bölgesindeki VM'ler üzerinde başlatmalar) | SMB 3.0 <br>(Şirket içinde ve bölgeler arası başlatmalar) |
    | --- | :---: | :---: |
    | Ubuntu Server | 14.04+ | 16.04+ |
    | RHEL | 7+ | 7.5+ |
    | CentOS | 7+ |  7.5+ |
    | Debian | 8+ |   |
    | openSUSE | 13.2+ | 42.3+ |
    | SUSE Linux Enterprise Server | 12 | 12 SP3+ |

- CIFS hizmet programları (cfs-utils) istemcide yüklü değil.
- En düşük SMB/CIFS sürüm 2.1, istemcide yüklü değil.
- SMB 3.0 şifreleme istemcide desteklenmiyor. SMB 3.0 şifreleme 16,4 Ubuntu ve SUSE 12.3 ve sonraki sürümleri ile birlikte sonraki sürümlerinde kullanılabilir. Diğer dağıtımları çekirdek 4.11 ve sonraki sürümleri gerektirir.
- Desteklenmeyen TCP bağlantı noktası 445, bir depolama hesabına bağlanmak çalışıyorsunuz.
- Bir Azure dosya paylaşımı için bir Azure VM'den bağlanmaya çalıştığınız ve VM, depolama hesabıyla aynı bölgede değil.
- Varsa [güvenli aktarım gerekli]( https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer) ayarı, depolama hesabında etkinleştirildiğinde, Azure dosyaları SMB 3.0 şifreleme ile kullanan bağlantılar sağlayacaktır.

### <a name="solution"></a>Çözüm

Sorunu gidermek için [Azure dosyaları Linux'ta bağlama hataları için sorun giderme aracı](https://gallery.technet.microsoft.com/Troubleshooting-tool-for-02184089). Bu aracı:

* Ortam çalıştıran istemci doğrulamaya yardımcı olur.
* Azure dosyaları için erişim hataya neden uyumlu istemci yapılandırmasında algılar.
* Kendi kendine düzeltme üzerinde normatif bir Rehber sağlar.
* Tanılama izlemeleri toplanır.

## <a name="ls-cannot-access-ltpathgt-inputoutput-error"></a>ls: erişemiyor '&lt;yolu&gt;': Giriş/Çıkış hatası

Bir Azure Dosya paylaşımındaki dosyaları listeleme ls komutunu kullanarak denediğinizde, komut dosyaları listelemenin kilitleniyor. Aşağıdaki hatayı alıyorsunuz:

**ls: erişemiyor '&lt;yolu&gt;': Giriş/Çıkış hatası**


### <a name="solution"></a>Çözüm
Bu sorun için düzeltme sahip aşağıdaki sürümler için Linux çekirdeğinin yükseltme:

- 4.4.87+
- 4.9.48+
- 4.12.11+
- Büyüktür veya eşittir 4.13 tüm sürümler

## <a name="cannot-create-symbolic-links---ln-failed-to-create-symbolic-link-t-operation-not-supported"></a>Sembolik bağlantılar - oluşturulamıyor ln: sembolik bağlantı 't oluşturma başarısız oldu ': İşlem desteklenmiyor

### <a name="cause"></a>Nedeni
Varsayılan olarak, Azure dosya paylaşımlarını CIFS kullanarak Linux'ta bağlama simgesel bağlantılar (çözümlemeyin) için destek sağlamaz. Bu gibi bir hata görürsünüz:
```
ln -s linked -n t
ln: failed to create symbolic link 't': Operation not supported
```
### <a name="solution"></a>Çözüm
Linux CIFS istemcisi oluşturulmasını Windows stili sembolik bağlantılar üzerinden SMB 2 veya 3 Protokolü desteklemiyor. Şu anda Linux istemci sembolik bağlantıların adlı başka bir stil destekler [Minshall + Fransızca çözümlemeyin](https://wiki.samba.org/index.php/UNIX_Extensions#Minshall.2BFrench_symlinks) her ikisini de oluşturma ve işlemleri izleyin. Sembolik bağlantılar ihtiyaç duyan müşteriler, "mfsymlinks" Bağlama seçeneği kullanabilirsiniz. Aynı zamanda, Mac kullanmak biçimde olduğundan "mfsymlinks" öneririz.

Çözümlemeyin kullanmak için aşağıdaki, CIFS bağlama komutunun sonuna ekleyin:

```
,mfsymlinks
```

Bu nedenle komut şöyle görünür:

```
sudo mount -t cifs //<storage-account-name>.file.core.windows.net/<share-name> <mount-point> -o vers=<smb-version>,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino,mfsymlinks
```

Daha sonra çözümlemeyin oluşturabilirsiniz üzerinde önerilen [wiki](https://wiki.samba.org/index.php/UNIX_Extensions#Storing_symlinks_on_Windows_servers).

[!INCLUDE [storage-files-condition-headers](../../../includes/storage-files-condition-headers.md)]

## <a name="need-help-contact-support"></a>Yardıma mı ihtiyacınız var? Desteğe başvurun.

Hala yardıma ihtiyacınız varsa [Destek ekibiyle iletişime geçin](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi alınamıyor.
