Bir bağlantı noktasını açmak veya bir alt ağ veya VM ağ arabirimine bir ağ filtre oluşturarak Azure'da sanal makine (VM) için bir uç nokta oluşturun. Trafiği alır kaynağa bağlı bir ağ güvenlik grubu hem gelen hem de giden trafiği denetleyen bu filtreler yerleştir.

Bağlantı noktası 80 üzerinde web trafiği yaygın bir örneği kullanalım. Sunmak için yapılandırılmış bir VM olduktan sonra web isteklerinde standart TCP bağlantı noktası 80, (unutmayın uygun hizmetleri başlatmak ve tüm işletim sistemi güvenlik duvarı kurallarını VM'de de açmak için):

1. Ağ güvenlik grubu oluşturun.
2. Trafiğe izin bir gelen kuralı oluşturun:
   * "80" hedef bağlantı noktası aralığı
   * Kaynak bağlantı noktası aralığını "*" (tüm kaynak bağlantı izin verme)
   * daha az 65,500 (olması önceliği varsayılan catch tümünü daha yüksek reddetmek için gelen kuralı) öncelik değeri
3. Ağ güvenlik grubu VM ağ arabirimine veya alt ağ ile ilişkilendirin.

Ağ güvenlik gruplarını ve kurallarını kullanarak ortamınızın güvenliğini sağlamak için karmaşık ağ yapılandırmaları oluşturabilirsiniz. Bizim örneğimizde HTTP trafiği veya uzaktan yönetim izin yalnızca bir veya iki kurallarını kullanır. Daha fazla bilgi için aşağıdakilere bakın ['Daha fazla bilgi'](#more-information-on-network-security-groups) bölüm veya [bir ağ güvenlik grubu nedir?](../articles/virtual-network/virtual-networks-nsg.md)

