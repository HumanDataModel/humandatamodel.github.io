---
layout: home
---

<h1>What is Human Data Model?</h1>

Human Data Model (HDM) is a novel programming model for implementing the Internet of Bodies solutions. The model aims to merge Cyber, Physical, and Social Worlds into each other, providing developers an intuitive way of leveraging the data collected by services and devices while implementing novel IoT applications. This site contains documentation and implementation details on the current implementation of the Human Data Model – HDM framework.


<center>
<img class="fig" src="{{site.baseurl}}/assets/img/PCS.png" width="50%">
</center>

<h1>Why Human Data Model?</h1>

Today, the number of interconnected devices and the amount of personal information gathered by them increases tremendously, resulting in the need for development tools to harness its potential. New devices are continually being introduced in people's daily lives, and they are already producing an unprecedented amount of data related to people's well-being. However, taking advantage of such information to create innovative Internet of Bodies solutions heavily relies on manually gathering the needed information from several sources on services and the devices involved. For this reason, we have developed a novel programming model named Human Data Model. The model is a new tool to combine personal information from several sources, perform computations over that information, and proactively schedule computer-human interactions. Developers that use the proposed model would obtain an opportunity to create the Internet of Bodies solutions using high-level abstractions of the users' personal information and taking advantage of the model's distributed approach. 

<center>
<img class="fig" src="{{site.baseurl}}/assets/img/HumanDataModel.png" width="80%">
</center>


<h1>Architecture of Human Data Model</h1>

The layered architecture and components of the Human Data Model framework have been introduced on the left-hand side in the figure below. The programming interface layer (see more on API page and Examples) executes the Human Data Model programming components. Human Data Model Instance Manager (HDMIM) layer, on the other hand, is responsible for executing the programming constructs (Analysis Model component) as well as connecting to other Human Data Model instances (Connectivity Manager component) and synchronizing data between them (Sensation Synchronization component) via Connectivity Layer. 

HDMIM also fetches Seed Files from the Human Data Model hub. The seed files contain identifiers related to the person and his/her devices and services. (Read more about Seed Files from API and Example pages.) With the programming constructs, the framework merges the Virtual, Physical, and Social worlds into each other by processing the data these different worlds provide. The result – Sensations – can then be leveraged in all three worlds by various applications.

<center>
<img class="fig" src="{{site.baseurl}}/assets/img/architecture.png" width="100%">
</center>

<h1>Running Human Data Model in the Fog</h1>

<center>
<img class="fig" src="{{site.baseurl}}/assets/img/Interaction-Edge-Fog-Cloud.png" width="100%">
</center>

