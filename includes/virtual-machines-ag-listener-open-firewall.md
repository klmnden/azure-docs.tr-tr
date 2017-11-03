Bu adımda, yük dengeli uç nokta için (daha önce belirtildiği gibi 59999) araştırma bağlantı noktasını açmak için bir güvenlik duvarı kuralı oluşturun ve kullanılabilirlik grubu dinleyicisinin bağlantı noktası açmak için başka bir kural. Kullanılabilirlik grubu çoğaltmalarının içeren sanal makinelerin yük dengeli uç nokta oluşturduğundan sonda bağlantı noktası ve ilgili VM'ler dinleyicisi bağlantı noktası açmanız gerekir.

1. Çoğaltmaları barındıran Vm'lerinde Başlat **Gelişmiş Güvenlik Özellikli Windows Güvenlik Duvarı**.

2. Sağ **gelen kuralları**ve ardından **yeni kural**.

3. Üzerinde **kural türü** sayfasında, **bağlantı noktası**ve ardından **sonraki**.

4. Üzerinde **protokol ve bağlantı noktaları** sayfasında, **TCP**, türü **59999** içinde **belirli yerel bağlantı noktaları** kutusuna ve ardından  **Sonraki**.

5. Üzerinde **eylem** sayfasında, tutmak **bağlantıya izin** seçili ve ardından **sonraki**.

6. Üzerinde **profil** sayfasında, varsayılan ayarları kabul edin ve ardından **sonraki**.

7. Üzerinde **adı** sayfasında **adı** metin kutusunda, bir kural adı gibi belirtin **her zaman dinleyicisi yoklama bağlantı noktası**ve ardından **son**.

8. Kullanılabilirlik grubu dinleyicisinin bağlantı noktası (daha önce komut dosyasının $EndpointPort parametresinde belirtildiği şekilde) için önceki adımları yineleyin ve uygun kural adı gibi belirtin **her zaman dinleyicisi bağlantı noktası**.

