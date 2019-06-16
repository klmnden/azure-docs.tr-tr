---
title: Azure Backup sunucusu ile VMware sanal makinelerini yedekleme
description: Bir VMware vCenter/ESXi sunucusuna çalıştıran VMware sanal makinelerini yedeklemek için Azure Backup sunucusu kullanın.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 12/11/2018
ms.author: raynew
ms.openlocfilehash: f034f31f2c8c49bbdfb88e2ba0a009ff5b795fa2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65789614"
---
# <a name="back-up-vmware-vms-with-azure-backup-server"></a>Azure Backup sunucusu ile VMware sanal makinelerini yedekleme

Bu makalede, Azure Backup sunucusu kullanarak Azure'a VMware ESXi ana bilgisayarları/vCenter Server'ı çalıştıran VMware sanal makinelerini yedeklemek açıklanmaktadır.

Bu makalede açıklanır nasıl yapılır:

- Azure Backup sunucusu VMware sunucuları ile HTTPS üzerinden iletişim kurabilmesi için güvenli bir kanalı ayarlayın.
- VMware sunucusuna erişmek için Azure Backup sunucusu kullanan bir VMware hesabı ayarlama.
- Azure Backup için hesap kimlik bilgilerini ekleyin.
- VCenter veya ESXi server için Azure Backup sunucusu ekleyin.
- Yedekleme, Yedekleme ayarlarını belirtin ve yedekleme zamanlama istediğiniz VMware Vm'leri içeren bir koruma grubu ayarlayın.

## <a name="before-you-start"></a>Başlamadan önce
- Yedekleme - sürüm 6.5, 6.0 ve 5.5 desteklenen vCenter/ESXi sürümünü çalıştırdığınızı doğrulayın.
- Azure Backup sunucusu ayarladığınızdan emin olun. Siz yapmadıysanız [bunun](backup-azure-microsoft-azure-backup.md) başlamadan önce. Azure Backup sunucusu ile en son güncelleştirmeleri çalıştırıyor olması gerekir.


## <a name="create-a-secure-connection-to-the-vcenter-server"></a>VCenter Server için güvenli bir bağlantı oluşturun

Varsayılan olarak, Azure Backup sunucusu VMware sunucularıyla HTTPS üzerinden iletişim kurar. HTTPS bağlantı kurmak için VMware sertifika yetkilisi (CA) sertifikasını indirin ve Azure Backup sunucusuna aktarın.


### <a name="before-you-start"></a>Başlamadan önce

