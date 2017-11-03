## <a name="scenario"></a>Senaryo
Bu belge, birden çok NIC Vm'lerde belirli bir senaryoda kullanan bir dağıtım size yol gösterecek. Bu senaryoda, Azure üzerinde barındırılan bir iki katmanlı Iaas iş yükü vardır. Her katman kendi alt ağda bir sanal ağ (VNet) dağıtılır. Bir yük dengeleyici için yüksek kullanılabilirlik kümesi içinde bir arada gruplandırılmış birkaç web sunucuları, ön uç katmanından oluşur. Birkaç veritabanı sunucularının arka uç katmanından oluşur. Bu veritabanı sunucuları, iki NIC içeren her birini diğer yönetim için veritabanı erişimi için dağıtılır. Senaryo, dağıtımda ağ güvenlik her alt ağda hangi trafiğe izin denetlemek için grupları (Nsg'ler) ve NIC de içerir. Aşağıdaki şekilde, bu senaryo temel mimarisini gösterir.  

![MultiNIC senaryosu](./media/virtual-network-deploy-multinic-scenario-include/Figure1.png)

