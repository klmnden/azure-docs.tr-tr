---
title: Azure yedekleme sunucusu ile VMware sunucularını yedekleme
description: Azure veya disk için bir VMware vCenter/ESXi sunucularını yedekleme için Azure yedekleme sunucusu kullanın. Bu makalede adımı sağlar yedekleme (veya koruma) için adım adım yönerge = VMware iş yükleri.
services: backup
author: markgalioto
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 07/24/2017
ms.author: adigan
ms.openlocfilehash: 9cf3c9d5df11e19045cd47a41d7ab9ac93bdf700
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34605432"
---
# <a name="back-up-a-vmware-server-to-azure"></a>Azure'da VMware sunucuyu yedekleme

Bu makalede VMware server iş yüklerini korumak için Azure yedekleme sunucusunun nasıl yapılandırılacağı açıklanmaktadır. Bu makalede Azure yedekleme sunucusu zaten olduğunu varsayar. Azure yedekleme sunucusu sahip değilseniz, bkz: [Azure yedekleme sunucusu kullanarak iş yüklerini Yedeklemeye hazırlanma](backup-azure-microsoft-azure-backup.md).

Azure yedekleme sunucusu yedeklemek veya VMware vCenter Server sürüm 6.5, 6.0 ve 5.5 korunmasına yardımcı olma.


## <a name="create-a-secure-connection-to-the-vcenter-server"></a>VCenter sunucusu güvenli bağlantı oluşturun

