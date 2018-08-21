- Sanal ağ, Batch hesabıyla aynı Azure **bölgesinde** ve **aboneliğinde** olmalıdır.

- Sanal makine yapılandırması ile oluşturulan havuzlar için yalnızca Azure Resource Manager tabanlı sanal ağlar desteklenir. Bulut hizmetleri yapılandırmasıyla oluşturulan havuzlar için yalnızca klasik sanal ağlar desteklenir.
  
- Klasik sanal ağ kullanmak için `MicrosoftAzureBatch` hizmet sorumlusu, belirtilen sanal ağ için `Classic Virtual Machine Contributor` Rol Tabanlı Erişim Denetimi (RBAC) rolüne sahip olmalıdır. Azure Resource Manager tabanlı bir sanal ağ kullanmak için, sanal ağa erişme ve alt ağda VM dağıtma izinlerinizin olması gerekir.

- Havuz için belirtilen alt ağda havuz için hedeflenen VM sayısına yetecek kadar atanmamış IP adresi bulunması gerekir. Başka bir deyişle bu değerin havuzun `targetDedicatedNodes` ve `targetLowPriorityNodes` özelliklerinin toplamı olması gerekir. Alt ağda yeterli sayıda atanmamış IP adresi yoksa havuz işlem düğümlerini kısmen ayırır ve bir yeniden boyutlandırma hatası oluşur. 

- Bir Azure Sanal Ağına dağıtılmış sanal makine yapılandırmasındaki havuzlar, ek Azure ağ kaynaklarını otomatik olarak ayırır. Bir sanal ağdaki her 50 havuz düğümü için şu kaynaklar gereklidir: 1 ağ güvenlik grubu, 1 genel IP adresi ve 1 yük dengeleyici. Bu kaynaklar, Batch havuzu oluşturulurken elde edilen sanal ağı içeren abonelikteki [kotalarla](../articles/batch/batch-quota-limit.md) sınırlıdır.

- İşlem düğümlerinde görev zamanlanabilmesi için alt ağın Batch hizmetinden gelen iletişimlere izin vermesi gerekir. Bu durum sanal ağ ile ilişkilendirilmiş ağ güvenlik grubu (NSG) olup olmadığının kontrol edilmesiyle doğrulanabilir. Belirtilen alt ağdaki işlem düğümleriyle iletişim kurulması bir NSG tarafından reddedilirse Batch hizmeti, işlem düğümlerinin durumunu **kullanılamıyor** olarak ayarlar. 

- Belirtilen sanal ağ ile ilişkilendirilmiş Ağ Güvenlik Grupları (NSG) ve/veya güvenlik duvarı varsa gelen ve giden bağlantı noktalarını aşağıdaki tablolarda gösterilen şekilde yapılandırın:


  |    Hedef Bağlantı Noktaları    |    Kaynak IP adresi      |   Kaynak bağlantı noktası    |    Batch, NSG ekliyor mu?    |    VM'nin kullanılabilmesi için gerekli mi?    |    Kullanıcı eylemi   |
  |---------------------------|---------------------------|----------------------------|----------------------------|-------------------------------------|-----------------------|
  |   <ul><li>Sanal makine yapılandırmasıyla oluşturulan havuzlar için: 29876, 29877</li><li>Bulut hizmetleri yapılandırmasıyla oluşturulan havuzlar için: 10100, 20100, 30100</li></ul>        |    * <br /><br />Batch hizmeti, etkin bir şekilde “tümüne izin ver” seçeneğini gerektirmesine rağmen tüm Batch hizmeti dışındaki IP adreslerini filtreleyerek dışlayan sanal makine yapılandırması altında oluşturulmuş her bir VM’ye ağ arabirimi düzeyinde NSG uygular. | * veya 443 |    Evet. Batch, VM'lere eklenmiş olan ağ arabirimleri (NIC) düzeyinde NSG'ler ekler. Bu NSG'ler yalnızca Batch hizmeti rolü IP adreslerinden gelen trafiğe izin verir. Bu bağlantı noktalarını İnternet’e açsanız dahi trafik NIC üzerinde engellenir. |    Yes  |  Batch yalnızca Batch IP adreslerine izin verdiğinden NSG belirtmeniz gerekmez. <br /><br /> Ancak bir NSG belirtirseniz bu bağlantı noktalarının gelen trafiğe açık olduğundan emin olun.|
  |    3389 (Windows), 22 (Linux)               |    VM'e uzaktan erişebilmeniz için hata ayıklama amacıyla kullanılan kullanıcı bilgisayarları.    |   *  | Hayır                                    |    Hayır                    |    VM için uzaktan erişime (RDP veya SSH) izin vermek istiyorsanız NSG ekleyin.   |                                


  |    Giden Bağlantı Noktaları    |    Hedef    |    Batch, NSG ekliyor mu?    |    VM'nin kullanılabilmesi için gerekli mi?    |    Kullanıcı eylemi    |
  |------------------------|-------------------|----------------------------|-------------------------------------|------------------------|
  |    443    |    Azure Storage    |    Hayır    |    Yes    |    NSG eklerseniz bu bağlantı noktasının giden trafik için açık olduğundan emin olun.    |

   Ayrıca, Azure Depolama uç noktanızın sanal ağınızda kullanılan özel DNS sunucuları tarafından çözümlenebildiğinden emin olun. Özellikle `<account>.table.core.windows.net`, `<account>.queue.core.windows.net` ve `<account>.blob.core.windows.net` biçimindeki URL'ler çözümlenebilir. 

   Resource Manager tabanlı bir NSG eklerseniz, giden bağlantılar için belirli bölgelere ait Depolama IP adreslerini seçmek üzere [hizmet etiketlerinden](../articles/virtual-network/security-overview.md#service-tags) faydalanabilirsiniz. Depolama IP adreslerinin Batch hesabınız ve sanal ağınız ile aynı bölgede olması gerektiğini unutmayın. Hizmet etiketleri şu anda seçili Azure bölgelerinde önizleme aşamasındadır.