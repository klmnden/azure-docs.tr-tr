---
title: "VMM bulutlarındaki Hyper-V sanal makinelerini Azure'a çoğaltma | Microsoft Belgeleri"
description: "System Center VMM bulutlarında yönetilen Hyper-V sanal makinelerinden Azure'a çoğaltma, yük devretme ve kurtarmayı düzenleme"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: tysonn
ms.assetid: 8e7d868e-00f3-4e8b-9a9e-f23365abf6ac
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 06/14/2017
ms.author: raynew
ms.translationtype: HT
ms.sourcegitcommit: 49bc337dac9d3372da188afc3fa7dff8e907c905
ms.openlocfilehash: b53e7f5454cd97f013fdce052f0a990a44958dee
ms.contentlocale: tr-tr
ms.lasthandoff: 07/14/2017

---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-azure-using-site-recovery-in-the-azure-portal"></a>Azure portalında Site Recovery’yi kullanarak VMM bulutlarındaki Hyper-V sanal makinelerini Azure'a çoğaltma
> [!div class="op_single_selector"]
> * [Azure portal](site-recovery-vmm-to-azure.md)
> * [Azure klasik](site-recovery-vmm-to-azure-classic.md)
> * [PowerShell Resource Manager](site-recovery-vmm-to-azure-powershell-resource-manager.md)
> * [PowerShell klasik](site-recovery-deploy-with-powershell.md)


Bu makalede, Azure portalındaki [Azure Site Recovery](site-recovery-overview.md) hizmetini kullanarak System Center VMM bulutlarında yönetilen şirket içi Hyper-V sanal makinelerini Azure'a nasıl çoğaltacağınız açıklanmıştır.

Bu makaleyi okuduktan sonra yapmak istediğiniz tüm yorumları makalenin alt kısmında veya [Azure Kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)'nda paylaşabilirsiniz.

Makineleri Azure'a geçirmek istiyorsanız (yeniden çalışma olmadan) [bu makaleden](site-recovery-migrate-to-azure.md) daha fazla bilgiye ulaşabilirsiniz.


## <a name="deployment-steps"></a>Dağıtım adımları

Bu dağıtım adımlarını tamamlamak için makaleyi izleyin:


1. Bu dağıtım mimarisi hakkında [daha fazla bilgi edinin](site-recovery-components.md). Ayrıca Site Recovery'de Hyper-V çoğaltmanın nasıl çalıştığı [hakkında bilgi edinin](site-recovery-hyper-v-azure-architecture.md).
2. Önkoşulları ve sınırlamaları doğrulayın.
3. Azure ağ ve depolama hesaplarını kurun.
4. Şirket içi VMM sunucusunu ve Hyper-V konaklarını hazırlayın.
5. Kurtarma Hizmetleri kasası oluşturun. Kasa yapılandırma ayarlarını içerir ve çoğaltmayı yönetir.
6. Kaynak ayarlarını belirtin. VMM sunucusunu kasaya kaydedin. Azure Site Recovery Sağlayıcısı'nı VMM sunucusuna, Microsoft Kurtarma Hizmetleri aracısını Hyper-V konağına yükleyin.
7. Hedef ve çoğaltma ayarlarını yapın.
8. VM'ler için çoğaltmayı etkinleştirin.
9. Her şeyin beklendiği gibi çalıştığından emin olmak için bir yük devretme testi çalıştırın.



## <a name="prerequisites"></a>Ön koşullar


