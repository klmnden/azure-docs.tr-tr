---
title: 'Öğretici: Windows dosya sunucularını Azure Dosya Eşitleme ile genişletme | Microsoft Docs'
description: Windows dosya sunucularını başlangıçtan sona kadar Azure Dosya Eşitleme ile genişletme hakkında bilgi edinin.
services: storage
author: wmgries
ms.service: storage
ms.topic: tutorial
ms.date: 10/23/2018
ms.author: wgries
ms.component: files
ms.openlocfilehash: 3ebf450f4e84fed572307a18f20f36013e32c7a5
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/19/2018
ms.locfileid: "53630708"
---
# <a name="tutorial-extend-windows-file-servers-with-azure-file-sync"></a>Öğretici: Windows dosya sunucularını Azure Dosya Eşitleme ile genişletme
Bu öğreticide, Azure Dosya Eşitleme kullanarak bir Windows Server’ın depolama kapasitesini genişletmeye yönelik temel adımları göstereceğiz. Bu öğreticide bir Windows Server Azure sanal makinesi kullanılmasına karşın, bu işlemi genellikle şirket içi sunucularınız için yapacaksınız. Azure Dosya Eşitleme’yi kendi ortamınızda dağıtmaya hazırsanız, bunun yerine [Azure Dosya Eşitleme’yi Dağıtma](storage-sync-files-deployment-guide.md) makalesini kullanın.

> [!div class="checklist"]
> * Depolama Eşitleme Hizmetini Dağıtma
> * Windows Server’ı Azure Dosya Eşitleme ile kullanmaya hazırlama
> * Azure Dosya Eşitleme aracısını yükleme
> * Windows Server’ı Depolama Eşitleme Hizmetine kaydetme
> * Bir eşitleme grubu ve bir bulut uç noktası oluşturma
> * Sunucu uç noktası oluşturma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma
https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="prepare-your-environment"></a>Ortamınızı hazırlama
Azure Dosya Eşitleme’yi dağıtmadan önce bu öğretici için yapmanız gereken birkaç ayar vardır. Azure Depolama hesabı ve dosya paylaşımı oluşturmanın yanı sıra bir Windows Server 2016 Datacenter sanal makinesi oluşturacak ve bu sunucuyu Azure Dosya Eşitleme için hazırlayacaksınız.

### <a name="create-a-folder-and-txt-file"></a>Klasör ve .txt dosyası oluşturma

Yerel bilgisayarınızda *EşitlenecekDosyalar* adlı yeni bir klasör oluşturun ve *testbelgem.txt* adlı bir metin dosyası ekleyin. Bu dosyayı öğreticinin sonraki bölümlerinde dosya paylaşımına yükleyeceksiniz.

### <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

[!INCLUDE [storage-create-account-portal-include](../../../includes/storage-create-account-portal-include.md)]

### <a name="create-a-file-share"></a>Dosya paylaşımı oluşturma
Sonra, bir dosya paylaşımı oluşturacaksınız.

1. Azure depolama hesabı dağıtımı tamamlandığında **Kaynağa git**’e tıklayın.
1. Depolama hesabı bölmesinden **Dosyalar**’a tıklayın.

    ![Dosyalar’a tıklayın](./media/storage-sync-files-extend-servers/click-files.png)

1. **+ Dosya Paylaşımı**’na tıklayın.

    ![Dosya paylaşımı ekle düğmesine tıklayın](./media/storage-sync-files-extend-servers/create-file-share-portal2.png)

1. Yeni dosya paylaşımını *afsdosyapaylaşımı* olarak adlandırın ve **Kota** için "1" girip **Oluştur**’a tıklayın. Kota en fazla 5 TiB olabilir, ancak bu öğretici için yalnızca 1 GB gerekir.

    ![Yeni dosya paylaşımı için bir ad ve kota belirtin](./media/storage-sync-files-extend-servers/create-file-share-portal3.png)

