
Bir ana bilgisayar yapılandırma dosyası içeren bir kök klasörü ve her biri aşağıdaki örnekteki gibi ayrı bir işleve kodunu içeren bir veya daha fazla alt tüm işlevleri verilen işlevi uygulama kodunu yaşadığı:

```
wwwroot
 | - host.json
 | - mynodefunction
 | | - function.json
 | | - index.js
 | | - node_modules
 | | | - ... packages ...
 | | - package.json
 | - mycsharpfunction
 | | - function.json
 | | - run.csx
```

*Host.json* dosyası bazı çalışma zamanı özel yapılandırma içeriyor ve işlev uygulaması kök klasöründe bulunur. Kullanılabilir ayarlar hakkında daha fazla bilgi için bkz: [host.json başvuru](../articles/azure-functions/functions-host-json.md).

Her işlevi bir veya daha fazla kod dosyaları, function.json yapılandırma ve başka bir bağımlılık içeren bir klasörü vardır.

