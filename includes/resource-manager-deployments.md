## <a name="incremental-and-complete-deployments"></a>Artımlı ve tam dağıtımları
Kaynaklarınızın dağıtırken dağıtım Artımlı güncelleştirme ya da tam güncelleştirme olduğunu belirtin. Bu iki mod arasındaki birincil fark, Resource Manager şablonunda olmayan mevcut kaynakları kaynak grubunda nasıl işlediğini şöyledir:

* Tam modunda Resource Manager **siler** kaynak grubunda var, ancak şablonda belirtilmeyen kaynakları. 
* Artımlı modunda Resource Manager **değişmeden bırakır** kaynak grubunda var, ancak şablonda belirtilmeyen kaynakları.

Her iki mod için Resource Manager şablonunda belirtilen tüm kaynakları sağlamak üzere çalışır. Kaynak kaynak grubunda zaten mevcut ve ayarlarını aynıdır, işlemi hiçbir değişikliğe neden olur. Kaynak bir kaynağın ayarlarını değiştirirseniz, bu yeni ayarlarla sağlanır. Konum veya varolan bir kaynak türü güncelleştirme denemesi dağıtımı bir hatayla başarısız olur. Bunun yerine, yeni bir kaynak konumu ile dağıtabilir veya gerektiğini bildiren yazın.

Varsayılan olarak, artımlı modu Kaynak Yöneticisi'ni kullanır.

Artımlı ve tam modları arasındaki farkı anlamak için aşağıdaki senaryoyu göz önünde bulundurun.

**Varolan bir kaynak grubu** içerir:

* Kaynak
* Kaynak B
* Kaynak C

**Şablon** tanımlar:

* Kaynak
* Kaynak B
* Kaynak D

İçinde dağıtıldığında **artımlı** modu, kaynak grubu içerir:

* Kaynak
* Kaynak B
* Kaynak C
* Kaynak D

İçinde dağıtıldığında **tam** modu, kaynak C silinir. Kaynak grubu içerir:

* Kaynak
* Kaynak B
* Kaynak D
