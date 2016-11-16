### <a name="a-namenoconnectionahow-to-add-or-remove-prefixes-no-gateway-connection"></a><a name="noconnection"></a>Ağ geçidi bağlantısı olmadan ön ek ekleme veya kaldırma
* Daha önce oluşturduğunuz ancak henüz ağ geçidi bağlantısı olmayan yerel ağ geçidine başka adres ön ekleri **eklemek için** aşağıdaki örneği kullanın. Değerleri kendi değerlerinizle değiştirdiğinizden emin olun.
  
        $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
        -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')
* **Kaldırma**; bir VPN bağlantısı olmayan yerel ağ geçidine ait adres öneklerini kaldırmak için aşağıdaki örneği kullanın. Artık gereği olmayan önekleri bırakın. Bu örnekte, 20.0.0.0/24 (önceki örnekten) öneki artık bize gerekmiyor; bu nedenle, yerel ağ geçidini güncelleştirip bu öneki de hariç tutacağız.
  
        $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
        -AddressPrefix @('10.0.0.0/24','30.0.0.0/24')

### <a name="a-namewithconnectionahow-to-add-or-remove-prefixes-existing-gateway-connection"></a><a name="withconnection"></a>Var olan ağ geçidi bağlantısını kullanarak ön ek ekleme veya kaldırma
Kendi ağ geçidi bağlantınızı oluşturduysanız ve yerel ağ geçidinizde bulunan IP adresi ön eklerini eklemek veya kaldırmak istiyorsanız aşağıdaki adımları sırasıyla uygulamanız gerekir. Bunun sonucunda, VPN bağlantınızda bazı kesintiler oluşacaktır. Ön ekleri güncelleştirirken ilk olarak bağlantıyı kaldırmanız, önekleri değiştirmeniz ve sonra yeni bir bağlantı oluşturmanız gerekir. Aşağıdaki örneklerde verilen değerleri, kendi değerlerinizle değiştirdiğinizden emin olun.

> [!IMPORTANT]
> VPN ağ geçidini silmeyin. böyle bir durumda, bunu yeniden oluşturmak için adımlarda geri gitmenizin yanı sıra şirket içi yönlendiriciyi de yeni ayarlarla yeniden yapılandırmanız gerekecektir.
> 
> 

1. Bağlantıyı kaldırın.
   
        Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName
2. Yerel ağ geçidiniz için adres ön eklerini değiştirin.
   
    LocalNetworkGateway için değişkeni ayarlayın.
   
        $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName
   
    Ön ekleri değiştirin.
   
        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
        -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')
3. Bağlantıyı oluşturun. Bu örnekte bir IPsec bağlantı türü yapılandırılmaktadır. Bağlantınızı yeniden oluşturduktan sonra, yapılandırmanız için belirtilen bağlantı türünü kullanın. Ek bağlantı türleri için [PowerShell cmdlet](https://msdn.microsoft.com/library/mt603611.aspx) sayfasına bakın.
   
     VirtualNetworkGateway için değişkeni ayarlayın.
   
        $gateway1 = Get-AzureRmVirtualNetworkGateway -Name RMGateway  -ResourceGroupName MyRGName
   
    Bağlantıyı oluşturun. Bu örnekte, bir önceki adımda belirlediğiniz $local değişkeninin kullanıldığını unutmayın.

        New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
        -ResourceGroupName MyRGName -Location 'West US' `
        -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
        -ConnectionType IPsec `
        -RoutingWeight 10 -SharedKey 'abc123'


<!--HONumber=Nov16_HO2-->


