## <a name="how-apim-proxy-server-responds-with-ssl-certificates-in-the-tls-handshake"></a>APIM Proxy sunucusu TLS el sıkışma SSL sertifikalarının nasıl yanıt verir

### <a name="clients-calling-with-sni-header"></a>SNI üstbilgiyle çağırma istemcileri
Müşterinin Proxy için yapılandırılmış bir veya birden fazla özel etki alanı varsa, APIM HTTPS isteklerini özel etki alanlarındaki (örneğin, contoso.com) yanı sıra varsayılan etki alanı yanıtlayan (örneğin, hizmet name.azure api.net apim). Sunucu adı göstergesi (SNI) üstbilgisinde yer alan bilgiler bağlı olarak, uygun sunucu sertifikasıyla APIM yanıt verir.

### <a name="clients-calling-without-sni-header"></a>SNI üstbilgisi olmadan çağırma istemcileri
Müşteri göndermemek bir istemci kullanıp kullanmadığını [SNI](https://tools.ietf.org/html/rfc6066#section-3) , üst APIM aşağıdaki mantığa göre yanıtları oluşturur:

* Hizmet proxy'si için yapılandırıldığında yalnızca bir özel etki alanı varsa, varsayılan sertifika Proxy özel etki alanı verilmiş olan sertifikadır.
* Hizmet proxy'si için birden fazla özel etki alanlarını yapılandırmış varsa (yalnızca desteklenen **Premium** katmanı), müşterinin hangi sertifikanın varsayılan sertifika olmalıdır belirleyebilirsiniz. Varsayılan Sertifika ayarlamak için [defaultSslBinding](https://docs.microsoft.com/rest/api/apimanagement/apimanagementservice/createorupdate#definitions_hostnameconfiguration) özelliğinin ayarlanması true ("defaultSslBinding": "true"). Müşteri özelliği ayarlı değil, varsayılan sertifika *.azure api.net barındırılan Varsayılan Proxy etki alanı için verilen sertifikadır.

## <a name="support-for-putpost-request-with-large-payload"></a>Büyük yükü ile PUT/POST isteği için destek

APIM Proxy sunucusu, istemci tarafı sertifikalar HTTPS kullanıldığında büyük yükü istekle destekler (örneğin, yükü > 40 KB). Sunucunun isteği dondurma gelen önlemek için müşteriler özelliği ayarlayabilirsiniz ["negotiateClientCertificate": "true"](https://docs.microsoft.com/rest/api/apimanagement/ApiManagementService/CreateOrUpdate#definitions_hostnameconfiguration) Proxy ana bilgisayar üzerinde. Özellik ayarlanmışsa, true olarak, istemci sertifika tüm HTTP isteği exchange önce SSL/TLS bağlantı zaman istenir. Konumunda ayar uygulanır. bu yana **Proxy Hostname** düzeyi, tüm bağlantı istekleri için istemci sertifikası isteyin. Müşteriler, en çok 20 özel etki alanları için Proxy yapılandırabilirsiniz (yalnızca desteklenen **Premium** katmanı) ve bu sınırlamaya geçici.

