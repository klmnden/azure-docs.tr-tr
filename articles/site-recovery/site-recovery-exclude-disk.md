---
title: "Diski Azure Site Recovery ile korumanın dışında tutma | Microsoft Belgeleri"
description: "VM disklerinin Vmware’den Azure’a ve Hyper-V’den Azure’a senaryolarında çoğaltma işleminden neden ve nasıl dışlanabileceğini açıklamaktadır."
services: site-recovery
documentationcenter: 
author: nsoneji
manager: garavd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 1/24/2017
ms.author: nisoneji
translationtype: Human Translation
ms.sourcegitcommit: af0d66d92ca542f415de779fb638db166ba5f26a
ms.openlocfilehash: 5e0527fb0a41d8892c9e22d6d6d2f252972e69d0


---
#<a name="exclude-disk-from-replication"></a>Diski çoğaltmanın dışında tutma
Bu makalede, çoğaltma bant genişliğini veya çoğaltılan diskler tarafından kullanılan hedef tarafı kaynaklarını iyileştirmek için disklerin nasıl çoğaltmanın dışında tutulabileceği açıklanmaktadır. Bu özellik, Vmware’den Azure’a ve Hyper-V’den Azure’a senaryoları için desteklenir.

##<a name="prerequisites"></a>Ön koşullar

Varsayılan olarak, bir makinedeki tüm diskler çoğaltılır. **Vmware’den Azure’a** çoğaltma yapıyorsanız diskleri çoğaltmanın dışında tutmak için, çoğaltmayı etkinleştirmeden önce Mobility hizmeti makineye elle yüklenmelidir.


## <a name="why-exclude-disks-from-replication"></a>Diskleri çoğaltmanın dışında tutma nedenleri nelerdir?
Disklerin çoğaltmanın dışında tutulması, çoğu zaman aşağıdaki nedenlerden dolayı gereklidir:

1. Dışlanan diskteki verilerin değişim sıklığı önemli değildir veya bunların çoğaltılması gerekmez.

2. Bu değişen verilerin çoğaltılmaması depolama ve ağ kaynaklarından tasarruf sağlayabilir.

##<a name="what-are-the-typical-scenarios"></a>Tipik senaryolar nelerdir?
Disk belleği dosyasına yazma, Microsoft SQL Server tempdb veritabanına yazma gibi bazı veri değişim sıklığı örnekleri, kolayca belirlenebilir ve dışlama için çok uygundur. İş yüküne ve depolama alt sistemine bağlı olarak, disk belleği dosyasında önemli ölçüde değişim gerçekleşebilir. Ancak bu verilerin birincil siteden Azure'a çoğaltılması yoğun bir kaynak kullanımına neden olacaktır. Bu nedenle, hem işletim sistemi hem de disk belleği dosyasının bulunduğu tek bir sanal diske sahip bir VM’nin çoğaltılması aşağıdaki şekilde iyileştirilebilir:

1. Tek sanal disk, biri işletim sistemini diğeri disk belleği dosyasını içeren iki sanal diske bölünür.
2. Disk belleği dosyasının bulunduğu disk çoğaltmanın dışında tutulur.

Benzer biçimde, tempdb ve sistem veritabanı dosyasının aynı disk üzerinde bulunduğu Microsoft SQL Server aşağıdaki şekilde iyileştirilebilir:

1. Sistem veritabanı ve tempdb iki farklı diskte tutulur.
2. Tempdb diski çoğaltmanın dışında tutulur.

##<a name="how-to-exclude-disk-from-replication"></a>Diski çoğaltmanın dışında tutma