1. Yeni dosya paylaşımını seçin, ardından dosya paylaşımı konumunda **Karşıya Yükle**’ye tıklayın.

    ![Dosyayı karşıya yükleme](./media/storage-sync-files-extend-servers/create-file-share-portal5.png)

1. .txt dosyanızı oluşturduğunuz *EşitlenecekDosyalar* klasörüne göz atın, *testbelgem.txt* dosyasını seçin ve **Karşıya Yükle**’ye tıklayın.

    ![Dosya paylaşımına göz atın](./media/storage-sync-files-extend-servers/create-file-share-portal6.png)

Bu noktada, bir Azure Depolama hesabı ve Azure’da, içinde bir dosya olan bir dosya paylaşımı oluşturdunuz. Şimdi bu öğreticide şirket içi sunucuyu temsil etmek üzere Windows Server 2016 Datacenter ile Azure sanal makinesini oluşturacaksınız.

### <a name="deploy-a-vm-and-attach-a-data-disk"></a>VM dağıtma ve veri diskini kullanıma açma

1. Ardından, portalın sol tarafındaki menüyü genişletin ve Azure portalın sol üst köşesindeki **Kaynak oluştur**’u seçin.
1. **Azure Market** kaynaklarının listesi üzerindeki arama kutusunda, **Windows Server 2016 Datacenter**’ı arayıp seçin, ardından **Oluştur**’u seçin.
1. **Temel Bilgiler** sekmesindeki **Proje ayrıntıları** altında, bu öğreticide oluşturduğunuz kaynak grubunu seçin.

   ![Portal dikey penceresinde VM’niz ile ilgili temel bilgileri girin](./media/storage-sync-files-extend-servers/vm-resource-group-and-subscription.png)

1. **Örnek ayrıntıları** altında *SanalMakinem* gibi bir VM adı belirtin.
1. **Bölge**, **Kullanılabilirlik seçenekleri**, **Görüntü** ve **Boyut** için varsayılan ayarları değiştirmeden bırakın.
1. **Yönetici hesabı** altında VM için bir **Kullanıcı adı** ve **Parola** girin.
1. **Gelen bağlantı noktası kuralları** altında **Seçilen bağlantı noktalarına izin ver**'i, sonra aşağı açılan listeden **RDP (3389)** ve **HTTP** değerlerini seçin.

   VM’yi oluşturmadan önce bir veri diski oluşturmanız gerekir.

1. **Sonraki: Diskler**’e tıklayın.

   ![Veri diski ekleme](./media/storage-sync-files-extend-servers/vm-add-data-disk.png)

1. **Diskler** sekmesindeki **Disk seçenekleri** altında varsayılan değerleri değiştirmeden bırakın.
1. **VERİ DİSKLERİ** altında **Yeni disk oluştur ve ekle**’ye tıklayın.

1. Bu öğreticide **Boyut (GiB)** ayarını **1 GB** olarak ayarlamak dışında varsayılan ayarları değiştirmeden bırakın.

   ![Veri diski ayrıntıları](./media/storage-sync-files-extend-servers/vm-create-new-disk-details.png)

1. **Tamam** düğmesine tıklayın.
1. **Gözden geçir ve oluştur**’a tıklayın.
1. **Oluştur**’a tıklayın.

   **Bildirimler** simgesine tıklayarak **Dağıtım ilerleme durumunu** izleyebilirsiniz. Yeni bir sanal makinenin oluşturulması birkaç dakika sürebilir.

1. VM dağıtımınız tamamlandıktan sonra **Kaynağa git**’e tıklayın.

   ![Kaynağa git](./media/storage-sync-files-extend-servers/vm-gotoresource.png)

   Bu noktada yeni bir sanal makine oluşturdunuz ve bir veri diskini kullanıma açtınız. Şimdi VM’ye bağlanmanız gerekir.

### <a name="connect-to-your-vm"></a>Sanal makinenize bağlanma

