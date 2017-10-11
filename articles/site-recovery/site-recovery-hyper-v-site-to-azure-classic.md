---
title: "Hyper-V sanal makineleri Klasik portalda Azure'a çoğaltma | Microsoft Docs"
description: "Bu makalede, makineler VMM bulutlarında yönetilmeyen, Hyper-V sanal makineleri azure'a açıklar."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: 3f4c4483-e3dd-495a-bd02-c16e9e28c88d
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 02/21/2017
ms.author: raynew
ms.openlocfilehash: 438f32ee3605e2dd0c46de7993a359cc269262fe
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="replicate-between-on-premises-hyper-v-virtual-machines-and-azure-without-vmm-with-azure-site-recovery"></a>Şirket içi Hyper-V sanal makineleri (VMM olmadan) Azure Site Recovery ile Azure arasındaki çoğaltma
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-hyper-v-site-to-azure.md)
> * [PowerShell - Resource Manager](site-recovery-deploy-with-powershell-resource-manager.md)
> * [Klasik Portal](site-recovery-hyper-v-site-to-azure-classic.md)
>
>

Bu makale, Azure için şirket içi Hyper-V sanal makineleri çoğaltmak açıklamaktadır kullanarak [Azure Site Recovery](site-recovery-overview.md) service, Azure portalında. Bu senaryoda, Hyper-V sunucuları VMM bulutlarında yönetilen değil.