###<a name="vmware-to-azure"></a>Vmware’den Azure’a
Azure Site Recovery portalında bir VM’yi korumaya yönelik [Çoğaltmayı etkinleştirme](site-recovery-vmware-to-azure.md#enable-replication) iş akışını izleyin. Çoğaltmayı etkinleştirmenin 4. adımındaki **ÇOĞALTILACAK DİSK** sütunu, diski çoğaltmanın dışında tutmak için kullanılabilir. Varsayılan olarak, tüm diskler seçilir. Çoğaltmanın dışında tutmak istediğiniz diskin seçimini kaldırın ve çoğaltmayı etkinleştirme adımlarını tamamlayın. 

![Çoğaltmayı etkinleştirme](./media/site-recovery-exclude-disk/v2a-enable-replication-exclude-disk1.png)
    
    
>[!NOTE]
> 
> * Yalnızca Mobility hizmeti zaten yüklü olan diskleri çoğaltmanın dışında tutabilirsiniz. Mobility hizmeti, ancak çoğaltma etkinleştirildikten sonra gönderim mekanizması kullanılarak yüklendiğinden Mobility hizmetini elle yüklemeniz gerekir.
> * Yalnızca temel diskler çoğaltma dışı bırakılabilir. İşletim sistemi disklerini veya dinamik diskleri çoğaltmanın dışında tutamazsınız.
> * Çoğaltmayı etkinleştirdikten sonra çoğaltma için disk ekleme veya kaldırma gerçekleştiremezsiniz. Disk eklemek veya dışlamak istiyorsanız, makinenin korumasını devre dışı bırakmanız ve yeniden etkinleştirmeniz gerekir.
> * Bir uygulamanın çalışması için gerekli olan bir diski dışlarsanız Azure'a yük devretme gerçekleştirildikten sonra, çoğaltılan uygulamanın çalışması için diski Azure'da el ile oluşturmanız gerekir. Alternatif olarak makinenin yük devri sırasında diskin otomatik olarak oluşturulması için Azure otomasyonunu bir kurtarma planıyla tümleştirebilirsiniz.
> * Windows VM: Azure'da elle oluşturduğunuz diskler yeniden çalışır duruma getirilmez. Örneğin, üç disk için yük devretme gerçekleştirirseniz ve iki diski doğrudan Azure VM'de oluşturursanız, yalnızca yük devretme gerçekleştirilen üç disk yeniden çalışır duruma getirilir. El ile oluşturulmuş diskleri, yeniden çalışma veya yeniden şirket içinden Azure'a koruma uygulama işlemlerine dahil edemezsiniz.
> * Linux VM: Azure'da elle oluşturduğunuz diskler yeniden çalışır duruma getirilir. Örneğin, üç disk için yük devretme gerçekleştirirseniz ve iki diski doğrudan Azure'da oluşturursanız, beş diskin tümü yeniden çalışır duruma getirilir. El ile oluşturulmuş diskleri, yeniden çalışmanın dışında tutamazsınız.
> 

###<a name="hyper-v-to-azure"></a>Hyper-V’den Azure’a
Azure Site Recovery portalında bir VM’yi korumaya yönelik [Çoğaltmayı etkinleştirme](site-recovery-hyper-v-site-to-azure.md#step-6-enable-replication) iş akışını izleyin. Çoğaltmayı etkinleştirmenin 4. adımındaki **ÇOĞALTILACAK DİSK** sütunu, diskleri çoğaltmanın dışında tutmak için kullanılabilir. Varsayılan olarak, tüm diskler çoğaltma için seçilir. Çoğaltmanın dışında tutmak istediğiniz diskin seçimini kaldırın ve çoğaltmayı etkinleştirme adımlarını tamamlayın. 

![Çoğaltmayı etkinleştirme](./media/site-recovery-vmm-to-azure/enable-replication6-with-exclude-disk.png)
    
>[!NOTE]
> 
> * Yalnızca temel diskler çoğaltma dışı bırakılabilir. İşletim sistemi diski çoğaltmanın dışında tutulamaz ve dinamik disklerin çoğaltmanın dışında tutulması önerilmez. ASR, konuk VM içindeki VHD disklerinin hangisinin temel, hangisinin dinamik olduğunu belirleyemez.  Bağımlı dinamik hacim disklerinin tümü dışlanmazsa, korumalı dinamik disk yük devredilen VM'de hatalı disk olarak gelir ve diskteki verilere erişim sağlanamaz.    
> * Çoğaltmayı etkinleştirdikten sonra çoğaltma için disk ekleme veya kaldırma gerçekleştiremezsiniz. Disk eklemek veya dışlamak istiyorsanız, VM’nin korumasını devre dışı bırakmanız ve yeniden etkinleştirmeniz gerekir.
> * Bir uygulamanın çalışması için gerekli olan bir diski hariç tutarsanız, Azure'a yük devretme gerçekleştirildikten sonra çoğaltılan uygulamanın çalışması için Azure'da el ile oluşturmanız gerekir. Alternatif olarak makinenin yük devri sırasında diskin otomatik olarak oluşturulması için Azure otomasyonunu bir kurtarma planıyla tümleştirebilirsiniz.
> * Azure'da elle oluşturduğunuz diskler yeniden çalışır duruma getirilmez. Örneğin, üç disk için yük devretme gerçekleştirirseniz ve ikisini doğrudan Azure VM'de oluşturursanız, yalnızca yük devretme gerçekleştirilen üç disk Azure'dan Hyper-V'ye çoğaltılarak yeniden çalışır hale getirilir. El ile oluşturulmuş diskleri yeniden çalışma veya Hyper-V'den Azure'a geri çoğaltma işlemine dahil edemezsiniz.
 


##<a name="end-to-end-scenarios-of-exclude-disks"></a>Disk dışlamaya ilişkin uçtan uca senaryolar
Disk dışlama özelliğini daha iyi anlamak için iki senaryoyu düşünelim.

1. SQL Server tempdb diski
2. Disk belleği dosyası diski

###<a name="excluding-the-sql-server-tempdb-disk"></a>SQL Server tempdb diskini dışlama
Dışlanabilecek bir tempdb’si olan bir SQL Server sanal makinesi düşünelim.

VM’nin adı: Kaynak VM’de SalesDB Diskleri:


**Disk Adı** | **Konuk işletim sistemi disk#** | **Sürücü harfi** | **Diskteki veri türü**
--- | --- | --- | ---
DB-Disk0-OS | DISK0 | C:\ | İşletim sistemi diski
DB-Disk1| Disk1 | D:\ | SQL sistem veritabanı ve Kullanıcı Veritabanı1
DB-Disk2 (Disk, korumanın dışında tutuldu) | Disk2 | E:\ | Geçici dosyalar
DB-Disk3 (Disk, korumanın dışında tutuldu) | Disk3 | F:\ | SQL tempdb veritabanı (klasör yolu (F:\MSSQL\Data\) --> yük devretmeden önce klasör yolunu not edin
DB-Disk4 | Disk4 |G:\ |Kullanıcı Veritabanı2

VM’nin iki diskindeki veri değişim sıklığı geçici yapıda olduğundan, SalesDB VM’yi korurken 'Disk2' ve 'Disk3' disklerini çoğaltmanın dışında tutun. Azure Site Recovery bu diskleri çoğaltmaz ve yük devretme sonrasında bu diskler Azure’daki yük devretme VM'sinde yer almaz

Yük devretme sonrasında Azure VM'deki diskler:

**Konuk işletim sistemi disk#** | **Sürücü harfi** | **Diskteki veri türü**
--- | --- | ---
DISK0 |    C:\ | İşletim sistemi diski
Disk1 |    E:\ | Geçici depolama [Azure bu diski ekler ve kullanılabilir ilk sürücü harfini buna atar]
Disk2 | D:\ | SQL sistem veritabanı ve Kullanıcı Veritabanı1
Disk3 | G:\ | Kullanıcı Veritabanı2

Disk2 ve Disk3, SalesDB VM'nin dışında tutulduğundan kullanılabilir sürücü harfleri listesindeki ilk harf E: olur. Azure, geçici depolama birimine E: harfini atar. Sürücü harfi, çoğaltılmış tüm diskler için aynı kalır.

SQL tempdb diski olan ve çoğaltmanın dışında tutulan Disk3 (tempdb klasör yolu F:\MSSQL\Data\) yük devretme VM’sinde yer almaz. Bunun sonucunda, SQL hizmeti durdurulmuş durumdadır ve F:\MSSQL\Data yolu gerekir.

Bu yolu oluşturmanın iki yöntemi vardır.

1. Yeni bir disk ekleyip tempdb klasör yolunu atayın veya
2. Var olan geçici depolama diskini, tempdb klasör yolu için kullanın

####<a name="add-a-new-disk"></a>Yeni bir disk ekleme:

1. Yük devretme öncesine SQL tempdb.mdf ve tempdb.ldf yolunu not edin.
2. Azure portalında yük devretme VM’sine, kaynak SQL tempdb diskiyle (Disk3) aynı boyutta veya daha büyük yeni bir disk ekleyin.
3. Azure VM'de oturum açın. Disk yönetimi (diskmgmt.msc) konsolundan yeni eklenen diski başlatıp biçimlendirin.
4. SQL tempdb diski için kullanılan sürücü harfinin aynısını (F:) atayın.
5. F: biriminde tempdb klasörünü (F:\MSSQL\Data) oluşturun.
6. Hizmet konsolundan SQL hizmetini başlatın.

####<a name="use-existing-temporary-storage-disk-for-sql-tempdb-folder-path"></a>Var olan geçici depolama diskini, SQL tempdb klasör yolu için kullanın:

1. Komut satırı konsolu açın
2. Komut satırı konsolundan SQL Server’ı kurtarma modunda çalıştırın

        Net start MSSQLSERVER /f / T3608

3. Tempdb yolunu yeni yola değiştirmek için aşağıdaki sqlcmd’yi çalıştırın

        sqlcmd -A -S SalesDB        **Use your SQL DBname**
        USE master;        
        GO        
        ALTER DATABASE tempdb        
        MODIFY FILE (NAME = tempdev, FILENAME = 'E:\MSSQL\tempdata\tempdb.mdf');
        GO        
        ALTER DATABASE tempdb        
        MODIFY FILE (NAME = templog, FILENAME = 'E:\MSSQL\tempdata\templog.ldf');        
        GO


4. Microsoft SQL Server hizmetini durdurun.

        Net stop MSSQLSERVER
5. Microsoft SQL Server hizmetini başlatın.

        Net start MSSQLSERVER

Geçici depolama diskiyle ilgili olarak aşağıdaki Azure kılavuzlarına bakın

* SQL Server TempDB’yi ve Arabellek Havuzu Genişletmelerini depolamak için Azure VM'lerde SSD kullanma
* Azure Sanal Makinelerde SQL Server için performansa yönelik en iyi yöntemler

###<a name="failback-from-azure-to-on-premises"></a>Yeniden çalışma (Azure'dan şirket içine)
Azure'dan şirket içi VMware veya Hyper-V konağınıza yük devretme yaptığınızda hangi disklerin çoğaltılacağına göz atalım. Azure'da elle oluşturduğunuz diskler çoğaltılmaz. Örneğin, üç disk için yük devretme gerçekleştirirseniz ve iki diski doğrudan Azure VM'de oluşturursanız, yalnızca yük devretme gerçekleştirilen üç disk yeniden çalışır duruma getirilir. El ile oluşturulmuş diskleri, yeniden çalışma veya yeniden şirket içinden Azure'a koruma uygulama işlemlerine dahil edemezsiniz. Geçici depolama diskleri de şirket içinde çoğaltılmaz.

####<a name="failback-to-original-location-recovery-olr"></a>Özgün konum kurtarması (OLR) ile yeniden çalışma

Yukarıdaki örnekteki Azure VM disk yapılandırması şu şekildedir:

**Konuk işletim sistemi disk#** | **Sürücü harfi** | **Diskteki veri türü** 
--- | --- | --- 
DISK0 | C:\ | İşletim sistemi diski
Disk1 |    E:\ | Geçici depolama [Azure bu diski ekler ve kullanılabilir ilk sürücü harfini buna atar]
Disk2 |    D:\ | SQL sistem veritabanı ve Kullanıcı Veritabanı1
Disk3 |    G:\ | Kullanıcı Veritabanı2


####<a name="vmware-to-azure"></a>Vmware’den Azure’a
Yeniden çalışma özgün konumda gerçekleştirildiğinde, yeniden çalışma VM disk yapılandırması dışlanan diski içermez. Bu nedenle, VMware’den Azure'a çoğaltmanın dışında tutulan diskler yeniden çalışma VM’sinde mevcut olmayacaktır. 

Azure'dan şirket içi VMware’e planlı yük devretmenin ardından VMWare VM’deki diskler (Özgün Konum):

**Konuk işletim sistemi disk#** | **Sürücü harfi** | **Diskteki veri türü** 
--- | --- | --- 
DISK0 | C:\ | İşletim sistemi diski
Disk1 |    D:\ | SQL sistem veritabanı ve Kullanıcı Veritabanı1
Disk2 |    G:\ | Kullanıcı Veritabanı2

####<a name="hyper-v-to-azure"></a>Hyper-V’den Azure’a
Yeniden çalışma özgün konumda gerçekleştirildiğinde, yeniden çalışma VM disk yapılandırması, Hyper-V’ye ait özgün VM disk yapılandırması ile aynı kalır. Bu nedenle, Hyper-V sitesinden Azure'a çoğaltmanın dışında tutulan diskler yeniden çalışma VM’sinde yer alacaktır.

Azure'dan şirket içi Hyper-V’ye planlı yük devretmenin ardından Hyper-V VM’deki diskler (Özgün Konum):

**Disk Adı** | **Konuk işletim sistemi disk#** | **Sürücü harfi** | **Diskteki veri türü**
--- | --- | --- | ---
DB-Disk0-OS | DISK0 |    C:\ | İşletim sistemi diski
DB-Disk1 | Disk1 | D:\ | SQL sistem veritabanı ve Kullanıcı Veritabanı1
DB-Disk2 (Dışlanan disk) | Disk2 | E:\ | Geçici dosyalar
DB-Disk3 (Dışlanan disk) | Disk3 | F:\ | SQL tempdb veritabanı (klasör yolu (F:\MSSQL\Data\)
DB-Disk4 | Disk4 | G:\ | Kullanıcı Veritabanı2


####<a name="exclude-paging-file-disk"></a>Disk belleği dosyası diskini dışlama

Dışlanabilecek bir disk belleği dosyası diski olan bir sanal makine düşünelim.
Olası iki durum vardır:

####<a name="case-1-pagefile-is-configured-on-the-d-drive"></a>Durum 1: Disk belleği dosyası D: sürücüsünde yapılandırılmış
Disk yapılandırması:


**Disk adı** | **Konuk işletim sistemi disk#** | **Sürücü harfi** | **Diskteki veri türü**
--- | --- | --- | ---
DB-Disk0-OS | DISK0 | C:\ | İşletim sistemi diski
DB-Disk1 (Disk, korumanın dışında tutuldu) | Disk1 | D:\ | pagefile.sys
DB-Disk2 | Disk2 | E:\ | Kullanıcı verileri 1
DB-Disk3 | Disk3 | F:\ | Kullanıcı verileri 2

Kaynak VM’deki disk belleği dosyası ayarları:

![Çoğaltmayı etkinleştirme](./media/site-recovery-exclude-disk/pagefile-on-d-drive-sourceVM.png)
    

VMware'den Azure’a veya Hyper-V'den Azure’a VM yük devretmesi sonrasında Azure VM’deki diskler:
**Disk adı** | **Konuk işletim sistemi disk#** | **Sürücü harfi** | **Diskteki veri türü**
--- | --- | --- | ---
DB-Disk0-OS | DISK0 | C:\ | İşletim sistemi diski
DB-Disk1 | Disk1 | D:\ | Geçici depolama – > pagefile.sys
DB-Disk2 | Disk2 | E:\ | Kullanıcı verileri 1
DB-Disk3 | Disk3 | F:\ | Kullanıcı verileri 2

Disk1 (D:) dışlandığından kullanılabilir sürücü harfleri listesindeki ilk harf D: olur. Azure geçici depolama birimine D: harfini atar.  D: Azure VM'de kullanılabilir olduğundan VM’nin disk belleği dosyası ayarları aynı kalır.

Azure VM'de disk belleği dosyası ayarları:

![Çoğaltmayı etkinleştirme](./media/site-recovery-exclude-disk/pagefile-on-Azure-vm-after-failover.png)

####<a name="case-2-pagefile-file-is-configured-on-any-other-driveother-than-d-drive"></a>Durum 2: Disk belleği dosyası başka bir sürücüde (D: sürücüsü dışında) yapılandırılmış

Kaynak VM disk yapılandırması:

**Disk adı** | **Konuk işletim sistemi disk#** | **Sürücü harfi** | **Diskteki veri türü**
--- | --- | --- | ---
DB-Disk0-OS | DISK0 | C:\ | İşletim sistemi diski
DB-Disk1 (Disk, korumanın dışında tutuldu) | Disk1 | G:\ | pagefile.sys
DB-Disk2 | Disk2 | E:\ | Kullanıcı verileri 1
DB-Disk3 | Disk3 | F:\ | Kullanıcı verileri 2

Şirket içi VM’deki disk belleği dosyası ayarları:

![Çoğaltmayı etkinleştirme](./media/site-recovery-exclude-disk/pagefile-on-g-drive-sourceVM.png)

VMware'den/Hyper-V'den Azure’a VM yük devretmesi sonrasında Azure VM’deki diskler:

**Disk adı**| **Konuk işletim sistemi disk#**| **Sürücü harfi** | **Diskteki veri türü**
--- | --- | --- | ---
DB-Disk0-OS | DISK0  |C:\ |İşletim sistemi diski
DB-Disk1 | Disk1 | D:\ | Geçici depolama – > pagefile.sys
DB-Disk2 | Disk2 | E:\ | Kullanıcı verileri 1
DB-Disk3 | Disk3 | F:\ | Kullanıcı verileri 2

D: kullanılabilir sürücü harfleri listesindeki ilk harf olduğundan Azure geçici depolama birimine D: harfini atar. Sürücü harfi, çoğaltılmış tüm diskler için aynı kalır. G: diski mevcut olmadığından sistem, disk belleği dosyası için C: sürücüsünü kullanır.

Azure VM'de disk belleği dosyası ayarları:

![Çoğaltmayı etkinleştirme](./media/site-recovery-exclude-disk/pagefile-on-Azure-vm-after-failover-2.png)

## <a name="next-steps"></a>Sonraki adımlar
Dağıtımınız ayarlandıktan ve çalışmaya başladıktan sonra farklı türdeki yük devretmeler hakkında [daha fazla bilgi edinebilirsiniz](site-recovery-failover.md).



<!--HONumber=Feb17_HO3-->


