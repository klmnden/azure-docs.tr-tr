#### Tek kayıtlı bir A kayıt kümesi oluşturma

Kayıt kümesi oluşturmak için `azure network dns record-set create` kullanın. Kaynak grubunu, bölge adını, kayıt kümesinin göreli adını, kayıt türünü ve yaşam süresini (TTL) belirtin.

    azure network dns record-set create myresourcegroup  contoso.com "test-a"  A --ttl 300

A kayıt kümesi oluşturduktan sonra `azure network dns record-set add-record` ile kayıt kümesine IPv4 adresini ekleyin.

    azure network dns record-set add-record myresourcegroup contoso.com "test-a" A -a 192.168.1.1

#### Tek kayıtlı bir AAAA kayıt kümesi oluşturma

    azure network dns record-set create myresourcegroup contoso.com "test-aaaa" AAAA --ttl 300

    azure network dns record-set add-record myresourcegroup contoso.com "test-aaaa" AAAA -b "2607:f8b0:4009:1803::1005"

#### Tek kayıtlı bir CNAME kayıt kümesi oluşturma

CNAME kayıtlarına yalnızca tek bir dize değeri için izin verilir.


    azure network dns record-set create -g myresourcegroup contoso.com  "test-cname" CNAME --ttl 300

    azure network dns record-set add-record  myresourcegroup contoso.com  test-cname CNAME -c "www.contoso.com"


#### Tek kayıtlı bir MX kayıt kümesi oluşturma

Bu örnekte, bölgenin tepesinde (bu durumda "contoso.com") MX kaydı oluşturmak için "@" kayıt kümesi adını kullanıyoruz. MX kayıtları için bu yaygındır.

    azure network dns record-set create myresourcegroup contoso.com  "@"  MX --ttl 300

    azure network dns record-set add-record -g myresourcegroup contoso.com  "@" MX -e "mail.contoso.com" -f 5


#### Tek kayıtlı bir NS kayıt kümesi oluşturma

    azure network dns record-set create myresourcegroup contoso.com test-ns  NS --ttl 300

    azure network dns record-set add-record myresourcegroup  contoso.com  "test-ns" NS -d "ns1.contoso.com"

#### Tek kayıtlı bir SRV kayıt kümesi oluşturma

Bölgenin kökünde bir SRV kaydı oluşturuyorsanız, kayıt adında "_service" and "_protocol" belirtebilirsiniz. Kayıt adına "@" eklemek gerekmez.


    azure network dns record-set create myresourcegroup contoso.com "_sip._tls" SRV --ttl 300

    azure network dns record-set add-record myresourcegroup contoso.com  "_sip._tls" SRV -p 0 - w 5 -o 8080 -u "sip.contoso.com"

#### Tek kayıtlı bir TXT kayıt kümesi oluşturma

    azure network dns record-set create myresourcegroup contoso.com "test-TXT" TXT --ttl 300

    azure network dns record-set add-record myresourcegroup contoso.com "test-txt" TXT -x "this is a TXT record"



<!--HONumber=Jun16_HO2-->


