---
title: Avere vFXT kümesi - Azure'ı yönetme
description: Avere kümesini yönetme - ekleme veya düğümleri kaldırma, yeniden başlatma, durdurma veya vFXT kümeyi yok edin
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 01/29/2019
ms.author: v-erkell
ms.openlocfilehash: be9205fdf7fec0661d7382ed0d1bedf47487b15e
ms.sourcegitcommit: b4ad15a9ffcfd07351836ffedf9692a3b5d0ac86
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2019
ms.locfileid: "59058059"
---
# <a name="manage-the-avere-vfxt-cluster"></a>Avere vFXT kümesini yönetme

Küme oluşturulduktan sonra küme düğümleri Ekle veya durdurmak kümeye yeniden başlatmanız gerekebilir. Ve projeniz tamamlandığında durdurma ve kümeyi kalıcı olarak kaldırma bilmeniz gerekir. 

Küme yönetimi göreve bağlı olarak, yapmak için Avere Denetim Masası'nı, vfxt.py komut satırı küme oluşturma komut veya Azure Portalı'nı kullanmanız gerekebilir. 

Bu tablo, her görev için hangi Araçlar kullanılabilir genel bir bakış sağlar. 

| Eylem | Avere Denetim Masası | vfxt.PY  | Azure portal |
| --- | --- | --- | --- |
| Küme Düğümleri Ekle | hayır | evet | hayır |
| Küme düğümleri kaldırma | evet | hayır | hayır |
| Bir küme düğümü durdurma | Evet (Ayrıca hizmetlerini yeniden başlatın veya yeniden başlatma) | hayır | portaldan bir düğüm VM aşağı destekleyen bir düğümde hata oluştuktan yorumlanır |
| Durdurulmuş bir düğümü başlatın | hayır | hayır | evet |
| Tek bir düğüm yok | hayır | hayır | evet |
| Küme yeniden başlatma |  |  |  |
| Kapatın veya kümeye güvenli bir şekilde Durdur | evet | evet | hayır |
| Kümeyi yok edin  | hayır | evet | Evet, ancak veri bütünlüğü garanti edilmez |

Her aracı için ayrıntılı yönergeler aşağıda verilmiştir.

## <a name="about-stopped-instances-in-azure"></a>Azure'da durdurulmuş örnekleri hakkında

Kapatma ya da herhangi bir Azure sanal Makinesini durdurmak işlem ücretlerinin yansıtılmasını durdurur ancak yine de depolama için ödeme yapmanız gerekir. Tüm vFXT küme vFXT düğüm artırıp kapatın ve yeniden düşünmüyorsanız, ilgili sanal makinelerini silmek için Azure portalını kullanmanız gerekir. 

Azure portalında bir *durduruldu* (başlatılabilir) düğüm durumu gösterilir **durduruldu** Azure portalındaki bir *silindi* düğümünü gösteren durum **durduruldu (serbest bırakıldı)**  ve onu artık işlem ve depolama ücreti alınmaz.

Sanal Makineyi silmeden önce tüm değiştirilen veri yazıldı, önbellekten arka uç depolama için kümeyi kapatın veya durdurmak için Denetim Masası'nı Avere veya vfxt.py seçeneklerini kullanarak emin olun.

## <a name="manage-the-cluster-with-avere-control-panel"></a>Avere Denetim Masası ile küme yönetme 

Bu görevler için Avere Denetim Masası'nı kullanılabilir: 

* Durdurmak veya tek tek düğümleri yeniden başlatın
* Kümeden düğüm kaldırma
* Durdurma veya tüm küme yeniden başlatma

Olası zararlı önce arka uç depolama herhangi değiştirilen verileri yazmak amacıyla çalışır böylece Avere Denetim Masası'ndaki veri bütünlüğü önceliklendirir. Bu Avere portalı daha güvenli bir seçenek sağlar. 

Erişim Avere Denetim Masası'ndan bir web tarayıcısı. Bölümündeki yönergeleri [vFXT kümeye erişmek](avere-vfxt-cluster-gui.md) yardıma ihtiyacınız varsa.

