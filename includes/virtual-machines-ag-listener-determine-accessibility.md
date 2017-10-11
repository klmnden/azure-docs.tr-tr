Azure'da bir kullanılabilirlik grubu dinleyicisi yapılandırmanın iki yolu vardır hayata geçirmek önemlidir. Yolu dinleyicisi oluştururken kullandığınız Azure yük dengeleyici türünde farklılık gösterir. Aşağıdaki tabloda farklar açıklanmaktadır:

| Yük Dengeleyici türü | Uygulama | Şu durumlarda kullanın: |
| --- | --- | --- |
| **Dış** |Kullanan *genel sanal IP adresi* sanal makineleri (VM'ler) barındıran bulut hizmeti. |Gelen dinleyicisi Internet'ten dahil olmak üzere sanal ağın dışında erişmesi gerekir. |
| **İç** |Kullanan bir *iç yük dengeleyici* dinleyici için özel bir adresine sahip. |Yalnızca aynı sanal ağda dinleyicisi erişebilir. Bu erişim siteden siteye VPN karma senaryolar içerir. |

> [!IMPORTANT]
> Bulut hizmetinin genel VIP (Dış yük dengeleyici) istemci olarak uzunluğunda kullanan bir dinleyici için dinleyici ve veritabanları aynı Azure bölgesinde, çıkış ücret uygulanmaz. Aksi takdirde dinleyici döndürülen herhangi bir veri çıkış olarak kabul edilir ve normal veri aktarım ücretlerinden ücretlendirilir. 
> 
> 

Bir ILB yalnızca sanal ağları bölgesel kapsama ile yapılandırılabilir. Bir benzeşim grubu için yapılandırılmış olan sanal ağlar bir ILB kullanamazsınız. Daha fazla bilgi için bkz: [iç yük dengeleyiciye genel bakış](../articles/load-balancer/load-balancer-internal-overview.md).

