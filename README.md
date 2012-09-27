jQuery.progressiveAjax
======================

Out of the need to present datas from a big ajax-response as soon as possible (as if it would be a stream), I started to build this plugin.

How to use it
======================
It's a simple jQuery-plugin, so all you need to call is $.progressiveAjax({ theParam: itsValue });.<br />
There are 2 parameters which must be given: the url which defines the url (no big surprise here)<br />
and the progFunc which defines the processing of each interval.
Speaking of the interval: you can set how often you want to fire the progFunc, along it other params:

Parameters
======================
    "url": null,
    "method": "GET", //can be GET or POST
    "progFunc": null,
    "progFuncInterval": 500, //milliseconds
    "timeout": 10000, //milliseconds
    "processOnlyNewPart": false,
    "successCallback": null,
    "errorCallback": null,
    "timeoutCallback": null

Example
======================
In this example, we're going to parse an rss-feed.<br />
Keep in mind that this is a bad example as we're just processing the new parts of the response.<br />
So we're going to loose some items, but out of simplification I won't add some logic to catch all.<br />
Secondly, you should prefer regex instead o doing everything with jQuery like here:

    var url = "http://www.computerweekly.com/rss/All-Computer-Weekly-content.xml";
    var feedItems = [];
    
    $.progressiveAjax({
        url: url,
        progFunc: function(d) {
            var newItems = $(d).find("item");
            if(newItems.length == 0) return;
    
            newItems.each(function() {feedItems.push(this);});
            console.log("added " + newItems.length + " items");
        },
        successCallback: function() {
            console.log("done!");
        },
        processOnlyNewPart: true
    });

Other things to mention
======================
If you want to stop your processing, just return false in your progFunc.<br />
That make sense if you need just the first n records and your responding server can't deal with limitation (like o-data) on it's own.<br />