


## İşlemin, ağın ve depolamanın Azure Resource Manager dağıtım modeli altında tümleştirilmesinin avantajları

Azure Resource Manager dağıtım modeli, önceden derlenmiş uygulama şablonlarını kolay bir şekilde geliştirme veya Azure’da işlem, ağ ve depolama kaynaklarını dağıtacak ve yönetecek uygulama şablonu oluşturma becerisini sunar. Bu bölümde, Azure Resource Manager dağıtım modelinde kaynakların dağıtılmasının getirdiği avantajlarda bir gezinti yapacağız.

-   Karmaşıklık basitleştirildi -- Paylaşılabilir bir şablon dosyasına ait Azure kaynaklarının (Web siteleri, SQL Veritabanları, Sanal Makineler veya Sanal Ağlar gibi) tam skalasını barındırabilen karmaşık uygulamalarda derleme, tümleştirme ve işbirliği yapma
-   Aynı şablon dosyasını kullandığınızda geliştirme, devOps ve sistem yöneticileri için yinelenebilir dağıtımlara sahip olacak esneklik
-   VM Uzantılarının (Özel Betikler, DSC, Chef, Puppet vb.) Azure Resource Manager ile bir şablon dosyasındaki kapsamlı tümleşmesi VM içi kurulum yapılandırmasına ait kolay düzenlemeyi sağlar
-   Etiketleri ve işlem, ağ ve depolama kaynakları için bu etiketlerin fatura yayılmasını tanımlama
-   Azure Rol Tabanlı Erişim Denetimi (RBAC) kullanarak basit ve hassas kurumsal kaynağa erişim yönetimi
-   Asıl şablonu değiştirip yeniden dağıtarak basitleştirilmiş Yükseltme/Güncelleştirme hikayesi


## Azure Resource Manager Altında İşlem, Ağ ve Depolama API'leri geliştirmeleri

Yukarıda söz edilen avantajlara ek olarak, yayımlanan API'lerde bazı önemli performans geliştirmeleri vardır:

