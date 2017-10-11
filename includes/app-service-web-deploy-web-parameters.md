Azure Resource Manager sayesinde, şablon dağıtıldığında belirtmek istediğiniz değerlerin parametrelerini siz tanımlarsınız. Şablon tüm parametre değerleri içeren parametre adlı bir bölüm içerir.
Dağıttığınız projesini temel alan veya dağıttığınız ortamı dayanarak değişir bu değerleri için bir parametre tanımlamanız gerekir. Her zaman aynı kalır değerleri parametrelerini tanımlamayın. Her parametre değeri, dağıtılan kaynakları tanımlamak için şablonda kullanılır. 

Parametreleri tanımlarken kullanın **allowedValues** hangi kullanıcı değerleri belirtmek için alanını dağıtımı sırasında sağlayabilir. Kullanım **defaultValue** dağıtımı sırasında herhangi bir değer sağlanmazsa parametresi için bir değer atamaya alan.

Biz şablondaki her bir parametreyi anlatmaktadır.

### <a name="sitename"></a>SiteName
Oluşturmak istediğiniz web uygulamasının adı.

    "siteName":{
      "type":"string"
    }

### <a name="hostingplanname"></a>hostingPlanName
Web uygulamasını barındırmak için kullanılacak uygulama hizmeti planının adı.

    "hostingPlanName":{
      "type":"string"
    }

### <a name="sku"></a>SKU
Barındırma planı için fiyatlandırma katmanı.

    "sku": {
      "type": "string",
      "allowedValues": [
        "F1",
        "D1",
        "B1",
        "B2",
        "B3",
        "S1",
        "S2",
        "S3",
        "P1",
        "P2",
        "P3",
        "P4"
      ],
      "defaultValue": "S1",
      "metadata": {
        "description": "The pricing tier for the hosting plan."
      }
    }

Şablon bu parametre için izin verilen değerleri tanımlar ve herhangi bir değer belirtilmezse varsayılan değer (S1) atar.

### <a name="workersize"></a>workerSize
Barındırma planı (küçük, Orta veya büyük) örnek boyutunu.

    "workerSize":{
      "type":"string",
      "allowedValues":[
        "0",
        "1",
        "2"
      ],
      "defaultValue":"0"
    }

Şablon (0, 1 veya 2) Bu parametre için izin verilen değerleri tanımlar ve herhangi bir değer belirtilmezse varsayılan değer (0) atar. Değerler için küçük, Orta ve büyük karşılık gelir.

