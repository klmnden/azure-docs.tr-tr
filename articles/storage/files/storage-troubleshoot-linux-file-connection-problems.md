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
ms.openlocfilehash: 232b4ca2ee4f3137069ed155cc82a5c5e3251420
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67807268"
---
# <a name="troubleshoot-azure-files-problems-in-linux"></a>Linux'ta Azure dosyaları sorunlarını giderme

Bu makalede Linux istemcilerden bağladığınızda, Azure dosyaları'na ilgili genel sorunları listeler. Ayrıca olası nedenleri ve çözümlemeleri için bu sorunları sağlar. 

Bu makalede sorun giderme adımları ek olarak, kullandığınız [AzFileDiagnostics](https://gallery.technet.microsoft.com/Troubleshooting-tool-for-02184089) Linux istemci önkoşulları doğru olduğundan emin olmak için. Bu makalede değinilen belirtileri çoğunu algılanması AzFileDiagnostics otomatikleştirir. En iyi performansı elde etmek için ortamınızı ayarlama yardımcı olur. Bu bilgiler de bulabilirsiniz [Azure dosyaları paylaşımlarını sorun giderici](https://support.microsoft.com/help/4022301/troubleshooter-for-azure-files-shares). Sorun giderici bağlanma, eşleme ve Azure dosya paylaşımlarını oluşturma sorunları yardımcı olması için adımları sağlar.

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
- SMB 3.0 şifreleme istemcide desteklenmiyor. Yukarıdaki tabloda, şirket içinde ve bölgeler arası destek bağlama Linux dağıtımlarının listesini sağlar. şifreleme kullanarak. Diğer dağıtımları çekirdek 4.11 ve sonraki sürümleri gerektirir.
- Desteklenmeyen TCP bağlantı noktası 445, bir depolama hesabına bağlanmak çalışıyorsunuz.
- Bir Azure dosya paylaşımı için bir Azure VM'den bağlanmaya çalıştığınız ve VM, depolama hesabıyla aynı bölgede değil.
- Varsa [güvenli aktarım gerekli]( https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer) ayarı, depolama hesabında etkinleştirildiğinde, Azure dosyaları SMB 3.0 şifreleme ile kullanan bağlantılar sağlayacaktır.

### <a name="solution"></a>Çözüm

Sorunu gidermek için [Azure dosyaları Linux'ta bağlama hataları için sorun giderme aracı](https://gallery.technet.microsoft.com/Troubleshooting-tool-for-02184089). Bu aracı:

* Ortam çalıştıran istemci doğrulamaya yardımcı olur.
* Azure dosyaları için erişim hataya neden uyumlu istemci yapılandırmasında algılar.
* Kendi kendine düzeltme üzerinde normatif bir Rehber sağlar.
* Tanılama izlemeleri toplanır.

<a id="mounterror13"></a>
## <a name="mount-error13-permission-denied-when-you-mount-an-azure-file-share"></a>"Error(13) bağlayın: İzin reddedildi"Azure dosya paylaşımını bağlama zaman

### <a name="cause-1-unencrypted-communication-channel"></a>1\. neden: Şifrelenmemiş iletişim kanalı

Güvenlik nedenleriyle, Azure dosya paylaşımlarını bağlantı iletişim kanalını şifreli değildir ve Azure dosya paylaşımlarını bulunduğu aynı veri merkezlerinden bağlantı girişimi yapılmadan değil engellenir. Aynı veri merkezindeki şifrelenmemiş bağlantıları da ise engellenir [güvenli aktarım gerekli](https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer) depolama hesabı seçeneği etkinleştirilmiştir. Şifreli iletişim kanalı, yalnızca kullanıcının istemci işletim sistemi SMB şifrelemesi destekliyorsa sağlanır.

Daha fazla bilgi için bkz. [Linux ve CIFS-utils paketi ile bir Azure dosya bağlama önkoşulları paylaşır](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-linux#prerequisites-for-mounting-an-azure-file-share-with-linux-and-the-cifs-utils-package). 

### <a name="solution-for-cause-1"></a>Çözüm nedeni 1 için

1. SMB Şifrelemesi'ni destekleyen bir istemciyi bağlanmak veya aynı veri merkezinde Azure dosya paylaşımı için kullanılan Azure depolama hesabı olarak bir sanal makinesinden bağlanabilirsiniz.
2. Doğrulama [güvenli aktarım gerekli](https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer) ayarı devre dışı depolama hesabında istemci SMB şifrelemesi desteklemiyorsa.

### <a name="cause-2-virtual-network-or-firewall-rules-are-enabled-on-the-storage-account"></a>2\. neden: Sanal ağ veya güvenlik duvarı kuralları depolama hesabı etkinleştirilir 

Sanal ağ (VNET) ve güvenlik duvarı kuralları depolama hesabında yapılandırılmışsa, istemci IP adresi veya sanal ağ erişimine izin verilmesini sürece ağ trafiğini erişimi reddedilir.

### <a name="solution-for-cause-2"></a>Neden 2 çözümü

Sanal ağ ve güvenlik duvarı kuralları depolama hesabı düzgün şekilde yapılandırıldığından doğrulayın. Sanal ağ veya güvenlik duvarı kuralları neden sorun varsa test etmek için geçici olarak depolama hesabı için ayarı değiştirmeniz **tüm ağlardan erişime izin ver**. Daha fazla bilgi için bkz. [yapılandırma Azure depolama güvenlik duvarlarını ve sanal ağlar](https://docs.microsoft.com/azure/storage/common/storage-network-security).

<a id="permissiondenied"></a>
## <a name="permission-denied-disk-quota-exceeded-when-you-try-to-open-a-file"></a>"[izin reddedildi;] Disk kotası aşıldı" bir dosyayı açmaya çalıştığınızda

Linux'ta, aşağıdakine benzer bir hata iletisi alırsınız:

**\<Dosya adı > [izin reddedildi;] Disk kotası aşıldı**

### <a name="cause"></a>Nedeni

Bir dosya için izin verilen eşzamanlı açık tanıtıcıları üst sınırına ulaştınız.

Tek bir dosya çubuğunda 2.000 açık tanıtıcıları kotası yoktur. 2\.000 açık tanıtıcıları olduğunda kotasına ulaşıldığında belirten bir hata iletisi görüntülenir.

### <a name="solution"></a>Çözüm

Bazı işler kapatarak eşzamanlı açık işleyicilerin sayısını azaltın ve sonra işlemi yeniden deneyin.

Dosya Paylaşımı, dizin veya dosya tanıtıcıları görüntülemek için kullanın [Get-AzStorageFileHandle](https://docs.microsoft.com/powershell/module/az.storage/get-azstoragefilehandle) PowerShell cmdlet'i.  

Dosya Paylaşımı, dizin veya dosya tanıtıcıları kapatmak için [Kapat AzStorageFileHandle](https://docs.microsoft.com/powershell/module/az.storage/close-azstoragefilehandle) PowerShell cmdlet'i.

> [!Note]  
> Get-AzStorageFileHandle ve Kapat AzStorageFileHandle cmdlet'leri Az PowerShell modülü 2.4 veya sonraki bir sürümü dahil edilir. En son Az PowerShell modülünü yüklemek için bkz: [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-az-ps).

<a id="slowfilecopying"></a>
## <a name="slow-file-copying-to-and-from-azure-files-in-linux"></a>Dosya ve Linux'ta Azure dosyalarından kopyalamak yavaş

- Belirli bir en düşük g/ç boyutu gereksinimi yoksa, en iyi performans için 1 MiB g/ç boyutu kullanmanızı öneririz.
- Doğru kopyalama yöntemi kullanın:
    - Kullanım [AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) iki dosya paylaşımları arasındaki tüm aktarımları için.
    - CP ya da dd ile paralel kullanarak kopyalama hızını artırmak, iş parçacığı sayısını, kullanım örneği ve iş yüküne bağlıdır. Aşağıdaki örnekler altı kullanın: 
    - cp örnek (cp kullanacağı varsayılan blok boyutu dosya sisteminin öbek boyutu): `find * -type f | parallel --will-cite -j 6 cp {} /mntpremium/ &`.
    - dd örnek: (Bu komut açıkça öbek boyutu 1 MiB için ayarlar) `find * -type f | parallel --will-cite-j 6 dd if={} of=/mnt/share/{} bs=1M`
    - Kaynak üçüncü taraf araçları gibi açın:
        - [GNU paralel](https://www.gnu.org/software/parallel/).
        - [Fpart](https://github.com/martymac/fpart) - dosyaları sıralar ve bunları bölümlere paketleri.
        - [Fpsync](https://github.com/martymac/fpart/blob/master/tools/fpsync) -için dst_url src_dir verileri geçirmeye Fpart kullanır ve bir kopyalama aracı birden çok örneği üretme.
        - [Çoklu](https://github.com/pkolano/mutil) -çok iş parçacıklı cp ve md5sum göre GNU coreutils üzerinde.
- Her bir genişletme yazma yapmak yerine dosya boyutunu önceden ayarlama dosya boyutu olduğu bilinir senaryolarda kopyalama hızı artırılmasına yardım eder. Yazma kaçınılması gerek genişletme, bir hedef dosya boyutu ayarlayabileceğiniz `truncate - size <size><file>` komutu. Bundan sonra `dd if=<source> of=<target> bs=1M conv=notrunc`komut kopyalar bir kaynak dosyası hedef dosya boyutunu sürekli güncelleştirmek zorunda kalmadan. Örneğin, kopyalamak istediğiniz her dosya için hedef dosya boyutu ayarlayabileceğiniz (bir paylaşım/mnt/paylaşım altında bağlı olduğunu varsayın):
    - `$ for i in `` find * -type f``; do truncate --size ``stat -c%s $i`` /mnt/share/$i; done`
    - ve ardından - dosya yazma paralel genişletmeden kopyalayın: `$find * -type f | parallel -j6 dd if={} of =/mnt/share/{} bs=1M conv=notrunc`

<a id="error115"></a>
## <a name="mount-error115-operation-now-in-progress-when-you-mount-azure-files-by-using-smb-30"></a>"Error(115) bağlayın: İşlem Sürüyor"olduğunda, bağlama Azure dosyaları SMB 3.0 kullanarak

### <a name="cause"></a>Nedeni

Bazı Linux dağıtımlarında, henüz de SMB 3.0 şifreleme özellikleri desteklemez. Eksik bir özellik nedeniyle SMB 3.0 kullanarak bağlama Azure dosyaları'na çalıştıklarında, kullanıcıların bir "115" hata iletisi alabilirsiniz. Yalnızca Ubuntu 16.04 veya sonraki bir sürümü kullanırken, tam şifrelemesi ile SMB 3.0 desteklenir.

### <a name="solution"></a>Çözüm

Linux için SMB 3.0 şifreleme özelliği 4.11 Çekirdeği'nde kullanıma sunulmuştur. Bu özellik, şirket içinde veya farklı bir Azure bölgesinde Azure dosya paylaşımını bağlama olanağı sağlar. Bu işlev, listelenen Linux dağıtımları yer [sürümleriyle ilgili bağlama özellikleri (SMB sürüm 2.1 veya SMB 3.0 sürümü) önerilen en az](storage-how-to-use-files-linux.md#minimum-recommended-versions-with-corresponding-mount-capabilities-smb-version-21-vs-smb-version-30). Diğer dağıtımları çekirdek 4.11 ve sonraki sürümleri gerektirir.

Şifreleme, Linux SMB istemcisinin desteklemiyorsa, bağlama Azure dosya paylaşımı ile aynı veri merkezinde bulunan bir Azure Linux VM gelen SMB 2.1 kullanarak dosyaları. Doğrulayın [güvenli aktarım gerekli]( https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer) ayarı depolama hesabı devre dışı. 

<a id="authorizationfailureportal"></a>
## <a name="error-authorization-failure-when-browsing-to-an-azure-file-share-in-the-portal"></a>"Yetkilendirme hatası" hata portalında bir Azure dosya paylaşımına göz atarken

Portalda bir Azure dosya paylaşımına göz attığınızda aşağıdaki hata iletisini alabilirsiniz:

Yetkilendirme hatası  
Erişiminiz yok

### <a name="cause-1-your-user-account-does-not-have-access-to-the-storage-account"></a>1\. neden: Kullanıcı hesabınızın, depolama hesabına erişimi yok

### <a name="solution-for-cause-1"></a>Çözüm nedeni 1 için

Azure dosya paylaşımının bulunduğu depolama hesabına Gözat'a tıklayın **erişim denetimi (IAM)** ve kullanıcı hesabınızın, depolama hesabına erişimi olduğunu doğrulayın. Daha fazla bilgi için bkz. [rol tabanlı erişim denetimi (RBAC) ile depolama hesabınızın güvenliğini sağlamak nasıl](https://docs.microsoft.com/azure/storage/common/storage-security-guide#how-to-secure-your-storage-account-with-role-based-access-control-rbac).

### <a name="cause-2-virtual-network-or-firewall-rules-are-enabled-on-the-storage-account"></a>2\. neden: Sanal ağ veya güvenlik duvarı kuralları depolama hesabı etkinleştirilir

### <a name="solution-for-cause-2"></a>Neden 2 çözümü

Sanal ağ ve güvenlik duvarı kuralları depolama hesabı düzgün şekilde yapılandırıldığından doğrulayın. Sanal ağ veya güvenlik duvarı kuralları neden sorun varsa test etmek için geçici olarak depolama hesabı için ayarı değiştirmeniz **tüm ağlardan erişime izin ver**. Daha fazla bilgi için bkz. [yapılandırma Azure depolama güvenlik duvarlarını ve sanal ağlar](https://docs.microsoft.com/azure/storage/common/storage-network-security).

<a id="open-handles"></a>
## <a name="unable-to-delete-a-file-or-directory-in-an-azure-file-share"></a>Bir dosya veya dizinde bir Azure dosya paylaşımı silinemedi

### <a name="cause"></a>Nedeni
Dosya veya dizin açık bir tanıtıcısı olması durumunda bu sorun genellikle oluşur. 

### <a name="solution"></a>Çözüm

Tüm açık tanıtıcıları SMB istemcileri kapatıldı ve Sorun oluşmaya devam, aşağıdakileri gerçekleştirin:

- Kullanım [Get-AzStorageFileHandle](https://docs.microsoft.com/powershell/module/az.storage/get-azstoragefilehandle) açık tanıtıcıları görüntülemek için PowerShell cmdlet'i.

- Kullanım [Kapat AzStorageFileHandle](https://docs.microsoft.com/powershell/module/az.storage/close-azstoragefilehandle) açık tanıtıcıları kapatmak için PowerShell cmdlet'i. 

> [!Note]  
> Get-AzStorageFileHandle ve Kapat AzStorageFileHandle cmdlet'leri Az PowerShell modülü 2.4 veya sonraki bir sürümü dahil edilir. En son Az PowerShell modülünü yüklemek için bkz: [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-az-ps).

<a id="slowperformance"></a>
## <a name="slow-performance-on-an-azure-file-share-mounted-on-a-linux-vm"></a>Azure dosya paylaşımını yavaş performans Linux sanal makinesine bağlı

### <a name="cause-1-caching"></a>1\. neden: Önbelleğe alma

Yavaş performans olası bir nedeni, önbelleğe alma devre dışı bırakıldı. Önbelleğe alma, aksi takdirde, bir dosyayı tekrar tekrar erişiyorsa, bir ek yükü olabilir yararlı olabilir. Devre dışı bırakmadan önce önbellek kullandığınızı kontrol edin.

### <a name="solution-for-cause-1"></a>Çözüm nedeni 1 için

Önbelleğe alma devre dışı olup olmadığını denetlemek için Aranan **önbellek =** girişi.

**Önbellek = none** önbelleğe alma devre dışı olduğunu belirtir. Varsayılan bağlama komutunu kullanarak veya açıkça ekleyerek paylaşımı yeniden bağlamak **önbellek strict =** seçeneği varsayılan önbelleğe alma sağlamak için bağlama komutu ya da "strict" önbelleğe alma modu etkin.

Bazı senaryolarda **serverino** bağlama seçeneği açabilir **ls** karşı her dizin girdisi stat çalıştırmak için komutu. Bu davranış, büyük bir dizin listelerken içinde performans düşüşü sonuçlanır. Bağlama seçeneklerini kontrol edebilirsiniz, **/etc/fstab** girişi:

`//azureuser.file.core.windows.net/cifs /cifs cifs vers=2.1,serverino,username=xxx,password=xxx,dir_mode=0777,file_mode=0777`

Ayrıca doğru seçenekleri çalıştırarak kullanılıp kullanılmadığını kontrol edebilirsiniz **sudo mount | grep CIFS** komut ve çıktısını denetleniyor. Örnek çıktı aşağıda verilmiştir:

```
//azureuser.file.core.windows.net/cifs on /cifs type cifs (rw,relatime,vers=2.1,sec=ntlmssp,cache=strict,username=xxx,domain=X,uid=0,noforceuid,gid=0,noforcegid,addr=192.168.10.1,file_mode=0777, dir_mode=0777,persistenthandles,nounix,serverino,mapposix,rsize=1048576,wsize=1048576,actimeo=1)
```

Varsa **önbellek strict =** veya **serverino** seçenektir değil sunmak, çıkarın ve yeniden bağlama komutunu çalıştırarak Azure dosyaları bağlama [belgeleri](../storage-how-to-use-files-linux.md). Ardından, yeniden denetle **/etc/fstab** giriş doğru seçeneği vardır.

### <a name="cause-2-throttling"></a>2\. neden: Azaltma

Azaltma yaşayan ve isteklerinizi kuyruğa gönderilme mümkündür. Bunu yararlanarak doğrulamak [Azure İzleyici'de Azure depolama ölçümleri](../common/storage-metrics-in-azure-monitor.md).

### <a name="solution-for-cause-2"></a>Neden 2 çözümü

Uygulamanızın içinde olduğundan emin olun [Azure dosyaları ölçeklendirme hedeflerini](storage-files-scale-targets.md#azure-files-scale-targets).

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

Ancak, bu değişiklikler henüz tüm Linux dağıtımları için unity'nin değil. Bu düzeltme ve diğer yeniden düzeltmeleri bulunabilir [sürümleriyle ilgili bağlama özellikleri (SMB sürüm 2.1 veya SMB 3.0 sürümü) önerilen en az](storage-how-to-use-files-linux.md#minimum-recommended-versions-with-corresponding-mount-capabilities-smb-version-21-vs-smb-version-30) bölümünü [LinuxileAzuredosyalarıkullan](storage-how-to-use-files-linux.md)makalesi. Bu önerilen çekirdek sürümlerinden birini yükselterek bu düzeltme elde edebilirsiniz.

### <a name="workaround"></a>Geçici Çözüm

Bir sabit bağlanması belirterek bu sorunu geçici olarak çalışabilir. Sabit bağlama bağlantı kurulana kadar veya açıkça kesilene kadar beklenecek istemci zorlar. Ağ zaman aşımı nedeniyle hataları önlemek için kullanabilirsiniz. Ancak, bu geçici çözüm belirsiz bekler neden olabilir. Bağlantıları gerektiği gibi durdurmak hazırlıklı olun.

En son çekirdek sürümlerine yükseltme yapamazsınız, her 30 saniyede bir veya daha az yazma Azure Dosya paylaşımındaki dosya tutarak bu sorunu geçici olarak çalışabilir. Bu dosya üzerinde oluşturulan veya değiştirilen tarihi yeniden yazma gibi bir yazma işlemi olması gerekir. Aksi takdirde, önbelleğe alınan sonuçları alabilirsiniz ve işleminizi yeniden tetikleyebilir değil.

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun.

Hala yardıma ihtiyacınız varsa [Destek ekibiyle iletişime geçin](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi alınamıyor.
