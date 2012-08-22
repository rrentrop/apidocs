---
layout: default
title: Alerts - Definitions
---

Overview
--------

Each of the API commands described here relate to the set of all Alert Definitions that have been created at your site. Today there are two types of Alert Definitions; those created for system alerts, (RevealCloud) and those created for website alerts (RevealUptime). 

Both types of definitions are presented and operated on in the same way by this API. Each Alert Definition is completely described by an Alert Definition hash. The key-value pairs within this hash are described in detail in the Create section.



Index
-----
List all Alert Definitions.

CURL Command:
{% highlight sh %}
curl -u APIKEY:U https://api.copperegg.com/v2/alerts/definitions.json
{% endhighlight %}

CURL Response:

Response is a JSON array of alert definition hashes. In this case, there is one alert definition, and it describes a RevealCloud 'Active Memory' alert.
{% highlight sh %}
[ 
  { 
    "id" : "50249b035d9f4f8422000006",
    "auto_clear_issues" : false,
    "notify_on_clear" : true,
    "auto_annotate" : false,
    "defn_desc" : "Himem",
    "comp_func" : ">=",
    "comp_val1" : [ "ce_rcsummary_v1","used_pcent" ],
    "comp_desc1" : "Active Memory",
    "comp_val2" : [ 0.84999999999999998 ],
    "comp_desc2" : "85%",
    "alert_duration":60,
    "type" : "ce_revealcloud"
    "match" : { "tag" : [ "myservice" ] },
    "state" : "disabled",
    "notify" : { 
      "email" : [  ],
      "webhook" : [ "http://requestb.in/11zcb5x1" ]
    },
  } 
]
{% endhighlight %}


Show
----
Show in-depth information about a single alert definition.

Required parameters: alert definition id as part of the path.

CURL Command:
{% highlight sh %}
curl -u APIKEY:U https://api.copperegg.com/v2/alerts/definitions/ALERTDEFID.json
{% endhighlight %}

CURL Response:

Response is a single JSON alert definition hash. In this case, the alert defined is a RevealCloud 'System Not Seen' alert.  
{% highlight javascript %}
{ 
  "id":"4f5e23ecbd9d2b360900048e",
  "auto_clear_issues":null,
  "notify_on_clear":null,
  "auto_annotate":null,
  "defn_desc":"Lost System",
  "comp_func":"",
  "comp_val1":["ce_health","health_state_last_updated"],
  "comp_desc1":"System Not Seen",
  "comp_val2":[0.0],
  "comp_desc2":"",
  "alert_duration":60,
  "type":"ce_revealcloud",
  "match":{},
  "state":"enabled",
  "notify":{
    "email":["myemail@gmail.com"],
    "sms":[],
    "pagerduty":[],
    "webhook":[] 
  }
}
{% endhighlight %}  

 
Create
------
Create a new alert definition.

####Required parameters:
You must include a string describing the alert (defn_desc), the type of the alert, (system or website) and all components of the Comparison. 
   
* defn_desc  
    Your short text description of the alert (string); e.g., "defn_desc":"MyAlert".  
  
* type  
    A string that may be "ce_revealcloud" (system alert), or "ce_revealuptime" (website alert); e.g., "type":"ce_revealcloud".  
  
* comparison parameters  
  Refer to 'Specifying Comparison parameters' for the following parameters:     
  * comp_func  
  * comp_desc1  
  * comp_val1  
  * comp_val2  
  * comp_desc2  
 
####Optional parameters:  

* auto_clear_issues  
    Clear the issue when the alert trigger condition is no longer true. May be true or false, not in quotes; e.g., "auto_clear_issues":true. 

* notify_on_clear   
    Send an 'ALERT CLEARED' notification when the alert is cleared. May be true or false, not in quotes; e.g., "notify_on_clear":false .  

* auto_annotate  
    Create a chart annotation when this alert triggers. May be true or false,  not in quotes; e.g., "auto_annotate":true.  

* alert_duration  
    The minimum time that the alert condition must be true (continuously) in order to trigger an alert, in seconds (number). e.g., "alert_duration":60.    

* state  
    An alert definition may be either "enabled" or "disabled". If disabled, no alerts will be generated by this alert definition. e.g., "state":"enabled".   

* match  
    If not specified, defaults to match anything. To specify one or more tags to match, use the following format: "match":{"tag":\["TAG1","TAG2"\]}.     

