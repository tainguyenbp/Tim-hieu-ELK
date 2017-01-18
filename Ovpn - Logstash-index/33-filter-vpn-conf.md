```
filter{
   if [type] == "ovpn-server" {
        grok {
           match => { "message" => "%{IPORHOST:server}\[%{INT:srv_port}]+[: ] %{GREEDYDATA:message}"  }
           overwrite => "message"
           # add_tag => [ "audis_base_ok" ]

         }
   }
}
```