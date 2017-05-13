### <a name="gwipnoconnection"></a> 'GatewayIpAddress' yerel ağ geçidini değiştirmek için - ağ geçidi bağlantısı yok

Bağlanmak istediğiniz VPN cihazının genel IP adresi değiştiyse, yerel ağ geçidini bu değişikliği yansıtacak şekilde değiştirmeniz gerekir. Ağ geçidi bağlantısı olmayan bir yerel ağ geçidini değiştirmek için örneği kullanın.

Bu değeri değiştirirken aynı zamanda adres ön eklerini de değiştirebilirsiniz. Geçerli ayarların üzerine yazmak için yerel ağ geçidinizin mevcut adını kullandığınızdan emin olun. Farklı bir ad kullanırsanız mevcut olanın üzerine yazmak yerine yeni bir yerel ağ geçidi oluşturursunuz.

```powershell
New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
-Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
-GatewayIpAddress "5.4.3.2" -ResourceGroupName MyRGName
```

### <a name="gwipwithconnection"></a> 'GatewayIpAddress' yerel ağ geçidini değiştirmek için - ağ geçidi bağlantısı var

Bağlanmak istediğiniz VPN cihazının genel IP adresi değiştiyse, yerel ağ geçidini bu değişikliği yansıtacak şekilde değiştirmeniz gerekir. Zaten bir ağ geçidi bağlantısı varsa öncelikle bu bağlantıyı kaldırmanız gerekir. Bağlantı kaldırıldıktan sonra ağ geçidi IP adresini değiştirebilir ve yeni bir bağlantı oluşturabilirsiniz. Aynı zamanda adres ön eklerini de değiştirebilirsiniz. Bunun sonucunda, VPN bağlantınızda kesinti oluşur. Ağ geçidi IP adresini değiştirirken, VPN ağ geçidini silmeniz gerekmez. Yalnızca bağlantıyı kaldırmanız gerekir.
 

1. Bağlantıyı kaldırın. 'Get-AzureRmVirtualNetworkGatewayConnection' cmdlet’ini kullanarak bağlantınızın adını bulabilirsiniz.

  ```powershell
  Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
  -ResourceGroupName MyRGName
  ```
2. 'GatewayIpAddress' değerini değiştirin. Aynı zamanda adres ön eklerini de değiştirebilirsiniz. Geçerli ayarların üzerine yazmak için yerel ağ geçidinizin mevcut adını kullandığınızdan emin olun. Bunu kullanmazsanız, mevcut olanın üzerine yazmak yerine yeni bir yerel ağ geçidi oluşturursunuz.

  ```powershell
  New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
  -Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
  -GatewayIpAddress "104.40.81.124" -ResourceGroupName MyRGName
  ```
3. Bağlantıyı oluşturun. Bu örnekte bir IPsec bağlantı türü yapılandırıyoruz. Bağlantınızı yeniden oluşturduktan sonra, yapılandırmanız için belirtilen bağlantı türünü kullanın. Ek bağlantı türleri için [PowerShell cmdlet](https://msdn.microsoft.com/library/mt603611.aspx) sayfasına bakın.  VirtualNetworkGateway adını almak için 'Get-AzureRmVirtualNetworkGateway' cmdlet'ini çalıştırabilirsiniz.
   
    Değişkenleri ayarlayın.

  ```powershell
  $local = Get-AzureRMLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
  $vnetgw = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName MyRGName
  ```
   
    Bağlantıyı oluşturun.

  ```powershell 
  New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName `
  -Location "West US" `
  -VirtualNetworkGateway1 $vnetgw `
  -LocalNetworkGateway2 $local `
  -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
  ```