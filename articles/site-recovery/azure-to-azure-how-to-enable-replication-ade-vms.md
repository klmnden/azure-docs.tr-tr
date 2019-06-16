---
title: Azure Site Recovery, Azure Disk şifrelemesi özellikli VM'ler için çoğaltma yapılandırma | Microsoft Docs
description: Bu makalede Site Recovery kullanarak çoğaltma için Azure Disk şifrelemesi özellikli VM'ler bir Azure bölgesinden diğerine yapılandırma açıklanır.
services: site-recovery
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 04/08/2019
ms.author: sutalasi
ms.openlocfilehash: 4943b730bb46ee00200d84faf95a7ccb069d3aa8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60791018"
---
# <a name="replicate-azure-disk-encryption-enabled-virtual-machines-to-another-azure-region"></a>Azure Disk şifrelemesi etkinleştirilmiş sanal makineleri başka bir Azure bölgesine çoğaltma

Bu makalede, Azure Disk şifrelemesi özellikli VM'ler bir Azure bölgesinden diğerine çoğaltın açıklar.

>[!NOTE]
>Azure Site Recovery şu anda yalnızca Azure Windows işletim sistemi çalıştıran ve olan Vm'leri destekler [Azure Active Directory (Azure AD) ile şifreleme etkin](https://aka.ms/ade-aad-app).

## <a name="required-user-permissions"></a>Kullanıcı gerekli izinleri
Site Recovery hedef bölge ve kopyalama anahtarları bölgeye anahtar kasasını oluşturmak için gerekli izinlere sahip olmasını gerektirir.

Disk şifrelemesi özellikli VM'ler Azure portalından çoğaltmayı etkinleştirmek için kullanıcı için şu izinler gereklidir:

- Anahtar kasası izinleri
    - Liste
    - Create
    - Al

-   Anahtar kasası gizli dizi izinleri
    - Liste
    - Create
    - Al

