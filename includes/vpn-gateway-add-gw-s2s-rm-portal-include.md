1. Portal sayfasının sol tarafına **+** simgesine tıklayın ve arama alanına ‘Sanal Ağ Geçidi’ yazın. **Sonuçlar** alanında **Sanal Ağ Geçidi**’ni bulup tıklayın.
2. "Sanal ağ geçidi" dikey penceresinin alt tarafındaki **Oluştur**'a tıklayın. Bu işlem **Sanal ağ geçidi oluştur** dikey penceresini açar.

    ![Sanal ağ geçidi oluştur dikey penceresinin alanları](./media/vpn-gateway-add-gw-s2s-rm-portal-include/vnet_gw.png "Yeni ağ geçidi")

3. **Sanal ağ geçidi oluştur** dikey penceresinde, sanal ağ geçidinize ait değerleri belirtin.

  - **Ad**: Ağ geçidinizi adlandırın. Bir ağ geçidi alt ağını adlandırmayla aynı değildir. Bu ad, oluşturduğunuz ağ geçidi nesnesinin adıdır.
  - **Ağ geçidi türü**: **VPN**’i seçin. VPN ağ geçitleri, **VPN** sanal ağ geçidi türünü kullanır. 
  - **VPN türü**: Yapılandırmanızla ilgili belirtilen VPN türünü seçin. Çoğu yapılandırma, Yol Tabanlı bir VPN türü gerektirir.
  - **SKU**: Açılır listeden ağ geçidi SKU’sunu seçin. Açılır listede sıralanan SKU’lar seçtiğiniz VPN türüne bağlıdır. Ağ geçidi SKU’ları hakkında bilgi için bkz. [Ağ geçidi SKU’ları](../articles/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsku).
  - **Konum**: Konumu görmek için kaydırmanız gerekebilir. **Konum** alanını, sanal ağınızın bulunduğu konumu işaret edecek şekilde ayarlayın. Konum, sanal ağınızın bulunduğu bölgeye işaret etmiyorsa, sonraki adım olan 'Sanal ağ seçin' açılır menüsünde sanal ağ görüntülenmez.
  - **Sanal ağ**: Bu ağ geçidini eklemek istediğiniz sanal ağı seçin. **Sanal ağ**'a tıklayarak "Sanal ağ seçin" dikey penceresini açın. VNet'i seçin. VNet'inizi görmüyorsanız Konum alanının, sanal ağınızın bulunduğu bölgeyi işaret ettiğinden emin olun.
  - **Genel IP adresi**: "Ortak IP adresi oluştur" dikey penceresinde genel bir IP adresi nesnesi oluşturulur. VPN ağ geçidi oluşturulduğunda genel IP adresi dinamik olarak atanır. VPN Gateway hizmeti şu anda yalnızca *Dinamik* Genel IP adresi ayırmayı desteklemektedir. Ancak, bu durum IP adresinin VPN ağ geçidinize atandıktan sonra değiştiği anlamına gelmez. Genel IP adresi, yalnızca ağ geçidi silinip yeniden oluşturulduğunda değişir. VPN ağ geçidiniz üzerinde gerçekleştirilen yeniden boyutlandırma, sıfırlama veya diğer iç bakım/yükseltme işlemleri sırasında değişmez.

    - Önce **Ortak IP Adresi**'ne tıklayarak "Ortak IP adresi seç" dikey penceresini, sonra da **+Yeni Oluştur**'a tıklayarak "Ortak IP adresi oluştur" dikey penceresini açın.
    - Ardından genel IP adresiniz için bir **Ad** girin ve değişikliklerinizi kaydetmek için bu dikey pencerenin alt tarafındaki **Tamam**'a tıklayın.

      ![Genel IP oluşturma](./media/vpn-gateway-add-gw-s2s-rm-portal-include/pip.png "PIP oluşturma")

4. Ayarları doğrulayın. Ağ geçidinizin panoda görünmesini istiyorsanız dikey pencerenin altında yer alan **Panoya sabitle** seçeneğini belirleyebilirsiniz. 
5. VPN ağ geçidi oluşturmaya başlamak için **Oluştur**'a tıklayın. Ayarlar doğrulandıktan sonra, panoda "Sanal ağ geçidi dağıtma" kutucuğunu görürsünüz. Bir ağ geçidi oluşturma 45 dakika kadar sürebilir. Tamamlanma durumunu görmek için portal sayfanızı yenilemeniz gerekebilir.

Ağ geçidi oluşturulduktan sonra, portalda sanal ağa bakarak bu ağ geçidine atanmış IP adresini görüntüleyin. Ağ geçidi bağlı bir cihaz gibi görüntülenir. Daha fazla bilgi görüntülemek için bağlı cihaza (sanal ağ geçidiniz) tıklayabilirsiniz.