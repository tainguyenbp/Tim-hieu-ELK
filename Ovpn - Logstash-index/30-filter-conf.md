```
filter {
    grok {
        match => { "message" => "%{TIMESTAMP_ISO8601:timesend_shipper} %{IPORHOST:logsend} %{SYSLOGTIMESTAMP:timesend_client} %{IPORHOST:logsource} %{GREEDYDATA:message}"
        }
        overwrite => "message"
    }
}
```