1. Azure portalda sanal makine özellikleri sayfasındaki **Bağlan**’a tıklayın.

   ![Portaldan bir Azure sanal makinesine bağlanma](./media/storage-sync-files-extend-servers/connect-vm.png)

1. **Sanal makineye bağlan** sayfasında, 3389 numaralı bağlantı noktası üzerinden **IP adresine** göre bağlanmak için varsayılan seçenekleri olduğu gibi bırakın ve **RDP dosyasını indir**’e tıklayın.

   ![RDP dosyasını indirme](./media/storage-sync-files-extend-servers/download-rdp.png)

1. İndirilen RDP dosyasını açın ve istendiğinde **Bağlan**’a tıklayın.
1. **Windows Güvenliği** penceresinde **Diğer seçenekler**'i ve ardından **Başka bir hesap kullanın**'ı seçin. Kullanıcı adını *localhost\kullanıcıadı* olarak yazın, sanal makine için oluşturduğunuz parolayı girin ve ardından **Tamam**’a tıklayın.

   ![Diğer seçenekler](./media/storage-sync-files-extend-servers/local-host2.png)

1. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Bağlantıyı oluşturmak için **Evet** veya **Devam**’a tıklayın.

### <a name="prepare-the-windows-server"></a>Windows Server’ı hazırlama
**Windows Server 2016 Datacenter** sunucusu için **Internet Explorer Artırılmış Güvenlik Yapılandırması**’nı devre dışı bırakın. Bu adım yalnızca ilk sunucu kaydı için gereklidir. Sunucu kaydedildikten sonra özelliği yeniden etkinleştirebilirsiniz.

**Windows Server 2016 Datacenter** sanal makinesinde **Sunucu Yöneticisi** otomatik olarak açılır.  **Sunucu Yöneticisi** varsayılan olarak açılmazsa, Explorer’da arama yapın.

1. **Sunucu Yöneticisi**’nde **Yerel Sunucu**’ya tıklayın.

   ![Sunucu Yöneticisi kullanıcı arabiriminin sol tarafındaki "Yerel Sunucu"](media/storage-sync-files-extend-servers/prepare-server-disable-ieesc-1.png)

1. **Özellikler** alt bölmesinde **IE Artırılmış Güvenlik Yapılandırması** bağlantısını seçin.  

    ![Sunucu Yöneticisi kullanıcı arabirimindeki "IE Artırılmış Güvenlik Yapılandırması" bölmesi](media/storage-sync-files-extend-servers/prepare-server-disable-ieesc-2.png)

1. **Internet Explorer Artırılmış Güvenlik Yapılandırması** iletişim kutusunda, **Yöneticiler** ve **Kullanıcılar** için **Kapalı** seçeneğini belirleyin.

    !["Kapalı" seçeneğinin işaretli olduğu Internet Explorer Artırılmış Güvenlik Yapılandırması açılır penceresi](media/storage-sync-files-extend-servers/prepare-server-disable-ieesc-3.png)

   Şimdi VM’ye veri diski ekleyebilirsiniz.

### <a name="add-the-data-disk"></a>Veri diski ekleme

1. **Windows Server 2016 Datacenter** sanal makinesindeyken **Dosyalar ve depolama hizmetleri** > **Birimler** > **Diskler**’e tıklayın.

    ![Veri diski](media/storage-sync-files-extend-servers/your-disk.png)

1. **Msft Virtual Disk** adlı 1 GB’lık diske sağ tıklayın ve **Yeni birim**’e tıklayın.
1. Varsayılan değerleri değiştirmeyip atanan sürücü harfini not ederek sihirbazı tamamlayın ve **Oluştur**’a tıklayın.
1. **Kapat**’a tıklayın.

   Bu noktada, diski çevrimiçi duruma getirdiniz ve bir birim oluşturdunuz. Sanal makinede Explorer’ı açıp yeni sürücünün mevcut olduğunu onaylayarak veri ekleme işleminin başarılı olduğunu doğrulayabilirsiniz.

