
VPN cihazınızı yapılandırmak için, şirket içi VPN cihazınızın yapılandırılması için size sanal ağ geçidinin genel IP adresi gerekecektir. Belirli yapılandırma bilgileri için cihaz üreticinizle birlikte çalışın ve cihazınızı yapılandırın. Azure ile verimli çalışan VPN cihazları hakkında daha fazla bilgi için [VPN Cihazları](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md)’na başvurun.

PowerShell kullanarak sanal ağ geçidinizin ortak IP adresini bulmak için şu örneği kullanın:

    Get-AzureRmPublicIpAddress -Name GW1PublicIP -ResourceGroupName TestRG

Azure portalı kullanarak da sanal ağ geçidiniz için genel IP adresini görüntüleyebilirsiniz. **Sanal Ağ Geçitleri**’ne gidin ve ağ geçidinizin adına tıklayın.

<!--HONumber=Sep16_HO3-->


