## <a name="scenario"></a>Senaryo
Bu belgede, bir sanal makine (VM) için ayrılan bir statik genel IP adresi kullanan bir dağıtım aracılığıyla anlatılmaktadır. Bu senaryoda, kendi statik genel IP adresi ile tek bir VM var. VM adlı bir alt ağın parçası olan **ön uç** ve ayrıca statik özel IP adresi vardır (**192.168.1.101**) bu alt ağdaki.

SSL sertifikasını bir IP adresine bağlı SSL bağlantılarını zorunlu tutmak web sunucuları için bir statik IP adresi gerekir. 

![GÖRÜNTÜ AÇIKLAMASI](./media/virtual-network-deploy-static-pip-scenario-include/figure1.png)

Yukarıdaki şekilde gösterilen ortamı dağıtmak için aşağıdaki adımları izleyebilirsiniz.

