<properties
   pageTitle="Azure Sanal Ağ Eşlemesi | Microsoft Azure"
   description="Azure’da VNet Eşlemesi hakkında bilgi edinin."
   services="virtual-network"
   documentationCenter="na"
   authors="narayanannamalai"
   manager="jefco"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/28/2016"
   ms.author="narayan" />

# VNet Eşlemesi

VNet Eşlemesi aynı bölgedeki iki Sanal Ağı Azure omurga ağı aracılığıyla birbirine bağlayan bir mekanizmadır. Eşlendikten sonra iki sanal ağ tüm bağlantılarda tek bir sanal ağ gibi görünür. Bunlar ayrı kaynaklar olarak yönetilmeye devam eder, ancak bu VNet'lerdeki sanal makineler özel IP adresi kullanarak birbirleriyle doğrudan iletişim kurabilirler. Eşlenen VNet’lerdeki Sanal makineler arasındaki trafik, Azure Altyapısı aracılığıyla aynı VNet’teki sanal makineler arasında olduğu gibi yönlendirilir. VNet Eşlemesi kullanmanın bazı avantajları şunlardır:

- Farklı VNet’lerdeki kaynaklar arasında düşük gecikme, yüksek bant genişliği bağlantısı.
- Ağ Sanal gereçleri gibi kaynakları ve VPN geçitlerini eşlenmiş VNet (Geçiş) içinde kullanabilme özelliği.
- Resource Manager VNet’i klasik bir VNet’e bağlayın ve bu VNet’lerdeki kaynaklar arasında tam bağlantı sağlayın

VNet eşlemesi ile ilgili gereksinimler temel unsurlar:

- Eşlenen iki Sanal Ağ aynı Azure bölgesinde olmalıdır
- Eşlenen VNet’lerin IP Adresi alanı çakışmamalıdır
- VNet Eşlemesi iki sanal ağ arasında gerçekleşir ve türetilmiş bir ilişki yoktur; örneğin VNet A ile VNet B eşlenirse ve VNet B ile VNet C eşlenirse VNet A ile VNet C’nin eşlendiği anlaşılmamalıdır
- İlgili aboneliklerin ayrıcalıklı bir kullanıcısı eşlemeyi yetkilendirdiği sürece iki farklı abonelikteki Sanal ağlar arasında eşleme kurulabilir
- Resource Manager VNet’i başka bir Resource Manager VNet’i ya da klasik VNet ile eşlenebilir, ancak iki klasik VNet birbiriyle eşlenemez
- Eşlenmiş VNet'lerde Sanal makineler arasındaki iletişim başka bir bant genişliği kısıtlaması içermese de VM boyutuna göre bant genişliği sınırı geçerli olmaya devam eder. 


![VNet Eşlemesi Temel Bilgileri](./media/virtual-networks-peering-overview/figure01.png)

## Bağlantı 
İki VNet eşlendikten sonra Vnet içindeki bir sanal makine (web/çalışan rolü) eşlenmiş VNet içindeki diğer sanal makinelerle bağlantı kurabilir. Bunlar tam IP düzeyi bağlantısına sahip olur. Eşlenmiş Vnet’ler içindeki iki Sanal makine arasında çift yönlü ağ gecikmesi, yerel VNet içindekiyle aynı olur. Ağ verimliliği, Sanal makine için boyutuyla orantılı olarak izin verilen bant genişliğine bağlıdır ve izin verilen bant genişliği üzerinde başka bir kısıtlama olmaz. Eşlenmiş Vnet’ler içindeki Sanal makineler arası trafik bir ağ geçidi üzerinden değil, doğrudan Azure arka uç altyapısı aracılığıyla yönlendirilir.

Bir Vnet içindeki sanal makineler, eşlenmiş VNet içindeki İç yük dengeli uç noktalara (ILB) erişebilir. İstenirse diğer Vnet’e veya alt ağa erişimi engellemek için herhangi bir Vnet’e Ağ Güvenlik Grupları uygulanabilir. Kullanıcı eşlemeyi yapılandırırken VNet’ler arasında Ağ Güvenlik Grubu kurallarını açma veya kapatma seçeneğine sahip olur. Kullanıcı eşlenmiş VNet’ler arasında tam bağlantıyı açmayı seçerse (varsayılan seçenek), belirli alt ağlarda veya sanal makinelerde belirli erişimleri engellemek ya da reddetmek için NSG’leri kullanabilir.

Azure tarafından sağlanan iç DNS ad çözünürlüğü, eşlenmiş VNet’lerde çalışmaz. Sanal makineler yalnızca yerel sanal ağ içinde çözümlenebilen iç DNS adlarına sahip olur. Ancak, kullanıcılar eşlenmiş Vnet’lerde çalışan Sanal makineleri bir Sanal ağın DNS sunucuları olarak yapılandırabilir. 

