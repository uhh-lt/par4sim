---
layout: post
title: Installation
permalink: /install/
icon: cog fa-spin
---

Demo
===
You can access the demo, which is based on the dataset we have collected during the experiment [here](https://ltmaggie.informatik.uni-hamburg.de/par4sim/).
To simulate that you are accessing it from MTurk interface for different HITs (so that you can see different HITs), click **Get new text**


MTurk specific installation (for the **COLING 2018 Par4Sim** paper)
===


To setup the tool in [MtUrk](https://www.mturk.com/) follow the following instruction. You have to first install [Docker](https://docs.docker.com/install/) and [docker-compose](https://docs.docker.com/compose/install/).

1. Download the docker-compose file from [here](https://github.com/uhh-lt/par4sem/blob/par4sim/docker-compose.yml).
1. Download the database dump from [here](http://ltdata1.informatik.uni-hamburg.de/par4sem/database/alldbs.tar.gz), unzip and put them under dumps folder
1. Start the docker as ``docker-compose up -d``

Now, the tool is ready to be integrated to the MTurk website.

Accessing the databases:

1. Login to the running mariadb in the docker.
   1. Get the container id: ``docker ps``
   1. Login to the database ``docker exec -it CONTAINERID bash``
1. You can put dumps of your own under the ``dumps`` folder and import it as the docker starts.


How to embed the tool into MTurk?
===
The demo works with the CWI dataset to highlight complex phrases. If you like to use your own CWI database, make sure to populate `cwi` database first.

Here, we show how to embed `Par4Sim` into MTurk using the Java MTurk SDK

* Define the question as follows:

For the MTruk Sandbox:
```
 String question ="<ExternalQuestion xmlns=\"http://mechanicalturk.amazonaws.com/AWSMechanicalTurkDataSchemas/2006-07-14/ExternalQuestion.xsd\">\n" + 
			         "  <ExternalURL>https://YOUR/ADDRESS:PORT/par4sim/?host=https://workersandbox.mturk.com/mturk/externalSubmit</ExternalURL>\n" + 
			         "  <FrameHeight>900</FrameHeight>\n" + 
			         "</ExternalQuestion>"; 
```
For the  MTruk:

```
 String question ="<ExternalQuestion xmlns=\"http://mechanicalturk.amazonaws.com/AWSMechanicalTurkDataSchemas/2006-07-14/ExternalQuestion.xsd\">\n" + 
		                     "  <ExternalURL>https://YOUR/ADDRESS:PORT/par4sim/?host=https://www.mturk.com/mturk/externalSubmit</ExternalURL>\n" + 
		                     "  <FrameHeight>900</FrameHeight>\n" + 
		                     "</ExternalQuestion>";

```

**https://YOUR/ADDRESS:PORT** is the public server address where **Par4Sim** is hosted.

* Then embed the HIT with the MTurk SDK as follows:

```
     HIT hit = service2.createHIT(null,
	                 title, description, keywords, question, reward, ASSIGNMENT_DURATION_IN_SECONDS,
	                 AUTO_APPROVAL_DELAY_IN_SECONDS, LIFETIME_IN_SECONDS, numAssignments, null, rs, null);
```

At this point, you need to store the hitId (**hit.getHITId()**) along the the text in the **Par4Sim** database.


Here is how we define the Qualification requirements **rs**

```
   QualificationRequirement[] rs =  new QualificationRequirement[] { nApproved, pApproved };

   ```

   The Qualification requirement **nApproved** and **pApproved** are defined as follows

   ```
 QualificationRequirement  nApproved = r = new QualificationRequirement();
		r.setQualificationTypeId("00000000000000000040");
		r.setRequiredToPreview(false);
		r.setComparator(Comparator.GreaterThanOrEqualTo);
		r.setIntegerValue(new int[] { 10000 });

		QualificationRequirement pApproved = new QualificationRequirement();
		r.setQualificationTypeId("000000000000000000L0");
		r.setRequiredToPreview(false);
		r.setComparator(Comparator.GreaterThanOrEqualTo);
		r.setIntegerValue(new int[] { 96 }

   ```

 Do you like to see generic adaptive paraphrasing for semantic-aware writing aid tool? check [here](https://uhh-lt.github.io/par4sem/)

For the general installation instruction, please refer [Par4Sem installation](https://uhh-lt.github.io/par4sem/install/)
