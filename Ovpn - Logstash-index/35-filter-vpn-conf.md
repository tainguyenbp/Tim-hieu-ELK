```
filter{
   if [type] == "ovpn-server" {
        grok {
           match => { "message" => "(?:%{WORD:status}|%{GREEDYDATA:status})\: (?:%{WORD:action}\: %{IP:ip_clientnew} \-\> %{GREEDYDATA:ip_clientold}|%{GREEDYDATA:message})"  }
           overwrite => "message"
           # add_tag => [ "audis_base_ok" ]

         }
   }
}
```
