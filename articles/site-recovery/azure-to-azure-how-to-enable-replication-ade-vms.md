---
title: Azure Site Recovery, Azure disk şifrelemesi (ADE) etkin çoğaltma Vm'leri yapılandırma | Microsoft Docs
description: Bu makalede, bir Azure bölgesinden diğerine Site RECOVERY'yi kullanarak ADE Vm'leri için çoğaltmayı yapılandırmak açıklar.
services: site-recovery
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 04/08/2019
ms.author: sutalasi
ms.openlocfilehash: b3e997a37bb5d030d559b6771b2c0e2f74cc62ab
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59277706"
---
# <a name="replicate-azure-disk-encryption-ade-enabled-virtual-machines-to-another-azure-region"></a>Azure disk şifrelemesi (ADE) etkinleştirilmiş sanal makineleri başka bir Azure bölgesine çoğaltma

Bu makalede, Azure disk şifrelemesi (ADE) etkin çoğaltma Vm'leri, bir Azure bölgesinden diğerine etkinleştirmeyi açıklar.

>[!NOTE]
>Azure Site Recovery şu an için yalnızca Windows işletim sistemini kullanan ve [Azure AD uygulamasıyla şifreleme için etkinleştirilmiş olan](https://aka.ms/ade-aad-app) Azure VM'lerini desteklemektedir.
>

## <a name="required-user-permissions"></a>Kullanıcı gerekli izinleri
Azure Site Recovery hedef bölge ve kopyalama anahtarları bölgeye anahtar kasası oluşturma iznine sahip olmasını gerektirir.

Kullanıcı Portalı'ndan ADE VM çoğaltmayı etkinleştirmek için olmalıdır aşağıdaki izinlerin.
- Anahtar kasası izinleri
    - list
    - Oluştur
    - Al

-   Anahtar kasası gizli dizi izinleri
    - Liste
    - Oluştur
    - Al

- Anahtar kasası anahtar izinleri (yalnızca VM'lerin Disk şifreleme anahtarlarını şifrelemek için anahtar şifreleme anahtarı kullanırsanız, gerekli)
    - Liste
    - Al
    - Oluştur
    - Şifreleme
    - Şifre Çözme

Anahtar kasası kaynak portalında gezinme ve kullanıcıya gerekli izinleri ekleyerek izinleri yönetebilirsiniz. Örneğin: adım adım kılavuz kaynak bölgede olan "ContosoWeb2Keyvault", Key vault için nasıl etkinleştirileceğini göstermektedir.


-  Gidin "Giriş > Keyvaults > ContosoWeb2KeyVault > erişim ilkeleri"

![keyvault izinleri](./media/azure-to-azure-how-to-enable-replication-ade-vms/key-vault-permission-1.png)



- Var. Bu nedenle hiçbir kullanıcı persimmon "Yeni Ekle" ve kullanıcı ve izinleri tıklayarak yukarıda belirtilen izni Ekle görebilirsiniz.

![keyvault izinleri](./media/azure-to-azure-how-to-enable-replication-ade-vms/key-vault-permission-2.png)

Olağanüstü Durum Kurtarma (DR) etkinleştirme kullanıcı anahtarları, kopyalayabilir için gerekli izinlere sahip değilse güvenlik yönetici anahtarları ve gizli şifreleme anahtarları için hedef bölgede kopyalamak için uygun izinlere sahip aşağıdaki betiği verilebilir.

Başvuru [bu makalede](#trusted-root-certificates-error-code-151066) izin sorunları gidermek için.
>[!NOTE]
>Portaldan ADE VM çoğaltmayı etkinleştirmek için en az anahtar kasası gizli dizileri ve anahtarları "List" izinlerini gerekir
>

## <a name="copy-ade-keys-to-dr-region-using-powershell-script"></a>PowerShell betiğini kullanarak DR bölgesindeki kopyalama ADE anahtarları

1. Tıklayarak 'CopyKeys' ham komut dosyası kodu bir tarayıcı penceresinde açın [bu bağlantıyı](https://aka.ms/ade-asr-copy-keys-code).
2. Betik bir dosyaya kopyalayın ve adlandırın ' kopyalama-keys.ps1'.
2. Windows PowerShell uygulamasını açın ve dosyanın bulunduğu klasörü konumuna gidin.
3. Launch 'Copy-keys.ps1'
4. Azure oturum açma kimlik bilgilerini sağlayın.
5. Seçin **Azure aboneliği** sanal makinelerinizin.
6. Kaynak grupları yükleyin ve ardından bekleyin **kaynak grubu** sanal makinelerinizin.
7. Vm'leri görüntülenen VM'ler listesinden seçin. Yalnızca VM'ler ile Azure disk şifrelemesi etkin listesinde gösterilir.
8. Seçin **hedef konum**.
9. **Disk şifreleme anahtar kasalarını**: Varsayılan olarak, Azure Site Recovery kaynak VM disk şifreleme anahtara göre "asr" sonekine sahip adı ile hedef bölgede yeni bir anahtar kasası oluşturur. Azure Site Recovery tarafından oluşturulmuş bir anahtar kasası varsa yeniden kullanılır. Farklı bir anahtar kasası gerekirse listeden seçebilirsiniz.
10. **Anahtar şifreleme anahtar kasalarını**: Varsayılan olarak, Azure Site Recovery kaynak VM anahtar şifreleme anahtara göre "asr" sonekine sahip adı ile hedef bölgede yeni bir anahtar kasası oluşturur. Azure Site Recovery tarafından oluşturulmuş bir anahtar kasası varsa yeniden kullanılır. Farklı bir anahtar kasası gerekirse listeden seçebilirsiniz.

## <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

Bu yordam, Doğu Asya Birincil Azure bölgesi olduğunu ve ikincil bölgeye Güney Doğu Asya olduğunu varsayar.

1. Kasaya tıklayın **+ Çoğalt**.
2. Aşağıdaki alanları dikkat edin:
    - **Kaynak**: Bu durumda sanal makinelerin başlangıç noktasını **Azure**.
    - **Kaynak konumu**: Sanal makinelerinizi korumak istediğiniz Azure bölgesi. Bu çizim için 'Doğu Asya' kaynak konumdur
    - **Dağıtım modeli**: Kaynak makineler Azure dağıtım modeli.
    - **Kaynak abonelik**: Kaynak sanal makinelerinize ait olduğu abonelik. Bu abonelik, kurtarma hizmetleri kasanızın bulunduğu Azure Active Directory kiracısında bulunan aboneliklerden biri olabilir.
    - **Kaynak grubu**: Kaynak sanal makinelerinize ait olduğu kaynak grubu. Sonraki adımda koruma için seçilen kaynak grubu altındaki tüm sanal makineler listelenmektedir.

3. İçinde **sanal makineler > sanal makineleri**tıklayın ve çoğaltmak istediğiniz her bir sanal Makineyi seçin. Yalnızca çoğaltmanın etkinleştirildiği makineleri seçebilirsiniz. ' A tıklayarak **Tamam**.

4. İçinde **ayarları**, isteğe bağlı olarak hedef site ayarları yapılandırabilirsiniz:

    - **Hedef konum**: Kaynak sanal makine verilerinizi burada çoğaltılır konumu. Seçili makineler konumuna bağlı olarak Site Recovery, uygun hedef bölgelerin listesini sağlar. Kurtarma Hizmetleri kasası konumu olarak aynı hedef konum tutmanızı öneririz.
    - **Hedef abonelik**: Olağanüstü durum kurtarma için kullanılan hedef aboneliği. Hedef abonelik varsayılan olarak kaynak abonelikle aynı olur.
    - **Hedef kaynak grubu**: Kaynak grubu için tüm çoğaltılan sanal makinelerinize ait. Varsayılan olarak Azure Site Recovery "asr" sonekine sahip adı ile hedef bölgede yeni bir kaynak grubu oluşturur. Azure Site Recovery tarafından zaten oluşturulan kaynak grubunun mevcut olması durumunda, yeniden kullanılır. Aşağıdaki bölümde gösterildiği gibi özelleştirmek seçebilirsiniz. Hedef kaynak grubu konumunu, kaynak sanal makineleri barındırdığı bölge dışında tüm Azure bölgelerine olabilir.
    - **Hedef sanal ağ**: Varsayılan olarak, Site Recovery "asr" sonekine sahip adı ile hedef bölgede yeni bir sanal ağ oluşturur. Bu kaynak ağa eşlenen ve gelecekteki tüm koruma için kullanılır. [Daha fazla bilgi edinin](site-recovery-network-mapping-azure-to-azure.md) ağ eşlemesi hakkında.
    - **Depolama hesapları (kaynak makine yönetilen diskleri kullanmıyorsa) hedef**: Varsayılan olarak, Site Recovery, kaynak VM depolama yapılandırması yakından taklit eden yeni bir hedef depolama hesabı oluşturur. Depolama hesabı zaten mevcut olması durumunda, yeniden kullanılır.
    - **Yönetilen çoğaltma diskleri (kaynak sanal makine yönetilen diskleri kullanıyorsa)**: Site Recovery kaynak sanal makinenin yönetilen disklerle aynı depolama türüne (standart veya premium) kaynağın VM'ye yönetilen disk olarak yansıtmak için hedef bölgede yeni yönetilen çoğaltma diskleri oluşturur.
    - **Önbellek depolama hesapları**: Site Recovery kaynak bölgede önbellek depolama adlı ek bir depolama hesabı gerekir. Kaynak VM üzerinde'olmuyor tüm değişiklikleri izlenir ve bu hedef konuma çoğaltılmadan önce önbellek depolama hesabına gönderilir.
    - **Kullanılabilirlik kümesi**: Varsayılan olarak, Azure Site Recovery, hedef bölgede "asr" sonekine sahip adıyla ayarlanmış yeni bir kullanılabilirlik oluşturur. Kullanılabilirlik kümesi zaten Azure Site Recovery tarafından oluşturulan mevcut durumda yeniden kullanılır.
    - **Disk şifreleme anahtar kasalarını**: Varsayılan olarak, Azure Site Recovery kaynak VM disk şifreleme anahtara göre "asr" sonekine sahip adı ile hedef bölgede yeni bir anahtar kasası oluşturur. Azure Site Recovery tarafından oluşturulmuş bir anahtar kasası varsa yeniden kullanılır.
    - **Anahtar şifreleme anahtar kasalarını**: Varsayılan olarak, Azure Site Recovery kaynak VM anahtar şifreleme anahtara göre "asr" sonekine sahip adı ile hedef bölgede yeni bir anahtar kasası oluşturur. Azure Site Recovery tarafından oluşturulmuş bir anahtar kasası varsa yeniden kullanılır.
    - **Çoğaltma İlkesi**: Kurtarma noktası bekletme geçmişine ve uygulama tutarlı anlık görüntü sıklığı ayarlarını tanımlar. Varsayılan olarak, Azure Site Recovery, ' 24 saatliğine kurtarma noktası bekletme ' 60 dakika için uygulamayla tutarlı anlık görüntü sıklığını ve varsayılan ayarlarla yeni bir çoğaltma ilkesi oluşturur.



## <a name="customize-target-resources"></a>Hedef kaynakları özelleştirme

Site Recovery tarafından kullanılan varsayılan hedef ayarlarını değiştirebilirsiniz.


1. Tıklayın **Özelleştir:** yanındaki 'Hedef aboneliği' varsayılan hedef aboneliği değiştirmek için. Aynı Azure Active Directory (AAD) kiracısında mevcut tüm abonelikleri listeden aboneliği seçin.

2. Tıklayın **Özelleştir:** yanındaki ' değiştirmek için kaynak grubu, ağ, depolama ve kullanılabilirlik kümeleri varsayılan ayarları aşağıda:
    - İçinde **hedef kaynak grubu**, aboneliğin hedef konumdaki tüm kaynak grupları listesinden kaynak grubunu seçin.
    - İçinde **hedef sanal ağ**, ağ sanal ağ içinde hedef konum listesinden seçin.
    - İçinde **kullanılabilirlik kümesi**, bir kullanılabilirlik kümesi kaynak bölgede parçası iseler VM kullanılabilirlik kümesi ayarları ekleyebilirsiniz.
    - İçinde **hedef depolama hesapları**, kullanmak istediğiniz hesabı seçin.


2. Tıklayın **Özelleştir:** "Şifreleme ayarları" yanındaki değiştirmek için varsayılan ayarları aşağıda:
   - İçinde **hedef disk şifreleme anahtar kasası**, hedef konum abonelik içindeki tüm anahtar kasalarına listesinden hedef disk şifreleme anahtar Kasası'nı seçin.
     - İçinde **hedef anahtar şifreleme anahtar kasası**, hedef konum abonelik içindeki tüm anahtar kasalarına listesinden hedef anahtar şifreleme anahtar Kasası'nı seçin.

3. Tıklayın **hedef kaynak oluşturma** > **çoğaltmayı etkinleştirme**.
4. VM'ler için çoğaltma etkinleştirildikten sonra durumu VM sistem durumu ' kontrol edebilirsiniz **çoğaltılan öğeler**

>[!NOTE]
>İlk çoğaltma sırasında durum, ilerleme durumu yenilemek için biraz zaman alabilir. Tıklayın **Yenile** düğme, son durumu almak için.
>

## <a name="update-target-vm-encryption-settings"></a>Hedef VM şifreleme ayarlarını güncelleştirme
İçinde senaryolar, hedef VM şifreleme ayarlarını güncelleştirmeniz gerekir.
  - Sanal makinedeki Site Recovery çoğaltma etkin ve daha sonraki bir tarihte Azure Disk şifrelemesi (ADE) kaynak VM üzerinde etkin
  - Sanal makinedeki Site Recovery çoğaltma etkin ve daha sonraki bir tarihte disk şifreleme anahtarı ve/veya kaynak VM üzerinde anahtar şifreleme anahtarı değiştirildi

Kullanabileceğiniz [betik](#copy-ade-keys-to-dr-region-using-powershell-script) şifreleme anahtarları için hedef bölgede kopyalayın ve ardından hedef şifreleme ayarları güncelleştirmek için **kurtarma Hizmetleri kasası -> çoğaltılan öğe -> Özellikler -> işlem ve ağ.**

![Güncelleştirme ade ayarları](./media/azure-to-azure-how-to-enable-replication-ade-vms/update-ade-settings.png)

## <a name="trusted-root-certificates-error-code-151066"></a>Azure'dan Azure'a VM çoğaltma sırasında anahtar kasası izin sorunları giderme

**1. neden:** Önceden oluşturulmuş bir anahtar kasası gerekli izinlere sahip değil, hedef bölgesi seçmiş olabilirsiniz.
Önceden oluşturulmuş bir anahtar kasası, hedef bölgede seçiyorsunuz yerine, Azure Site Recovery oluşturmak istiyorum. Emin Key vault gerektiriyor yukarıda da belirtildiği gibi izinleri.</br>
*Örneğin*: Bir kullanıcı, bir anahtar kasası kaynak bölgede varsayalım "ContososourceKeyvault" olan bir VM'yi çoğaltma çalışın.
Kullanıcının tüm kaynak bölge key vault izni ancak belirliyor koruma sırasında önceden oluşturulmuş bir anahtar kasası "koruması sonra hangi izne sahip olmayan ContosotargetKeyvault", bir hata oluşturur.</br>
**Nasıl:** Gerekir "Giriş > Keyvaults > ContososourceKeyvault > erişim ilkeleri" yukarıda gösterildiği gibi izinleri ekleyin.

**2. neden:** Önceden oluşturulmuş seçtiğiniz izinleri Keyvault decry sahip olmayan hedef bölgedeki pt-şifreleme.
Önceden oluşturulmuş bir anahtar kasası, hedef bölgede seçiyorsunuz yerine, Azure Site Recovery oluşturmak istiyorum. Kullanıcı sahip olduğundan emin olun anahtarı çok kaynak bölgesi şifrelediğiniz durumunda izinleri şifresini çözme-şifreleme.</br>
*Örneğin*: Bir kullanıcı, bir anahtar kasası kaynak bölgede varsayalım "ContososourceKeyvault" olan bir VM'yi çoğaltma çalışın.
Kullanıcının tüm kaynak bölge key vault izni ancak koruma sırasında değil "ContosotargetKeyvault" şifrelemek ve şifresini çözmek için izni olan önceden oluşturulmuş bir anahtar kasası belirliyor.</br>
**Nasıl:** Gerekir "Giriş > Keyvaults > ContososourceKeyvault > erişim ilkeleri" ve anahtar izinleri altında izin Ekle > şifreleme işlemleri.

## <a name="next-steps"></a>Sonraki adımlar

[Daha fazla bilgi edinin](site-recovery-test-failover-to-azure.md) yük devretme testi çalıştırma hakkında.
