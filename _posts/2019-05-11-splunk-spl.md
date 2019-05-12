keyword<br>
booleans<br>
phrases<br>
field search<br>
wildcards<br>
comparisions<br>
time picker<br>
be specific<br>
가능한 한 앞쪽에 filter<br>
search terms: 대소문자 구분안함 / field: 대소문자 구분<br>

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
<br>
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
Q: Display the top 3 IP for all users on the network during the last 24 hours.
```
sourcetype=linux_secure 
| top user, src_ip limit=3 
```
Q: Display the top 3 IP on the network for each user during the last 24 hours.
```
sourcetype=linux_secure 
| top src_ip by user limit=3 
```
```
sourcetype=linux_secure 
| top user src_ip limit=3 countfield=Attempts showperc=f
```
**rare**
<br>
 Q: Which IP was least common for security events on the network during the past 60 minutes?
 ```
 sourcetype=linux_secure 
| rare showperc=f limit=1 src_ip
 ```
**stats**
<br>
count, dc, sum, avg, list, values
<br>
Q: Display invalid or failed login attempts during the last 60 minutes
```
sourcetype=linux_secure (invalid OR failed) 
| stats count 
```
Q: Count the number of events during the last 15 minutes that contain a vendor action field. Also count the total events
```
sourcetype=linux_secure 
| stats count(vendor_action) as ActionEvents, count as TotalEvents
```
Q: Display the number of vendor actions  by user and application during the last 15 minutes.
```
sourcetype=linux_secure
| stats count by user, app, vendor_action
```
Q: How many unique websites have our employees visited?
```
sourcetype=cisco_wsa_squid 
| stats dc(s_hostname) as "Websites visited:"
```
Q: How much bandwidth did employees spend at each website during the past week?
```
sourcetype=cisco_wsa_squid
| stats sum(sc_bytes) as Bandwidth by s_hostname 
| sort -Bandwidth 
```
Q: Report the number of retail units sold and sales revenue for each product during the past week. 
```
sourcetype=vendor_sales
| stats count(price) as "Units Sold", sum(price) as "Total Sales" by product_name
| sort -"Total Sales" 
```
Q: What is the average bandwidth used for each website usage type?
```
sourcetype=cisco_wsa_squid
| stats avg(sc_bytes) as "Average Bytes" by usage
```
Q: Which websites have our employees accessed during the last 60 minutes?
```
sourcetype=cisco_wsa_squid 
| stats list(s_hostname) as "Websites visited:" by cs_username
```
Q: Display the user names of failed attempts by IP in the last 60 minutes. 
```
sourcetype=linux_secure fail* 
| stats values(user) as "User Names", count(user) as Attempts by src_ip
```

**chart**
<br>