1. Sanal makinede açtığınız Explorer’da **Bu Bilgisayar**’ı genişletin ve yeni sürücüye çift tıklayın. Bu örnekte yeni sürücü F: sürücüsüdür.
1. Sağ tıklayıp **Yeni** > **Klasör**’ü seçin. Klasörü *EşitlenecekDosyalar* olarak adlandırın.
1. **EşitlenecekDosyalar** klasörüne çift tıklayın.
1. Sağ tıklayıp **Yeni** > **Metin Belgesi**’ni seçin. Metin dosyasını *TestDosyam* olarak adlandırın.

    ![Yeni metin dosyası ekleme](media/storage-sync-files-extend-servers/new-file.png)

1. **Explorer**’ı ve **Sunucu Yöneticisi**’ni kapatın.

### <a name="download-the-azure-powershell-module"></a>Azure PowerShell modülünü indirin
Ardından **Windows Server 2016 Datacenter** VM yükleme **Azure PowerShell Modülü** sunucusunda.

1. Bir VM’de, yükseltilmiş bir PowerShell penceresi açın
1. Şu komutu çalıştırın:

   ```powershell
   Install-Module -Name Az -AllowClobber
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

`Az` modülü, Azure PowerShell cmdlet’leri için toplu bir modüldür. Bu modülü yüklediğinizde kullanılabilir durumdaki tüm Azure Resource Manager modülleri indirilir ve cmdlet’leri kullanıma sunulur.

Bu noktada, öğretici için ortamınızı ayarlamayı tamamladınız ve **Depolama Eşitleme Hizmeti**’ni dağıtmaya başlamak için hazırsınız.

## <a name="deploy-the-service"></a>Hizmeti dağıtma 
Azure Dosya Eşitleme’yi dağıtma işleminde ilk olarak **Depolama Eşitleme Hizmeti** kaynağını, seçtiğiniz abonelikte bulunan bir kaynak grubuna yerleştirirsiniz. Depolama Eşitleme Hizmeti, abonelikten ve hizmeti dağıttığınız kaynak grubundan erişim izinlerini devralır.

1. Azure portalda **Kaynak oluştur**’a tıklayın ve sonra **Azure Dosya Eşitleme**’yi arayın.
1. Arama sonuçlarında **Azure Dosya Eşitleme**’yi seçin.
1. **Oluştur**’u seçerek **Depolama Eşitleme’yi Dağıt** sekmesini açın.

   ![Depolama Eşitleme’yi Dağıtma](media/storage-sync-files-extend-servers/afs-info.png)

   Açılan bölmeye aşağıdaki bilgileri girin:

   | Değer | Açıklama |
   | ----- | ----- |
   | **Ad** | Depolama Eşitleme Hizmeti için benzersiz bir ad (abonelik başına).<br><br>Bu öğreticide *afseşitlemehizmeti02* kullanılmaktadır. |
   | **Abonelik** | Bu öğretici için kullandığınız abonelik. |
   | **Kaynak grubu** | Bu öğretici için kullandığınız kaynak grubu.<br><br>Bu öğreticide *afskaynakgrubu101918* kullanılmaktadır. |
   | **Konum** | Doğu ABD |

1. İşiniz bittiğinde **Oluştur**’u seçerek **Depolama Eşitleme Hizmeti**’ni dağıtın.
1. **Bildirimler** sekmesi > **Kaynağa git** öğesine tıklayın.

## <a name="install-the-agent"></a>Aracıyı yükleme
Azure Dosya Eşitleme aracısı, Windows Server’ın bir Azure dosya paylaşımı ile eşitlenmesini sağlayan indirilebilir bir pakettir.

1. **Windows Server 2016 Datacenter** sanal makinesine geri dönüp **Internet Explorer**’ı açın.
1. [Microsoft İndirme Merkezi](https://go.microsoft.com/fwlink/?linkid=858257)’ne gidin. Ekranı **Azure Dosya Eşitleme Aracısı** bölümüne kaydırın ve **İndir**’e tıklayın.

   ![Eşitleme aracısını indirme](media/storage-sync-files-extend-servers/sync-agent-download.png)

1. **StorageSyncAgent_V3_WS2016.EXE** kutusunu işaretleyip **İleri**’ye tıklayın.

   ![Aracı seçme](media/storage-sync-files-extend-servers/select-agent.png)

1. Dosya için **Bir kez izin ver** > **Çalıştır** > **Aç**’a tıklayın.
1. Henüz yapmadıysanız, PowerShell penceresini kapatın.
1. **Depolama Eşitleme Aracısı Kurulum Sihirbazı**’nda varsayılan ayarları kabul edin.
1. **Yükle**'ye tıklayın.
1. **Son**'a tıklayın.

Azure Eşitleme Hizmeti’ni dağıttınız ve aracıyı **Windows Server 2016 Datacenter** sanal makinesine yüklediniz. Şimdi VM’yi **Depolama Eşitleme Hizmeti**’ne kaydetmeniz gerekir.

## <a name="register-windows-server"></a>Windows Server’ı kaydetme
Windows Server’ı bir Depolama Eşitleme Hizmeti’ne kaydetmek, sunucunuz (veya kümeniz) ile Depolama Eşitleme Hizmeti arasında bir güven ilişkisi kurar. Bir sunucu yalnızca bir Depolama Eşitleme Hizmeti’ne kaydedilebilir ve aynı Depolama Eşitleme Hizmeti ile ilişkili diğer sunucular ve Azure dosya paylaşımları ile eşitlenebilir.

**Azure Dosya Eşitleme aracısı** yüklendikten sonra Sunucu Kaydı kullanıcı arabirimi otomatik olarak açılmalıdır. Seçili değilse bunu dosya konumundan el ile açabilirsiniz: C:\Program Files\Azure\StorageSyncAgent\ServerRegistration.exe.

1. Sunucu Kaydı kullanıcı arabirimi sanal makinede açıldığında **Tamam**’a tıklayın.
1. Başlamak için **Oturum açın**’a tıklayın.
1. Azure hesabı kimlik bilgilerinizi girin ve **Oturum açın**’a tıklayın.
1. Şu bilgileri belirtin:

   ![Sunucu Kaydı kullanıcı arabiriminin ekran görüntüsü](media/storage-sync-files-extend-servers/signin.png)

   | | |
   | ----- | ----- |
   | Değer | Açıklama |
   | **Azure Aboneliği** | Bu öğretici için Depolama Eşitleme Hizmetini içeren abonelik. |
   | **Kaynak Grubu** | Bu öğretici için Depolama Eşitleme Hizmetini içeren kaynak grubu. Bu öğreticide *afskaynakgrubu101918* kullanılmaktadır. |
   | **Depolama Eşitleme Hizmeti** | Bu öğreticide kullandığınız Depolama Eşitleme Hizmetinin adı. Bu öğreticide *afseşitlemehizmeti02* kullanılmaktadır. |

1. Sunucu kaydını tamamlamak için **Kaydet**’e tıklayın.
1. Kayıt işleminin bir parçası olarak bir kez daha oturum açmanız istenir. Oturum açın ve **İleri**’ye tıklayın.
1. **Tamam** düğmesine tıklayın.

## <a name="create-a-sync-group"></a>Eşitleme grubu oluşturma
Eşitleme grubu, bir dosya kümesi için eşitleme topolojisini tanımlar. Eşitleme grubu, bir Azure dosya paylaşımı ve en az bir sunucu uç noktasını temsil eden bir bulut uç noktası içermelidir. Sunucu uç noktası, kayıtlı bir sunucu üzerindeki bir yolu temsil eder.

1. Bir eşitleme grubu oluşturmak için, [Azure portalda](https://portal.azure.com/) bu öğreticide oluşturduğunuz Depolama Eşitleme Hizmeti’nden **+ Eşitleme grubu**’nu seçin. Bu öğreticide örnek olarak *afseşitlemehizmeti02* kullanılmıştır.

   ![Azure portalda yeni bir eşitleme grubu oluşturma](media/storage-sync-files-extend-servers/add-sync-group.png)

1. Açılan bölmede, bulut uç noktası olan bir eşitleme grubu oluşturmak için aşağıdaki bilgileri girin:

   | Değer | Açıklama |
   | ----- | ----- |
   | **Eşitleme grubu adı** | Bu ad Depolama Eşitleme Hizmetinde benzersiz olmalıdır, ancak size mantıklı gelen herhangi bir ad olabilir. Bu öğreticide *afseşitlemegrubu* kullanılmaktadır.|
   | **Abonelik** | Bu öğretici için Depolama Eşitleme Hizmetini dağıttığınız abonelik. |
   | **Depolama hesabı** |**Depolama hesabı seçin**’e tıklayın. Görüntülenen bölmede, bu öğretici için oluşturduğunuz Azure dosya paylaşımını içeren depolama hesabını seçin. Burada *afsdepolamahesabı101918* kullanılmıştır. |
   | **Azure dosya paylaşımı** | Bu öğretici için oluşturduğunuz Azure dosya paylaşımının adı. Burada *afsdosyapaylaşımı* kullanılmıştır. |

1. **Oluştur**’a tıklayın.

Eşitleme grubunuzu seçerseniz, artık bir **bulut uç noktanızın** olduğunu görebilirsiniz.

## <a name="add-a-server-endpoint"></a>Sunucu uç noktası ekleme
Sunucu uç noktası, bir sunucu birimi üzerindeki klasör gibi kayıtlı bir sunucu üzerindeki belirli bir noktayı temsil eder.

1. Sunucu uç noktası eklemek için, yeni oluşturulan eşitleme grubunu ve sonra **Sunucu uç noktası ekle**’yi seçin.

   ![Eşitleme grubu bölmesine yeni bir sunucu uç noktası ekleme](media/storage-sync-files-extend-servers/add-server-endpoint.png)

1. **Sunucu uç noktası ekle** bölmesinde bir sunucu uç noktası oluşturmak için aşağıdaki bilgileri girin:

   | | |
   | ----- | ----- |
   | Değer | Açıklama |
   | **Kayıtlı sunucu** | Bu öğretici için oluşturduğunuz sunucunun adı. Bu öğreticide *afsvm101918* kullanılmıştır |
   | **Path** | Bu öğretici için oluşturduğunuz sürücünün Windows Server yolu. Bu örnekte *f:\eşitlenecekdosyalar* şeklindedir. |
   | **Bulutta Katmanlama** | Bu öğretici için devre dışı bırakın. |
   | **Birim Boş Alanı** | Bu öğretici için boş bırakın. |

1. **Oluştur**’a tıklayın.

Dosyalarınız artık Azure dosya paylaşımında ve Windows Server’da eşitlenmiş durumdadır.

![Azure Depolama başarıyla eşitlendi](media/storage-sync-files-extend-servers/files-synced-in-azurestorage.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [storage-files-clean-up-portal](../../../includes/storage-files-clean-up-portal.md)]

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, Azure Dosya Eşitleme kullanarak bir Windows Server’ın depolama kapasitesini genişletmeye yönelik temel adımları öğrendiniz. Bir Azure Dosya Eşitleme dağıtımını planlamaya daha ayrıntılı bir bakış için bu bağlantıyı izleyin.

> [!div class="nextstepaction"]
> [Azure Dosya Eşitleme dağıtımı planlama](./storage-sync-files-planning.md)
