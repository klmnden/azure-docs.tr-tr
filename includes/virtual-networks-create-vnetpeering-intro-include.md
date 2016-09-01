VNet Eşlemesi aynı bölgedeki iki Sanal Ağı Azure omurga ağı aracılığıyla birbirine bağlayan bir mekanizmadır. Eşlendikten sonra iki Sanal Ağ tüm bağlantılarda tek bir Sanal Ağ gibi görünür. VNet Eşlemesi hakkında yeterli bilgi sahibi değilseniz [VNet Eşlemeye genel bakış](../articles/virtual-network/virtual-network-peering-overview.md) konusunu okuyun.

VNet Eşlemesi genel önizleme modundadır. Eşlemeyi kullanabilmek için aşağıdaki komutu kullanarak kaydolmanız gerekir:

    Register-AzureRmProviderFeature -FeatureName AllowVnetPeering -ProviderNamespace Microsoft.Network

    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
 


<!--HONumber=Aug16_HO4-->


