

## <a name="deployment-considerations"></a>Dağıtma konuları
* **Azure aboneliği** – daha çok sayıda işlem yoğunluklu örnekler dağıtmak için Kullandıkça Öde aboneliğine veya diğer satın alma seçenekleri göz önünde bulundurun. [Ücretsiz Azure hesabı](https://azure.microsoft.com/free/) kullanıyorsanız, yalnızca sınırlı sayıda Azure işlem çekirdeği kullanabilirsiniz.

* **Fiyatlandırma ve kullanılabilirlik** -bu VM boyutları, yalnızca standart fiyatlandırma katmanı sunulur. [Ürünler kullanılabilir bölgeye göre] denetleyin (https://azure.microsoft.com/regions/services/) Azure bölgelerindeki kullanılabilirlik. 
* **Çekirdek kota** – Azure aboneliğinizde varsayılan değerden çekirdek Kotayı artırmak gerekebilir. Aboneliğinizi H-serisi dahil olmak üzere belirli VM boyutu aileleri, dağıtabilirsiniz çekirdek sayısı da sınırlayabilir. Bir kota artışı isteği göndermek üzere [bir çevrimiçi müşteri destek isteği açma](../articles/azure-supportability/how-to-create-azure-support-request.md) herhangi bir ücret alınmaz. (Varsayılan sınırları abonelik kategoriye göre farklılık gösterebilir.)
  
  > [!NOTE]
  > Büyük ölçekli kapasite gereksinimlerini varsa Azure desteğine başvurun. Azure kotaları alacak olan sınırlar, değil kapasite garantiler. Kotanızı bağımsız olarak, kullandığınız yalnızca çekirdek için ücretlendirilirsiniz.
  > 
  > 
* **Sanal ağ** – Azure bir [sanal ağ](https://azure.microsoft.com/documentation/services/virtual-network/) işlem yoğunluklu örnekler kullanmak için gerekli değildir. Şirket içi kaynaklara erişmesi gerekirse, ancak birçok dağıtımları için en az bir bulut tabanlı Azure sanal ağı veya siteden siteye bağlantı gerekir. Gerekli olduğunda, örnekleri dağıtmak için yeni bir sanal ağ oluşturun. Bir benzeşim grubundaki sanal bir ağa işlem yoğunluklu VM'ler ekleme desteklenmiyor.
* **Yeniden boyutlandırma** – kendi özelleştirilmiş donanım nedeniyle, yalnızca işlem yoğunluklu örnekler boyutu ailesini (S-serisi veya işlem yoğunluklu A-series) içinde yeniden boyutlandırabilirsiniz. Örneğin, yalnızca H-serisi bir boyut H-serisi bir VM'den diğerine boyutlandırabilirsiniz. Buna ek olarak, yoğun olmayan sabit boyutlu bir işlem yoğunluklu boyutuna yeniden boyutlandırma desteklenmiyor.  
