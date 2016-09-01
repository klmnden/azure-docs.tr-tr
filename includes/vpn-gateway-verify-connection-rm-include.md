### PowerShell'i kullanarak bağlantınızı doğrulama

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

### Azure portalını kullanarak bağlantınızı doğrulama

Azure portalında, bağlantıya giderek bağlantı durumunu görüntüleyebilirsiniz. Bunu yapmanın birden çok yolu vardır. Aşağıda, bağlantınıza gitmenin yollarından biri açıklanmıştır.

1. [Azure portalında](http://portal.azure.com) **Sanal ağ geçitleri**'ne gidin. Ağ geçidi adınıza tıklayın.
2. Bölmede, **Ayarlar** öğesinin altında **Bağlantılar**'a tıklayın. Her bağlantının durumunu görebilirsiniz.
3. Bağlantı hakkında daha fazla bilgi edinmek için bağlantının adına tıklayın. Bağlantınıza ilişkin Temel Bileşenler sayfasında **Bağlantı Durumu**'na dikkat edin. Başarılı bir bağlantı gerçekleştirdiğinizde, durum "Başarılı" ve "Bağlandı" olur. **Veri girişi** ve **Veri çıkışı**'na bakarak veri akışını kontrol edebilirsiniz.

    Aşağıdaki örnekte **Bağlantı Durumu** "Bağlı değil"dir. 

    ![Bağlantıyı doğrulayın](./media/vpn-gateway-verify-connection-rm-include/connectionverify450.png)


<!--HONumber=Aug16_HO4-->


