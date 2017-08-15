
* Dönüştürme işlemi VM’nin yeniden başlatılmasını gerektirir, bu nedenle VM'lerinizin geçişini önceden var olan bir bakım penceresi sırasında zamanlayın. 

* Bu dönüştürme geri alınamaz. 

* Dönüştürmeyi test ettiğinizden emin olun. Üretimde geçişi gerçekleştirmeden önce bir sınama sanal makinesini geçirin.

* Dönüştürme sırasında VM’yi serbest bırakın. VM, dönüştürmeden sonra başlatıldığında yeni bir IP adresi alır. Gerekirse VM’ye [statik bir IP adresi atayabilirsiniz](../articles/virtual-network/virtual-network-ip-addresses-overview-arm.md).

* Özgün VHD’ler ve dönüştürme öncesinde VM tarafından kullanılan depolama hesabı silinmez. Ücretler uygulanmaya devam eder. Bunlar için ücret alınmasını önlemek istiyorsanız, dönüştürmenin tamamlandığını doğruladıktan sonra özgün VHD bloblarını silin.
