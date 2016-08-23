### <a name="noconnection"></a>VPN ağ geçidi bağlantısı olmadan ön ek ekleme veya kaldırma

- **Ekleme**; ek adres öneklerini, oluşturduğunuz ancak henüz bir VPN Gateway bağlantısı olmayan yerel ağ geçidine eklemek için aşağıdaki örneği kullanın.

        $local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg
        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')


- **Kaldırma**; bir VPN bağlantısı olmayan yerel ağ geçidine ait adres öneklerini kaldırmak için aşağıdaki örneği kullanın. Artık gereği olmayan önekleri bırakın. Bu örnekte, 20.0.0.0/24 (önceki örnekten) öneki artık bize gerekmiyor; bu nedenle, yerel ağ geçidini güncelleştirip bu öneki de hariç tutacağız.

        $local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg
        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local -AddressPrefix @('10.0.0.0/24','30.0.0.0/24')

### <a name="withconnection"></a>VPN ağ geçidi bağlantısı ile ön ek ekleme veya kaldırma

VPN bağlantınızı oluşturduysanız ve yerel ağ geçidinizinde bulunan IP adresi öneklerini ekleme veya kaldırmak istiyorsanız.aşağıdaki adımları sırayla uygulamanız gerekir. Bunun sonucunda, VPN bağlantınızda bazı kesintiler oluşacaktır. Ön ekleri güncelleştirirken ilk olarak bağlantıyı kaldırmanız, önekleri değiştirmeniz ve sonra yeni bir bağlantı oluşturmanız gerekir. 

>[AZURE.IMPORTANT] VPN ağ geçidini silmeyin. böyle bir durumda, bunu yeniden oluşturmak için adımlarda geri gitmenizin yanı sıra şirket içi yönlendiriciyi de yeni ayarlarla yeniden yapılandırmanız gerekecektir.
 
1. Değişkenleri belirtin.

        $gateway1 = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
        $local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg

2. Bağlantıyı kaldırın.

        Remove-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg

3. Ön ekleri değiştirin.

        $local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
        -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')

4. Bağlantıyı oluşturun. Bu örnekte bir IPsec bağlantı türü yapılandırılmaktadır. Ek bağlantı türleri için [PowerShell cmdlet](https://msdn.microsoft.com/library/mt603611.aspx) sayfasına bakın.
    
        New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg `
        -Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local -ConnectionType IPsec `
        -RoutingWeight 10 -SharedKey 'abc123'



<!--HONumber=Aug16_HO1-->


