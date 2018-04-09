
* Dönüştürme işlemi VM’nin yeniden başlatılmasını gerektirir, bu nedenle VM'lerinizin geçişini önceden var olan bir bakım penceresi sırasında zamanlayın. 

* Bu dönüştürme geri alınamaz. 

* Herhangi bir unutmayın kullanıcılarla [sanal makine Katılımcısı](../articles/active-directory/role-based-access-built-in-roles.md#virtual-machine-contributor) rol (ön dönüştürme verebilir gibi) VM boyutunu değiştirmek mümkün olmayacak. Yönetilen disklerle VM'ler işletim sistemi disklerinde Microsoft.Compute/disks/write izninin kullanıcının gerektiren olmasıdır.

* Dönüştürmeyi test ettiğinizden emin olun. Üretimde geçişi gerçekleştirmeden önce bir sınama sanal makinesini geçirin.

* Dönüştürme sırasında VM’yi serbest bırakın. VM, dönüştürmeden sonra başlatıldığında yeni bir IP adresi alır. Gerekirse VM’ye [statik bir IP adresi atayabilirsiniz](../articles/virtual-network/virtual-network-ip-addresses-overview-arm.md).

* Özgün VHD’ler ve dönüştürme öncesinde VM tarafından kullanılan depolama hesabı silinmez. Ücretler uygulanmaya devam eder. Bunlar için ücret alınmasını önlemek istiyorsanız, dönüştürmenin tamamlandığını doğruladıktan sonra özgün VHD bloblarını silin.

* Azure VM Aracısı dönüştürme işlemini desteklemek için gerekli en düşük sürümünü gözden geçirin. Denetleyin ve aracı sürümü güncelleştirme hakkında daha fazla bilgi için bkz: [Azure VM aracıları için en düşük sürüm desteği](https://support.microsoft.com/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support)