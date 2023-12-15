---
title: Home
layout: home
has_children: true
has_toc: true
---

# How to Use Our Stats Service API

Our stats service API allows our volunteers, project owners and project contributors to view their efforts and contributions to project/s.

Our Stats API uses HTTP GET requests with JSON arguments and JSON responses. You can view here ([https://github.com/zooniverse/eras/wiki/API-Callout-Examples](https://github.com/zooniverse/eras/wiki/API-Callout-Examples))  for full documentation and more callout examples.

In this page, we provide common examples project owners or user group admins might use in order to query specific volunteer classification/comment counts.


### Differences Between eras.zooniverse.org vs Defunct stats.zooniverse.org

If you are familiar with our older stats service ([https://github.com/zooniverse/zoo-event-stats](https://github.com/zooniverse/zoo-event-stats); [https://stats.zooniverse.org/](https://stats.zooniverse.org/)),  there are some key differences between the new service [https://eras.zooniverse.org](https://eras.zooniverse.org) and the old service [https://stats.zooniverse.org/](https://stats.zooniverse.org/).



* Differences in the Requests
    * URL changes
        * No need to include `/counts` in URL for eras.zooniverse.org
        * Period is now a parameter (`?period`) vs a fixed part of URL
        * Period is not a required parameter in eras.zooniverse.org
        * Eras.zooniverse.org uses pluralized version of `classifications` and `comments`
            * Eg. [https://eras.zooniverse.org/classifications?period=week](https://eras.zooniverse.org/classifications?period=week) vs [https://stats.zooniverse.org/counts/classification/week](https://eras.zooniverse.org/classification/week)
    * Valid `period`s are now only:
        * Day
        * Week
        * Month
        * Year
    * Some requests in eras.zooniverse.org will require an Authorization Header
* Differences in Responses
    * Responses of [https://eras.zooniverse.org](https://eras.zooniverse.org) will only return the total counts unless you specify a `period` you want to bucket your data by.
    * Response keys are different
        * [https://eras.zooniverse.org](https://eras.zooniverse.org) Response Example:
        * [https://stats.zooniverse.org](https://stats.zooniverse.org) Response Example:




## Querying Classification Counts (Unauthenticated)

We allow querying classification counts without Authentication (i.e. No Authorization Header within your request) if you are querying by the following:



* project_id/s
    * can search by multiple project_ids when entering a `,` separated string of ids
    * eg. `?project_id=1,2,3,4`
* workflow_id/s
    * can search by multiple workflow_ids when entering a `,` separated string of ids
    * eg. `?workflow_id=1,2,3,4`
* Start_date
    * Date Format must be in `YYYY-MM-DD`
* End_date
    * Date Format must be in `YYYY-MM-DD`
* Period
    * If this is a parameter, the response will include a `data` key which shows the breakdown of classification counts bucketed by your entered period.
    * Allowable buckets are either:
        * `day`
        * `week`
        * `month`
        * `year`

**One caveat is that we do not allow you to query by BOTH project_id AND workflow_id (either one or the other). **


### Example: Querying Classification Counts in total for Zooniverse

If one was curious on how many total classifications we currently have on the Zooniverse, you could query the following:

This will return the total count of classifications of the entire Zooniverse.

Response will look like:


### Example: Querying Classifications for a Specific Project

If interested in querying classification count for a specific project, we can do the following:

Response will look like:


### Example: Querying Classifications for a Specific Project With Count Breakdown

If interested in querying for classification count for a specific project (for eg. project with id `1234`) and also interested in the monthly counts that make up the total count of the response, we can query the following:

Here, we utilize the `?period` parameter to bucket by month. Allowable `period`s are `day`, `week`, `month`, `year`.

Response will look like:


### Example: Querying Classification Counts for a Specific Project With Count Breakdown Within A Certain Date Range

If interested in querying for classification count for a specific project (for eg. project with id `1234`) between the days of September 18, 2023 and September 22, 2023, and also interested in the daily counts that make up the total count of the response, we can query the following:

**It is important to note that when entering a date range (a `start_date` or an `end_date` or both), dates entered MUST be in the format YYYY-MM-DD **

Response:

**The API uses UTC and are strings in the ISO 8601 “combined date and time representation” format (https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) :**

**`<code>2015-05-15T15:50:38Z</code>`</strong>


### Example: Querying Classification Counts of Multiple Projects With Count Breakdowns Within A Certain Date Range

If interested in querying the classification counts of multiple projects (for eg. if one was the owner of projects with ID `1234` and `4321`) and were interested in total classification for both projects altogether between the days of May 05, 2015 and June 05, 2015, and also interested in the daily counts that make up the total count of the response, we can query the following:

**Note that the two project ids are separated by a `,`. **

**We expect the response to give the TOTAL classification count of both projects**



* **i.e. classification counts of project with id 1234 + classification counts of project with id 4321**

Response:


## Querying Comment Counts

We also allow querying comment counts without Authentication (i.e. No Authorization Header within your request).

With comment counts you can also filter your count query by the following parameters:



* project_id/s
    * can search by multiple project_ids when entering a `,` separated string of ids
    * eg. `?project_id=1,2,3,4`
* user_id/s
    * can search by multiple user_ids when entering a `,` separated string of ids
    * eg. `?user_id=1,2,3,4`
* Start_date
    * Date Format must be in `YYYY-MM-DD`
* End_date
    * Date Format must be in `YYYY-MM-DD`
* Period
    * If this is a parameter, the response will include a `data` key which shows the breakdown of comment counts bucketed by your entered period.
    * Allowable buckets are either:
        * `day`
        * `week`
        * `month`
        * `year`


### Example: Querying Comment Counts

If one was curious on how many total comments we currently have on the Zooniverse, you could query with the following:

Response will look something like this:


### Example: Querying Comment Counts By Project With Count 	Breakdown

Similar to querying classification counts, our stats API allows querying comment counts by project. The following example shows how one would query for comment counts for a specific project (eg. project with id `1234`) broken down by month.

Similar to `/classifications` endpoint, valid `period` buckets are either by `day`, `week`, `month`, `year`.

Response:


## Querying Classification Counts By User (Authenticated)

Our stats API allows querying for a volunteer’s personal classification stats as long as the person querying has proper authorizations._ In other words, querying classification counts by user requires an authentication token to be supplied. _

This authentication token is known as a bearer token and is usually supplied as a HTTP `Authorization` header with the value prefixed by `Bearer` and then the token data.

For example:

These tokens are generated by our main backend Panoptes. For more information on retrieving a Bearer token from Panoptes, please refer to our Panoptes documentation, specifically [https://zooniverse.github.io/panoptes/#example-using-postman](https://zooniverse.github.io/panoptes/#example-using-postman).


### Example: Retrieving Bearer Token From Panoptes

The easiest way to get started is to use client credentials OAuth flow.

You will need to create an OAuth application within our system via : [https://signin.zooniverse.org/oauth/applications/new](https://signin.zooniverse.org/oauth/applications/new)

**Note that it is imperative that you do NOT share the OAuth application secret **as it can gain access to your Zooniverse account as if you were using the system.

Once you have your OAuth application set up, you can do the following:


---

When calling out to Stats API’s `/classifications/users/?` route, you will not have access to another person’s classification stats through this route; you will only have access to view your own classification counts. More information can be found in our full documentation: [https://github.com/zooniverse/eras/wiki/API-Callout-Examples#classificationsusersid](https://github.com/zooniverse/eras/wiki/API-Callout-Examples#classificationsusersid)

You can query personal classification counts filtering by any of the following:



* project_id/s
    * can search by multiple project_ids when entering a `,` separated string of ids
    * eg. `?project_id=1,2,3,4`
* workflow_id/s
    * can search by multiple workflow_ids when entering a `,` separated string of ids
    * eg. `?workflow_id=1,2,3,4`
* Start_date
    * Date Format must be in `YYYY-MM-DD`
* End_date
    * Date Format must be in `YYYY-MM-DD`
* Period
    * If this is a parameter, the response will include a `data` key which shows the breakdown of classification counts bucketed by your entered period.
    * Allowable buckets are either:
        * `day`
        * `week`
        * `month`
        * `year`
* Time_spent (true/false)
    * Boolean that dictates whether your response will calculate the approximate time spent **<span style="text-decoration:underline;">in seconds</span>** on your classifications.
        * Note that this calculation does not include any time you have spent on Talk
* Project_contributions (true/false)
    * Boolean that dictates whether your response will display all your project contributions broken down.
        * This list is ordered by top contributing projects, by classification count
        * This list does not include any time and efforts you may have spent on Talk

**CAVEATS**



* **We do not allow you to query by BOTH project_id AND workflow_id (either one or the other)**
* **We do not allow you to query by both `project_id`/`workflow_id` AND `?project_contributions=true`. **

For the following examples, we use the user_id `1234`


### Example: Query Personal Classification Counts

If you were interested in your own personal classification counts of all time. You will need your user_id and run the following:

Response:


### Example: Query Personal Classification Counts and Approximate Time Spent

If you were interested in both your own personal classification counts of all time and approximate time spent on those classifications, you would query the following:

Response will look something like this:

**Noting that `time_spent` calculation is time in _<span style="text-decoration:underline;">seconds</span>_**


### Example: Query Personal Classification Counts, Time Spent, and  Breakdown of Classification Counts

If interested in querying for your own personal classification counts of all time, approximate time spent on those classifications, also interested in the yearly counts that make up the total count of the response, we can query the following:

Response:


### Example: Query Personal Classification Counts, Time Spent, And All Project Contributions

If interested in querying for your own personal classification counts of all time, approximate time spent on those classifications, and also interested in your project contributions in terms of classification count, we can query the following:

Response will look something like this:

Note that the list of `project_contributions` is in order by `count`; which is your classification count per project.


### Example: Query Personal Classification Counts For A Specific Project/s

If interested in querying for your own personal classification counts during a date range for a specific project, along approximate time spent on those classifications, we can query the following:



In this example, we use the user with id `1234`’s project classification counts for project with id `1972` in between the date range of `2022-03-10` (March 10, 2022) through `2023-03-10` (March 10, 2023)

Response:


## Querying Classification Counts By User Group


### What Are User Groups?

As a new feature of our stats service, we introduce the idea of user groups so that a group of volunteers can set shared goals and celebrate milestones. Whether it’s a classroom, after school club, a group of friends, or corporate volunteering program, this new group feature provides new avenues for fostering community and collaboration for our volunteers and contributors.

For more documentation on user groups within our stats service, you can view our documentation: here (https://github.com/zooniverse/eras/wiki/API-Callout-Examples#classificationsuser_groupsid)

Our stats API allows querying for a user group’s classification stats as long as the person querying has proper authorizations to access the group statistics. _In other words, querying classification counts by user group requires an authentication token to be supplied. _

This authentication token is known as a bearer token and is usually supplied as a HTTP `Authorization` header with the value prefixed by `Bearer` and then the token data.


---

You can query user group classification counts filtering by any of the following:



* project_id/s
    * can search by multiple project_ids when entering a `,` separated string of ids
    * eg. `?project_id=1,2,3,4`
* workflow_id/s
    * can search by multiple workflow_ids when entering a `,` separated string of ids
    * eg. `?workflow_id=1,2,3,4`
* Start_date
    * Date Format must be in `YYYY-MM-DD`
* End_date
    * Date Format must be in `YYYY-MM-DD`
* Period
    * If this is a parameter, the response will include a `data` key which shows the breakdown of classification counts bucketed by your entered period.
    * Allowable buckets are either:
        * `day`
        * `week`
        * `month`
        * `year`
* top_contributors (integer)
    * Limit that dictates whether your response will show top contributors of the user group
* individual_stats_breakdown (true/false)
    * Boolean that dictates whether your response will shows show a roster stats report per each individual member for the user group


### Example: Query User Group Classification Counts

If you were interested in the user group with user_group id=1234’s classification counts of all time. You will need your user_group_id and run the following:

Response:

The response for querying user group classification counts will look a bit different than the other queries from the previous examples. By default, querying user group classification counts will return the following:



* Total_count
    * Integer
    * The total count of classifications of queried user group
* Time_spent
    * Float
    * Total session time IN SECONDS of total classifications of user group
* Active_users
    * Integer
    * Total active users of the user group
    * Active users being users who have made a classification given request parameters
* Project_contributions
    * List
    * List of all project contributions (project_id and count) of user group given request parameters
    * NOTE: if `project_id` or `workflow_id` is a parameter in your request, the response will NOT include this list
* data
    * Only returned when `period` is a request parameter
    * This shows the total breakdown of classifications of the user group bucketed by `period` that make up the response’s `total_count`


### Example: Query User Group’s Group Member Stats Breakdown

If you were interested in the user group with user_group id=1234’s group member stats breakdown of all time, we can utilize the `?individual_stats_breakdown=true` parameter and request the following:

Response:

Note that in this particular response, it returns a list of each group member’s project contributions, session time and classification count, ordered by top total classification count of members in the group.


## Examples in Other Languages


### Python


### Javascript

The following example is an authenticated callout to `/users` where `user_id=1234`
