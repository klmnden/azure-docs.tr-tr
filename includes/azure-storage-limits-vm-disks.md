Bir Azure sanal makinesi birkaç veri diskini eklemeyi destekler. En iyi performans için, olası azalmayı önlemek için sanal makineye bağlı olup yüksek oranda kullanılan disk sayısını azaltmanız gerekebilir. Tüm diskler aynı anda yüksek oranda kullanılmıyorsa, depolama hesabı daha fazla sayıda diski destekler.

* **Azure Yönetilen Diskler için:** Yönetilen Disk sayısı sınırı, abonelik için bölgeseldir. Varsayılan geçici sınır 2.000'dir. Sınırınızı artırmak için Azure desteğine başvurun.

    Yönetilen Anlık Görüntüler ve Görüntüler, Yönetilen Diskler sınırına göre sayılır.

* **Standart depolama hesapları için:** Standart bir depolama hesabı en fazla toplam 20.000 IOPS istek oranına sahiptir. Standart bir depolama hesabındaki tüm sanal makine disklerinizde toplam IOPS bu sınırı aşmamalıdır.
  
    Tek bir standart depolama hesabı tarafından desteklenen yüksek kullanımlı disk sayısını, istek oranı sınırına göre kabaca hesaplayabilirsiniz. Örneğin, aşağıdaki tabloda gösterildiği gibi, bir Temel Katman sanal makine için yüksek oranda kullanılan en fazla disk 66 (disk başına 20.000/300 IOPS) iken, Standart Katman sanal makine için bu sayı yaklaşık 40’tır (disk başına 20.000/500 IOPS). 
* **Premium depolama hesapları için:** Premium depolama hesabında en fazla aktarım hızı 50 Gbps’dir. Tüm sanal makinelerdeki toplam aktarım hızı bu sınırı aşmamalıdır.



<!--HONumber=Feb17_HO2-->


