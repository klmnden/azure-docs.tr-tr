
Bu makalede Azure sorunu ele alınmamışsa ziyaret [MSDN ve yığın taşması Azure forumları](https://azure.microsoft.com/support/forums/). Bu forumları veya çok sorununuzu nakledebilirsiniz @AzureSupport Twitter'da. Ayrıca, size bir Azure destek isteği seçerek dosya **alma desteği** üzerinde [Azure Destek](https://azure.microsoft.com/support/options/) site.

## <a name="general-troubleshooting-steps"></a>Genel sorun giderme adımları
### <a name="troubleshoot-common-allocation-failures-in-the-classic-deployment-model"></a>Klasik dağıtım modelinde ortak ayırma hatalarını giderme
Bu adımlar, sanal makinelerde birçok ayırma hatalarını gidermek yardımcı olabilir:

* Farklı bir VM boyutu VM'i yeniden boyutlandırın.<br>
    Tıklatın **tümüne Gözat** > **sanal makineleri (Klasik)** > sanal makineniz > **ayarları** > **boyutu**. Ayrıntılı adımlar için bkz: [sanal makine yeniden boyutlandırma](https://msdn.microsoft.com/library/dn168976.aspx).
* Bulut hizmetinden tüm sanal makineleri silin ve sanal makineleri yeniden oluşturun.<br>
    Tıklatın **tümüne Gözat** > **sanal makineleri (Klasik)** > sanal makineniz > **silmek**. Ardından **kaynak oluşturma** > **işlem** > [sanal makine görüntüsü].

### <a name="troubleshoot-common-allocation-failures-in-the-azure-resource-manager-deployment-model"></a>Azure Resource Manager dağıtım modelinde ortak ayırma hatalarını giderme
Bu adımlar, sanal makinelerde birçok ayırma hatalarını gidermek yardımcı olabilir:

* Durdur (deallocate) tüm sanal makineleri aynı kullanılabilirlik ayarlayın, sonra her birini yeniden başlatın.<br>
    Durdurmak için: tıklatın **kaynak grupları** > kaynak grubunuz > **kaynakları** > kullanılabilirlik kümesi > **sanal makineleri** > sanal makineniz >  **Durdur**.
  
    Tüm sanal makineleri sonra durdurmak, ilk VM seçin ve tıklatın **Başlat**.

## <a name="background-information"></a>Arka plan bilgileri
### <a name="how-allocation-works"></a>Ayırma nasıl çalışır?
Azure veri merkezlerindeki sunucular kümelere bölünmüştür. Normalde, birden fazla kümede bir ayırma isteğinde bulunulur, ancak ayırma isteğindeki belirli kısıtlamalar, Azure platformunu yalnızca bir kümede istekte bulunmaya zorlar. Bu makalede, biz buna "bir kümeye sabitlenmiş gibi." başvuruyor Aşağıda 1 diyagram birden çok kümelerde denenir normal bir ayırma durumunu gösterir. Diyagram 2 bir ayırma durumunun gösterir, varolan bir bulut hizmeti CS_1 veya kullanılabilirlik kümesini barındırıldığı olduğu için küme 2'ye sabitlenmiş.
![Ayırma diyagramı](./media/virtual-machines-common-allocation-failure/Allocation1.png)

### <a name="why-allocation-failures-happen"></a>Ayırma hatalarının neden olması
Ayırma isteği bir kümeye sabitlenmiş, kullanılabilir kaynak havuzu daha küçük olduğundan boş kaynakları bulmak başarısız olan, daha yüksek bir fırsat yok. Küme kaynakları serbest bırakmak olsa bile Ayrıca, bir kümeye ayırma isteği sabitlenmiş ancak, istenen kaynak türü, küme tarafından desteklenmiyor, isteğiniz başarısız olur. Aşağıdaki diyagram 3 yalnızca adayı küme kaynakları serbest bırakmak olmadığından burada Sabitlenmiş ayırma başarısız durumda gösterir. Diyagram 4 yalnızca adayı küme istenen VM boyutu desteklemediği için küme kaynakları serbest bırakmak sahip olsa bile burada Sabitlenmiş ayırma başarısız durumda gösterir.

![Sabitlenmiş ayırma hatası](./media/virtual-machines-common-allocation-failure/Allocation2.png)

## <a name="detailed-troubleshoot-steps-specific-allocation-failure-scenarios-in-the-classic-deployment-model"></a>Ayrıntılı adımları belirli ayırma hatası senaryoları Klasik dağıtım modelinde sorun giderme
Sabitlenmelidir ayırma isteği neden ortak ayırma senaryolar verilmiştir. Her senaryo bu makalenin sonraki bölümlerinde içine dalın.

* Bir VM'yi yeniden boyutlandırın veya sanal makineleri veya rol örnekleri olan bir bulut hizmetini ekleme
* Kısmen durduruldu (serbest bırakıldığında) sanal makineleri yeniden başlatın
* Tam olarak durduruldu (serbest bırakıldığında) sanal makineleri yeniden başlatın
* Hazırlama/üretim dağıtımları (yalnızca bir hizmet olarak platform)
* Benzeşim grubu (VM/hizmet yakınlık)
* Benzeşim grubu tabanlı sanal ağ

Ayırma hatası aldığınızda, açıklanan senaryoların herhangi biri, hata geçerli değilse bakın. Azure platformu tarafından döndürülen ayırma hatası ilgili senaryoyu tanımlamak için kullanın. İsteğiniz sabitlenmiş, daha fazla kümeler, böylece ayırma Başarı şansı artırma isteğinizi açmak için sabitleme kısıtlamaları bazılarını kaldırın.

Hata "istenen VM boyutu desteklenmiyor" bildirmediği sürece, yeterli kaynak isteğiniz uyum sağlayacak şekilde kümede serbest bırakılmış olabilir gibi genel olarak, size her zaman daha sonraki bir zamanda yeniden deneyebilir. İstenen VM boyutu desteklenmiyor sorunsa, farklı bir VM boyutu deneyin. Aksi durumda, tek seçenek sabitleme kısıtlaması kaldırmaktır.

İki ortak hatası senaryoları için benzeşim grupları ilişkilidir. Geçmişte, yakınında VM'ler/hizmet örneklerine sağlamak için kullanılan bir benzeşim grubu veya bir sanal ağ oluşturulmasını sağlamak üzere kullanıldı. Bölgesel sanal ağlar başlanmasıyla, benzeşim grupları artık bir sanal ağ oluşturmak için gerekli değildir. Azure altyapı içindeki ağ gecikme süresi azaltma ile VM/hizmet yakınlık için benzeşim grupları kullanmak için öneri değişti.

Aşağıdaki çizime 5 (sabitlenmiş) ayırma senaryoları sınıflandırma gösterir.
![Sabitlenmiş ayırma sınıflandırma](./media/virtual-machines-common-allocation-failure/Allocation3.png)

> [!NOTE]
> Her bir ayırma senaryo listelenen hata kısa bir biçimidir. Başvurmak [hata dizesi arama](#Error string lookup) ayrıntılı hata dizeleri.
> 
> 

## <a name="allocation-scenario-resize-a-vm-or-add-vms-or-role-instances-to-an-existing-cloud-service"></a>Ayırma senaryo: bir VM'yi yeniden boyutlandırın veya sanal makineleri veya rol örnekleri olan bir bulut hizmetini ekleme
**Hata**

Upgrade_VMSizeNotSupported veya GeneralError

**Küme sabitleme nedeni**

Mevcut bulut hizmetini barındıran özgün kümesine denenmesi bir VM'yi yeniden boyutlandırın veya bir VM veya rol örneği var olan bir bulut hizmetine eklemek için bir istek aldı. Yeni bir bulut hizmeti oluşturulması, istenen VM boyutu destekler veya kaynakları serbest bırakmak sahip başka bir küme bulmak Azure platformu sağlar.

**Geçici çözüm**

Hata Upgrade_VMSizeNotSupported * varsa, farklı bir VM boyutu deneyin. Farklı bir VM boyutu kullanarak bir seçenek değilse, ancak farklı bir sanal IP adresi (VIP) kullanmak için kabul edilebilir ise, yeni VM barındırmak ve var olan VM'ler çalıştırdığı bölgesel sanal ağ için yeni bulut hizmeti eklemek için yeni bir bulut hizmeti oluşturun. Mevcut bulut hizmetiniz bölgesel bir sanal ağ kullanmıyorsa, hala yeni bulut hizmeti için yeni bir sanal ağ oluşturduğunuzda ve ardından bağlanmak, [yeni bir sanal ağ mevcut sanal ağa](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/). Daha fazla gördükleri hakkında [bölgesel sanal ağlar](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/).

Hata GeneralError * varsa, (örneğin, belirli bir VM boyutu) kaynak türü küme tarafından desteklenir, ancak küme şu anda kaynakları serbest bırakmak yok olasıdır. Yukarıdaki senaryosu benzer yeni bir bulut hizmeti (yeni bulut hizmeti farklı bir VIP kullanmak olduğunu unutmayın) oluşturma aracılığıyla istenen işlem kaynak ekleyin ve bulut hizmetlerinizi bağlanmak için bir bölgesel sanal ağ kullanın.

## <a name="allocation-scenario-restart-partially-stopped-deallocated-vms"></a>Ayırma senaryo: yeniden başlatma kısmen durduruldu (serbest bırakıldığında) VM'ler
**Hata**

GeneralError *

**Küme sabitleme nedeni**

Kısmi ayırmayı kaldırma (serbest bırakıldığında) bir veya daha fazlasını ancak değil tüm VM'lerin bir bulut hizmetinde durduruldu anlamına gelir. Ne zaman durdurup (deallocate) VM, bir ilişkili kaynakları serbest bırakılır. Bu durduruldu (serbest bırakıldığında) VM'yi yeniden başlatırken bu nedenle yeni ayırma isteğidir. VM'ler kısmen deallocated bulut hizmetinde yeniden başlatma var olan bir bulut hizmetini VM'ler ekleme ile eşdeğerdir. Mevcut bulut hizmetini barındıran özgün kümesine denenmesi ayırma isteğini sahiptir. Farklı bir bulut hizmeti oluşturulması, istenen VM boyutu destekler veya ücretsiz kaynağa sahip başka bir küme bulmak Azure platformu sağlar.

**Geçici çözüm**

Farklı bir VIP kullanın, durduruldu (serbest bırakıldığında) sanal makineleri silin (ancak ilişkili diskler tutmak için) kabul edilebilir ve eklerseniz farklı bir bulut hizmeti sanal makineleri yedekleyin. Bulut hizmetlerinizi bağlanmak için bir bölgesel sanal ağ kullanın:

* Mevcut bulut hizmetiniz bölgesel bir sanal ağ kullanıyorsa, yeni bulut hizmeti aynı sanal ağa eklemeniz yeterlidir.
* Mevcut bulut hizmetiniz bölgesel bir sanal ağ kullanmıyorsa, yeni bulut hizmeti için yeni bir sanal ağ oluşturun ve ardından [yeni bir sanal ağa varolan sanal ağınıza bağlamak](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/). Daha fazla gördükleri hakkında [bölgesel sanal ağlar](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/).

## <a name="allocation-scenario-restart-fully-stopped-deallocated-vms"></a>Ayırma senaryo: yeniden başlatma tam olarak durduruldu (serbest bırakıldığında) VM'ler
**Hata**

GeneralError *

**Küme sabitleme nedeni**

Durduruldu tam ayırmayı kaldırma anlamına gelir (tüm sanal makineler bir bulut hizmetinden serbest). Bu sanal makineleri yeniden başlatmayı ayırma isteklerini bulut hizmetini barındıran özgün kümesine denenmesi gerekir. Yeni bir bulut hizmeti oluşturulması, istenen VM boyutu destekler veya kaynakları serbest bırakmak sahip başka bir küme bulmak Azure platformu sağlar.

**Geçici çözüm**

Farklı bir VIP kullanın, özgün durduruldu (serbest bırakıldığında) sanal makineleri silin (ancak ilişkili diskler tutmak için) kabul edilebilir ise ve karşılık gelen bulut hizmetini silin ((serbest bırakıldığında) durduğunda ilişkili işlem kaynaklarını zaten yayımlanan VM'ler). Sanal makineleri geri eklemek için yeni bir bulut hizmeti oluşturun.

## <a name="allocation-scenario-stagingproduction-deployments-platform-as-a-service-only"></a>Ayırma senaryo: hazırlama/üretim dağıtımları (yalnızca bir hizmet olarak platform)
**Hata**

New_General * veya New_VMSizeNotSupported *

**Küme sabitleme nedeni**

Hazırlama dağıtımı ve bir bulut hizmeti Üretim dağıtımı aynı küme içinde barındırılır. İkinci dağıtımı eklediğinizde, ilk dağıtım barındıran aynı küme içinde karşılık gelen ayırma isteğini denenir.

**Geçici çözüm**

İlk dağıtımı silin ve özgün bulut hizmeti ve bulut hizmeti yeniden dağıtın. Bu eylem, her iki dağıtım sığması için ücretsiz yeterli kaynaklara sahip bir küme veya istediğiniz VM boyutları destekleyen bir küme ilk dağıtım güden.

## <a name="allocation-scenario-affinity-group-vmservice-proximity"></a>Ayırma senaryo: benzeşim grubu (VM/hizmet yakınlık)
**Hata**

New_General * veya New_VMSizeNotSupported *

**Küme sabitleme nedeni**

Herhangi bir benzeşim grubuna atanan kaynak bir kümeye bağlanır işlem. Benzeşim grubu çalıştı varolan kaynakları barındırıldığı kümede, yeni işlem kaynağı ister. Yeni kaynaklar var olan bir bulut hizmetini veya yeni bir bulut hizmeti aracılığıyla oluşturulan bu geçerlidir.

**Geçici çözüm**

Bir benzeşim grubu gerekli değilse, olmayan bir benzeşim grubu kullanın veya işlem kaynaklarınızı birden çok benzeşim gruplar halinde gruplandırabilirsiniz.

## <a name="allocation-scenario-affinity-group-based-virtual-network"></a>Ayırma senaryo: benzeşim grubuna bağlı sanal ağ
**Hata**

New_General * veya New_VMSizeNotSupported *

**Küme sabitleme nedeni**

Bölgesel sanal ağlar sunulmadan önce bir sanal ağ bir benzeşim grubu ile ilişkilendirmek için gerekli olmuştur. Sonuç olarak, bir benzeşim grubu yerleştirilen kaynakları açıklandığı gibi aynı kısıtlamalar tarafından bağlı işlem "ayırma senaryo: benzeşim grubu (VM/hizmet yakınlık)" Yukarıdaki bölümde. İşlem kaynaklarını kümeye bağlıdır.

**Geçici çözüm**

Bir benzeşim grubu gerekmiyorsa ekleyeceğiniz, yeni kaynaklar için yeni bir bölgesel sanal ağ oluşturun ve ardından [yeni bir sanal ağa varolan sanal ağınıza bağlamak](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/). Daha fazla gördükleri hakkında [bölgesel sanal ağlar](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/).

Alternatif olarak, [benzeşim grubu tabanlı sanal ağınızı bölgesel bir sanal ağa geçirmeniz](https://azure.microsoft.com/blog/2014/11/26/migrating-existing-services-to-regional-scope/)ve ardından istenen kaynakları yeniden ekleyin.

## <a name="detailed-troubleshooting-steps-specific-allocation-failure-scenarios-in-the-azure-resource-manager-deployment-model"></a>Ayrıntılı adımları belirli ayırma hatası senaryoları Azure Resource Manager dağıtım modelinde sorun giderme
Sabitlenmelidir ayırma isteği neden ortak ayırma senaryolar verilmiştir. Her senaryo bu makalenin sonraki bölümlerinde içine dalın.

* Bir VM'yi yeniden boyutlandırın veya sanal makineleri veya rol örnekleri olan bir bulut hizmetini ekleme
* Kısmen durduruldu (serbest bırakıldığında) sanal makineleri yeniden başlatın
* Tam olarak durduruldu (serbest bırakıldığında) sanal makineleri yeniden başlatın

Ayırma hatası aldığınızda, açıklanan senaryoların herhangi biri, hata geçerli değilse bakın. Azure platformu tarafından döndürülen ayırma hatası ilgili senaryoyu tanımlamak için kullanın. İsteğiniz varolan bir kümeye sabitlenmiş, daha fazla kümeler, böylece ayırma Başarı şansı artırma isteğinizi açmak için sabitleme kısıtlamaları bazılarını kaldırın.

Hata "istenen VM boyutu desteklenmiyor" bildirmediği sürece, yeterli kaynak isteğiniz uyum sağlayacak şekilde kümede serbest bırakılmış olabilir gibi genel olarak, size her zaman daha sonraki bir zamanda yeniden deneyebilir. Aşağıda sorun istenen VM boyutu desteklenmiyor ise, geçici çözümler için bkz.

## <a name="allocation-scenario-resize-a-vm-or-add-vms-to-an-existing-availability-set"></a>Ayırma senaryo: bir VM'yi yeniden boyutlandırın veya VM'ler var olan bir kullanılabilirlik kümesine ekleme
**Hata**

Upgrade_VMSizeNotSupported * veya GeneralError *

**Küme sabitleme nedeni**

Varolan bir kullanılabilirlik kümesini barındıran özgün kümesine denenmesi bir VM'yi yeniden boyutlandırın veya VM var olan bir kullanılabilirlik kümesine eklemek için bir istek aldı. Yeni bir kullanılabilirlik kümesi oluşturmak istediğiniz VM boyutu destekler veya kaynakları serbest bırakmak sahip başka bir küme bulmak Azure platformu sağlar.

**Geçici çözüm**

Hata Upgrade_VMSizeNotSupported * varsa, farklı bir VM boyutu deneyin. Farklı bir VM boyutu kullanarak bir seçenek değilse, tüm sanal makineleri kullanılabilirlik kümesinde durdurun. İstenen VM boyutu destekleyen bir kümeye VM ayırır sanal makine boyutunu daha sonra değiştirebilirsiniz.

Hata GeneralError * varsa, (örneğin, belirli bir VM boyutu) kaynak türü küme tarafından desteklenir, ancak küme şu anda kaynakları serbest bırakmak yok olasıdır. VM farklı bir kullanılabilirlik kümesinin bir parçası olabilir, farklı bir kullanılabilirlik (aynı bölgede) kümesinde yeni bir VM oluşturun. Bu yeni VM sonra aynı sanal ağa eklenebilir.  

## <a name="allocation-scenario-restart-partially-stopped-deallocated-vms"></a>Ayırma senaryo: yeniden başlatma kısmen durduruldu (serbest bırakıldığında) VM'ler
**Hata**

GeneralError *

**Küme sabitleme nedeni**

Kısmi ayırmayı kaldırma (serbest bırakıldığında) bir veya daha fazla durduruldu ancak tümü, sanal makineleri bir kullanılabilirlik kümesi anlamına gelir. Ne zaman durdurup (deallocate) VM, bir ilişkili kaynakları serbest bırakılır. Bu durduruldu (serbest bırakıldığında) VM'yi yeniden başlatırken bu nedenle yeni ayırma isteğidir. Kısmen deallocated kullanılabilirlik kümesindeki sanal makineleri yeniden başlatmayı VM'ler var olan bir kullanılabilirlik kümesine ekleme ile eşdeğerdir. Varolan bir kullanılabilirlik kümesini barındıran özgün kümesine denenmesi ayırma isteğini sahiptir.

**Geçici çözüm**

Kullanılabilirlik kümesindeki ilk yeniden başlatmadan önce tüm sanal makineleri durdurun. Bu yeni bir ayırma girişimi çalıştırılır ve yeni bir küme kullanılabilir kapasiteye sahip seçilebilir emin olun.

## <a name="allocation-scenario-restart-fully-stopped-deallocated"></a>Ayırma senaryo: yeniden başlatma tam olarak durduruldu (serbest bırakıldığında)
**Hata**

GeneralError *

**Küme sabitleme nedeni**

Durduruldu tam ayırmayı kaldırma anlamına gelir (tüm sanal makineleri bir kullanılabilirlik kümesinde serbest). Bu sanal makineleri yeniden başlatmayı ayırma isteği istenen boyut destekleyen tüm kümeleri hedefleyecektir.

**Geçici çözüm**

Ayırmak için yeni bir VM boyutunu seçin. Bu işe yaramazsa, lütfen daha sonra yeniden deneyin.

<a name="Error string lookup"></a>

## <a name="error-string-lookup"></a>Hata dizesi arama
**New_VMSizeNotSupported***

"VM boyutu (veya VM boyutları birleşimi) bu dağıtımın gerektirdiği dağıtım isteği kısıtlamaları nedeniyle sağlanamıyor. Mümkünse, başka bir dağıtım içindeki ve farklı bir benzeşim grubuna ya da hiçbir benzeşim grubuna barındırılan bir hizmete sanal ağ bağlamaları gibi kısıtlamaları gevşetme deneyin veya farklı bir bölgeye dağıtmayı deneyin."

**New_General***

"Ayırma başarısız oldu; İstekteki kısıtlamalar karşılamak kurulamıyor. İstenen yeni hizmet dağıtımı bir benzeşim grubuna bağlı veya bir sanal ağ hedefler ya da bu barındırılan hizmet altında varolan bir dağıtım yok. Bu koşulların herhangi biri yeni dağıtımı belirli Azure kaynaklarına kısıtlar. Lütfen daha sonra yeniden deneyin veya VM boyutunu veya rol örneklerinin sayısını azaltmayı deneyin. Alternatif olarak, mümkünse, daha önce bahsedilen kısıtlamaları kaldırın veya farklı bir bölgeye dağıtmayı deneyin."

**Upgrade_VMSizeNotSupported***

"Dağıtım yükseltme yapılamıyor. İstenen VM boyutu XXX mevcut dağıtımını destekleyen kaynaklarda kullanılamayabilir. Lütfen daha sonra yeniden deneyin, farklı bir VM boyutu veya daha az sayıda rol örneği ile deneyin veya yeni bir benzeşim grubu ya da benzeşim grubu bağlaması olmadan boş bir barındırılan hizmet altında dağıtım oluşturun."

**GeneralError***

"Sunucu bir iç hatayla karşılaştı. Lütfen isteği yeniden deneyin." Veya "hizmeti için bir ayırma üretmek başarısız oldu."