* notify {campfire\[\],email\[\],hipchat\[\],pagerduty\[\],sms\[\],twitter\[\],webhook\[\]}  
    An array of notification fields; when a field is empty, no notification will be generated of that type. To generate one or more notifications for this alert, fill in the desired notification field(s) as follows:  
  
  * "campfire":\["CAMPFIREAPITOKEN","CAMPFIREROOMNAME","CAMPFIRESUBDOMAIN"\]  

  * "email":\["EMAILADDR1","EMAILADDR2"\]  
 
  * "hipchat":\["HIPCHATAUTHTOKEN","HIPCHATROOMNAME","COLOR"\]  

  * "pagerduty":\["PAGERDUTYAPIKEY"\]  

  * "sms":\["SMSNUMBER1","SMSNUMBER2"\]  

  * "twitter":\["DM1","DM2"\]   

  * "webhook":\["URL"\]  


###Specifying Comparison parameters  

####RevealCloud  
{% highlight javascript %}
  Process List  
    "comp_func":"COMPFUNC"              "COMPFUNC" may be "=~" or "!~" 
    "comp_val1":["ce_usage","payload"]
    "comp_desc1":"Process list"
    "comp_val2":["YOURPROCESS"]         "YOURPROCESS" is a string containing the
                                        name of the process of interest
    "comp_desc2":"YOURPROCESS"          same string as above     

  CPU Total Usage
    "comp_func": "COMPFUNC"             "COMPFUNC" may be "<", ">", "<=", ">=",
                                        "=" or "!=" 
    "comp_val1":["ce_rcsummary_v1","active"]
    "comp_desc1":"CPU Total Usage"
    "comp_val2":[FRACTION]              FRACTION is 0.0 to 1.0 (number)
    "comp_desc2":"PERCENTAGE"           PERCENTAGE is comp_val2 expressed as a
                                        percentage, and converted to a string;
                                        "0%" to "100%".  

  CPU Steal Usage
    "comp_func": "COMPFUNC"             "COMPFUNC" may be "<", ">", "<=", ">=",
                                        "=" or "!=" 
    "comp_val1":["ce_rcsummary_v1","steal"]
    "comp_desc1":"CPU Steal Usage"
    "comp_val2":[FRACTION]              FRACTION is 0.0 to 1.0 (number)
    "comp_desc2":"PERCENTAGE"           PERCENTAGE is comp_val2 expressed as a
                                        percentage, and converted to a string;
                                        "0%" to "100%".  

  CPU IOWait Usage
    "comp_func": "COMPFUNC"             "COMPFUNC" may be "<", ">", "<=", ">=",
                                        "=" or "!=" 
    "comp_val1":["ce_rcsummary_v1","iowait"]
    "comp_desc1":"CPU IOWait Usage"
    "comp_val2":[FRACTION]              FRACTION is 0.0 to 1.0 (number)
    "comp_desc2":"PERCENTAGE"           PERCENTAGE is comp_val2 expressed as a
                                        percentage, and converted to a string;
                                        "0%" to "100%".  

  Active Memory Usage   
    "comp_func": "COMPFUNC"             "COMPFUNC" may be "<", ">", "<=", ">=",
                                        "=" or "!=" 
    "comp_val1":["ce_rcsummary_v1","used_pcent"]
    "comp_desc1":"Active Memory"
    "comp_val2":[FRACTION]              FRACTION is 0.0 to 1.0 (number)
    "comp_desc2":"PERCENTAGE"           PERCENTAGE is comp_val2 expressed as a
                                        percentage, and converted to a string;
                                        "0%" to "100%".  

  Filesystem Usage
    "comp_func": "COMPFUNC"             "COMPFUNC" may be "<", ">", "<=", ">=",
                                        "=" or "!=" 
    "comp_val1":["ce_filesystem_v1","used_pcent"]
    "comp_desc1":"Filesystem Usage"
    "comp_val2":[FRACTION]              FRACTION is 0.0 to 1.0 (number)
    "comp_desc2":"PERCENTAGE"           PERCENTAGE is comp_val2 expressed as a
                                        percentage, and converted to a string;
                                        "0%" to "100%".  

  Load
    "comp_func": "COMPFUNC"             "COMPFUNC" may be "<", ">", "<=", ">=",
                                        "=" or "!=" 
    "comp_val1":["ce_rcsummary_v1","load_shortterm"]
    "comp_desc1":"Load"
    "comp_val2":[NUMBER]                NUMBER is >= 0 (number)
    "comp_desc2":"NUMBERSTR",           NUMBERSTR is comp_val2 converted to a
                                        string; "0" and higher
                                          
  Network Bytes Sent
    "comp_func": "COMPFUNC"             "COMPFUNC" may be "<", ">", "<=", ">=",
                                        "=" or "!=" 
    "comp_val1":["ce_rcsummary_v1","if_bytes_tx_gauge"]
    "comp_desc1":"Network Bytes Sent"
    "comp_val2":[BYTESPERSEC]           BYTESPERSEC is number of bytes sent per
                                        second; will be >= 0 (number); e.g,
                                        BYTESPERSEC of 1048576 should be
                                        followed by "comp_desc2":"1 MB/S" 
                                        NOTE: 1048576 bytes/sec = 1 MB/s
    "comp_desc2":"MBPERSEC+UNITS"       "MBPERSEC+UNITS" is comp_val2
                                        expressed as MB/s, converted to str
                                        with 'MB/s' appended. (e.g., "1 MB/s")

  Network Bytes Received
    "comp_func": "COMPFUNC"             "COMPFUNC" may be "<", ">", "<=", ">=",
                                        "=" or "!=" 
    "comp_val1":["ce_rcsummary_v1","if_bytes_rx_gauge"]
    "comp_desc1":"Network Bytes Received"
    "comp_val2":[BYTESPERSEC]           BYTESPERSEC is number of bytes received
                                        per second; will be >= 0 (number); e.g,
                                        BYTESPERSEC of 1048576 should be
                                        followed by "1 MB/S" 
                                        NOTE: 1048576 bytes/sec = 1 MB/s
    "comp_desc2":"MBPERSEC+UNITS"       "MBPERSEC+UNITS" is comp_val2
                                        expressed as MB/s, converted to str
                                        with 'MB/s' appended. (e.g., "1 MB/s")    

  Health
    "comp_func": "COMPFUNC"             "COMPFUNC" may be "<", ">", "<=", ">=",
                                        "=" or "!=" 
    "comp_val1":["ce_health","health_index"]
    "comp_desc1":"Health Index"
    "comp_val2":[FRACTION]              FRACTION is 0.0 to 1.0 (number)
    "comp_desc2":"PERCENTAGE"           PERCENTAGE is comp_val2 expressed as a
                                        percentage, and converted to a string;
                                        "0%" to "100%".  

  System Not Seen
    "comp_func":""                      must be ""  
    "comp_val1":["ce_health","health_state_last_updated"]  
    "comp_desc1":"System Not Seen"  
    "comp_val2":[""]                    must be ""  
    "comp_desc2":""                     must be ""    
{% endhighlight %}  

