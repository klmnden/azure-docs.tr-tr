Bağlantınızın başarılı olduğunu, 'Get-AzureRmVirtualNetworkGatewayConnection' cmdlet’ini '-Debug' ile veya '-Debug' olmadan kullanarak doğrulayabilirsiniz. 

1. Aşağıdaki cmdlet örneğini kullanın ve değerleri, kendi değerlerinizle eşleşecek şekilde yapılandırın. İstendiğinde "Tümünü" çalıştırmak için "A" seçeneğini belirleyin. Örnekte '-Name', oluşturduğunuz ve test etmek istediğiniz bağlantının adıdır.

  ```powershell
  Get-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnection -ResourceGroupName MyRG
  ```
2. cmdlet tamamlandıktan sonra değerleri görüntüleyin. Aşağıdaki örnekte, bağlantı durumu "Bağlandı" olarak gösterilir; ayrıca giriş ve çıkış baytlarını görebilirsiniz.
   
  ```
  "connectionStatus": "Connected",
  "ingressBytesTransferred": 33509044,
  "egressBytesTransferred": 4142431
  ```