**Destek gereksinimi** | **Ayrıntılar**
--- | ---
**Azure** | [Azure gereksinimleri](site-recovery-prereq.md#azure-requirements) hakkında bilgi edinin.
**Şirket içi sunucular** | Şirket içi VMM sunucusu ve Hyper-V konakları hakkında [daha fazla bilgi edinin](site-recovery-prereq.md#disaster-recovery-of-hyper-v-virtual-machines-in-virtual-machine-manager-clouds-to-azure).
**Şirket içi Hyper-V VM'leri** | Çoğaltmak istediğiniz VM'lerin [desteklenen işletim sistemi](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions) çalıştırıyor olması ve [Azure önkoşullarına](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) uygun olması gerekir.
**Azure URL'leri** | VMM sunucusunun şu URL'lere erişimi olmalıdır:<br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/> IP adresi tabanlı güvenlik duvarı kurallarına sahipseniz bu kuralların Azure ile iletişim kurmaya izin verdiğinden emin olun.<br/></br> [Azure Veri Merkezi IP Aralıkları](https://www.microsoft.com/download/confirmation.aspx?id=41653)'na ve HTTPS (443) bağlantı noktasına izin verin.<br/></br> Aboneliğinizin Azure bölgesi ve Batı ABD (Access Control ve Identity Management için kullanılır) için IP adresi aralıklarına izin verin.


## <a name="prepare-for-deployment"></a>Dağıtım için hazırlanma
Dağıtıma hazırlanmak için şunları yapmanız gerekir:

1. Yük devretmeden sonra Azure VM’lerinin içinde bulunacağı [bir Azure ağı ayarlayın](#set-up-an-azure-network).
2. Çoğaltılan veriler için [Azure depolama hesabı ayarlama](#set-up-an-azure-storage-account).
3. Site Recovery dağıtımı için [VMM sunucusunu hazırlama](#prepare-the-vmm-server).
4. Ağ eşlemesi için hazırlanın. Site Recovery dağıtımı sırasında ağ eşlemesini yapılandırabilmeniz için ağları ayarlayın.

### <a name="set-up-an-azure-network"></a>Azure ağı ayarlama
Yük devretme işleminden sonra oluşturulan Azure VM'lerinin bağlanacağı bir Azure ağınızın olması gerekir.

* Ağın, Kurtarma Hizmetleri kasasıyla aynı konumda olması gerekir.
* Azure ağını, yük devri yapılan Azure VM'lerinde kullanmak istediğiniz kaynak modeline bağlı olarak [Resource Manager modunda](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) veya [klasik modda](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) ayarlarsınız.
* Başlamadan önce bir ağ ayarlamanızı öneririz. Aksi takdirde, Site Recovery dağıtımı sırasında yapmanız gerekir.
Site Recovery tarafından kullanılan Azure ağları, aynı abonelik içinde veya farklı abonelikler arasında [taşınamaz](../azure-resource-manager/resource-group-move-resources.md).

### <a name="set-up-an-azure-storage-account"></a>Azure depolama hesabı ayarlama
* Azure'a çoğaltılan verileri tutmak için standart/premium Azure depolama hesabına sahip olmanız gerekir.[Premium depolama](../storage/storage-premium-storage.md) sürekli yüksek G/Ç performansına ve yoğun G/Ç kullanan iş yüklerini barındırmak için düşük gecikme süresine ihtiyaç duyan sanal makineler için kullanılır. Çoğaltılan veriler için bir premium depolama hesabı kullanmak istiyorsanız, şirket içi verilerde gerçekleşen değişiklikleri yakalayan çoğaltma günlüklerini depolamak üzere ek bir standart depolama hesabı da ayarlamanız gerekir. Hesabın, Kurtarma Hizmetleri kasasıyla aynı bölgede olması gerekir.
* Yük devri yapılan Azure VM'lerinde kullanmak istediğiniz kaynak modeline bağlı olarak [Resource Manager modunda](../storage/storage-create-storage-account.md) veya [klasik modda](../storage/storage-create-storage-account-classic-portal.md) bir hesap ayarlarsınız.
* Başlamadan önce bir hesap ayarlamanızı öneririz. Aksi takdirde, Site Recovery dağıtımı sırasında yapmanız gerekir.
- Site Recovery tarafından kullanılan depolama hesaplarının, aynı abonelik içinde veya farklı abonelikler arasında [taşınamadığını](../azure-resource-manager/resource-group-move-resources.md) unutmayın.

### <a name="prepare-the-vmm-server"></a>VMM sunucusunu hazırlama
* VMM sunucusunun [önkoşullar](#prerequisites) ile uyum sağladığından emin olun.
* Site Recovery dağıtımı sırasında, VMM sunucusundaki tüm bulutların Azure portalda kullanılabilir olması gerektiğini belirtebilirsiniz. Portalda yalnızca belirli bulutların görünmesini istiyorsanız VMM yönetici konsolunda ilgili bulut için bu ayarı etkinleştirebilirsiniz.

### <a name="prepare-for-network-mapping"></a>Ağ eşlemesi için hazırlanma
Site Recovery dağıtımı sırasında ağ eşlemesini ayarlamanız gerekir. Şunları sağlamak için kaynak VMM VM ağları ile hedef Azure ağları arasında ağ eşlemesi:

* Aynı ağda yük devretme işlemini gerçekleştiren makineler, yük devretmeyi aynı şekilde veya aynı kurtarma planında gerçekleştirmiyor olsalar bile birbirlerine bağlanabilir.
* Hedef Azure ağında bir ağ geçidi ayarlanırsa Azure sanal makineleri şirket içi sanal makinelere bağlanabilir.
* Ağ eşlemesini ayarlamak için şunları yapmanız gerekir:

  * Kaynak Hyper-V ana bilgisayar sunucusundaki VM'lerin bir VMM VM ağına bağlı olduğundan emin olun. Bu ağın, bulutla ilişkilendirilen mantıksal bir ağ ile bağlantılı olması gerekir.
  * [Yukarıda](#set-up-an-azure-network) açıklandığı gibi bir Azure ağına sahip olmanız gerekir.

## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. **Yeni** > **İzleme + Yönetim** > **Backup and Site Recovery (OMS)** öğesine tıklayın.

    ![Yeni kasa](./media/site-recovery-vmm-to-azure/new-vault3.png)
3. **Ad** alanında, kasayı tanımlamak için bir kolay ad belirtin. Birden fazla aboneliğiniz varsa bunlardan birini seçin.
4. [Kaynak grubu oluşturun](../azure-resource-manager/resource-group-template-deploy-portal.md) veya var olan bir grubu seçin. Bir Azure bölgesi belirtin. Makineler bu bölgeye çoğaltılır. Desteklenen bölgeleri kontrol etmek için [Azure Site Recovery Fiyatlandırma Ayrıntıları](https://azure.microsoft.com/pricing/details/site-recovery/) bölümündeki Coğrafi Kullanılabilirlik kısmına bakın.
5. Pano'dan kasaya hızlıca erişmek istiyorsanız **Panoya sabitle** > **Kasa oluştur** seçeneğine tıklayın.

    ![Yeni kasa](./media/site-recovery-vmm-to-azure/new-vault.png)

Yeni kasa **Pano** > **Tüm kaynaklar** bölümünde ve ana **Kurtarma Hizmetleri kasaları** dikey penceresinde görünür.


## <a name="select-the-protection-goal"></a>Koruma hedefi seçme

Neleri çoğaltmak istediğinizi ve bunları nereye çoğaltacağınızı seçin.

1. **Kurtarma Hizmetleri kasaları** bölümünde kasayı seçin.
2. **Başlarken** bölümünde, **Site Recovery** > **Altyapıyı Hazırlama** > **Koruma hedefi** seçeneklerine tıklayın.

    ![Hedefleri seçme](./media/site-recovery-vmm-to-azure/choose-goals.png)
3. **Koruma hedefi** kısmında **Azure'a** seçeneğini belirleyin ve **Evet, Hyper-V ile**'yi seçin. Hyper-V ana bilgisayarlarını ve kurtarma sitesini yönetmek için VMM kullandığınızı doğrulamak için **Evet** seçeneğini belirleyin. Daha sonra, **Tamam**'a tıklayın.

## <a name="set-up-the-source-environment"></a>Kaynak ortamı ayarlama

Azure Site Recovery Sağlayıcısı'nı VMM sunucusuna yükleyin ve sunucuyu kasaya kaydedin. Azure Kurtarma Hizmetleri aracısını Hyper-V ana bilgisayarlarına yükleyin.

1. **Altyapıyı Hazırlama** > **Kaynak** seçeneklerine tıklayın.

    ![Kaynağı ayarlama](./media/site-recovery-vmm-to-azure/set-source1.png)

2. **Kaynağı ayarla** kısmında, bir VMM sunucusu eklemek için **+ VMM**'ye tıklayın.

    ![Kaynağı ayarlama](./media/site-recovery-vmm-to-azure/set-source2.png)

3. **Sunucu Ekle** sayfasında, **Sunucu türü** alanında **System Center VMM sunucusunun** görünüp görünmediğini ve VMM sunucusunun [önkoşulları ve URL gereksinimlerini](#prerequisites) karşılayıp karşılamadığını kontrol edin.
4. Azure Site Recovery Sağlayıcısı yükleme dosyasını indirin.
5. Kayıt anahtarını indirin. Kurulumu çalıştırırken buna ihtiyacınız olur. Anahtar, oluşturulduktan sonra beş gün boyunca geçerlidir.

    ![Kaynağı ayarlama](./media/site-recovery-vmm-to-azure/set-source3.png)


## <a name="install-the-provider-on-the-vmm-server"></a>Sağlayıcıyı VMM sunucusuna yükleme

1. Sağlayıcı kurulum dosyasını VMM sunucusunda çalıştırın.
2. **Microsoft Update** kısmından güncelleştirmeleri seçebilirsiniz. Böylece, Sağlayıcı güncelleştirmeleri Microsoft Update ilkenize uygun şekilde yüklenir.
3. **Yükleme** bölümünde varsayılan Sağlayıcı yükleme konumunu kabul edin veya değiştirin ve **Yükle**'ye tıklayın.

    ![Yükleme konumu](./media/site-recovery-vmm-to-azure/provider2.png)
4. Yükleme tamamlandığında VMM sunucusunu kasaya kaydetmek için **Kaydet**'e tıklayın.
5. **Kasa Ayarları** sayfasında, kasa anahtarı dosyasını seçmek için **Gözat**’a tıklayın. Azure Site Recovery aboneliğini ve kasa adını belirtin.

    ![Sunucu kaydı](./media/site-recovery-vmm-to-azure/provider10.PNG)
6. **İnternet Bağlantısı** alanında, VMM sunucusunda çalışan Sağlayıcı'nın İnternet üzerinden Site Recovery'ye nasıl bağlanacağını belirtin.

   * Sağlayıcı'nın doğrudan bağlanmasını istiyorsanız **Ara sunucu olmadan doğrudan Azure Site Recovery hizmetine bağlan** seçeneğini belirleyin.
   * Mevcut ara sunucunuz kimlik doğrulaması gerektiriyorsa veya özel bir ara sunucu kullanmak istiyorsanız, **Ara sunucu kullanarak Azure Site Recovery hizmetine bağlan** seçeneğini belirleyin.
   * Özel bir ara sunucu kullanıyorsanız adresi, bağlantı noktasını ve kimlik bilgilerini belirtin.
   * Ara sunucu kullanıyorsanız [önkoşullar](#on-premises-prerequisites) bölümünde açıklanan URL'lere izin vermiş olmanız gerekir.
   * Özel bir ara sunucu kullanırsanız belirtilen ara sunucu kimlik bilgileriyle otomatik olarak bir VMM RunAs hesabı (DRAProxyAccount) oluşturulacaktır. Bu hesabın kimlik doğrulamasını başarıyla gerçekleştirebilmesi için ara sunucuyu yapılandırın. VMM RunAs hesabı ayarları VMM konsolundan değiştirilebilir. **Ayarlar** kısmında, **Güvenlik** > **Run As Hesapları** seçeneklerini genişletin ve ardından DRAProxyAccount parolasını değiştirin. Bu ayarın etkili olabilmesi için VMM hizmetinin yeniden başlatılması gerekir.

     ![internet](./media/site-recovery-vmm-to-azure/provider13.PNG)
7. Veri şifreleme için otomatik olarak oluşturulan SSL sertifikasının konumunu kabul edin veya değiştirin. Bu sertifika, Azure Site Recovery portalında Azure tarafından korunan bir bulut için veri şifrelemeyi etkinleştirdiğinizde kullanılır. Bu sertifikayı güvenli bir yerde saklayın. Azure'a yük devretme işlemi çalıştırdığınızda veri şifreleme etkinse şifrenin çözülmesi gerekir.
8. **Sunucu adı** alanında, kasadaki VMM sunucusunu tanımlamak için bir kolay ad belirtin. Bir küme yapılandırmasında VMM küme rolünün adını belirtin.
9. VMM sunucusundaki tüm bulutlara yönelik meta verileri kasayla eşitlemek isterseniz **Bulut meta verilerini eşitle** seçeneğini etkinleştirin. Bu eylemin her sunucuda yalnızca bir kez gerçekleştirilmesi gerekir. Tüm bulutları eşitlemek istemezseniz bu ayarı işaretlemeden bırakıp her bulutu VMM konsolundaki bulut özelliklerinde bağımsız olarak eşitleyebilirsiniz. İşlemi tamamlamak için **Kaydet**'e tıklayın.

    ![Sunucu kaydı](./media/site-recovery-vmm-to-azure/provider16.PNG)
10. Kayıt başlar. Kayıt tamamlandıktan sonra, sunucu **Site Recovery Altyapısı** > **VMM Sunucuları** içinde gösterilir.


## <a name="install-the-azure-recovery-services-agent-on-hyper-v-hosts"></a>Azure Kurtarma Hizmetleri aracısını Hyper-V ana bilgisayarlarına yükleme

1. Sağlayıcı'yı ayarladıktan sonra Azure Kurtarma Hizmetleri aracısı için yükleme dosyasını indirmeniz gerekir. VMM bulutundaki tüm Hyper-V sunucularında kurulum çalıştırma

    ![Hyper-V siteleri](./media/site-recovery-vmm-to-azure/hyperv-agent1.png)
2. **Önkoşul Denetimi**’nde **İleri**'ye tıklayın. Eksik önkoşullar otomatik olarak yüklenir.

    ![Kurtarma Hizmetleri Aracısı Önkoşulları](./media/site-recovery-vmm-to-azure/hyperv-agent2.png)
3. **Yükleme Ayarları**’nda yükleme ve önbellek konumunu kabul edin veya değiştirin. Önbelleği, en az 5 GB kullanılabilir depolama alanı bulunan bir sürücüde yapılandırabilirsiniz, ancak 600 GB veya daha fazla boş alana sahip bir önbellek sürücüsü kullanmanızı öneririz. Ardından **Yükle**'ye tıklayın.
4. Yükleme tamamlandıktan sonra **Kapat**’a tıklayarak işlemi sonlandırın.

    ![MARS Aracısı'nı Kaydetme](./media/site-recovery-vmm-to-azure/hyperv-agent3.png)

### <a name="command-line-installation"></a>Komut satırı yüklemesi
Aşağıdaki komutu kullanarak Microsoft Azure Kurtarma Hizmetleri Aracısı'nı komut satırından yükleyebilirsiniz:

     marsagentinstaller.exe /q /nu

### <a name="set-up-internet-proxy-access-to-site-recovery-from-hyper-v-hosts"></a>Hyper-V ana bilgisayarlarından Site Recovery'ye internet ara sunucusu erişimini ayarlama

Hyper-V ana bilgisayarlarında çalışan Kurtarma Hizmetleri aracısının VM çoğaltması için Azure'a yönelik internet erişimi gerekir. İnternet'e bir ara sunucu aracılığıyla erişiyorsanız ara sunucuyu aşağıdaki gibi ayarlayın:

1. Hyper-V ana bilgisayarındaki Microsoft Azure Backup MMC ek bileşenini açın. Varsayılan olarak Microsoft Azure Backup için masaüstünde veya C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin yolunda bir kısayol bulunur.
2. Ek bileşende **Özellikleri Değiştir**'e tıklayın.
3. **Ara Sunucu Yapılandırması** sekmesinde ara sunucu bilgilerini belirtin.

    ![MARS Aracısı'nı Kaydetme](./media/site-recovery-vmm-to-azure/mars-proxy.png)
4. Aracının [önkoşullar](#on-premises-prerequisites) bölümünde açıklanan URL'lere erişebildiğini denetleyin.

## <a name="set-up-the-target-environment"></a>Hedef ortamı ayarlama
Çoğaltma için kullanılacak Azure depolama hesabını ve yük devretmenin ardından Azure VM'lerinin bağlanacağı Azure ağını belirtin.

1. **Altyapıyı hazırlama** > **Hedef** seçeneklerine tıklayıp, kullanmak istediğiniz aboneliği ve yükü devredilmiş sanal makineleri oluşturmak istediğiniz kaynak grubunu seçin. Yükü devredilen sanal makineler için Azure’da kullanmak istediğiniz dağıtım modelini (klasik veya kaynak yönetimi) seçin.

    ![Depolama](./media/site-recovery-vmm-to-azure/enablerep3.png)

2. Site Recovery, bir veya birden çok uyumlu Azure depolama hesabınızın ve ağınızın olup olmadığını denetler.

    ![Depolama](./media/site-recovery-vmm-to-azure/compatible-storage.png)

3. Depolama hesabı oluşturmadıysanız ve Resource Manager’ı kullanarak bir hesap oluşturmak istiyorsanız bu işlemi satır içinde yapmak için **+Depolama hesabı** seçeneğine tıklayın.  **Depolama hesabı oluştur** dikey penceresinde hesap adını, türünü, aboneliği ve konumu belirtin. Hesabın, Kurtarma Hizmetleri kasasıyla aynı konumda olması gerekir.

   ![Depolama](./media/site-recovery-vmm-to-azure/gs-createstorage.png)


   * Klasik modeli kullanarak bir depolama hesabı oluşturmak istiyorsanız bu işlemi Azure portalından gerçekleştirin. [Daha fazla bilgi](../storage/storage-create-storage-account-classic-portal.md)
   * Çoğaltılan veriler için bir premium depolama hesabı kullanıyorsanız, şirket içi verilerde gerçekleşen değişiklikleri yakalayan çoğaltma günlüklerini depolamak üzere ek bir standart depolama hesabı ayarlayın.
5. Henüz bir Azure ağı oluşturmadıysanız ve Resource Manager’ı kullanarak bir ağ oluşturmak istiyorsanız bu işlemi satır içinde yapmak için **+Ağ** seçeneğine tıklayın. **Sanal ağ oluştur** dikey penceresinde ağ adı, adres aralığı, alt ağ ayrıntıları, abonelik ve konum belirtin. Ağın, Kurtarma Hizmetleri kasasıyla aynı konumda olması gerekir.

   ![Ağ](./media/site-recovery-vmm-to-azure/gs-createnetwork.png)

   Klasik modeli kullanarak bir ağ oluşturmak isterseniz bu işlemi Azure portalından gerçekleştirin. [Daha fazla bilgi edinin](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).

### <a name="configure-network-mapping"></a>Ağ eşlemesini yapılandırma

* Ağ eşlemesi işlevinin genel hatlarını [okuyun](#prepare-for-network-mapping).
* VMM sunucusu üzerindeki sanal makinelerin VM ağına bağlı olduğunu ve en az bir Azure sanal ağı oluşturduğunuzu doğrulayın. Tek bir Azure ağına birden çok VM ağı eşlenebilir.

Eşleme işlemini şu şekilde yapılandırın:

1. **Site Recovery Altyapısı** > **Ağ eşlemeleri** > **Ağ Eşleme** bölümünde, **+Ağ Eşlemesi** simgesine tıklayın.

    ![Ağ eşlemesi](./media/site-recovery-vmm-to-azure/network-mapping1.png)
2. **Ağ eşlemesi ekle** seçeneğinde, kaynak olarak VMM sunucusunu, hedef olarak ise **Azure**'u seçin.
3. Yük devretme işleminden sonra dağıtım modelini ve aboneliği seçin.
4. **Kaynak ağ** bölümünde, VMM sunucusuyla ilişkili listeden eşlemek istediğiniz kaynak şirket içi VM ağını seçin.
5. **Hedef ağ** bölümünde, çoğaltılan Azure VM'lerinin çalışmaya başladığında bulunacağı Azure ağını seçin. Daha sonra, **Tamam**'a tıklayın.

    ![Ağ eşlemesi](./media/site-recovery-vmm-to-azure/network-mapping2.png)

Ağ eşlemesi başladığında gerçekleşecekler şunlardır:

* Eşleme başladığında kaynak VM ağındaki var olan VM’ler hedef ağa bağlanır. Kaynak VM ağına bağlanan yeni VM’ler, çoğaltma gerçekleştiğinde eşlenen Azure ağına bağlanır.
* Mevcut bir ağ eşlemesini değiştirirseniz çoğaltılan sanal makineler yeni ayarlar kullanılarak bağlanır.
* Hedef ağın birden çok alt ağı varsa ve bu alt ağlardan biri kaynak sanal makinenin bulunduğu alt ağ ile aynı adı taşıyorsa, çoğaltılan sanal makine yük devretme işleminin ardından hedef alt ağa bağlanır.
* Eşleşen ada sahip bir hedef alt ağ yoksa sanal makine ağdaki ilk alt ağa bağlanır.

## <a name="configure-replication-settings"></a>Çoğaltma ayarlarını yapılandırma
1. Yeni bir çoğaltma ilkesi oluşturmak için **Altyapıyı hazırlama** > **Çoğaltma Ayarları** > **+Oluştur ve ilişkilendir** seçeneklerine tıklayın.

    ![Ağ](./media/site-recovery-vmm-to-azure/gs-replication.png)
2. **İlke oluştur ve ilişkilendir** bölümünde bir ilke adı belirtin.
3. **Kopyalama sıklığı** kısmında, ilk çoğaltmadan sonra değişim verilerini ne sıklıkta çoğaltacağınızı belirleyin (30 saniyede, 5 veya 15 dakikada bir).

    > [!NOTE]
    >  Premium depolama hesabına çoğaltırken 30 saniyelik aralık desteklenmez. Sınırlama premium depolama tarafından desteklenen blob başına anlık görüntü sayısıyla (100) belirlenir. [Daha fazla bilgi](../storage/storage-premium-storage.md#snapshots-and-copy-blob)

4. **Kurtarma noktası bekletme** bölümünde, her kurtarma noktası için bekletme süresinin ne kadar olacağını saat cinsinden belirtin. Korumalı makineler, bu süre içindeki herhangi bir noktaya kurtarılabilir.
5. **Uygulamayla tutarlı anlık görüntü sıklığı** kısmında, uygulamayla tutarlı anlık görüntüleri içeren kurtarma noktasının hangi sıklıkta oluşturulacağını (1-12 saat) belirtin. Hyper-V iki çeşit anlık görüntü kullanır: tüm sanal makinenin artımlı anlık görüntüsünü sunan standart anlık görüntü ve sanal makine içinde uygulama verilerinin belirli bir noktadaki anlık görüntüsünü alan uygulamayla tutarlı anlık görüntü. Uygulamayla tutarlı anlık görüntüler, anlık görüntü alınırken uygulamaların tutarlı bir durumda olmasını sağlamak için Birim Gölge Kopyası Hizmeti'ni (VSS) kullanır. Uygulamayla tutarlı anlık görüntüleri etkinleştirirseniz kaynak sanal makinelerde çalışan uygulamaların performansının etkileneceğini unutmayın. Ayarladığınız değerin, yapılandırdığınız ilave kurtarma noktası sayısından daha az olduğundan emin olun.
6. **İlk çoğaltma başlangıç zamanı** bölümünde, ilk çoğaltmanın başlatılacağı zamanını belirtin. Çoğaltma işlemi internet bant genişliğiniz üzerinden gerçekleştirileceğinden, çoğaltma zamanını yoğun saatlerin dışında olacak şekilde ayarlamak isteyebilirsiniz.
7. **Azure'da depolanan verileri şifrele** bölümünde, Azure depolama alanındaki bekleyen verilerin şifrelenip şifrelenmeyeceğini belirtin. Daha sonra, **Tamam**'a tıklayın.

    ![Çoğaltma ilkesi](./media/site-recovery-vmm-to-azure/gs-replication2.png)
8. Yeni bir ilke oluşturduğunuzda, bu ilke otomatik olarak VMM bulutuyla ilişkilendirilir. **Tamam**’a tıklayın. **Çoğaltma** > ilke adı > **VMM Bulutu ile İlişkilendir** üzerinden ilave VMM bulutlarını (ve bu bulutların içindeki VM’leri) bu çoğaltma ilkesi ile ilişkilendirebilirsiniz.

    ![Çoğaltma ilkesi](./media/site-recovery-vmm-to-azure/policy-associate.png)

## <a name="capacity-planning"></a>Kapasite planlaması

Temel altyapınızı ayarladığınıza göre kapasite planlaması yapmayı ve ilave kaynaklara ihtiyacınız olup olmadığını düşünün.

Site Recovery; kaynak ortamınız, Site Recovery bileşenleri, ağ ve depolama için doğru kaynakları ayırmanıza yardımcı olmak üzere bir kapasite planlayıcısı sunar. Planlayıcıyı; depolama alanı, disk ve VM'lerin ortalama sayısına göre tahmin oluşturmak için hızlı modda ve iş yükü düzeyinde sayılar gireceğiniz ayrıntılı modda çalıştırabilirsiniz. Başlamadan önce:

* VM'ler, VM başına disk ve disk başına depolama dahil olmak üzere çoğaltma ortamınız hakkında bilgi toplayın.
* Çoğaltılan veriler için günlük değişim (dalgalanma) hızına yönelik tahminde bulunun. Bu işlemi gerçekleştirmenize yardımcı olması için [Hyper-V Çoğaltma için Capacity Planner](https://www.microsoft.com/download/details.aspx?id=39057)'ı kullanabilirsiniz.

1. **İndir**'e tıklayarak aracı indirin ve ardından çalıştırın. Araç ile birlikte sunulan [makaleyi okuyun](site-recovery-capacity-planner.md).
2. Okumayı tamamladığınızda **Have you run the Capacity Planner?** (Capacity Planner’ı çalıştırdınız mı?) sorusuna **Evet** yanıtını verin.

   ![Kapasite planlaması](./media/site-recovery-vmm-to-azure/gs-capacity-planning.png)

   [Ağ bant genişliğini denetleme](#network-bandwidth-considerations) hakkında daha fazla bilgi edinin




## <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

Başlamadan önce Azure kullanıcı hesabınızın yeni bir sanal makinenin Azure'a çoğaltılmasını sağlamak için gerekli [izinlere](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) sahip olduğundan emin olun.

Şimdi aşağıda belirtilen şekilde çoğaltmayı etkinleştirin:

1. **2. Adım: Uygulama çoğaltma** > **Kaynak** seçeneklerine tıklayın. Çoğaltmayı ilk kez etkinleştirdikten sonra ek makineler için çoğaltma işlemini etkinleştirmek istiyorsanız kasada **+Çoğalt**'a tıklayın.

    ![Çoğaltmayı etkinleştirme](./media/site-recovery-vmm-to-azure/enable-replication1.png)
2. **Kaynak** dikey penceresinde Hyper-V konaklarının bulunduğu VMM sunucusunu ve bulutu seçin. Daha sonra, **Tamam**'a tıklayın.

    ![Çoğaltmayı etkinleştirme](./media/site-recovery-vmm-to-azure/enable-replication-source.png)
3. **Hedef** kısmında aboneliği, yük devretme sonrası dağıtım modelini ve çoğaltılan veriler için kullandığınız depolama hesabını seçin.

    ![Çoğaltmayı etkinleştirme](./media/site-recovery-vmm-to-azure/enable-replication-target.png)
4. Kullanmak istediğiniz depolama hesabını seçin. Sahip olduğunuz hesaplardan farklı bir depolama hesabı kullanmak isterseniz [yeni bir hesap](#set-up-an-azure-storage-account) oluşturabilirsiniz. Çoğaltılan veriler için bir premium depolama hesabı kullanıyorsanız şirket içi verilerde gerçekleşen değişiklikleri yakalayan çoğaltma günlüklerini depolamak üzere ek bir standart depolama hesabı seçmeniz gerekir. Resource Manager kullanarak depolama hesabı oluşturmak için **Yeni oluştur**'a tıklayın. Klasik modeli kullanarak bir depolama hesabı oluşturmak istiyorsanız bu işlemi [Azure portalından](../storage/storage-create-storage-account-classic-portal.md) gerçekleştirin. Daha sonra, **Tamam**'a tıklayın.
5. Yük devretme sonrasında oluşturulan Azure VM'lerinin bağlanacağı Azure ağını ve alt ağını seçin. Koruma için seçtiğiniz tüm makinelere ağ ayarını uygulamak için **Seçili makineler için şimdi yapılandır**’ı seçin. Her makine için Azure ağını ayrı ayrı seçmek üzere **Daha sonra yapılandır**'ı seçin. Sahip olduğunuz ağlardan farklı bir ağ kullanmak isterseniz [yeni bir ağ](#set-up-an-azure-network) oluşturabilirsiniz. Resource Manager modelini kullanarak bir ağ oluşturmak için **Yeni oluştur**'a tıklayın. Klasik modeli kullanarak bir ağ oluşturmak isterseniz bu işlemi [Azure portalından](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) gerçekleştirin. Bir alt ağ (varsa) seçin. Daha sonra, **Tamam**'a tıklayın.
6. **Sanal Makineler** > **Sanal makine seçin** seçeneklerine tıklayın ve çoğaltmak istediğiniz makineleri seçin. Yalnızca çoğaltmanın etkinleştirildiği makineleri seçebilirsiniz. Daha sonra, **Tamam**'a tıklayın.

    ![Çoğaltmayı etkinleştirme](./media/site-recovery-vmm-to-azure/enable-replication5.png)

7. **Özellikler** > **Özellikleri yapılandır** seçeneklerinde, seçili VM'ler için işletme sistemini ve OS diskini seçin.

    - Azure VM adının (hedef ad) [Azure sanal makine gereksinimlerine](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) uygun olduğundan emin olun.   
    - Varsayılan olarak VM'nin tüm diskleri çoğaltma için seçilir ancak istemediklerinizin işaretini kaldırarak onları hariç tutabilirsiniz.

        - Çoğaltma bant genişliğini azaltmak için diskleri hariç tutmak isteyebilirsiniz. Örneğin geçici verilere veya bilgisayar ya da uygulama yeniden başlatıldığında yenilenen verilere (pagefile.sys veya Microsoft SQL Server tempdb gibi) sahip diskleri çoğaltmak istemeyebilirsiniz. Diskin seçimini kaldırarak çoğaltma kapsamı dışında bırakabilirsiniz.
        - Yalnızca temel diskler dışarıda bırakılabilir. İşletim sistemi diskleri dışarıda bırakılamaz.
        - Dinamik diskleri dışarıda tutmamanızı öneririz. Site Recovery, konuk VM içerisindeki bir sanal sabit diskin temel mi yoksa dinamik mi olduğunu anlayamaz. Bağımlı dinamik birim disklerinin tümü hariç bırakılmazsa, korumalı dinamik disk VM yük devrinde başarısız olur ve diskteki verilere erişim sağlanamaz.
        - Çoğaltmayı etkinleştirdikten sonra çoğaltma için disk ekleme veya kaldırma gerçekleştiremezsiniz. Disk eklemek veya dışlamak istiyorsanız, VM’nin korumasını devre dışı bırakmanız ve yeniden etkinleştirmeniz gerekir.
        - Azure'da el ile oluşturduğunuz diskler yeniden çalışır duruma getirilmez. Örneğin, üç disk için yük devretme gerçekleştirir ve ikisini doğrudan Azure sanal makinesinde oluşturursanız, yalnızca yük devretme gerçekleştirilen üç disk Azure'dan Hyper-V'ye çoğaltılarak yeniden çalışır hale getirilir. El ile oluşturulmuş diskleri yeniden çalışma veya Hyper-V'den Azure'a geri çoğaltma işlemine dahil edemezsiniz.
        - Bir uygulamanın çalışması için gerekli olan bir diski hariç tutarsanız, Azure'a yük devretme gerçekleştirildikten sonra çoğaltılan uygulamanın çalışması için Azure'da el ile oluşturmanız gerekir. Alternatif olarak makinenin yük devri sırasında diskin otomatik olarak oluşturulması için Azure otomasyonunu bir kurtarma planıyla tümleştirebilirsiniz.

    Değişiklikleri kaydetmek için **Tamam**'a tıklayın. Daha sonra ek özellikleri ayarlayabilirsiniz.

    ![Çoğaltmayı etkinleştirme](./media/site-recovery-vmm-to-azure/enable-replication6-with-exclude-disk.png)

8. **Çoğaltma ayarları** > **Çoğaltma ayarlarını yapılandır** seçeneklerinde, korumalı VM'lere uygulamak istediğiniz çoğaltma ilkesini seçin. Daha sonra, **Tamam**'a tıklayın. **Çoğaltma ilkeleri** > ilke adı > **Ayarları Düzenle**’de çoğaltma ilkesini değiştirebilirsiniz. Uyguladığınız değişiklikler, zaten çoğaltılmakta olan makinelere ve yeni makinelere uygulanır.

   ![Çoğaltmayı etkinleştirme](./media/site-recovery-vmm-to-azure/enable-replication7.png)

**İşler** > **Site Recovery işleri** üzerinden **Korumayı Etkinleştir** işinin ilerleyişini izleyebilirsiniz. **Korumayı Sonlandır** işi çalıştırıldıktan sonra makine yük devretme için hazırdır.

### <a name="view-and-manage-vm-properties"></a>VM özelliklerini görüntüleme ve yönetme

Kaynak makinenin özelliklerini doğrulamanızı öneririz. Azure VM adının [Azure sanal makine gereksinimlerini](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) karşılaması gerektiğini unutmayın.

1. **Korunan Öğeler**’de **Çoğaltılan Öğeler**’e tıklayıp ayrıntılarını görmek istediğiniz makineyi seçin.

    ![Çoğaltmayı etkinleştirme](./media/site-recovery-vmm-to-azure/vm-essentials.png)
2. **Özellikler** kısmında VM'nin çoğaltma ve yük devretme bilgilerini inceleyebilirsiniz.

    ![Çoğaltmayı etkinleştirme](./media/site-recovery-vmm-to-azure/test-failover2.png)
3. **İşlem ve Ağ** > **İşlem özellikleri** seçeneklerinden Azure VM adını ve hedef boyutu belirtebilirsiniz. Gerekirse [Azure gereksinimleri](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) ile uyum sağlamak için adı değiştirin. Azure VM'sine atanan IP adresi, alt ağ ve hedef ağ ile ilgili bilgileri de görüntüleyip değiştirebilirsiniz.
Şunlara dikkat edin:

   * Hedef IP adresini ayarlayabilirsiniz. Bir IP adresi sağlamazsanız yük devredilen makine DHCP kullanır. Yük devretmede kullanılamayan bir adres ayarlarsanız yük devretme işlemi başarısız olur. Hedef IP adresi, yük devretme ağı testinde kullanılabilirse aynı IP adresi yük devretme sınamasında da kullanılabilir.
   * Ağ bağdaştırıcılarının sayısı, hedef sanal makine için sizin belirlediğiniz boyuta göre aşağıdaki gibi belirlenmiştir:

     * Kaynak makinedeki ağ bağdaştırıcılarının sayısı, hedef makine boyutu için verilen ağ bağdaştırıcısı sayısına eşitse veya daha azsa hedef makine kaynakla aynı sayıda bağdaştırıcıya sahip olur.
     * Kaynak sanal makinenin bağdaştırıcı sayısı hedef boyut için izin verilen sayıyı aşarsa maksimum hedef boyutu kullanılır.
     * Örneğin, kaynak makinenin iki bağdaştırıcısı varsa ve hedef makine boyutu dört adet bağdaştırıcıyı destekliyorsa hedef makinenin iki bağdaştırıcısı olur. Kaynak makinenin iki bağdaştırıcısı varken hedef boyut yalnızca bir bağdaştırıcıyı destekliyorsa hedef makinenin bir bağdaştırıcısı olur.     
     * VM'nin birden çok bağdaştırıcısı varsa bunların tümü aynı ağa bağlanır.

     ![Çoğaltmayı etkinleştirme](./media/site-recovery-vmm-to-azure/test-failover4.png)

4. **Diskler** kısmında çoğaltılacak VM'deki işletim sistemi ve veri disklerini görebilirsiniz.

#### <a name="managed-disks"></a>Yönetilen diskler

Makinenizi Azure'a geçirirken yönetilen diskleri takmak istiyorsanız **İşlem ve Ağ** > **İşlem özellikleri** sayfasında "Yönetilen diskleri kullan" ayarını "Evet" olarak belirleyebilirsiniz. Yönetilen diskler, VM diskleriyle ilişkilendirilmiş depolama hesaplarını yöneterek Azure IaaS VM'leri için disk yönetimini kolaylaştırır. [Yönetilen diskler hakkında daha fazla bilgi edinin](https://docs.microsoft.com/en-us/azure/storage/storage-managed-disks-overview).

   - Yönetilen diskler yalnızca Azure'a yük devretme sonrasında oluşturulur ve sanal makineye eklenir. Koruma etkinleştirildikten sonra şirket içi makinelerdeki veriler depolama hesaplarında çoğaltılmaya devam eder.
   Yönetilen diskler yalnızca Resource Manager dağıtım modeli kullanılarak dağıtılan sanal makineler için oluşturulabilir.  

  > [!NOTE]
  > Azure'dan şirket içi Hyper-V ortamına yeniden çalışma, yönetilen disklere sahip makineler için şu anda desteklenmemektedir. Bu makineyi Azure'a taşımak istiyorsanız "Yönetilen diskleri kullan" ayarını "Evet" olarak belirleyin.

   - "Yönetilen diskleri kullan" ayarını "Evet" olarak belirlediğinizde yalnızca kaynak grubundaki "Yönetilen diskleri kullan" ayarı "Evet" olarak belirlenmiş kullanılabilirlik kümeleri seçilebilir. Bunu nedeni yönetilen disklere sahip sanal makinelerin yalnızca "Yönetilen diskleri kullan" özelliği "Evet" olarak belirlenmiş kullanılabilirlik kümelerinin parçası olmasıdır. "Yönetilen diskleri kullan" özelliğiyle kullanılabilirlik kümelerini, yük devretme sırasında yönetilen diskleri kullanma isteğinize göre belirlendiğinden emin olun.  Benzer şekilde "Yönetilen diskleri kullan" ayarını "Hayır" olarak belirlediğinizde yalnızca kaynak grubundaki "Yönetilen diskleri kullan" özelliği "Hayır" olarak belirlenmiş kullanılabilirlik kümeleri seçilebilir. [Yönetilen diskler ve kullanılabilirlik kümeleri hakkında daha fazla bilgi edinin](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/manage-availability#use-managed-disks-for-vms-in-an-availability-set).

  > [!NOTE]
  > Çoğaltma için kullanılan depolama hesabı herhangi bir zamanda Depolama Hizmeti Şifrelemesiyle şifrelendiyse yük devretme sırasında yönetilen disk oluşturma işlemi başarısız olacaktır. "Yönetilen diskleri kullan" özelliğini "Hayır" olarak belirleyip yük devretmeyi tekrar deneyebilir veya sanal makine için korumayı devre dışı bırakıp Depolama Hizmeti Şifrelemesi etkinleştirilmemiş bir depolama hesabında korumaya alabilirsiniz.
  > [Depolama Hizmeti Şifrelemesi ve yönetilen diskler hakkında daha fazla bilgi edinin](https://docs.microsoft.com/en-us/azure/storage/storage-managed-disks-overview#managed-disks-and-encryption).


## <a name="test-the-deployment"></a>Dağıtımı test etme

Dağıtımı test etmek için tek bir sanal makine için yük devretme testi veya bir ya da daha fazla sanal makine içeren bir kurtarma planı çalıştırabilirsiniz.

### <a name="before-you-start"></a>Başlamadan önce

 - Yük devretmeden sonra RDP kullanarak Azure VM'lerine bağlanmak isterseniz [bağlanmaya hazırlanma](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover) hakkında bilgi edinin.
 - Tam anlamıyla test etmek için Active Directory ve DNS'in bir kopyasını test ortamınızda bulundurmanız gerekir. [Daha fazla bilgi edinin](site-recovery-active-directory.md#test-failover-considerations).

### <a name="run-a-test-failover"></a>Yük devretme testi çalıştırma

1. Tek bir VM’de yük devretme için **Çoğaltılan Öğeler** altında VM > **+Yük Devretme Testi**’ne tıklayın.
2. Kurtarma planında yük devretme için **Kurtarma Planları** seçeneklerinde plana sağ tıklayıp **Yük Devretme Testi**'ne tıklayın. Kurtarma planı oluşturmak için [aşağıdaki yönergeleri uygulayın](site-recovery-create-recovery-plans.md).
3. **Yük Devretme Testi** kısmında, yük devretme gerçekleştikten sonra Azure VM'lerinin bağlandığı Azure ağını seçin.
4. Yük devretmeyi başlatmak için **Tamam**'a tıklayın. Özelliklerini açmak için VM’e tıklayarak veya **Site Recovery işleri** seçeneklerinde **Yük Devretme Testi**’ne tıklayarak ilerlemeyi izleyebilirsiniz.
5. Yük devretme tamamlandıktan sonra çoğaltılan makineyi Azure portalı > **Sanal Makineler** kısmında da görmeniz gerekir. VM'nin uygun boyutta olduğundan, uygun bir ağa bağlandığından ve çalıştığından emin olmanız gerekir.
6. Yük devretme sonrasındaki bağlantılar için hazırlık yaptıysanız Azure VM'ye bağlanabilmeniz gerekir.
7. İşiniz bittiğinde kurtarma planındaki **Temizleme testi yük devretme** öğesine tıklayın. Yük devretme testiyle ilişkili gözlemlerinizi kaydetmek ve saklamak için **Notlar**'a tıklayın. Bunun yapılması, yük devretme testi sırasında oluşturulan sanal makineleri siler.

Daha fazla ayrıntı için [Azure'a yük devretme testi](site-recovery-test-failover-to-azure.md) makalesini okuyun.

## <a name="monitor-the-deployment"></a>Dağıtımı izleme

Site Recovery dağıtımının durumunu, yapılandırma ayarlarını ve sistem durumunu izlemeniz için yapmanız gerekenler:

1. **Temel Bileşenler** panosuna erişmeniz için kasa adına tıklayın. Bu panoda Site Recovery işlerini, çoğaltma durumunu, kurtarma planlarını, sunucu sistem durumunu ve olayları izleyebilirsiniz.  **Temel Bileşenler** panosunu, Site Recovery ve Backup kasalarının durumu dahil olmak üzere sizin için en faydalı olan kutucukları ve düzenlemeleri gösterecek şekilde özelleştirebilirsiniz.

    ![Temel Bileşenler](./media/site-recovery-vmm-to-azure/essentials.png)
2. **Sistem Durumu** bölümünde yerinde sunuculardaki (VMM veya yapılandırma sunucuları) sorunları ve Site Recovery tarafından son 24 saat içinde tetiklenen olayları izleyebilirsiniz.
3. **Çoğaltılan Öğeler**, **Kurtarma Planları** ve **Site Recovery İşleri** kutucuklarında çoğaltma işlemini yönetebilir ve izleyebilirsiniz. **İşler** > **Site Recovery İşleri** seçeneklerinden işlerin ayrıntılarına ulaşabilirsiniz.

## <a name="command-line-installation-for-the-azure-site-recovery-provider"></a>Azure Site Recovery Sağlayıcısı'na yönelik komut satırı yüklemesi

Azure Site Recovery Sağlayıcısı komut satırından yüklenebilir. Bu yöntem, Sağlayıcı'yı Windows Server 2012 R2 için Sunucu Çekirdeği'nde yüklerken kullanılabilir.

1. Sağlayıcı yükleme dosyasını ve kayıt anahtarını bir klasöre indirin. Örneğin, C:\ASR.
2. Yükseltilmiş bir komut isteminde, Sağlayıcı yükleyicisini ayıklamak için şu komutları çalıştırın:

            C:\Windows\System32> CD C:\ASR
            C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
3. Bileşenleri yüklemek için şu komutu çalıştırın:

            C:\ASR> setupdr.exe /i
4. Ardından sunucuyu kasaya kaydetmek için şu komutları çalıştırın:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>       

Konumlar:

* **/Credentials**: Kayıt anahtarı dosyasının bulunduğu konumu belirten zorunlu parametre.  
* **/FriendlyName**: Azure Site Recovery portalında görünen Hyper-V konak sunucusunun adı için zorunlu parametre.
* * **/EncryptionEnabled**: VMM bulutlarındaki Hyper-V VM'lerini Azure'a çoğaltırken kullanılan isteğe bağlı parametre. Sanal makineleri Azure'da şifrelemek isterseniz bunu kullanın (bekleyen şifreleme). Dosya adının **.pfx** uzantısını içerdiğinden emin olun. Varsayılan olarak şifreleme kapalıdır.
* **/proxyAddress**: Ara sunucunun adresini belirten isteğe bağlı parametre.
* **/proxyport**: Ara sunucunun bağlantı noktasını belirten isteğe bağlı parametre.
* **/proxyUsername**Ara sunucu kullanıcı adını belirten isteğe bağlı parametre (ara sunucu kimlik doğrulaması gerekiyorsa).
* **/proxyPassword**: Ara sunucu ile kimlik doğrulamak için kullanılacak parolayı belirten isteğe bağlı parametre (ara sunucu kimlik doğrulaması gerekiyorsa).


### <a name="network-bandwidth-considerations"></a>Ağ bandı genişliği ile ilgili dikkat edilmesi gerekenler
Çoğaltma (ilk çoğaltma ve ardından değişim) için gereken bant genişliğini hesaplamak üzere Capacity Planner'ı kullanabilirsiniz. Çoğaltmada kullanılan bant genişliği miktarını kontrol etmek için birkaç seçenek sunulur:

* **Bant genişliğini kısıtlama**: İkincil bir siteye çoğaltılan Hyper-V trafiği belirli bir Hyper-V konağı üzerinden geçer. Ana bilgisayar sunucusunda bant genişliğini kısıtlayabilirsiniz.
* **Bant genişliğine ince ayar uygulama**: Birkaç kayıt defteri anahtarı kullanarak çoğaltma için kullanılan bant genişliği üzerinde etki oluşturabilirsiniz.

#### <a name="throttle-bandwidth"></a>Bant genişliğini kısıtlama
1. Hyper-V ana bilgisayar sunucusunda Microsoft Azure Backup MMC ek bileşenini açın. Varsayılan olarak Microsoft Azure Backup için masaüstünde veya C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin yolunda bir kısayol bulunur.
2. Ek bileşende **Özellikleri Değiştir**'e tıklayın.
3. **Azaltma** sekmesinde **Yedekleme işlemleri için internet bant genişliği kullanımını azaltmayı etkinleştir** seçeneğini belirleyin ve çalışma saatleri ve çalışma dışı saatler için sınırları ayarlayın. Geçerli aralıklar saniye başına 512 Kbps ila 102 Mbps arasındadır.

    ![Bant genişliğini kısıtlama](./media/site-recovery-vmm-to-azure/throttle2.png)

Ayrıca, azaltma ayarı için [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) cmdlet'ini de kullanabilirsiniz. Bu ayara ilişkin örneği aşağıda bulabilirsiniz:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Set-OBMachineSetting - NoThrottle** hiçbir azaltmanın gerekli olmadığını gösterir.

#### <a name="influence-network-bandwidth"></a>Ağ bant genişliği üzerinde etki oluşturma
**UploadThreadsPerVM** kayıt defteri değeri, bir diskin veri aktarımı (başlangıç ve değişim çoğaltması) için kullanılan iş parçacıklarının sayısını denetler. Daha yüksek bir değer, çoğaltma işlemi için kullanılan ağ bant genişliğini artırır. **DownloadThreadsPerVM** kayıt defteri değeri, yeniden çalışma sırasındaki veri aktarımı için kullanılan iş parçacıklarının sayısını belirler.

1. Kayıt defterinde **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication** hedefine gidin.

   * Disk çoğaltması için kullanılan iş parçacıklarını denetlemek için **UploadThreadsPerVM** değerini değiştirin (veya anahtar yoksa anahtar oluşturun).
   * Azure'dan gelen yeniden çalışma trafiği için kullanılan iş parçacıklarını denetlemek için **DownloadThreadsPerVM** değerini değiştirin (veya anahtar yoksa anahtar oluşturun).
2. Varsayılan değer 4'tür. "Fazla sağlanan" bir ağda, bu kayıt defteri anahtarlarının varsayılan değerlerinin değiştirilmesi gerekir. Maksimum değer 32'dir. Değeri iyileştirmek için trafiği izleyin.

## <a name="next-steps"></a>Sonraki adımlar

İlk çoğaltma tamamlandıktan ve dağıtım test edildikten sonra ihtiyaca göre yük devretme gerçekleştirebilirsiniz. Farklı yük devretme türleri ve onları çalıştırma hakkında [daha fazla bilgi edinin](site-recovery-failover.md).