####RevealUptime
{% highlight javascript %}
  Response Time
    "comp_func": "COMPFUNC"             "COMPFUNC" may be "<", ">", "<=", ">=",
                                        "=" or "!=" 
    "comp_val1":["ce_probe_summary_v1","time"]
    "comp_desc1":"Response Time"
    "comp_val2":["NUMBERSTR"]           NUMBERSTR is comparison value in
                                        milliseconds, in string format; "0" and
                                        above.
    "comp_desc2":"NUMBERSTR+UNITS"      NUMBERSTR+UNITS is the comp_val2 string
                                        with "ms" appended; e.g., "3000ms" 

  Response Status Code
    "comp_func": "COMPFUNC"             "=~" or "!~" 
    "comp_val1":["ce_probe_summary_v1","status_code"]
    "comp_desc1":"Status Code"
    "comp_val2":["REGEXSTR"]            "REGEXSTR" is a regex expression in
                                        string format; e.g., "^[45]", or "500"
    "comp_desc2":"REGEXSTR"             "REGEXSTR" is the same string as above 

  Uptime
    "comp_func": "COMPFUNC"             "COMPFUNC" may be "<", ">", "<=", ">=",
                                        "=" or "!=" 
    "comp_val1":["ce_probe_summary_v1","status"],
    "comp_desc1":"Uptime",
    "comp_val2":[FRACTION]              FRACTION is 0.0 to 1.0 (number)
    "comp_desc2":"PERCENTAGE"           PERCENTAGE is comp_val2 expressed as a
                                        percentage, and converted to a string;
                                        "0%" to "100%".  


  Health
    "comp_func": "COMPFUNC"             "COMPFUNC" may be "<", ">", "<=", ">=",
                                        "=" or "!=" 
    "comp_val1":["ce_probe_summary_v1","health"]
    "comp_desc1":"Health Index"
    "comp_val2":[FRACTION]              FRACTION is 0.0 to 1.0 (number)
    "comp_desc2":"PERCENTAGE"           PERCENTAGE is comp_val2 expressed as a
                                        percentage, and converted to a string;
                                        "0%" to "100%".  
{% endhighlight %}




###Create Example 1: create a new alert definition, to demonstrate defaults.

CURL Command:
{% highlight sh %}
curl -u APIKEY:U -H "Content-Type: application/json" -XPOST -d '{"defn_desc":"Himem","comp_func":">=","comp_desc1":"Active Memory","comp_val1":["ce_rcsummary_v1","used_pcent"],"comp_val2":[0.85],"comp_desc2":"85%","type":"ce_revealcloud"}' https://api.copperegg.com/v2/alerts/definitions.json | json_reformat
{% endhighlight %}

