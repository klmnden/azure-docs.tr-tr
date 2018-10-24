---
title: Azure dosya eşitleme ile genişletmek öğretici bir Windows dosya sunucuları | Microsoft Docs
description: Bilgi nasıl Azure dosya eşitleme ile Windows genişletme dosya sunucuları için başlangıç tamamlanması.
services: storage
author: wmgries
ms.service: storage
ms.topic: article
ms.date: 10/23/2018
ms.author: wgries
ms.component: files
ms.openlocfilehash: 4c3ceead792f60ceac7c5eddb64dc4644d662f83
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "49960675"
---
<!---Customer intent: As a IT Administrator, I want see how to extend Windows file servers with Azure File Sync, so I can evaluate the process for extending storage capacity of my Windows Servers.
--->

# <a name="tutorial-extend-windows-file-servers-with-azure-file-sync"></a>Öğretici: Azure dosya eşitleme ile Windows dosya sunucuları genişletme
Azure dosya eşitleme kuruluşunuzun dosya paylaşımlarını tek bir merkezden yönetin ve bir şirket içi dosya sunucusunun depolama kapasitesini genişletmeyi kullanın. Azure dosya eşitleme Windows Server, Azure dosya paylaşımınızın hızlı bir önbelleğine dönüştürür.

Bu öğreticide, bir Windows Server depolama kapasitesini genişletmek için temel adımları kullanarak Azure dosya eşitleme göstereceğiz. Bu öğretici için bir Windows Server Azure VM kullanıyoruz, ancak bu işlem şirket içi sunucularınız için genellikle yaptığınız. Azure dosya eşitleme kendi ortamınızda dağıtmak hazırsanız kullanın [dağıtma Azure dosya eşitleme](storage-sync-files-deployment-guide.md) yerine makalesi.

> [!div class="checklist"]
> * Depolama eşitleme hizmetini dağıtma
> * Windows Server'ın Azure dosya eşitleme ile kullanmak üzere hazırlama
> * Azure dosya eşitleme aracısını yükleme
> * Windows Server depolama eşitleme hizmeti ile kaydetme
> * Bir eşitleme grubuna ve bir bulut uç noktası oluşturma
> * Sunucu uç noktası oluşturma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma
https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="prepare-your-environment-for-the-tutorial"></a>Öğretici için ortamınızı hazırlama
Azure dosya eşitleme'yi dağıtmadan önce Bu öğretici için bir yere koymak için gereken birkaç nokta vardır. Bir Azure depolama hesabını ve dosya oluşturma birlikte paylaşmak, Windows Server 2016 Datacenter VM oluşturacak ve bu sunucu hazırlamak için Azure dosya eşitleme.

### <a name="create-a-folder-and-txt-file"></a>Bir klasörü ve .txt dosyası oluşturun

Yerel bilgisayarınızda adlı yeni bir klasör oluşturma *FilesToSync* adlı bir metin dosyası ekleyin *mytestdoc.txt*. Bu öğreticide daha sonra dosya paylaşımı için bu dosyayı yükleyeceksiniz.

### <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

[!INCLUDE [storage-create-account-portal-include](../../../includes/storage-create-account-portal-include.md)]

### <a name="create-a-file-share"></a>Dosya paylaşımı oluşturma
Ardından, bir dosya paylaşımı oluşturun.