## Hizmet Zincirleme
Kullanıcılar eşlenmiş VNet’lerdeki Sanal makineleri işaret eden kullanıcı tanımlı yönlendirme tablolarını sonraki atlama olarak yapılandırabilir (aşağıdaki diyagramda gösterildiği gibi). Bunun yapılması kullanıcıların bir VNet’ten eşlenmiş VNet’teki Sanal gerece yönelik trafiği kullanıcı tanımlı yönlendirme tabloları ile yönlendirebileceği hizmet zincirlemesini sağlar. Kullanıcılar ayrıca Hub ve uç tipi ortamları etkili bir şekilde derleyebilir ve bu ortamlarda Hub, Sanal Ağ gereci gibi altyapısal bileşenleri barındırırken, tüm uç VNet’ler bununla eşlenebilir ve hub VNet’inde çalışan gereçlere trafik alt ağını yönlendirebilir. Kısacası, VNet eşlemesi ‘Kullanıcı tanımlı yönlendirme tablosundaki’ sonraki atlama IP Adresinin, eşlenmiş VNet’teki sanal makinenin IP Adresi olmasına imkan tanır.

## Ağ Geçitleri ve Şirket içi bağlantısı
Başka bir Vnet ile eşlenip eşlenmemesine bakılmaksızın her Sanal Ağ kendi ağ geçidine sahip olabilir ve şirket içine bağlanmak için kullanılabilir. Kullanıcılar ayrıca VNet’ler eşlenmiş olsa bile ağ geçitlerini kullanarak Sanal Ağdan Sanal Ağa bağlantıyı (bağlantı verme) yapılandırabilir. VNet ara bağlantısına ilişkin her iki seçenek de yapılandırıldığında VNet’ler arasındaki trafik, eşleme bağlantı (Azure omurga) aracılığıyla akar. 

VNet’ler eşlendiğinde kullanıcılar eşlenmiş Vnet’teki ağ geçidini şirket içine bir geçiş noktası olarak kullanılacak şekilde de yapılandırabilir. Bu durumda, uzak bir ağ geçidi kullanan VNet kendi ağ geçidine sahip olamaz; daha basit anlatmak gerekirse, aşağıdaki resimde gösterildiği gibi bir VNet yerel ağ geçidi ya da uzak ağ geçidi olarak yalnızca bir ağ geçidine sahip olabilir. Ağ Geçidi Geçişi bir Resource Manager ve klasik Vnet arasında desteklenmez; ağ geçidi geçişinin çalışması için eşleme ilişkisindeki her iki Vnet’in de Resource Manager Vnet’i olması gerekir.
Tek bir ER devresini paylaşan VNet’ler eşlendiğinde aralarındaki trafik bir eşleme ilişkisinden geçer (Azure omurga ağı aracılığıyla). Kullanıcılar şirket içi devreye bağlanmak için her bir Vnet’teki yerel ağ geçitlerini kullanmaya devam edebilir ya da paylaşılan bir ağ geçidi kullanarak şirket içi bağlantı geçişini yapılandırabilir.

![VNet Eşleme Geçişi](./media/virtual-networks-peering-overview/figure02.png)

## Sağlama
VNet Eşlemesi ayrıcalıklı bir işlemdir. Sanal Ağ ad alanı altındaki ayrı bir işlevdir. Bir kullanıcıya eşlemeyi yetkilendirmesi için belirli haklar verilebilir. VNet üzerinde okuma-yazma erişimi olan bir kullanıcı bunu otomatik olarak devralır. Eşleme özelliğinin yönetici ya da ayrıcalıklı kullanıcısı olan bir kullanıcı başka bir VNet ile eşleme işlemi başlatabilir. Diğer tarafta eşleme için eşleşen bir istek varsa ve diğer gereksinimler karşılanırsa eşleme kurulur. 

İki Sanal Ağ arasında VNet eşlemesi kurma hakkında daha fazla bilgi için lütfen Nasıl Yapılır makalelerine bakın.

## Sınırlar
Tek bir Sanal ağ için izin verilen eşleme sayısı sınırlıdır; daha fazla bilgi için lütfen [Azure Ağ Oluşturma sınırlamaları](../azure-subscription-service-limits.md#networking-limits) bölümüne bakın.

## Fiyatlandırma
VNet Eşlemesi değerlendirme süresi boyunca ücretlendirilmez. Genel Kullanıma sunulduktan sonra eşlemeyi kullanan giriş ve çıkış trafiği için nominal bir ücret alınacaktır. Daha fazla bilgi için lütfen [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/virtual-network)na bakın 


## Sonraki adımlar
- [Sanal Ağlar arasında eşlemeyi ayarlama](virtual-networks-create-vnetpeering-arm-portal.md).
- [NSG'ler](virtual-networks-nsg.md) hakkında bilgi edinin.
- [Kullanıcı tanımlı yollar ve IP iletimi](virtual-networks-udr-overview.md) hakkında bilgi edinin.



<!--HONumber=Aug16_HO1-->


