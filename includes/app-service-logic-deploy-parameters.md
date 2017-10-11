Azure Resource Manager sayesinde, şablon dağıtıldığında belirtmek istediğiniz değerlerin parametrelerini siz tanımlarsınız. Şablon tüm parametre değerleri içeren parametre adlı bir bölüm içerir.
Dağıttığınız projesini temel alan veya dağıttığınız ortamı dayanarak değişir bu değerleri için bir parametre tanımlamanız gerekir. Her zaman aynı kalır değerleri parametrelerini tanımlamayın. Her parametre değeri şablonda dağıtmak olan kaynakları tanımlamak için kullanılır. 

Parametreleri tanımlarken kullanın **allowedValues** hangi kullanıcı değerleri belirtmek için alanını dağıtımı sırasında sağlayabilir. Kullanım **defaultValue** dağıtımı sırasında herhangi bir değer sağlanmazsa parametresi için bir değer atamaya alan.

Biz şablondaki her bir parametreyi anlatmaktadır.

### <a name="logicappname"></a>logicAppName
Oluşturmak için mantıksal uygulama adı.

    "logicAppName": {
        "type": "string"
    }