Varsayılan olarak, Azure yedekleme sunucusu her bir HTTPS kanaldan vCenter Server ile iletişim kurar. Üzerinde güvenli iletişimi etkinleştirmek için Azure yedekleme sunucusuna VMware sertifika yetkilisi (CA) sertifikası yüklemeniz önerilir. Güvenli iletişim gerektirmez ve HTTPS gereksinimini devre dışı bırakmak tercih ediyorsanız, bkz: [devre dışı bırak güvenli iletişim protokolü](backup-azure-backup-server-vmware.md#disable-secure-communication-protocol). Azure yedekleme sunucusu ve vCenter sunucusu arasında güvenli bir bağlantı oluşturmak için Azure yedekleme sunucusu güvenilen sertifikayı içeri aktarın.

Genellikle, bir tarayıcı Azure yedekleme sunucusu makinede vCenter Server vSphere Web istemcisi aracılığıyla bağlanmak için kullanılır. Azure yedekleme sunucusu tarayıcı vCenter sunucusuna bağlanmak için kullandığınız ilk kez bağlantı güvenli değil. Aşağıdaki görüntüde güvenli olmayan bir bağlantı gösterir.

![VMware server güvenli bağlantısı örneği](./media/backup-azure-backup-server-vmware/unsecure-url.png)

Bu sorunu gidermek ve güvenli bir bağlantı oluşturmak için güvenilen kök CA sertifikaları indirin.

1. Azure yedekleme sunucusu üzerindeki tarayıcıda Web istemcisi vSphere için URL'yi girin. VSphere Web istemci oturum açma sayfası görüntülenir.

    ![vSphere Web istemcisi](./media/backup-azure-backup-server-vmware/vsphere-web-client.png)

    Yöneticiler ve geliştiriciler için bilgiler alt kısmındaki bulun **indirme güvenilen kök CA sertifikaları** bağlantı.

    ![Güvenilen kök CA sertifikaları indirmek için bağlantı](./media/backup-azure-backup-server-vmware/vmware-download-ca-cert-prompt.png)

  VSphere Web istemci oturum açma sayfası görmüyorsanız, tarayıcınızın proxy ayarlarını denetleyin.

2. Tıklatın **indirme güvenilen kök CA sertifikaları**.

    VCenter Server bir dosyayı yerel bilgisayarınıza yükler. Dosya adının adlı **karşıdan**. Tarayıcınıza bağlı olarak, dosyayı açma veya kaydetme verilip soran bir ileti alırsınız.

    ![sertifikaları indirildiğinde iletiyi yükle](./media/backup-azure-backup-server-vmware/download-certs.png)

3. Dosyayı Azure yedekleme sunucusu üzerindeki bir konuma kaydedin. Dosyayı kaydettiğinizde, .zip dosya adı uzantısına ekleyin.

    Dosya sertifikaları hakkında bilgi içeren bir .zip dosyasıdır. .Zip uzantısı ile ayıklama araçları kullanabilirsiniz.

4. Sağ **download.zip**ve ardından **tümünü Ayıkla** içerik ayıklanamadı.

    .Zip dosyasını içeriğinin adlı bir klasöre ayıklar **sertifikaları**. İki tür dosyası sertifikaları klasöründe yer alır. Kök sertifika dosyasını.0 ve.1 gibi numaralı bir dizi ile başlayan bir uzantı var.
    
    CRL dosyasını .r0 veya .r1 gibi bir dizi ile başlayan bir uzantı var. CRL dosyasını bir sertifika ile ilişkilidir.

    ![Yerel olarak ayıklanan dosyasını indirin ](./media/backup-azure-backup-server-vmware/extracted-files-in-certs-folder.png)

5. İçinde **sertifikaları** klasörü, kök sertifika dosyasını sağ tıklatın ve ardından **yeniden adlandırma**.

    ![Kök sertifikayı yeniden adlandırma ](./media/backup-azure-backup-server-vmware/rename-cert.png)

    Kök sertifikanın uzantısını .crt için değiştirin. Uzantı değiştirmek istediğinizden eminseniz tıklatın sorulduğunda **Evet** veya **Tamam**. Aksi takdirde, dosyanın hedeflenen işlevini değiştirin. Bir kök sertifikası temsil eden bir simge dosyası değişiklikleri simgesi.

6. Kök sertifikayı sağ tıklatın ve açılır menüsünden seçin **sertifikayı yükle**.

    **Sertifika Alma Sihirbazı'nı** iletişim kutusu görüntülenir.

7. İçinde **Sertifika Alma Sihirbazı'nı** iletişim kutusunda **yerel makine** sertifika ve ardından için hedef olarak **sonraki** devam etmek için.

    ![Sertifika Depolama Hedef seçenekleri ](./media/backup-azure-backup-server-vmware/certificate-import-wizard1.png)

    Bilgisayara değişikliklere izin vermek istiyorsanız, sorulursa **Evet** veya **Tamam**, tüm değişiklikler.

8. Üzerinde **sertifika deposu** sayfasında, **tüm sertifikaları aşağıdaki depolama alanına yerleştir**ve ardından **Gözat** sertifika deposu seçin.

    ![Bir özel depolama nokta sertifikaları Yerleştir](./media/backup-azure-backup-server-vmware/cert-import-wizard-local-store.png)

    **Sertifika deposu Seç** iletişim kutusu görüntülenir.

    ![Sertifika depolama klasör hiyerarşisi](./media/backup-azure-backup-server-vmware/cert-store.png)

9. Seçin **güvenilen kök sertifika yetkilileri** sertifikaları ve ardından için hedef klasör olarak **Tamam**.

    ![Sertifika hedef klasör](./media/backup-azure-backup-server-vmware/certificate-store-selected.png)

    **Güvenilen kök sertifika yetkilileri** klasörü sertifika deposu olarak onaylandı. **İleri**’ye tıklayın.

    ![Sertifika Deposu klasörü](./media/backup-azure-backup-server-vmware/certificate-import-wizard2.png)

10. Üzerinde **Sertifika Alma Sihirbazı Tamamlanıyor** sayfasında, sertifika istenen klasöründe olduğunu doğrulayın ve ardından **son**.

    ![Sertifika uygun klasöründe olduğunu doğrulayın](./media/backup-azure-backup-server-vmware/cert-wizard-final-screen.png)

    Bir iletişim kutusu görüntülenirse, başarılı sertifika içeri aktarma doğrulanır.

11. VCenter sunucusuna, bağlantının güvenli olduğunu onaylamak için oturum açın.

  Sertifika içeri aktarma işlemi başarılı olmaz ve güvenli bir bağlantı kurulamıyor, VMware vSphere üzerinde belgelere [sunucu sertifikaları alma](http://pubs.vmware.com/vsphere-60/index.jsp#com.vmware.wssdk.dsg.doc/sdk_sg_server_certificate_Appendixes.6.4.html).

  Kuruluşunuz içinde güvenli sınırları varsa ve HTTPS protokolünü etkinleştirmek istemiyorsanız, güvenli iletişim devre dışı bırakmak için aşağıdaki yordamı kullanın.

### <a name="disable-secure-communication-protocol"></a>Güvenli iletişim protokolü devre dışı bırak

Kuruluşunuz HTTPS protokolünü gerektirmez, HTTPS devre dışı bırakmak için aşağıdaki adımları kullanın. Varsayılan davranışı devre dışı bırakmak için varsayılan davranış yoksayar bir kayıt defteri anahtarı oluşturun.

1. Kopyalayın ve bir .txt dosyasına aşağıdaki metni yapıştırın.

  ```
  Windows Registry Editor Version 5.00
  [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft Data Protection Manager\VMWare]
  "IgnoreCertificateValidation"=dword:00000001
  ```

2. Dosyayı Azure yedekleme sunucusu bilgisayarınıza kaydedin. Dosya adı için DisableSecureAuthentication.reg kullanın.

3. Kayıt defteri girdisini etkinleştirmek için dosyaya çift tıklayın.


## <a name="create-a-role-and-user-account-on-the-vcenter-server"></a>VCenter Server bir rol ve kullanıcı hesabı oluşturun

VCenter Server, bir önceden tanımlanmış bir dizi ayrıcalık rolüdür. Bir vCenter sunucusu yöneticisi rolü oluşturur. İzinler atamak için kullanıcı hesapları ile bir rolü yönetici çiftleri. VCenter Server bilgisayarını yedeklemek için gerekli kullanıcı kimlik bilgileri oluşturmak için belirli ayrıcalıklara sahip bir rol oluşturmak ve ardından kullanıcı hesabı rolü ile ilişkilendir.

Azure yedekleme sunucusu vCenter Server ile kimlik doğrulaması için bir kullanıcı adı ve parola kullanır. Azure Backup sunucusu bu kimlik bilgileri tüm yedekleme işlemleri için kimlik doğrulaması olarak kullanır.

Bir vCenter sunucusu rolü ve kendi ayrıcalıklarını bir yedekleme Yöneticisi eklemek için:

1. VCenter sunucusu için ve vcenter Server'daki oturum **Gezgini** öğesine tıklayın **Yönetim**.

    ![VCenter Server gezinti panelinde yönetim seçeneği](./media/backup-azure-backup-server-vmware/vmware-navigator-panel.png)

2. İçinde **Yönetim** seçin **rolleri**ve ardından **rolleri** panel Ekle rol simgesini tıklatın (+ simgesi).

    ![Rol ekle](./media/backup-azure-backup-server-vmware/vmware-define-new-role.png)

    **Rol Oluştur** iletişim kutusu görüntülenir.

    ![Rolü oluşturma](./media/backup-azure-backup-server-vmware/vmware-define-new-role-priv.png)

3. İçinde **Rol Oluştur** iletişim kutusunda **rol adı** kutusuna *BackupAdminRole*. Rol adı ne olursa olsun, ister olabilir, ancak rolün amaçla tanınabilir olmalıdır.

4. VCenter'ün uygun sürümüne ayrıcalıklarını seçin ve ardından **Tamam**. Aşağıdaki tabloda vCenter 6.0 ve vCenter 5.5 için gerekli ayrıcalıklara tanımlar.

  Ayrıcalıkları seçtiğinizde, üst genişletin ve alt ayrıcalıkları görüntülemek için üst etiket yanındaki simgesine tıklayın. Sanal makinesi ayrıcalıkları seçmek için üst alt hiyerarşiye birkaç düzeyleri gitmeniz gerekir. Üst ayrıcalık içindeki tüm alt ayrıcalıklarını seçin gerek yoktur.

  ![Üst alt ayrıcalık hiyerarşisi](./media/backup-azure-backup-server-vmware/cert-add-privilege-expand.png)

  Tıklattıktan sonra **Tamam**, yeni rol rolleri panelindeki listesinde görünür.

|VCenter 6.0 için ayrıcalıkları| VCenter 5.5 için ayrıcalıkları|
|--------------------------|---------------------------|
|Datastore.AllocateSpace   | Datastore.AllocateSpace|
|Global.ManageCustomFields | Global.ManageCustomerFields|
|Global.SetCustomFields    |   |
|Host.Local.CreateVM       | Network.Assign |
|Network.Assign            |  |
|Resource.AssignVMToPool   |  |
|VirtualMachine.Config.AddNewDisk  | VirtualMachine.Config.AddNewDisk   |
|VirtualMachine.Config.AdvanceConfig| VirtualMachine.Config.AdvancedConfig|
|VirtualMachine.Config.ChangeTracking| VirtualMachine.Config.ChangeTracking |
|VirtualMachine.Config.HostUSBDevice||
|VirtualMachine.Config.QueryUnownedFiles|    |
|VirtualMachine.Config.SwapPlacement| VirtualMachine.Config.SwapPlacement |
|VirtualMachine.Interact.PowerOff| VirtualMachine.Interact.PowerOff |
|VirtualMachine.Inventory.Create| VirtualMachine.Inventory.Create |
|VirtualMachine.Provisioning.DiskRandomAccess| |
|VirtualMachine.Provisioning.DiskRandomRead|VirtualMachine.Provisioning.DiskRandomRead |
|VirtualMachine.State.CreateSnapshot| VirtualMachine.State.CreateSnapshot|
|VirtualMachine.State.RemoveSnapshot|VirtualMachine.State.RemoveSnapshot |
</br>



## <a name="create-a-vcenter-server-user-account-and-permissions"></a>Bir vCenter Server kullanıcı hesabı ve izinler oluşturma

Rol ayrıcalıklarıyla ayarladıktan sonra bir kullanıcı hesabı oluşturun. Kullanıcı hesabı adı ve parola kimlik doğrulaması için kullanılan kimlik bilgilerini sağlar sahiptir.

1. VCenter Server ' bir kullanıcı hesabı oluşturmak için **Gezgini** öğesine tıklayın **kullanıcılar ve gruplar**.

    ![Kullanıcılar ve gruplar seçeneği](./media/backup-azure-backup-server-vmware/vmware-userandgroup-panel.png)

    **VCenter kullanıcılar ve gruplar** panelinde görüntülenir.

    ![vCenter kullanıcılar ve gruplar paneli](./media/backup-azure-backup-server-vmware/usersandgroups.png)

2. İçinde **vCenter kullanıcılar ve gruplar** paneli, select **kullanıcılar** sekmesini ve ardından Ekle kullanıcılar simgesine tıklayın (+ simgesi).

    **Yeni kullanıcı** iletişim kutusu görüntülenir.

3. İçinde **yeni kullanıcı** iletişim kutusunda, kullanıcının bilgileri ekleyin ve ardından **Tamam**. Bu yordamda, BackupAdmin kullanıcı adıdır.

    ![Yeni kullanıcı iletişim kutusu](./media/backup-azure-backup-server-vmware/vmware-new-user-account.png)

    Yeni kullanıcı hesabı listede görüntülenir.

4. Kullanıcı hesabı olarak rolüyle ilişkilendirmek için **Gezgini** öğesine tıklayın **genel izinleri**. İçinde **genel izinleri** paneli, select **Yönet** sekmesini ve ardından Ekle simgesini tıklatın (+ simgesi).

    ![Genel izinleri paneli](./media/backup-azure-backup-server-vmware/vmware-add-new-perms.png)

    **Genel izinleri kök - ekleme izni** iletişim kutusu görüntülenir.

5. İçinde **genel izin kök - ekleme izni** iletişim kutusu, tıklatın **Ekle** kullanıcıyı veya grubu seçin.

    ![Kullanıcı veya grup seçin](./media/backup-azure-backup-server-vmware/vmware-add-new-global-perm.png)

    **Kullanıcıları/Grupları Seç** iletişim kutusu görüntülenir.

6. İçinde **kullanıcıları/Grupları Seç** iletişim kutusunda, seçin **BackupAdmin** ve ardından **Ekle**.

    İçinde **kullanıcılar**, *etkialanı\kullanıcıadı* biçimi, kullanıcı hesabı için kullanılır. Farklı bir etki alanı kullanmak isterseniz, buradan seçin **etki alanı** listesi.

    ![BackupAdmin kullanıcı ekleme](./media/backup-azure-backup-server-vmware/vmware-assign-account-to-role.png)

    Tıklatın **Tamam** Seçili kullanıcılar için eklenecek **ekleme izni** iletişim kutusu.

7. Kullanıcı tanımladınız, kullanıcı rolüne atayın. İçinde **atanan rolü**, aşağı açılan listesinden **BackupAdminRole**ve ardından **Tamam**.

    ![Kullanıcı rolüne atayın](./media/backup-azure-backup-server-vmware/vmware-choose-role.png)

  Üzerinde **Yönet** sekmesinde **genel izinleri** paneli, yeni kullanıcı hesabı ve ilişkili rol listede görünür.


## <a name="establish-vcenter-server-credentials-on-azure-backup-server"></a>VCenter sunucusu kimlik bilgilerini Azure yedekleme sunucusu kurma

VMware server için Azure yedekleme sunucusu eklemeden önce yükleme [Azure yedekleme sunucusu için Güncelleştirme 1](https://support.microsoft.com/help/3175529/update-1-for-microsoft-azure-backup-server).

1. Azure Backup'ı açmak için Azure yedekleme sunucusu masaüstünde simgesini çift tıklatın.

    ![Azure yedekleme sunucusu simgesi](./media/backup-azure-backup-server-vmware/mabs-icon.png)

    Masaüstünde simge bulamazsanız, Azure yedekleme sunucusu yüklü uygulamalar listesinden açın. Azure yedekleme sunucusu uygulama adı, Microsoft Azure yedekleme denir.

2. Azure yedekleme sunucusu konsolunda **Yönetim**, tıklatın **üretim sunucuları**ve araç şeridinde, ardından **yönetmek VMware**.

    ![Azure yedekleme sunucusu konsol](./media/backup-azure-backup-server-vmware/add-vmware-credentials.png)

    **Kimlik bilgilerini Yönet** iletişim kutusu görüntülenir.

    ![Azure yedekleme sunucusu kimlik bilgilerini Yönet iletişim kutusu](./media/backup-azure-backup-server-vmware/mabs-manage-credentials-dialog.png)

3. İçinde **kimlik bilgilerini Yönet** iletişim kutusu, tıklatın **Ekle** açmak için **kimlik bilgileri Ekle** iletişim kutusu.

4. İçinde **kimlik bilgileri Ekle** iletişim kutusunda, bir ad ve yeni bir kimlik bilgisi için bir açıklama girin. Ardından kullanıcı adı ve parola belirtin. Ad *Contoso Vcenter kimlik bilgisi* sonraki yordamda kimlik bilgilerini tanımlamak için kullanılır. Aynı kullanıcı adı ve vCenter sunucusu için kullanılan parolayı kullanın. VCenter Server ve Azure yedekleme sunucusu aynı etki alanında içinde değilse **kullanıcı adı**, etki alanını belirtin.

    ![Azure yedekleme sunucusu kimlik bilgileri Ekle iletişim kutusu](./media/backup-azure-backup-server-vmware/mabs-add-credential-dialog2.png)

    Tıklatın **Ekle** yeni kimlik bilgilerini Azure yedekleme sunucusu eklemek için. Yeni bir kimlik bilgisi listesinde görünür **kimlik bilgilerini Yönet** iletişim kutusu.
    
    ![Azure yedekleme sunucusu kimlik bilgilerini Yönet iletişim kutusu](./media/backup-azure-backup-server-vmware/new-list-of-mabs-creds.png)

5. Kapatmak için **kimlik bilgilerini Yönet** iletişim kutusu, tıklatın **X** sağ üst köşedeki.


## <a name="add-the-vcenter-server-to-azure-backup-server"></a>Azure yedekleme sunucusu vCenter Server ekleme

Üretim Sunucusu Ekleme Sihirbazı'nı Azure yedekleme sunucusuna vCenter sunucusu eklemek için kullanılır.

Üretim Sunucusu Ekleme Sihirbazı'nı açmak için aşağıdaki yordamı izleyin:

1. Azure yedekleme sunucusu konsolunda **Yönetim**, tıklatın **üretim sunucuları**ve ardından **Ekle**.

    ![Açık Üretim Sunucusu Ekleme Sihirbazı](./media/backup-azure-backup-server-vmware/add-vcenter-to-mabs.png)

    **Üretim Sunucusu Ekleme Sihirbazı'nı** iletişim kutusu görüntülenir.

    ![Üretim Sunucusu Ekleme Sihirbazı](./media/backup-azure-backup-server-vmware/production-server-add-wizard.png)

2. Üzerinde **üretim sunucusu seçin türü** sayfasında **VMware sunucuları**ve ardından **sonraki**.

3. İçinde **sunucu adı/IP adresi**, tam etki alanı adı (FQDN) veya VMware sunucusunun IP adresini belirtin. Tüm ESXi sunucuları aynı vCenter tarafından yönetiliyorsa, vCenter adı kullanabilirsiniz.

    ![VMware server FQDN veya IP adresi belirtin](./media/backup-azure-backup-server-vmware/add-vmware-server-provide-server-name.png)

4. İçinde **SSL bağlantı noktası**, VMware sunucusu ile iletişim kurmak için kullanılan bağlantı noktasını girin. Farklı bir bağlantı gerekli olduğunu bilmiyorsanız, varsayılan bağlantı noktası 443 numaralı bağlantı noktasını kullanın.

5. İçinde **kimlik bilgisi belirtin**, daha önce oluşturduğunuz kimlik bilgisi seçin.

    ![Kimlik bilgisi belirtin](./media/backup-azure-backup-server-vmware/identify-creds.png)

6. Tıklatın **Ekle** VMware server listesine eklemek için **eklenen VMware sunucuları**ve ardından **sonraki** Sihirbazı sonraki sayfaya gitmek için.

    ![VMWare server ve kimlik bilgisi Ekle](./media/backup-azure-backup-server-vmware/add-vmware-server-credentials.png)

7. İçinde **Özet** sayfasında, **Ekle** belirtilen VMware sunucusu Azure yedekleme sunucusu eklemek için.

    ![Azure yedekleme sunucusuna VMware sunucusu ekleyin](./media/backup-azure-backup-server-vmware/tasks-screen.png)

  VMware server yedekleme aracısız yedeğidir ve yeni sunucunun hemen eklenir. **Son** sayfası sonuçları gösterir.

  ![Son sayfası](./media/backup-azure-backup-server-vmware/summary-screen.png)

  VCenter Server birden çok örneğini Azure yedekleme sunucusu eklemek için bu bölümün önceki adımları yineleyin.

VCenter sunucusu Azure yedekleme sunucusu ekledikten sonra sonraki adım bir koruma grubu oluşturmaktır. Koruma grubunun kısa veya uzun vadeli bekletme için çeşitli ayrıntılarını belirtir, ve burada tanımlayın ve yedekleme ilkesini uygulayın. Yedekleme İlkesi yedeklemeleri olduğunda ve ne yedeklenir zamanlamadır.


## <a name="configure-a-protection-group"></a>Bir koruma grubunu yapılandırın

System Center Data Protection Manager veya Azure yedekleme sunucusu önce kullanmadıysanız bkz [planlama disk yedeklemeleri](https://technet.microsoft.com/library/hh758026.aspx) donanım ortamınızı hazırlamak için. Uygun depolama alanına sahip denetledikten sonra VMware sanal makineleri eklemek için yeni koruma grubu oluşturma Sihirbazı'nı kullanın.

1. Azure yedekleme sunucusu konsolunda **koruma**, araç şeridinde tıklatıp **yeni** yeni koruma grubu oluşturma Sihirbazı'nı açmak için.

    ![Yeni koruma grubu oluşturma Sihirbazı'nı açın](./media/backup-azure-backup-server-vmware/open-protection-wizard.png)

    **Yeni koruma grubu oluşturma** Sihirbazı iletişim kutusu görüntülenir.

    ![Yeni koruma grubu oluşturma Sihirbazı iletişim kutusu](./media/backup-azure-backup-server-vmware/protection-wizard.png)

    Tıklatın **sonraki** ilerletmek için **koruma grubu türünü seçin** sayfası.

2. Üzerinde **seçin koruma grubu türünü** sayfasında **sunucuları** ve ardından **sonraki**. **Grup üyelerini seçin** sayfası görüntülenir.

3. Üzerinde **grup üyelerini seçin** sayfasında, kullanılabilir üyeler ve seçilen üyeleri görünür. Koruma ve ardından istediğiniz üyeleri seçin **sonraki**.

    ![Grup üyelerini seçin](./media/backup-azure-backup-server-vmware/server-add-selected-members.png)

    Diğer klasörlere veya sanal makineleri içeren bir klasör seçerseniz bir üye seçin, bu klasör ve VM'ler da seçilir. VM'ler üst klasörde ve alt klasörlerin ekleme klasör düzeyinde koruma adı verilir. Bir klasör veya VM kaldırmak için onay kutusunu temizleyin.

    Azure'a zaten korumalı bir VM veya bir VM'yi içeren bir klasör varsa, bu VM yeniden seçemezsiniz. Bir VM için Azure korunduktan sonra diğer bir deyişle, onu yeniden yinelenen kurtarma noktaları için bir VM oluşturulmasını engeller korunamaz. Hangi Azure yedekleme sunucusu örneği bir üye zaten korur görmek istiyorsanız, koruma sunucunun adını görmek için üye gelin.

4. Üzerinde **veri koruma yöntemini seçin** sayfasında, koruma grubu için bir ad girin. Kısa vadeli koruma (diske) ve çevrimiçi koruma seçilir. (Azure için) çevrimiçi korumayı kullanmak istiyorsanız, diske kısa süreli koruma kullanmanız gerekir. Tıklatın **sonraki** kısa vadeli koruma aralığının devam etmek için.

    ![Veri koruma yöntemini seçin](./media/backup-azure-backup-server-vmware/name-protection-group.png)

5. Üzerinde **kısa vadeli hedefleri belirtin** sayfası için **bekletme aralığı**, Kurtarma noktalarını korumak istediğiniz gün sayısını belirtin *diske depolanmış*. Saati değiştirmek ve gün bir kurtarma noktası alınma tıklatın istiyorsanız **Değiştir**. Kısa vadeli kurtarma noktaları tam yedeklemeler edilir. Artımlı yedeklemeler değiller. Kısa vadeli hedefleri ile memnun kaldığınızda, tıklatın **sonraki**.

    ![Kısa vadeli hedefleri belirtin](./media/backup-azure-backup-server-vmware/short-term-goals.png)

6. Üzerinde **Disk ayırmayı gözden** sayfasında inceleyin ve gerekirse, VM'ler için disk alanını değiştirin. Belirtilen saklama aralığı önerdiği disk ayırmalarını dayalı **kısa vadeli hedefleri belirtin** sayfasında, iş yükü türüne ve (3. adımda tanımlanan) korumalı verilerin boyutu.  

  - **Veri boyutu:** koruma grubundaki verilerin boyutu.
  - **Disk alanı:** önerilen koruma grubu için disk alanı miktarı. Bu ayarı değiştirmek istiyorsanız, her veri kaynağı büyür tahmin ettiğiniz miktardan biraz daha büyük toplam alan ayırın.
  - **Verileri birlikte bulundurma:** üzerinde birlikte bulundurmaya kapatırsanız, birimi koruma kaynakları tek bir çoğaltma ve kurtarma için eşlenebilir birden fazla veri noktası. Birlikte bulundurma, tüm iş yükleri için desteklenmez.
  - **Otomatik olarak Büyüt:** korumalı gruptaki veriler ilk ayırma boyutunu aşarsa bu ayarı üzerinde kapatma, System Center Data Protection Manager 25 oranında disk boyutunu artırmak çalışır.
  - **Depolama havuzu ayrıntıları:** kalan disk boyutu ve toplam dahil olmak üzere depolama havuzunun durumunu gösterir.

    ![Disk ayırmayı İncele](./media/backup-azure-backup-server-vmware/review-disk-allocation.png)

    Alanı ayırma ile memnun kaldığınızda, tıklatın **sonraki**.

7. Üzerinde **çoğaltma oluşturma yöntemini seçin** sayfasında, nasıl ilk kopyalama ya da Azure yedekleme sunucusu korunan verilerin kopyasını oluşturmak istediğinizi belirtin.

    Varsayılan değer **otomatik olarak ağ üzerinden** ve **şimdi**. Varsayılan kullanırsanız, yoğun olmayan bir zamanı belirtin öneririz. Seçin **sonra** ve gününü ve saatini belirtin.

    Büyük miktarlarda veri veya en iyi durumdan daha az ağ koşulları için Çıkarılabilir medya kullanarak çevrimdışı veri çoğaltmayı düşünebilirsiniz.

    Seçimlerinizi yaptıktan sonra tıklatın **sonraki**.

    ![Çoğaltma oluşturma yöntemini seçin](./media/backup-azure-backup-server-vmware/replica-creation.png)

8. Üzerinde **tutarlılık denetimi seçenekleri** sayfası, select nasıl ve ne zaman tutarlılık denetimleri otomatik hale getirmek. Çoğaltma verileri tutarsız hale geldiğinde veya belirlenmiş bir zamanlamaya üzerinde tutarlılık denetimleri çalıştırabilirsiniz.

    Otomatik tutarlılık denetimleri yapılandırmak istemiyorsanız, el ile denetim çalıştırabilirsiniz. Azure yedekleme sunucusu konsolunun koruma bölmesinde koruma grubunu sağ tıklatın ve ardından **tutarlılık denetimi gerçekleştir**.

    Tıklatın **sonraki** sonraki sayfaya geçmek için.

9. Üzerinde **çevrimiçi koruma verilerini belirtin** sayfasında, korumak istediğiniz bir veya daha fazla veri kaynağını seçin. Tek tek üyeleri seçin veya tıklatın **Tümünü Seç** tüm üyeleri seçin. Üyeleri seçtikten sonra tıklatın **sonraki**.

    ![Çevrimiçi koruma verilerini belirtin](./media/backup-azure-backup-server-vmware/select-data-to-protect.png)

10. Üzerinde **çevrimiçi yedekleme zamanlamasını belirtin** sayfasında, diskten kurtarma noktaları oluşturmak üzere zamanlama belirtin. Kurtarma noktası oluşturulduktan sonra Azure kurtarma Hizmetleri kasasına aktarılır. İle çevrimiçi yedekleme zamanlamasını memnun kaldığınızda, tıklatın **sonraki**.

    ![Çevrimiçi Yedekleme zamanlamasını belirtin](./media/backup-azure-backup-server-vmware/online-backup-schedule.png)

11. Üzerinde **çevrimiçi bekletme ilkesini belirtin** sayfasında, yedekleme verilerini azure'da korumak istediğiniz ne kadar süreyle belirtin. İlke tanımlandıktan sonra tıklatın **sonraki**.

    ![Çevrimiçi bekletme ilkesini belirtin](./media/backup-azure-backup-server-vmware/retention-policy.png)

    Ne kadar veri Azure'da tutabilirsiniz için süre sınırı yoktur. Kurtarma noktası verilerini Azure'da depoladığınızda, yalnızca korumalı örneği başına birden fazla 9999 kurtarma noktaları olamaz sınırıdır. Bu örnekte, korumalı VMware server örneğidir.

12. Üzerinde **Özet** sayfasında, ayarları ve koruma grubu üyeleri için ayrıntıları gözden geçirin ve ardından **Grup Oluştur**.

    ![Koruma grubu üyesi ve ayar özeti](./media/backup-azure-backup-server-vmware/protection-group-summary.png)

## <a name="next-steps"></a>Sonraki adımlar
VMware iş yüklerini korumak için Azure yedekleme sunucusu kullanıyorsanız, korunmasına yardımcı olmak için Azure yedekleme sunucusu kullanarak ilgilenebilirsiniz bir [Microsoft Exchange server](./backup-azure-exchange-mabs.md), [Microsoft SharePoint grubu](./backup-azure-backup-sharepoint-mabs.md), veya bir [SQL Server veritabanı](./backup-azure-sql-mabs.md).

Koruma grubu yapılandırma veya işler, yedekleme aracısını kaydetmeden sorunları hakkında daha fazla bilgi için bkz [sorun giderme Azure yedekleme sunucusu](./backup-azure-mabs-troubleshoot.md).