- Anahtar kasası anahtar izinleri (yalnızca VM'lerin disk şifreleme anahtarlarını şifrelemek için anahtar şifreleme anahtarı kullanırsanız, gerekli)
    - Liste
    - Al
    - Create
    - Şifreleme
    - Şifre Çözme

İzinleri yönetmek için Portalı'nda anahtar kasası kaynak gidin. Kullanıcı için gerekli izinleri ekleyin. Aşağıdaki örnek, anahtar kasası için izinleri etkinleştirmek üzere gösterilmektedir *ContosoWeb2Keyvault*, kaynak bölgede olduğu.

1. Git **giriş** > **Keyvaults** > **ContosoWeb2KeyVault > erişim ilkeleri**.

   ![Anahtar kasası izinler penceresi](./media/azure-to-azure-how-to-enable-replication-ade-vms/key-vault-permission-1.png)

2. Hiçbir kullanıcı izinleri olduğunu görebilirsiniz. Seçin **yeni Ekle**. Kullanıcı ve izinleri bilgileri girin.

   ![keyvault izinleri](./media/azure-to-azure-how-to-enable-replication-ade-vms/key-vault-permission-2.png)

Olağanüstü Durum Kurtarma (DR) etkinleştirme kullanıcı anahtarları kopyalamak için izinleri yoksa, uygun izinlere sahip bir güvenlik yönetici anahtarları ve gizli şifreleme anahtarları için hedef bölgede kopyalamak için aşağıdaki betiği kullanabilirsiniz.

İzinleri gidermek için başvurmak [anahtar kasası izin sorunları](#trusted-root-certificates-error-code-151066) bu makalenin ilerleyen bölümlerinde.

>[!NOTE]
>Disk şifrelemesi özellikli VM'ler portalından, çoğaltmayı etkinleştirmek için en az "anahtar kasalarını, gizli dizileri ve anahtarları izinlerini List".

## <a name="copy-disk-encryption-keys-to-the-dr-region-by-using-the-powershell-script"></a>PowerShell betiğini kullanarak disk şifreleme anahtarları için DR bölgesindeki kopyalayın

1. ["CopyKeys" ham komut dosyası kodunu açmak](https://aka.ms/ade-asr-copy-keys-code).
2. Betik bir dosyaya kopyalayın ve adlandırın **kopyalama keys.ps1**.
3. Windows PowerShell uygulamasını açın ve dosyayı kaydettiğiniz klasöre gidin.
4. Kopyalama keys.ps1 yürütün.
5. Oturum açmak için Azure kimlik bilgilerini sağlayın.
6. Seçin **Azure aboneliği** sanal makinelerinizin.
7. Yüklemek kaynak gruplar için bekleyin ve ardından **kaynak grubu** sanal makinelerinizin.
8. Vm'leri görüntülenen listeden seçin. Disk şifrelemesi için etkinleştirilmiş olan VM'ler listede yer.
9. Seçin **hedef konum**.

    - **Disk şifreleme anahtar kasası**
    - **Anahtar şifreleme anahtar kasası**

   Varsayılan olarak, Site Recovery, hedef bölgede yeni bir anahtar kasası oluşturur. Kasanın adı, kaynak VM disk şifreleme anahtara göre bir "asr" sonekine sahip. Site Recovery tarafından oluşturulmuş bir anahtar kasası zaten var, yeniden kullanılır. Farklı bir anahtar kasası gerekirse listeden seçin.

## <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

Bu örnekte, Birincil Azure bölge Doğu Asya ve ikincil bölgeye Güneydoğu Asya.

1. Kasada seçin **+ Çoğalt**.
2. Aşağıdaki alanları unutmayın.
    - **Kaynak**: Bu durumda sanal makinelerin başlangıç noktasını **Azure**.
    - **Kaynak konumu**: Sanal makinelerinizi korumak istediğiniz Azure bölgesi. Bu örnekte, "Doğu Asya" kaynak konumu
    - **Dağıtım modeli**: Kaynak makineler Azure dağıtım modeli.
    - **Kaynak abonelik**: Kaynak sanal makinelerinize ait olduğu abonelik. Bu, Kurtarma Hizmetleri kasasıyla aynı Azure Active Directory kiracısı olan herhangi bir aboneliği olabilir.
    - **Kaynak grubu**: Kaynak sanal makinelerinize ait olduğu kaynak grubu. Sonraki adımda koruma için seçilen kaynak grubundaki tüm sanal makineler listelenmektedir.

3. İçinde **sanal makineler** > **sanal makineleri**, çoğaltmak istediğiniz her bir sanal Makineyi seçin. Yalnızca çoğaltmanın etkinleştirildiği makineleri seçebilirsiniz. Sonra **Tamam**’ı seçin.

4. İçinde **ayarları**, aşağıdaki hedef site ayarlarını yapılandırabilirsiniz.

    - **Hedef konum**: Kaynak sanal makine verilerinizi burada çoğaltılır konumu. Site Recovery seçili makinenin konumuna göre uygun hedef bölgelerin bir listesini sağlar. Kurtarma Hizmetleri kasasının konumu olarak aynı konumu kullanmanızı öneririz.
    - **Hedef abonelik**: Olağanüstü durum kurtarma için kullanılan hedef aboneliği. Varsayılan olarak, hedef abonelik kaynak abonelik ile aynıdır.
    - **Hedef kaynak grubu**: Kaynak grubu için tüm çoğaltılan sanal makinelerinize ait. Varsayılan olarak, Site Recovery, hedef bölgede yeni bir kaynak grubu oluşturur. "Asr" sonekine adını alır. Azure Site Recovery tarafından oluşturulan bir kaynak grubu zaten var, yeniden kullanılır. Ayrıca, özelleştirmek aşağıdaki bölümde gösterildiği gibi seçebilirsiniz. Hedef kaynak grubu konumunu, kaynak sanal makineleri barındırıldığı bölgeyi dışında tüm Azure bölgelerine olabilir.
    - **Hedef sanal ağ**: Varsayılan olarak, Site Recovery, hedef bölgede yeni bir sanal ağ oluşturur. "Asr" sonekine adını alır. Kaynak ağa eşlenen ve gelecekteki tüm koruma için kullanılan. [Daha fazla bilgi edinin](site-recovery-network-mapping-azure-to-azure.md) ağ eşlemesi hakkında.
    - **Hedef depolama hesapları (VM kullanmaz, kaynak yönetilen diskleri kullanmıyorsa)** : Varsayılan olarak, Site Recovery, kaynak VM depolama yapılandırmanızı yakından taklit eden yeni bir hedef depolama hesabı oluşturur. Bir depolama hesabı zaten varsa, yeniden kullanılır.
    - **Yönetilen çoğaltma diskleri (kaynak sanal makine yönetilen diskleri kullanıyorsa)** : Site Recovery, kaynak sanal makinenin yönetilen diskleri aynı depolama türü (standart veya premium) olarak kaynak sanal makinenin yönetilen disklerini yansıtmak için hedef bölgede yeni yönetilen çoğaltma diskleri oluşturur.
    - **Önbellek depolama hesapları**: Site kurtarma adlı bir ek depolama hesabı gerekir *önbellek depolama* kaynak bölgede. Kaynak VM'lerin üzerindeki tüm değişiklikleri izlenir ve önbellek depolama hesabına gönderilir. Ardından hedef konuma çoğaltılır.
    - **Kullanılabilirlik kümesi**: Varsayılan olarak, Site Recovery, hedef bölgede yeni bir kullanılabilirlik oluşturur. Adı "asr" sonekine sahip. Site Recovery tarafından zaten oluşturulmuş bir kullanılabilirlik kümesi varsa, yeniden kullanılır.
    - **Disk şifreleme anahtar kasalarını**: Varsayılan olarak, Site Recovery, hedef bölgede yeni bir anahtar kasası oluşturur. Bu, kaynak VM disk şifreleme anahtara göre bir "asr" sonekine sahip. Azure Site Recovery tarafından zaten oluşturulmuş bir anahtar kasası zaten varsa, yeniden kullanılır.
    - **Anahtar şifreleme anahtar kasalarını**: Varsayılan olarak, Site Recovery, hedef bölgede yeni bir anahtar kasası oluşturur. Adı, kaynak VM anahtar şifreleme anahtara göre bir "asr" sonekine sahip. Azure Site Recovery tarafından önceden oluşturulmuş bir anahtar kasası zaten varsa, yeniden kullanılır.
    - **Çoğaltma İlkesi**: Kurtarma noktası bekletme geçmişine ve uygulamayla tutarlı anlık görüntü sıklığı ayarlarını tanımlar. Varsayılan olarak, Site Recovery varsayılan ayarlarla yeni bir çoğaltma ilkesi oluşturur *24 saat* için kurtarma noktası bekletme ve *60 dakika* için uygulamayla tutarlı anlık görüntü sıklığı.

## <a name="customize-target-resources"></a>Hedef kaynakları özelleştirme

Site Recovery varsayılan hedef ayarlarını değiştirmek için aşağıdaki adımları izleyin.

1. Seçin **Özelleştir** "Hedef abonelik" yanındaki varsayılan hedef aboneliği değiştirmek için. Azure AD kiracısında mevcut olan aboneliklerden listeden aboneliği seçin.

2. Seçin **Özelleştir** yanındaki "kaynak grubu, ağ, depolama ve kullanılabilirlik kümeleri" aşağıdaki varsayılan ayarlarını değiştirmek için:
    - İçin **hedef kaynak grubu**, aboneliğin hedef konumda kaynak grupları listesinden kaynak grubunu seçin.
    - İçin **hedef sanal ağ**, hedef konumdaki sanal ağlar listesinden bir ağ seçin.
    - İçin **kullanılabilirlik kümesi**, bir kullanılabilirlik kümesi kaynak bölgede parçası iseler VM kullanılabilirlik kümesi ayarları ekleyebilirsiniz.
    - İçin **hedef depolama hesapları**, kullanılacak hesabı seçin.

2. Seçin **Özelleştir** "Şifreleme ayarları" yanında aşağıdaki varsayılan ayarlarını değiştirmek için:
   - İçin **hedef disk şifreleme anahtar kasası**, hedef disk şifreleme anahtar kasası anahtar kasalarını abonelik hedef konumda listesinden seçin.
   - İçin **hedef anahtar şifreleme anahtar kasası**, hedef konum abonelik içindeki anahtar kasalarını listesinden hedef anahtar şifreleme anahtar kasasını seçin.

3. Seçin **hedef kaynak oluşturma** > **çoğaltmayı etkinleştirme**.
4. VM'ler için çoğaltma etkinleştirildikten sonra altında sanal makinelerin sistem durumunu kontrol edebilirsiniz **çoğaltılan öğeler**.

>[!NOTE]
>İlk çoğaltma sırasında durumunu, görünen ilerleme yenilemek için biraz zaman alabilir. Tıklayın **Yenile** son durumu almak için.

## <a name="update-target-vm-encryption-settings"></a>Hedef VM şifreleme ayarlarını güncelleştirme
Aşağıdaki senaryolarda, hedef VM şifreleme ayarları güncelleştirmeniz gerekecek:
  - Sanal makinedeki Site Recovery çoğaltması etkin. Daha sonra kaynak VM'deki disk şifrelemesi etkin.
  - Sanal makinedeki Site Recovery çoğaltması etkin. Daha sonra disk şifreleme anahtarı veya anahtar şifreleme anahtarı ' % s'kaynak VM üzerinde değiştirildi.

Kullanabileceğiniz [bir betik](#copy-disk-encryption-keys-to-the-dr-region-by-using-the-powershell-script) şifreleme anahtarları için hedef bölgede kopyalayın ve ardından hedef şifreleme ayarları güncelleştirmek için **kurtarma Hizmetleri kasası** > *çoğaltılan öğe*  >  **Özellikleri** > **işlem ve ağ**.

![Güncelleştirme ADE Ayarları iletişim penceresi](./media/azure-to-azure-how-to-enable-replication-ade-vms/update-ade-settings.png)

## <a id="trusted-root-certificates-error-code-151066"></a>Azure'dan Azure'a VM çoğaltma sırasında anahtar kasası izin sorunları giderme

**1. neden:** Hedef bölgesi zaten oluşturulan ve Site Recovery oluşturmak izin vermek yerine gerekli izinlere sahip değil bir anahtar kasası seçmiş olabilirsiniz. Anahtar kasası olduğundan emin olun daha önce açıklanan şekilde izinleri gerektirir.

*Örneğin*: Anahtar kasası olduğundan bir VM'yi çoğaltma çalıştığınızda *ContososourceKeyvault* kaynak bölge.
Kaynak bölge key vault tüm izinlere sahip. Ancak, önceden oluşturulmuş anahtar kasası iznine sahip değil. ContosotargetKeyvault, seçtiğiniz koruma sırasında. Bir hata oluşur.

**Nasıl:** Git **giriş** > **Keyvaults** > **ContososourceKeyvault** > **erişim ilkeleri** ve uygun izinleri ekleyin.

**2. neden:** Önceden oluşturulmuş ve sahip olmayan bir anahtar kasası hedef bölgesi seçmiş olabilirsiniz izinlerine oluşturun Site Recovery izin vererek yerine şifresini çözme şifreleyin. Sahip olduğunuzdan emin olun şifresini çözme-izinleri de kaynak bölgeye anahtarı şifreleme şifreleyebilirsiniz.</br>

*Örneğin*: Bir anahtar kasası olduğundan bir VM'yi çoğaltma çalıştığınızda *ContososourceKeyvault* kaynak bölge. Kaynak bölge key vault tüm gerekli izni var. Ancak, önceden oluşturduğunuz anahtar kasasının şifrelemek ve şifresini çözmek için gerekli izinlere sahip değil ContosotargetKeyvault, seçtiğiniz koruma sırasında. Bir hata oluşur.</br>

**Nasıl:** Git **giriş** > **Keyvaults** > **ContososourceKeyvault** > **erişim ilkeleri**. İzinler altında ekleme **anahtar izinleri** > **şifreleme işlemleri**.

## <a name="next-steps"></a>Sonraki adımlar

[Daha fazla bilgi edinin](site-recovery-test-failover-to-azure.md) yük devretme testi çalıştırma hakkında.
