keyword
booleans
phrases
field search
wildcards
comparisions
time picker
be specific
가능한 한 앞쪽에 filter
search terms: 대소문자 구분안함 / field: 대소문자 구분

**table**
```
 sourcetype = access_combined 
| table clientip, action, productId, status
```

**rename**
```
sourcetype = access_combined 
| table clientip, action, productId, status
| rename productId as ProductID, action as "Customer Action", status as "HTTP Status"
```
**fields**
<br>
**dedub, sort**
```
sourcetype=vendor_sales
| dedup Vendor
| sort – VendorCountry, +VendorStateProvince, VendorCity, Vendor
| table VendorCountry, VendorStateProvince, VendorCity, Vendor
```
**top**
Q: During the last 60 minutes, which IPs generated the most attacks?
```
sourcetype=linux_secure
| top src_ip
```
Q: During the last hour, display the top 5 IPs that generated the most attacks.
```
sourcetype=linux_secure (fail* OR invalid) 
| top limit=5 src_ip 
```
<br>
```
sourcetype=linux_secure 
| top user, src_ip limit=3 
```
<br>
```
sourcetype=linux_secure 
| top user src_ip limit=3 countfield=Attempts showperc=f
```