Bu makaleyi okuduktan sonra altındaki bir yorum gönderin ya da teknik sorular [Azure kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="site-recovery-in-the-azure-portal"></a>Azure portalında Site Recovery

Azure, kaynak oluşturmak ve bu kaynaklarla çalışmak için iki [dağıtım modeli](../resource-manager-deployment-model.md) kullanır: Azure Resource Manager ve klasik model. Azure’da ayrıca iki portal bulunur: Klasik Azure portalı ve Azure portalı.

Bu makale, Klasik portalda dağıtmayı açıklar. Klasik portal, mevcut kasaları korumak için kullanılabilir. Klasik portalı kullanarak yeni kasalar oluşturamazsınız.

## <a name="site-recovery-in-your-business"></a>İşletmenizde Site Recovery

Kuruluşlar, planlı ve plansız kesinti süreleri boyunca uygulamalarla verilerin çalışır durumda ve kullanılabilir olmasına yönelik izlenecek yolu belirleyip mümkün olan en kısa sürede normal çalışma koşullarına dönmeyi sağlayan bir BCDR stratejisine gereksinim duyar. Site Recovery'nin sağladığı avantajlar şunlardır:

* Hyper-V VM’lerinde çalışan iş uygulamaları için şirket dışında koruma.
* Çoğaltma, yük devretme ve kurtarma işlemlerini izlemek, yönetmek ve ayarlamak için tek bir konum.
* Azure'a kolayca yük devretme ve Azure'dan şirket içi konumunuzdaki Hyper-V konak sunucularından yeniden çalışma (kurtarma).
* Katmanlı uygulama iş yüklerinin birlikte yük devredebilmesi için birden çok VM içeren kurtarma planları.

## <a name="azure-prerequisites"></a>Azure önkoşulları
* Bir [Microsoft Azure](https://azure.microsoft.com/) hesabınızın olması gerekir. [Ücretsiz deneme sürümüyle](https://azure.microsoft.com/pricing/free-trial/) başlayabilirsiniz.
* Çoğaltılan verileri depolamak için bir Azure depolama hesabınızın olması gerekir. Hesap coğrafi çoğaltmanın etkinleştirilmiş olmalıdır. Bu Azure Site Recovery kasasıyla aynı bölgede olması ve aynı abonelikle ilişkilendirilmiş olması gerekir. [Azure storage hakkında daha fazla bilgi](../storage/common/storage-introduction.md). Taşıma depolama hesapları kullanılarak oluşturulan desteklemiyoruz Not [yeni Azure portalına](../storage/common/storage-create-storage-account.md) kaynak grupları arasında.
* Böylece, birincil sitenizden üzerinden başarısız olduğunda Azure sanal makineleri bir ağa bağlı bir Azure sanal ağı gerekir.

## <a name="hyper-v-prerequisites"></a>Hyper-V önkoşulları
* Kaynak şirket içi sitede çalıştıran bir veya daha fazla sunucuya ihtiyacınız olacak **Windows Server 2012 R2** Hyper-V rolü yüklü olan veya **Microsoft Hyper-V Server 2012 R2**. Bu sunucu gerekir:
* Bir veya daha fazla sanal makine içeriyor.
* Internet'e doğrudan veya bir proxy üzerinden bağlı.
* KB açıklanan düzeltmeler çalıştırması [2961977](https://support.microsoft.com/en-us/kb/2961977 "KB2961977").

## <a name="virtual-machine-prerequisites"></a>Sanal makine önkoşulları
Korumak istediğiniz sanal makineleri ile uygun olması [Azure sanal makine gereksinimlerini](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

## <a name="provider-and-agent-prerequisites"></a>Sağlayıcı ve aracı önkoşulları
Azure Site Recovery dağıtımının bir parçası her Hyper-V sunucusunda Azure Site kurtarma Sağlayıcısı'nı ve Azure kurtarma Hizmetleri Aracısı'nı yükleyeceksiniz. Şunlara dikkat edin:

* Her zaman en son sürümlerini sağlayıcı ve aracı çalıştırmanızı öneririz. Bu Site kurtarma Portalı'nda kullanılabilir.
* Bir kasadaki tüm Hyper-V sunucularında aynı sağlayıcı ve Aracı sürümleri olması gerekir.
* Sunucuda çalışan sağlayıcı internet üzerinden Site Recovery hizmetine bağlanır. Bir ara sunucu olmadan Hyper-V sunucusunda yapılandırılmış proxy ayarlarıyla ya da sağlayıcı yüklemesi sırasında yapılandırırsınız özel proxy ayarlarıyla bunu yapabilirsiniz. Kullanmak istediğiniz proxy sunucusu bu URL'leri Azure'a bağlanmak için erişim sağlayabildiğinizden emin olmak gerekir:

  * *.accesscontrol.windows.net
  * *.backup.windowsazure.com
  * *.hypervrecoverymanager.windowsazure.com
  * *.store.core.windows.net      
  * *.blob.core.windows.net
  - https://www.msftncsi.com/ncsi.txt
  - time.windows.com
  - time.nist.gov
* Ayrıca açıklanan IP adreslerinin izin [Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653) ve HTTPS (443) protokolü. Kullanmayı düşündüğünüz Azure bölgesinin IP aralıklarını ve, Batı ABD geçmesine izin vermelisiniz.

Bu grafik düzenleme ve çoğaltma için Site Recovery tarafından kullanılan bağlantı noktaları ve farklı iletişim kanalları gösterir

![B2A topolojisi](./media/site-recovery-hyper-v-site-to-azure-classic/b2a-topology.png)

## <a name="step-1-create-a-vault"></a>1. adım: bir kasa oluşturma
1. Oturum [Yönetim Portalı](https://portal.azure.com).
2. Genişletme **Veri Hizmetleri** > **kurtarma Hizmetleri** tıklatıp **Site Recovery kasası**.
3. **Yeni Oluştur** > **Hızlı Oluştur**'a tıklayın.
4. **Ad** alanına kasayı tanımlamak için kolay bir ad girin.
5. **Bölge** içinde, kasa için coğrafi bölgeyi seçin. Desteklenen bölgeleri kontrol etmek için [Azure Site Recovery Fiyatlandırma Ayrıntıları](https://azure.microsoft.com/pricing/details/site-recovery/) bölümünde Coğrafi Kullanılabilirlik kısmına bakın.
6. **Kasa Oluştur**'a tıklayın.

    ![Yeni kasa](./media/site-recovery-hyper-v-site-to-azure-classic/vault.png)

Kasanın başarıyla oluşturulduğunu doğrulamak için durum çubuğunu kontrol edin. Kasa, ana Kurtarma Hizmetleri sayfasında **Etkin** olarak listelenir.

## <a name="step-2-create-a-hyper-v-site"></a>2. adım: bir Hyper-V sitesi oluşturma
1. Kurtarma Hizmetleri sayfasında, hızlı başlangıç sayfasını açmak için kasaya tıklayın. Ayrıca, Hızlı Başlangıç sayfasını istediğiniz zaman simgesini kullanarak da açabilirsiniz.

    ![Hızlı Başlangıç](./media/site-recovery-hyper-v-site-to-azure-classic/quick-start-icon.png)
2. Aşağı açılan listesinde seçin **bir şirket içi Hyper-V sitesi ile Azure arasında**.

    ![Hyper-V sitesi senaryosu](./media/site-recovery-hyper-v-site-to-azure-classic/select-scenario.png)
3. İçinde **bir Hyper-V sitesi oluşturmak** tıklatın **oluşturma Hyper-V sitesi**. Bir site adı belirtin ve kaydedin.

    ![Hyper-V sitesi](./media/site-recovery-hyper-v-site-to-azure-classic/create-site.png)

## <a name="step-3-install-the-provider-and-agent"></a>3. adım: sağlayıcı ve aracı yükleme
Sağlayıcı ve aracının korumak istediğiniz sanal makineleri olan her Hyper-V sunucusuna yükleyin.

Hyper-V kümesi üzerinde yüklüyorsanız, yük devretme kümesindeki her düğümde 5-11 adımları gerçekleştirir. Bunlar kümedeki düğümleri arasında geçirmek olsa bile tüm düğümleri kaydedilir ve koruma etkinleştirildikten sonra sanal makineler korunur.

1. İçinde **hazırlama Hyper-V sunucuları**, tıklatın **bir kayıt anahtarı indirin** dosya.
2. Üzerinde **kayıt anahtarını indir** sayfasında, **karşıdan** site yanındaki. Hyper-V sunucusu tarafından kolayca erişilebilen güvenli bir konuma anahtarını indirin. Anahtar oluşturulduktan sonra 5 gün boyunca geçerlidir.

    ![Kayıt anahtarı](./media/site-recovery-hyper-v-site-to-azure-classic/download-key.png)
3. Tıklatın **sağlayıcıyı indirmek** en son sürümü edinmek için.
4. Kasaya kaydetmek istediğiniz her Hyper-V sunucusunda dosyasını çalıştırın. Dosya iki bileşenleri yükler:
   * **Azure Site Recovery sağlayıcısı**— iletişim ve Hyper-V sunucusu ve Azure Site Recovery portalı arasında orchestration işler.
   * **Azure kurtarma Hizmetleri Aracısı**— işler kaynak Hyper-V sunucusu ve Azure depolama üzerinde çalışan sanal makineler arasında veri taşıma.
5. **Microsoft Update** kısmında güncelleştirmeleri seçebilirsiniz. Bu ayar etkinse, sağlayıcı ve aracı güncelleştirmeleri, Microsoft Update ilkenize uygun olarak yüklenir.

    ![Microsoft Güncelleştirmeleri](./media/site-recovery-hyper-v-site-to-azure-classic/provider1.png)
6. İçinde **yükleme** Hyper-V sunucusunda sağlayıcı ve aracı yüklemek istediğiniz yeri belirtin.

    ![Yükleme konumu](./media/site-recovery-hyper-v-site-to-azure-classic/provider2.png)
7. Yükleme tamamlandıktan sonra sunucuyu kasaya kaydetmek için kuruluma devam edin.
8. Üzerinde **kasa ayarlarını** sayfasında, **Gözat** anahtar dosyası seçin. Azure Site Recovery abonelik, kasa adını ve Hyper-V sunucusuna ait olduğu Hyper-V sitesi belirtin.

    ![Sunucu kaydı](./media/site-recovery-hyper-v-site-to-azure-classic/provider8.PNG)
9. Üzerinde **Internet bağlantısı** sayfası sağlayıcı Azure Site Recovery'e nasıl bağlandığını belirtin. Sunucuda yapılandırılan varsayılan İnternet bağlantısı ayarlarını kullanmak için **Varsayılan sistem ara sunucu ayarlarını kullan**'a tıklayın. Bir değer belirtmezseniz varsayılan ayarlar kullanılır.

   ![İnternet Ayarları](./media/site-recovery-hyper-v-site-to-azure-classic/provider7.PNG)
10. Sunucuyu kasaya kaydetmek kayıt başlar.

    ![Sunucu kaydı](./media/site-recovery-hyper-v-site-to-azure-classic/provider15.PNG)
11. Meta veri kayıt tamamlandıktan sonra Hyper-V server Azure Site Recovery tarafından alınır ve sunucu gösterilir **Hyper-V sitelerini** sekmesi **sunucuları** kasası sayfasında.

### <a name="install-the-provider-from-the-command-line"></a>Komut satırından sağlayıcıyı yükleyin
Alternatif olarak, Azure Site Recovery sağlayıcısı komut satırından yükleyebilirsiniz. Sağlayıcı Windows 2012 R2 Sunucu Çekirdeği çalıştıran bir bilgisayara yüklemek istiyorsanız, bu yöntem kullanmanız gerekir. Komut satırından aşağıdaki gibi çalıştırın:

1. Sağlayıcı yükleme dosyasını ve kayıt anahtarını bir klasöre indirin. Örneğin, C:\ASR.
2. Bir komut istemi, bir yönetici ve türü olarak çalıştırın:

        C:\Windows\System32> CD C:\ASR
        C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
3. Sonra sağlayıcının çalıştırarak yükleyin:

        C:\ASR> setupdr.exe /i
4. Kayıt işlemini tamamlamak için aşağıdaki komutu çalıştırın:

        CD C:\Program Files\Microsoft Azure Site Recovery Provider
        C:\Program Files\Microsoft Azure Site Recovery Provider\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>         

Parametreler burada şunları içerir:

* **/ Credentials**: indirdiğiniz kayıt anahtarı konumunu belirtin.
* **/ FriendlyName**: Hyper-V konak sunucusu tanımlamak için bir ad belirtin. Bu ad Portalı'nda görünür
* **/ EncryptionEnabled**: isteğe bağlı. Çoğaltma sanal makineleri azure'da (Bekleyen şifreleme) şifrelemek isteyip istemediğinizi belirtin.
* **/ proxyaddress**; **/proxyport**; **/proxyusername**; **/proxypassword**: isteğe bağlı. Özel bir ara sunucu kullanmak istediğiniz veya var olan ara sunucunuz kimlik doğrulaması gerektiriyorsa proxy parametrelerini belirtin.

## <a name="step-4-create-an-azure-storage-account"></a>4. Adım: Azure depolama hesabı oluşturma
* İçinde **kaynakları hazırla** seçin **depolama hesabı oluştur** , yoksa, Azure storage hesabı oluşturmak için. Hesabı coğrafi çoğaltmanın etkinleştirilmiş olması gerekir. Azure Site Recovery kasasıyla aynı bölgede olması ve aynı abonelikle ilişkilendirilmiş olması.

    ![Depolama hesabı oluşturma](./media/site-recovery-hyper-v-site-to-azure-classic/create-resources.png)

> [!NOTE]
> 1. Depolama hesapları kullanılarak oluşturulan taşıma desteklemiyoruz [yeni Azure portalına](../storage/common/storage-create-storage-account.md) kaynak grupları arasında.
> 2. Site Recovery dağıtımında kullanılan depolama hesapları için aynı abonelik içindeki kaynak grupları arasında veya abonelik arasında [depolama hesapları geçişi](../azure-resource-manager/resource-group-move-resources.md) desteklenmez.
>

## <a name="step-5-create-and-configure-protection-groups"></a>Adım 5: Oluşturma ve koruma gruplarını yapılandırma
Koruma grupları aynı koruma ayarlarını kullanarak korumak istediğiniz sanal makinelerin mantıksal gruplandırmaları olan. Bir koruma grubu için koruma ayarları uygulamak ve bu ayarları gruba eklediğiniz tüm sanal makinelere uygulanır.

1. İçinde **oluşturma ve koruma gruplarını yapılandırma** tıklatın **koruma grubu oluşturma**. Tüm Önkoşullar yerinde yoksa bir ileti verilir ve tıklayabilirsiniz **ayrıntıları görüntüleyin** daha fazla bilgi için.
2. İçinde **koruma grupları** sekmesinde, bir koruma grubu ekleyin. Bir ad, kaynak Hyper-V sitesi, hedef belirtin **Azure**, Azure Site Recovery abonelik adınızı ve Azure depolama hesabı.

    ![Koruma grubu](./media/site-recovery-hyper-v-site-to-azure-classic/protection-group.png)
3. İçinde **çoğaltma ayarları** ayarlamak **kopyalama sıklığı** veri değişim kaynak ve hedef arasında ne sıklıkla eşitlenmesi gerektiği belirtmek için. 30 saniye, 5 dakika veya 15 dakika ayarlayabilirsiniz.
4. İçinde **kurtarma noktalarını korumak** kurtarma geçmişini kaç saat saklanması gerektiğini belirtin.
5. İçinde **uygulamayla tutarlı anlık görüntü sıklığı** anlık görüntü alınırken uygulamaların tutarlı bir durumda olduğundan emin olmak için Birim Gölge Kopyası Hizmeti (VSS) kullanan anlık görüntülerini almak belirtebilirsiniz. Varsayılan olarak bu gerçekleştirilecek değil. Bu değer, yapılandırdığınız ilave kurtarma noktası sayısından daha az şekilde ayarlandığından emin olun. Bu, yalnızca sanal makineyi bir Windows işletim sistemi çalıştırıyorsa desteklenir.
6. İçinde **ilk çoğaltma başlangıç zamanı** koruma grubundaki sanal makinelerin ilk çoğaltmasının Azure'a ne zaman gönderilmesi gerektiğini belirtin.

    ![Koruma grubu](./media/site-recovery-hyper-v-site-to-azure-classic/protection-group2.png)

## <a name="step-6-enable-virtual-machine-protection"></a>6. adım: sanal makine korumayı etkinleştirin
Sanal makineler bunları korumayı etkinleştirmek için bir koruma grubuna ekleyin.

> [!NOTE]
> Statik bir IP adresiyle Linux çalıştıran VM'leri koruma işlemi desteklenmez.
>
>

1. Üzerinde **makineler** koruma grubu için sekmesi, tıklatın ** Ekle sanal makineleri koruma ** etkinleştirmek için koruma grupları için.
2. Üzerinde **sanal makine korumayı etkinleştir** sayfasını korumak istediğiniz sanal makineleri seçin.

    ![Sanal makine korumasını etkinleştirme](./media/site-recovery-hyper-v-site-to-azure-classic/add-vm.png)

    Korumayı etkinleştir işleri başlar. Üzerinde ilerleme durumunu izleyebilirsiniz **işleri** sekmesi. Korumayı Sonlandır işini çalıştıktan sonra sanal makine yük devretme için hazır olur.
3. Sonra koruma olduğundan, ayarlama yapabilirsiniz:

   * Sanal makinelerde görüntülemek **korunan öğeler** > **koruma grupları** > *protectiongroup_name* > **sanal makineleri** makine ayrıntılarında aşağıya doğru inebilir **özellikleri** sekmesini..
   * Bir sanal makineler için yük devretme özelliklerini yapılandırmak **korunan öğeler** > **koruma grupları** > *protectiongroup_name* > **sanal makineleri** *virtual_machine_name* > **yapılandırma**. Aşağıdakileri yapılandırabilirsiniz:

     * **Ad**: azure'da sanal makine adı.
     * **Boyutu**: yöneltilir sanal makinenin hedef boyutunu.

       ![Sanal makine özelliklerini yapılandırın](./media/site-recovery-hyper-v-site-to-azure-classic/vm-properties.png)
   * Ek sanal makine ayarlarında yapılandırdığınız *korunan öğeler** > **koruma grupları** > *protectiongroup_name* > **sanal makineleri** *virtual_machine_name* > **yapılandırma**dahil:

     * **Ağ bağdaştırıcıları**: ağ bağdaştırıcılarının sayısı hedef sanal makine için belirlediğiniz boyuta göre dikte edilir. Denetleme [sanal makine boyutu özellikleri](../virtual-machines/linux/sizes.md) sanal makine boyutu tarafından desteklenen NIC sayısı.

       Bir sanal makine için boyutu değiştirip ayarları kaydettiğinizde, **Yapılandır** sayfasını bir sonraki açışınızda ağ bağdaştırıcısı sayısı değişir. Hedef sanal makinelerin ağ bağdaştırıcısı sayısı en düşük kaynak sanal makinedeki ağ bağdaştırıcısı sayısını ve seçilen sanal makine boyutu tarafından desteklenen ağ bağdaştırıcıların maksimum sayısı. Aşağıda açıklanan:

       * Kaynak makinedeki ağ bağdaştırıcılarının sayısı, hedef makine boyutu için verilen ağ bağdaştırıcısı sayısına eşitse veya daha azsa hedef makine kaynakla aynı sayıda bağdaştırıcıya sahip olur.
       * Kaynak sanal makinenin bağdaştırıcı sayısı, hedef boyut için izin verilen sayıyı aşarsa maksimum hedef boyutu kullanılır.
       * Örneğin, kaynak makinenin iki bağdaştırıcısı varsa ve hedef makine boyutu dört adet bağdaştırıcıyı destekliyorsa hedef makinenin iki bağdaştırıcısı olur. Kaynak makinenin iki bağdaştırıcısı varken hedef boyut yalnızca bir bağdaştırıcıyı destekliyorsa hedef makinenin bir bağdaştırıcısı olur.

     * **Azure ağı**: için sanal makine başarısız olmalıdır üzerinden ağ belirtin. Sanal makine varsa, tüm bağdaştırıcıların gereken birden çok ağ bağdaştırıcısı aynı Azure ağına bağlı.
     * **Alt ağ** sanal makinedeki her ağ bağdaştırıcısı için alt ağ için makine bağlanma yük devretme sonrasında Azure ağı seçin.
     * **Hedef IP adresi**: kaynak sanal makinenin ağ bağdaştırıcısı statik bir IP adresi makinenin yük devretme sonrasında aynı IP adresi olduğundan emin olmak hedef sanal makine için IP adresi belirtip kullanmak için yapılandırılmışsa.  Bir IP adresi belirtmezseniz herhangi bir kullanılabilir adresi yük devretme sırasında atanır. Kullanımda olan bir adresi belirtirseniz, yük devretme başarısız olur.

     > [!NOTE]
     > Site Recovery dağıtımında kullanılan ağlar için aynı abonelik içindeki kaynak grupları arasında veya abonelik arasında [ağ geçişi](../azure-resource-manager/resource-group-move-resources.md) desteklenmez.
     >

     ![Sanal makine özelliklerini yapılandırın](./media/site-recovery-hyper-v-site-to-azure-classic/multiple-nic.png)




## <a name="step-7-create-a-recovery-plan"></a>7. adım: bir kurtarma planı oluşturma
Dağıtımı test etmek için bir yük devretme sınaması için tek bir sanal makine ya da bir veya daha fazla sanal makine içeren bir kurtarma planı çalıştırabilirsiniz. [Daha fazla bilgi edinin](site-recovery-create-recovery-plans.md) bir kurtarma planı oluşturma hakkında daha fazla.

## <a name="step-8-test-the-deployment"></a>8. adım: dağıtımı test etme
Azure için yük devretme testi çalıştırmanın iki yöntemi vardır.

* **Azure ağı olmadan yük devretme testi** - Bu türdeki yük devretme testi sanal makinenin Azure'a doğru şekilde alınıp alınmadığını denetler. Yük devretmeden sonra sanal makine herhangi bir Azure ağına bağlanmaz.
* **Azure ağıyla yük devretme testi**— bu türdeki yük devretme denetimleri tüm çoğaltma ortamının istendiği şekilde geldiğini ve, devredilen sanal makinelerin belirtilen hedef Azure ağını bağlanır. Yük devretme sınaması için test sanal makinenin çoğaltma sanal makinesinin alt ağda göre belirlenir olduğunu unutmayın. Bu işlem, çoğaltılan sanal makinenin alt ağının kaynak sanal makinenin alt ağına bağlı olduğunda normal çoğaltmadan farklıdır.

Bir Azure ağı belirtmeden bir yük devretme testi çalıştırmak isterseniz herhangi bir şeyi hazırlamanız gerekmez.

Hedef Azure ağıyla bir yük devretme testi çalıştırmak isterseniz Azure üretim ağınızdan yalıtılmış olan yeni bir Azure ağı oluşturmanız gerekir (Azure'da yeni bir ağ oluştururken, varsayılan davranıştır). Okuma [bir yük devretme testi](site-recovery-failover.md) daha fazla ayrıntı için.

Çoğaltma ve ağ dağıtımınızı tam olarak test etmek için altyapıyı olarak çalışmak için çoğaltılan sanal makinenin beklenen şekilde ayarlamak gerekir. Bir sanal makine kurun DNS etki alanı denetleyicisi olarak ve yük devretme testi çalıştırarak test ağında oluşturmak için Site RECOVERY'yi kullanarak Azure'a çoğaltma için için bunu yapmanın bir yolu.  [Daha fazla bilgi](site-recovery-active-directory.md#test-failover-considerations) Active Directory için test yük devretme konusunda.

Yük devretme testi çalıştırmak için şunları yapın:

> [!NOTE]
> Azure'a yük devretme sırasında en iyi performansı elde etmek için korumalı makineye Azure Aracısı'nı yüklediğinizden emin olun. Bu işlem, daha hızlı önyükleme yapılmasına ve sorunların meydana gelmesi durumunda tanılama işlemine yardımcı olur. Linux aracısına [buradan](https://github.com/Azure/WALinuxAgent) ve Windows aracısına da [buradan](http://go.microsoft.com/fwlink/?LinkID=394789) ulaşabilirsiniz.
>
>

1. **Kurtarma Planları** sekmesinde planı seçin ve **Yük devretme Testi**'ne tıklayın.
2. **Yük Devretme Testi Onaylama** sayfasında **Hiçbiri** veya özel Azure ağı seçimlerini belirleyin.  Seçtiğiniz gerçekleştiriyorsanız **hiçbiri** yük devretme testi sanal makinenin Azure'a doğru şekilde kopyalanan ancak çoğaltma ağı yapılandırmasını denetlemez kontrol eder.

    ![Yük devretme sınaması](./media/site-recovery-hyper-v-site-to-azure-classic/test-nonetwork.png)
3. **İşler** sekmesinden yük devretme işleminin ilerleyişini izleyebilirsiniz. Ayrıca, sanal makine testi çoğaltmasının Azure portalında görünür olması gerekir. Şirket içi ağınızdan sanal makinelere erişimi ayarladıysanız sanal makineye yönelik Uzak Masaüstü bağlantısını başlatabilirsiniz.
4. Yük devretme işlemi **Testi Tamamla** aşamasına ulaştığında yük devretmeyi sonlandırmak için **Testi Tamamla**'ya tıklayın. Yük devretme işleminin ilerleyişini durumunu izlemek için ve gerekli eylemleri gerçekleştirmek için **İş** sekmesinin ayrıntılarına göz atabilirsiniz.
5. Yük devretmeden sonra sanal makinenin test çoğaltmasını Azure portalında görürsünüz. Şirket içi ağınızdan sanal makinelere erişimi ayarladıysanız sanal makineye yönelik Uzak Masaüstü bağlantısını başlatabilirsiniz.

   1. Sanal makinelerin başarılı bir şekilde başlatıldığını doğrulayın.
   2. Yük devretmenin ardından Azure'daki sanal makineye Uzak Masaüstü kullanarak bağlanmak isterseniz yük devretme testini çalıştırmadan önceden sanal makinenizde Uzak Masaüstü Bağlantısı'nı etkinleştirin. Sanal makinede bir RDP uç noktası eklemeniz gerekir. Yararlanabileceğiniz bir [Azure Otomasyonu runbook'u](site-recovery-runbook-automation.md) Bunu yapmak için.
   3. Yük devretmenin ardından Uzak Masaüstü'nü kullanarak Azure'daki sanal makineye bağlanmak için genel bir IP adresi kullanırsanız genel bir adres kullanarak sanal makineye bağlanmanızı engelleyecek etki alanı ilkelerine sahip olmadığınızdan emin olun
6. Test tamamlandıktan sonra şunları yapın:

   * **Yük devretme testi tamamlandı** seçeneğine tıklayın. Otomatik olarak kapatmak ve sanal makine testlerini silmek için test ortamını temizleyin.
   * Yük devretme testiyle ilişkili gözlemlerinizi kaydetmek ve saklamak için **Notlar**'a tıklayın.
7. Yük devretme ulaştığında **testi Tamamla** aşaması son doğrulama gibi:
   1. Azure portalında çoğaltılan sanal makineyi görüntüleyin. Sanal makinenin başarılı bir şekilde başlatıldığını doğrulayın.
   2. Şirket içi ağınızdan sanal makinelere erişimi ayarladıysanız sanal makineye yönelik Uzak Masaüstü bağlantısını başlatabilirsiniz.
   3. Bitirmek için **Testi Tamamla**'ya tıklayın.
   4. Yük devretme testiyle ilişkili gözlemlerinizi kaydetmek ve saklamak için **Notlar**'a tıklayın.
   5. **Yük devretme testi tamamlandı** seçeneğine tıklayın. Otomatik olarak kapatmak ve sanal makine testini silmek için test ortamını temizleyin.

## <a name="next-steps"></a>Sonraki adımlar
Dağıtımınız ayarlandıktan ve çalışmaya başladıktan sonra yük devretme hakkında [daha fazla bilgi edinebilirsiniz](site-recovery-failover.md).
