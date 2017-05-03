Ağ geçidi IP adresini değiştirmek için 'New-AzureRmVirtualNetworkGatewayConnection' cmdlet’ini kullanın. Şu anda, ‘Set’ cmdlet'i ağ geçidi IP adresinin değiştirilmesini desteklememektedir.

### <a name="gwipnoconnection"></a>Ağ geçidi IP adresini değiştirme - ağ geçidi bağlantısı yok
Henüz bağlantısı olmayan yerel ağ geçidiniz için ağ geçidi IP adresini değiştirmek üzere aşağıdaki örneği kullanın. Aynı zamanda adres ön eklerini de değiştirebilirsiniz. Geçerli ayarların üzerine yazmak için yerel ağ geçidinizin mevcut adını kullandığınızdan emin olun. Bunu kullanmazsanız, mevcut olanın üzerine yazmak yerine yeni bir yerel ağ geçidi oluşturursunuz.

Aşağıdaki örneği kullanarak değerleri kendi değerlerinizle değiştirin:

```powershell
New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
-Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
-GatewayIpAddress "5.4.3.2" -ResourceGroupName MyRGName
```

### <a name="gwipwithconnection"></a>Ağ geçidi IP adresini değiştirme - mevcut ağ geçidi bağlantısı
Zaten bir ağ geçidi bağlantısı varsa öncelikle bu bağlantıyı kaldırmanız gerekir. Bağlantı kaldırıldıktan sonra ağ geçidi IP adresini değiştirebilir ve yeni bir bağlantı oluşturabilirsiniz. Aynı zamanda adres ön eklerini de değiştirebilirsiniz. Bunun sonucunda, VPN bağlantınızda kesinti oluşur.

> [!IMPORTANT]
> VPN ağ geçidini silmeyin. Bunu yaparsanız, yeniden oluşturmak için geri dönüp adımları yeniden izlemeniz gerekir. Buna ek olarak, şirket içi VPN cihazınızı da yeni VPN ağ geçidi IP adresiyle güncelleştirmelisiniz.
> 
> 

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