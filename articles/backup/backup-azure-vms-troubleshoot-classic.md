---
title: "Klasik Portalı'nda Azure yedekleme hatalarında sorun giderme | Microsoft Docs"
description: "Azure Backup ve Azure sanal makineleri Klasik portalda geri yükleme sorunlarını giderin."
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
ms.assetid: 117201fb-c0cd-4be4-b47f-abd88fe914cf
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 1/23/2017
ms.author: cwatson
ms.openlocfilehash: 658a4576c5fd664ce33737a1fcf9deafccd4c8b0
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2017
---
# <a name="troubleshoot-azure-virtual-machine-backup"></a>Azure sanal makine yedekleme sorunlarını giderme
> [!div class="op_single_selector"]
> * [Kurtarma Hizmetleri kasası](backup-azure-vms-troubleshoot.md)
> * [Yedekleme kasası](backup-azure-vms-troubleshoot-classic.md)
>
>

Azure Backup bilgileri kullanarak, aşağıdaki tabloda listelenen sırasında oluşan hatalar giderebilirsiniz.

## <a name="discovery"></a>Bulma
| Yedekleme işlemi | Hata ayrıntıları | Geçici çözüm |
| --- | --- | --- |
| Bulma |Yeni öğe - Microsoft Azure yedekleme ile karşılaştı ve iç hata bulmak başarısız oldu. Birkaç dakika bekleyin ve sonra işlemi yeniden deneyin. |Bulma işlemi, 15 dakika sonra yeniden deneyin. |
| Bulma |Yeni öğeler – bulamadı başka bir bulma işlemi zaten devam ediyor. Geçerli bulma işlemi tamamlanana kadar bekleyin. |None |