-   Virtual Machines’in toplu ve paralel dağıtımını etkinleştirme
-   Kullanılabilirlik Kümelerinde 3 Hata Etki Alanı için destek
-   Herhangi bir genel erişimli özel URL’ye ait betiklerin belirtimine izin veren Geliştirilmiş Özel Betik uzantısı
- [FIPS doğrulamalı](http://wikipedia.org/wiki/FIPS_140-2) [Donanım Güvenlik Modülleri](http://wikipedia.org/wiki/Hardware_security_module)’ne ait çok gizli depolama ve gizli dizilerin özel dağıtımı için Azure Anahtar Kasasıyla Virtual Machines’in tümleşmesi
-   Ağ Arabirimleri, Yük Dengeleyicileri ve Sanal Ağları da kapsayan karmaşık uygulamaları müşterilerin oluşturmasını sağlamak için API’ler aracılığıyla ağın temel yapı taşlarını sağlar
-   Yeni nesne olarak Ağ Arabirimleri, Virtual Machines için dayanacak ve yeniden kullanılacak karmaşık ağ yapılandırmasını sağlar
-   Birinci sınıf kaynak olarak Yük Dengeleyiciler IP Adresi atamalarını sağlar
-   Ayrıntılı Sanal Ağ API'leri, tek tek Sanal Ağların yönetimini basitleştirmenize izin verir

## Yeni API’lerin girişiyle kavramsal farklar

Bu bölümde, bugün kullanımda olana XML tabanlı API’ler ve İşlem, Ağ ve Depolama için Azure Resource Manager’da kullanılabilen JSON tabanlı API’ler arasındaki en önemli kavramsal farklılıklardan bazılarında gezineceğiz.

 Öğe | Azure Hizmet Yönetimi (XML tabanlı)    | İşlem, Ağ ve Depolama Sağlayıcıları (JSON tabanlı)
 ---|---|---
| Virtual Machines için Bulut Hizmeti |  Bulut Hizmeti, platformdan ve Yük Dengeleme’den Uygunluk gereken sanal makineleri barındırmak için bir kapsayıcıydı. | Bulut Hizmeti artık yeni modeli kullanarak bir Sanal Makine oluşturmak için gereken nesne değildir. |
| Kullanılabilirlik Kümeleri | Platformun uygunluğu Sanal Makinelerde "AvailabilitySetName" yapılandırılarak belirtilirdi. Hata etki alanlarının en yüksek sayısı 2’ydi. | Kullanılabilirlik kümesi, Microsoft.Compute Sağlayıcısı tarafından sağlanan bir kaynaktır. Yüksek kullanılabilirliğin gerektiği Sanal Makineler Kullanılabilirlik Kümesi içinde bulunmalıdır. Hata etki alanlarının en yüksek sayısı artık 3’tür. |
| Benzeşim Grupları | Benzeşim Grupları Sanal Ağlar oluşturmak için gerekliydi. Ancak, Bölgesel Sanal Aağlar girişiyle artık gerekmiyordu. |Basitleştirmek için, Benzeşim Grupları kavramı Azure Resource Manager aracılığıyla sunulan API'lerde yok. |
| Yük Dengeleme    | Bulut Hizmeti oluşturulması dağıtılan Virtual Machines için örtük yük dengeleyici sağlar. | Load Balancer, Microsoft.Network sağlayıcısı tarafından sunulan bir kaynaktır. Yük dengeli olması gereken Virtual Machines’in birincil ağ arabirimi yük dengeleyiciye baş vurmalıdır. Yük Dengeleyiciler dahili ve harici olabilir. [Daha fazla bilgi edinin.](../articles/resource-groups-networking.md) |
|Sanal IP Adresi | bulut hizmetine Bir VM eklendiğinde, Cloud Services varsayılan VIP’i (Sanal IP Adresi) alır. Sanal IP Adresi örtük yük dengeleyiciyle ilişkili adrestir.   | Genel IP Adresi, Microsoft.Network sağlayıcısı tarafından sunulan bir kaynaktır. Genel IP Adresi Statik (Ayrılmış) veya Dinamik olabilir. Dinamik Genel IP’ler bir Load Balancer atanabilir. Genel IP’ler Güvenlik Grupları kullanılarak güvenli hale getirilebilir. |
|Ayrılmış IP Adresi|   Azure’da bir IP adresini ayırabilir ve IP Adresinin yapışkan olmasını sağlamak için bunu Bulut Hizmetiyle ilişkilendirebilirsiniz.   | Genel IP Adresi "Statik" modunda oluşturulabilir ve bir "Ayrılmış IP Adresi" ile aynı özelliği sunar. Statik Genel IP’ler yalnızca bir Yük dengeleyiciye hemen şimdi atanabilir. |
|Genel IP Adresi (PIP) / VM | Genel IP Adresleri ayrıca VM ile doğrudan ilişkilendirilebilir. | Genel IP Adresi, Microsoft.Network sağlayıcısı tarafından sunulan bir kaynaktır. Genel IP Adresi Statik (Ayrılmış) veya Dinamik olabilir. Ancak, yalnızca dinamik Genel IP’ler, her VM için hemen şimdi bir Genel IP almak amacıyla bir Ağ Arabirimine atanabilir. |
|Uç Noktalar| Giriş Uç Noktaları, belirli bağlantı noktaları bağlantısının açık olması için bir Sanal Makinede yapılandırılmalıdır. Sanal makinelere bağlanmanın yaygın modlarında biri de giriş uç noktaları ayarlanarak yapılır. | Gelen NAT kuralları, VM’lere bağlanması için belirli bağlantı noktalarındaki uç noktaların etkinleştirilmesiyle aynı beceriyi gerçekleştirmek için Yük Dengeleyicilerde yapılandırılabilir. |
|DNS Adı| Bulut hizmeti örtük bir genel benzersiz DNS Adı almalıdır. Örneğin: `mycoffeeshop.cloudapp.net`. | DNS Adları, Genel IP Adresi kaynağında belirtilebilen isteğe bağlı parametrelerdir. FQDN şu biçimde olacaktır - `<domainlabel>.<region>.cloudapp.azure.com`. |
|Ağ Arabirimleri | Birincil ve İkincil Ağ Arabirimi ve özellikleri Sanal makinenin ağ yapılandırması olarak tanımlanmıştı. | Ağ Arabirimi, Microsoft.Network Sağlayıcısı tarafından sunulan bir kaynaktır. Ağ Arabiriminin yaşam döngüsü bir Sanal Makineye bağlı değildir. |

## Sanal Makineler için Azure Şablonlarına Başlama

Platformda geliştirme ve dağıtma için sahip olduğumuz çeşitli araçları geliştirerek Azure Şablonlarına başlayabiliriz.

### Azure Portal

Azure portalı, klasik dağıtım modeline sahip Sanal Makineleri ve Resource Manager dağıtım modeline sahip Sanal Makineleri aynı anda dağıtma seçeneğiyle devam edecektir. Azure portalı özel şablon dağıtımlarına da izin verecektir.

### Azure PowerShell

Azure PowerShell iki dağıtım moduna sahip olacaktır – **AzureServiceManagement** modu ve **AzureResourceManager** modu.  AzureResourceManager modunda artık Sanal Makineleri, Sanal Ağları ve Depolama Hesaplarını yönetecek cmdlet’ler bulunmaktadır. Bunun hakkında daha fazla bilgiyi [buradan](../articles/powershell-azure-resource-manager.md) edinebilirsiniz.

### Azure CLI

Azure Komut Satırı Arabirimi (Azure CLI) iki dağıtım moduna sahip olacaktır – **AzureServiceManagement** modu ve **AzureResourceManager** modu. AzureResourceManager modunda artık Sanal Makineleri, Sanal Ağları ve Depolama Hesaplarını yönetecek komutlar bulunmaktadır. Bunun hakkında daha fazla bilgiyi [buradan](../articles/xplat-cli-azure-resource-manager.md) edinebilirsiniz.

### Visual Studio

Visual Studio için en son Azure SDK sürümüyle Sanal Makineleri ve karmaşık uygulamaları hemen Visual Studio'dan yazabilir ve dağıtabilirsiniz. Visual Studio, şablonların önceden derlenmiş listesinden dağıtma veya boş bir şablondan başlama becerisini sunar.

### REST API'leri

İşlem, Ağ ve Depolama Kaynak Sağlayıcıları için ayrıntılı REST API belgelerini [burada](https://msdn.microsoft.com/library/azure/dn790568.aspx) bulabilirsiniz.

## Sık Sorulan Sorular

**Azure Hizmet Yönetim API'leri kullanılarak oluşturulan Sanal Ağ veya Depolama Hesabında dağıtmak üzere yeni Azure Resource Manager’ı kullanarak Sanal Makine oluşturabilir miyim?**

Bu, şu anda desteklenmiyor. Hizmet Yönetim API'leri kullanılarak oluşturulmuş Sanal Ağda bir Sanal Makine dağıtmak için yeni Azure Resource Manager API’lerini kullanarak dağıtamazsınız.

**Azure Hizmet Yönetim API'leri kullanılarak oluşturulmuş kullanıcı görüntüsünden yeni Azure Resource Manager’ı kullanarak Sanal Makine oluşturabilir miyim?**

Bu, şu anda desteklenmiyor. Ancak, Hizmet Yönetim API'leri kullanılarak oluşturulmuş Depolama Hesabına ait VHD dosyalarını kopyalayabilir ve bunu yeni Azure Resource Manager API’leri kullanılarak oluşturulan yeni hesaba kopyalayabilirsiniz.

**Aboneliğimle ilgili kotaya etkisi nedir?**

Yeni Azure Resource Manager API’leri üzerinden oluşturulan Sanal Makineler, Sanal Ağlar ve Depolama Hesapları kotaları şu anda sahip olduğunuz kotalardan ayrıdır. Her abonelik, yeni API'leri kullanarak kaynak oluşturmak için yeni kotalar alır. Ek kotalar hakkında daha fazla bilgiyi [burada](../articles/azure-subscription-service-limits.md) okuyabilirsiniz.

**Yeni Azure Resource Manager API’leri aracılığıyla Sanal Makineleri, Sanal Ağları, Depolama Hesaplarını vb. sağlamak için otomatik betiklerimi kullanmaya devam edebilir miyim?**

Derlediğiniz tüm otomasyon ve betikler, Azure Hizmet Yönetimi modu altında oluşturulan varolan Sanal Makineler, Sanal Ağlar için çalışmaya devam edecektir. Ancak, yeni Azure Resource Manager aracılığıyla aynı kaynakların oluşturulmasında yeni şemayı kullanmak için betikler güncelleştirilmelidir.

**Azure Resource Manager Şablonları örneklerini nerede bulabilirim?**

Yeni başlayanlar için kapsamlı bir dizi şablon [Azure Resource Manager Hızlı Başlangıç Şablonlarında](https://azure.microsoft.com/documentation/templates/) bulunabilir.


<!--HONumber=Sep16_HO3-->


