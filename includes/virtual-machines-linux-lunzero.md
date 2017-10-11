Bir diski LUN 0 mevcut değilse veri disklerinin bir Linux VM eklerken, hatalarla karşılaşabilir. Kullanarak el ile bir disk ekliyorsanız `azure vm disk attach-new` komutu ve belirtirseniz bir LUN (`--lun`) uygun LUN belirlemek Azure platformu izin vermek yerine dikkatli bir disk zaten var. / LUN 0 yer alır. 

Çıktısını parçacığıyla gösteren aşağıdaki örneği göz önünde bulundurun `lsscsi`:

```bash
[5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc 
[5:0:0:1]    disk    Msft     Virtual Disk     1.0   /dev/sdd 
```

İki veri diski LUN 0 ve LUN 1 mevcut (ilk sütunda `lsscsi` çıkış ayrıntıları `[host:channel:target:lun]`). Her iki diskin VM içinden gelen accessbile olmalıdır. LUN 1 eklenecek ilk disk ve LUN 2 konumundaki ikinci diski el ile belirtilen, diskleri doğru VM içinden göremeyebilirsiniz.

> [!NOTE]
> Azure `host` değer 5 Bu örneklerde, ancak bu seçtiğiniz depolama türüne bağlı olarak değişebilir.
> 
> 

Bu disk davranış Azure sorun ancak Linux çekirdek SCSI belirtimleri izleyen yolu değil. Linux çekirdek SCSI veri yoluna bağlı aygıtları için tarama yaptığında ek cihazlar için taramaya devam etmek için LUN 0 değerine sırada sistem için bir aygıt bulunması gerekir. Bu nedenle:

* Çıkışı gözden `lsscsi` LUN 0 bir diske sahip olduğunuzu doğrulamak için bir veri diski ekledikten sonra.
* Diskinizin doğru VM içinden gösterilmez, bir diski LUN 0 mevcut doğrulayın.

