---
layout: page
title: Examples
permalink: /examples/
---



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