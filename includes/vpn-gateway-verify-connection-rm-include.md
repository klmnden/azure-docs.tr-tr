**Sanal ağ geçitleri** **>** ***ağ geçidi adınıza tıklayın*** **>** **Ayarlar** **>** **Bağlantılar**’a giderek Azure portalda VPN bağlantısını doğrulayabilirsiniz. Bağlantının adını seçerek bağlantı hakkında daha fazla bilgi görüntüleyebilirsiniz. Aşağıdaki örnekte, bağlantı bağlı değildir ve üzerinden akan veri yoktur.


![Bağlantıyı doğrulayın](./media/vpn-gateway-verify-connection-rm-include/connectionverify450.png)


### PowerShell kullanarak bağlantınızı doğrulamak için

`Get-AzureRmVirtualNetworkGatewayConnection –Debug` kullanarak da bağlantınızın sorunsuz olduğunu doğrulayabilirsiniz. Aşağıdaki cmdlet örneğini, değerleri kendileriyle eşleşecek şekilde yapılandırırken kullanabilirsiniz. İstendiğinde, tümünü çalıştırmak için 'A' seçeneğini belirleyin.

    Get-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg -Debug

 Cmdlet tamamlandıktan sonra değerleri görüntülemek için ekranı kaydırın. Aşağıdaki örnekte, bağlantı durumu *Bağlandı* olarak gösterilmektedir; giriş ve çıkış baytlarını görebilirsiniz.

    Body:
    {
      "name": "localtovon",
      "id":
    "/subscriptions/086cfaa0-0d1d-4b1c-9455-f8e3da2a0c7789/resourceGroups/testrg/providers/Microsoft.Network/connections/loca
    ltovon",
      "properties": {
        "provisioningState": "Succeeded",
        "resourceGuid": "1c484f82-23ec-47e2-8cd8-231107450446b",
        "virtualNetworkGateway1": {
          "id":
    "/subscriptions/086cfaa0-0d1d-4b1c-9455-f8e3da2a0c7789/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworkGa
    teways/vnetgw1"
        },
        "localNetworkGateway2": {
          "id":
    "/subscriptions/086cfaa0-0d1d-4b1c-9455-f8e3da2a0c7789/resourceGroups/testrg/providers/Microsoft.Network/localNetworkGate
    ways/LocalSite"
        },
        "connectionType": "IPsec",
        "routingWeight": 10,
        "sharedKey": "abc123",
        "connectionStatus": "Connected",
        "ingressBytesTransferred": 33509044,
        "egressBytesTransferred": 4142431
      }


<!--HONumber=Jun16_HO2-->


