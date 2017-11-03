1. Portalda, sol taraftaki **+** simgesine tıklayın ve arama alanına "sanal ağ geçidi" yazın. Arama sonuçlarında **Sanal ağ geçidi** seçeneğini bulun ve girişe tıklayın. **Sanal ağ geçidi** sayfasının en altındaki **Oluştur** seçeneğine tıklayarak **Sanal ağ geçidi oluştur** sayfasını açın.
2. **Sanal ağ geçidi oluştur** sayfasında, sanal ağ geçidinize ait değerleri girin.

  ![Sanal ağ geçidi oluştur sayfasının alanları](./media/vpn-gateway-add-gw-rm-portal-include/gw.png "Sanal ağ geçidi oluştur sayfasının alanları")
3. Üzerinde **sanal ağ geçidi Oluştur** sayfasında, sanal ağ geçidiniz için değerleri belirtin.

  - **Ad**: Ağ geçidinizi adlandırın. Bir ağ geçidi alt ağını adlandırmayla aynı değildir. Bu ad, oluşturduğunuz ağ geçidi nesnesinin adıdır.
  - **Ağ geçidi türü**: **VPN**’i seçin. VPN ağ geçitleri, **VPN** sanal ağ geçidi türünü kullanır. 
  - **VPN türü**: Yapılandırmanızla ilgili belirtilen VPN türünü seçin. Çoğu yapılandırma, Yol Tabanlı bir VPN türü gerektirir.
  - **SKU**: Açılır listeden ağ geçidi SKU’sunu seçin. Açılır listede sıralanan SKU’lar seçtiğiniz VPN türüne bağlıdır. Ağ geçidi SKU’ları hakkında bilgi için bkz. [Ağ geçidi SKU’ları](../articles/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsku).
  - **Konum**: Konumu görmek için kaydırmanız gerekebilir. **Konum** alanını, sanal ağınızın bulunduğu konumu işaret edecek şekilde ayarlayın. Konum sonraki adımda bir sanal ağı seçtiğinizde sanal ağınız, bulunduğu bölgeye işaret etmiyor, aşağı açılan listede görünmez.
  - **Sanal ağ**: Bu ağ geçidini eklemek istediğiniz sanal ağı seçin. Tıklatın **sanal ağ** 'sanal ağ seçin' sayfasını açın. VNet'i seçin. VNet'inizi görmüyorsanız Konum alanının, sanal ağınızın bulunduğu bölgeyi işaret ettiğinden emin olun.
  - **Ağ geçidi alt ağ adres aralığı**: bir ağ geçidi alt ağı için sanal ağınıza daha önce oluşturmadıysanız Bu ayar yalnızca görürsünüz. Daha önce geçerli bir ağ geçidi alt ağı oluşturduysanız, bu ayar görünmez.
  - **İlk IP yapılandırması**: 'genel IP adresi seçin' sayfasına VPN ağ geçidi için ilişkili alır bir ortak IP adresi nesnesi oluşturur. VPN ağ geçidi oluşturulduğunda, bu nesne için genel IP adresi dinamik olarak atanır. VPN Gateway hizmeti şu anda yalnızca *Dinamik* Genel IP adresi ayırmayı desteklemektedir. Ancak, bu durum IP adresinin VPN ağ geçidinize atandıktan sonra değiştiği anlamına gelmez. Genel IP adresi, yalnızca ağ geçidi silinip yeniden oluşturulduğunda değişir. VPN ağ geçidiniz üzerinde gerçekleştirilen yeniden boyutlandırma, sıfırlama veya diğer iç bakım/yükseltme işlemleri sırasında değişmez.

    - Önce tıklatın **oluşturma ağ geçidi IP yapılandırması** 'genel IP adresi seçin' sayfasını açın ve ardından için **+ Yeni Oluştur** 'ortak IP adresi oluştur' sayfasını açın.
    - Ardından, Giriş bir **adı** ortak IP adresi için. SKU olarak bırakın **temel** başka bir şey için değiştirmek için özel bir nedeniniz olmadıkça ardından **Tamam** yaptığınız değişiklikleri kaydetmek için bu sayfanın sonundaki.

      ![Genel IP oluşturma](./media/vpn-gateway-add-gw-s2s-rm-portal-include/gwip.png "PIP oluşturma")

4. Ayarları doğrulayın. Seçebileceğiniz **panoya Sabitle** geçidinizin Panoda görünmesini istiyorsanız, sayfanın sonundaki. 
5. VPN ağ geçidi oluşturmaya başlamak için **Oluştur**'a tıklayın. Ayarları doğrulanır ve "dağıtma sanal ağ geçidi" görürsünüz döşeme Panoda. Bir ağ geçidi oluşturma 45 dakika kadar sürebilir. Tamamlanma durumunu görmek için portal sayfanızı yenilemeniz gerekebilir.

Ağ geçidi oluşturulduktan sonra, portalda sanal ağa bakarak bu ağ geçidine atanmış IP adresini görüntüleyin. Ağ geçidi bağlı bir cihaz gibi görüntülenir. Daha fazla bilgi görüntülemek için bağlı cihaza (sanal ağ geçidiniz) tıklayabilirsiniz.