- HTTPS kullanmak istemiyorsanız yapabilecekleriniz [tüm VMware sunucuları için HTTPS sertifikası doğrulamasını devre dışı bırakmak](backup-azure-backup-server-vmware.md#disable-https-certificate-validation).
- Genellikle Azure Backup sunucusu makinesindeki bir tarayıcıdan vSphere Web istemcisi kullanarak vCenter/ESXi sunucusuna bağlanırsınız. Bağlantı bunu ilk kez güvenli değildir ve şunu verecektir.
- Azure Backup sunucusu yedekleme nasıl işlediğini anlamanız önemlidir.
    - İlk adım olarak, Azure Backup sunucusu yerel disk depolama alanına verileri yedekler. Azure Backup sunucusu, bir depolama havuzu, diskler ve birimlerle Azure Backup sunucusu, korunan verilerin kurtarma noktalarını disk depoladığı kümesi kullanır. Depolama havuzu, doğrudan bağlı depolama (DAS), bir fiber kanal SAN veya iSCSI depolama cihazı veya SAN olabilir. Yerel arka için yeterli depolama alanı VMware VM verilerinizi sağlamak önemlidir.
    - Azure Backup sunucusu sonra yerel disk depolama alanından Azure'da yedekler.
    - [Yardım alma](https://docs.microsoft.com/system-center/dpm/create-dpm-protection-groups?view=sc-dpm-1807#figure-out-how-much-storage-space-you-need) ne kadar depolama alanı, bulmanız gerekir. Bilgiler için DPM, ancak Azure Backup sunucusu için çok kullanılabilir.

### <a name="set-up-the-certificate"></a>Sertifika ayarlama

Güvenli bir kanalı aşağıdaki gibi ayarlayın:

1. Azure Backup sunucusu üzerinde bir tarayıcıda, vSphere Web istemcisi URL'si girin. Oturum açma sayfasına görünmüyorsa, bağlantı ve Tarayıcı proxy ayarlarını doğrulayın.

    ![vSphere Web istemcisi](./media/backup-azure-backup-server-vmware/vsphere-web-client.png)

2. VSphere Web istemcisi oturum açma sayfasında tıklayın **indirme güvenilen kök CA sertifikaları**.

    ![Güvenilen kök CA sertifikasını indir](./media/backup-azure-backup-server-vmware/vmware-download-ca-cert-prompt.png)

3. Adlı bir dosya **indirme** indirilir. Tarayıcınıza bağlı olarak mı dosyasını açmanız veya kaydetmeniz soran bir ileti alırsınız.

    ![CA sertifikasını indir](./media/backup-azure-backup-server-vmware/download-certs.png)

4. Azure Backup sunucusu makinesindeki dosyayı bir .zip uzantısı ile kaydedin.

5. Sağ **download.zip** > **tümünü Ayıkla**. .Zip dosyasının içeriğini ayıklar **sertifikaları** içeren klasörü:
   - Kök sertifika dosyasını.0 ve.1 gibi numaralı bir dizi ile başlayan bir uzantıya sahip.
   - CRL dosyasını .r0 veya .r1 gibi bir dizi ile başlayan uzantısına sahiptir. CRL dosyasını bir sertifika ile ilişkilidir.

     ![İndirilen sertifika](./media/backup-azure-backup-server-vmware/extracted-files-in-certs-folder.png)

5. İçinde **sertifikaları** klasörü, kök sertifika dosyasını sağ tıklayın > **Yeniden Adlandır**.

    ![Kök sertifikayı yeniden adlandır](./media/backup-azure-backup-server-vmware/rename-cert.png)

6. .Crt için kök sertifikanın uzantısını değiştirir ve onaylayın. Bir kök sertifikayı temsil eden bir dosya simgesi değişir.

7. Kök sertifikaya sağ tıklayın ve açılan menüden **sertifikayı yükle**.

8. İçinde **Sertifika Alma Sihirbazı**seçin **yerel makine** sertifika ve ardından hedefi olarak **sonraki**. Bilgisayarda değişiklikler izin vermek isteyip istemediğinizi sorulur olmadığını onaylayın.

    ![Sihirbazına Hoş Geldiniz](./media/backup-azure-backup-server-vmware/certificate-import-wizard1.png)


9. Üzerinde **sertifika Store** sayfasında **tüm sertifikaları aşağıdaki depolama alanına yerleştir**ve ardından **Gözat** sertifika deposunu seçme.

     ![Sertifika depolama](./media/backup-azure-backup-server-vmware/cert-import-wizard-local-store.png)

10. İçinde **seçin sertifika Store**seçin **güvenilen kök sertifika yetkilileri** sertifikaları ve ardından için hedef klasör olarak **Tamam**.

    ![Sertifika hedef klasör](./media/backup-azure-backup-server-vmware/certificate-store-selected.png)

11. İçinde **sertifika İçeri Aktarma Sihirbazı Tamamlanıyor**klasörü doğrulayın ve ardından **son**.

    ![Sertifika uygun klasöründe olduğunu doğrulayın](./media/backup-azure-backup-server-vmware/cert-wizard-final-screen.png)


12. Sertifika içeri aktarma onaylandıktan sonra vCenter Server'a bağlantınızı güvenli olduğundan emin olmak için oturum açın.




### <a name="disable-https-certificate-validation"></a>HTTPS sertifika doğrulaması devre dışı bırak

Kuruluşunuzun içinde güvenli sınırları olan ve VMware sunucularını ve Azure Backup sunucusu makine arasında HTTPS protokolünü kullanmak istemiyorsanız, aşağıdaki gibi HTTPS'yi devre dışı: u
1. Kopyalayıp bir .txt dosyasına aşağıdaki metni yapıştırın.

      ```
      Windows Registry Editor Version 5.00
      [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft Data Protection Manager\VMWare]
      "IgnoreCertificateValidation"=dword:00000001
      ```

2. Azure Backup sunucusu makine üzerinde dosya adıyla kaydedin **DisableSecureAuthentication.reg**.

3. Kayıt defteri girişini etkinleştirmek için dosyaya çift tıklayın.


## <a name="create-a-vmware-role"></a>Bir VMware rolü oluşturun

V-Center sunucusu erişim izinleri olan bir kullanıcı hesabı Azure yedekleme sunucusu gerekli/ESXi ana bilgisayarı. Belirli ayrıcalıklarına sahip bir VMware rol oluşturmak ve ardından bir kullanıcı rolü ile ilişkilendir.

1. VCenter Server (veya vCenter sunucusu kullanmıyorsanız ESXi ana bilgisayarı) oturum açın.
2. İçinde **Gezgin** panelinde, tıklayın **Yönetim**.

    ![Yönetim](./media/backup-azure-backup-server-vmware/vmware-navigator-panel.png)

3. İçinde **Yönetim** > **rolleri**, Rol Ekle simgesini (+ simgesi).

    ![Rol Ekle](./media/backup-azure-backup-server-vmware/vmware-define-new-role.png)


4. İçinde **Rol Oluştur** > **rol adı**, girin *BackupAdminRole*. Rol adı dilediğiniz olabilir ancak rolün amaçla tanınabilir olmalıdır.

5. Aşağıdaki tabloda özetlenen ayrıcalıklarıyla seçin ve ardından **Tamam**.  Yeni rol listede görünür **rolleri** paneli.
   - Üst genişletmek ve alt ayrıcalıkları görüntülemek için üst etiket yanındaki simgeye tıklayın.
   - VirtualMachine ayrıcalıkları seçmek için üst alt hiyerarşide birden fazla düzeyi gitmeniz gerekiyor.
   - Üst ayrıcalık içindeki tüm alt ayrıcalıklarını seçin gerek yoktur.

     ![Üst alt ayrıcalık hiyerarşisi](./media/backup-azure-backup-server-vmware/cert-add-privilege-expand.png)

### <a name="role-permissions"></a>Rol izinleri
**6.5/6.0** | **5.5**
--- | ---
Datastore.AllocateSpace | Datastore.AllocateSpace
Global.ManageCustomFields | Global.ManageCustomFields
Global.SetCustomField |
Host.Local.CreateVM | Network.Assign
Network.Assign |
Resource.AssignVMToPool |
VirtualMachine.Config.AddNewDisk  | VirtualMachine.Config.AddNewDisk   
VirtualMachine.Config.AdvancedConfig| VirtualMachine.Config.AdvancedConfig
VirtualMachine.Config.ChangeTracking| VirtualMachine.Config.ChangeTracking
VirtualMachine.Config.HostUSBDevice |
VirtualMachine.Config.QueryUnownedFiles |
VirtualMachine.Config.SwapPlacement| VirtualMachine.Config.SwapPlacement
VirtualMachine.Interact.PowerOff| VirtualMachine.Interact.PowerOff
VirtualMachine.Inventory.Create| VirtualMachine.Inventory.Create
VirtualMachine.Provisioning.DiskRandomAccess |
VirtualMachine.Provisioning.DiskRandomRead | VirtualMachine.Provisioning.DiskRandomRead
VirtualMachine.State.CreateSnapshot | VirtualMachine.State.CreateSnapshot
VirtualMachine.State.RemoveSnapshot | VirtualMachine.State.RemoveSnapshot




## <a name="create-a-vmware-account"></a>Bir VMware hesabı oluşturun

1. Vcenter Server'daki **Gezgin** panelinde, tıklayın **kullanıcılar ve gruplar**. VCenter Server kullanmıyorsanız, uygun bir ESXi ana bilgisayarındaki hesabı oluşturun.

    ![Kullanıcılar ve gruplar seçeneği](./media/backup-azure-backup-server-vmware/vmware-userandgroup-panel.png)

    **VCenter kullanıcılar ve gruplar** paneli görüntülenir.


2. İçinde **vCenter kullanıcılar ve gruplar** paneli, select **kullanıcılar** sekmesini ve sonra kullanıcılar ekleme simgesini (+ simgesi).

     ![vCenter kullanıcılar ve gruplar paneli](./media/backup-azure-backup-server-vmware/usersandgroups.png)


3. İçinde **yeni kullanıcı** iletişim kutusunda, kullanıcı bilgilerini Ekle > **Tamam**. Bu yordamda, BackupAdmin kullanıcı adıdır.

    ![Yeni kullanıcı iletişim kutusu](./media/backup-azure-backup-server-vmware/vmware-new-user-account.png)


4. Kullanıcı hesabı içinde rolüyle ilişkilendirmek için **Gezgin** panelinde, tıklayın **genel izinleri**. İçinde **genel izinleri** paneli, select **Yönet** sekmesini ve sonra Ekle simgesini (+ simgesi).

    ![Genel izinler paneli](./media/backup-azure-backup-server-vmware/vmware-add-new-perms.png)


5. İçinde **genel izin Root - ekleme izni**, tıklayın **Ekle** kullanıcı veya grup seçmek için.

    ![Kullanıcı veya grup seçin](./media/backup-azure-backup-server-vmware/vmware-add-new-global-perm.png)

6. İçinde **kullanıcıları/Grupları Seç**, seçin **BackupAdmin** > **Ekle**. İçinde **kullanıcılar**, *etki alanı\kullanıcı adı* biçimi, kullanıcı hesabı için kullanılır. Farklı bir etki alanı kullanmak istiyorsanız, buradan seçin **etki alanı** listesi. Tıklayın **Tamam** Seçili kullanıcılar için eklenecek **ekleme izni** iletişim kutusu.

    ![BackupAdmin kullanıcı ekleme](./media/backup-azure-backup-server-vmware/vmware-assign-account-to-role.png)


7.  İçinde **atanmış rol**, aşağı açılan listesinden **BackupAdminRole** > **Tamam**.

    ![Kullanıcı rolüne atayın.](./media/backup-azure-backup-server-vmware/vmware-choose-role.png)


Üzerinde **Yönet** sekmesinde **genel izinleri** paneli, yeni kullanıcı hesabı ve ilişkili rol listede görünür.


## <a name="add-the-account-on-azure-backup-server"></a>Azure Backup sunucusu hesabı Ekle


1. Azure Backup Sunucusu'nu açın. Masaüstünde simge bulamıyorsanız, Microsoft Azure Backup uygulamalar listesinden açın.

    ![Azure Backup sunucusu simgesi](./media/backup-azure-backup-server-vmware/mabs-icon.png)

2. Azure Backup sunucusu konsolunda **Yönetim** >  **üretim sunucularına** > **yönetme VMware**.

    ![Azure Backup Sunucusu konsolu](./media/backup-azure-backup-server-vmware/add-vmware-credentials.png)


3. İçinde **kimlik bilgilerini Yönet** iletişim kutusu, tıklayın **Ekle**.

    ![Azure Backup sunucusu kimlik bilgilerini Yönet iletişim kutusu](./media/backup-azure-backup-server-vmware/mabs-manage-credentials-dialog.png)

4. İçinde **kimlik bilgileri Ekle** , bir ad ve yeni kimlik bilgisi için bir açıklama girin ve kullanıcı adı ve parola VMware sunucusunda tanımlanan belirtin. Adı *Contoso Vcenter kimlik bilgisi* kimlik bilgisi bu yordamdaki tanımlamak için kullanılır. Azure Backup sunucusu ve VMware sunucusu aynı etki alanında değilse, etki alanı kullanıcı adını belirtin.

    ![Azure Backup sunucusu kimlik bilgisi Ekle iletişim kutusu](./media/backup-azure-backup-server-vmware/mabs-add-credential-dialog2.png)

5. Tıklayın **Ekle** yeni kimlik bilgisi eklemek için.

    ![Azure Backup sunucusu kimlik bilgilerini Yönet iletişim kutusu](./media/backup-azure-backup-server-vmware/new-list-of-mabs-creds.png)


## <a name="add-the-vcenter-server"></a>VCenter Server ekleme

VCenter Server için Azure Backup sunucusu ekleyin.


1. Azure Backup sunucusu konsolunda **Yönetim** > **üretim sunucularına** > **Ekle**.

    ![Açık Üretim Sunucusu Ekleme Sihirbazı](./media/backup-azure-backup-server-vmware/add-vcenter-to-mabs.png)


2. İçinde **Üretim Sunucusu Ekleme Sihirbazı** > **seçin üretim sunucusu türünü** sayfasında **VMware sunucularını**ve ardından **sonraki**.

     ![Üretim Sunucusu Ekleme Sihirbazı](./media/backup-azure-backup-server-vmware/production-server-add-wizard.png)

3. İçinde **bilgisayarları seçin** **sunucu adı/IP adresi**, VMware sunucusu için FQDN veya IP adresini belirtin. Tüm ESXi sunucuları aynı vCenter tarafından yönetiliyorsa, vCenter adını belirtin. Aksi takdirde ESXi ana bilgisayar ekleyin.

    ![VMware sunucusu belirtin](./media/backup-azure-backup-server-vmware/add-vmware-server-provide-server-name.png)

4. İçinde **SSL bağlantı noktası**, VMware sunucusu ile iletişim kurmak için kullanılan bağlantı noktasını girin. Varsayılan bağlantı noktası 443 olduğu halde, VMware sunucusu farklı bir bağlantı noktasında dinliyorsa değiştirebilirsiniz.

5. İçinde **kimlik bilgisi belirtin**, daha önce oluşturduğunuz kimlik bilgilerini seçin.

    ![Kimlik bilgisini belirtin](./media/backup-azure-backup-server-vmware/identify-creds.png)

6. Tıklayın **Ekle** VMware sunucusu sunucuları listesine eklenecek. Ardından **İleri**'ye tıklayın.

    ![VMWare sunucusu ve kimlik bilgisi Ekle](./media/backup-azure-backup-server-vmware/add-vmware-server-credentials.png)

7. İçinde **özeti** sayfasında **Ekle** VMware sunucusu için Azure Backup sunucusu eklemek için. Yeni Sunucu aracı VMware sunucusunda gereklidir hemen eklenir.

    ![Azure Backup sunucusu için VMware sunucusunu ekleme](./media/backup-azure-backup-server-vmware/tasks-screen.png)

8. Ayarları doğrulayın **son** sayfası.

   ![Son sayfa](./media/backup-azure-backup-server-vmware/summary-screen.png)

VCenter sunucusu tarafından yönetilmeyen birden çok ESXi ana bilgisayarları varsa, veya vCenter Server'ın birden çok örneğe sahip sunucuları eklemek için sihirbazı yeniden çalıştırmanız gerekir.




## <a name="configure-a-protection-group"></a>Bir koruma grubunu yapılandırın

VMware Vm'leri için yedekleme ekleyin. Koruma grupları, birden çok VM toplayın ve gruptaki tüm VM'ler aynı veri saklama ve yedekleme ayarlarını uygulamak.


1. Azure Backup sunucusu konsolunda **koruma**, > **yeni**.

    ![Yeni koruma grubu oluşturma Sihirbazı'nı açın](./media/backup-azure-backup-server-vmware/open-protection-wizard.png)

1. İçinde **yeni koruma grubu oluşturma** Sihirbazı Karşılama sayfasını tıklatın **sonraki**.

    ![Yeni koruma grubu oluşturma Sihirbazı iletişim kutusu](./media/backup-azure-backup-server-vmware/protection-wizard.png)

1. Üzerinde **seçin koruma grubu türünü** sayfasında **sunucuları** ve ardından **sonraki**. **Grup üyelerini seçin** sayfası görüntülenir.

1. İçinde **grup üyelerini seçin** > Vm'leri seçin (veya VM klasörler), yedeklemek istediğiniz. Ardından **İleri**'ye tıklayın.

    - Ne zaman bir klasör seçin veya sanal makineleri veya klasör bu klasörün içinde yedekleme için seçilir. Klasörleri veya yedekleme istemediğiniz Vm'leri işaretini kaldırabilirsiniz.
1. Bir VM veya klasör zaten yedeklenen, seçemezsiniz. Bu garanti yinelenen kurtarma noktaları bir VM'nin oluşturulmayacak. .

     ![Grup üyelerini seçin](./media/backup-azure-backup-server-vmware/server-add-selected-members.png)


1. İçinde **veri koruma yöntemini seçin** sayfasında, koruma grubunun ve koruma ayarları için bir ad girin. Azure'a, kısa vadeli koruma döndürülmek için **Disk** ve çevrimiçi korumayı etkinleştirin. Ardından **İleri**'ye tıklayın.

    ![Veri koruma yöntemini seçin](./media/backup-azure-backup-server-vmware/name-protection-group.png)

1. İçinde **kısa vadeli hedefleri belirtin**, diske yedeklenen veri saklamak istediğiniz süreyi belirtin.
   - İçinde **bekletme aralığı**, disk kurtarma noktaları saklanır geçmesi gereken gün sayısını belirtin.
   - İçinde **eşitleme sıklığı**, ne sıklıkta belirtin disk kurtarma noktaları alınır.
       - Bir yedekleme aralığı ayarlamak istemiyorsanız denetleyebilirsiniz **bir kurtarma noktasından hemen önce** böylece yalnızca her kurtarma noktası zamanlanmadan önce bir yedekleme çalıştırır.
       - Kısa dönem yedeklemeler olan tam yedekleme ve artımlı değil.
       - Tıklayın **Değiştir** değiştirmek için kısa vadeli yedekleme gerçekleştiğinde kez/tarihler.

     ![Kısa vadeli hedefleri belirtin](./media/backup-azure-backup-server-vmware/short-term-goals.png)

1. İçinde **Disk ayırmayı gözden**, VM yedeklemeleri için sağlanan disk alanını inceleyin. VM'ler için.

   - Önerilen disk ayırmaları, belirttiğiniz bekletme aralığı, iş yükü türüne ve korunan verilerin boyutunu temel alır. Gerekli değişiklikleri yapın ve ardından **sonraki**.
   - **Veri boyutu:** Koruma grubundaki verilerin boyutu.
   - **Disk alanı:** Önerilen koruma grubu için disk alanı miktarı. Bu ayarı değiştirmek istiyorsanız, her veri kaynağı büyüdükçe tahmin miktardan biraz daha büyük toplam alan ayırın.
   - **Verileri ortak Konumlandır:** Üzerinde birlikte bulundurmayı etkinleştirirseniz, koruma veri kaynaklarında tek bir çoğaltma ve kurtarma noktası birimi için eşleyebilirsiniz. Birlikte bulundurma, tüm iş yükleri için desteklenmiyor.
   - **Otomatik olarak Büyüt:** Korumalı gruptaki veriler ilk ayırma boyutunu aşarsa bu ayarı etkinleştirirseniz, Azure Backup sunucusu, disk boyutunu yüzde 25 artırmayı dener.
   - **Depolama havuzu ayrıntıları:** Kalan disk boyutu ve toplam dahil olmak üzere depolama havuzunun durumunu gösterir.

     ![Disk ayırmayı İncele](./media/backup-azure-backup-server-vmware/review-disk-allocation.png)

1. İçinde **çoğaltma oluşturma yöntemini seçin** sayfasında, ilk yedekleme yapın ve ardından istediğiniz belirtin **sonraki**.
   - Varsayılan değer **ağ üzerinden otomatik olarak** ve **artık**.
   - Varsayılan kullanırsanız, yoğun olmayan bir zamanı belirtmenizi öneririz. Seçin **sonra** ve gün ve saati belirtin.
   - Büyük miktarlarda veri ya da daha az-en iyi ağ koşulları için Çıkarılabilir medya kullanarak çevrimdışı veri çoğaltmayı göz önünde bulundurun.

     ![Çoğaltma oluşturma yöntemini seçin](./media/backup-azure-backup-server-vmware/replica-creation.png)

1. İçinde **tutarlılık denetimi seçenekleri**, nasıl alacağınızı seçin ve tutarlılık denetimleri otomatik hale getirmek ne zaman. Ardından **İleri**'ye tıklayın.
      - Çoğaltma verileri tutarsız hale geldiğinde veya bir programa, tutarlılık denetimlerinin çalıştırabilirsiniz.
      - Otomatik tutarlılık denetimlerinin yapılandırmak istemiyorsanız, bir el ile denetim gerçekleştirebilirsiniz. Bunu yapmak için koruma grubuna sağ tıklayın > **tutarlılık denetimi gerçekleştir**.

1. İçinde **çevrimiçi koruma verilerini belirtin** sayfasında, VM'ler veya VM seçin, yedeklemek istediğiniz klasörleri. Tek tek üyeleri seçin veya tıklatın **Tümünü Seç** için tüm üyeleri seçin. Ardından **İleri**'ye tıklayın.

      ![Çevrimiçi koruma verilerini belirtin](./media/backup-azure-backup-server-vmware/select-data-to-protect.png)

1. Üzerinde **çevrimiçi yedekleme zamanlamasını belirtin** sayfasında, verileri yerel depolama alanından Azure'a yedeklemek istediğiniz sıklığı belirtin.

    - Bulut verileri için kurtarma noktaları zamanlamaya göre oluşturulur. Ardından **İleri**'ye tıklayın.
    - Kurtarma noktası oluşturulduktan sonra Azure kurtarma Hizmetleri kasasına aktarılır.

      ![Çevrimiçi Yedekleme zamanlamasını belirtin](./media/backup-azure-backup-server-vmware/online-backup-schedule.png)

1. Üzerinde **çevrimiçi saklama ilkesini belirtin** sayfasında, Azure Günlük/Haftalık/Aylık/yıllık yedeklerden oluşturulan kurtarma noktalarını saklamak istediğiniz süreyi belirtin. Ardından **sonraki**.

    - Azure'da verileri ne kadar süreyle kalmasını sağlayabilirsiniz için zaman sınırı yoktur.
    - Korumalı örnek başına birden fazla 9999 kurtarma noktası olamaz yalnızca sınırdır. Bu örnekte, korumalı örnek VMware sunucusudur.

      ![Çevrimiçi bekletme ilkesini belirtin](./media/backup-azure-backup-server-vmware/retention-policy.png)


1. Üzerinde **özeti** sayfasında, ayarları gözden geçirin ve ardından **Grup Oluştur**.

     ![Koruma grubu üyesi ve ayar özeti](./media/backup-azure-backup-server-vmware/protection-group-summary.png)

## <a name="vmware-vsphere-67"></a>VMWare vSphere 6.7

Yedekleme vSphere 6.7 için aşağıdakileri yapın:

- TLS 1.2 DPM sunucusunda etkinleştir
  >[!Note]
  >VMWare 6.7 başlayarak TLS iletişim protokolü olarak etkin.

- Kayıt defteri anahtarlarını aşağıdaki gibi ayarlayın:  

  Windows Kayıt Defteri Düzenleyicisi'ni sürüm 5.00

  [HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\\. NETFramework\v2.0.50727] "SystemDefaultTlsVersions" = dword: 00000001 "SchUseStrongCrypto" = dword: 00000001

  [HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\\. NETFramework\v4.0.30319] "SystemDefaultTlsVersions" = dword: 00000001 "SchUseStrongCrypto" = dword: 00000001

  [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\\. NETFramework\v2.0.50727] "SystemDefaultTlsVersions" = dword: 00000001 "SchUseStrongCrypto" = dword: 00000001

  [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\\. NETFramework\v4.0.30319] "SystemDefaultTlsVersions" = dword: 00000001 s "SchUseStrongCrypto" = dword: 00000001


## <a name="next-steps"></a>Sonraki adımlar

Yedeklemeleri ayarlama, sorunlarını gidermek için gözden [Azure Backup sunucusu için sorun giderme kılavuzu](./backup-azure-mabs-troubleshoot.md).