CURL Response:

Response is Status 200, empty JSON:

{% highlight javascript %}
{
}
{% endhighlight %}

The following is returned from a subsequent :index command:
{% highlight javascript %}
{ 
  "id" : "50254fec8d0dc71230000011",
  "auto_clear_issues":null,
  "notify_on_clear":null,
  "auto_annotate":null,
  "defn_desc":"Himem",
  "comp_func":">=",
  "comp_val1":["ce_rcsummary_v1","used_pcent"],
  "comp_desc1":"Active Memory",
  "comp_val2":[0.85],
  "comp_desc2":"85%",
  "alert_duration":600,
  "type":"ce_revealcloud",
  "match":{},
  "state":"enabled",
  "notify":{}
}
{% endhighlight %}


###Create Example 2: create a new alert definition, with all parameters.

CURL Command:
{% highlight sh %}
curl -u APIKEY:U -H "Content-Type: application/json" -XPOST -d '{"defn_desc":"Himem2","comp_func":">=","comp_desc1":"Active Memory","auto_clear_issues":true,"notify_on_clear":true,"auto_annotate":false,"comp_val1":["ce_rcsummary_v1","used_pcent"],"comp_val2":[0.91],"comp_desc2":"91%","alert_duration":60,"type":"ce_revealcloud","match":{"tag":["my_webtier"]},"notify":{"email":[],"webhook":["http://requestb.in/11zcb5x1"]}}' https://api.copperegg.com/v2/alerts/definitions.json | json_reformat
{% endhighlight %}  


CURL Response:  

Response is Status 200, empty JSON:  
{% highlight javascript %}
{
}
{% endhighlight %}  

The following is returned from a subsequent :index command:  
{% highlight javascript %}
{
  "id":"502be5705ead5b63ae000012",
  "auto_clear_issues":true,
  "notify_on_clear":true,
  "auto_annotate":false,
  "defn_desc":"Himem2",
  "comp_func":">=",
  "comp_val1":["ce_rcsummary_v1",
  "used_pcent"],
  "comp_desc1":"Active Memory",
  "comp_val2":[0.91],
  "comp_desc2":"91%",
  "alert_duration":60,
  "type":"ce_revealcloud",
  "match":{"tag":["my_webtier"]},
  "state":"enabled",
  "notify":{
    "email":[],
    "webhook":["http://requestb.in/11zcb5x1"]
  }
}
{% endhighlight %}  


Update  
------
Update an existing alert definition.

Required parameter:   alert definition id as part of the path.

Optional parameters:   same as described for :create.
  

Example: Change alert duration, and some flags using Update.  

CURL Command:  
{% highlight sh %}
curl -u APIKEY:U -H "Content-Type: application/json" -XPUT -d '{"auto_clear_issues":false,"auto_annotate":true,"notify_on_clear":false,"alert_duration":120}' https://api.copperegg.com/v2/alerts/definitions/ALERTDEFID.json | json_reformat
{% endhighlight %}

CURL Response:  

Response is Status 200, empty JSON:  
{% highlight javascript %}
{
}
{% endhighlight %}

Use the :show command to retrieve the updated alert definition.

CURL Command:  
{% highlight sh %}
curl -u APIKEY:U https://api.copperegg.com/v2/alerts/definitions/ALERTDEFID.json
{% endhighlight %}

CURL Response:  
{% highlight javascript %}
{ 
  "alert_duration" : 120,
  "auto_annotate" : true,
  "auto_clear_issues" : false,
  "comp_desc1" : "Active Memory",
  "comp_desc2" : "85%",
  "comp_func" : ">=",
  "comp_val1" : [ "ce_rcsummary_v1","used_pcent" ],
  "comp_val2" : [ 0.84999999999999998 ],
  "defn_desc" : "himem",
  "id" : "502557848d0dc719ac000003",
  "match" : { "tag" : [ "my_webtier" ] },
  "notify" : { 
    "email" : [  ],
    "webhook" : [ "http://requestb.in/11zcb5x1" ]
  },
  "notify_on_clear" : false,
  "state" : "enabled",
  "type" : "ce_revealcloud"
}
{% endhighlight %}


Destroy
-------
Delete the specified alert definition.

Required parameter:   alert definition id as part of the path.

CURL Command:
{% highlight sh %}
curl  -u APIKEY:U -XDELETE  https://api.copperegg.com/v2/alerts/definitions/ALERTDEFID.json
{% endhighlight %}

CURL Response:

Response is Status 200, empty JSON:

{% highlight javascript %}
{

}
{% endhighlight %}