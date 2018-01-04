
Belirli bir işlev uygulamasında tüm işlevleri için kod içeren bir ana bilgisayar yapılandırma dosyası kök klasör ve bir veya daha fazla alt klasörler bulunur. Her alt aşağıdaki örnekteki gibi ayrı bir işlev için kod içerir:

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

Host.json dosyası bazı çalışma zamanı özel yapılandırmaları içerir ve işlev uygulaması kök klasöründe bulunur. Kullanılabilir ayarlar hakkında daha fazla bilgi için bkz: [host.json başvuru](../articles/azure-functions/functions-host-json.md).

Her işlevi bir veya daha fazla kod dosyaları, function.json yapılandırma ve başka bir bağımlılık içeren bir klasörü vardır.

