# featureplus

A development approach which integrates feature management tightly with coding.
(A variation of Literate programming concept). 

# Intro

Over the time, the codes become extremely complex. For a developer to figure out what features are actually existing in code, and chasing the bugs becomes very hard. The feature requirements and code become out of sync, and are often not documented at finer grain level.

Literate programming approach suggests writing the implementation as a document which explains what, why and how's, and embed the code within. From this document, source code is auto-generated. 

In FeaturePlus approach, we want a feature to be documented with help of APIs, which make the feature development process central to developing a code: We want feature to be as fine grained as possible, and even capture incomplete and yet-to-be implemented aspects. With a fine grained feature, we then let developer identify and assign fine-grained code, which will then be inserted with master codebase which is centrally managed. 

While the concepts are fundamentally based on literate programming, we would like to look at the objectives totally from fresh perspective and design from scratch. We would necessarily want to retain the current coding approach and practices. 

# Developer workflow

The overall development approach will be as follows:

1. There are two development trees. One is main-codebase which is centrally hosted, and allows any branch to be committed, compiled and create testable code. Normal developers will not necessarily be directly accessing this codebase. (They can have read only access.)

2. Second development tree is a feature-codebase which uses FeaturePlus API, and allows developers to create their code. The developer would have to add features/sub-features, associate its artifacts, and write incremental code specific to features. Incremental code here means a set of modules, functions, new members to struct/types, small lines of code to be added to existing functions and so on. The assumption here is that an incremental addition to feature will result in incremental changes to existing codebase.

3. When developer compiles his feature-codebase and runs, it will manipulate feature database, checks out the main-codebase, integrates the changes, and prepare the executable for developer to test. 

4. If all goes well, the feature-codebase is run in production mode (by manager) to integrate it into master feature database, as well as main-codebase. The branches are appropriately edited for next feature cycles. 

Here we are making some assumptions:

* The incremental codes are going to be treated as strings in feature-codebase.
This results in developer  having to switch between reading integrated code, and making changes in feature-codebase.  As such, this makes debugging difficult. When IDEs embrace the concept, this pain will be much less. 

* Architect will have to annotate entry points in main-codebase, where feature-specific code will be inserted. Inserting new modules and functions would be easy. But inserting in existing function (for e.g. initialization sequences, command line parameter parsing) will be difficult. Neverthless, it is not a rocket science.  

* The feature codebase will manage a centralized Feature Database, which tracks each feature/sub-feature/sub-sub-feature at fine grain level. With each feature, it will maintain variety of artificats contributed by different folks (such as UI, Developers, Testers). Making changes to main codebase outside of what features are tracking is discouraged in this model. 

* Features may be inter-dependent. The system will necessirily capture the dependencies. As such, this will streamline and force development to happen in parallel with multiple developers. The ordering of features will affect the order in which code is inserted in main codebase, and therefore dependencies have to be managed appropriately. 

* Feature Codebase, once completed and made available in feature codebase, can be archived in git itself. It means, finished features can be removed from feature codebase, and can be revisited later if the requirements come up.  As such, every feature development can be a branch as usual; except that the feature codebase will be considered temporary. It is only to manage dependencies and changes related to features in feature database.

Since we are associating changes to the main codebase in feature database, a question arises as to "Why not identify the feature related changes in usual codebase and then add them to feature DB?". Well, in theory, this should work. But this won't force features to be managed at all, since features documentation is not a priority activity. We should turn it totally and establish a new culture. And also as codebase grows, the features oriented approach is quite scalable and manageable - intutively speaking.

# What API will be like

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

# Pros and Cons of this approach

The Pros:
* Forces features and code to be part of same codebase.
* All aspects of a feature - UI, server code, database code, members in structures - all these will be in one place to browse. 
* All artifacts related to each feature at one place. Understanding and verifying the code now becomes a focused activity, and there is no need to browse the main codebase as in currently used approach.
* Incremental changes, bug fixes will now be much more ease.
* Functions which are global in nature (e.g. command line options, initializers) will be explicitly managed by conventions. How a feature can insert code within such functions will be quite a focused activity.
* Allows architect to establish and communicate the changes that need to be done to main codebase to the developer. And the result will itself be reflected in contributed code and be verified.
* Feature codebase is currently in Golang. But the main codebase can be in any language, as long as we develop connectors to different code change actions.
* Enables outsourced code development, since developer only needs conventions, and will not need whole of codebase.


The Cons:
* Fundamentally a different approach. Untested. And instead of one codebase, we have two. 
* Code for a given feature is essentially a string (in feature codebase). This may become convenient because IDEs don't support it easily. 
* Since two features may contribute a code inserted into single function, the local variables shouldn't clash. This must be guaranteed by some automation.


# Status of Development

Done
* Document Objective
* Brainstorm Approach and Architecture
* Rough idea of API

TODO
* Creating APIs for driving example code (e.g. 2 page website)
* ...