1. Azure depolama hesabı dağıtımı tamamlandığında, tıklayın **kaynağa Git**.
1. Tıklayın **dosyaları** depolama hesabı bölmesinden.

    ![Dosyalar'ı tıklatın](./media/storage-sync-files-extend-servers/click-files.png)

1. Tıklayın **+ dosya paylaşımı**.

    ![Dosya Paylaşımı ekleme düğmesine tıklayın](./media/storage-sync-files-extend-servers/create-file-share-portal2.png)

1. Yeni dosya paylaşımı adı *afsfileshare* için "1" girin **kota**, ardından **Oluştur**. Kota en çok 5 TiB olabilir, ancak bu öğretici için yalnızca 1 GB gerekir.

    ![Yeni dosya paylaşımı için bir ad ve kota belirtin](./media/storage-sync-files-extend-servers/create-file-share-portal3.png)

1. Yeni dosya paylaşımını seçin ve ardından dosya paylaşım konumuna tıklayın **karşıya**.

    ![Dosyayı karşıya yükleme](./media/storage-sync-files-extend-servers/create-file-share-portal5.png)

1. Gözat *FilesToSync* seçin, .txt dosyasının oluşturulduğu klasöre *mytestdoc.txt* tıklatıp **karşıya**.

    ![Dosya paylaşımına göz atın](./media/storage-sync-files-extend-servers/create-file-share-portal6.png)

Bu noktada, Azure içindeki bir dosya ile bir Azure depolama hesabı ve dosya paylaşımı oluşturdunuz. Şimdi bu öğreticide şirket içi sunucusunu temsil edecek Windows Server 2016 Datacenter ile Azure VM oluşturmayı öğreneceksiniz.

### <a name="deploy-a-vm-and-attach-a-data-disk"></a>Bir VM'yi dağıtın ve bir veri diski ekleme

1. Seçin ve ardından, portalın sol taraftaki menüyü genişleterek **kaynak Oluştur** Azure portalının sol üst köşedeki.
1. Arama kutusuna listesinin üzerindeki **Azure Marketi** kaynakları arayın ve seçin **Windows Server 2016 Datacenter**, ardından **Oluştur**.
1. İçinde **Temelleri** sekmesindeki **proje ayrıntıları**, Bu öğretici için oluşturduğunuz kaynak grubunu seçin.

   ![Portal dikey penceresinde VM’niz ile ilgili temel bilgileri girin](./media/storage-sync-files-extend-servers/vm-resource-group-and-subscription.png)

1. Altında **örnek ayrıntıları**, bir VM adı sağlayın *myVM*.
1. Varsayılan ayarlarını bırakın **bölge**, **kullanılabilirlik seçeneklerini**, **görüntü**, ve **boyutu**.
1. Altında **yönetici hesabı**, sağlayan bir **kullanıcıadı** ve **parola** VM için.
1. **Gelen bağlantı noktası kuralları** altında **Seçilen bağlantı noktalarına izin ver**'i, sonra aşağı açılan listeden **RDP (3389)** ve **HTTP** değerlerini seçin.

   VM oluşturmadan önce bir veri diski oluşturmak gerekir.

1. Tıklayın **sonraki: diskleri**

   ![Veri diski ekleme](./media/storage-sync-files-extend-servers/vm-add-data-disk.png)

1. İçinde **diskleri** sekmesindeki **Disk seçenekleri**, varsayılan değerleri bırakın. 
1. Altında **veri DİSKLERİ**, tıklayın **oluşturun ve yeni bir disk eklemek**.

1. Değiştirme dışındaki varsayılan değerleri bırakın **boyutu (GiB)** için **1 GB** Bu öğretici için.

   ![Veri disk ayrıntıları](./media/storage-sync-files-extend-servers/vm-create-new-disk-details.png)

1. **Tamam** düğmesine tıklayın.
1. **Gözden geçir ve oluştur**’a tıklayın.
1. **Oluştur**’a tıklayın.

   Tıklayabilirsiniz **bildirimleri** izlemek için simge **dağıtım ilerleme durumu**. Yeni VM oluşturma tamamlanması birkaç dakika sürer.

1. VM dağıtımınız tamamlandıktan sonra tıklayın **kaynağa Git**.

   ![Kaynağa git](./media/storage-sync-files-extend-servers/vm-gotoresource.png)

   Bu noktada yeni bir sanal makine oluşturduğunuz ve bir veri diski eklenmiş. Artık VM bağlanmanız gerekir.

### <a name="connect-to-your-vm"></a>Sanal Makinenize bağlanın

1. Azure portalında **Connect** sanal makine özellikleri sayfasında.

   ![Portaldan bir Azure sanal makinesine bağlanma](./media/storage-sync-files-extend-servers/connect-vm.png)

1. İçinde **sanal makineye bağlanma** sayfasında, saklama tarafından bağlanmak için varsayılan seçenekleri **IP adresi** üzerinde 3389 numaralı bağlantı noktası ve tıklayın **indirme RDP dosyası**.

   ![RDP dosyasını indir](./media/storage-sync-files-extend-servers/download-rdp.png)

1. İndirilen RDP dosyasını açın ve istendiğinde **Bağlan**’a tıklayın.
1. **Windows Güvenliği** penceresinde **Diğer seçenekler**'i ve ardından **Başka bir hesap kullanın**'ı seçin. Kullanıcı adı olarak yazın *localhost\username*, sanal makine için oluşturduğunuz parola girin ve ardından **Tamam**.

   ![Daha fazla seçenek](./media/storage-sync-files-extend-servers/local-host2.png)

1. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Bağlantıyı oluşturmak için **Evet** veya **Devam**’a tıklayın.

### <a name="prepare-the-windows-server"></a>Windows Server Hazırlama
İçin **Windows Server 2016 Datacenter** sunucusu devre dışı **Internet Explorer Artırılmış Güvenlik Yapılandırması**. Bu adım yalnızca ilk sunucu kaydı için gereklidir. Sunucu kaydedildikten sonra yeniden etkinleştirebilirsiniz.

İçinde **Windows Server 2016 Datacenter** VM **Sunucu Yöneticisi'ni** otomatik olarak açılır.  Varsa **Sunucu Yöneticisi'ni** varsayılan olarak, onun için arama Gezgini içinde açmaz.

1. İçinde **Sunucu Yöneticisi'ni** tıklayın **yerel sunucu**.

   !["Yerel sunucuda" sol tarafındaki Sunucu Yöneticisi kullanıcı Arabirimi](media/storage-sync-files-extend-servers/prepare-server-disable-ieesc-1.png)

1. Üzerinde **özellikleri** alt bölme, bağlantısını seçin **IE Artırılmış Güvenlik Yapılandırması**.  

    ![Sunucu Yöneticisi kullanıcı arabiriminde "IE Artırılmış Güvenlik Yapılandırması" bölmesi](media/storage-sync-files-extend-servers/prepare-server-disable-ieesc-2.png)

1. İçinde **Internet Explorer Artırılmış Güvenlik Yapılandırması** iletişim kutusunda **kapalı** için **Yöneticiler** ve **kullanıcılar**:  

    ![Internet Explorer Artırılmış Güvenlik Yapılandırması pop penceresi "Kapalı" ile işaretli](media/storage-sync-files-extend-servers/prepare-server-disable-ieesc-3.png)

   Şimdi VM veri diski ekleyebilirsiniz.

### <a name="add-the-data-disk"></a>Veri diski ekleme

1. Yine **Windows Server 2016 Datacenter** VM tıklayın **dosya ve depolama hizmetleri** > **birimleri** > **diskler** .

    ![Veri diski](media/storage-sync-files-extend-servers/your-disk.png)

1. Adlı 1 GB disk sağ **Msft Sanal Disk** tıklatıp **yeni birim**.
1. Belirtmeye atanan sürücü harfini Varsayılanları yerinde bırakarak Sihirbazı tamamlayın ve tıklayın **Oluştur**.
1. **Kapat**’a tıklayın.

   Bu noktada, diski çevrimiçi duruma ve bir birim oluşturduğunuzda. Veri diski ekleme VM'de Gezgini'ni açarak ve yeni bir sürücüye mevcut olduğunu onaylayan başarılı olduğunu doğrulayabilirsiniz.

1. VM üzerinde Gezgini'nde **bu PC** ve yeni bir sürücüye çift tıklayın. Bu örnek, F: sürücüdür.
1. Sağ tıklayıp **yeni** > **klasör**. Klasör adı *FilesToSync*.
1. Çift **FilesToSync** klasör.
1. Sağ tıklayıp **yeni** > **metin belgesi**. Metin dosyasının adı *MyTestFile*.

    ![Yeni bir metin dosyası ekleyin](media/storage-sync-files-extend-servers/new-file.png)

1. Kapat **Gezgini** ve **Sunucu Yöneticisi**.

### <a name="download-the-azurerm-powershell-module"></a>AzureRM PowerShell modülünü indirin
Ardından **Windows Server 2016 Datacenter** VM yükleme **AzureRM PowerShell Modülü** sunucusunda.

1. Bir VM'de, yükseltilmiş bir PowerShell penceresi açın.
1. Şu komutu çalıştırın:

   ```powershell
   Install-Module -Name AzureRM -AllowClobber
   ```

   > [!NOTE]
   > NuGet sürümünüz 2.8.5.201'den eskiyse, en son NuGet sürümünü indirip yüklemeniz istenir.

   PowerShell galerisi varsayılan olarak PowerShellGet için güvenilir depo olarak yapılandırılmamıştır. PSGallery'yi ilk kez kullandığınızda şu istemle karşılaşırsınız:

   ```output
   Untrusted repository

   You are installing the modules from an untrusted repository. If you trust this repository, change its InstallationPolicy value by running the Set-PSRepository cmdlet.

   Are you sure you want to install the modules from 'PSGallery'?
   [Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "N"):
   ```

1. Yükleme işlemine devam etmek için `Yes` veya `Yes to All` yanıtını verin.

`AzureRM` modülü, Azure PowerShell cmdlet’leri için toplu bir modüldür. Yükleme, tüm kullanılabilir Azure Resource Manager modüllerini yükler ve içerdikleri cmdlet'ler kullanılabilir hale getirir.

Bu noktada, ortamınızı ayarı için öğreticiyi bitirdikten sonra ve dağıtmaya başlamak hazırsınız **depolama eşitleme hizmeti**.

## <a name="deploy-the-storage-sync-service"></a>Depolama eşitleme hizmetini dağıtma 
Azure dosya eşitleme dağıtımı başlatır yerleştirme ile bir **depolama eşitleme hizmeti** , seçili abonelik için bir kaynak grubuna kaynak. Depolama eşitleme hizmeti içine dağıtma abonelik ve kaynak grubuna erişim izinleri devralır.

1. Azure portalında **kaynak Oluştur** bulun **Azure dosya eşitleme**.
1. Arama sonuçlarında seçin **Azure dosya eşitleme**.
1. Seçin **Oluştur** açmak için **dağıtma depolama eşitleme** sekmesi.

   ![Depolama Eşitleme'yi Dağıt](media/storage-sync-files-extend-servers/afs-info.png)

   Açılan bölmede aşağıdaki bilgileri girin:

   | Değer | Açıklama |
   | ----- | ----- |
   | **Ad** | Depolama eşitleme hizmeti için benzersiz bir ad (başına abonelik).<br><br>Bu öğreticide kullanıyoruz *afssyncservice02*. |
   | **Abonelik** | Bu öğretici için kullandığınız abonelik. |
   | **Kaynak grubu** | Bu öğretici için kullandığınız kaynak grubu.<br><br>Bu öğreticide kullanıyoruz *afsresgroup101918*. |
   | **Konum** | Doğu ABD |

1. İşlemi tamamladığınızda, seçin **Oluştur** dağıtmak için **depolama eşitleme hizmeti**.
1. Tıklayın **bildirimleri** sekmesi > **kaynağa Git**.

## <a name="install-the-azure-file-sync-agent"></a>Azure dosya eşitleme aracısını yükleme
Azure dosya eşitleme aracısını Windows Server'ın bir Azure dosya paylaşımı ile eşitlenmesine imkan sağlayan indirilebilir bir pakettir.

1. Dönmek **Windows Server 2016 Datacenter** VM ve **Internet Explorer**
1. Git [Microsoft İndirme Merkezi](https://go.microsoft.com/fwlink/?linkid=858257). Ekranı aşağı kaydırarak **Azure dosya eşitleme Aracısı** tıklayın ve bölüm **indirin**.

   ![Eşitleme Aracısı indirme](media/storage-sync-files-extend-servers/sync-agent-download.png)

1. İçin kutuyu **StorageSyncAgent_V3_WS2016. EXE** tıklatıp **sonraki**.

   ![Aracısı'nı seçin](media/storage-sync-files-extend-servers/select-agent.png)

1. **Bir kez izin ver** > **çalıştırma** > **açık** dosya.
1. Zaten yapmadıysanız, PowerShell penceresini kapatın.
1. Varsayılan değerleri kabul **depolama eşitleme Aracısı Kurulum Sihirbazı'nı**.
1. **Yükle**'ye tıklayın.
1. **Son**'a tıklayın.

Dağıtılan Azure eşitleme hizmeti ve aracı yüklü **Windows Server 2016 Datacenter** VM. Artık VM ile kaydetmek gerekiyor **depolama eşitleme hizmeti**.

## <a name="register-windows-server-with-storage-sync-service"></a>Windows Server depolama eşitleme hizmeti ile kaydetme
Windows Server depolama eşitleme hizmeti ile kaydetme, sunucu (veya küme) arasında bir güven ilişkisi oluşturur ve depolama eşitleme hizmeti. Bir sunucu yalnızca bir depolama eşitleme hizmetine kayıtlı olması ve diğer sunuculara ve Azure ile eşitleyebilirsiniz dosya paylaşımları aynı depolama eşitleme hizmeti ile ilişkili.

Yükledikten sonra sunucu kaydı UI otomatik olarak açılmalıdır **Azure dosya eşitleme aracısının**. Seçili değilse bunu el ile dosya konumundan açabilirsiniz: C:\Program Files\Azure\StorageSyncAgent\ServerRegistration.exe.

1. Sunucu kaydı UI VM açıldığında tıklayın **Tamam**.
1. Tıklayın **oturum** başlamak için.
1. Azure hesabı kimlik bilgilerinizle oturum açın ve tıklayın **oturum açma**.
1. Şu bilgileri belirtin:

   ![Sunucu kaydı UI görüntüsü](media/storage-sync-files-extend-servers/signin.png)

   | | |
   | ----- | ----- |
   | Değer | Açıklama |
   | **Azure aboneliği** | Bu öğretici için depolama eşitleme hizmeti içeren aboneliği. |
   | **Kaynak Grubu** | Bu öğretici için depolama eşitleme hizmeti içeren bir kaynak grubu. Kullandığımız *afsresgroup101918* Bu öğretici boyunca. |
   | **Depolama eşitleme hizmeti** | Bu öğreticide kullanılan depolama eşitleme hizmeti adı. Kullandığımız *afssyncservice02* Bu öğretici boyunca. |

1. Tıklayın **kaydetme** sunucu kaydını tamamlamak için.
1. Bir ek oturum açma için kayıt işleminin bir parçası olarak istenir. Oturum açın ve tıklayın **sonraki**.
1. **Tamam**’a tıklayın.

## <a name="create-a-sync-group-and-a-cloud-endpoint"></a>Bir eşitleme grubuna ve bir bulut uç noktası oluşturma
Bir eşitleme grubu eşitleme topolojisi için bir dosya kümesini tanımlar. Bir Azure dosya paylaşımı ve bir veya daha fazla sunucu uç noktalarını temsil eden bir bulut uç noktası, bir eşitleme grubu içermelidir. Sunucu uç noktası bir kayıtlı sunucudaki bir yolu temsil eder.

1. Bir eşitleme grubu oluşturmak için [Azure portalında](https://portal.azure.com/)seçin **+ eşitleme grubu** Bu öğretici için oluşturduğunuz depolama eşitleme hizmeti. Kullandık *afssyncservice02* Bu öğreticide örnek olarak.

   ![Azure portalında yeni bir eşitleme grubu oluşturma](media/storage-sync-files-extend-servers/add-sync-group.png)

1. Açılan bölmede bir bulut uç noktası ile bir eşitleme grubu oluşturmak için aşağıdaki bilgileri girin:

   | Değer | Açıklama |
   | ----- | ----- |
   | **Eşitleme grubu adı** | Bu ad, depolama eşitleme hizmeti içinde benzersiz olmalıdır, ancak sizin için mantıklı olan herhangi bir ad olabilir. Bu öğreticide, kullandığımız *afssyncgroup*.|
   | **Abonelik** | Bu öğretici için depolama eşitleme hizmeti dağıtıldığı abonelik. |
   | **Depolama hesabı** |Tıklayın **depolama hesabını seçin**. Görünen bölmede, Bu öğretici için oluşturduğunuz Azure dosya paylaşımını içeren depolama hesabını seçin. Kullandık *afsstoracct101918*. |
   | **Azure dosya paylaşımı** | Bu öğretici için oluşturduğunuz Azure dosya paylaşımının adı. Kullandık *afsfileshare*. |

1. **Oluştur**’a tıklayın.

Eşitleme grubunuz seçerseniz, artık bir olduğunu görebilirsiniz **bulut uç noktası**.

## <a name="add-a-server-endpoint"></a>Sunucu uç noktası ekleme
Sunucu uç noktası, kayıtlı bir sunucuda, bir sunucu birimdeki bir klasör gibi belirli bir konuma temsil eder.

1. Sunucu uç noktası eklemek için yeni oluşturulan eşitleme grubunu seçin ve ardından **sunucusu uç noktası ekleme**.

   ![Eşitleme grubu bölmesinde yeni bir sunucu uç noktası ekleme](media/storage-sync-files-extend-servers/add-server-endpoint.png)

1. İçinde **sunucusu uç noktası ekleme** bölmesinde, sunucu uç noktası oluşturmak için aşağıdaki bilgileri girin:

   | | |
   | ----- | ----- |
   | Değer | Açıklama |
   | **Kayıtlı sunucu** | Bu öğretici için oluşturduğunuz sunucunun adı. Kullandık *afsvm101918* bu öğreticideki ||
   | **Path** | Bu öğretici için oluşturduğunuz sürücü Windows Server yolu. Bu örnekte olduğu *f:\filestosync*. |
   | **Bulut Katmanlaması** | Bu öğretici için devre dışı bırakın. |
   | **Birim boş alanı** | Bu öğretici için boş bırakın. |

1. **Oluştur**’a tıklayın.

Dosyalarınız artık Windows Server ve Azure dosya paylaşımı arasında eşitlenmiş durumda tutulur.

![Azure depolama başarıyla eşitlendi](media/storage-sync-files-extend-servers/files-synced-in-azurestorage.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [storage-files-clean-up-portal](../../../includes/storage-files-clean-up-portal.md)]

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, Azure dosya eşitleme kullanan bir Windows Server depolama kapasitesini genişletmek için temel adımlarını öğrendiniz. Bir Azure dosya eşitleme dağıtımı planlama daha kapsamlı bir bakış için bu bağlantıyı izleyin.

> [!div class="nextstepaction"]
> [Azure dosya eşitleme dağıtımı planlama](./storage-sync-files-planning.md)