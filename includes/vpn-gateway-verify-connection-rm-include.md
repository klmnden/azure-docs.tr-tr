### <a name="to-verify-your-connection-by-using-powershell"></a>PowerShell'i kullanarak bağlantınızı doğrulama
`-Debug` öğesinin mevcut olup olmadığından bağımsız olarak `Get-AzureRmVirtualNetworkGatewayConnection` cmdlet'i ile bağlantınızın başarılı olup olmadığını doğrulayabilirsiniz. 

1. Aşağıdaki cmdlet örneğini kullanın ve değerleri, kendi değerlerinizle eşleşecek şekilde yapılandırın. İstendiğinde "Tümünü" çalıştırmak için "A" seçeneğini belirleyin. Örnekte `-Name`, oluşturduğunuz ve test etmek istediğiniz bağlantının adıdır.
   
        Get-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnection -ResourceGroupName MyRG
2. cmdlet tamamlandıktan sonra değerleri görüntüleyin. Aşağıdaki örnekte, bağlantı durumu "Bağlandı" olarak gösterilir; ayrıca giriş ve çıkış baytlarını görebilirsiniz.
   
        Body:
        {
          "name": "MyGWConnection",
          "id":
        "/subscriptions/086cfaa0-0d1d-4b1c-94544-f8e3da2a0c7789/resourceGroups/MyRG/providers/Microsoft.Network/connections/MyGWConnection",
          "properties": {
            "provisioningState": "Succeeded",
            "resourceGuid": "1c484f82-23ec-47e2-8cd8-231107450446b",
            "virtualNetworkGateway1": {
              "id":
        "/subscriptions/086cfaa0-0d1d-4b1c-94544-f8e3da2a0c7789/resourceGroups/MyRG/providers/Microsoft.Network/virtualNetworkGa
        teways/vnetgw1"
            },
            "localNetworkGateway2": {
              "id":
        "/subscriptions/086cfaa0-0d1d-4b1c-94544-f8e3da2a0c7789/resourceGroups/MyRG/providers/Microsoft.Network/localNetworkGate
        ways/LocalSite"
            },
            "connectionType": "IPsec",
            "routingWeight": 10,
            "sharedKey": "abc123",
            "connectionStatus": "Connected",
            "ingressBytesTransferred": 33509044,
            "egressBytesTransferred": 4142431
          }

### <a name="to-verify-your-connection-by-using-the-azure-portal"></a>Azure portalını kullanarak bağlantınızı doğrulama
Azure portalında, bağlantıya giderek bağlantı durumunu görüntüleyebilirsiniz. Bunu yapmanın birden çok yolu vardır. Aşağıdaki adımlarda, bağlantınıza gitmek ve doğrulamak için bir yol gösterilmiştir.

1. [Azure portal](http://portal.azure.com)’da, **Tüm kaynaklar**’a tıklayın ve sanal ağ geçidinize gidin.
2. Sanal ağ geçidinizin dikey penceresinde **Bağlantılar**’a tıklayın. Her bağlantının durumunu görebilirsiniz.
3. **Temel Bileşenler**’i açmak için doğrulamak istediğiniz bağlantının adına tıklayın. Temel Bileşenler’de, bağlantınız hakkında daha fazla bilgi görebilirsiniz. Başarılı bir bağlantı gerçekleştirdiğinizde, **Durum** ‘Başarılı’ ve ‘Bağlandı’ olur.
   
    ![Bağlantıyı doğrulayın](./media/vpn-gateway-verify-connection-rm-include/connectionsucceeded.png)

<!--HONumber=Oct16_HO3-->


