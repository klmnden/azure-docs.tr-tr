Azure Storage’daki her blob bir kapsayıcıda yer almalıdır. Kapsayıcı, blob adının bir bölümünü oluşturur. Örneğin, bu örnek blob URI’lerinde `mycontainer` kapsayıcının adıdır:

    https://storagesample.blob.core.windows.net/mycontainer/blob1.txt
    https://storagesample.blob.core.windows.net/mycontainer/photos/myphoto.jpg

Kapsayıcı adı, aşağıdaki adlandırma kurallarına uygun geçerli bir DNS adı olmalıdır:

1. Kapsayıcı adları bir harf veya rakamla başlamalıdır; yalnızca harf, rakam ve tire (-) karakterinden oluşabilir.
2. Her tire (-) karakterinin hemen önünde ve arkasında bir harf veya rakam bulunmalıdır; kapsayıcı adlarında art arda tirelere izin verilmez.
3. Kapsayıcı adındaki tüm harfler küçük harf olmalıdır.
4. Kapsayıcı adları 3-63 karakter arası uzunlukta olmalıdır.

> [!IMPORTANT]
> Kapsayıcı adının her zaman küçük harfli olması gerektiğini unutmayın. Kapsayıcı adına bir büyük harf katarsanız ya da başka bir deyişle kapsayıcı adlandırma kurallarını ihlal ederseniz 400 hatası alabilirsiniz (Hatalı İstek). 
> 
> 

<!--HONumber=Sep16_HO3-->


