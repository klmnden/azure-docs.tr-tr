---
title: Azure Backup sunucusu ile VMware sunucularını yedekleme
description: Azure veya disk bir VMware vCenter/ESXi sunucularını yedeklemek için Azure Backup sunucusu kullanma. Bu makalede adım sağlayan adım adım yönergeler için Yedekleme (veya korumak) = VMware iş yüklerinizi.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 07/24/2017
ms.author: adigan
ms.openlocfilehash: e39e5d12610164ca4a1372830cf25ea203fd382c
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52968843"
---
# <a name="back-up-a-vmware-server-to-azure"></a>Bir VMware sunucusunu yedeklemek'ı Azure'da yedekleme

Bu makalede, VMware sunucusu iş yüklerini korumak için Azure Backup sunucusu yapılandırma açıklanmaktadır. Bu makalede, yüklü Azure Backup sunucusu zaten sahip olduğunuz varsayılır. Azure Backup sunucusu yüklü yoksa bkz [kullanarak Azure Backup sunucusu iş yüklerini yedeklemeye hazırlama](backup-azure-microsoft-azure-backup.md).

Azure Backup sunucusu yedekleme veya VMware vCenter Server sürüm 6.5, 6.0 ve 5.5 korunmasına yardımcı olma.


## <a name="create-a-secure-connection-to-the-vcenter-server"></a>VCenter Server için güvenli bir bağlantı oluşturun