### <a name="manage-nodes-with-avere-control-panel"></a>Düğümleri Avere Denetim Masası ile yönetme

**FXT düğümleri** Ayarları sayfası, tek tek düğümlere yönetmeye yönelik denetimler içerir.

Kapatma, yeniden başlatma veya bir düğümü kaldırmak için üzerinde listesinde düğümü bulun **FXT düğümleri** sayfasında ve uygun düğmesine tıklayın, **eylemleri** sütun.

> [!NOTE] 
> Etkin düğüm sayısını değiştiğinde, IP adresleri küme düğümleri arasında taşıyabilirsiniz.

Okuma [küme > düğümler FXT](<https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_fxt_nodes.html#gui-fxt-nodes>) Avere küme ayarları Kılavuzu daha fazla bilgi için.

### <a name="stop-or-reboot-the-cluster-with-avere-control-panel"></a>Durdurma veya küme Avere Denetim Masası ile yeniden başlatma

**Sistem Bakımı** Ayarları sayfası, Küme Hizmetleri yeniden başlatma, kümeyi yeniden başlatma veya güvenli bir şekilde aşağı küme destekleyen komutları içerir. Okuma [Yönetim > Sistem Bakımı](<https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_system_maintenance.html#gui-system-maintenance>) (Avere içinde küme ayarları Kılavuzu) Ayrıntılar için.

Bir küme kapatılıyor, durum iletileri gönderir. **Pano** ilk sekmesi. Birkaç dakika sonra Avere Denetim Masası'ndaki oturumunu yanıt vermeyi anlamına kümeye kapatıldı durdurur.

## <a name="manage-the-cluster-with-vfxtpy"></a>Vfxt.py ile küme yönetme

vfxt.PY bir küme oluşturma ve yönetme için komut satırı aracıdır. 

vfxt.PY VM kümesi denetleyicisinde önceden yüklenir. Başka bir sistem üzerinde yüklemek istiyorsanız, adresindeki belgelere başvurun <https://github.com/Azure/AvereSDK>.

Vfxt.py betiği bu küme yönetim görevleri için kullanılabilir:

* Bir kümeye yeni düğümler ekleme
* Bir küme başlatma veya durdurma  
* Bir kümeyi yok edin

Avere Denetim Masası'nda gibi değiştirilen verileri kalıcı olarak arka uç depolama kümesi veya düğüm yok etme veya kapatmadan önce depolanan emin olmak vfxt.py işlemleri deneyin. Bu Avere portalı daha güvenli bir seçenek sağlar.

Tam vfxt.py Kullanım Kılavuzu, GitHub üzerinde kullanılabilir: [Bulut vfxt.py ile küme yönetimi](https://github.com/azure/averesdk/blob/master/docs/README.md)

### <a name="add-cluster-nodes-with-vfxtpy"></a>Vfxt.py ile küme düğümleri Ekle

Küme düğümleri eklemek için bir örnek komut küme denetleyicisinde bulunur. Bulun ``./add-nodes`` denetleyicisinde ve küme bilgilerinizle özelleştirmek için bir düzenleyicide açın. 

Bu komutu kullanmak için küme çalıştırılması gerekir. 

Aşağıdaki değerleri sağlayın: 

* Küme için ve aynı küme yoksa Ağ ve depolama kaynakları için kaynak grubu adı
* Küme konumu
* Küme ağı ve alt ağ 
* Küme düğümü erişim rolünü (yerleşik rolü kullanmak [Avere işleci](../role-based-access-control/built-in-roles.md#avere-operator))
* Küme yönetim IP adresi ve yönetici parolası 
* (1, 2 veya 3) eklemek için düğüm sayısı
* Düğüm örnek türü ve önbellek boyutu değerleri 

Prototip kullanmıyorsanız, yukarıda açıklanan bilgiler dahil olmak üzere tüm aşağıdaki gibi bir komut oluşturmak gerekir. 

```bash
   vfxt.py --cloud-type azure --from-environment \
   --resource-group GROUP_NAME \ 
   [--network-resource-group GROUP_NAME --storage-resource-group GROUP_NAME]  \
   --location LOCATION --azure-network NETWORK_NAME --azure-subnet SUBNET_NAME \
   --add-nodes --nodes NODE_COUNT \
   --management-address CLUSTER_IP --admin-password ADMIN_PASSWORD \
   --instance-type TYPE --node-cache-size SIZE \
   --azure-role "Avere Operator" \
   --log ~/vfxt.log
```

Daha fazla bilgi için okuma [bir kümeye düğüm ekleme](https://github.com/Azure/AvereSDK/blob/master/docs/using_vfxt_py.md#add-nodes-to-a-cluster) vfxt.py Kullanım Kılavuzu'nda.

### <a name="stop-a-cluster-with-vfxtpy"></a>Bir küme ile vfxt.py Durdur

```bash
vfxt.py --cloud-type azure --from-environment --stop --resource-group GROUPNAME --admin-password PASSWORD --management-address ADMIN_IP --location LOCATION --azure-network NETWORK --azure-subnet SUBNET
```

### <a name="start-a-stopped-cluster-with-vfxtpy"></a>Durdurulmuş bir küme ile vfxt.py Başlat

```bash
vfxt.py --cloud-type azure --from-environment --start --resource-group GROUPNAME --admin-password PASSWORD --management-address ADMIN_IP --location LOCATION --azure-network NETWORK --azure-subnet SUBNET --instances INSTANCE1_ID INSTANCE2_ID INSTANCE3_ID ...
```    

Küme durdurulduğundan, küme düğümleri belirtmek için örnek tanımlayıcılarını geçmesi gerekir. Okuma [değiştirmek için hangi küme belirtme](https://github.com/Azure/AvereSDK/blob/master/docs/using_vfxt_py.md#specifying-which-cluster-to-modify) daha fazla bilgi için vfxt.py Kullanım Kılavuzu'nda.

### <a name="destroy-a-cluster-with-vfxtpy"></a>Vfxt.py ile bir kümeyi yok edin

```bash
vfxt.py --cloud-type azure --from-environment --destroy --resource-group GROUPNAME --admin-password PASSWORD --management-address ADMIN_IP --location LOCATION --azure-network NETWORK --azure-subnet SUBNET --management-address ADMIN_IP
```

Seçenek ``--quick-destroy`` küme önbellekten değiştirilen verileri yazmak istemiyorsanız kullanılabilir.

Okuma [vfxt.py Kullanım Kılavuzu](<https://github.com/Azure/AvereSDK/blob/master/docs/README.md>) ek bilgi için.  

## <a name="manage-cluster-vms-from-the-azure-portal"></a>Azure portalından küme Vm'leri yönetme 

Azure portalı küme sanal makineleri ayrı ayrı yok etmek için kullanılabilir, ancak küme indrebilirsiniz ilk makineyi değil, veri bütünlüğünü garanti edilmez. 

Azure portalı, bu küme yönetim görevleri için kullanılabilir: 

* Durdurulan vFXT düğümü başlatın
* (Küme bunu, bir düğüm hatası olarak'yorumlar) bir bireysel vFXT düğümü durdurma
* VFXT kümeyi yok edin *varsa* küme önbelleğinde değiştirilen verileri için çekirdek dosyalayıcı yazıldığından emin olmanız gerekmez
* Bunlar güvenli bir şekilde kapatıldığından sonra vFXT düğümlerini ve diğer küme kaynaklarını kalıcı olarak kaldırın

### <a name="restart-vfxt-instances-from-the-azure-portal"></a>Azure portalından vFXT örneklerini yeniden başlatın

Durdurulmuş bir düğümü yeniden başlatmanız gerekirse, Azure portalını kullanmanız gerekir. Seçin **sanal makineler** sol menüsünü ve ardından, genel bakış sayfasını açmak için listedeki VM adı.

Tıklayın **Başlat** VM etkinleştirmeye genel bakış sayfasının üstündeki düğmesi.

![Bir VM'yi durdurduğunuzda seçeneğini gösteren azure portal ekran](media/avere-vfxt-start-stopped-incurring-annot.png)

### <a name="delete-cluster-nodes"></a>Küme düğümleri Sil

VFXT kümeden bir düğümü silmek, ancak kalan küme tutmak istiyorsanız, önce gereken [düğümü kümeden kaldırma](#manage-nodes-with-avere-control-panel) Avere Denetim Masası ile.

> [!CAUTION]
> İlk vFXT kümeden kaldırmadan bir düğümü silmek, veriler kaybolabilir.

VFXT düğümü olarak kullanılan bir veya daha fazla örneğini kalıcı olarak yok etmek için Azure portalını kullanın.
Seçin **sanal makineler** sol menüsünü ve ardından, genel bakış sayfasını açmak için listedeki VM adı.

Tıklayın **Sil** kalıcı olarak VM yok edilecek genel bakış sayfasının üstündeki düğmesi.

Bunlar güvenli bir şekilde kapatıldığından sonra küme düğümlerinin kalıcı olarak kaldırmak için bu yöntemi kullanabilirsiniz. 

### <a name="destroy-the-cluster-from-the-azure-portal"></a>Azure portalından kümeyi yok edin

> [!NOTE] 
> Arka uç depolama alanına yazılacak önbellekte kalan istemci değişiklikleri istiyorsanız vfxt.py kullanın `--destroy` seçeneğini veya düğüm örnekleri Azure Portalı'nda kaldırmadan önce kümeyi düzgün bir şekilde kapatmaya Avere Denetim Masası'nı kullanın.

Azure portalında silerek düğümü örneği kalıcı olarak yok edebilir. Bunları teker teker yukarıda açıklandığı ya da kullanabilir silebilirsiniz **sanal makineler** tüm küme Vm'leri bulmak için bunları ile onay kutularını seçin ve tıklayın sayfa **Sil** bunları kaldırmak için düğmeyi tümü bir eylem.

![VM'lerin listesini portalında, "gözden geçirdi ve vurgulanmış küme", üç, dört Terime göre filtrelenmiş](media/avere-vfxt-multi-vm-delete.png)

### <a name="delete-additional-cluster-resources-from-the-azure-portal"></a>Azure portalından ek küme kaynaklarını silin

Özellikle vFXT küme için ek kaynaklar oluşturduysanız, bunları kümeyi bozmadan bir parçası olarak kaldırmak isteyebilirsiniz. İhtiyacınız olan verileri veya diğer projeleriyle paylaştığı öğeleri içeren öğeler yok.

Küme düğümleri silmeye ek olarak, bu bileşenlerin kaldırılması göz önünde bulundurun: 

* Küme Denetleyicisi VM
* Küme düğümleri ile ilişkili veri diskleri
* Ağ arabirimleri ve küme bileşenleri ile ilişkili ortak IP'ler
* Sanal ağlar
* Depolama hesapları (**yalnızca** önemli hiçbir veri içeren)
* Kullanılabilirlik kümesi 

![Azure portalında kaynakları gösteren "tüm kaynaklar" listesini bir test kümesi için oluşturuldu.](media/avere-vfxt-all-resources-list.png)

### <a name="delete-a-clusters-resource-group-from-the-azure-portal"></a>Azure Portal'dan bir küme kaynak grubunu silme

Özellikle küme barındırmak için bir kaynak grubu oluşturduysanız, küme için tüm ilgili kaynakları kaynak grubu yok edilirken yok edebilir. 

> [!Caution] 
> Hiçbir değer grubunda yer alıyor olması durumunda, yalnızca kaynak grubu yok. Örneğin, kaynak grubu içinde herhangi bir depolama kapsayıcılardan gerekli tüm verileri taşıdık emin olun.  

Bir kaynak grubu silmek için tıklayın **kaynak grupları** portalı ve filtre sol taraftaki menüde vFXT küme için oluşturulan bir bulmak için kaynak gruplarının listesi. Kaynak grubunu seçin ve panelin sağ tarafındaki üç noktaya tıklayın. **Kaynak grubunu sil**'i seçin. Portal, geri alınamaz silmeyi onaylamak için sorar.  

!["Kaynak grubunu sil" eylemini gösteren bir kaynak grubu](media/avere-vfxt-delete-resource-group.png)
