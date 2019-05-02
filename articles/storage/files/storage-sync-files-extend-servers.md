---
title: Öğretici - Azure dosya eşitleme ile Windows dosya sunucuları genişletin | Microsoft Docs
description: Windows dosya sunucuları, Azure dosya eşitleme ile baştan genişletmeyi öğrenin.
services: storage
author: roygara
ms.service: storage
ms.topic: tutorial
ms.date: 10/23/2018
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: df3850a839ac789957a9adffb7122a0b58987781
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64705056"
---
# <a name="tutorial-extend-windows-file-servers-with-azure-file-sync"></a>Öğretici: Windows dosya sunucularını Azure Dosya Eşitleme ile genişletme

Bu makalede, Azure dosya eşitleme'yi kullanarak bir Windows server depolama kapasitesini genişletmek için temel adımlar gösterilmektedir. Öğretici, bir Azure sanal makinesi (VM) olarak Windows Server özelliklerini olsa da, bu işlem genellikle şirket içi sunucularınız için gerçekleştirilir. Kendi ortamınızda Azure dosya eşitleme dağıtımı için yönergeler bulabilirsiniz [dağıtma Azure dosya eşitleme](storage-sync-files-deployment-guide.md) makalesi.

> [!div class="checklist"]
> * Depolama Eşitleme Hizmetini Dağıtma
> * Windows Server’ı Azure Dosya Eşitleme ile kullanmaya hazırlama
> * Azure Dosya Eşitleme aracısını yükleme
> * Windows Server depolama eşitleme hizmeti ile kaydetme
> * Bir eşitleme grubu ve bir bulut uç noktası oluşturma
> * Sunucu uç noktası oluşturma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Azure Portal](https://portal.azure.com) oturum açın.

## <a name="prepare-your-environment"></a>Ortamınızı hazırlama

Bu öğreticide, Azure dosya eşitleme dağıtmadan önce aşağıdakileri yapmanız gerekir:

- Bir Azure depolama hesabına ve dosya paylaşımı oluşturma
- Bir Windows Server 2016 Datacenter VM ayarlama
- Azure dosya eşitleme için Windows Server VM hazırlama

### <a name="create-a-folder-and-txt-file"></a>Klasör ve .txt dosyası oluşturma

Yerel bilgisayarınızda _EşitlenecekDosyalar_ adlı yeni bir klasör oluşturun ve _testbelgem.txt_ adlı bir metin dosyası ekleyin. Bu öğreticide daha sonra dosya paylaşımı için bu dosyayı yükleyeceksiniz.

### <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

[!INCLUDE [storage-create-account-portal-include](../../../includes/storage-create-account-portal-include.md)]

### <a name="create-a-file-share"></a>Dosya paylaşımı oluşturma

Azure depolama hesabınız dağıttıktan sonra bir dosya paylaşımı oluşturun.

1. Azure portalında **kaynağa Git**.
1. Seçin **dosyaları** depolama hesabı bölmesinden.

    ![Dosyaları seçin](./media/storage-sync-files-extend-servers/click-files.png)

1. Seçin **+ dosya paylaşımı**.

    ![Dosya Paylaşımı ekleme düğmesine seçin](./media/storage-sync-files-extend-servers/create-file-share-portal2.png)

1. Yeni dosya paylaşımı adı _afsfileshare_. "1" girin **kota**ve ardından **Oluştur**. Kota en fazla 5 TiB olabilir, ancak bu öğretici için yalnızca 1 GB gerekir.

    ![Yeni dosya paylaşımı için bir ad ve kota belirtin](./media/storage-sync-files-extend-servers/create-file-share-portal3.png)

1. Yeni dosya paylaşımı seçin. Dosya paylaşımı konumu seçin **karşıya**.

    ![Dosyayı karşıya yükleme](./media/storage-sync-files-extend-servers/create-file-share-portal5.png)

1. Gözat _FilesToSync_ seçin, .txt dosyasının oluşturulduğu klasöre _mytestdoc.txt_ seçip **karşıya**.

    ![Dosya paylaşımına göz atın](./media/storage-sync-files-extend-servers/create-file-share-portal6.png)

Bu noktada, bir dosyada ile bir depolama hesabı ve dosya paylaşımı oluşturdunuz. Ardından, bu öğreticide şirket içi sunucusunu temsil edecek Windows Server 2016 Datacenter ile bir Azure VM dağıtın.

### <a name="deploy-a-vm-and-attach-a-data-disk"></a>VM dağıtma ve veri diskini kullanıma açma

1. Azure portalına gidin ve sol taraftaki menüyü genişleterek. Seçin **kaynak Oluştur** sol üst köşesinde.
1. Arama kutusuna listesinin üzerindeki **Azure Marketi** kaynakları arayın **Windows Server 2016 Datacenter** ve sonuçları seçin. **Oluştur**’u seçin.
1. Git **Temelleri** sekmesi. Altında **proje ayrıntıları**, Bu öğretici için oluşturduğunuz kaynak grubunu seçin.

   ![Portal dikey penceresinde VM'niz ile ilgili temel bilgileri girin](./media/storage-sync-files-extend-servers/vm-resource-group-and-subscription.png)

1. Altında **örnek ayrıntıları**, bir VM adı sağlayın. Örneğin, _myVM_.
1. İçin varsayılan ayarları değiştirmeyin **bölge**, **kullanılabilirlik seçeneklerini**, **görüntü**, ve **boyutu**.
1. **Yönetici hesabı** altında VM için bir **Kullanıcı adı** ve **Parola** girin.
1. Altında **gelen bağlantı noktası kuralları**, seçin **Seçili bağlantı noktalarına izin** seçip **RDP (3389)** ve **HTTP** aşağı açılan menüden.

1. VM’yi oluşturmadan önce bir veri diski oluşturmanız gerekir.

   1. Seçin **sonraki: diskleri**.

      ![Veri diski ekleme](./media/storage-sync-files-extend-servers/vm-add-data-disk.png)

   1. Üzerinde **diskleri** sekmesindeki **Disk seçenekleri**, varsayılan değerleri bırakın.
   1. Altında **veri DİSKLERİ**seçin **oluşturun ve yeni bir disk eklemek**.

   1. Dışında varsayılan ayarları kullanın **boyutu (GiB)**, için değiştirebileceğiniz **1 GB** Bu öğretici için.

      ![Veri diski ayrıntıları](./media/storage-sync-files-extend-servers/vm-create-new-disk-details.png)

   1. **Tamam**’ı seçin.
1. **İncele ve oluştur**’u seçin.
1. **Oluştur**’u seçin.

   Seçebileceğiniz **bildirimleri** izlemek için simge **dağıtım ilerleme durumu**. Yeni VM oluşturma tamamlanması birkaç dakika sürebilir.

1. VM dağıtımınız tamamlandıktan sonra seçin **kaynağa Git**.

   ![Kaynağa git](./media/storage-sync-files-extend-servers/vm-gotoresource.png)

Bu noktada yeni bir sanal makine oluşturdunuz ve bir veri diskini kullanıma açtınız. Sonraki VM'ye bağlanın.

### <a name="connect-to-your-vm"></a>Sanal makinenize bağlanma

1. Azure portalında **Connect** sanal makine özellikleri sayfasında.

   ![Portaldan bir Azure sanal makinesine bağlanma](./media/storage-sync-files-extend-servers/connect-vm.png)

1. Üzerinde **sanal makineye bağlanma** sayfasında, saklama tarafından bağlanmak için varsayılan seçenekleri **IP adresi** bağlantı noktası 3389 üzerinden. **RDP dosyasını indir**'i seçin.

   ![RDP dosyasını indirme](./media/storage-sync-files-extend-servers/download-rdp.png)

1. İndirilen RDP dosyasını açın ve seçin **Connect** istendiğinde.
1. **Windows Güvenliği** penceresinde **Diğer seçenekler**'i ve ardından **Başka bir hesap kullanın**'ı seçin. Kullanıcı adı olarak yazın *localhost\username*, sanal makine için oluşturduğunuz parola girin ve ardından **Tamam**.

   ![Diğer seçenekler](./media/storage-sync-files-extend-servers/local-host2.png)

1. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Seçin **Evet** veya **devam** bağlantı oluşturmak için.

### <a name="prepare-the-windows-server"></a>Windows server hazırlama

Windows Server 2016 Datacenter sunucusu için Internet Explorer Artırılmış Güvenlik Yapılandırması devre dışı bırakın. Bu adım yalnızca ilk sunucu kaydı için gereklidir. Sunucu kaydedildikten sonra özelliği yeniden etkinleştirebilirsiniz.

Windows Server 2016 Datacenter VM, Sunucu Yöneticisi otomatik olarak açılır.  Sunucu Yöneticisi varsayılan olarak açık değilse, dosya Gezgini'nde arama.

1. İçinde **Sunucu Yöneticisi'ni**seçin **yerel sunucu**.

   ![Sunucu Yöneticisi kullanıcı arabiriminin sol tarafındaki "Yerel Sunucu"](media/storage-sync-files-extend-servers/prepare-server-disable-ieesc-1.png)

1. Üzerinde **özellikleri** bölmesinde bağlantısını seçin **IE Artırılmış Güvenlik Yapılandırması**.  

    ![Sunucu Yöneticisi kullanıcı arabirimindeki "IE Artırılmış Güvenlik Yapılandırması" bölmesi](media/storage-sync-files-extend-servers/prepare-server-disable-ieesc-2.png)

1. **Internet Explorer Artırılmış Güvenlik Yapılandırması** iletişim kutusunda, **Yöneticiler** ve **Kullanıcılar** için **Kapalı** seçeneğini belirleyin.

    !["Kapalı" seçeneğinin işaretli olduğu Internet Explorer Artırılmış Güvenlik Yapılandırması açılır penceresi](media/storage-sync-files-extend-servers/prepare-server-disable-ieesc-3.png)

Şimdi VM’ye veri diski ekleyebilirsiniz.

### <a name="add-the-data-disk"></a>Veri diski ekleme

1. Yine **Windows Server 2016 Datacenter** VM'yi seçin **dosya ve depolama hizmetleri** > **birimleri** > **diskler** .

    ![Veri diski](media/storage-sync-files-extend-servers/your-disk.png)

1. Adlı 1 GB disk sağ **Msft Sanal Disk** seçip **yeni birim**.
1. Sihirbazı tamamlayın. Varsayılan ayarları kullanın ve atanan sürücü harfini not edin.
1. **Oluştur**’u seçin.
1. Seçin **Kapat**.

   Bu noktada, diski çevrimiçi duruma getirdiniz ve bir birim oluşturdunuz. Son eklenen veri diski varlığını onaylamak için Windows Server VM dosya Gezgini'ni açın.

1. Sanal makine içindeki dosya Gezgini'nde **bu PC** ve yeni bir sürücüye açın. Bu örnekte yeni sürücü F: sürücüsüdür.
1. Sağ tıklayıp **Yeni** > **Klasör**’ü seçin. Klasörü _EşitlenecekDosyalar_ olarak adlandırın.
1. Açık **FilesToSync** klasör.
1. Sağ tıklayıp **Yeni** > **Metin Belgesi**’ni seçin. Metin dosyasını _TestDosyam_ olarak adlandırın.

    ![Yeni metin dosyası ekleme](media/storage-sync-files-extend-servers/new-file.png)

1. Kapat **dosya Gezgini** ve **Sunucu Yöneticisi**.

### <a name="download-the-azure-powershell-module"></a>Azure PowerShell modülünü indirin

Ardından, Windows Server 2016 Datacenter VM sunucuda Azure PowerShell modülünü yükleyin.

1. Bir VM'de, yükseltilmiş bir PowerShell penceresi açın.
1. Şu komutu çalıştırın:

   ```powershell
   Install-Module -Name Az
   ```

   > [!NOTE]
   > 2.8.5.201 eski bir NuGet sürümü varsa, en son NuGet sürümünü indirip istenir.

   PowerShell galerisi varsayılan olarak PowerShellGet için güvenilir depo olarak yapılandırılmamıştır. Psgallery'yi ilk kez aşağıdaki iletiyi görürsünüz:

   ```output
   Untrusted repository

   You are installing the modules from an untrusted repository. If you trust this repository, change its InstallationPolicy value by running the Set-PSRepository cmdlet.

   Are you sure you want to install the modules from 'PSGallery'?
   [Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "N"):
   ```

1. Yanıt **Evet** veya **Tümüne Evet** yüklemeye devam etmek için.

`Az` modülü, Azure PowerShell cmdlet’leri için toplu bir modüldür. Bu modülü yüklediğinizde kullanılabilir durumdaki tüm Azure Resource Manager modülleri indirilir ve cmdlet’leri kullanıma sunulur.

Bu noktada, Öğretici için ortamınızı ayarlama. Depolama eşitleme hizmeti dağıtmaya hazırsınız.

## <a name="deploy-the-service"></a>Hizmeti dağıtma

Azure dosya eşitleme dağıtmak için önce koymanız bir **depolama eşitleme hizmeti** , seçili abonelik için bir kaynak grubuna kaynak. Depolama eşitleme hizmeti, kaynak grubu ve abonelik erişim izinleri devralır.

1. Azure portalında **kaynak Oluştur** bulun **Azure dosya eşitleme**.
1. Arama sonuçlarında **Azure Dosya Eşitleme**’yi seçin.
1. **Oluştur**’u seçerek **Depolama Eşitleme’yi Dağıt** sekmesini açın.

   ![Depolama Eşitleme’yi Dağıtma](media/storage-sync-files-extend-servers/afs-info.png)

   Açılan bölmeye aşağıdaki bilgileri girin:

   | Değer | Açıklama |
   | ----- | ----- |
   | **Ad** | Depolama Eşitleme Hizmeti için benzersiz bir ad (abonelik başına).<br><br>Kullanım _afssyncservice02_ Bu öğretici için. |
   | **Abonelik** | Bu öğretici için kullandığınız Azure aboneliği. |
   | **Kaynak grubu** | Depolama eşitleme hizmeti içeren bir kaynak grubu.<br><br>Kullanım _afsresgroup101918_ Bu öğretici için. |
   | **Konum** | Doğu ABD |

1. İşiniz bittiğinde **Oluştur**’u seçerek **Depolama Eşitleme Hizmeti**’ni dağıtın.
1. Seçin **bildirimleri** sekmesi > **kaynağa Git**.

## <a name="install-the-agent"></a>Aracıyı yükleme

Azure Dosya Eşitleme aracısı, Windows Server’ın bir Azure dosya paylaşımı ile eşitlenmesini sağlayan indirilebilir bir pakettir.

1. İçinde **Windows Server 2016 Datacenter** VM, açık **Internet Explorer**.
1. [Microsoft İndirme Merkezi](https://go.microsoft.com/fwlink/?linkid=858257)’ne gidin. Ekranı aşağı kaydırarak **Azure dosya eşitleme Aracısı** bölümünde ve seçin **indirme**.

   ![Eşitleme aracısını indirme](media/storage-sync-files-extend-servers/sync-agent-download.png)

1. Onay kutusunu seçin **StorageSyncAgent_V3_WS2016. EXE** seçip **sonraki**.

   ![Aracı seçme](media/storage-sync-files-extend-servers/select-agent.png)

1. Seçin **bir kez izin ver** > **çalıştırma** > **açık**.
1. Henüz yapmadıysanız, PowerShell penceresini kapatın.
1. **Depolama Eşitleme Aracısı Kurulum Sihirbazı**’nda varsayılan ayarları kabul edin.
1. **Yükle**’yi seçin.
1. **Son**’u seçin.

Azure eşitleme hizmeti dağıtılan ve aracı Windows Server 2016 Datacenter VM üzerinde yüklü. Artık VM depolama eşitleme hizmetine kaydetmeniz gerekir.

## <a name="register-windows-server"></a>Windows Server’ı kaydetme

Windows server'ınıza bir depolama eşitleme hizmeti ile kaydetme, sunucu (veya küme) arasında bir güven ilişkisi oluşturur ve depolama eşitleme hizmeti. Bir sunucu yalnızca bir depolama eşitleme hizmetine kayıtlı olması. Diğer sunuculara ve bu depolama eşitleme hizmeti ile ilişkili Azure dosya paylaşımları ile eşitleyebilirsiniz.

Azure dosya eşitleme aracısını yükledikten sonra sunucu kaydı UI otomatik olarak açmanız gerekir. Seçili değilse bunu dosya konumundan el ile açabilirsiniz: `C:\Program Files\Azure\StorageSyncAgent\ServerRegistration.exe.`

1. Sunucu kaydı UI VM açıldığında seçin **Tamam**.
1. Seçin **oturum** başlamak için.
1. Azure hesabı kimlik bilgilerinizle oturum açın ve seçin **oturum açma**.
1. Şu bilgileri belirtin:

   ![Sunucu Kaydı kullanıcı arabiriminin ekran görüntüsü](media/storage-sync-files-extend-servers/signin.png)

   | | |
   | ----- | ----- |
   | Değer | Açıklama |
   | **Azure Aboneliği** | Bu öğretici için Depolama Eşitleme Hizmetini içeren abonelik. |
   | **Kaynak Grubu** | Depolama eşitleme hizmeti içeren bir kaynak grubu. Kullanım _afsresgroup101918_ Bu öğretici için. |
   | **Depolama Eşitleme Hizmeti** | Depolama eşitleme hizmeti adı. Kullanım _afssyncservice02_ Bu öğretici için. |

1. Seçin **kaydetme** sunucu kaydını tamamlamak için.
1. Bir ek oturum açma için kayıt işleminin bir parçası olarak istenir. Oturum açın ve seçin **sonraki**.
1. **Tamam**’ı seçin.

## <a name="create-a-sync-group"></a>Eşitleme grubu oluşturma

Eşitleme grubu, bir dosya kümesi için eşitleme topolojisini tanımlar. Azure dosya paylaşımını temsil eden bir bulut uç noktası, bir eşitleme grubu içermelidir. Bir eşitleme grubu bir veya daha fazla sunucu uç noktalarını da içermelidir. Sunucu uç noktası, kayıtlı bir sunucu üzerindeki bir yolu temsil eder. Bir eşitleme grubu oluşturmak için:

1. İçinde [Azure portalında](https://portal.azure.com/)seçin **+ eşitleme grubu** depolama eşitleme hizmeti. Kullanım *afssyncservice02* Bu öğretici için.

   ![Azure portalda yeni bir eşitleme grubu oluşturma](media/storage-sync-files-extend-servers/add-sync-group.png)

1. Bir bulut uç noktası ile bir eşitleme grubu oluşturmak için aşağıdaki bilgileri girin:

   | Değer | Açıklama |
   | ----- | ----- |
   | **Eşitleme grubu adı** | Bu ad Depolama Eşitleme Hizmetinde benzersiz olmalıdır, ancak size mantıklı gelen herhangi bir ad olabilir. Kullanım *afssyncgroup* Bu öğretici için.|
   | **Abonelik** | Bu öğretici için Depolama Eşitleme Hizmetini dağıttığınız abonelik. |
   | **Depolama hesabı** | Seçin **depolama hesabını seçin**. Görünen bölmede, oluşturduğunuz Azure dosya paylaşımını içeren depolama hesabını seçin. Kullanım *afsstoracct101918* Bu öğretici için. |
   | **Azure dosya paylaşımı** | Oluşturduğunuz Azure dosya paylaşımının adı. Kullanım *afsfileshare* Bu öğretici için. |

1. **Oluştur**’u seçin.

Eşitleme grubunuzu seçerseniz, artık bir **bulut uç noktanızın** olduğunu görebilirsiniz.

## <a name="add-a-server-endpoint"></a>Sunucu uç noktası ekleme

Sunucu uç noktası, kayıtlı bir sunucudaki belirli bir konuma temsil eder. Örneğin, bir sunucu birimdeki bir klasör. Sunucu uç noktası eklemek için:

1. Yeni oluşturulan eşitleme grubunu seçin ve ardından **sunucusu uç noktası ekleme**.

   ![Eşitleme grubu bölmesine yeni bir sunucu uç noktası ekleme](media/storage-sync-files-extend-servers/add-server-endpoint.png)

1. Üzerinde **sunucusu uç noktası ekleme** bölmesinde, sunucu uç noktası oluşturmak için aşağıdaki bilgileri girin:

   | | |
   | ----- | ----- |
   | Değer | Açıklama |
   | **Kayıtlı sunucu** | Oluşturduğunuz sunucunun adı. Kullanım *afsvm101918* Bu öğretici için. |
   | **Path** | Oluşturduğunuz sürücü Windows Server yolu. Kullanım *f:\filestosync* bu öğreticideki. |
   | **Bulutta Katmanlama** | Bu öğretici için devre dışı bırakın. |
   | **Birim Boş Alanı** | Bu öğretici için boş bırakın. |

1. **Oluştur**’u seçin.

Dosyalarınız artık Azure dosya paylaşımında ve Windows Server’da eşitlenmiş durumdadır.

![Azure Depolama başarıyla eşitlendi](media/storage-sync-files-extend-servers/files-synced-in-azurestorage.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [storage-files-clean-up-portal](../../../includes/storage-files-clean-up-portal.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure dosya eşitleme'yi kullanarak bir Windows server depolama kapasitesini genişletmek için temel adımlarını öğrendiniz. Bir Azure dosya eşitleme dağıtımı planlama bir daha kapsamlı bakış için bkz:

> [!div class="nextstepaction"]
> [Azure Dosya Eşitleme dağıtımı planlama](./storage-sync-files-planning.md)
