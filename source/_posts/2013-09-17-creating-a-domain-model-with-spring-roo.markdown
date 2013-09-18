---
layout: post
title: "Creating a Domain Model with Spring Roo"
date: 2013-09-17 23:53
comments: true
categories: 
---
[Spring Roo](https://github.com/spring-projects/spring-roo) provides an easy way of creating domain models in Java,
complete with [JPA](http://en.wikipedia.org/wiki/Java_Persistence_API)-based persistence support.

Example:
<div class="mxgraph" style="position:relative;overflow:hidden;width:762px;height:272px;"><div style="width:1px;height:1px;overflow:hidden;">5VpLc5swEP41vnTGHYP8PIY8mkM7kxl3pu1RRgJrIiMqRGL311cCgS1BapwQg9MeUrQSy+rb1X7L4gG43my/cBivvzGE6cAdoe0A3Axcdz4ayb9KsLMEIScoFzlakBKEE0MkGKOCxKbQZ1GEfWHIAkZNZTEMcUWw9CGtSn8QJNZa6hTWqYl7TMK1fs58oicSsSt0IBzAlIphJnLz6e1Ir1/k450eO+NFoTgyLPjD2MYQcJyQP6aVAdFWaBtWjCPMDREl0eMhTOB2AK45YyK/2myvMVV+KTDPb7t7YbZEh+NINLlB7/4J0lSbPnCnVN7qxQZm09+pMsnbQB4SCcSVnB3FMlY8eZELh4LF+cS4mBB4K4aQklDf4Uur1P69vUJ5Fer/s8cmgrMoLKQPkAviE4m9KBbIvZhrDiZiW7bmAwXAnS0/fXMUB8LcXXUHajllpYql4CQzs8Y8KyKfsNonpFc5VjcZlJ5G7iZ/tsfkqoCyZykJiPQi8AIWiWUedI6rx3dwQ6g6s/eYPmGlVeEgNlQtqoaHFikD8NaOQ5kbMNtgwXfqhOSzCx24Oi04xfl6PjiNUy1bHxzE4j6ooz0sNe/DU17oCK2PVtDvaP0Ok8cLCVOZsH1OYkFYg2B9b2PwNsZRoihErUUsXcmHdGIJSvENFDhfml11YoaQgbQUpSHfy+EHyyVj0GEycWY12cTCEkfoivMMpohFUuhhJMsRvYBxsWYhiyC93Uu9F3FJWMp9bPCukHGBhZHc1AOaYOfOTOzc0T8w0VoeGFFE2lhFbrG+67CKsBTZfqwoyvdZUZS5qNxaM6/NK15jz5FM07brsnIMrrLhqHoA7FOyYkKogs6jcIWpB/3HkLM0QteMsrxiA0H2zzoqoxp3z4x6V9uwL/man49hWVpSKMiTWVy+KfYXFRQ/NUaQ5+fwMiA8FcH6AB86VkCzIEjwW2O53E1P6xlPGnoZ5QzcyEAT3dcPK4IOeNsrRh+Mtt2pme4BOCNtl+a2TNt4S8RPlek+zyZ6+CvDS15H0safOgtmg1/ZQnfyWq4vTn5jsncsgp2fTvbHVDQle2BXDbai9si+pj0hTxjqFdtb3a1zsr3ln50ZW+1SVfXVu1cFQ3teaLlg0E4Zup8n7+GW8XsnQ/fFZOi8KhmCtyfDsZWAgB3xx91zVEXTZDgFRxS1mAwnFV+LrOHUn1Q4/i9S4bTnqbA1L7STCs+UCeu6OT3rDS8Fjlt6obrcl4dplx8Q3Gr3qBW+fDX1zU6jvqn15uX8C5QXGOuYiqbUN3ePKGqR+qrtqj7R3rwz2mv84azaaeoXZ7UGYZ/7faCuddEjjvrKfJh9EbyQph9CMmST7r9fqlATKcLd9x+pdFZDUz5OBXHW9iNwO6wginLB+GzonFZCzOYmeq9oJR5V0bSEWIzP1koEdb8d6U0JUURVb96c34f/xhUf0JJy+lOMtOeMlso5Odz/IjDHff+TTXD7Fw==</div></div>

<!-- more -->
This can be turned into working Java code by entering the following Roo commands:
```
jpa setup --database MYSQL --provider HIBERNATE 
entity jpa --class ~.domain.Participant
field string --fieldName login
entity jpa --class ~.domain.Task
entity jpa --class ~.domain.Bid
focus --class ~.domain.Participant
field set --fieldName bids --type ~.domain.Bid --cardinality ONE_TO_MANY --mappedBy bidder
focus --class ~.domain.Bid
field reference --fieldName bidder --type ~.domain.Participant --cardinality MANY_TO_ONE 
focus --class ~.domain.Task
field set --fieldName bids --type ~.domain.Bid --cardinality ONE_TO_MANY --mappedBy task
focus --class ~.domain.Bid
field reference --fieldName task --type ~.domain.Task --cardinality MANY_TO_ONE 
focus --class ~.domain.Participant
field set --fieldName owned_tasks --type ~.domain.Task --cardinality ONE_TO_MANY --mappedBy owner
focus --class ~.domain.Task
field reference --fieldName owner --type ~.domain.Participant --cardinality MANY_TO_ONE 
field string --fieldName description
field number --fieldName expenses --type double
field date --fieldName dueDate --type java.util.Date
focus --class ~.domain.Bid
field number --fieldName amount --type double
focus --class ~.domain.Task
field enum --fieldName taskState --type ~.domain.TaskState --enumType ORDINAL
focus --class ~.domain.Bid
field enum --fieldName bidState --type ~.domain.BidState --enumType ORDINAL 
focus --class ~.domain.Task
entity jpa --class ~.domain.Location
entity jpa --class ~.domain.TaskStep
field reference --fieldName location --type ~.domain.Location --cardinality MANY_TO_ONE 
focus --class ~.domain.Task
field list --fieldName steps --type ~.domain.TaskStep --cardinality ONE_TO_MANY
focus --class ~.domain.Location
field string --fieldName address
field number --fieldName latitude --type double
field number --fieldName longitude --type double
```

The result is [here](https://github.com/bglomann/fetch-backend/tree/v0.0.1/src/main/java/com/whocanfetchit/domain).
