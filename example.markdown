---
layout: page
title: Examples
permalink: /examples/
---

<h1>Example scenario</h1>

Imagine the following scenario: Bob is meeting his friends at university, and their phones proactively suggest they jog together since, based on their personal profiles, they are all stressed about their courses. It seems that they have lately been quite inactive in any physical activity.

The figure below presents the outcome of this example build with the Human Data Model. In the figure, the users' HDM instances run on different devices (one on a NodeJS server, another in Chrome Web Browser), collecting and processing data.

<center>
<img src="{{site.baseurl}}/assets/img/running.png" width="100%">
</center>


<h1>How can such proactive suggestions be build with Human Data Model?</h1>

First, the users must have `SeedFiles` on their Human Data Model instances. Below is Bob's `SeedFile` containing his identifiers on HDM, his phone's (companion device) identifier, his Facebook identifier, and his other personal device identifiers. The `SeedFile` can either be copy-pasted on the instance or be automatically fetched from HDM Hub.

{% highlight js %}
var bob = {
    username: "bob",
    identity: "bob@hdm",
    companionUUID: "FB694B90-F49E-4597-8306-171BBA78F844",
    facebookID: "102684690214746",
    devices: {
        "5BF2E050-4730-46DE-B6A7-2C8BE4D9FA36": "bob@iphoneSE",
        "8B034F7B-FA9B-540F-ACF3-88C0CA70C84F": "bob@ibeacon"
    }
};
{% endhighlight %}


The `SenstaionGenerator` below leverages the data injected to HDM instances. It the following preprocessed data (Sensations) into higher abstraction level Sensations: `proxemic_devices` and `facebook_friends`. The former is based on preprocessed Bluetooth Signal Strangeth Indicators (RSSI) where the device identifiers have been replaced with human-friendly names (e.g., bob@iphoneSE) and the strangth with values: Immediate, Nearby, Far, etc. This all preprocessing is done by the `Analysis Component` of the HDM framework by using the information provided in the users' `SeedFiles`.

The second argument, `facebook_friends` is also fetched by the instance. In the `SenstaionGenerator` it is used to form `social_proximity_set` sensation to indicate which (Facebook) friends and devices are nearby.

The third argument tells the HDM instance the interval in seconds for how frequently the generator can be called, and the fourth argument the `valid time value` indicates how long the generated sensation is stored in the model before it is being void.

{% highlight js %}
hdModel.addSensationGenerator(
    'social_proximity_set', ['proxemic_devices', 'facebook_friends'], 3, 5,
    function (theModel) {

        var sensation = {
            type: 'social_proximity_set',
            value: {
                friends: {}
            }
        };

        var user;
        for (user in theModel.proxemicUsers) {
            if (theModel.proxemicUsers.hasOwnProperty(user)) {
                if (Object.keys(theModel.facebookFriends).indexOf(user) !== -1) {
                    sensation.value.friends[user] = theModel.facebookFriends[user];
                }
            }
        }
        return sensation;
    }
);
{% endhighlight %}

The code below shows how the `social_proximity_set` sensation is leverged in a single page Web application (like in the figure on top of this page). In the code, it is first ensured that the sensation has not yet been consumed, and if not the graph (implemented in D3.js in the figure) is then updated. If the sensation was void, then the devices are removed from the graph (see the else-branch).


{% highlight js %}
var currentSocialGraphMD5 = '';
hdModel.on('social_proximity_set', function (event) {

    if (currentSocialGraphMD5 !== event.md5 && hdModel.proxemicDevices) {
        currentSocialGraphMD5 = event.md5;

        $('.thegraph').fadeIn();

        updateProximityGraph(
            event.sender,
            hdModel.proxemicDevices
        );

    } else {

        $('.thegraph').fadeOut(5000);
    }
});
{% endhighlight %}