Varsayılan olarak, Azure Backup sunucusu, her bir HTTPS kanalı aracılığıyla vCenter Server ile iletişim kurar. Güvenli iletişimi etkinleştirmek için Azure Backup sunucusu üzerinde VMware sertifika yetkilisi (CA) sertifikası yüklemeniz önerilir. Güvenli iletişim gerektirmez ve HTTPS gereksinimini devre dışı bırakmak tercih ettiğiniz bkz [devre dışı bırakma güvenli iletişim protokolü](backup-azure-backup-server-vmware.md#disable-secure-communication-protocol). Azure Backup sunucusu ile vCenter Server arasında güvenli bir bağlantı oluşturmak için Azure Backup sunucusu güvenilen sertifikayı içeri aktarın.

Genellikle, bir tarayıcı için Azure Backup sunucusu makinesinde vSphere Web istemcisi üzerinden vCenter Server'a bağlanmak için kullanın. Azure Backup sunucusu tarayıcı vCenter sunucusuna bağlanmak için kullandığınız ilk kez bağlantı güvenli değil. Aşağıdaki resimde, güvenli olmayan bir bağlantı gösterilir.

![VMware sunucusuna güvenli bağlantı örneği](./media/backup-azure-backup-server-vmware/unsecure-url.png)

Bu sorunu düzeltmek ve güvenli bir bağlantı oluşturmak için güvenilen kök CA sertifikaları'nı indirin.

1. Azure Backup sunucusu üzerinde bir tarayıcıda, vSphere Web istemcisi için URL'yi girin. VSphere Web istemcisi oturum açma sayfası görüntülenir.

    ![vSphere Web istemcisi](./media/backup-azure-backup-server-vmware/vsphere-web-client.png)

    Yöneticiler ve geliştiriciler için bilgi sonunda bulun **indirme güvenilen kök CA sertifikaları** bağlantı.

    ![Güvenilen kök CA sertifikalarını indirmek için bağlantı](./media/backup-azure-backup-server-vmware/vmware-download-ca-cert-prompt.png)

  VSphere Web istemcisi oturum açma sayfasına görmüyorsanız, tarayıcınızın proxy ayarlarını kontrol edin.

2. Tıklayın **indirme güvenilen kök CA sertifikaları**.

    VCenter Server bir dosyayı yerel bilgisayarınıza indirir. Dosya adının adlı **indirme**. Tarayıcınıza bağlı olarak mı dosyasını açmanız veya kaydetmeniz soran bir ileti alırsınız.

    ![sertifikaları indirildiğinde ileti indirin](./media/backup-azure-backup-server-vmware/download-certs.png)

3. Dosyayı, Azure Backup sunucusu üzerinde bir konuma kaydedin. Dosyayı kaydettiğinizde, .zip dosya adı uzantısını ekleyin.

    Dosya sertifikaları hakkında bilgi içeren bir .zip dosyasıdır. .Zip uzantılı Ayıklama Araçları'nı kullanabilirsiniz.

4. Sağ **download.zip**ve ardından **tümünü Ayıkla** içerik ayıklanamadı.

    .Zip dosyasının içeriğini adlı bir klasöre ayıklar **sertifikaları**. İki tür dosyaların sertifikaları klasöründe yer alır. Kök sertifika dosyasını.0 ve.1 gibi numaralı bir dizi ile başlayan uzantısına sahiptir.

    CRL dosyasını .r0 veya .r1 gibi bir dizi ile başlayan uzantısına sahiptir. CRL dosyasını bir sertifika ile ilişkilidir.

    ![Yerel olarak ayıklanan dosyasını indirin ](./media/backup-azure-backup-server-vmware/extracted-files-in-certs-folder.png)

5. İçinde **sertifikaları** klasörü, kök sertifika dosyasını sağ tıklayın ve ardından **Yeniden Adlandır**.

    ![Kök sertifikayı yeniden adlandır ](./media/backup-azure-backup-server-vmware/rename-cert.png)

    Kök sertifikanın uzantısını .crt için değiştirin. Uzantı değiştirmek istediğinizden eminseniz tıklayın sorulduğunda **Evet** veya **Tamam**. Aksi takdirde, dosyanın hedeflenen işlevini değiştirin. Dosya değişiklikleri bir kök sertifikayı temsil eden bir simge için simge.

6. Kök sertifikaya sağ tıklayın ve açılan menüden **sertifikayı yükle**.

    **Sertifika Alma Sihirbazı** iletişim kutusu görüntülenir.

7. İçinde **Sertifika Alma Sihirbazı** iletişim kutusunda **yerel makine** sertifika ve ardından hedefi olarak **sonraki** devam etmek için.

    ![Sertifika Depolama Hedef seçenekleri ](./media/backup-azure-backup-server-vmware/certificate-import-wizard1.png)

    Bilgisayarda değişiklikler izin vermek istiyorsanız, tıklayın istenirse **Evet** veya **Tamam**, tüm değişiklikler.

8. Üzerinde **sertifika Store** sayfasında **tüm sertifikaları aşağıdaki depolama alanına yerleştir**ve ardından **Gözat** sertifika deposunu seçme.

    ![Sertifikaları bir noktada belirli depolama alanına yerleştir](./media/backup-azure-backup-server-vmware/cert-import-wizard-local-store.png)

    **Seçin sertifika Store** iletişim kutusu görüntülenir.

    ![Sertifika depolama klasör hiyerarşisi](./media/backup-azure-backup-server-vmware/cert-store.png)

9. Seçin **güvenilen kök sertifika yetkilileri** sertifikaları ve ardından için hedef klasör olarak **Tamam**.

    ![Sertifika hedef klasör](./media/backup-azure-backup-server-vmware/certificate-store-selected.png)

    **Güvenilen kök sertifika yetkilileri** klasör sertifika deposunu onaylandı. **İleri**’ye tıklayın.

    ![Sertifika Deposu klasörü](./media/backup-azure-backup-server-vmware/certificate-import-wizard2.png)

10. Üzerinde **sertifika İçeri Aktarma Sihirbazı Tamamlanıyor** sayfasında, sertifika istenen klasöründe olduğunu doğrulayın ve ardından **son**.

    ![Sertifika uygun klasöründe olduğunu doğrulayın](./media/backup-azure-backup-server-vmware/cert-wizard-final-screen.png)

    Bir iletişim kutusu görüntülenirse, başarılı sertifika içeri aktarma doğrulanır.

11. VCenter Server'a bağlantınızı güvenli olduğundan emin olmak için oturum açın.

  Sertifika içeri aktarma işlemi başarılı olmaz ve güvenli bir bağlantı kurulamıyor, VMware vSphere üzerinde belgelerine [sunucu sertifikaları alma](http://pubs.vmware.com/vsphere-60/index.jsp#com.vmware.wssdk.dsg.doc/sdk_sg_server_certificate_Appendixes.6.4.html).

  Kuruluşunuzun içinde güvenli sınırları olan ve HTTPS protokolünü etkinleştirmek istemiyorsanız, güvenli iletişim devre dışı bırakmak için aşağıdaki yordamı kullanın.

### <a name="disable-secure-communication-protocol"></a>Güvenli iletişim protokolünü devre dışı bırak

Kuruluşunuz, HTTPS protokolünü gerektirmiyorsa, HTTPS devre dışı bırakmak için aşağıdaki adımları kullanın. Varsayılan davranışı devre dışı bırakmak için varsayılan davranışı yoksayar bir kayıt defteri anahtarı oluşturun.

1. Kopyalayıp bir .txt dosyasına aşağıdaki metni yapıştırın.

  ```
  Windows Registry Editor Version 5.00
  [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft Data Protection Manager\VMWare]
  "IgnoreCertificateValidation"=dword:00000001
  ```

2. Dosyayı, Azure Backup sunucusu bilgisayarınıza kaydedin. Dosya adı için DisableSecureAuthentication.reg kullanın.

3. Kayıt defteri girişini etkinleştirmek için dosyaya çift tıklayın.


## <a name="create-a-role-and-user-account-on-the-vcenter-server"></a>VCenter Server'da bir rol ve kullanıcı hesabı oluşturun

VCenter Server'da, bir önceden tanımlanmış bir dizi ayrıcalık rolüdür. Bir vCenter Server Yönetici rolü oluşturur. İzinler atamak için kullanıcı hesaplarını bir rolü olan yönetici çiftlerini. VCenter Server bilgisayarını yedeklemek için gerekli kullanıcı kimlik bilgilerini oluşturmak için belirli ayrıcalıklarına sahip bir rol oluşturun ve sonra kullanıcı hesabını rolü ile ilişkilendir.

Azure Backup sunucusu, vCenter Server ile kimlik doğrulaması için bir kullanıcı adı ve parola kullanır. Azure Backup sunucusu, kimlik doğrulaması olarak tüm yedekleme işlemleri için bu kimlik bilgilerini kullanır.

Bir vCenter sunucusu rolü ve kendi ayrıcalıklarını için yedek bir yöneticiniz eklemek için:

1. Vcenter Server ve vcenter Server'ı oturum **Gezgin** panelinde, tıklayın **Yönetim**.

    ![VCenter Server gezinti panelinde yönetim seçeneği](./media/backup-azure-backup-server-vmware/vmware-navigator-panel.png)

2. İçinde **Yönetim** seçin **rolleri**ve ardından **rolleri** panel Ekle rol simgesine tıklayın (+ simgesi).

    ![Rol ekle](./media/backup-azure-backup-server-vmware/vmware-define-new-role.png)

    **Rol Oluştur** iletişim kutusu görüntülenir.

    ![Rol oluşturma](./media/backup-azure-backup-server-vmware/vmware-define-new-role-priv.png)

3. İçinde **Rol Oluştur** iletişim kutusundaki **rol adı** kutusuna *BackupAdminRole*. Rol adı dilediğiniz olabilir ancak rolün amaçla tanınabilir olmalıdır.

4. VCenter'ün uygun sürümüne ayrıcalıklarını seçin ve ardından **Tamam**. Aşağıdaki tabloda, vCenter 6.0 ve vCenter 5.5 için gerekli ayrıcalıklara tanımlar.

  Ayrıcalıkları seçtiğinizde, üst genişletmek ve alt ayrıcalıkları görüntülemek için üst etiket yanındaki simgeye tıklayın. VirtualMachine ayrıcalıkları seçmek için üst alt hiyerarşide birden fazla düzeyi gitmeniz gerekiyor. Üst ayrıcalık içindeki tüm alt ayrıcalıklarını seçin gerek yoktur.

  ![Üst alt ayrıcalık hiyerarşisi](./media/backup-azure-backup-server-vmware/cert-add-privilege-expand.png)

  Tıkladıktan sonra **Tamam**, yeni rol rolleri panosunda listesinde görünür.

|VCenter 6.0 ve 6.5 ayrıcalıkları| VCenter 5.5 ayrıcalıkları|
|----------------------------------|---------------------------|
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

Rol ayrıcalıklarını ile ayarlandıktan sonra bir kullanıcı hesabı oluşturun. Kullanıcı hesabı adı ve parola kimlik doğrulaması için kullanılan kimlik bilgilerini sağlayan sahiptir.

1. VCenter Server'da bir kullanıcı hesabı oluşturmak için **Gezgin** panelinde, tıklayın **kullanıcılar ve gruplar**.

    ![Kullanıcılar ve gruplar seçeneği](./media/backup-azure-backup-server-vmware/vmware-userandgroup-panel.png)

    **VCenter kullanıcılar ve gruplar** paneli görüntülenir.

    ![vCenter kullanıcılar ve gruplar paneli](./media/backup-azure-backup-server-vmware/usersandgroups.png)

2. İçinde **vCenter kullanıcılar ve gruplar** paneli, select **kullanıcılar** sekmesini ve sonra kullanıcılar ekleme simgesini (+ simgesi).

    **Yeni kullanıcı** iletişim kutusu görüntülenir.

3. İçinde **yeni kullanıcı** iletişim kutusu, kullanıcının bilgileri ekleyin ve ardından **Tamam**. Bu yordamda, BackupAdmin kullanıcı adıdır.

    ![Yeni kullanıcı iletişim kutusu](./media/backup-azure-backup-server-vmware/vmware-new-user-account.png)

    Yeni kullanıcı hesabının listesinde görünür.

4. Kullanıcı hesabı içinde rolüyle ilişkilendirmek için **Gezgin** panelinde, tıklayın **genel izinleri**. İçinde **genel izinleri** paneli, select **Yönet** sekmesini ve sonra Ekle simgesini (+ simgesi).

    ![Genel izinler paneli](./media/backup-azure-backup-server-vmware/vmware-add-new-perms.png)

    **Genel izinleri Root - ekleme izni** iletişim kutusu görüntülenir.

5. İçinde **genel izin Root - ekleme izni** iletişim kutusu, tıklayın **Ekle** kullanıcı veya grup seçmek için.

    ![Kullanıcı veya grup seçin](./media/backup-azure-backup-server-vmware/vmware-add-new-global-perm.png)

    **Kullanıcıları/Grupları Seç** iletişim kutusu görüntülenir.

6. İçinde **kullanıcıları/Grupları Seç** iletişim kutusunda **BackupAdmin** ve ardından **Ekle**.

    İçinde **kullanıcılar**, *etki alanı\kullanıcı adı* biçimi, kullanıcı hesabı için kullanılır. Farklı bir etki alanı kullanmak istiyorsanız, buradan seçin **etki alanı** listesi.

    ![BackupAdmin kullanıcı ekleme](./media/backup-azure-backup-server-vmware/vmware-assign-account-to-role.png)

    Tıklayın **Tamam** Seçili kullanıcılar için eklenecek **ekleme izni** iletişim kutusu.

7. Kullanıcı tanımladığınıza göre kullanıcı rolüne atayın. İçinde **atanmış rol**, aşağı açılan listesinden **BackupAdminRole**ve ardından **Tamam**.

    ![Kullanıcı rolüne atayın.](./media/backup-azure-backup-server-vmware/vmware-choose-role.png)

  Üzerinde **Yönet** sekmesinde **genel izinleri** paneli, yeni kullanıcı hesabı ve ilişkili rol listede görünür.


## <a name="establish-vcenter-server-credentials-on-azure-backup-server"></a>Azure Backup sunucusu vCenter Server kimlik bilgisi oluştur

Azure Backup sunucusu için VMware sunucusu eklemeden önce yükleme [Azure Backup sunucusu için Güncelleştirme 1](https://support.microsoft.com/help/3175529/update-1-for-microsoft-azure-backup-server).

1. Azure Backup sunucusu açmak için Azure Backup sunucusu simgesine çift tıklayın.

    ![Azure Backup sunucusu simgesi](./media/backup-azure-backup-server-vmware/mabs-icon.png)

    Masaüstünde simge bulamazsanız, Azure Backup sunucusu yüklü uygulamalar listesinden açın. Azure Backup sunucusu uygulama adı, Microsoft Azure Backup çağrılır.

2. Azure Backup sunucusu konsolunda **Yönetim**, tıklayın **üretim sunucularına**ve araç şeridinde, ardından **yönetme VMware**.

    ![Azure Backup Sunucusu konsolu](./media/backup-azure-backup-server-vmware/add-vmware-credentials.png)

    **Kimlik bilgilerini Yönet** iletişim kutusu görüntülenir.

    ![Azure Backup sunucusu kimlik bilgilerini Yönet iletişim kutusu](./media/backup-azure-backup-server-vmware/mabs-manage-credentials-dialog.png)

3. İçinde **kimlik bilgilerini Yönet** iletişim kutusu, tıklayın **Ekle** açmak için **kimlik bilgileri Ekle** iletişim kutusu.

4. İçinde **kimlik bilgileri Ekle** iletişim kutusunda, bir ad ve yeni kimlik bilgisi için bir açıklama girin. Ardından kullanıcı adı ve parola belirtin. Adı *Contoso Vcenter kimlik bilgisi* sonraki yordamda kimlik bilgisi tanımlamak için kullanılır. Aynı kullanıcı adı ve vCenter sunucusu için kullanılan parolayı kullanın. VCenter Server ve Azure Backup sunucusu aynı etki alanında içinde değilse **kullanıcı adı**, etki alanını belirtin.

    ![Azure Backup sunucusu kimlik bilgisi Ekle iletişim kutusu](./media/backup-azure-backup-server-vmware/mabs-add-credential-dialog2.png)

    Tıklayın **Ekle** Azure Backup sunucusu için yeni kimlik bilgisi eklemek için. Yeni kimlik bilgisi listede görünür **kimlik bilgilerini Yönet** iletişim kutusu.

    ![Azure Backup sunucusu kimlik bilgilerini Yönet iletişim kutusu](./media/backup-azure-backup-server-vmware/new-list-of-mabs-creds.png)

5. Kapatmak için **kimlik bilgilerini Yönet** iletişim kutusu, tıklayın **X** sağ üst köşedeki.


## <a name="add-the-vcenter-server-to-azure-backup-server"></a>Azure Backup Sunucusu'na vCenter Server ekleme

Üretim Sunucusu Ekleme Sihirbazı, Azure Backup Sunucusu'na vCenter Server eklemek için kullanılır.

Üretim Sunucusu Ekleme Sihirbazı'nı açmak için aşağıdaki yordamı izleyin:

1. Azure Backup sunucusu konsolunda **Yönetim**, tıklayın **üretim sunucularına**ve ardından **Ekle**.

    ![Açık Üretim Sunucusu Ekleme Sihirbazı](./media/backup-azure-backup-server-vmware/add-vcenter-to-mabs.png)

    **Üretim Sunucusu Ekleme Sihirbazı** iletişim kutusu görüntülenir.

    ![Üretim Sunucusu Ekleme Sihirbazı](./media/backup-azure-backup-server-vmware/production-server-add-wizard.png)

2. Üzerinde **seçin üretim sunucusu türünü** sayfasında **VMware sunucularını**ve ardından **sonraki**.

3. İçinde **sunucu adı/IP adresi**, tam etki alanı adı (FQDN) veya (ESXi ana bilgisayar sunucusu) VMware sunucusunun IP adresini belirtin. Tüm ESXi sunucuları aynı vCenter tarafından yönetiliyorsa, vCenter adı kullanabilirsiniz.

    ![VMware server FQDN veya IP adresi belirtin](./media/backup-azure-backup-server-vmware/add-vmware-server-provide-server-name.png)

4. İçinde **SSL bağlantı noktası**, VMware sunucusu ile iletişim kurmak için kullanılan bağlantı noktasını girin. Farklı bir bağlantı noktası gerekli olduğunu bilmiyorsanız varsayılan bağlantı noktası, bağlantı noktası 443 de kullanın.

5. İçinde **kimlik bilgisi belirtin**, daha önce oluşturduğunuz kimlik bilgilerini seçin.

    ![Kimlik bilgisini belirtin](./media/backup-azure-backup-server-vmware/identify-creds.png)

6. Tıklayın **Ekle** VMware sunucusu listesine eklemek için **eklenen VMware sunucuları**ve ardından **sonraki** sihirbazdaki sonraki sayfaya taşınır.

    ![VMWare sunucusu ve kimlik bilgisi Ekle](./media/backup-azure-backup-server-vmware/add-vmware-server-credentials.png)

7. İçinde **özeti** sayfasında **Ekle** belirtilen VMware sunucusu için Azure Backup sunucusu eklemek için.

    ![Azure Backup sunucusu için VMware sunucusunu ekleme](./media/backup-azure-backup-server-vmware/tasks-screen.png)

  VMware server yedeğidir aracısız bir yedekleme ve yeni sunucuya hemen eklenir. **Son** sayfası sonuçları gösterir.

  ![Son sayfa](./media/backup-azure-backup-server-vmware/summary-screen.png)

  Azure Backup sunucusu için birden fazla vCenter Server eklemek için bu bölümün önceki adımları yineleyin.

VCenter Server için Azure Backup sunucusu ekledikten sonra sonraki adım bir koruma grubu oluşturmaktır. Koruma grubunun kısa veya uzun süreli saklama için çeşitli ayrıntılarını belirtir, ve burada tanımlayın ve yedekleme ilkesini uygulayın. Yedekleme İlkesi, yedeklemelerin ne zaman ve ne yedeklenir yönelik zamanlamadır.


## <a name="configure-a-protection-group"></a>Bir koruma grubunu yapılandırın

System Center Data Protection Manager veya Azure Backup sunucusu önce kullanmadıysanız bkz [Plan for disk backups](https://technet.microsoft.com/library/hh758026.aspx) donanım ortamınızı hazırlamak için. Uygun depolama sahip iade ettikten sonra VMware sanal makinelerini eklemek için yeni koruma grubu oluşturma Sihirbazı'nı kullanın.

1. Azure Backup sunucusu konsolunda **koruma**ve araç şeridinde tıklayın **yeni** yeni koruma grubu oluşturma Sihirbazı'nı açın.

    ![Yeni koruma grubu oluşturma Sihirbazı'nı açın](./media/backup-azure-backup-server-vmware/open-protection-wizard.png)

    **Yeni koruma grubu oluşturma** Sihirbazı iletişim kutusu görüntülenir.

    ![Yeni koruma grubu oluşturma Sihirbazı iletişim kutusu](./media/backup-azure-backup-server-vmware/protection-wizard.png)

    Tıklayın **sonraki** ilerletmek için **koruma grubu türünü seçin** sayfası.

2. Üzerinde **seçin koruma grubu türünü** sayfasında **sunucuları** ve ardından **sonraki**. **Grup üyelerini seçin** sayfası görüntülenir.

3. Üzerinde **grup üyelerini seçin** sayfasında, kullanılabilir üyeler ve seçilen üyeleri görünür. Koruyun ve ardından istediğiniz üyeleri seçin **sonraki**.

    ![Grup üyelerini seçin](./media/backup-azure-backup-server-vmware/server-add-selected-members.png)

    Diğer klasörler veya Vm'leri içeren bir klasör seçerseniz bir üye seçtiğinizde, bu klasörleri ve Vm'leri de seçilir. İçerme klasörleri ve VM'lerin üst klasördeki klasör düzeyinde koruma olarak adlandırılır. Bir klasörü veya VM kaldırmak için onay kutusunu temizleyin.

    Azure'da bir VM için veya bir VM içeren bir klasör zaten korunuyorsa, bu VM'yi tekrar seçemezsiniz. Azure'da bir VM korunmaya başladıktan sonra diğer bir deyişle, onu yeniden yinelenen kurtarma noktaları için bir VM oluşturulmasını engeller korunamaz. Hangi Azure Backup sunucusu örneğine zaten bir üye korur görmek istiyorsanız, koruma sunucusunun adını görmek için bir üyesine işaret edin.

4. Üzerinde **veri koruma yöntemini seçin** sayfasında, koruma grubu için bir ad girin. Kısa vadeli koruma (diske) ve çevrimiçi koruma seçilir. (Azure'a) çevrimiçi koruma kullanmak istiyorsanız, disk için kısa vadeli koruma kullanmanız gerekir. Tıklayın **sonraki** kısa vadeli koruma aralığın devam etmek için.

    ![Veri koruma yöntemini seçin](./media/backup-azure-backup-server-vmware/name-protection-group.png)

5. Üzerinde **kısa vadeli hedefleri belirtin** sayfası için **bekletme aralığı**, Kurtarma noktalarını saklamak istediğiniz gün sayısını belirtin *diske depolanmış*. Gün bir kurtarma noktası alınma zamanını değiştirin ve isterseniz **Değiştir**. Kısa vadeli kurtarma noktaları tam yedeklemeler ' dir. Artımlı yedeklemeleri değiller. Kısa vadeli hedefleri ile memnun kaldığınızda, tıklayın **sonraki**.

    ![Kısa vadeli hedefleri belirtin](./media/backup-azure-backup-server-vmware/short-term-goals.png)

6. Üzerinde **Disk ayırmayı gözden** sayfasında gözden geçirin ve gerekirse, sanal makineler için disk alanını değiştirin. Önerilen disk ayırmaları belirtilen bekletme aralığına dayalıdır **kısa vadeli hedefleri belirtin** sayfasında, iş yükü türüne ve (3. adımda tanımlanan) korunan verilerin boyutu.  

  - **Veri boyutu:** koruma grubundaki verilerin boyutu.
  - **Disk alanı:** önerilen koruma grubu için disk alanı miktarı. Bu ayarı değiştirmek istiyorsanız, her veri kaynağı büyüdükçe tahmin miktardan biraz daha büyük toplam alan ayırın.
  - **Verileri ortak Konumlandır:** üzerinde birlikte bulundurmaya kapatırsanız, birden çok veri kaynağı koruma için bir tek çoğaltma ve kurtarma noktasına eşleyebilir. Birlikte bulundurma, tüm iş yükleri için desteklenmiyor.
  - **Otomatik olarak Büyüt:** korumalı gruptaki veriler ilk ayırma boyutunu aşarsa bu ayarı üzerinde kapatma, System Center Data Protection Manager disk boyutunu yüzde 25 artırmayı dener.
  - **Depolama havuzu ayrıntıları:** dahil toplam ve kalan disk boyutu depolama havuzunun durumunu gösterir.

    ![Disk ayırmayı İncele](./media/backup-azure-backup-server-vmware/review-disk-allocation.png)

    Alanı ayırma ile memnun kaldığınızda, tıklayın **sonraki**.

7. Üzerinde **çoğaltma oluşturma yöntemini seçin** sayfasında, nasıl ilk kopyalama ya da Azure Backup sunucusu korunan verilerin kopyasını oluşturmak istediğinizi belirtin.

    Varsayılan değer **ağ üzerinden otomatik olarak** ve **artık**. Varsayılan kullanırsanız, yoğun olmayan bir zamanı belirtmenizi öneririz. Seçin **sonra** ve gün ve saati belirtin.

    Büyük miktarlarda veri ya da daha az-en iyi ağ koşulları için Çıkarılabilir medya kullanarak çevrimdışı veri çoğaltmayı göz önünde bulundurun.

    Seçimlerinizi yaptıktan sonra tıklayın **sonraki**.

    ![Çoğaltma oluşturma yöntemini seçin](./media/backup-azure-backup-server-vmware/replica-creation.png)

8. Üzerinde **tutarlılık denetimi seçenekleri** sayfası, nasıl alacağınızı seçin ve tutarlılık denetimleri otomatik hale getirmek ne zaman. Çoğaltma verileri tutarsız hale geldiğinde veya bir programa, tutarlılık denetimlerinin çalıştırabilirsiniz.

    Otomatik tutarlılık denetimlerinin yapılandırmak istemiyorsanız, bir el ile denetim gerçekleştirebilirsiniz. Azure Backup sunucusu konsolunun koruma alanındaki koruma grubuna sağ tıklayın ve ardından **tutarlılık denetimi gerçekleştir**.

    Tıklayın **sonraki** sonraki sayfaya taşınır.

9. Üzerinde **çevrimiçi koruma verilerini belirtin** sayfasında, korumak istediğiniz bir veya daha fazla veri kaynağı seçin. Tek tek üyeleri seçin veya tıklatın **Tümünü Seç** için tüm üyeleri seçin. Üyeleri seçin sonra tıklayın **sonraki**.

    ![Çevrimiçi koruma verilerini belirtin](./media/backup-azure-backup-server-vmware/select-data-to-protect.png)

10. Üzerinde **çevrimiçi yedekleme zamanlamasını belirtin** sayfasında, Kurtarma noktaları disk yedekleme oluşturmak için zamanlamayı belirtin. Kurtarma noktası oluşturulduktan sonra Azure kurtarma Hizmetleri kasasına aktarılır. İle çevrimiçi yedekleme zamanlamasını memnun kaldığınızda, tıklayın **sonraki**.

    ![Çevrimiçi Yedekleme zamanlamasını belirtin](./media/backup-azure-backup-server-vmware/online-backup-schedule.png)

11. Üzerinde **çevrimiçi saklama ilkesini belirtin** sayfasında, yedekleme verilerini azure'da korumak istediğiniz ne kadar süreyle belirtin. İlke tanımlandıktan sonra tıklayın **sonraki**.

    ![Çevrimiçi bekletme ilkesini belirtin](./media/backup-azure-backup-server-vmware/retention-policy.png)

    Azure'da verileri ne kadar süreyle kalmasını sağlayabilirsiniz için zaman sınırı yoktur. Azure'da kurtarma noktası verilerini depoladığınızda, yalnızca korumalı örnek başına birden fazla 9999 kurtarma noktası olamaz sınırdır. Bu örnekte, korumalı örnek VMware sunucusudur.

12. Üzerinde **özeti** sayfasında, ayarları ve koruma grubu üyeleri için ayrıntıları gözden geçirin ve ardından **Grup Oluştur**.

    ![Koruma grubu üyesi ve ayar özeti](./media/backup-azure-backup-server-vmware/protection-group-summary.png)

## <a name="next-steps"></a>Sonraki adımlar
VMware iş yüklerini korumak için Azure Backup sunucusu kullanıyorsanız, korumaya yardımcı olmak için Azure Backup sunucusu kullanarak ilginizi çekebilir bir [Microsoft Exchange server](./backup-azure-exchange-mabs.md), [Microsoft SharePoint grubu](./backup-azure-backup-sharepoint-mabs.md), veya bir [SQL Server veritabanı](./backup-azure-sql-mabs.md).

Koruma grubunu yapılandırma ve işlerini, yedekleme Aracısı'nı kaydetme sorunları hakkında daha fazla bilgi için bkz [Azure Backup sunucusu sorunlarını giderme](./backup-azure-mabs-troubleshoot.md).
