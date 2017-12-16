## <a name="deployment-considerations"></a>Dağıtma konuları

* N-serisi VM'ler kullanılabilirlik için bkz: [bölgeye göre ürünleri](https://azure.microsoft.com/en-us/regions/services/).

* N-serisi VM'ler yalnızca Resource Manager dağıtım modelinde dağıtılabilir.

* Azure portal kullanarak bir N-serisi VM oluştururken **Temelleri** dikey penceresinde, select bir **VM disk türü** , **HDD**. Üzerinde bir kullanılabilir, N-serisi bir boyut, seçmek için **boyutu** dikey penceresinde tıklatın **tüm görüntüle**.

* NC ve NV VM'ler Azure Premium storage tarafından yedeklenen VM diskleri desteklemez.

* Birden fazla birkaç N-serisi sanal makineleri dağıtmak istiyorsanız, Kullandıkça Öde aboneliğine veya diğer satın alma seçenekleri göz önünde bulundurun. [Ücretsiz Azure hesabı](https://azure.microsoft.com/free/) kullanıyorsanız, yalnızca sınırlı sayıda Azure işlem çekirdeği kullanabilirsiniz.

* Azure aboneliğinizde (her bölge) çekirdeğini Kotayı artırmak ve NC, NCv2, ND veya NV çekirdekleri ayrı Kotayı artırmak gerekebilir. Bir kota artışı isteği göndermek üzere [bir çevrimiçi müşteri destek isteği açma](../articles/azure-supportability/how-to-create-azure-support-request.md) herhangi bir ücret alınmaz. Varsayılan sınır abonelik kategoriye göre farklılık gösterebilir.

* N-serisi sanal makineler dağıtabileceğiniz bir VM görüntü [Azure veri bilimi sanal makine](../articles/machine-learning/data-science-virtual-machine/overview.md). Veri bilimi sanal makine, önceden yükleyen ve birçok popüler veri bilimi ve araçları öğrenme derin yapılandırır. NVIDIA Tesla GPU sürücülerinin çalışmasını engeller.





