## <a name="scenario"></a>Senaryo
Udr'ler oluşturmak nasıl daha iyi anlamak için bu belge aşağıdaki senaryoyu kullanır.

![GÖRÜNTÜ AÇIKLAMASI](./media/virtual-network-create-udr-scenario-include/figure1.png)

Bu senaryoda bir UDR için oluşturacağınız *ön uç alt* ve için başka bir UDR *arka uç alt* , aşağıda açıklandığı gibi: 

* **Ön uç UDR**. UDR uygulanacak ön uç *ön uç* alt ağı ve bir rota içerir:    
  * **RouteToBackend**. Bu rota tüm trafik arka uç alt ağına göndereceği **FW1** sanal makine.
* **Arka uç UDR**. UDR uygulanacak arka uç *arka uç* alt ağı ve bir rota içerir:    
  * **RouteToFrontend**. Bu rota tüm trafik için ön uç alt göndereceği **FW1** sanal makine.

Bu yollar bileşimi için bir alt ağdan diğerine giden tüm trafiği yönlendirilmesini sağlamak **FW1** sanal gereç olarak kullanılan sanal makine. Ayrıca diğer VM'ler için hedefleyen trafik alabilir emin olmak için bu VM için IP iletimini etkinleştirmek gerekir.

