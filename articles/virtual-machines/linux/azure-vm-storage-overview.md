---
title: Azure Linux VM'ler ve Azure depolama | Microsoft Docs
description: "Azure standart ve Premium depolama ile yönetilen ve yönetilmeyen diskler ile Linux sanal makineleri açıklar."
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: vlivech
manager: timlt
editor: 
ms.assetid: d364c69e-0bd1-4f80-9838-bbc0a95af48c
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 2/7/2017
ms.author: rasquill
ms.openlocfilehash: 934580f6fcfdbff6e61626ed685459478559717d
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2017
---
# <a name="azure-and-linux-vm-storage"></a>Azure ve Linux VM depolama
Azure Storage, müşterilerin ihtiyaçlarını karşılamak üzere sağlamlık, kullanılabilirlik ve ölçeklenebilirliğe dayanan modern uygulamalara yönelik bulut depolama çözümüdür.  Geliştiricilerin yeni senaryoları desteklemek için büyük ölçekli uygulamalar oluşturmalarını mümkün kolaylaştırarak yanı sıra, Azure Storage ayrıca depolama foundation Azure sanal makineler için olarak sağlar.

## <a name="managed-disks"></a>Yönetilen Diskler

Kullanılabilir kullanarak Azure VM'ler şimdi olan [Azure yönetilen diskleri](../windows/managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), oluşturma veya herhangi bir yönetme Vm'leriniz oluşturmanıza olanak sağlayan [Azure depolama hesapları](../../storage/common/storage-introduction.md) kendiniz. İstediğiniz Premium veya standart depolama ve disk ne kadar büyük olmalıdır ve Azure VM diskleri sizin için oluşturduğu olup olmadığını belirtin. Yönetilen disklerle VM'ler dahil olmak üzere birçok önemli özelliklere sahiptir:

- Otomatik ölçeklenebilirlik desteği. Azure diskleri oluşturur ve abonelik başına en fazla 10.000 diskleri desteklemek için temel alınan depolama alanı yönetir.
- Daha fazla güvenilirlik kullanılabilirlik kümeleri ile. Azure VM diskleri otomatik olarak birbirinden kullanılabilirlik kümeleri içinde yalıtılmış olduğundan emin olunmasını sağlar.
- Daha fazla erişim denetimi. Yönetilen disk işlemleri tarafından denetlenen çeşitli kullanıma [Azure rol tabanlı erişim denetimi (RBAC)](../../active-directory/role-based-access-control-what-is.md).

