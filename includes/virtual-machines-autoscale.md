Kolayca [otomatik olarak ölçeklendirme](../articles/monitoring-and-diagnostics/insights-autoscale-best-practices.md) , [sanal makineleri (VM'ler)](../articles/virtual-machines/windows/overview.md) kullandığınızda [sanal makine ölçek kümeleri](../articles/virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) ve [Azure otomatik ölçeklendirme özelliği İzleyici](../articles/monitoring-and-diagnostics/monitoring-overview-autoscale.md). Sanal makinelerinizin bir ölçek kümesinin otomatik olarak ölçeklendirilmesi üyelerinin olması gerekir. Bu makalede, dikey ve yatay otomatik ve el ile yöntemleri kullanarak sanal makinelerinizi ölçeklendirmek nasıl daha iyi anlamak sağlayan bilgiler sağlar.

## <a name="horizontal-or-vertical-scaling"></a>Yatay veya dikey ölçeklendirme

Azure İzleyici otomatik ölçeklendirme özelliği yalnızca yatay bir artış ("dışarı" olmak üzere) veya ("içinde") sanal makinelerinin sayısını azaltın ölçeklendirir. Yatay ölçeklendirme potansiyel olarak binlerce sanal makinelerin yükünü işlemek için çalıştırmanıza izin verdiğinden, bir bulut durumda daha esnektir. Yatay Ölçek kümesinin kapasitesi (veya örnek sayısı) otomatik olarak veya elle değiştirerek ölçeği. 

Dikey ölçeklendirme VM'lerin aynı sayıda tutar, ancak daha fazla ("yukarı") veya ("kapalı") güçlü Vm'leri yapar. Güç, bellek, CPU hızı veya disk alanı gibi öznitelikleri ölçülür. Dikey ölçeklendirme hızla ziyaret sayısı üst sınırını ve bölgelere göre daha büyük donanım kullanılabilirliğine bağlıdır. Dikey ölçeklendirme da genellikle sanal Makineyi durdurun ve yeniden başlatma gerektirir. Yeni bir boyut ölçek kümesindeki sanal makinelerin yapılandırma ayarlayarak dikey olarak ölçeklendirme.

Runbook'ları kullanarak [Azure Otomasyonu](../articles/automation/automation-intro.md), kolayca [bir ölçek kümesindeki Vm'leri ölçeklendirme](../articles/virtual-machine-scale-sets/virtual-machine-scale-sets-vertical-scale-reprovision.md) yukarı veya aşağı doğru.

## <a name="create-a-virtual-machine-scale-set"></a>Sanal makine ölçek kümesi oluşturma

Ölçek kümeleri, birbirinin aynısı olan Vm'leri bir küme olarak yönetmek ve dağıtmak kolay hale getirir. Linux oluşturabilir veya Windows ölçek kümelerini kullanarak [Azure portalında](../articles/virtual-machine-scale-sets/virtual-machine-scale-sets-portal-create.md), [Azure PowerShell](../articles/virtual-machines/windows/tutorial-create-vmss.md), veya [Azure CLI](../articles/virtual-machines/linux/tutorial-create-vmss.md). Ayrıca oluşturabilir ve SDK'ları ile ölçek kümeleri gibi yönetme [Python](https://azure.microsoft.com/develop/python/) veya [Node.js](/nodejs/azure), veya doğrudan [REST API'leri](/rest/api/compute/virtualmachinescalesets). Sanal makinelerin otomatik ölçeklendirme, ölçüm ve kurallarını ölçek kümesine uygulayarak elde edilir.

## <a name="configure-autoscale-for-a-scale-set"></a>Bir ölçek kümesi için otomatik ölçeklendirmeyi yapılandırma

Otomatik ölçeklendirmeyi, uygulamanızın üzerindeki yükü işlemek için VM doğru sayısını sağlar. Vm'leri yük artışlarını işlemek ve boşta oturan Vm'leri kaldırarak tasarruf etmek için eklemenize olanak sağlar. VM'lerin birtakım kurallara göre çalıştırılacak bir minimum ve maksimum sayı belirtin. Bir en düşük yapar emin olması, uygulamanızın her zaman bile hiçbir yük altında çalışıyor. En büyük değere sahip saatlik maliyeti, toplam olası sınırlar.

Kullanan ölçek kümesi oluşturduğunuzda, otomatik ölçeklendirme etkinleştirebilirsiniz [Azure PowerShell](../articles/monitoring-and-diagnostics/insights-powershell-samples.md#create-and-manage-autoscale-settings) veya [Azure CLI](https://docs.microsoft.com/cli/azure/monitor/autoscale-settings). Ölçek kümesi oluşturulduktan sonra ayrıca etkinleştirebilirsiniz. Bir ölçek kümesi oluşturma, uzantıyı yüklemek ve otomatik ölçeklendirme yapılandırma bir [Azure Resource Manager şablonu](../articles/virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md). Azure portalında Azure İzleyicisi'nden otomatik ölçeklendirmeyi etkinleştirme veya otomatik ölçeklendirme ölçek kümesi ayarlarından etkinleştirin.

![Otomatik ölçeklendirmeyi etkinleştir](./media/virtual-machines-autoscale/virtual-machines-autoscale-enable.png)
 
### <a name="metrics"></a>Ölçümler

Azure İzleyici otomatik ölçeklendirme özelliği, çalışan VM'lerin sayısını ölçekleyebilmenizi sağlar veya aşağı dayanarak [ölçümleri](../articles/monitoring-and-diagnostics/insights-autoscale-common-metrics.md). Varsayılan olarak, disk, ağ ve CPU kullanımı için sanal makineleri temel konak düzeyinde ölçümler sağlar. Tanılama uzantısını kullanarak Tanılama verileri toplamayı yapılandırırken, ek bir konuk işletim sistemi performans sayaçları disk, CPU ve bellek için kullanılabilir hale gelir.

![Ölçüm ölçütleri](./media/virtual-machines-autoscale/virtual-machines-autoscale-criteria.png)

Uygulamanızın ana bilgisayar üzerinden kullanılabilir olmayan olan ölçümler temelinde ölçeklendirmek için gereken sonra ölçek kümesindeki Vm'leri sahip olmanız gereken [Linux tanılama uzantısı](../articles/virtual-machines/linux/diagnostic-extension.md) veya [Windows Tanılama uzantısı](../articles/virtual-machines/windows/ps-extensions-diagnostics.md)yüklü. Azure portalını kullanarak bir ölçek kümesi oluşturursanız, uzantıyı gereken tanılama yapılandırması ile yüklemek için Azure PowerShell veya Azure CLI de kullanmanız gerekir.
 
### <a name="rules"></a>Kurallar

[Kuralları](../articles/monitoring-and-diagnostics/monitoring-autoscale-scale-by-custom-metric.md) bir ölçüm gerçekleştirilecek bir eylem ile birleştirin. Bir veya daha fazla otomatik ölçeklendirme eylemlerinin kural koşulları karşılandığında tetiklenir. Örneğin, ortalama CPU kullanımı yüzde 85 üzerinde olursa VM sayısını 1 artıran, tanımlı bir kuralınız olabilir.

![Otomatik ölçeklendirme eylemleri](./media/virtual-machines-autoscale/virtual-machines-autoscale-actions.png)
 
### <a name="notifications"></a>Bildirimler

Yapabilecekleriniz [Tetikleyiciler ayarlamak](../articles/monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md) e-postalarının gönderildiği tabanlı otomatik ölçeklendirme kuralları, oluşturduğunuz ya da belirli web URL'lerini çağrılır. Web kancaları, işlem sonrası veya özel bildirimleri için diğer sistemlere Azure uyarı bildirimleri yönlendirmek olanak sağlar.

## <a name="manually-scale-vms-in-a-scale-set"></a>El ile bir ölçek kümesindeki VM ölçeklendirme

### <a name="horizontal"></a>Yatay

Ekleyebilir veya VM ölçek kümesi kapasitesini değiştirerek kaldırın. Azure portalında azaltmak veya sanal makinelerin sayısını artırmak (olarak gösterilen **örnek sayısı**) ölçek geçersiz kılma koşul çubuğu ölçeklendirme ekranda sola veya sağa kaydırarak.

Azure PowerShell kullanarak ölçek kümesi nesnesini kullanarak almanız gereken [Get-AzureRmVmss](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmss). Ardından **sku.capacity** istediğiniz VM'lerin ve güncelleştirme özelliği ölçek kümesi ile [Update-AzureRmVmss](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvmss). Azure CLI'yı kullanarak ile kapasitesini değiştirme **--yeni kapasite** parametresi için [az vmss ölçek](https://docs.microsoft.com/cli/azure/vmss#az_vmss_scale) komutu.

### <a name="vertical"></a>Dikey

Vm'leri Azure portalında ölçek kümesi boyutu ekranında boyutunu el ile değiştirebilirsiniz. Get-görüntü başvurusu sku özelliğini ayarlayarak ve ardından kullanarak AzureRmVmss ile Azure PowerShell kullanabilirsiniz [Update-AzureRmVmss](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvmss) ve [Update-Azurermvmssınstance](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvmssinstance).

## <a name="next-steps"></a>Sonraki adımlar

- Ölçek kümeleri hakkında daha fazla bilgi [ölçek kümeleri için tasarım konuları](../articles/virtual-machine-scale-sets/virtual-machine-scale-sets-design-overview.md).

