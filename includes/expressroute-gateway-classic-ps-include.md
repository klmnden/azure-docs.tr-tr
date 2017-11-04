VNet ve bir ağ geçidi alt ağı önce aşağıdaki görevlere çalışmaya başlamadan önce oluşturmanız gerekir. Makalesine bakın [Klasik portalı kullanarak bir sanal ağ yapılandırma](../articles/expressroute/expressroute-howto-vnet-portal-classic.md) daha fazla bilgi için.

> [!NOTE]
> Bu örnekler S2S/ExpressRoute için geçerli olmayan yapılandırmalar bir arada.
> Coexist yapılandırmasında ağ geçitleri ile çalışma hakkında daha fazla bilgi için bkz: [arada var olabilen bağlantılar yapılandırmak](../articles/expressroute/expressroute-howto-coexist-classic.md#gw)

## <a name="add-a-gateway"></a>Bir ağ geçidi Ekle

Bir ağ geçidi oluşturmak için aşağıdaki komutu kullanın. Kendi ilgili tüm değerleri değiştirdiğinizden emin olun.

```powershell
New-AzureVirtualNetworkGateway -VNetName "MyAzureVNET" -GatewayName "ERGateway" -GatewayType Dedicated -GatewaySKU  Standard
```

## <a name="verify-the-gateway-was-created"></a>Ağ geçidinin oluşturulduğunu doğrulayın

Ağ geçidinin oluşturulduğunu doğrulamak için aşağıdaki komutu kullanın. Bu komut ayrıca diğer işlemleri için gereken ağ geçidi kimliği alır.

```powershell
Get-AzureVirtualNetworkGateway
```

## <a name="resize-a-gateway"></a>Bir ağ geçidi yeniden boyutlandırma

Bir dizi vardır [ağ geçidi SKU'ları](../articles/expressroute/expressroute-about-virtual-network-gateways.md). Ağ geçidi SKU'su herhangi bir zamanda değiştirmek için aşağıdaki komutu kullanabilirsiniz.

> [!IMPORTANT]
> Bu komut için UltraPerformance ağ geçidi çalışmıyor. UltraPerformance ağ geçidi için ağ geçidiniz değiştirmek için önce varolan ExpressRoute ağ geçidi kaldırın ve yeni UltraPerformance ağ geçidi oluşturmak. Ağ geçidiniz UltraPerformance geçidinden düşürmek için ilk UltraPerformance ağ geçidi kaldırın ve ardından yeni bir ağ geçidi oluşturun. 
>
>

```powershell
Resize-AzureVirtualNetworkGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance
```

## <a name="remove-a-gateway"></a>Bir ağ geçidi kaldırma

Bir ağ geçidini kaldırmak için aşağıdaki komutu kullanın

```powershell
Remove-AzureVirtualNetworkGateway -GatewayId <Gateway ID>
```
