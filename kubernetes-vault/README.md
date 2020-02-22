$ helm status vault 
```
NAME: vault
LAST DEPLOYED: Tue Feb 18 22:33:08 2020
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
Thank you for installing HashiCorp Vault!
```


$ kubectl exec -it vault-0 -- vault operator init --key-shares=1 --key-threshold=1
```
Unseal Key 1: 7uHWDxzTmWfi6HzxIDgIL31itZRcl9JiS2aNYsDWGpg=

Initial Root Token: s.6mlDuaGO5my95jlhDtOefxo8
```

$ kubectl exec -it vault-2 -- vault status
```
Key                    Value
---                    -----
Seal Type              shamir
Initialized            true
Sealed                 false
Total Shares           1
Threshold              1
Version                1.3.1
Cluster Name           vault-cluster-005cd8a0
Cluster ID             36d5bf2a-c4ba-7285-6c47-23e126585cf1
HA Enabled             true
HA Cluster             https://10.244.0.15:8201
HA Mode                standby
Active Node Address    http://10.244.0.15:8200
```

$ kubectl exec -it vault-0 -- vault login
```
Token (will be hidden): 
Success! You are now authenticated. The token information displayed below
is already stored in the token helper. You do NOT need to run "vault login"
again. Future Vault requests will automatically use this token.

Key                  Value
---                  -----
token                s.6mlDuaGO5my95jlhDtOefxo8
token_accessor       pOtUpPm5rV3y9MyA6ydbOj5U
token_duration       ∞
token_renewable      false
token_policies       ["root"]
identity_policies    []
policies             ["root"]
```

$ kubectl exec -it vault-0 -- vault auth list
```
Path      Type     Accessor               Description
----      ----     --------               -----------
token/    token    auth_token_6a201c7e    token based credentials
```
$ kubectl exec -it vault-0 -- vault read otus/otus-ro/config
```
Key                 Value
---                 -----
refresh_interval    768h
password            asajkjkahs
username            otus
```

$ kubectl exec -it vault-0 -- vault auth list
```
Path           Type          Accessor                    Description
----           ----          --------                    -----------
kubernetes/    kubernetes    auth_kubernetes_cb27f55e    n/a
token/         token         auth_token_6a201c7e         token based credentials
```

Что бы обновлять существующие записи нужно в политику добавить update

$ kubectl exec -ti vault-agent-example -- cat /etc/secrets/index.html
```
Defaulting container name to consul-template.
Use 'kubectl describe pod/vault-agent-example -n default' to see all of the containers in this pod.
  <html>
  <body>
  <p>Some secrets:</p>
  <ul>
  <li><pre>username: otus</pre></li>
  <li><pre>password: asajkjkahs</pre></li>
  </ul>
  
  </body>
  </html>
```  



