Azure şu VPN ağ geçidi SKU'larını sunar:

|**SKU**   | **S2S/VNet-VNet<br>Tünelleri** | **P2S<br>Bağlantıları** | **Toplam<br>Aktarım Hızı** |
|---       | ---                             | ---                    | ---                         |
|**VpnGw1**| En çok, 30                         | En çok, 128               | 500 Mbps                    |
|**VpnGw2**| En çok, 30                         | En çok, 128               | 1 Gbps                      |
|**VpnGw3**| En çok, 30                         | En çok, 128               | 1,25 Gb/sn                   |
|**Temel** | En çok, 10                         | En çok, 128               | 100 Mbps                    | 
|          |                                 |                        |                             | 

- Aktarım hızı, tek bir ağ geçidi üzerinde yer alan birden fazla tünelin ölçümlerine bağlıdır. İnternet trafiğinin koşulları ve uygulamanızın davranışı nedeniyle bu aktarım hızı kesin değildir.

- Fiyatlandırma bilgileri [Fiyatlandırma](https://azure.microsoft.com/pricing/details/vpn-gateway) sayfasında bulunabilir.

- SLA (Hizmet Düzeyi Sözleşmesi) bilgilerine [SLA](https://azure.microsoft.com/en-us/support/legal/sla/vpn-gateway/) sayfasından ulaşılabilir.