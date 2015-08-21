#The Search Dashboard

##By Ben Yetton

**Version 0.1.0**

**August 21, 2015**

Technologies Involved:
- Mithril - Client side framework for patterns, DOM management and requests
- C3.js -  Charting librarary for interactive charts
- Elasticsearch - database and search library for index and collating documents (nodes)
- All code is in Javascript

***[1] Basic Specification:***
Can be added to any page to display search results and statistics from an elasticsearch database via ‘search widgets’
A search dashboard component contains basic information on what requests to send elastic and how/when to run them
Results from requests are piped to widgets, which then parse this information and display it.
Widgets can be of any type (charts, display results, searchbar, etc), and can easily be created and laid out by developers
Interaction with widgets is possible, and via callbacks to the dashboard, elastic search requests can be changed. In practice this means filters can be applied and removed from requests to update the result data that each widget displays.  

***[2] Technical Specifications:***
- Add to any page via .mount call
- Search requests are asynchronous and can run in serial or parallel
- Search requests fully customizable and open to developer
- Widgets are fully customizable and easy to create and layout
- Full widget communication via spoke and wheel style pattern (widgets communicate with dashboard, and dashboard back to widgets)
- Mithril c3 and elasticsearch can be abstracted away from developer


Outstanding Bugs:
None visible



TODOs:
The time series widget pulls time range data from the main request, and filtering the date range then updates this main request. This means that when a range filter is applied, the data points outside of the filter do not display on the subgraph. The desired behavior is to have the subgraph always display the full range of data, regardless of the date filters applied. In the image below, all data points show on subchart.
But when filter is applied (grey box), the other data points disappear.
FIX:
Create a new parallel request which feeds aggregation data to the timechart, but does not have range filters applied to it. All other filters such as project type, contributor, etc would be applied as normal. Range filters would continue to be applied to the main request. This functionality already exists, a new request just needs to be written.


Error handling of elastic errors. Elastic errors will only return the same error every time (‘invalid query’) but other errors such as bad aggregation are possible.



Future Features:
New widgets: 
Tag cloud widget: Top tags are displayed in a word cloud: the more mentions, the bigger the cloud. Clicking a tag, filters the results by that tag
Contributions over time: In github style, personal contributions over time could be plotted from log information. Currently log information is not indexed by elasticsearch, but that it a simple fix. A separate serial request (after getting contributor names) would be necessary to get the correct logs.
‘You Activity points over time’ widget (again requires log information)
Contributors chart could be populated from contribution amount over all projects (via logs  and activity points) instead of the current ‘number of projects involved in’. 
On a single project page (4) could show the contribution percentages of collaborators instead of the current green/blue line.
Smaller charts embedded into results widget: smaller donut/time charts could be added to individual results to show individual project stats.
OR filters. Currently only and filters are used, however, the code can handle OR filters (Contributor is XX OR contributor is XX). Some UI changes would have to be made to accommodate or filters. Potentially, AND filters could be switched to OR filters with a click.
Added to project page, main search page, share page
Nested parallel requests: Currently requests can be either parallel (fire at the same time) or serial (fire only after previous request completes). Adding ‘nested parallel’ requests means we can have parallel processes fire after the completion of another request (i.e. parallel requests nested in serial requests). Modification of the runRequests function is required, and potential use of async libraries such as async.js may be helpful