$ kubectl exec -it vault-0 -- vault write pki_int/issue/example-dot-ru common_name="gitlab.example.ru" ttl="24h"
```
Key                 Value
---                 -----
ca_chain            [-----BEGIN CERTIFICATE-----
MIIDnDCCAoSgAwIBAgIUVod1vN5IkwGsmjHTIuQn7v7hHz8wDQYJKoZIhvcNAQEL
BQAwFTETMBEGA1UEAxMKZXhtYXBsZS5ydTAeFw0yMDAyMjIxMTMwMTVaFw0yNTAy
MjAxMTMwNDVaMCwxKjAoBgNVBAMTIWV4YW1wbGUucnUgSW50ZXJtZWRpYXRlIEF1
dGhvcml0eTCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBANHD3Df43LNq
NqSgS6cCgoQ+8ODOrY87JNqaVVDC49MTjk74wovmzFjkTTll94U7shUaQJL+55S4
N6PFybimio8eRUF6Jhf2SX+xi0i1pyJOW9k0mRVgZVVcx/ddY4wArtrHoPRQhkKK
j//sONpRbIX3wOtDAvkoIiE1+CldtS74JpBB+dEvJIPCG9s+dfcn7Na+LcEaFEZ9
FPOVUsFnewW1wJF5NR3ctztDHlS6AmCjrbOKGVQa7p1aSFZb2F2JzE3fSctxYKCX
vNfrrAyglvWPGQw3jMY7qqXNT7JA8ruY5l4uhSf28KAfQuDYlwUb5pAKStBzwzN2
A8v4MDsQdVsCAwEAAaOBzDCByTAOBgNVHQ8BAf8EBAMCAQYwDwYDVR0TAQH/BAUw
AwEB/zAdBgNVHQ4EFgQUMggWwcW6YmGj1ldX0osXhub+i3swHwYDVR0jBBgwFoAU
tgnlLJ3Uu4hV6PMTeARJO4ozlcowNwYIKwYBBQUHAQEEKzApMCcGCCsGAQUFBzAC
hhtodHRwOi8vdmF1bHQ6ODIwMC92MS9wa2kvY2EwLQYDVR0fBCYwJDAioCCgHoYc
aHR0cDovL3ZhdWx0OjgyMDAvdjEvcGtpL2NybDANBgkqhkiG9w0BAQsFAAOCAQEA
J7dHv8/suo6ARsmns6ZkJlwFqRCtQ4RNEP9LHIMJbXhiTvWrdSVykLA4qfarUDaf
EnhjTX+yj1k/F90/2oZVg5J3cn6Zl1htvWWmbnfIsB1higGRDi8fKk67APosyW5N
EePwBW0UzUtE1jkML+bHUOZzksM0dTv2D8x+Hq27ws7b19Bhn13KDX5sJHl2Ft96
unrkU+ZEsKk9E+BJagm3vcy9x/WO7JFpUndAd1iTAFnSz0kgtAxPvTioDJ9RjV/+
/hQ63bTtskApkG70g5wxHcMDjIk3doTkYVc1lKnr9fTL2Mc9BYaRSSyTi6sxM+jj
KjhXcV9jvKrtY7lJ4x474Q==
-----END CERTIFICATE-----]
certificate         -----BEGIN CERTIFICATE-----
MIIDZzCCAk+gAwIBAgIUDeNY4oBpUzrn6i67P7TI2m+rNnMwDQYJKoZIhvcNAQEL
BQAwLDEqMCgGA1UEAxMhZXhhbXBsZS5ydSBJbnRlcm1lZGlhdGUgQXV0aG9yaXR5
MB4XDTIwMDIyMjExMzMwOFoXDTIwMDIyMzExMzMzOFowHDEaMBgGA1UEAxMRZ2l0
bGFiLmV4YW1wbGUucnUwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQCS
lVGklBP9/L175UbMcIFJRgPSjrtE25VdHtydVBXLZTWcA4N6QxIHS1tP5/7KDXA9
g3FCrtcEqiDGhFQFEksHjpm/SskR6Ltw4x+ER+Msy33PaRk7IJ7ZZwePHIp5p47N
7RK+cb+egdvFWSiZTkpVHzJBVxJZO46X2Zrs0SlA2xoyV3ui92IZj5uWKxCr1amh
tgQ70jDRrdmOHfNw8Ea1rMp5P6MpptMLcWsYrSjPHD8ovXl2afc8hxDIBQvebAMh
f+5YlWlWDVQvCIs1IKq466DsaFyHFHx3fyLehQdchNUkMynzWPpDvax3L6PP60+U
9U2cxl0ysnY+WEcBQkCpAgMBAAGjgZAwgY0wDgYDVR0PAQH/BAQDAgOoMB0GA1Ud
JQQWMBQGCCsGAQUFBwMBBggrBgEFBQcDAjAdBgNVHQ4EFgQURCcsX56Y3bynfZTS
PU1xuwK9Hm4wHwYDVR0jBBgwFoAUMggWwcW6YmGj1ldX0osXhub+i3swHAYDVR0R
BBUwE4IRZ2l0bGFiLmV4YW1wbGUucnUwDQYJKoZIhvcNAQELBQADggEBAAxbF5jd
/71KTlMC9p7NDn9N/gklC6H6ejdtS16sDZuLGVsPllQ+VW7XE+CkIQJEtOeKrVZG
/Sd1rrMAAcy86MAu1hnBRuubhM5P8N6v4uxtOdrsDPacilMmfu8o3o838nc4FUYv
xmStetZHEKgCygLMNoKeyO93Ng16tdGLua1ypW00iCIhgREDdn3Xlq9dhCKpMuun
4tw6t1/Eoj+gZ486l7BSat5C04n7rC9JihBNEnSJEQ4XxlWmjLEaVq+YFJNz05TK
vn+8X4JYT1cgLX5qXahbZq7xylu18C+4Xk8GTvjanaFOW5B2Auy3kKaiUA8aD6yv
of6bZXwNgAsA5+E=
-----END CERTIFICATE-----
expiration          1582457618
issuing_ca          -----BEGIN CERTIFICATE-----
MIIDnDCCAoSgAwIBAgIUVod1vN5IkwGsmjHTIuQn7v7hHz8wDQYJKoZIhvcNAQEL
BQAwFTETMBEGA1UEAxMKZXhtYXBsZS5ydTAeFw0yMDAyMjIxMTMwMTVaFw0yNTAy
MjAxMTMwNDVaMCwxKjAoBgNVBAMTIWV4YW1wbGUucnUgSW50ZXJtZWRpYXRlIEF1
dGhvcml0eTCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBANHD3Df43LNq
NqSgS6cCgoQ+8ODOrY87JNqaVVDC49MTjk74wovmzFjkTTll94U7shUaQJL+55S4
N6PFybimio8eRUF6Jhf2SX+xi0i1pyJOW9k0mRVgZVVcx/ddY4wArtrHoPRQhkKK
j//sONpRbIX3wOtDAvkoIiE1+CldtS74JpBB+dEvJIPCG9s+dfcn7Na+LcEaFEZ9
FPOVUsFnewW1wJF5NR3ctztDHlS6AmCjrbOKGVQa7p1aSFZb2F2JzE3fSctxYKCX
vNfrrAyglvWPGQw3jMY7qqXNT7JA8ruY5l4uhSf28KAfQuDYlwUb5pAKStBzwzN2
A8v4MDsQdVsCAwEAAaOBzDCByTAOBgNVHQ8BAf8EBAMCAQYwDwYDVR0TAQH/BAUw
AwEB/zAdBgNVHQ4EFgQUMggWwcW6YmGj1ldX0osXhub+i3swHwYDVR0jBBgwFoAU
tgnlLJ3Uu4hV6PMTeARJO4ozlcowNwYIKwYBBQUHAQEEKzApMCcGCCsGAQUFBzAC
hhtodHRwOi8vdmF1bHQ6ODIwMC92MS9wa2kvY2EwLQYDVR0fBCYwJDAioCCgHoYc
aHR0cDovL3ZhdWx0OjgyMDAvdjEvcGtpL2NybDANBgkqhkiG9w0BAQsFAAOCAQEA
J7dHv8/suo6ARsmns6ZkJlwFqRCtQ4RNEP9LHIMJbXhiTvWrdSVykLA4qfarUDaf
EnhjTX+yj1k/F90/2oZVg5J3cn6Zl1htvWWmbnfIsB1higGRDi8fKk67APosyW5N
EePwBW0UzUtE1jkML+bHUOZzksM0dTv2D8x+Hq27ws7b19Bhn13KDX5sJHl2Ft96
unrkU+ZEsKk9E+BJagm3vcy9x/WO7JFpUndAd1iTAFnSz0kgtAxPvTioDJ9RjV/+
/hQ63bTtskApkG70g5wxHcMDjIk3doTkYVc1lKnr9fTL2Mc9BYaRSSyTi6sxM+jj
KjhXcV9jvKrtY7lJ4x474Q==
-----END CERTIFICATE-----
private_key         -----BEGIN RSA PRIVATE KEY-----
MIIEowIBAAKCAQEAkpVRpJQT/fy9e+VGzHCBSUYD0o67RNuVXR7cnVQVy2U1nAOD
ekMSB0tbT+f+yg1wPYNxQq7XBKogxoRUBRJLB46Zv0rJEei7cOMfhEfjLMt9z2kZ
OyCe2WcHjxyKeaeOze0SvnG/noHbxVkomU5KVR8yQVcSWTuOl9ma7NEpQNsaMld7
ovdiGY+blisQq9WpobYEO9Iw0a3Zjh3zcPBGtazKeT+jKabTC3FrGK0ozxw/KL15
dmn3PIcQyAUL3mwDIX/uWJVpVg1ULwiLNSCquOug7GhchxR8d38i3oUHXITVJDMp
81j6Q72sdy+jz+tPlPVNnMZdMrJ2PlhHAUJAqQIDAQABAoIBABhEhEm9AjpQd4Zl
hP0fuyfIYaWgX7ycpfPOwjOB0kHkNaXopwG8zOVMQofOHs6Qvv5QHpYtoAdzqw1y
pv5X8vgDUczrsrI2V6Hu2C7sP94QqnmGKtkxI1tWxVeaIArYBLpu/2VsK48wbJQV
mLUY0xGYopdStQT06TyWmCGQ10WXQHCxoFELyqi8tKLf5kqmw9Rc57ZEwIk7mK3a
dGBlEYIVJt0tBCjkfv2xwV7qpT6xspPsgYuSbX5mGfIWNZpmOyYmnuUPtifIdCDW
ooP433vpVVc+d39k/oADbczLdgvMYmCWXBtORTDWivcmfae2LZMpzeKFKH9jkgrv
rZRP5YkCgYEAwYNsbXwVfvhTFsM2XQNu2fBWYL3iZ0jqb/4BEY2WtocGNZUKfeet
hfVSaL0QJRYYItbC6b1dWhtp7TfFM4f7cYu+WTNlVMGRi8FicyZW47TyRqwwiBhH
+bgJ4vKNSSwuJeWSN8F/ujpQWc+NpL1G/b+usCPdRx+fAlnkJC79dUsCgYEAwep4
FEVE/a9KQ/8QnDEhVnwsoCk4uOyQl4m3L04atber/GqOrbrzZ75hGIVT9lYuID7v
oEjJlJGTGmeFjOpVchnX7OyYDlztgqcPSxoUrdhmPC2JKoI6pvcYbva+h7P8UEcO
4UMEOkDtcgruYFMPB9s8ffFDznnaFZjQ/d9mTVsCgYBmFn1HLSTx/PNomMe/PiZm
10Hae5JLRs5XErthlT6jQIxoDB6i2WxTtV4qX0N7LTLCfmYJhZsQBFJXkQp56w0d
k8lxqYmVsyCjh/v2H43LRxRhcEmSIq0l8o9UqP0cUzBtUbVXsL8/cbAeET76X9hp
2YvA5MrB0M7EIMQYyqlwDQKBgGll7LBv2gjczsvYhgmvNoSQZ50B6r+wbQLAqp1+
oUvlsgg3Tqek9omL07CFP1akDtwd+RawmUg0O7VdURx/fcPPwioXiqo73ihmbwyN
93FqLl9FDMnbENARe+lMGdEeheSIStErINAc3DJhOKGIY6IMinuVuBow5tVYQzfJ
xgwDAoGBAKuyJt9cE+1eGLwBmViDSQoKjIX1KLx4qcur7TSEE1t4svA3HtnwGjY7
sR90xQMG35FsXAA0TJjih2YG+hgw49la71HWW+ew/U9serWMS7MrKiVYZSVN00nZ
WGLeYft/cQ8ARlv7W76AHi4hqD6/6y9OSny3cuJLTPtqlIhRdWIk
-----END RSA PRIVATE KEY-----
private_key_type    rsa
serial_number       0d:e3:58:e2:80:69:53:3a:e7:ea:2e:bb:3f:b4:c8:da:6f:ab:36:73
```


