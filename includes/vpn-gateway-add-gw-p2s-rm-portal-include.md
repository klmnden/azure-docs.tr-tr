1. Portalda, sol taraftaki **+** simgesine tıklayın ve arama alanına ‘Sanal Ağ Geçidi’ yazın. Arama sonuçlarında **Sanal ağ geçidi** seçeneğini bulun ve girişe tıklayın. **Sanal ağ geçidi** sayfasının en altındaki **Oluştur** seçeneğine tıklayarak **Sanal ağ geçidi oluştur** sayfasını açın.
2. **Sanal ağ geçidi oluştur** sayfasında, sanal ağ geçidinize ait değerleri girin.

  ![Sanal ağ geçidi oluştur sayfasının alanları](./media/vpn-gateway-add-gw-p2s-rm-portal-include/p2sgw.png "Sanal ağ geçidi oluştur sayfasının alanları")
3. **Ad**: Ağ geçidinizi adlandırın. Ağ geçidini adlandırma, bir ağ geçidi alt ağını adlandırmayla aynı değildir. Bu ad, oluşturduğunuz ağ geçidi nesnesinin adıdır.
4. **Ağ geçidi türü**: **VPN**’i seçin. VPN ağ geçitleri, **VPN** sanal ağ geçidi türünü kullanır.
5. **VPN türü**: Yapılandırmanızla ilgili belirtilen VPN türünü seçin. Çoğu yapılandırma, Yol Tabanlı bir VPN türü gerektirir.
6. **SKU**: Açılır listeden ağ geçidi SKU’sunu seçin. Açılır listede sıralanan SKU’lar seçtiğiniz VPN türüne bağlıdır.
7. **Konum**: **Konum** alanını, sanal ağınızın bulunduğu konumu işaret edecek şekilde ayarlayın. Konum, sanal ağınızın bulunduğu bölgeye işaret etmiyorsa, 'Sanal ağ seçin' açılır menüsünde sanal ağ görüntülenmez.
8. Ağ geçidi eklemek istediğiniz sanal ağı seçin. **Sanal ağ**'a tıklayarak **Sanal ağ seçin** sayfasını açın. VNet'i seçin. Sanal ağınızı görmüyorsanız **Konum** alanının, sanal ağınızın bulunduğu bölgeyi işaret ettiğinden emin olun.
9. **Genel IP adresi**: Dinamik olarak bir genel IP adresinin atanacağı bir genel IP adresi nesnesi oluşturun. **Genel IP adresi**'ne tıklayarak **Genel IP adresi seçin** sayfasını açın. **+Yeni Oluştur**'a tıklayarak **Genel IP adresi oluştur** sayfasını açın. Genel IP adresiniz için bir ad girin. Değişikliklerinizi kaydetmek için **Tamam**’a tıklayın. VPN ağ geçidi oluşturulurken, IP adresi dinamik olarak atanır. VPN Gateway hizmeti şu anda yalnızca *Dinamik* Genel IP adresi ayırmayı desteklemektedir. Ancak, bu durum IP adresinin VPN ağ geçidinize atandıktan sonra değiştiği anlamına gelmez. Genel IP adresi, yalnızca ağ geçidi silinip yeniden oluşturulduğunda değişir. VPN ağ geçidiniz üzerinde gerçekleştirilen yeniden boyutlandırma, sıfırlama veya diğer iç bakım/yükseltme işlemleri sırasında değişmez.
10. **Abonelik**: Doğru aboneliğin seçildiğini doğrulayın.
11. **Kaynak grubu**: Bu ayar, seçtiğiniz Sanal Ağ tarafından belirlenir.
12. Önceki ayarları belirttikten sonra **Konum**'u ayarlamayın.
13. Ayarları doğrulayın. Ağ geçidinizin panoda görünmesini istiyorsanız sayfanın en altında yer alan **Panoya sabitle** seçeneğini belirleyebilirsiniz.
14. Ağ geçidi oluşturmaya başlamak için **Oluştur**’a tıklayın. Ayarlar doğrulanır ve ağ geçidi dağıtılır. Bir ağ geçidi oluşturma 45 dakika kadar sürebilir.

Ağ geçidi oluşturulduktan sonra, sanal ağı görüntüleyerek bu ağ geçidine atanmış IP adresini görüntüleyebilirsiniz. Ağ geçidi bağlı bir cihaz gibi görüntülenir. Daha fazla bilgi görüntülemek için bağlı cihaza (sanal ağ geçidiniz) tıklayabilirsiniz.