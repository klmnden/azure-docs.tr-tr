Ağ geçidi IP adresini değiştirmek için `New-AzureRmVirtualNetworkGatewayConnection` cmdlet'ini kullanın. Yerel ağ geçidinin adını mevcut ad ile aynı şekilde tuttuğunuz sürece ayarlar mevcut ayarların üzerine yazılır. Bu noktada, "Set" cmdlet'i ağ geçidi IP adresinin değiştirilmesini desteklemez.

### <a name="gwipnoconnection"></a>Ağ geçidi bağlantısı olmadan ağ geçidi IP adresini değiştirme

Henüz bağlantısı olmayan yerel ağ geçidiniz için ağ geçidi IP adresini güncelleştirmek üzere aşağıdaki örneği kullanın. Aynı zamanda adres ön eklerini de güncelleştirebilirsiniz. Belirttiğiniz ayarlar, mevcut ayarların üzerine yazılır. Yerel ağ geçidinizin mevcut adını kullandığınızdan emin olun. Aksi halde mevcut olanın üzerine yazmaz ve yeni bir yerel ağ geçidi oluşturursunuz.

Aşağıdaki örneği kullanarak değerleri kendi değerlerinizle değiştirin.

    New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
    -Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
    -GatewayIpAddress "5.4.3.2" -ResourceGroupName MyRGName


### <a name="gwipwithconnection"></a>Mevcut ağ geçidi bağlantısını kullanarak ağ geçidi IP adresini değiştirme

Zaten bir ağ geçidi bağlantısı varsa öncelikle bu bağlantıyı kaldırmanız gerekir. Ardından ağ geçidi IP adresini değiştirebilir ve yeni bir bağlantı oluşturabilirsiniz. Bunun sonucunda, VPN bağlantınızda bazı kesintiler oluşacaktır.


>[AZURE.IMPORTANT] VPN ağ geçidini silmeyin. Böyle bir durumda, bağlantıyı yeniden oluşturma adımlarını tekrar etmeniz ve şirket içi yönlendiricinizi, yeni oluşturulan ağ geçidine atanan IP adresi ile yeniden yapılandırmanız gerekir.
 

1. Bağlantıyı kaldırın. `Get-AzureRmVirtualNetworkGatewayConnection` cmdlet'ini kullanarak bağlantınızın adını bulabilirsiniz.

        Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
        -ResourceGroupName MyRGName

2. GatewayIpAddress değerini değiştirin. Gerekirse bu noktada adres ön eklerinizi de değiştirebilirsiniz. Bu değişikliğin, var olan yerel ağ geçidi ayarlarınızın üzerine yazılacağını unutmayın. Mevcut ayarların üzerine yazılması için, değişiklik yaparken yerel ağ geçidinizin mevcut adını kullanın. Aksi halde mevcut olanı değiştirmez ve yeni bir yerel ağ geçidi oluşturursunuz.

        New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
        -Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
        -GatewayIpAddress "104.40.81.124" -ResourceGroupName MyRGName

3. Bağlantıyı oluşturun. Bu örnekte bir IPsec bağlantı türü yapılandırılmaktadır. Bağlantınızı yeniden oluşturduktan sonra, yapılandırmanız için belirtilen bağlantı türünü kullanın. Ek bağlantı türleri için [PowerShell cmdlet](https://msdn.microsoft.com/library/mt603611.aspx) sayfasına bakın.  VirtualNetworkGateway adını almak için `Get-AzureRmVirtualNetworkGateway` cmdlet'ini çalıştırın.

    Değişkenleri ayarlayın:

        $local = Get-AzureRMLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
        $vnetgw = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName MyRGName

    Bağlantıyı oluşturun:
    
        New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName `
        -Location "West US" `
        -VirtualNetworkGateway1 $vnetgw `
        -LocalNetworkGateway2 $local `
        -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'




<!--HONumber=Aug16_HO4-->


