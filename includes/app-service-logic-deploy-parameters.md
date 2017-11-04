Azure Resource Manager ile şablonu dağıtırken kullanılacak değerler için parametreleri tanımlayabilirsiniz. Şablon içeren bir `parameters` tüm parametre değerleri içeren bölümü. Her parametre değeri, dağıtmak istediğiniz kaynakları tanımlamak için şablon tarafından kullanılır.

> [!NOTE]
> Her zaman aynı kalan değerler için parametre tanımlamayın. Dağıttığınız projesini temel alan veya dağıttığınız ortamına bağlı yalnızca farklılık, değerleri parametrelerini tanımlayın.

Ne zaman parametrelerini tanımlayın:

* Bir kullanıcı dağıtım sırasında sağlayabileceğiniz izin verilen değerler belirtmek için kullanın **allowedValues** alan.

* Dağıtım sırasında hiçbir değer sağlandığında parametresi için varsayılan değerleri atamak için kullandığınız **defaultValue** alan. 
