> [!div class="op_single_selector"]
> * [Portal](../articles/virtual-network/virtual-network-multiple-ip-addresses-portal.md)
> * [PowerShell](../articles/virtual-network/virtual-network-multiple-ip-addresses-powershell.md)
> * [CLI](../articles/virtual-network/virtual-network-multiple-ip-addresses-cli.md)
> * [Şablon](../articles/virtual-network/virtual-network-multiple-ip-addresses-template.md)
>

Bir Azure Sanal Makinesine (VM) bağlı bir veya daha fazla ağ arabirimi (NIC) vardır. Herhangi bir NIC’e atanmış bir veya daha fazla statik ya da dinamik ortak ve özel IP adresi olabilir. Bir sanal makineye birden fazla IP adresinin atanması aşağıdaki özellikleri sağlar:

* Tek bir sunucuda farklı IP adreslerine ve SSL sertifikalarına sahip birden fazla web sitesi veya hizmetin barındırılması.
* Güvenlik duvarı veya yük dengeleyici gibi bir sanal ağ gereci olarak görev yapma.
* NIC’lerin herhangi biri için herhangi bir özel IP adresini Azure Load Balancer arka uç havuzuna ekleyebilme. Geçmişte, arka uç havuzuna yalnızca birincil NIC’nin birincil IP adresi eklenebiliyordu. Birden fazla IP yapılandırmasının yük dengelemesi hakkında daha fazla bilgi için [Birden fazla IP yapılandırmasının yük dengelemesi](../articles/load-balancer/load-balancer-multiple-ip.md) makalesini okuyun.

Bir sanal makineye bağlanan her NIC ile ilişkili bir veya daha fazla IP yapılandırması vardır. Her yapılandırmaya bir statik veya dinamik özel IP adresi atanır. Her yapılandırmayla ilişkili bir genel IP adresi kaynağı da olabilir. Genel bir IP adresi kaynağına atanmış dinamik veya statik bir genel IP adresi vardır. Azure'daki IP adresleri hakkında daha fazla bilgi edinmek için [Azure’da IP adresleri](../articles/virtual-network/virtual-network-ip-addresses-overview-arm.md) makalesini okuyun. Her NIC’ye en fazla 250 özel IP adresi atayabilirsiniz. Her NIC'ye birden fazla genel IP adresi atayabilseniz de, bir Azure aboneliğinde kullanılabilecek genel IP adreslerinin sayısı sınırlıdır. Ayrıntılar için [Azure limitleri](../articles/azure-subscription-service-limits.md#networking-limits) makalesini okuyun.


<!--HONumber=Jan17_HO3-->