## <a name="register"></a>Kaydolma
| Yedekleme işlemi | Hata ayrıntıları | Geçici çözüm |
| --- | --- | --- |
| Kaydolma |Sanal makineye bağlı veri diski sayısı desteklenen sınırı aşıldı - Lütfen bazı veri diski bu sanal makinede ayırın ve işlemi yeniden deneyin. Azure yedekleme yedekleme için bir Azure sanal makineye bağlı en fazla 16 veri diskleri destekler |None |
| Kaydolma |Microsoft Azure yedekleme ile karşılaştı bir iç hata - bekleme süresi birkaç dakika ve işlemi yeniden deneyin. Sorun devam ederse, Microsoft Destek'e başvurun. |Premium LRS üzerinde VM aşağıdaki desteklenmeyen yapılandırmasını biri nedeniyle bu hatayı alabilirsiniz. <br> Premium depolama Vm'leri kurtarma Hizmetleri kasası kullanılarak yedeklenebilir. [Daha Fazla Bilgi](backup-introduction-to-azure-backup.md#using-premium-storage-vms-with-azure-backup) |
| Kaydolma |Kayıt Aracısı yükleme işlemi zaman aşımı ile başarısız oldu |Sanal makinenin işletim sistemi sürümü desteklenip desteklenmediğini denetleyin. |
| Kaydolma |Komut yürütme başarısız oldu - başka bir işlem bu öğeyi devam ediyor. Önceki işlem tamamlanana kadar bekleyin |None |
| Kaydolma |Premium depolama alanında depolanan sanal sabit diskler sahip sanal makineler için yedekleme desteklenmez. |None |
| Kaydolma |Sanal makine Aracısı sanal makinede mevcut değil - Lütfen gerekli önkoşul, VM agent'ı yükleyin ve işlemi yeniden deneyin. |[Daha fazla bilgi](#vm-agent) VM Aracısı yükleme ve VM Aracısı yüklemesini doğrulamak nasıl hakkında. |

## <a name="backup"></a>Backup
| Yedekleme işlemi | Hata ayrıntıları | Geçici çözüm |
| --- | --- | --- |
| Backup |Anlık görüntü durumu için VM aracısıyla iletişim kurulamadı. Anlık görüntü VM alt görevi zaman aşımına uğradı. -Lütfen bu sorunu gidermek nasıl hakkında sorun giderme kılavuzuna bakın. |Bu hata, VM Aracısı ile ilgili bir sorun veya başka bir yolla Azure altyapısı için ağ erişim engellendi atılır. Daha fazla bilgi edinmek [anlık görüntü sorunlarını VM'yi hata ayıklama](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md). <br> VM Aracısı sorunları neden, VM'yi yeniden başlatın. Bazen, yanlış bir VM durum sorunlara neden olabilir ve VM'yi yeniden başlatırken bu "bozuk" sıfırlar |
| Backup |Yedekleme bir iç hatayla başarısız oldu - Lütfen işlemi birkaç dakika içinde yeniden deneyin. Sorun devam ederse, Microsoft Support başvurun |Lütfen olup geçici bir sorun VM depolama erişirken denetleyin. Lütfen denetleyin [Azure durum](https://azure.microsoft.com/en-us/status/) devam eden herhangi bir sorun olup olmadığını görmek için depolama/işlem/ağ bölgede ilgili. Lütfen yedekleme sonrası sorunu azaltıldığından yeniden deneyin. |
| Backup |VM artık mevcut olmadığından işlem gerçekleştirilemiyor. |Yedekleme için yapılandırılmış VM silindiğinden, yedekleme gerçekleştirilemiyor. Lütfen korumalı öğeleri görüntülemek için giderek daha fazla yedeklemeleri durdurmak, korumalı öğe seçin ve korumayı Durdur'ı tıklatın. Yedekleme korumak veri seçeneğini belirleyerek verileri koruyabilirsiniz. Tıklayarak bu sanal makine üzerinde yapılandırmak için kayıtlı öğeler görünümü korumadan koruma daha sonra devam edebilirsiniz |
| Backup |Azure kurtarma Hizmetleri yüklenemedi seçilen öğenin - VM Aracısı Azure kurtarma Hizmetleri uzantısı için önkoşul uzantısıdır. Lütfen Azure VM aracısını yükleyin ve kayıt işlemini yeniden başlatın |<ol> <li>VM Aracısı'nı doğru şekilde yüklenip yüklenmediğini denetleyin. <li>VM yapılandırma bayrağı doğru ayarlandığından emin olun.</ol> [Daha fazla bilgi](#validating-vm-agent-installation) VM Aracısı yükleme ve VM Aracısı yüklemesini doğrulamak nasıl hakkında. |
| Backup |Komut yürütme başarısız oldu - başka bir işlem şu anda bu öğeyi devam ediyor. Önceki işlem tamamlanana kadar bekleyin ve sonra yeniden deneyin |Var olan yedekleme veya geri yükleme işi VM çalıştıran ve var olan iş çalışırken yeni bir işi başlatılamıyor. |
| Backup |Uzantı yüklemesi "COM + için Microsoft Distributed Transaction Coordinator konuşun başaramadı hatasıyla başarısız oldu |Bu, genellikle COM + hizmeti çalışmadığı anlamına gelir. Bu sorunu düzeltme konusunda yardım için Microsoft Destek'e başvurun. |
| Backup |Anlık görüntü işlemi, "Bu sürücünün BitLocker Sürücü Şifrelemesi tarafından kilitlenmiş. VSS işlemi hatasıyla başarısız oldu Denetim Masası'ndan bu sürücünün kilidini açmanız gerekir. |VM üzerindeki tüm sürücüleri için BitLocker'ı açın ve VSS sorunun giderilip giderilmediğini inceleyin |
| Backup |Premium depolama alanında depolanan sanal sabit diskler sahip sanal makineler için yedekleme desteklenmez. |None |
| Backup |Azure sanal makine bulunamadı. |Bu, birincil VM silindiği, ancak yedekleme gerçekleştirmek bir VM için aramak yedekleme ilkenizi devam durumlarda gerçekleşir. Bu hatayı düzeltmek için: <ol><li>Aynı kaynak grubu adı [bulut hizmet adı] ve aynı ada sahip sanal makine oluşturun <br>(VEYA) <li> Sonraki yedeklemeleri değil tetiklenen için bu sanal makine için koruma devre dışı bırakın. </ol> |
| Backup |Sanal makine Aracısı sanal makinede mevcut değil - Lütfen gerekli önkoşul, VM agent'ı yükleyin ve işlemi yeniden deneyin. |[Daha fazla bilgi](#vm-agent) VM Aracısı yükleme ve VM Aracısı yüklemesini doğrulamak nasıl hakkında. |

## <a name="jobs"></a>İşler
| İşlem | Hata ayrıntıları | Geçici çözüm |
| --- | --- | --- |
| İşi iptal et |İptal bu iş türü için desteklenmeyen - iş tamamlanana kadar bekleyin. |None |
| İşi iptal et |İş iptal edilebilen bir durumda değil - iş tamamlanana kadar bekleyin. <br>OR<br> Seçilen işin iptal edilebilen bir durumda değil - işi tamamlamak lütfen bekleyin. |Tüm olasılığını içinde iş neredeyse tamamlandı; iş tamamlanana kadar bekleyin |
| İşi iptal et |İş sürüyor değil - iptal yalnızca sürmekte olan işleri için desteklenen olduğundan iptal edilemez. Lütfen girişimi iptal bir sürüyor işi. |Bu hatanın nedeni geçici bir durum nedeniyle oluşur. Bir dakika bekleyin ve İptal işlemi yeniden deneyin |
| İşi iptal et |Başarısız işi iptal-işi tamamlanana kadar bekleyin. |None |

## <a name="restore"></a>Geri Yükleme
| İşlem | Hata ayrıntıları | Geçici çözüm |
| --- | --- | --- |
| Geri Yükleme |Geri yükleme bulut iç hata vererek başarısız oldu |<ol><li>Bulut hizmeti geri yüklemeye çalıştığınız DNS ayarlarıyla yapılandırılır. Kontrol edebilirsiniz <br>$deployment get-AzureDeployment - ServiceName "ServiceName" =-yuvası "Üretim" Get-AzureDns - DnsSettings $deployment. DnsSettings<br>Yapılandırılmış adresi varsa, bu DNS ayarlarının yapılandırıldığını anlamına gelir.<br> <li>Bulut hizmeti olduğu için geri yüklemeye çalıştığınız ReservedIP ile yapılandırıldığından ve bulut hizmetindeki VM'ler durdurulmuş durumda.<br>Bir bulut hizmeti powershell cmdlet'lerini kullanarak IP ayrılmış denetleyebilirsiniz:<br>$deployment get-AzureDeployment - ServiceName "servicename" =-yuvası "Üretim" $DEP Reservedıpname <br><li>Aşağıdaki özel ağ yapılandırmaları bir sanal makineyle aynı bulut hizmetine geri yüklemeye çalıştığınız. <br>-Sanal makineler yük dengeleyici yapılandırması (dahili ve harici)<br>-Sanal makinelerle birden çok ayrılmış IP<br>-Birden çok NIC içeren sanal makinelere<br>Lütfen yeni bir bulut hizmeti kullanıcı Arabiriminde seçin veya lütfen [konuları geri](backup-azure-restore-vms.md#restoring-vms-with-special-network-configurations) özel ağ yapılandırmaları olan VM'ler için</ol> |
| Geri Yükleme |Seçili DNS adı zaten alınmış - Lütfen farklı bir DNS adı belirtin ve yeniden deneyin. |Bulut hizmeti adı için DNS adını buraya başvuruyor (genellikle ile biten. cloudapp.net). Bu, benzersiz olması gerekir. Bu hatayla karşılaşırsanız, geri yükleme sırasında farklı bir VM adı seçmeniz gerekir. <br><br> Bu hata yalnızca Azure portalında kullanıcılara gösterilir. Yalnızca diskleri geri yükler ve VM oluşturma değil çünkü PowerShell aracılığıyla geri yükleme işlemi başarılı olur. Disk geri yükledikten sonra işlemi VM açıkça sizin tarafınızdan oluşturulduğunda hata karşılaştığı. |
| Geri Yükleme |Belirtilen sanal ağ yapılandırması doğru değil - Lütfen farklı bir sanal ağ yapılandırması belirtin ve yeniden deneyin. |None |
| Geri Yükleme |Belirtilen bulut hizmeti değil geri yüklenen sanal makine yapılandırmasıyla eşleşen - Lütfen farklı bir bulut hizmeti, bir ayrılmış IP kullanmayan belirtin veya geri yüklemek için başka bir kurtarma noktası seçin bir ayrılmış IP kullanıyor. |None |
| Geri Yükleme |Bulut hizmeti giriş uç noktası sayısı sınırına - mevcut bir uç noktası kullanarak veya farklı bir bulut hizmeti belirterek işlemi yeniden deneyin. |None |
| Geri Yükleme |Yedekleme kasası ve hedef depolama hesabı olan iki farklı bölgelerde - geri yükleme işleminde belirtilen depolama hesabı aynı Azure bölgesinde yedekleme kasası olarak olduğundan emin olun. |None |
| Geri Yükleme |Bir geri yükleme işlem için belirtilen depolama hesabına desteklenen - yalnızca temel/standart depolama hesapları ile yerel olarak yedekli veya coğrafi olarak yedekli çoğaltma ayarları desteklenir. Lütfen desteklenen depolama hesabı seçin |None |
| Geri Yükleme |Geri yükleme işlemi için belirtilen depolama hesabı türü çevrimiçi değil - geri yükleme işleminde belirtilen depolama hesabı çevrimiçi olduğundan emin olun |Azure Storage veya kesinti nedeniyle geçici bir hata nedeniyle gerçekleşebilir. Lütfen başka bir depolama hesabı seçin. |
| Geri Yükleme |Kaynak grubu kotasına ulaşıldı - Lütfen Azure Portalı'ndan bazı kaynak gruplarını silin veya sınırları artırmak için Azure desteğine başvurun. |None |
| Geri Yükleme |Seçilen alt ağ yok - Lütfen var olan bir alt ağ seçin |None |

## <a name="policy"></a>İlke
| İşlem | Hata ayrıntıları | Geçici çözüm |
| --- | --- | --- |
| İlke Oluştur |-İlke oluşturma başarısız oldu, lütfen ilke yapılandırması ile devam etmek için elde tutma seçimleri azaltın. |None |

## <a name="vm-agent"></a>VM Aracısı
### <a name="setting-up-the-vm-agent"></a>VM Aracısı'nı ayarlama
Genellikle, VM Aracısı zaten Azure galerisinden oluşturulan VM'ler bulunur. Ancak, şirket içi veri merkezlerinden Geçişi sanal makineleri VM Aracısı'nın yüklü sahip olmaz. Bu tür VM'ler için VM Aracısı açıkça yüklü olması gerekir. Daha fazla bilgi edinin [var olan bir VM üzerinde VM Aracısı yükleme](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx).

Windows VM'ler için:

* [Aracı MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) dosyasını indirip yükleyin. Yüklemeyi tamamlamak için Yönetici ayrıcalıklarına sahip olmanız gerekir.
* Aracının yüklü olduğunu belirtmek üzere [VM özelliğini güncelleştirin](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx).

Linux VM'ler için:

* Son yükleme [Linux Aracısı](https://github.com/Azure/WALinuxAgent) github'dan.
* Aracının yüklü olduğunu belirtmek üzere [VM özelliğini güncelleştirin](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx).

### <a name="updating-the-vm-agent"></a>VM Aracısı'nı güncelleştirme
Windows VM'ler için:

* VM Aracısı'nı güncelleştirmek için [VM Aracısı ikili dosyalarının](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) yeniden yüklenmesi yeterlidir. Ancak, VM Aracısı güncelleştirilirken herhangi bir yedekleme işleminin çalıştığından emin olmak gerekir.

Linux VM'ler için:

* Yönergeleri izleyerek [güncelleştirme Linux VM Aracısı](../virtual-machines/linux/update-agent.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="validating-vm-agent-installation"></a>VM Aracısı yüklemesini doğrulama
Windows vm'lerde VM Aracısı'nın sürümünü denetlemek nasıl:

1. Azure sanal makinede oturum açın ve klasöre gidin *C:\WindowsAzure\Packages*. Mevcut WaAppAgent.exe dosyasını bulmanız gerekir.
2. Dosyaya sağ tıklayın, **Özellikler**'e gidin ve ardından **Ayrıntılar** sekmesini seçin. Ürün sürümü alanı 2.6.1198.718 olmalıdır veya üzeri
