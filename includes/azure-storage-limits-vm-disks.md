Bir Azure sanal makinesi birkaç veri diskini eklemeyi destekler. Bu makalede, bir sanal makinenin veri diskleri için ölçeklenebilirlik ve performans hedefleri açıklanır. Bu hedefler, performans ve kapasite gereksinimlerinizi karşılamak için gereken disk türü ve numarası karar vermenize yardımcı olması için kullanın. 

> [!IMPORTANT]
> En iyi performans için olası azalmayı önlemek için sanal makineye bağlı, yüksek oranda kullanılan disk sayısını sınırlayın. Ardından sanal makine aynı anda tüm ekli disklerin yüksek oranda kullanılmaz, çok sayıda disk destekleyebilir.

* **İçin Azure yönetilen diskler:** 

> | Kaynak | Varsayılan Sınır | Üst Sınır |
> | --- | --- | --- |
> | Standart Yönetilen Diskler | 10,000 | 50,000 |
> | Standart SSD Yönetilen Diskler | 10,000 | 50,000 |
> | Premium Yönetilen Diskler | 10,000 | 50,000 |
> | Standard_LRS anlık görüntüleri | 10,000 | 50,000 |
> | Standard_ZRS anlık görüntüleri | 10,000 | 50,000 |
> | Yönetilen bir görüntü | 10,000 | 50,000 |

* **Standart depolama hesapları için:** Standart bir depolama hesabı en fazla toplam 20.000 IOPS istek oranına sahiptir. Standart bir depolama hesabındaki tüm sanal makine disklerinizde toplam IOPS bu sınırı aşmamalıdır.
  
    Tek bir standart depolama hesabı tarafından desteklenen yüksek kullanımlı disk sayısını, istek oranı sınırına göre kabaca hesaplayabilirsiniz. Örneğin, bir temel katman sanal makine sayısının yüksek kullanımlı disk 66 (disk başına 20.000/300 IOPS) ve standart katman sanal makine için yaklaşık 40'tır (disk başına 20.000/500 IOPS) olur. 

* **Premium depolama hesapları için:** Premium depolama hesabında en fazla aktarım hızı 50 Gbps’dir. Tüm sanal makinelerdeki toplam aktarım hızı bu sınırı aşmamalıdır.

