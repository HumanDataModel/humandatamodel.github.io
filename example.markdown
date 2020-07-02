---
layout: page
title: Examples
permalink: /examples/
---

<h1>Example scenario</h1>

Imagine the following scenario: Bob is meeting his friends at university, and their phones proactively suggest they jog together since, based on their personal profiles, they are all stressed about their courses. It seems that they have lately been quite inactive in any physical activity.

The figure below presents the outcome of this example build with the Human Data Model. In the figure, the users' HDM instances run on different devices, collecting and processing data.

<center>
<img src="{{site.baseurl}}/assets/img/running.png" width="100%">
</center>


<h1>How can such proactive suggestions be build with Human Data Model?</h1>

First, the users must have SeedFiles on their Human Data Model instances. Below is Bob's SeedFile containing his identifiers on HDM, his phone's (companion device) identifier, his Facebook identifier, and his other personal device identifiers. The SeedFile can either be copy-pasted on the instance or be automatically fetched from HDM Hub.

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


The 

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