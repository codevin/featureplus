# featureplus

A development approach which integrates feature management tightly with coding.
The package will offer an API for golang which helps:
* Identify a feature (and subfeatures), 
* Associate variety of artifacts feature - feature details document, UI artifacts and also development artifacts (code segments)
* Allow specification of how these artifacts have to be integrated into existing codebase (against a particular branch/commit). In particular, we would like code related to feature to be specified outside the codebase (i.e. local artifacts identifying incremental changes), and integrate them into codebase programmatically.  
* The new codebase is then committed and used as a base for next feature.

Instead of directly changing the codebase, we want codebase to be modified programmatically, and this part of API would have capabilities as below:
* Introduce a new Module/Component into codebase
* Introduce a new function in a module
* Introduce a code segment within a function (or a structure)
* Or, create semantically meaningful actions which add code programmatically. For e.g. add a new menu item to the menu (in HTML code). 

To facilitate these type of actions, we will have to model the codebase to 
identify individually identifiable components which will support actions to add/remove code within them.


