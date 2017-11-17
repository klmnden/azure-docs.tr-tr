Bir Azure sanal makinesi birkaç veri diskini eklemeyi destekler. Bu makalede, bir sanal makinenin veri diski için ölçeklenebilirlik ve performans hedefleri açıklanmaktadır. Bu hedeflerde sayısı ve performans ve kapasite gereksinimlerinizi karşılamak için gereken disk türüne karar vermenize yardımcı olması için kullanın. 

> [!IMPORTANT]
> En iyi performans için mümkün azaltma önlemek için sanal makineye bağlı yüksek oranda kullanılan disk sayısını sınırlayın. Eklenen tüm diskler aynı anda yüksek oranda kullanılmaz, sanal makine disklerin daha büyük bir sayı destekleyebilir.

* **Azure yönetilen disklerin:** disk yönetilen diskler için disk türünü ve bölge başına sınırlıdır. Üst sınırı, aynı zamanda varsayılan sınır ise bir abonelik için disk türünü ve bölge başına 10.000 yönetilen diskleri. Örneğin, yönetilen en fazla 10.000 standart oluşturabilirsiniz diskleri ve ayrıca 10.000 premium yönetilen abonelik başına bir bölgede diskler.

    Yönetilen anlık görüntüler ve görüntüleri yönetilen diskleri sınırınızı etkiler.

* **Standart depolama hesapları için:** Standart bir depolama hesabı en fazla toplam 20.000 IOPS istek oranına sahiptir. Standart bir depolama hesabındaki tüm sanal makine disklerinizde toplam IOPS bu sınırı aşmamalıdır.
  
    Tek bir standart depolama hesabı tarafından desteklenen yüksek kullanımlı disk sayısını, istek oranı sınırına göre kabaca hesaplayabilirsiniz. Örneğin, temel katmanı VM, yüksek oranda kullanılan disk sayısı ve standart katmanı VM 66 (disk başına 20.000/300 IOPS) hakkında için yaklaşık 40 (disk başına 20.000/500 IOPS) olur. 

* **Premium depolama hesapları için:** Premium depolama hesabında en fazla aktarım hızı 50 Gbps’dir. Tüm sanal makinelerdeki toplam aktarım hızı bu sınırı aşmamalıdır.

