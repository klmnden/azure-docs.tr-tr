Bir sanal ağ geçidi oluşturduğunuzda, kullanmak istediğiniz ağ geçidi SKU’sunu belirtmeniz gerekir. Daha yüksek bir ağ geçidi SKU’su seçtiğinizde ağ geçidine daha fazla CPU ve ağ bant genişliği ayrılır ve sonuç olarak ağ geçidi, sanal ağda daha yüksek ağ verimliliğini destekleyebilir.

VPN Gateway aşağıdaki SKU’ları kullanabilir:

* Temel
* Standart
* HighPerformance

VPN Gateway, UltraPerformance ağ geçidi SKU’sunu kullanmaz. UltraPerformance SKU’su hakkında bilgi edinmek için [ExpressRoute](../articles/expressroute/expressroute-about-virtual-network-gateways.md) belgelerine bakın.

SKU seçerken şunları göz önünde bulundurun:

* PolicyBased VPN türünü kullanmak istiyorsanız Temel SKU'yu kullanmanız gerekir. PolicyBased VPN'ler (daha önce Statik Yönlendirme olarak biliniyordu), diğer SKU’larda desteklenmez.
* BGP, Temel SKU’da desteklenmez.
* ExpressRoute-VPN Gateway ağ geçidi bir arada var olabilen yapılandırmaları, temel SKU'da desteklenmez.
* Etkin-etkin S2S VPN Gateway bağlantıları, yalnızca HighPerformance değerine sahip SKU'larda yapılandırılabilir.



<!--HONumber=Nov16_HO2-->


