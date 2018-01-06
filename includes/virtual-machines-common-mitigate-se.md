
 
Son açıklanması bir [CPU güvenlik açıklarının yeni sınıf](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180002) kurgusal yürütme yan kanal saldırıları olarak bilinen daha fazla netlik aramayı müşterilerden soruları sonuçlandı. 

 
## <a name="azure-infrastructure"></a>Azure altyapı

Güvenlik Açığı açığa açıklanan sorunları, hiper yönetici sınır atlayıp iki birlikte barındırılan sanal makineler arasında belleğin açığa çıkmasına neden için kullanılabilir. Daha önceki bir bildirilen [blog gönderisi](https://azure.microsoft.com/blog/securing-azure-customers-from-cpu-vulnerability/), Azure müşteriler bu güvenlik açığına karşı korumak için Azaltıcı Etkenler uygulanan.  Microsoft, müşterilerin kendi işletim sistemi satıcısının tüm güvenlik güncelleştirmelerini yükleme dahil olmak üzere, VM görüntüleri için en iyi yöntemler uygulaması her zaman önerir.

## <a name="paas-services-on-azure"></a>Azure üzerinde PaaS Hizmetleri
Azure PaaS teklifleri varsayılan olarak dağıtılan Azaltıcı Etkenler vardır. Müşteriler tarafından herhangi bir eylem gerekli. Aşağıda Azure bulut Hizmetleri için özel duruma bakın.  


## <a name="azure-cloud-services"></a>Azure Cloud Services

[Azure bulut Hizmetleri](https://azure.microsoft.com/services/cloud-services/) Azaltıcı Etkenler bu güvenlik açıklarına içerir konuk işletim sistemi sürümünü otomatik olarak etkinleştirilmiş otomatik güncelleştirme ile alırsınız. 

Aşağıdaki konuk işletim sistemi sürümleri kurgusal yürütme yan kanal güvenlik açıklarına karşı koruma güncelleştirmeleriyle şunlardır:

* WA-GUEST-OS-5.15_201801-01
* WA-GUEST-OS-4.50_201801-01


Güvenilmeyen kodu barındırmak için Azure bulut hizmetlerini kullanan müşteriler az sayıda etkinleştirmelisiniz [çekirdek sanal adres gölgeleme](#enabling-kernel-virtual-address-shadowing-on-windows-server) konuk işletim sistemi güncelleştirme yanı sıra. Bu yan kanal güvenlik açıkları kurgusal yürütme karşı ek koruma sağlar. Bu bir başlangıç görevi gerçekleştirilebilir. Daha fazla bilgi hakkında müşteriler ve kullanım senaryoları bu özellik ve bunun, nasıl etkinleştirileceğini gerektiren altında sağlanır.


## <a name="azure-virtual-machines-windows--linux"></a>Azure sanal makineleri (Windows & Linux)

Microsoft, müşterilerin tüm güvenlik güncelleştirmelerini yüklemek her zaman önerir. 

Ocak 2018 güvenlik dökümü bu güvenlik açıkları ve daha fazlası için düzeltmeler içerir. İşletim sistemi güncelleştirmelerini yüklemeden önce desteklenen bir virüsten koruma uygulaması çalıştığını doğrulayın. Uyumluluk bilgileri için virüsten koruma yazılımı satıcınıza başvurun. 

Adres kurgusal yürütme güvenlik açıkları, çekirdek gerekli olacaktır ve kullanılabilir olduğunda dağıtım sağlayıcınızdan elde edilebilir Linux güncelleştirmeler için. 

Güvenilmeyen kodu barındırmak için Azure sanal makineleri (Windows) kullanan müşteriler az sayıda etkinleştirmelisiniz [çekirdek sanal adres gölgeleme](#enabling-kernel-virtual-address-shadowing-on-windows-server) kurgusal yürütme yan kanallı karşı ek koruma sağlar güvenlik açıkları.  Hangi müşterilerin ve kullanımı hakkında senaryolar bu özellik ve bunun nasıl etkinleştirileceğini gerektirir. daha fazla bilgi aşağıda verilmiştir.


## <a name="enabling-kernel-virtual-address-shadowing-on-windows-server"></a>Windows Server'da gölgeleme çekirdek sanal adres etkinleştirme

Güvenilmeyen kod yürütmek için Windows Server kullanan müşteriler, çekirdek sanal adres sistemleri için koruma sağlayan gölgeleme güvenilmeyen kod düşük kullanıcı ayrıcalıklarıyla burada yürütüyor adlı bir özelliği etkinleştirmeniz gerekir.

Bu ek koruma performansını etkileyebilir ve varsayılan olarak kapalıdır. Çekirdek sanal adres gölgeleme işlemi işlemi ve çekirdek işlem bilgilerin açığa çıkmasına karşı korur.

Aşağıdaki kategorilerden birini halindeyse VM'ler, bulut Hizmetleri veya şirket içi sunucular riski vardır:

* Sanallaştırma Hyper-V konakları Azure iç içe geçmiş
* Ana bilgisayar (RDSH) Uzak Masaüstü Hizmetleri
* Sanal makine ya da kapsayıcıları ya da veritabanı, güvenilmeyen web içeriği veya iş yükleri için güvenilmeyen uzantıları gibi güvenilmeyen kod çalıştıran bulut Hizmetleri dış kaynaklardan sağlanan kodu çalıştırın.

Çekirdek sanal adres gölgeleme gerekli olduğu bir senaryo örneği: 

|     |
|-----|
|Bir Azure sanal makinesi bir hizmet güvenilmeyen kullanıcıların gönderebileceği yürütülür JavaScript kodu sınırlı ayrıcalıkları ile çalışır. Aynı VM'de, bu kullanıcılar için erişilebilir olmamalıdır gizli verileri içeren üst düzey ayrıcalıklı bir işlem bulunmaktadır. Bu durumda, çekirdek sanal adres gölgeleme ikisi arasındaki açığa çıkmasına karşı koruma sağlamak gereklidir.|
|     | 

Çekirdek sanal adres gölgeleme etkinleştirme hakkında belirli yönergeler aracılığıyla kullanılabilen [Windows Server KB4072698](https://support.microsoft.com/help/4072698/windows-server-guidance-to-protect-against-the-speculative-execution).


> [!NOTE]
> Yayın zaman çekirdek sanal adres gölgeleme yalnızca Windows Server 2016, Windows Server 2012 R2 ve Windows Server 2008 R2'de kullanılabilir.  
>
>

Bir Linux sunucusu çalıştırıyorsanız, güncelleştirmeler ve yönergeler için işletim sistemlerinin satıcısına başvurun.

## <a name="branch-target-injection-mitigation-support-microcode"></a>Şube hedef ekleme azaltma desteği (mikro kodları)

Mikro kodları tabanlı Azaltıcı varlığını tespit araçları kullanan müşteriler, Azure yüklenmemiş olduğunu bildirir. Bu yanlıştır. Bu yayın zaman, Azure sanal makine ya da Azure Cloud Services Azure hiper yöneticiden şube hedef ekleme azaltma desteği gösterilmez. Bu sanal makineleri mikro kodları varlığını farkında değildir ve kendi genişletilmiş yönerge kümesi kullanamazsınız anlamına gelir. Bu, Azure arası VM kurgusal yürütme yan kanal saldırılarına karşı savunmasız olduğunu gelmez.
 
Biz endüstri iş ortaklarının ile çalışırken daha fazla güncelleştirme kullanılabilir duruma gelebilir.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz: [CPU güvenlik açığı güvenliğini sağlama Azure müşterilerden](https://azure.microsoft.com/blog/securing-azure-customers-from-cpu-vulnerability/).