TLS настроил по этой инструкции, выпустив свой сертификат 
https://www.vaultproject.io/docs/platform/k8s/helm/examples/standalone-tls/
Из тестового пода:
```
export VAULT_ADDR=https://vault:8200
export KUBE_TOKEN=$(cat /var/run/secrets/kubernetes.io/serviceaccount/token)
curl -s --cacert /var/run/secrets/kubernetes.io/serviceaccount/ca.crt --request POST --data '{"jwt": "'$KUBE_TOKEN'", "role": "otus"}' $VAULT_ADDR/v1/auth/kubernetes/login 
{"request_id":"4390ebbe-4190-353f-3c63-7267e8fab62c","lease_id":"","renewable":false,"lease_duration":0,"data":null,"wrap_info":null,"warnings":null,"auth":{"client_token":"s.Ab0Ang87gukqcpfZN9riCFue","accessor":"RvW9nggfhUUG4b8h876UHOYh","policies":["default","otus-policy"],"token_policies":["default","otus-policy"],"metadata":{"role":"otus","service_account_name":"vault-auth","service_account_namespace":"default","service_account_secret_name":"vault-auth-token-xfsbs","service_account_uid":"e3623150-da45-4491-81b5-59b0b1ea8e7e"},"lease_duration":86400,"renewable":true,"entity_id":"afccece5-68ed-6b75-1339-ef186b4b501f","token_type":"service","orphan":true}}
```

Сделал загрузку сертификатов через аннотации:
![1.PNG](/kubernetes-vault/1.PNG)
![2.PNG](/kubernetes-vault/2.PNG)