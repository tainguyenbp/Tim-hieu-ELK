```
filter{
    if [type] == "ovpn-server" {
        grok {
           match => { "message" => "%{HOSTPORT:ip-client} %{GREEDYDATA:status}\: %{GREEDYDATA:message}"  }
           overwrite => "message"
           # add_tag => [ "audis_base_ok" ]

         }
   }
}
```