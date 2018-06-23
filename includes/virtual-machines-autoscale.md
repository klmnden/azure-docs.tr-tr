Kolayca [otomatik olarak ölçeklendirme](../articles/monitoring-and-diagnostics/insights-autoscale-best-practices.md) , [sanal makineleri (VM'ler)](../articles/virtual-machines/windows/overview.md) kullandığınızda [sanal makine ölçek kümeleri](../articles/virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) ve [Azure otomatik ölçeklendirmeyi özelliği İzleyici](../articles/monitoring-and-diagnostics/monitoring-overview-autoscale.md). Vm'leriniz ölçeği otomatik olarak ölçeklendirilmesi Ayarla üyesi olması gerekir. Bu makalede, dikey ve yatay olarak otomatik ve el ile yöntemlerini kullanarak, Vm'lerde ölçeklendirmek nasıl daha iyi anlamak sağlayan bilgiler sağlar.

## <a name="horizontal-or-vertical-scaling"></a>Yatay veya dikey ölçekleme

Azure İzleyici'nin otomatik ölçeklendirme özelliği yalnızca yatay artışı ("out") veya ("içinde") VM'lerin sayısını azaltın ölçeklendirir. Yatay ölçekleme yükü işlemek üzere VM'ler binlerce potansiyel olarak çalıştırmanıza izin veren gibi bir bulut durumda daha esnektir. Yatay Ölçek kümesi kapasitesi (veya örnek sayısı) otomatik olarak veya el ile değiştirerek ölçeklendirin. 

Dikey ölçekleme VM'ler ile aynı sayıda tutar ancak sanal makineleri ("yukarı") daha fazla veya az ("kapalı") güçlü yapar. Güç bellek, CPU hızı veya disk alanı gibi öznitelikleri ölçülür. Dikey ölçeklendirme hızla bir üst sınır İsabetli ve bölgeye göre farklılık gösterebilir büyük donanım kullanılabilirliği bağımlıdır. Dikey ölçeklendirme da genellikle durdurup yeniden başlatmak bir VM gerektirir. Ölçek kümesindeki sanal makineleri yapılandırmasındaki yeni bir boyutu ayarlayarak dikey olarak ölçeklendirin.

Runbook'ları kullanarak [Azure Otomasyonu](../articles/automation/automation-intro.md), kolayca [ölçek kümesindeki sanal makineleri ölçeklendirme](../articles/virtual-machine-scale-sets/virtual-machine-scale-sets-vertical-scale-reprovision.md) yukarı veya aşağı.

## <a name="create-a-virtual-machine-scale-set"></a>Sanal makine ölçek kümesi oluşturma

Ölçek kümeleri, dağıtmak ve bir küme olarak aynı sanal makineleri yönetmek kolay hale getirir. Linux oluşturabilir veya Windows ölçek ayarlar kullanarak [Azure portal](../articles/virtual-machine-scale-sets/virtual-machine-scale-sets-portal-create.md), [Azure PowerShell](../articles/virtual-machines/windows/tutorial-create-vmss.md), veya [Azure CLI](../articles/virtual-machines/linux/tutorial-create-vmss.md). Ayrıca oluşturmak ve SDK'ları ile ölçek kümeleri gibi yönetmek [Python](/develop/python) veya [Node.js](/nodejs/azure), veya doğrudan [REST API'leri](/rest/api/compute/virtualmachinescalesets). Otomatik ölçeklendirme Vm'leri ölçümleri ve kuralları ölçek kümesine uygulayarak gerçekleştirilir.

## <a name="configure-autoscale-for-a-scale-set"></a>Otomatik ölçeklendirme ölçek kümesi için yapılandırın

Otomatik ölçeklendirme, uygulamanızın üzerinde yükü işlemek üzere VM sağ sayısını sağlar. Yük arttıkça işlemek ve boşta durduğunu VM'ler kaldırarak paradan tasarruf VM'ler eklemenize olanak tanır. Bir dizi kurala dayalı olarak çalışan sanal makineleri minimum ve maksimum sayısı belirtin. Bir minimum yapar emin olması, uygulamanızın her zaman bile herhangi bir yük altında çalışıyor. En büyük değere sahip saatlik maliyet, toplam olası sınırlar.

Kullanılarak ayarlanan ölçek oluşturduğunuzda, otomatik ölçeklendirme etkinleştirebilirsiniz [Azure PowerShell](../articles/monitoring-and-diagnostics/insights-powershell-samples.md#create-and-manage-autoscale-settings) veya [Azure CLI](https://docs.microsoft.com/cli/azure/monitor/autoscale-settings). Ölçek kümesi oluşturulduktan sonra aynı zamanda etkinleştirebilirsiniz. Bir ölçek kümesi oluşturma, uzantıyı yüklemek ve otomatik ölçeklendirme kullanarak yapılandırma bir [Azure Resource Manager şablonu](../articles/virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md). Azure portalında Azure İzleyicisi'nden otomatik ölçeklendirme etkinleştirebilir ya da otomatik ölçeklendirme ölçek kümesi ayarlarından etkinleştirebilirsiniz.

![Otomatik ölçeklendirmeyi etkinleştir](./media/virtual-machines-autoscale/virtual-machines-autoscale-enable.png)
 
### <a name="metrics"></a>Ölçümler

Azure İzleyici'nin otomatik ölçeklendirme özelliği VM'ler çalışan sayısını ölçeklendirmenizi sağlar veya aşağı dayanarak [ölçümleri](../articles/monitoring-and-diagnostics/insights-autoscale-common-metrics.md). Varsayılan olarak, disk, ağ ve CPU kullanımı için sanal makineleri temel ana bilgisayar düzeyinde ölçümleri sağlar. Tanılama uzantısını kullanarak Tanılama verileri koleksiyonu yapılandırırken, ek konuk işletim sistemi performans sayaçları disk, CPU ve bellek için kullanılabilir hale gelir.

![Ölçüm ölçütleri](./media/virtual-machines-autoscale/virtual-machines-autoscale-criteria.png)

Ana bilgisayar üzerinden kullanılabilir değil temel alınarak ölçümleri ölçeklendirmek uygulamanız gereken sonra ölçek kümesindeki sanal makineleri ya da gerek [Linux tanılama uzantısını](../articles/virtual-machines/linux/diagnostic-extension.md) veya [Windows Tanılama uzantısını](../articles/virtual-machines/windows/ps-extensions-diagnostics.md)yüklü. Azure portalı kullanılarak ayarlanan bir ölçek oluşturursanız, Azure PowerShell veya Azure CLI ihtiyacınız tanılama yapılandırması ile uzantıyı yüklemek için de gerekir.
 
### <a name="rules"></a>Kurallar

[Kuralları](../articles/monitoring-and-diagnostics/monitoring-autoscale-scale-by-custom-metric.md) ölçüm gerçekleştirilecek bir eylem ile birleştirin. Bir veya daha fazla otomatik ölçeklendirme eylemi Kural koşulu karşılandığında tetiklenir. Örneğin, ortalama CPU kullanımı yüzde 85 ' kalırsa VM'lerin sayısını 1 arttırır tanımlanan bir kural olabilir.

![Otomatik ölçeklendirme eylemleri](./media/virtual-machines-autoscale/virtual-machines-autoscale-actions.png)
 
### <a name="notifications"></a>Bildirimler

Yapabilecekleriniz [Tetikleyicileri ayarlamak](../articles/monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md) belirli web URL'leri çağrılır veya e-postalarının gönderildiği tabanlı oluşturduğunuz otomatik ölçeklendirme kurallarını. Web kancası, diğer sistemlere işlem sonrası veya özel bildirimleri için Azure uyarı bildirimleri yönlendirmek izin verir.

## <a name="manually-scale-vms-in-a-scale-set"></a>VM ölçek kümesindeki elle ölçeklendirme

### <a name="horizontal"></a>Yatay

Ekleme veya VM ölçek kümesi kapasitesi değiştirerek kaldırın. Azure portalında azaltın veya VM sayısını artırın (olarak gösterilen **örnek sayısını**) ölçeklendirme ekranda sola veya sağa geçersiz kılma durum çubuğu kaydırarak ayarlamak ölçeğinde.

Azure PowerShell kullanarak, Ölçek kümesi nesnesi kullanarak almanız gereken [Get-AzureRmVmss](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmss). Ardından **sku.capacity** istediğiniz sanal makineleri ve güncelleştirme sayısını özelliğine ölçek kümesi ile [güncelleştirme AzureRmVmss](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvmss). Azure CLI kullanarak, kapasiteyle değiştirme **--yeni kapasite** parametresi için [az vmss ölçek](https://docs.microsoft.com/cli/azure/vmss#az_vmss_scale) komutu.

### <a name="vertical"></a>Dikey

VM ölçek kümesi için boyutu ekranında Azure portalında boyutunu el ile değiştirebilirsiniz. Get-resim başvurusu sku özelliği ayarlama ve ardından kullanarak AzureRmVmss ile Azure PowerShell'i kullanabilirsiniz [güncelleştirme AzureRmVmss](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvmss) ve [güncelleştirme AzureRmVmssInstance](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvmssinstance).

## <a name="next-steps"></a>Sonraki adımlar

- Ölçek kümeleri hakkında daha fazla bilgi [ölçek ayarlar için tasarım değerlendirmeleri](../articles/virtual-machine-scale-sets/virtual-machine-scale-sets-design-overview.md).

