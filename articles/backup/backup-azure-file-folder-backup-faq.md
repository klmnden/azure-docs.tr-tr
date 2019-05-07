---
title: Azure Backup ile dosya ve klasörleri yedekleme sık sorulan sorular
description: Azure Backup ile dosya ve klasörleri yedekleme ile ilgili yaygın sorular ele alınmıştır.
author: dcurwin
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 04/30/2019
ms.author: dacurwin
ms.openlocfilehash: 5dbd4fefd5c5e1acd7e12ace547ddb8866b7f081
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65148585"
---
# <a name="common-questions-about-backing-up-files-and-folders"></a>Dosya ve klasör yedekleme hakkında sık sorulan sorular 

Bu makalede Microsoft Azure kurtarma Hizmetleri (MARS) aracısı ile dosya ve klasörleri yedekleme abound yaygın soruların yanıtları bulunur [Azure Backup](backup-overview.md) hizmeti.

## <a name="general"></a>Genel

### <a name="why-does-the-mars-agent-need-net-framework-452-or-higher"></a>Neden MARS Aracısı gerekli .NET framework 4.5.2 veya üzeri?

Bulunan yeni işlevleri [anında geri yükleme](backup-azure-restore-windows-server.md#use-instant-restore-to-recover-data-to-the-same-machine) gerekli .NET Framework 4.5.2 veya üzeri.

## <a name="configure-backups"></a>Yedeklemeleri Yapılandırma

### <a name="where-can-i-download-the-latest-version-of-the-mars-agent"></a>MARS aracısının en son sürümünü nereden indirebilirim? 
Windows Server makineleri, System Center DPM ve Microsoft Azure Backup sunucusunu yedeklerken kullanılan en yeni MARS Aracısı kullanılabilir [indirme](https://aka.ms/azurebackup_agent). 

### <a name="how-long-are-vault-credentials-valid"></a>Kasa kimlik bilgileri geçerli ne kadar?
Kasa kimlik bilgilerinin süresi 48 sonra dolar. Kimlik bilgileri dosyasının süresi dolarsa, dosyayı Azure portalından yeniden yükleyin.

### <a name="from-what-drives-can-i-back-up-files-and-folders"></a>Hangi sürücülerden miyim dosya ve klasörleri yedekleyebilir miyim? 

Aşağıdaki türde sürücüleri ve birimleri yedekleyemezsiniz:

* Çıkarılabilir medya: Tüm yedekleme öğesi kaynaklarının sabit olarak bildirilmesi gerekir.
* Salt okunur birimler: Birimi, Birim Gölge Kopyası Hizmeti (VSS) çalışması için yazılabilir olmalıdır.
* Çevrimdışı birimler: Birimin çalışması için VSS'ye yönelik olarak çevrimiçi olması gerekir.
* Ağ paylaşımları: Birimin çevrimiçi yedekleme kullanılarak Yedeklenebilmesi için sunucuya yerel olması gerekir.
* BitLocker korumalı birimler: Yedeklemenin gerçekleşebilmesi birimin kilidinin açık olması gerekir.
* Dosya sistemi tanımı: NTFS, desteklenen tek dosya sistemidir.

### <a name="what-file-and-folder-types-are-supported"></a>Hangi dosya ve klasör türleri desteklenir?

Aşağıdaki türler desteklenir:

* Şifreli
* Sıkıştırılmış
* Seyrek
* Sıkıştırılmış + Seyrek
* Sabit bağlantılar: Desteklenmez, atlanır
* Yeniden ayrıştırma noktası: Desteklenmez, atlanır
* Şifreli + seyrek: Desteklenmez, atlanır
* Sıkıştırılmış Stream: Desteklenmez, atlanır
* Yeniden ayrıştırma noktaları, DFS bağlantıları ve birleşim noktaları da dahil olmak üzere


### <a name="can-i-use-the-mars-agent-to-back-up-files-and-folders-on-an-azure-vm"></a>Bir Azure sanal makinesinde dosyaları ve klasörleri yedeklemek için MARS Aracısı kullanabilir miyim?  
Evet. Azure Backup, Azure VM Aracısı VM uzantısı kullanarak, Azure Vm'leri için VM düzeyinde yedekleme sağlar. Sanal makinedeki Konuk Windows işletim sisteminde dosya ve klasörleri yedeklemek istiyorsanız, bunu yapmak için MARS Aracısı yükleyebilirsiniz. 

### <a name="can-i-use-the-mars-agent-to-back-up-files-and-folders-on-temporary-storage-for-the-azure-vm"></a>Dosya ve klasörleri Azure VM için geçici depolama alanında yedeklemek için MARS Aracısı kullanabilir miyim? 
Evet. MARS aracısı yükleyin ve dosya ve klasörleri geçici depolama alanına Konuk Windows işletim sisteminde yedekleyin. -Geçici depolama verileri silindikten hakkında yedekleme işleri başarısız.
- Geçici depolama verileri silinirse, yalnızca geçici olmayan depolama birimine geri yükleyebilirsiniz.

### <a name="how-do-i-register-a-server-to-another-region"></a>Bir sunucu başka bir bölgeye nasıl kaydedebilirim?

Yedekleme verileri, sunucunun kayıtlı olduğu kasanın veri merkezine gönderilir. Veri merkezini değiştirmenin en kolay yolu, kaldırmak ve aracıyı yeniden yükleyin ve ardından yeni bir kasa makineye ihtiyacınız bölgede kaydetme

### <a name="does-the-mars-agent-support-windows-server-2012-deduplication"></a>MARS Aracısı destek Windows Server 2012 yinelenenleri kaldırma işlemi yapar?
Evet. MARS Aracısı, yedekleme işlemini hazırlarken yinelenenleri kaldırılmış verileri normal verilere dönüştürür. Ardından verileri yedekleme için en iyi duruma getirir, verileri şifreler ve şifrelenmiş verileri kasaya ardından gönderir.

## <a name="manage-backups"></a>Yedekleri yönetme

### <a name="what-happens-if-i-rename-a-windows-machine-configured-for-backup"></a>Ben yedekleme için yapılandırılmış bir Windows makine yeniden adlandırırsanız ne olur?

Windows makine yeniden adlandırdığınızda, geçerli olarak yapılandırılmış tüm yedeklemeler durdurulur.

- Yeni makine adını Backup kasasına kaydetmeniz gerekir.
- Yeni adı kasaya kaydettiğinizde, ilk işlem, bir *tam* yedekleme.
- Eski sunucu adıyla kasaya yedeklenen verileri kurtarmanız gerekiyorsa, Veri Kurtarma Sihirbazı'nı alternatif bir konuma geri yüklemek için bu seçeneği kullanın. [Daha fazla bilgi edinin](backup-azure-restore-windows-server.md#use-instant-restore-to-restore-data-to-an-alternate-machine). 

### <a name="what-is-the-maximum-file-path-length-for-backup"></a>Yedekleme için en fazla dosya yolu uzunluğu nedir?
MARS Aracısı NTFS kullanır ve sınırlı dosya yolu uzunluğu belirtimi kullanır [Windows API](/windows/desktop/FileIO/naming-a-file#fully_qualified_vs._relative_paths). Korumak istediğiniz dosyaları üst klasörünü veya disk sürücüsünü yedekleyin, izin verilen değer daha uzun olması durumunda.  

### <a name="what-characters-are-allowed-in-file-paths"></a>Dosya yolları hangi karakterlere izin verilir?

MARS Aracısı NTFS kullanır ve sağlayan [karakterler](/windows/desktop/FileIO/naming-a-file#naming_conventions) dosya adları/yollarda.

### <a name="the-warning-azure-backups-have-not-been-configured-for-this-server-appears"></a>"Azure yedeklemeleri bu sunucu için yapılandırılmamış" uyarısı görüntülenir.
Bu uyarı, yerel sunucuda depolanan yedekleme zamanlaması ayarları yedekleme kasasında depolanan ayarlarla aynı olmadığında bir yedekleme İlkesi yapılandırmış olduğunuz olsa bile görünebilir.
- Sunucu ya da ayarlar bilinen bir iyi duruma getirilerek kurtarıldığında, yedekleme zamanlamaları eşitlenmemiş hale gelebilir.
- Bu uyarıyı alırsanız [yapılandırma](backup-azure-manage-windows-server.md) yedekleme ilkesini yeniden ve ardından yerel sunucuyu Azure ile yeniden eşitlemek için Çalıştır isteğe bağlı bir yedekleme.


## <a name="manage-the-backup-cache-folder"></a>Yedekleme önbelleği klasörü Yönet

### <a name="whats-the-minimum-size-requirement-for-the-cache-folder"></a>Önbellek klasörü için minimum boyut gereksinimini nedir?
Önbellek klasörünün boyutu, yedeklediğiniz veri miktarını belirler.
- Önbellek klasörü birimleri, yedekleme verilerinin toplam boyutunun en az 5-%10 eşittir boş alan olması gerekir.
- Birim % 5'ten az boş alan varsa, birim boyutunu artırın veya önbellek klasörünü yeterli alana sahip bir birime taşıyın.
- 
### <a name="how-do-i-change-the-cache-location-for-the-mars-agent"></a>MARS aracısı için Önbellek konumunu nasıl değiştiririm?


1. Yedekleme Altyapısı durdurmak için yükseltilmiş bir komut isteminde şu komutu çalıştırın:

    ```PS C:\> Net stop obengine```

2. Dosyaları taşımayın. Bunun yerine, önbellek alanı klasörünü yeterli alana sahip başka bir sürücüye kopyalayın.
3. Aşağıdaki kayıt defteri girdilerini yeni önbellek klasörünün yolu ile güncelleştirin.<br/>

    | Kayıt defteri yolu | Kayıt Defteri Anahtarı | Değer |
    | --- | --- | --- |
    | `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config` |ScratchLocation |*Yeni önbellek klasörü konumu* |
    | `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider` |ScratchLocation |*Yeni önbellek klasörü konumu* |

4. Yükseltilmiş bir komut isteminde Backup altyapısını yeniden başlatın:

    ```PS C:\> Net start obengine```

5. Yeni konumu başarıyla kullanarak Yedekleme tamamlandıktan sonra özgün önbellek klasörünü kaldırabilirsiniz.


### <a name="where-should-the-cache-folder-be-located"></a>Burada önbellek klasörünün bulunduğu olmalıdır?

Önbellek klasörü için aşağıdaki konumlar önerilmez:

* Ağ paylaşımı/çıkarılabilir medya: Önbellek klasörü, çevrimiçi yedekleme kullanılarak yedeklenmesi gereken sunucu için yerel olmalıdır. Ağ konumlarını veya USB sürücüleri gibi çıkarılabilir medya desteklenmez.
* Çevrimdışı birimler: Önbellek klasörü Azure Backup Aracısı kullanılarak gerçekleştirilecek beklenen yedekleme için çevrimiçi olması gerekir

### <a name="are-there-any-attributes-of-the-cache-folder-that-arent-supported"></a>Desteklenmeyen herhangi bir özniteliği önbellek klasörünün var mı?
Aşağıdaki öznitelikler veya bunların bileşimleri, önbellek klasörü için desteklenmez:

* Şifreli
* Yinelenenleri kaldırma işlemi uygulanmış
* Sıkıştırılmış
* Seyrek
* Yeniden Ayrıştırma Noktası

Önbellek klasörü ve meta veri VHD’si, Azure Backup aracısı için gerekli özniteliklere sahip değildir.

### <a name="is-there-a-way-to-adjust-the-amount-of-bandwidth-used-for-backup"></a>Yedekleme için kullanılan bant genişliği miktarını ayarlamanın bir yolu var mı?
 
Evet, kullanabileceğiniz **özelliklerini değiştirme** MARS Aracısı bant genişliği ve zamanlama ayarlamak için seçeneği. [Daha fazla bilgi edinin](backup-configure-vault.md#enable-network-throttling)**.

## <a name="restore"></a>Geri Yükleme

### <a name="what-happens-if-i-cancel-an-ongoing-restore-job"></a>Ben bir devam eden geri yükleme işi iptal edersem ne olur?

Devam eden geri yükleme işi iptal edilirse, geri yükleme işlemi durdurur. Tüm dosyaları iptalden önce geri herhangi bir geri alma işlemleri yapılandırılan hedef (konum), özgün veya alternatif olarak kalır.


## <a name="next-steps"></a>Sonraki adımlar

[Bilgi](tutorial-backup-windows-server-to-azure.md) Windows makinesini yedekleme.