Yönetilen diskler için fiyatlandırma, yönetilmeyen diskleri için farklıdır. Bu bilgi için bkz: [fiyatlandırma ve faturalama yönetilen disklerde](../windows/managed-disks-overview.md#pricing-and-billing).

Yönetilmeyen diskleri yönetilen disklerle kullanmayı var olan VM'ler dönüştürebilirsiniz [az vm dönüştürme](/cli/azure/vm#convert). Daha fazla bilgi için bkz: [bir Linux VM yönetilmeyen disklerden yönetilen Azure disklere dönüştürme](convert-unmanaged-to-managed-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Yönetilmeyen disk veya herhangi bir zamanda bırakıldı, kullanılarak şifrelenmiş bir depolama hesabında ise, bir yönetilmeyen disk yönetilen bir diske dönüştüremezsiniz [Azure depolama hizmeti şifreleme (SSE)](../../storage/common/storage-service-encryption.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Aşağıdaki adımlar için veya bir şifrelenmiş depolama hesabında bırakıldı yönetilmeyen diskleri dönüştürme vermektedir:

- Sanal sabit disk (VHD) ile kopyalama [az depolama blob kopyalama başlangıç](/cli/azure/storage/blob/copy#start) Azure depolama hizmeti şifrelemesi için hiçbir zaman etkinleştirilmiş bir depolama hesabı.
- Yönetilen diskleri kullanan ve ile oluşturma sırasında bu VHD dosyasını belirtmek bir VM oluşturma [az vm oluşturma](/cli/azure/vm#create), veya
- İle kopyalanan VHD'yi [az vm diskini](/cli/azure/vm/disk#attach) yönetilen diskler ile çalışan bir VM için.


## <a name="azure-storage-standard-and-premium"></a>Azure Storage: Standart ve Premium
Azure VM'ler--Yönetilen diskleri kullanarak olup olmadığını veya yönetilmeyen--standart depolama disklerini veya premium depolama diskleri yerleştirilmiş olabilir. Portal, VM seçmek için kullanırken üzerinde bir açılır geçiş gerekir **Temelleri** standart ve premium disklerin görüntülemek için ekranı. SSD, yalnızca etkin VM'ler gösterilir, premium storage yükseğe zaman SSD tarafından desteklenen tüm sürücüleri.  Premium ile birlikte HDD, disk sürücüleri gösterilir, dönen tarafından desteklenen standart depolama etkin VM'ler yükseğe zaman depolama VM'ler tarafından SSD olarak yedeklenir.

Bir sanal makineden oluştururken `azure-cli` aracılığıyla VM boyutu seçerken, standart ve premium arasında seçebilirsiniz `-z` veya `--vm-size` CLI bayrağı.

## <a name="creating-a-vm-with-a-managed-disk"></a>Bir VM ile yönetilen bir Disk oluşturma

Aşağıdaki örnek yapabilecekleriniz Azure CLI 2.0 gerektirir [burada yüklemek](/cli/azure/install-azure-cli).

İlk olarak, kaynaklarla yönetmek için bir kaynak grubu oluşturmak [az grubu oluşturma](/cli/azure/group#create):

```azurecli
az group create --location westus --name myResourceGroup
```

Şimdi ile VM oluşturma [az vm oluşturma](/cli/azure/vm#create). Benzersiz bir belirtin `--public-ip-address-dns-name` bağımsız değişken olarak `mypublicdns` olasılıkla alınır.

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --public-ip-address-dns-name mypublicdns
```

Önceki örnek bir VM ile standart depolama hesabı yönetilen bir disk oluşturur. Premium depolama hesabı kullanmak için add `--storage-sku Premium_LRS` aşağıdaki örneğe benzer bir bağımsız değişken:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --public-ip-address-dns-name mypublicdns \
    --storage-sku Premium_LRS
```

## <a name="standard-storage"></a>Standart depolama
Azure Standard Storage depolama varsayılan türüdür.  Standart depolama, kullanıcı hala devam ederken uygun maliyetli olması.  

## <a name="premium-storage"></a>Premium depolama
Azure Premium depolama g/Ç kullanımı yoğun iş yükleri çalıştıran sanal makineler için yüksek performanslı, düşük gecikmeli disk desteği sunar. Premium depolama kullanan sanal makine (VM) diskler katı hal sürücüleri (SSD) verileri depolar. Uygulamanızın VM diskleri hızına ve bu disklerin performans yararlanacak şekilde Azure Premium Storage'a geçiş yapabilirsiniz.

Premium depolama özellikleri:

* Premium depolama diskleri: Azure Premium Storage DS, DSv2 veya GS serisi Azure VM'ler için eklenebilecek VM diskleri destekler.
* Premium sayfa blobu: Premium depolama Azure sanal makineleri (VM'ler) kalıcı diskleri tutmak için kullanılan Azure sayfa Bloblarını destekler.
* Premium yerel olarak yedekli depolama: Premium Storage hesabı yalnızca yerel olarak yedekli depolama (LRS) çoğaltma seçeneği olarak destekler ve tek bir bölge içinde verileri üç kopyasını tutar.
* [Premium depolama](../windows/premium-storage.md)

## <a name="premium-storage-supported-vms"></a>Premium depolama Vm'leri desteklenen
Premium depolama destekleyen DS serisi, DSv2 serisi, GS serisi ve Fs-serisi Azure sanal makineleri (VM'ler). Premium depolama Vm'leri desteklenen standart ve Premium depolama diskleri kullanabilirsiniz. Ancak Premium Storage uyumlu olmayan VM serisiyle Premium depolama diskleri kullanamazsınız.

Aşağıda, biz doğrulanmış Linux dağıtımları Premium Storage ile verilmiştir.

| Dağıtım | Sürüm | Desteklenen çekirdek |
| --- | --- | --- |
| Ubuntu |12.04 |3.2.0-75.110+ |
| Ubuntu |14.04 |3.13.0-44.73+ |
| Debian |7.x, 8.x |3.16.7-ckt4-1+ |
| SLES |SLES 12 |3.12.36-38.1+ |
| SLES |SLES 11 SP4 |3.0.101-0.63.1+ |
| CoreOS |584.0.0+ |3.18.4+ |
| Centos |6.5, 6.6, 6.7, 7.0, 7.1 |3.10.0-229.1.2.el7+ |
| RHEL |6.8+, 7.2+ | |

## <a name="azure-file-storage"></a>Azure dosya depolama
Azure File storage standart SMB protokolü kullanılarak bulutta dosya paylaşımları sağlar. Azure dosyaları ile Azure dosya sunucularına kullanan kurumsal uygulamalar geçirebilirsiniz. Azure'da çalışan uygulamalar, Linux çalıştıran Azure sanal makinelerden dosya paylaşımları kolayca bağlayabilirsiniz. Ve dosya depolama en son sürümüne sahip da SMB 3.0 destekleyen bir şirket içi uygulamasından bir dosya paylaşımı bağlayabilir.  Dosya paylaşımları SMB paylaşımları olduğundan bunlara standart dosya sistemi API'leriyle erişebilirsiniz.

Dosya depolama kullanılabilirlik, dayanıklılık, ölçeklenebilirlik ve Azure depolama platformuna yerleşik coğrafi artıklık sağlamadığından dosya depolama Blob, tablo ve kuyruk depolama aynı teknolojiye üzerine inşa edilmiştir. Azure Storage ölçeklenebilirlik ve performans hedefleri dosya depolama performans hedefleri ve sınırları hakkında daha fazla ayrıntı için bkz.

* [Azure Dosya depolamayı Linux ile kullanma](../../storage/files/storage-how-to-use-files-linux.md)

## <a name="hot-storage"></a>Sık erişimli depolama
Azure sık erişimli depolama katmanı, sık erişilen verileri depolamak için en iyi duruma getirilmiştir.  Sık erişimli depolama blob depoları için varsayılan depolama türüdür.

## <a name="cool-storage"></a>Seyrek erişimli depolama
Azure seyrek erişimli depolama katmanı, seyrek erişilen ve uzun süreli verileri depolamak için en iyi duruma getirilmiştir. Örnek kullanım durumları seyrek erişimli depolama için yedeklemeleri, medya içeriği, bilimsel verilerin, uyumluluk ve Arşiv verileri içerir. Genel olarak, nadiren erişilir herhangi bir veri seyrek erişimli depolama için mükemmel bir adaydır.

|  | Sık erişimli depolama katmanı | Seyrek erişimli depolama katmanı |
|:--- |:---:|:---:|
| Kullanılabilirlik |%99,9 |%99 |
| Kullanılabilirlik (RA-GRS okumaları) |%99,99 |%99,9 |
| Kullanım ücretleri |Daha yüksek depolama maliyetleri |Daha düşük depolama maliyetleri |
| Daha düşük erişim |Daha yüksek erişim | |
| ve işlem maliyetleri |ve işlem maliyetleri | |

## <a name="redundancy"></a>Yedeklilik
Microsoft Azure depolama hesabınızdaki veriler dayanıklılık ve yüksek kullanılabilirlik, Azure depolama SLA geçici donanım arızalarında bile karşısında toplantı emin olmak için her zaman çoğaltılır.

Bir depolama hesabı oluşturduğunuzda, aşağıdaki çoğaltma seçeneklerinden birini seçmeniz gerekir:

* Yerel olarak yedekli depolama (LRS)
* Bölge olarak yedekli depolama (ZRS)
* Coğrafi olarak yedekli depolama (GRS)
* Okuma erişimli coğrafi olarak yedekli depolama (RA-GRS)

### <a name="locally-redundant-storage"></a>Yerel olarak yedekli depolama
Yerel olarak yedekli depolama (LRS) depolama hesabınız oluşturuldu bölge içindeki verilerinizi çoğaltır. Dayanıklılığı en üst düzeye çıkarmak için depolama hesabınızdaki verilere karşı yapılan her isteği üç kez çoğaltılır. Bu üç çoğaltmaların her ayrı hata etki alanları ve yükseltme etki alanları bulunur.  Yalnızca tüm üç çoğaltmalar için yazıldıktan sonra bir istek başarılı bir şekilde döndürür.

### <a name="zone-redundant-storage"></a>Bölge olarak yedekli depolama
Bölge olarak yedekli depolama (ZRS) verilerinizi LRS daha fazla dayanıklılık sağlayan iki veya üç tesis tek bir bölge içinde veya iki bölgede arasında çoğaltılır. Etkin ZRS depolama hesabınız varsa, verilerinizi tesis birinde hatası durumunda bile dayanıklı.

### <a name="geo-redundant-storage"></a>Coğrafi Olarak Yedekli Depolama
Coğrafi olarak yedekli depolama (GRS) verileriniz birincil bölge çıktığınızda mil yüzlerce olan ikincil bir bölgeye çoğaltır. Etkin GRS depolama hesabınız varsa, verilerinizi bile tam bölgesel bir kesintinin veya bir olağanüstü durumda birincil bölge kurtarılabilir değil söz konusu olduğunda dayanıklı.

### <a name="read-access-geo-redundant-storage"></a>Coğrafi olarak yedekli depolamaya okuma erişimi
Okuma erişimli coğrafi olarak yedekli depolama (RA-GRS) GRS tarafından sağlanan iki bölgede çoğaltma ek olarak ikincil konumdaki verileri salt okunur erişim sağlayarak depolama hesabınız için kullanılabilirliği en üst düzeye çıkarır. Uygulamanızın veri birincil bölgede kullanılamaz hale gelmesi durumunda, verileri ikincil bölge ' okuyabilir.

Azure Depolama artıklık bakın içine derinlemesine bir bakış için:

* [Azure Depolama çoğaltması](../../storage/common/storage-redundancy.md)

## <a name="scalability"></a>Ölçeklenebilirlik
Azure Storage çok büyük boyutta ölçeklenebilir. Bu sayede bilimsel, finansal analiz ve medya uygulamalarının ihtiyaç duyduğu büyük veri senaryolarını desteklemek üzere yüzlerce terabayt veriyi depolayıp işleyebilirsiniz. Veya küçük ölçekli bir işletmenin web sitesi için gerekli olan küçük miktarda veriyi depolayabilirsiniz. İhtiyaçlarınız ne olursa olsun yalnızca depoladığınız veri için ücret ödersiniz. Şu anda Azure Storage onlarca trilyon benzersiz müşteri nesnesi depolamaktadır ve saniyede ortalama olarak milyonlarca talebi ele alır.

Standart depolama hesapları için: bir standart depolama hesabı 20.000 IOPS en büyük toplam istek oranı vardır. Standart bir depolama hesabındaki tüm sanal makine disklerinizde toplam IOPS bu sınırı aşmamalıdır.

Premium depolama hesapları için: 50 GB/sn maksimum toplam üretilen iş oranını premium depolama hesabı vardır. Tüm sanal makinelerdeki toplam aktarım hızı bu sınırı aşmamalıdır.

## <a name="availability"></a>Kullanılabilirlik
Birincil bölgede gerçekleşen veri okuma denemelerinin başarısız olması durumunda yeniden deneme işleminin ikincil bölgede yapılması şartıyla, Okuma Erişimli Coğrafi Olarak Yedekli Depolama (RA-GRS) Hesaplarından veri okuma isteklerinin en az %99,99’unu (Seyrek Erişimli Katman için %99,9) başarıyla işleyeceğimizi garanti ediyoruz.

Yerel Olarak Yedekli Depolama (LRS), Bölgesel Olarak Yedekli Depolama (ZRS) ve Coğrafi Olarak Yedekli Depolama (GRS) Hesaplarından veri okuma isteklerinin en az %99,9’unu (Seyrek Erişimli Katman için %99) başarıyla işleyeceğimizi garanti ediyoruz.

Yerel Olarak Yedekli Depolama (LRS), Bölgesel Olarak Yedekli Depolama (ZRS) ve Coğrafi Olarak Yedekli Depolama (GRS) Hesapları ile Okuma Erişimli Coğrafi Olarak Yedekli Depolama (RA-GRS) Hesaplarına veri yazma isteklerinin en az %99,9’unu (Seyrek Erişimli Katman için %99) başarıyla işleyeceğimizi garanti ediyoruz.

* [Depolama için Azure SLA](https://azure.microsoft.com/support/legal/sla/storage/v1_1/)

## <a name="regions"></a>Bölgeler
Azure dünyanın 30 bölgelerdeki genel olarak kullanılabilir ve planları 4 ek bölgeler için bildirme. Coğrafi genişleme, müşterilerimizin daha iyi performans elde etmelerini sağladığından ve veri konumu açısından müşterilerin gereksinimleriyle tercihlerini desteklediğinden Azure için bir önceliktir.  Başlatmak için azures son içinde Almanya bölgedir.

Microsoft Cloud Almanya, yenilik artan fırsatları ve yüksek oranda düzenlenen iş ortakları için ekonomik büyüme ve müşteriler Almanya içinde oluşturma Avrupa arasında zaten mevcut Microsoft Cloud services için farklı bir seçenek sağlar Avrupa Birliği (AB) ve Avrupa Serbest Ticaret Birliği (EFTA).

Bu yeni veri merkezlerinde, Magdeburg ve Frankfurt, müşteri verileri bir veri güvenliği, T-sistemleri uluslararası, bir bağımsız Almanca şirket ve Alman Telekom temsilcisine denetimi altında yönetilir. Bu veri merkezlerinde Microsoft'un ticari bulut hizmetlerini düzenlemelerini işleme Almanca veri uyması ve müşteriler nasıl ve nerede veriler işlenir, ek seçenekler verin.

* [Harita Azure bölgeleri](https://azure.microsoft.com/regions/)

## <a name="security"></a>Güvenlik
Azure depolama birlikte geliştiricilerin güvenli uygulamalar oluşturmalarını sağlayan güvenlik özellikleri kapsamlı bir kümesini sağlar. Depolama hesabı rol tabanlı erişim denetimi ve Azure Active Directory kullanılarak güvenli hale getirilebilir. Verileri bir uygulama ile Azure arasında aktarımda istemci tarafı şifreleme, HTTPS veya SMB 3.0 kullanarak güvenli hale getirilebilir. Verileri Azure depolama birimine yazılırken otomatik olarak şifrelenmesini depolama hizmeti şifreleme (SSE) kullanılarak ayarlanabilir. Sanal makineler tarafından kullanılan işletim sistemi ve veri diskleri Azure Disk şifrelemesi kullanılarak şifrelenmesi için ayarlayabilirsiniz. Azure storage'da veri nesneleri yetkilendirilmiş erişim paylaşılan erişim imzaları kullanılarak verilebilir.

### <a name="management-plane-security"></a>Yönetim düzlemi güvenliği
Yönetim düzeyi, depolama hesabınızı yönetmek için kullanılan kaynakları oluşur. Bu bölümde, Azure Resource Manager dağıtım modeli ve rol tabanlı erişim denetimi (RBAC), depolama hesaplarınıza erişimi denetlemek için nasıl kullanılacağı hakkında size konuşun. Biz de depolama hesabı anahtarlarını ve bunları yeniden nasıl yönetme hakkında konuşur.

### <a name="data-plane-security"></a>Veri düzlemi güvenliği
Bu bölümde, biz gerçek veri nesnelerine erişimi depolama hesabınızdaki BLOB'lar, dosyalar, kuyruklar ve tablolar gibi izin vermeyi paylaşılan erişim imzaları ve depolanan erişim ilkeleri kullanarak göreceğiz. Hizmet düzeyi SAS ve hesap düzeyinde SAS ele alınacaktır. Ayrıca belirli bir IP adresi (veya IP adresi aralığı) erişimi sınırlamak nasıl, HTTPS için kullanılan protokol sınırlamak nasıl ve paylaşılan erişim imzası sona tamamlanmasını beklemeden iptal etme göreceğiz.

## <a name="encryption-in-transit"></a>Aktarımdaki şifreleme
Bu bölümde içine veya dışına Azure Storage aktardığınızda verilerin güvenliğini sağlamak nasıl açıklanmaktadır. HTTPS ve Azure dosya paylaşımları için SMB 3.0 tarafından kullanılan şifreleme önerilen kullanımıyla ilgili konuşun. Biz de ve bu sayede istemci uygulamasında depolama alanına aktarılır önce verilerin şifrelenmesi ve depolama alanı biterse aktarıldıktan sonra verilerin şifresini çözmek için istemci tarafı şifreleme göz atın.

## <a name="encryption-at-rest"></a>Bekleyen şifreleme
Depolama hizmeti şifreleme (SSE) ve nasıl, blok blobları, sayfa bloblarını kaynaklanan bir depolama hesabı için etkinleştirmek ve ilave blobları için Azure Storage yazılırken otomatik olarak Şifrelenmekte hakkında konuşur. Ayrıca Azure Disk şifrelemesi kullanın ve temel farklar ve istemci tarafı şifreleme karşı SSE karşı Disk şifrelemesi örneklerini keşfedin nasıl ele alacağız. Kısaca ABD için FIPS uyumluluk ele alacağız Kamu bilgisayarlar.

* [Azure depolama Güvenlik Kılavuzu](../../storage/common/storage-security-guide.md)

## <a name="temporary-disk"></a>Geçici disk
Her VM geçici disk içerir. Geçici disk uygulamalar ve işlemler için kısa vadeli depolama sağlar ve yalnızca sayfa veya takas dosyaları gibi verileri depolamak için kullanılması amaçlanmıştır. Geçici disk üzerindeki verileri kaybolabilir sırasında bir [bakım olayı](manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#understand-vm-reboots---maintenance-vs-downtime) veya ne zaman, [VM yeniden](redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Sırasında standart yeniden başlatma VM geçici sürücüdeki verilerin kalıcı olması.

Linux sanal makineleri, genellikle disktir **/dev/sdb** ve biçimlendirilmiş ve bağlanan **/mnt** Azure Linux aracısı tarafından. Geçici diskin boyutunu, sanal makine boyutuna göre değişir. Daha fazla bilgi için bkz: [Linux sanal makineler için Boyutlar](sizes.md).

Azure geçici disk nasıl kullandığı hakkında daha fazla bilgi için bkz: [geçici sürücü Microsoft Azure sanal makineler üzerinde anlama](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)

## <a name="cost-savings"></a>Maliyet tasarrufları
* [Depolama maliyeti](https://azure.microsoft.com/pricing/details/storage/)
* [Depolama maliyeti hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/?service=storage)

## <a name="storage-limits"></a>Depolama sınırları
* [Depolama hizmet sınırları](../../azure-subscription-service-limits.md#storage-limits)
