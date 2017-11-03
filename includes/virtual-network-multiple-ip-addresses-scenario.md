## <a name="scenario"></a>Senaryo
Tek bir NIC ile VM oluşturulur ve sanal bir ağa bağlı. VM üç farklı gerektirir *özel* IP adresleri ve iki *ortak* IP adresleri. IP adreslerini aşağıdaki IP yapılandırmaları atanır:

* **Ipconfig-1:** atayan bir *statik* özel IP adresi ve bir *statik* genel IP adresi.
* **Ipconfig-2:** atayan bir *statik* özel IP adresi ve bir *statik* genel IP adresi.
* **Ipconfig-3:** atayan bir *statik* özel IP adresi ve ortak IP adresi yok.
  
    ![Birden çok IP adresi](./media/virtual-network-multiple-ip-addresses-scenario/multiple-ipconfigs.png)

NIC oluşturulduğunda ve VM oluşturulduğunda NIC VM'ye bağlı olduğu IP yapılandırmaları NIC'ye ilişkilendirilir. Çizim için bu senaryo için kullanılan IP adresleri türleridir. İhtiyaç duyduğunuz hangi IP adresi ve atama türleri atayabilirsiniz.

> [!NOTE]
> Bu makaledeki adımları atar ancak tüm IP yapılandırmaları için tek bir NIC birden fazla IP yapılandırması bir multi-NIC VM herhangi bir NIC atayabilirsiniz. Bir VM ile birden çok NIC oluşturmayı öğrenmek için okuma [bir VM ile birden çok NIC oluşturma](../articles/virtual-machines/windows/multiple-nics.md) makalesi.